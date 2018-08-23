---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Toplu ekleme (C#) | Microsoft Docs
author: rick-anderson
description: Tek bir işlemde birden çok veritabanı kayıtlarının nasıl ekleneceğini öğrenin. Kullanıcı arabirimi katmanda biz birden çok n girmek izin vermek için GridView genişletme...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 561acc9b473bac7d39e7ed4d511d8b979657131d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752357"
---
<a name="batch-inserting-c"></a>Toplu ekleme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) veya [PDF olarak indirin](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Tek bir işlemde birden çok veritabanı kayıtlarının nasıl ekleneceğini öğrenin. Kullanıcı arabirimi katmanda biz GridView'ın birden çok yeni kayıtlar girmesini izin verecek şekilde genişletin. Veri erişim katmanındaki tüm başarılı olması veya tüm geri alınacak emin olmak için bir işlem içinde birden çok ekleme işlemi biz kaydır.


## <a name="introduction"></a>Giriş

İçinde [toplu işlemi güncellenirken şu](batch-updating-cs.md) Öğreticisi burada birden çok kaydı düzenlenebilir bir arabirim sunmak için GridView denetiminde özelleştirme sırasında incelemiştik. Sayfasını ziyaret ederek kullanıcı, bir dizi değişikliği yapın ve ardından, bir düğmeye tıklatma ile bir toplu güncelleştirmeyi gerçekleştirin. Kullanıcıların yaygın olarak güncelleştirme tek bir seferde çok sayıda kayıt durumlarda, böyle bir arabirim sayısız tıklama kaydedebilir ve varsayılan karşılaştırıldığında klavye ve fare bağlam geçişi-satır başına ilk geri keşfedilmemiş düzenleme özellikleri [bir Genel Bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğretici.

Bu kavram, kayıtları eklerken uygulanabilir. Bu Imagine burada Northwind Traders yaygın olarak yalnızca belirli bir kategorideki ürün sayısını içeren Suppliers aldığımız. Örnek olarak, şu altı farklı Çay ve kahve ürün sevk irsaliyesi Tokyo Traders ' alabilirsiniz. Bir DetailsView denetimi birer altı ürünleri bir kullanıcının girdiği varsa, bunlar birçok aynı değer tekrar tekrar seçin gerekecektir: aynı kategoride (İçecekler) aynı sağlayıcı (Tokyo Traders) seçmeniz gerekir, aynı değeri (kullanımdan kaldırıldı False) ve aynı birimlere sıra değeri (0). Bu yinelenen bir veri girişi, yalnızca zaman alıcı değildir, ancak hatalara açıktır.

Kullanıcının üretici ve kategoriye bir kez, bir dizi ürün adları ve birim fiyatları girin ve ardından veritabanına yeni ürün eklemek için bir düğmeye tıklayın seçmesini sağlayan arabirimi ekleme toplu iş oluşturabiliriz ile biraz iş (bkz. Şekil 1). Her ürün eklendiğinde, kendi `ProductName` ve `UnitPrice` veri alanlarını, metin kutuları girilen değerler atanır ancak kendi `CategoryID` ve `SupplierID` değerleri, en üst fo form DropDownList gelen değerler atanır. `Discontinued` Ve `UnitsOnOrder` değerleri sabit kodlanmış değerler ayarlanmış `false` ve 0, sırasıyla.


[![Toplu ekleme arabirimi](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Şekil 1**: toplu ekleme arabirimi ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image3.png))


Bu öğreticide, Şekil 1'de gösterilen arabirimi ekleme batch uygulayan bir sayfa oluşturacağız. Olarak iki önceki öğreticilerle biz eklemeler yaparak kararlılık emin olmak için bir işlem kapsamında kaydırılır. Let s başlayın!

## <a name="step-1-creating-the-display-interface"></a>1. adım: ekran arabirimi oluşturma

Bu öğreticide iki bölgeye bölünmüş bir tek sayfalı oluşur: bir görüntü bölge ve ekleme bir bölge. Bu adımda oluşturacağız, görüntü arabirimi içinde GridView ürünleri gösterir ve işlem ürün sevk adlı bir düğme içerir. Bu düğmeye tıklandığında görünen arabirimi Şekil 1'de gösterilen ekleme arabirimi ile değiştirilir. Görüntü arabirimi sonra ürün ekleme yapılan sevkiyat döndürür veya İptal düğmeleri tıkladı. 2. adımda ekleme arabirimi oluşturacağız.

Bir sayfa oluşturma yalnızca biri aynı anda görülebilir, iki arabirim olduğunda her arabirim içinde genellikle yerleştirilir bir [paneli Web denetimi](http://www.w3schools.com/aspnet/control_panel.asp), diğer denetimler için kapsayıcı olarak görev gören. Bu nedenle sayfamızı her arabirim için iki Panel denetimleri bir olacaktır.

Başlangıç açarak `BatchInsert.aspx` sayfasını `BatchData` klasörü ve tasarımcı araç kutusundan bir panele sürükleyin (bkz: Şekil 2). S paneli kümesi `ID` özelliğini `DisplayInterface`. Panel Tasarımcı eklerken, `Height` ve `Width` özellikleri ayarlanır 50px ve 125px, sırasıyla. Bu özellik değerleri Özellikler penceresinden Temizle.


[![Tasarımcı araç kutusundan bir Panel sürükleme](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Şekil 2**: bir panelin tasarımcı araç kutusundan sürükleyin ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image6.png))


Ardından, bir düğme ve GridView denetimi paneline sürükleyin. S düğmesi ayarlamak `ID` özelliğini `ProcessShipment` ve kendi `Text` işlem ürün sevk irsaliyesi için özellik. GridView s ayarlamak `ID` özelliğini `ProductsGrid` ve isteğe bağlı olarak, akıllı etiketten adlı yeni bir ObjectDataSource bağlama `ProductsDataSource`. ObjectDataSource kendi verileri çekmek için yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu GridView yalnızca verileri görüntülemek için kullanıldığından, güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN. Veri Kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın.


[![S ProductsBLL sınıfı GetProducts yönteminden döndürülen verileri görüntüleyin](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Şekil 3**: döndürülen verileri görüntüleme `ProductsBLL` s sınıfı `GetProducts` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image9.png))


[![Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Şekil 4**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image12.png))


ObjectDataSource sihirbazını tamamladıktan sonra Visual Studio BoundFields ve ürün veri alanları için bir CheckBoxField ekleyeceksiniz. Kaldırma dışındaki tüm `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, ve `Discontinued` alanları. Estetik tüm özelleştirmeler çekinmeyin. Biçimlendirme karar `UnitPrice` alan bir para birimi değeri olarak alanları yeniden ve birçok alan yeniden adlandırılmış `HeaderText` değerleri. Ayrıca GridView sayfalama ve Destek GridView s akıllı etiket etkinleştirme sayfalama ve sıralamayı etkinleştir onay kutularını işaretleyerek sıralama içerecek şekilde yapılandırın.

Paneli, düğme, GridView ve ObjectDataSource denetimi ekleme ve GridView s alanları özelleştirdikten sonra sayfa s, bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

GridView ve düğme için biçimlendirme, açılış ve kapanış içinde görünen Not `<asp:Panel>` etiketler. Bu denetimleri içinde olduğundan `DisplayInterface` paneli, biz Gizle bunları s paneli ayarlayarak `Visible` özelliğini `false`. 3. adım arar s paneli program aracılığıyla değiştirme sırasında `Visible` özelliği yanıt olarak bir düğmeyi tıklatın diğer gizleyerek bir arabirim gösterilecek.

Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Şekil 5 gösterildiği gibi aynı anda on ürünleri listeleyen GridView işlem ürün sevk düğmesini görmeniz gerekir.


[![GridView olduğu ürünleri listeler ve sıralama ve disk belleği özellikleri sunar.](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Şekil 5**: GridView ürünleri ve sıralama sunar ve disk belleği özellikleri listeler ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>2. adım: ekleme arabirimi oluşturma

Görüntü arabirimi ile tam, biz re ekleme arabirimi oluşturmak için hazır. Bu öğreticide, tek bir üretici ve kategoriye değer ister ve ardından en fazla beş ürün adları ve birim fiyatı değerleri girmesini sağlayan bir ekleme arabirim oluşturmak s olanak tanır. Bu arabirim, tüm aynı kategoriye ve tedarikçi paylaşır, ancak fiyatları ve benzersiz ürün adlarına sahip bir ile beş yeni ürünler kullanıcı ekleyebilirsiniz.

Tasarımcıya varolan altındaki yerleştirmek için araç kutusundan bir Panel sürükleyerek başlangıç `DisplayInterface` paneli. Ayarlama `ID` bu özelliği yeni eklenen paneline `InsertingInterface` ve kendi `Visible` özelliğini `false`. Ayarlar kod ekleyeceğiz `InsertingInterface` paneli s `Visible` özelliğini `true` adım 3'te. Ayrıca s panelini dışarı Temizle `Height` ve `Width` özellik değerleri.

Ardından, Şekil 1'de gösterilen ekleme arabirimi oluşturmak ihtiyacımız var. Bu arabirim, çeşitli HTML teknikleri aracılığıyla oluşturulabilir ancak oldukça basit bir kullanacağız: dört sütun, satır içi yedi bir tablo.

> [!NOTE]
> İşaretleme için HTML girerken `<table>` öğeleri tercih ediyorum kaynak görünümü kullanmak. Visual Studio Araçları ekleme açıkken `<table>` öğeleri Tasarımcısı aracılığıyla Tasarımcısı gibi görünüyor ekleme tüm çok istekli için sorulmamış `style` biçimlendirme ayarlarını. Ben oluşturduktan sonra `<table>` biçimlendirme, ı genellikle iade Web denetimleri ekleme ve bunların özelliklerini ayarlamak için tasarımcı. Önceden belirlenen sütunları ve satırları tablo oluştururken, statik HTML kullanarak istemiyorum yerine [Tablo Web denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) yerleştirilmiş bir tablo Web denetimi tüm Web denetimleri kullanarak yalnızca erişilebilir olduğu `FindControl("controlID")` deseni. Ancak, Tablo Web denetimleri (yorumlar) olan satırlar veya sütunlar bazı veritabanı veya kullanıcı tarafından belirtilen ölçütlere göre tablolar için dinamik olarak boyutlu tablo denetimi program aracılığıyla oluşturulabilir Web beri kullanıyorum.


Aşağıdaki biçimlendirme içinde girin `<asp:Panel>` etiketleri `InsertingInterface` paneli:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Bu `<table>` biçimlendirme herhangi bir Web denetim içermez henüz bu bir kısa bir süre içinde ekleyeceğiz. Unutmayın, her `<tr>` öğe içeren belirli bir CSS sınıfı ayarı: `BatchInsertHeaderRow` burada üretici ve kategoriye DropDownList gider; üst bilgi satırı için `BatchInsertFooterRow` sevkiyat ve İptal düğmeleri ekleme ürünleri nereye; altbilgi satır ve değişen `BatchInsertRow` ve `BatchInsertAlternatingRow` ürün ve birimi içeren satırlar için değerleri fiyat TextBox denetimleri. Ben ve karşılık gelen CSS sınıfları oluşturulan `Styles.css` dosya ekleme arabirimi GridView ve DetailsView benzer bir görünüm sağlamak için biz Bu öğretici kullanılan ve denetler. Bu CSS sınıfları aşağıda gösterilmektedir.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Girilen bu biçimlendirmeyle Tasarım görünümüne geri dönün. Bu `<table>` Şekil 6 gösterildiği gibi dört sütun, satır içi yedi Tablo Tasarımcısı'nda olarak göstermelidir.


[![Ekleyerek arabirime oluşan bir dört sütunlu, satır içi yedi tablo,](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Şekil 6**: ekleme arabirimi oluşur, bir dört sütunlu, satır içi yedi tablo ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image18.png))


Şu anda yeniden ekleme arabirime Web denetimleri eklemek için hazır. İki DropDownList uygun hücrelere tablosunda bir tedarikçi ve kategori için araç kutusundan sürükleyin.

DropDownList s sağlayıcısına ayarlamak `ID` özelliğini `Suppliers` ve adlı yeni bir ObjectDataSource bağlama `SuppliersDataSource`. Yapılandırma, verileri almak için yeni ObjectDataSource `SuppliersBLL` s sınıfı `GetSuppliers` yöntemi ve küme güncelleştirme s açılır listede (hiçbiri) için sekmesinde. Sihirbazı tamamlamak için Son'u tıklatın.


[![ObjectDataSource s SuppliersBLL sınıfı GetSuppliers yöntemi kullanmak üzere yapılandırma](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Şekil 7**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image21.png))


Sahip `Suppliers` DropDownList görünen `CompanyName` veri alan ve kullanım `SupplierID` veri alanı olarak kendi `ListItem` s değerleri.


[![CompanyName veri alanı görüntülemek ve SupplierID değeri olarak kullanın](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Şekil 8**: görüntü `CompanyName` veri alan ve kullanım `SupplierID` değeri ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image24.png))


İkinci DropDownList ad `Categories` ve adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource`. Yapılandırma `CategoriesDataSource` kullanılacak ObjectDataSource `CategoriesBLL` s sınıfı `GetCategories` yöntemi; açılan listeler UPDATE ve DELETE sekmeler (hiçbiri) ve tıklayın kümesi son Sihirbazı tamamlayın. Son olarak, DropDownList görüntülemesi `CategoryName` veri alan ve kullanım `CategoryID` değeri.

Bu iki DropDownList ekledikten ve uygun şekilde yapılandırılmış ObjectDataSources için bağlı sonra Şekil 9'ekranınızın benzemelidir.


[![Üst bilgi satırı artık üreticiler ve kategorileri DropDownList içerir](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Şekil 9**: üst bilgi satırı artık içeren `Suppliers` ve `Categories` DropDownList ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image27.png))


Şimdi yeni her ürün için fiyat ve adını toplamak için metin kutuları oluşturmak ihtiyacımız var. TextBox denetimi Tasarımcısı araç kutusundan her beş ürün adı ve fiyat satırları için sürükleyin. Ayarlama `ID` özelliklerine metin kutuları için `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`ve benzeri.

Her birim fiyatı ayarlama metin kutuları, sonra bir CompareValidator ekleme `ControlToValidate` özelliğini uygun `ID`. Ayrıca `Operator` özelliğini `GreaterThanEqual`, `ValueToCompare` 0 ve `Type` için `Currency`. Bu ayarlar, fiyat, girdiyseniz, büyük veya sıfıra eşit bir geçerli para birimi değeri olduğundan emin olmak için CompareValidator isteyin. Ayarlama `Text` özelliğini \*, ve `ErrorMessage` fiyatına büyük veya sıfıra eşit olmalıdır. Ayrıca, Lütfen herhangi bir para birimi simgeleri yok sayın.

> [!NOTE]
> Ekleme arabirimi RequiredFieldValidator denetimlerde olsa bile içermeyen `ProductName` alanındaki `Products` veritabanı tablosu izin vermiyor `NULL` değerleri. En fazla beş ürün girmesine izin vermek istiyoruz olmasıdır. Kullanıcı, ürün adı ve birim fiyatı için ilk üç satırı sağlamak için olsaydı, örneğin, son iki satırını boş bırakarak d yalnızca üç yeni ürünler sisteme ekledik. Bu yana `ProductName` olan gerekli, ancak biz program aracılığıyla olması durumunda bir birim fiyatı emin olmak için girilen karşılık gelen bir ürün adı değer sağlanır denetlemeniz gerekir. Biz bu 4. adımda iade üstesinden.


Değer bir para birimi simgesi içeriyorsa CompareValidator s kullanıcı girişini doğrulama sırasında geçersiz veri bildirir. Her birim fiyatı fiyatı girerken para birimi simgesi atlamak için kullanıcı yönlendiren görsel bir ipucu hizmet vermek için metin kutuları önünde bir $ ekleyin.

Son olarak, ValidationSummary denetimine ekleme `InsertingInterface` paneli, ayarları, `ShowMessageBox` özelliğini `true` ve kendi `ShowSummary` özelliğini `false`. Bu ayarlarla kullanıcının bir geçersiz birim fiyat değeri girerse, soruna neden olan TextBox denetimi yanında bir yıldız işareti görünür ve daha önce belirtilen hata iletisini gösteren bir istemci-tarafı messagebox, ValidationSummary görüntüler.

Bu noktada, ekran Şekil 10'a benzer görünmelidir.


[![Ekleme arabirimi ürünleri için metin kutuları artık içeriyor. ad ve fiyat](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Şekil 10**: ekleme arabirimi artık içeren metin kutuları fiyatları ve ürün adları için ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image30.png))


Sonraki biz ürün ekleme sevkiyat ve İptal düğmeleri alt bilgi satırına eklemeniz gerekir. Sürükleme iki düğme denetimleri araç kutusundan ekleme arabirimi alt bilgisi ayarlama düğmeleri `ID` özelliklerine `AddProducts` ve `CancelButton` ve `Text` sevkiyat ve iptal etme, ürünler sırasıyla eklemek için özellikler. Buna ek olarak, `CancelButton` denetim s `CausesValidation` özelliğini `false`.

Son olarak, iki arabirim durum iletilerini görüntüleyen bir etiket Web denetimi eklemek ihtiyacımız var. Kullanıcı başarıyla yeni bir sevkiyat ürünlerin eklediğinde, örneğin, görüntü arabirimine geri dönün ve bir onay iletisi görüntülemek istiyoruz. Ancak, kullanıcı ürün adı devre dışı bırakır, ancak yeni bir ürün için bir fiyat sağlar, bu yana bir uyarı iletisi görüntülenecek ihtiyacımız `ProductName` alanı gereklidir. Bu ileti için her iki arabirimde görüntülemek için ihtiyacımız olduğundan, paneller dışında bir sayfanın üstündeki yerleştirin.

Bir etiket Web denetimi araç kutusundan Tasarımcısı'nda sayfanın en üstüne sürükleyin. Ayarlama `ID` özelliğini `StatusLabel`, kullanıma açık `Text` özelliği ve kümesi `Visible` ve `EnableViewState` özelliklerine `false`. Önceki öğreticilerde anlatıldığı gibi ayarlama `EnableViewState` özelliğini `false` programlı olarak etiket s özellik değerlerini değiştirin ve bunları otomatik olarak sonraki geri gönderme üzerinde kendi varsayılanlarına geri dönmek sağlıyor. Bu, sonraki geri göndermede kaybolur bazı kullanıcı eylemine yanıt olarak bir durum iletisi gösteren kodunu basitleştirir. Son olarak, ayarlama `StatusLabel` denetim s `CssClass` özelliği CSS sınıfının adı olan bir uyarı için tanımlanan `Styles.css` metni büyük, italik, kalın, kırmızı bir yazı tipinde görüntüleyen.

Etiket eklendi ve yapılandırılmış sonra Visual Studio tasarımcısı Şekil 11 gösterir.


[![StatusLabel denetimin üstünde iki Panel denetimleri yerleştirin](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Şekil 11**: yerde `StatusLabel` denetim Yukarıdaki iki Panel denetimleri ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>3. adım: ekran ve ekleme arabirimleri arasında geçiş yapma

Bu noktada bizim görüntüleme ve ekleme arabirimleri ancak yeniden yine de iki görevlerle sol biz biçimlendirme tamamladınız:

- Görünümü arasında geçiş yapma ve arabirimleri ekleme
- Veritabanı sevkiyata ürünleri ekleme

Şu anda görüntü arabirimi görülebilir ancak ekleme arabirimi gizlenir. Bunun nedeni, `DisplayInterface` paneli s `Visible` özelliği `true` (varsayılan değer) sırada `InsertingInterface` paneli s `Visible` özelliği `false`. Yalnızca ihtiyacımız her denetim s geçiş yapmak için iki arabirim arasında geçiş yapmak için `Visible` özellik değeri.

İşlem ürün sevk düğmesine tıklandığında görüntü arabiriminden ekleme arabirimi taşımak istiyoruz. Bu nedenle, bu düğmeyi s'için bir olay işleyicisi oluşturun `Click` aşağıdaki kodu içeren olay:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Bu kod yalnızca gizler `DisplayInterface` paneli ve gösterir `InsertingInterface` paneli.

Ardından, sevkiyat ve iptal düğmesi denetimlerde ekleme arabirimi ekleme ürünleri için olay işleyicileri oluşturun. Bu düğmeler birini tıkladığınızda, görüntü arabirimine geri dönmek ihtiyacımız var. Oluşturma `Click` her ikisi için olay işleyicileri düğme denetimleri böylece bunlar çağrı `ReturnToDisplayInterface`, biz kısa bir süre içinde add yöntemi. Gizleme yanı sıra `InsertingInterface` paneli ve gösteren `DisplayInterface` panelinde `ReturnToDisplayInterface` Web denetimleri önceden düzenleme durumlarına döndürmek yöntemin gerekir. Bu ayar DropDownList içerir `SelectedIndex` 0 ve temizleme Özellikler `Text` metin denetimlerin özelliklerini açma.

> [!NOTE]
> Ne olacağını düşünün, biz etmedi t dönüş denetimleri önceden düzenleme durumlarına görüntüleme arabirimi döndürmeden önce. Bir kullanıcı, işlem ürün sevk düğmesine tıklayın, ürünleri yapılan sevkiyat girin ve ürünleri sevkiyat Ekle'ye tıklayın. Bu ürünleri ekleme ve kullanıcının görünen arabirimine döndürür. Bu noktada kullanıcı, başka bir sevkiyat eklemek isteyebilirsiniz. Ekleme arabirimi ancak DropDownList döndürecekti işlem ürün sevk düğme tıklatıldığında tamamlanacaktır seçim ve metin değerlerini yine de önceki değerleriyle doldurulması.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Her ikisi de `Click` olay işleyicileri çağırarak yeterlidir `ReturnToDisplayInterface` yöntemi, ürün ekleme yapılan sevkiyat getireceğiz rağmen `Click` olay işleyicisinde 4. adım ve ürünleri kaydetmek için kodu ekleyin. `ReturnToDisplayInterface` döndürerek başlar `Suppliers` ve `Categories` DropDownList kendi ilk seçenekleri. İki sabit `firstControlID` ve `lastControlID` başlangıç ve bitiş adlandırma ekleme, metin kutuları arabirim ve sınırları içinde kullanılan ürün adı ve birim fiyatı kullanılan denetim dizin değerlerinin işaretlemek `for` Ayarlardöngü`Text`TextBox denetimleri özelliklerini yedeklemek için boş bir dize. Son olarak, panellerin `Visible` özellikleri, böylece ekleme arabirimi gizli ve gösterilen görüntüleme arabirimi sıfırlanır.

Bir tarayıcıda bu sayfası test etmek için bir dakikanızı ayırın. Sayfa ilk ziyaret edildiğinde görüntüle arabirimi Şekil 5'te gösterildiği görmeniz gerekir. İşlem ürün sevk düğmesine tıklayın. Sayfa geri gönderme ve artık ekleme arabirimi Şekil 12'de gösterildiği gibi görmeniz gerekir. Ya da ekleme ürünleri sevkiyat ya da İptal düğmeleri tıklayarak görüntüleme arabirimi döndürür.

> [!NOTE]
> Ekleme arabirimi görüntülerken CompareValidators metin kutuları birim fiyatı üzerinden kullanıma test etmek için bir dakikanızı ayırın. Bir istemci-tarafı messagebox sevkiyat düğme geçersiz bir para birimi değerleri ile veya bir değeri sıfırdan küçük fiyatlarıyla ekleme ürünleri tıklandığında uyarı görmeniz gerekir.


[![Ekleyerek arabirime işlem ürün sevk düğmesine Tıklandıktan sonra görüntülenir.](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Şekil 12**: arabirim ekleme işlemi ürün sevk düğmesine Tıklandıktan sonra görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>4. adım: ürün ekleme

Sevkiyat düğmesi s ürünleri ürün ekleme veritabanına kaydetmek için Bu öğretici için kalan tüm `Click` olay işleyicisi. Bu oluşturarak yapılabilir bir `ProductsDataTable` ekleyerek bir `ProductsRow` sağlanan ürün adlarının her biri için örneği. Bu kez `ProductsRow` s, biz bir çağrı yapacak eklenmiştir `ProductsBLL` s sınıfı `UpdateWithTransaction` tümleştirilmesidir yöntemi `ProductsDataTable`. Bu geri çağırma `UpdateWithTransaction` geri oluşturulduğu yöntemi [veritabanı değişikliklerini bir işlemin içinde sarmalama](wrapping-database-modifications-within-a-transaction-cs.md) Öğreticisi, geçişleri `ProductsDataTable` için `ProductsTableAdapter` s `UpdateWithTransaction` yöntemi. Burada, ADO.NET işlem başlatılır ve TableAdatper sorunları bir `INSERT` veritabanına eklenen her deyim `ProductsRow` DataTable. İşlem, hata eklenen tüm ürünleri varsayıldığında, aksi takdirde, geri alınır.

Sevkiyat düğmesi s ürün ekleme kodunu `Click` olay işleyicisi biraz hata denetimi gerçekleştirmek de gerekir. Ekleme arabiriminde kullanılan hiçbir RequiredFieldValidators olduğundan, bir kullanıcı adı içermeden bir ürün için fiyat girebilirsiniz. Ürün s ad gerekli olduğundan bir koşul açılan biz kullanıcıyı uyarmak ve ekler devam değil gerekir. Tam `Click` olay işleyici kodu izler:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Olay işleyicisi, sağlayarak başlar `Page.IsValid` özellik değerini döndürdüğünde `true`. Döndürürse `false`, ardından bir anlamına CompareValidators birkaçı geçersiz veri raporlama; böyle bir durumda girilen Ürün eklemeye çalışırsanız istiyoruz değil veya şu özel durumla kullanıcı tarafından girilen birim fiyatı atamak çalışırken edersiniz değerini `ProductsRow` s `UnitPrice` özelliği.

Ardından, yeni bir `ProductsDataTable` örneği oluşturulur (`products`). A `for` döngü, ürün adı ve birim fiyatı metin kutuları yineleme için kullanılır ve `Text` özellikleri yerel değişkenlere okuma `productName` ve `unitPrice`. Birim fiyatı için ilgili ürün adı, ancak bir değer kullanıcının girdiği varsa `StatusLabel` görüntüler fiyat birimi sağlarsanız, ileti gerekir ayrıca ürün adını içerir ve olay işleyicisi çıkıldı.

Bir ürün adı, yeni bir sağlamışsa `ProductsRow` örneği kullanılarak oluşturulan `ProductsDataTable` s `NewProductsRow` yöntemi. Bu yeni `ProductsRow` örneği s `ProductName` özelliği geçerli bir ürün için ayarlanmış sırasında metin adı `SupplierID` ve `CategoryID` için atanan özellikler `SelectedValue` ekleme arabirimi s üst bilgisindeki DropDownList özellikleri. Ürün s fiyatı kullanıcı girilen değer, atandığı `ProductsRow` örneği s `UnitPrice` özelliği; Aksi takdirde, özelliğidir sonuçlanır atanmamış, soldaki bir `NULL` değerini `UnitPrice` veritabanında. Son olarak, `Discontinued` ve `UnitsOnOrder` özellikleri sabit kodlanmış değerler atanır `false` ve 0, sırasıyla.

Özellikler için atandıktan sonra `ProductsRow` eklenir örneği `ProductsDataTable`.

Tamamlandığında `for` döngü, biz denetleyin ürünlerden eklenip eklenmediğini. Kullanıcı, herhangi bir ürün adları veya fiyatları ulaşmadan önce sevkiyat ekleme ürünleri tıkladı olabilir. En az bir üründe olduğunda `ProductsDataTable`, `ProductsBLL` s sınıfı `UpdateWithTransaction` yöntemi çağrılır. Ardından, verileri için DataSet'e `ProductsGrid` GridView böylece yeni eklenen ürünleri görüntü arabiriminde görünür. `StatusLabel` Bir onay iletisi görüntüleyecek şekilde güncelleştirildi ve `ReturnToDisplayInterface` , arabirim ekleme ve görüntüleme arabirimi gösteren gizleme çağrılır.

Ürün yok girilirse ekleme arabirimi iletinin ürün eklendi görüntülenmeye devam eder. Lütfen ürün adlarını girin ve birim fiyatına metin kutuları görüntülenir.

Şekil s 13, 14 ve 15 ekleme Göster ve arabirimleri eylemi görüntüler. Şekil 13'te, kullanıcı bir birim fiyat değeri karşılık gelen bir ürün adı olmadan geçti. Şekil 14 görünen arabirim üç sonra yeni Şekil 15 iki yeni eklenen ürün (üçüncü önceki sayfada) GridView gösterir ancak ürünler başarıyla eklenen gösterir.


[![Gerekli olduğunda girerek bir birim fiyatı ürün adıdır](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Şekil 13**: bir ürün adı olan gerekli olduğunda girerek bir birim fiyatı ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image39.png))


[![Sağlayıcı için üç yeni Veggies eklenmiştir Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Şekil 14**: üç yeni Veggies eklenmiştir tedarikçi Mayumi s ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image42.png))


[![Yeni ürün GridView son sayfasında bulunabilir.](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Şekil 15**: yeni ürünler bulunabilir GridView'ın son sayfa ([tam boyutlu görüntüyü görmek için tıklatın](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> Bu öğreticide kullanılan mantığı ekleme toplu işlem kapsamında ekler sarmalar. Bunu doğrulamak için bir veritabanı düzeyinde hata kullanılamıyor.%n%nÇözüm tanıtır. Örneğin, yeni atama yerine `ProductsRow` örneği s `CategoryID` seçili değer özelliğini `Categories` DropDownList, atama için bir değer ister `i * 5`. Burada `i` döngü Indexer ve 1 ile 5 arasında değişen bir değer. Bu nedenle, iki veya daha fazla ürünleri toplu işlemindeki ilk Ürün Ekle ekleme olduğunda geçerli bir `CategoryID` değeri (5), ancak sonraki ürünleri olacaktır `CategoryID` kadar eşleşmeyen değerler `CategoryID` değerler `Categories` tablo. Net etkisiyle olan ilk `INSERT` başarılı olur, sonraki olanları bir yabancı anahtar kısıtlaması ihlali ile başarısız olur. Toplu INSERT atomic, olduğundan ilk `INSERT` toplu işlem eklemeden durumuna veritabanı başlangıcından döndüren geri alınacak.


## <a name="summary"></a>Özet

Bu ve önceki iki öğreticiler güncelleştirmek için silme, izin arabirimleri oluşturduk ve toplu veri ekleme, her biri işlem desteği ekledik veri erişim katmanı için kullanılan [veritabanı değişikliklerini sarmalama bir işlem içinde](wrapping-database-modifications-within-a-transaction-cs.md) öğretici. Belirli senaryoları, gibi toplu işleme kullanıcı arabirimleri büyük ölçüde son kullanıcı verimliliği keserek ayrıca temel alınan verilerin bütünlüğünü sürdürürken tıklama, Geri göndermeler ve klavye ve fare bağlam anahtarları sayısını artırın.

Bu öğreticide, toplu verilerle çalışma bizim göz tamamlar. Sonraki öğreticiler kümesini çeşitli DAL, bağlantı dizeleri şifreleme ve daha fazla bağlantı ve komut düzeyi ayarlarını yapılandırma TableAdapter s yöntemleriyle saklı yordamlar kullanma dahil olmak üzere, Gelişmiş Veri erişim katmanı senaryoları ele!

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Gözden geçirenler, Bu öğretici için olan ren Jacob Lauritsen Hilton Giesenow ve S sağlama. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](batch-deleting-cs.md)
> [İleri](wrapping-database-modifications-within-a-transaction-vb.md)
