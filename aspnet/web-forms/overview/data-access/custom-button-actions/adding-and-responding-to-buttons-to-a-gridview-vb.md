---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Ekleme ve bunları yanıtlama (VB) GridView'a düğme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir şablona ve alanları GridView veya DetailsView denetimi için özel düğmeler, ekleme şu konuları inceleyeceğiz. Özellikle, satırını gerekir...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0834d43f95bd19fffb603dcde640714bd779fd80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752954"
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Ekleme ve bunları yanıtlama (VB) GridView'a düğme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) veya [PDF olarak indirin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> Bu öğreticide bir şablona ve alanları GridView veya DetailsView denetimi için özel düğmeler, ekleme şu konuları inceleyeceğiz. Özellikle, tedarikçileri sayfasında kullanıcıya izin veren bir FormView'da arabirimdeki oluşturacağız.


## <a name="introduction"></a>Giriş

Rapor verilerine salt okunur erişim raporlama birçok senaryo içerir ancak bu eylemleri gerçekleştirme olanağı dahil etmek için raporları değil seyrek s tabanlı görüntülenen veriler. Genellikle Bu raporda görüntülenen her bir kayıt ile düğme, LinkButton veya ImageButton Web denetim ekleme dahil, tıklandığında geri göndermeye neden olur ve bazı sunucu tarafı kodu çağırır. Kayıt kayıt temelinde verileri düzenleme ve silmeye en yaygın örnektir. Aslında, başlayarak gördüğümüz gibi [genel bakış, ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) FormView GridView ve DetailsView denetimlerini olmadan bu işlevselliğin destekleyebilir kadar sık Öğreticisi, düzenleme ve silme tek satırlık bir kod yazmak için gerekir.

Buna ek olarak düzenlemek ve düğmeler, GridView, DetailsView ve FormView silmek için denetimleri de düğmeler, LinkButtons veya ImageButtons içerebilir, tıklandığında, özel sunucu tarafı mantık gerçekleştirin. Bu öğreticide bir şablona ve alanları GridView veya DetailsView denetimi için özel düğmeler, ekleme şu konuları inceleyeceğiz. Özellikle, tedarikçileri sayfasında kullanıcıya izin veren bir FormView'da arabirimdeki oluşturacağız. İçin belirli bir tedarikçi, FormView tedarikçi seçeneğine tıkladıysanız, tüm bunların ilişkili ürünleri kullanımdan olarak işaretler, bir düğme Web denetimi hakkında bilgi gösterir. Ayrıca, GridView artırmak fiyat ve indirimli fiyat, seçeneğine tıkladıysanız, yükseltmek veya ürün s azaltmak düğmeler içeren her satır seçili sağlayıcı tarafından sağlanan bu ürünlerin listeler `UnitPrice` 10 oranında (bkz. Şekil 1).


[![FormView ve GridView özel eylemleri gerçekleştiren düğmeleri içerir](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Şekil 1**: her iki FormView ve GridView içeren düğmeler, özel eylemleri ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>1. adım: düğme Eğitmen Web sayfaları ekleme

Özel bir düğme eklemek nasıl tümleştirildiği incelenmektedir önce öncelikle Bu öğretici için yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `CustomButtons`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki iki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `CustomButtons.aspx`


![Özel düğmeler ilgili öğreticiler için ASP.NET sayfaları ekleme](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Şekil 2**: özel düğmeler ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `CustomButtons` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Şekil 3**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Son olarak, girişleri olarak istediğiniz sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme sayfalama ve sıralama sonra eklemeniz `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık düzenleme, ekleme ve silme öğreticiler için öğeleri içerir.


![Site Haritası, giriş artık özel düğmeler öğretici içerir.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Şekil 4**: Site Haritası, giriş artık özel düğmeler öğretici içerir.


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>2. adım: tedarikçileri listeleyen bir FormView'da ekleme

Bu öğreticiyle tedarikçileri listeler FormView ekleyerek başlayın s olanak tanır. Giriş açıklandığı gibi bu FormView sayfası aracılığıyla gösteren GridView tedarikçi sağladığı ürünler Tedarikçiler, kullanıcıya izin verir. Ayrıca, bu FormView bir düğme içerir, tıklandığında, tüm tedarikçi s ürünleri kullanımdan olarak işaretler. Biz kendimize FormView için özel bir düğme ekleme ile ilgili önce ilk üretici bilgilerini görüntüler yalnızca FormView oluşturun s olanak tanır.

Başlangıç açarak `CustomButtons.aspx` sayfasını `CustomButtons` klasör. Bir FormView'da kümesi ve Tasarımcısı araç kutusundan sürükleyerek sayfaya ekleyin, `ID` özelliğini `Suppliers`. FormView s akıllı etiketten adlı yeni bir ObjectDataSource oluşturmak için iyileştirilmiş `SuppliersDataSource`.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Şekil 5**: adlı yeni bir ObjectDataSource oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Bu yeni ObjectDataSource gelen sorgular gibi yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` metodu (bkz. Şekil 6). Bu FormView (hiçbiri) güncelleştirme sekmeyi aşağı açılan listeden seçeneğini tedarikçi bilgi, select güncelleştirmek için bir arabirimi sağlamadığından.


[![S GetSuppliers() yöntemi SuppliersBLL sınıfını kullanmak için bu veri kaynağını yapılandırma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Şekil 6**: kullanmak için bu veri kaynağını yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio oluşturacak bir `InsertItemTemplate`, `EditItemTemplate`, ve `ItemTemplate` FormView için. Kaldırma `InsertItemTemplate` ve `EditItemTemplate` ve değiştirme `ItemTemplate` yalnızca tedarikçi s şirket adını ve telefon numarasını görüntüler. Son olarak, disk belleği desteği FormView akıllı etiketinde sayfalama etkinleştir onay kutusunu işaretleyerek etkinleştirmek (veya ayarlayarak onun `AllowPaging` özelliğini `True`). Bu değişikliklerden sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Şekil 7 bir tarayıcıdan görüntülendiğinde CustomButtons.aspx sayfada gösterilir.


[![FormView CompanyName ve şu anda seçili tedarikçiden telefon alanlarını listeler.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Şekil 7**: FormView listeler `CompanyName` ve `Phone` şu anda seçili sağlayıcı alanlardan ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>3. adım: seçili sağlayıcı s ürünleri listeleyen GridView ekleme

Tüm ürünleri Durdur düğmesini FormView s şablona ekleyebilmek ilk olarak seçili sağlayıcı tarafından sağlanan olduğu ürünleri listeler FormView altındaki GridView ekleyin s olanak tanır. Bunu başarmak eklemek için GridView sayfasına ayarlayın, `ID` özelliğini `SuppliersProducts`, adlı yeni bir ObjectDataSource ekleyin `SuppliersProductsDataSource`.


[![SuppliersProductsDataSource adlı yeni bir ObjectDataSource oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Şekil 8**: adlı yeni bir ObjectDataSource oluşturma `SuppliersProductsDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


S ProductsBLL sınıfını kullanmak için bu ObjectDataSource yapılandırma `GetProductsBySupplierID(supplierID)` metodu (bkz. Şekil 9). Bu GridView ayarlanması için bir ürün s fiyatı izin verir ancak kazandığını t düzenleme veya GridView ' özellikleri silme yerleşik kullanıyor. Bu nedenle, biz UPDATE, INSERT ve DELETE sekmeler için ObjectDataSource s (hiçbiri) açılan listeye ayarlayabilirsiniz.


[![S GetProductsBySupplierID(supplierID) yöntemi ProductsBLL sınıfını kullanmak için bu veri kaynağını yapılandırma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Şekil 9**: kullanmak için bu veri kaynağını yapılandırma `ProductsBLL` s sınıfı `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Bu yana `GetProductsBySupplierID(supplierID)` yöntemi kabul giriş parametresi, bize Bu parametre değerinin kaynağı ObjectDataSource Sihirbazı'nı ister. İçinde geçirilecek `SupplierID` değer FormView seçin için parametre kaynak aşağı açılan liste denetimi ve ControlId aşağı açılan listeye ayarlayın `Suppliers` (FormView kimliği 2. adımda oluşturmuştunuz).


[![SatýrýnSupplierID tedarikçileri FormView denetiminden parametresi gelmesi gerektiğini belirtmek](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Şekil 10**: belirtmek *`supplierID`* gereken parametre gelen `Suppliers` FormView denetimi ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


ObjectDataSource sihirbazını tamamladıktan sonra GridView BoundField veya CheckBoxField her ürün s veri alanlarını içerir. Let s trim Bu aşağı göstermek için yalnızca `ProductName` ve `UnitPrice` BoundFields ile birlikte `Discontinued` CheckBoxField; Ayrıca, s biçimi izin `UnitPrice` BoundField sağlayacak şekilde metni bir para birimi olarak biçimlendirilir. GridView ve `SuppliersProductsDataSource` ObjectDataSource s bildirim temelli biçimlendirme, aşağıdaki işaretlemede benzer görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

Bu noktada öğreticimize FormView üst öğesinden bir sağlayıcı seçin ve altındaki GridView aracılığıyla üretici tarafından sağlanan ürünleri görüntülemek için kullanıcının bir ana/ayrıntılar raporu görüntüler. Şekil 11 Tokyo Traders tedarikçi FormView seçerken bu sayfanın ekran görüntüsü gösterilmektedir.


[![Seçili sağlayıcı s ürünleri GridView içinde görüntülenir.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Şekil 11**: seçili sağlayıcı s ürünleri GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>4. adım: DAL ve BLL yöntemleri tüm ürünler için bir sağlayıcı kesmek için oluşturma

Bir düğme için FormView eklemeden önce tıklandığında, tüm sona erdirir tedarikçi s ürünlerinin ilk DAL ve bu eylem gerçekleştiren BLL için bir yöntem eklemek ihtiyacımız. Özellikle, bu yöntem adlandırılacağını `DiscontinueAllProductsForSupplier(supplierID)`. FormView s düğmesine tıklandığında, biz bu yöntemi, iş mantığı katmanı geçirme seçili tedarikçi s çağırma `SupplierID`; BLL verecek karşılık gelen veri erişim katmanı yönteme sonra çağıran bir `UPDATE` deyimi Belirtilen sağlayıcı s ürünleri sona erdirir veritabanı.

Önceki öğreticilerimizden uyguladığımız gibi bir aşağıdan yukarıya yaklaşım DAL yöntemi, ardından BLL yöntemi oluşturma ve son olarak ASP.NET sayfasında işlevlerini uygulama ile başlayan kullanacağız. Açık `Northwind.xsd` türü belirtilmiş veri kümesi `App_Code/DAL` klasör ve bir yeni yöntem ekleyin `ProductsTableAdapter` (sağ `ProductsTableAdapter` ve Sorgu Ekle seçin). Bunun yapılması bize yeni yöntem ekleme işleminde size kılavuzluk eder TableAdapter sorgu Yapılandırma Sihirbazı çıkarır. Bizim DAL yöntemi geçici SQL deyimi kullandığınızı belirterek başlayın.


[![Geçici SQL deyimi kullanarak DAL yöntemi oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Şekil 12**: DAL yöntemini kullanarak bir geçici SQL ifadesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Ardından, sihirbaz bize ne tür bir sorgu oluşturmak için farklı ister. Olduğundan `DiscontinueAllProductsForSupplier(supplierID)` yöntemi güncelleştirmeniz gerekir `Products` ayarlama, veritabanı tablosu `Discontinued` alanında belirtilen tarafından sağlanan tüm ürünler için 1 *`supplierID`*, verileri güncelleştiren bir sorgu için oluşturmamız gerekir.


[![Güncelleştirme sorgu türünü seçin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Şekil 13**: güncelleştirme sorgu türünü seçin ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


TableAdapter mevcut s sonraki sihirbaz ekranında sağlar `UPDATE` tanımlanan alanların her biri güncelleştiren deyimi `Products` DataTable. Bu sorgu metni, aşağıdaki ifade ile değiştirin:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Bu sorgu girme ve son Sihirbazı ekran için yeni yöntem s adını ister İleri tıklayarak sonra kullanın `DiscontinueAllProductsForSupplier`. Son düğmesini tıklatarak Sihirbazı tamamlayın. Veri kümesini tasarımcıya döndüren bağlı yeni bir yöntemde görmelisiniz `ProductsTableAdapter` adlı `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Yeni DAL yöntemi DiscontinueAllProductsForSupplier adı](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Şekil 14**: yeni DAL yöntem adı `DiscontinueAllProductsForSupplier` ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


İle `DiscontinueAllProductsForSupplier(supplierID)` bizim sonraki görev veri erişim katmanında oluşturulan yöntemi olduğundan oluşturmak için `DiscontinueAllProductsForSupplier(supplierID)` iş mantığı katmanı yöntemi. Bunu gerçekleştirmek için açık `ProductsBLL` sınıf dosyası ve aşağıdakileri ekleyin:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Bu yöntem yalnızca aşağı çağırır `DiscontinueAllProductsForSupplier(supplierID)` sağlanan geçirme DAL yönteminde *`supplierID`* parametre değeri. Yalnızca belirli koşullar altında sona erecek şekilde s ürünleri bir tedarikçi izin verilen herhangi bir iş kuralları varsa, bu kuralları burada BLL uygulanmalıdır.

> [!NOTE]
> Farklı `UpdateProduct` de aşırı `ProductsBLL` sınıfı `DiscontinueAllProductsForSupplier(supplierID)` yöntem imzası içermez `DataObjectMethodAttribute` özniteliği (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Bu ışığının `DiscontinueAllProductsForSupplier(supplierID)` UPDATE sekmesi ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı'nı s aşağı açılan listeden yöntemi. Ben ve biz çağırma çünkü bu öznitelik atlanmış `DiscontinueAllProductsForSupplier(supplierID)` doğrudan ASP.NET sayfamızı bir olay işleyicisi yöntemi.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>5. adım: Ekleme bir FormView tüm ürünleri düğmeye Durdur

İle `DiscontinueAllProductsForSupplier(supplierID)` BLL ve DAL yöntemi tamamlandı, seçili sağlayıcı FormView s düğmesi Web denetimi eklemek için tüm ürünler bulunmayarak özelliği eklemek için son adım `ItemTemplate`. Let s düğme metnini, kesmek tüm ürünler tedarikçi s telefon numarası aşağıda böyle düğme ekleyin ve bir `ID` özelliği değerinin `DiscontinueAllProductsForSupplier`. FormView s akıllı etiketinde Şablonları Düzenle bağlantısına tıklayarak bu düğme Web denetimi Tasarımcısı aracılığıyla ekleyebilirsiniz (bkz. Şekil 15), veya doğrudan bildirim temelli söz dizimi.


[![Ekleme bir FormView s ItemTemplate için tüm ürünleri düğme Web Denetimi Durdur](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Şekil 15**: kesmek tüm ürünleri düğmesi Web denetim FormView s ekleme `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Ne zaman düğmesine tıklandığında bir kullanıcı ziyaret edin sayfasında, bir geri gönderme ensues ve FormView s tarafından [ `ItemCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) ateşlenir. Bu düğmeye tıkladı yanıt özel kod yürütmek için bu olay için bir olay işleyicisi oluşturabiliriz. , Yine de anlamak `ItemCommand` olay ateşlenir *herhangi* düğmesi, LinkButton veya ImageButton Web denetimi FormView içinde tıkladı. Diğer bir deyişle, kullanıcı bir sayfadan diğerine FormView getirdiğinde `ItemCommand` olay harekete geçirilir; kullanıcı yeni, düzenleme, tıklar veya ekleme, güncelleme veya silme destekleyen bir FormView'da Sil aynı şey.

Bu yana `ItemCommand` bakılmaksızın olay işleyicisi ihtiyacımız tüm ürünleri Durdur düğmesine tıklandığını belirleme için bir yol veya başka bir düğme ise hangi düğmeye tıklandığında harekete geçirilir. Bunu yapmak için biz s düğmesi Web denetimi ayarlayabilirsiniz `CommandName` özelliğini bazı tanımlayan bir değer. Ne zaman düğmesine tıklandığında, bu `CommandName` içine geçirilen değer `ItemCommand` olay işleyicisi, tüm ürünleri Durdur düğmesini tıklanan düğme olup olmadığını belirlemek için bize etkinleştirme. Tüm ürünleri düğmesi Bulunmayarak s ayarlamak `CommandName` DiscontinueProducts özelliği.

Son olarak, kullanıcının gerçekten seçili sağlayıcı s ürünleri devam etmek istediğini emin olmak için bir istemci-tarafı Onayla iletişim kutusunu kullanın s olanak tanır. İçinde gördüğümüz gibi [ekleme istemci tarafı doğrulama zaman silme](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) öğretici, bu gerçekleştirilebilir JavaScript bir bit. Özellikle düğme Web denetimi s OnClientClick özelliğini ayarlayın `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Bu değişiklikleri yaptıktan sonra FormView s bildirim temelli söz dizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Ardından, FormView s için bir olay işleyicisi oluşturun `ItemCommand` olay. Bu olay işleyicisinde biz önce tüm ürünleri Durdur düğmesine tıklandığını olup olmadığını belirlemeniz gerekir. Bu nedenle, biz bir örneğini oluşturmak istiyorsanız `ProductsBLL` sınıfı ve çağırma kendi `DiscontinueAllProductsForSupplier(supplierID)` tümleştirilmesidir yöntemi `SupplierID` , seçili FormView:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Unutmayın `SupplierID` FormView'da geçerli seçili sağlayıcısının FormView s kullanılarak erişilebilir [ `SelectedValue` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Özelliği ilk veri anahtar FormView'da görüntülenmesini kaydın değerini döndürür. FormView s [ `DataKeyNames` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), verilerin anahtar değerlerini alanları çekilen, verileri gösteren otomatik olarak ayarlandığı `SupplierID` ObjectDataSource FormView geri bağlanırken Visual Studio tarafından Adım 2'de.

İle `ItemCommand` oluşturulan olay işleyicisi sayfayı test etmek için biraz yararlanın. Cooperativa de Quesos için Gözat 'Las Cabras' tedarikçi (Bu benim için FormView'da beşinci tedarikçi s). İkisi de olan iki ürünler, Queso Cabrales ve Queso Manchego La Pastora, bu sağlayıcı sağlar *değil* kullanımdan kaldırıldı.

Cooperativa de Quesos 'Las Cabras' faaliyet geçti ve bu nedenle sona erecek şekilde ürünlerinden olduğunu hayal edin. ' E tıklayın tüm ürünleri düğmesi bırakmaktır. Bu istemci-tarafı Onayla iletişim kutusu görüntüler (bkz. Şekil 16) kutusunda.


[![Cooperativa de Quesos Las Cabras kaynakları iki etkin ürünler](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Şekil 16**: Cooperativa de Quesos Las Cabras kaynakları iki etkin ürünler ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


İstemci tarafı Onayla iletişim kutusunda Tamam'ı, form gönderme, geri göndermeye neden devam FormView s `ItemCommand` olayını ateşle. Çağrılırken, oluşturduğumuz olay işleyicisi sonra yürütülür `DiscontinueAllProductsForSupplier(supplierID)` yöntemi ve Queso Cabrales hem Queso Manchego La Pastora ürünleri durduruluyor.

GridView s görünüm durumu devre dışı bıraktıysanız, GridView'temel alınan veri deposuna her geri göndermede DataSet'e ve bu nedenle hemen (bkz. Şekil 17), bu iki ürün artık üretilmeyen yansıtacak şekilde güncelleştirilir. Ancak, GridView Görünüm durumuna devre dışı bıraktığınız değil, veri GridView'ın bu değişikliği yaptıktan sonra el ile yeniden bağlamanız gerekecektir. Bunu yapmak için basitçe GridView s çağrı yapmak `DataBind()` yöntemi çağırma hemen sonra `DiscontinueAllProductsForSupplier(supplierID)` yöntemi.


[![Tüm ürünler Durdur düğmesine Tıklandıktan sonra tedarikçi s ürün uygun şekilde güncelleştirilmiş değil](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Şekil 17**: tüm ürünleri Durdur düğmesine Tıklandıktan sonra tedarikçi s ürünleri uygun şekilde güncelleştirilmiş olan ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>6. adım: bir UpdateProduct aşırı bir ürün s fiyatı ayarlamak için iş mantığı katmanı oluşturma

Gibi tüm ürünleri Durdur düğmesini FormView'da, artan ve azalan GridView bir ürün için fiyat için düğmeler eklemek için ilk olarak uygun veri erişim katmanı ve iş mantığı katmanı yöntemleri eklemek ihtiyacımız var. Biz zaten bir DAL tek ürün satır güncelleştiren bir yöntem olduğundan, biz için yeni bir aşırı yükleme oluşturarak bu işlevselliği sağlayabilen `UpdateProduct` BLL yöntemi.

Bizim geçmiş `UpdateProduct` aşırı ürün alanları bileşimi skaler giriş değerleri olarak yaptıktan ve ardından belirtilen ürün için yalnızca bu alanları güncelleştirdik. Bu aşırı yükleme için Biz bu standart biraz farklı ve bunun yerine ürünün s geçirin `ProductID` ve yüzde olarak ayarlamak `UnitPrice` (yeni geçirme aksine ayarlanmış `UnitPrice` kendisini). Bu yaklaşım ASP.NET sayfası arka plan kod sınıfında yazmak için ihtiyacımız kod basitleştirir, biz ki bu yana t sahip geçerli bir ürün s belirleme ile rahatsız etmeyi `UnitPrice`.

`UpdateProduct` Bu öğretici, aşağıda gösterilen için aşırı yükleme:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Bu aşırı yükleme DAL s aracılığıyla belirtilen ürün hakkındaki bilgileri alır `GetProductByProductID(productID)` yöntemi. Ardından bakar olmadığını s ürün `UnitPrice` veritabanı atanmış `NULL` değeri. Bu durumda, fiyat bırakılır değiştirilmemiş. Ancak, varsa, olmayan bir`NULL` `UnitPrice` değeri, yöntem s ürün güncelleştirmeleri `UnitPrice` belirtilen yüzde (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>7. adım: GridView'a artırma ve azaltma düğmeleri ekleme

GridView (ve DetailsView) hem de alanlarının bir koleksiyonu oluşturulur. BoundFields CheckBoxFields ve TemplateField ek olarak, ASP.NET adından da anlaşılacağı gibi her satır için bir düğme, LinkButton veya ImageButton sahip bir sütun olarak işler ButtonField içerir. Benzer şekilde tıklayarak FormView *herhangi* düğmesi GridView sayfalama düğmeleri, düzenleme veya silme düğmeler, sıralama düğmeler ve benzeri içindeki geri göndermeye neden olur ve GridView s başlatır [ `RowCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

ButtonField sahip bir `CommandName` her düğmeleri için belirtilen değer atayan özelliği `CommandName` özellikleri. FormView ile gibi `CommandName` değeri tarafından kullanılan `RowCommand` hangi düğmesine tıklandığını belirleme için olay işleyicisi.

Let s GridView bir düğme metni fiyatı + 10 ile iki yeni ButtonFields eklemek % ve diğer metin fiyat -10 ile %. Bu ButtonFields eklemek, GridView s akıllı etiketinde sütunları Düzenle bağlantısına tıklayın, sol üstteki listede ButtonField alan türünü seçin ve Ekle düğmesine tıklayın.


![GridView'a iki ButtonFields Ekle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Şekil 18**: iki ButtonFields GridView'a Ekle


İki ButtonFields ilk iki GridView alanları olarak görünecek şekilde taşıyın. Ardından, ayarlama `Text` + %10 fiyat için bu iki ButtonFields ve fiyat -10 özelliklerini % ve `CommandName` IncreasePrice ve DecreasePrice, özellikleri sırasıyla. Varsayılan olarak, bir ButtonField düğme, sütunu LinkButtons işler. Bu, ancak ButtonField s değiştirilebilir [ `ButtonType` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Bu iki ButtonFields normal düğmeler işlenen sahip s sağlar; Bu nedenle `ButtonType` özelliğini `Button`. Şekil 19, bu değişiklikleri yaptıktan sonra alanları iletişim kutusu gösterir; Aşağıdaki GridView s bildirim temelli biçimlendirme olur.


![ButtonFields metin, CommandName ve ButtonType özelliklerini yapılandırma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Şekil 19**: ButtonFields yapılandırma `Text`, `CommandName`, ve `ButtonType` özellikleri


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Oluşturulan bu ButtonFields ile son adım GridView s için bir olay işleyicisi oluşturmaktır `RowCommand` olay. Çünkü harekete varsa bu olay işleyicisi ya da fiyatı + %10 veya % düğmeleri fiyat -10 tıklattığında, gereksinimlerini belirlemek için `ProductID` satır, düğmesine tıklandığını ve ardından çağırmak için `ProductsBLL` s sınıfı `UpdateProduct` uygun geçirme yöntemi `UnitPrice` yüzde ayarlama ile birlikte `ProductID`. Aşağıdaki kod, bu görevleri gerçekleştirir:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Belirleyebilmesi `ProductID` satır için fiyatı + 10 % veya fiyat -10% düğmesine tıkladı, GridView s başvurmanız ihtiyacımız `DataKeys` koleksiyonu. Bu koleksiyon, belirtilen alanların değerlerini tutan `DataKeyNames` her GridView satırında özelliği. GridView s beri `DataKeyNames` özelliğinin ayarlandığı ProductID için Visual Studio tarafından ObjectDataSource GridView'a bağlanırken `DataKeys(rowIndex).Value` sağlar `ProductID` için belirtilen *rowIndex*.

ButtonField içinde otomatik olarak geçirir. *rowIndex* , düğmeye tıkladı aracılığıyla satırın `e.CommandArgument` parametresi. Bu nedenle, belirlemek için `ProductID` satır için fiyatı + 10 % veya fiyat -10% düğmesine tıkladı, kullanıyoruz: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Olarak tüm ürünleri Bulunmayarak düğmesiyle GridView s görünüm durumu, devre dışı bıraktıysanız GridView temel alınan veri deposuna her geri göndermede DataSet'e ve bu nedenle hemen tıklama ile oluşan bir fiyat değişikliği yansıtacak şekilde güncelleştirilir düğme ya da. Ancak, GridView Görünüm durumuna devre dışı bıraktığınız değil, veri GridView'ın bu değişikliği yaptıktan sonra el ile yeniden bağlamanız gerekecektir. Bunu yapmak için basitçe GridView s çağrı yapmak `DataBind()` yöntemi çağırma hemen sonra `UpdateProduct` yöntemi.

Şekil 20 büyükanne Kelly'nın yerimizle göre sağlanan ürünlerin görüntülerken sayfada gösterilir. Şekil 21, % düğmesini iki kez büyükanne'nın Böğürtlen dağıtabilir ve fiyat -10% düğme için bir kez için Northwoods Cranberry Sauce tıklatıldıktan + 10 fiyat sonra sonuçları gösterilmektedir.


[![GridView fiyatı + 10 içerir % ve % düğmeleri -10 fiyat](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Şekil 20**: GridView içeren fiyatı + %10 ve % düğmeleri -10 Fiyat ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Birinci ve üçüncü ürünün fiyatı + 10 fiyat güncelleştirilip güncelleştirilmediğini % ve % düğmeleri -10 fiyat](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Şekil 21**: fiyatları birinci ve üçüncü ürün güncelleştirildi + 10 fiyat üzerinden % ve % düğmeleri -10 Fiyat ([tam boyutlu görüntüyü görmek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> GridView (ve DetailsView) de düğmeler, LinkButtons veya kendi TemplateField için eklenen ImageButtons olabilir. GridView s BoundField ile tıklandığında bu düğmeler, bir geri gönderme anlamına şekilde yükseltme `RowCommand` olay. Ne zaman ekleme düğmeleri bir TemplateField içinde ancak s düğmesi `CommandArgument` ButtonFields kullanırken olduğu gibi satır dizini otomatik olarak ayarlanmadı. Satır dizini içinde tıklandığını düğmenin belirlemeniz gerekiyorsa `RowCommand` olay işleyicisi, s düğmesi el ile ayarlamanız gerekir `CommandArgument` bildirim temelli söz dizimini kullanarak kod gibi TemplateField içinde bir özellik:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Özet

FormView GridView ve DetailsView denetimlerini tüm düğmeler, LinkButtons veya ImageButtons içerebilir. Böyle düğme tıklandığında, bir geri göndermeye neden olur ve yükseltmek `ItemCommand` FormView ve DetailsView denetimlerini olayı ve `RowCommand` GridView olayı. Bu veri Web denetimleri kayıtlarını düzenleme veya silme gibi yaygın komut ile ilgili eylemler işlemek için yerleşik işlevselliğe sahiptir. Ancak, aynı zamanda çözmeye çalışacağız kullanım düğmeler, tıklandığında, kendi özel kod yürütme ile yanıt.

Bunu gerçekleştirmek için bir olay işleyicisi oluşturmak ihtiyacımız `ItemCommand` veya `RowCommand` olay. Bu olay işleyicisinde biz öncelikle gelen denetleme `CommandName` hangi düğmesine tıklandığını belirleme ve uygun özel eylem için değer. Bu öğreticide düğmeler ve ButtonFields tüm ürünler için belirtilen bir tedarikçi kesmek veya artırmak veya belirli bir ürünün fiyatı 10 oranında azaltmak için nasıl kullanılacağını gördük.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-and-responding-to-buttons-to-a-gridview-cs.md)
