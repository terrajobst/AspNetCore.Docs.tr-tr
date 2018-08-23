---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: (VB) GridView'ın alt bilgisinden yeni kayıt ekleme | Microsoft Docs
author: rick-anderson
description: GridView denetiminde veri yeni kayıt ekleme için yerleşik destek sağlamaz, ancak bu öğretici, GridView içerecek şekilde genişletmek nasıl gösterir. bir...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c661190125e818d3abaf54f50a0067d0944a956
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754038"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>(VB) GridView'ın alt bilgisinden yeni kayıt ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) veya [PDF olarak indirin](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> GridView denetiminde veri yeni kayıt ekleme için yerleşik destek sağlamaz, ancak bu öğreticide GridView ekleme arabirim içerecek şekilde genişletmek gösterilir.


## <a name="introduction"></a>Giriş

Bölümünde açıklandığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) öğretici GridView ve DetailsView FormView Web denetimleri her yerleşik veri değiştirme özelliklerini içerir. Bildirim temelli bir veri kaynağı denetimleri ile kullanıldığında, bu üç Web denetimleri hızla ve kolayca veri - değiştirmek için yapılandırılabilir ve tek satırlık bir kod yazmaya gerek kalmadan senaryolarda. Ne yazık ki yalnızca DetailsView ve FormView denetimleri yerleşik ekleme, düzenleme ve silme özelliklerini sağlar. GridView yalnızca düzenleme ve silme desteği sunar. Ancak, biraz dirsek yağ ile biz GridView ekleme arabirim içerecek şekilde genişletebilirsiniz.

GridView'a ekleme özellikleri eklemek, biz nasıl yeni kayıtları eklenir karar, ekleme arabirimi oluşturmak ve yeni kayıt eklemek için kod yazma yükümlü olursunuz. Bu öğreticide size GridView s altbilgi ekleme arabirim ekleme sırasında görünür (bkz. Şekil 1) satır. Altbilgi hücrenin her sütun için uygun veri koleksiyonu kullanıcı arabirimi öğesi (s ürün adı için bir metin kutusu, bir DropDownList tedarikçi, vb.) içerir. Ayrıca bir sütuna ihtiyacımız bir ekleme düğmesini, tıklandığında, bir geri göndermeye neden olur ve yeni bir kayıt eklemek `Products` altbilgi satırında sağlanan değerler kullanarak tablo.


[![Altbilgi satırında yeni ürün eklemek için bir arabirim sağlar.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Şekil 1**: alt bilgi satır, yeni ürün eklemek için bir arabirim sağlar ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>1. adım: GridView ürün bilgilerini görüntüleme

Biz kendimize GridView s altbilgi ekleme arabirimi oluşturma ile ilgili önce veritabanında olduğu ürünleri listeler sayfasına GridView ekleme s ilk odağı sağlar. Başlangıç açarak `InsertThroughFooter.aspx` sayfasını `EnhancedGridView` klasörü ve GridView s ayar Tasarımcısı araç kutusundan sürükleyip GridView `ID` özelliğini `Products`. Ardından, adlı yeni bir ObjectDataSource bağlamak için GridView s akıllı etiket kullanın `ProductsDataSource`.


[![ProductsDataSource adlı yeni bir ObjectDataSource oluşturma](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Şekil 2**: adlı yeni bir ObjectDataSource oluşturma `ProductsDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` ürün bilgisi almak için yöntemi. Bu öğretici için kesinlikle ekleme özelliklerini ekleyerek s odaklanmasına olanak tanır ve düzenleme ve silme hakkında endişelenmenize gerek kalmaz. Bu nedenle, Ekle sekmesine aşağı açılan listeden ayarlandığından emin olun `AddProduct()` ve UPDATE ve DELETE sekmeleri açılan listelerde (hiçbiri) ayarlanır.


[![ObjectDataSource s INSERT() yönteme map AddProduct yöntemi](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Şekil 3**: eşleme `AddProduct` ObjectDataSource s yöntemi `Insert()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Güncelleştirme ve silme sekmeler açılan listeleri (yok)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Şekil 4**: güncelleştirme ve silme sekmeleri açılan listeler (hiçbiri) olarak ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak alanları GridView'a karşılık gelen veri alanların her biri için ekler. Şimdilik, tüm Visual Studio tarafından eklenen alanları bırakın. Biz geri dönüp kaldırmak daha sonra Bu öğreticide bazı değerleri t ki alanları yeni kayıt eklerken belirtilmesi gerekir.

Veritabanında 80 ürünleri yakın olduğundan, bir kullanıcı yeni bir kayıt eklemek için tüm web sayfasının altındaki aşağı kaydırarak gerekecektir. Bu nedenle, ekleme arabirimi daha görünür ve erişilebilir hale getirmek ICollection s olanak tanır. Sayfalamayı etkinleştirmek için basitçe GridView s akıllı etiket disk belleğini etkinleştir onay denetleyin.

Bu noktada, GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Tüm ürün veri alanları disk belleğine alınan GridView görüntülenir](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Şekil 5**: tüm ürün veri alanları disk belleğine alınan GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>2. adım: alt bilgi satır ekleme

Üst bilgi ve veri satırları yanı sıra GridView altbilgi satır içerir. Üstbilgi ve altbilgi satırları GridView s değerlerine bağlı olarak görüntülenen [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) ve [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) özellikleri. Alt satır göstermek için ayarlamanız yeterlidir `ShowFooter` özelliğini `True`. Şekil 6 gösterildiği gibi ayarlama `ShowFooter` özelliğini `True` kılavuza altbilgi satırı ekler.


[![Altbilgi satırında görüntülenecek ShowFooter True olarak ayarlayın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Şekil 6**: alt bilgi satır görüntülenecek kümesi `ShowFooter` için `True` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


Alt satır koyu kırmızı arka plan rengi olduğuna dikkat edin. Bu DataWebControls oluşturulur ve tüm sayfalar için uygulanan temayı kaynaklanır geri [görüntüleyen veri ile ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) öğretici. Özellikle, `GridView.skin` dosya yapılandırır `FooterStyle` kullandığı özelliği bu tür `FooterStyle` CSS sınıfı. `FooterStyle` Sınıfı içinde tanımlanan `Styles.css` gibi:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Biz ve GridView s altbilgi satırın önceki öğreticilerde kullanarak incelediniz. Gerekirse, kiracıurl [özet bilgileri gösteren GridView'ın alt](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Yenileyici Öğreticisi.


Ayarlanmasından sonra `ShowFooter` özelliğini `True`, çıktıyı bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şu anda altbilgi satır eklenmemişse t herhangi bir metin veya Web denetimleri içerir. Adım 3'te uygun ekleme arabirimi içerir, böylece biz her GridView alan altbilgisi değiştireceksiniz.


[![Görüntülenen yukarıda sayfalama arabirimi denetimleri boş bir alt bilgi satırdır](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Şekil 7**: Boş altbilgi satırdır görüntülenen yukarıda sayfalama arabirimi denetimleri ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>3. adım: alt bilgi satırı özelleştirme

Geri [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici büyük ölçüde kullanma (aksine, BoundFields veya CheckBoxFields) belirli bir GridView Sütun görüntüsünü özelleştirmek nasıl gördük; [ Veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) TemplateField kullanma GridView içinde düzenleme arabirimini özelleştirme sırasında incelemiştik. Belirli kullanılan türleri satır geri çağırma bir TemplateField biçimlendirme, Web denetimleri ve veri bağlama söz dizimi karışımını tanımlayan bir dizi şablon oluşur. `ItemTemplate`, Örneğin, salt okunur satırlar için kullanılan şablonu belirtir ancak `EditItemTemplate` düzenlenebilir satır için bir şablon tanımlar.

İle birlikte `ItemTemplate` ve `EditItemTemplate`, TemplateField de içeren bir `FooterTemplate` altbilgi satır içeriğini belirtir. Bu nedenle, her alan s arabirimine eklemek için gereken Web denetimleri ekleyebiliriz `FooterTemplate`. Başlamak için tüm alanların GridView TemplateField için dönüştürün. Bu her alan sol alt köşede seçme ve bu alan dönüştürme TemplateField bağlantıya tıklandığında GridView s akıllı etiket sütunları Düzenle bağlantısına tıkladıktan yapılabilir.


![Her alanın bir TemplateField Dönüştür](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Şekil 8**: her alanı bir TemplateField dönüştürün


Dönüştür'ı tıklatarak bir eşdeğer TemplateField geçerli alan türü bu alana bir TemplateField kapatır. Örneğin, her BoundField bir TemplateField ile değiştirilmiştir bir `ItemTemplate` ilgili veri alanı görüntüleyen bir etiket içeren ve bir `EditItemTemplate` , bir metin kutusunda veri alanını görüntüler. `ProductName` BoundField aşağıdaki TemplateField biçimlendirme dönüştürülmüş:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Benzer şekilde, `Discontinued` CheckBoxField dönüştürülmüş bir TemplateField ayarlanmış `ItemTemplate` ve `EditItemTemplate` içeren bir onay kutusu Web denetimi (ile `ItemTemplate` s devre dışı onay kutusu). Salt okunur `ProductID` BoundField hem de bir etiket denetimi ile bir TemplateField içine dönüştürülmüş `ItemTemplate` ve `EditItemTemplate`. Kısacası, var olan bir GridView dönüştürme bir TemplateField alanına mevcut alan s işlevleri kaybetmeden için daha fazla özelleştirilebilir TemplateField geçmek için hızlı ve kolay bir yol olduğu.

GridView beri size destek t eklenmemişse düzenleme ile çalışmayı yeniden gönderebilirsiniz kaldırmak ücretsiz `EditItemTemplate` her TemplateField yalnızca bırakarak `ItemTemplate`. Bunu yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Her GridView alanın bir TemplateField dönüştürülmüş, size uygun ekleme arabirimi her alan s girebilirsiniz `FooterTemplate`. Bazı alanlar ekleme arabirime sahip olmaz (`ProductID`, örneği için); başkalarının yeni ürün s bilgilerini toplamak için kullanılan Web denetimlerinde farklılık gösterir.

Düzenleme arabirimi oluşturmak için GridView s akıllı etiketi Düzen şablonları bağlantıyı seçin. Ardından, açılan listeden uygun alanı s seçin `FooterTemplate` ve uygun denetimi Tasarımcısı araç kutusundan sürükleyin.


[![Her alan s FooterTemplate için uygun ekleme arabirimi ekleyin](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Şekil 9**: s her alan uygun ekleme arabirimi ekleyin `FooterTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


Aşağıdaki madde işaretli liste eklemek için ekleme arabirimi belirtme GridView alanları listelenmektedir:

- `ProductID` Yok.
- `ProductName` bir metin kutusu ekleme ve kendi `ID` için `NewProductName`. Kullanıcı yeni s ürün adı için bir değer girdiğinden emin olmak için de bir RequiredFieldValidator ekleyin.
- `SupplierID` Yok.
- `CategoryID` Yok.
- `QuantityPerUnit` ayarı, bir metin kutusu ekleyin, `ID` için `NewQuantityPerUnit`.
- `UnitPrice` adlı bir metin kutusu ekleme `NewUnitPrice` ve girilen değer sağlayan bir CompareValidator değerinden büyük veya sıfıra eşit bir para birimi değeri.
- `UnitsInStock` TextBox kullanma, `ID` ayarlanır `NewUnitsInStock`. Girilen değer değerinden büyük veya sıfıra eşit bir tamsayı olmasını sağlar bir CompareValidator içerir.
- `UnitsOnOrder` TextBox kullanma, `ID` ayarlanır `NewUnitsOnOrder`. Girilen değer değerinden büyük veya sıfıra eşit bir tamsayı olmasını sağlar bir CompareValidator içerir.
- `ReorderLevel` TextBox kullanma, `ID` ayarlanır `NewReorderLevel`. Girilen değer değerinden büyük veya sıfıra eşit bir tamsayı olmasını sağlar bir CompareValidator içerir.
- `Discontinued` ayarı, bir onay kutusu ekleme, `ID` için `NewDiscontinued`.
- `CategoryName` bir DropDownList ekleme ve kendi `ID` için `NewCategoryID`. Adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource` ve kullanacak şekilde yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi. DropDownList s sahip `ListItem` s görünen `CategoryName` verileri kullanarak, alan `CategoryID` veri alanı değerlerini olarak.
- `SupplierName` bir DropDownList ekleme ve kendi `ID` için `NewSupplierID`. Adlı yeni bir ObjectDataSource bağlama `SuppliersDataSource` ve kullanacak şekilde yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi. DropDownList s sahip `ListItem` s görünen `CompanyName` verileri kullanarak, alan `SupplierID` veri alanı değerlerini olarak.

Her doğrulama denetimleri Temizle `ForeColor` özelliği böylece `FooterStyle` CSS sınıfı s Beyaz ön plan rengini kırmızı varsayılan yerine kullanılır. Ayrıca `ErrorMessage` ayrıntılı bir açıklaması için özelliği ancak ayarlamak `Text` özelliği için bir yıldız işareti. Doğrulama denetimi s metin ekleme arabirimi iki satırdan uzun neden olmasını önlemek için ayarlanmış `FooterStyle` s `Wrap` özelliğini her biri için false `FooterTemplate` doğrulama denetimi kullanın, s. Son olarak GridView ve kümesi altında bir ValidationSummary denetimi ekleyin, `ShowMessageBox` özelliğini `True` ve kendi `ShowSummary` özelliğini `False`.

Yeni ürün eklerken sağlamak için ihtiyacımız `CategoryID` ve `SupplierID`. Bu bilgiler için alt bilgi hücrelerdeki DropDownList ile yakalanır `CategoryName` ve `SupplierName` alanları. Bu alanlar olarak kullanmak seçtiğim `CategoryID` ve `SupplierID` TemplateField s kılavuzunda, kullanıcı veri satırlarına olduğundan kimliği değerlerine yerine kategori ve tedarikçi adları görmeniz büyük olasılıkla daha ilgi. Bu yana `CategoryID` ve `SupplierID` değerleri artık yakalanır `CategoryName` ve `SupplierName` alan s ekleme arabirimleri, biz kaldırabilirsiniz `CategoryID` ve `SupplierID` TemplateField GridView öğesinden.

Benzer şekilde, `ProductID` yeni bir ürün eklerken kullanılmaz böylece `ProductID` TemplateField de kaldırılabilir. Ancak, s bırakın izin `ProductID` kılavuzunda alan. Metin kutuları, DropDownList, onay kutularını ve doğrulama denetimleri ekleme arabirimi oluşturan yanı sıra, ayrıca bir ekleme gerekir, düğmesine tıklandığında, yeni ürün eklemek için mantığı gerçekleştirir. Adım 4'te biz bir ekleme düğmesini ekleme arabiriminde içinde yer alacak `ProductID` TemplateField s `FooterTemplate`.

Çeşitli GridView alanları görünüşünü iyileştirmek çekinmeyin. Örneğin, biçimlendirmek isteyebilirsiniz `UnitPrice` değerleri bir para birimi olarak Sağa Hizala `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` alanları ve güncelleştirme `HeaderText` TemplateField değerleri.

Arabirimlerde ekleme slew oluşturduktan sonra `FooterTemplate` s, kaldırma `SupplierID`, ve `CategoryID` TemplateField ve estetik kılavuzunun biçimlendirme ve bildirim temelli, GridView s TemplateField hizalama aracılığıyla geliştirme biçimlendirme, aşağıdakine benzer görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

GridView s altbilgi satırı artık bir tarayıcıdan görüntülendiğinde, tamamlanmış içerir. ekleme arabirim (bkz. Şekil 10). Bu noktada, ekleme arabirimi eklenmemişse t, she s verileri yeni bir ürün için girilen ve veritabanına yeni bir kayıt eklemek isteyen kullanıcı için bir yol ekleyin. Ayrıca, biz ve altbilgi girilen verilerin yeni bir kaydın içine nasıl İngilizceye henüz yönelik `Products` veritabanı. Adım 4 baktığımızda bir ekleme düğmesini ekleme arabirimine nasıl eklendiğini ve kod yürütmek nasıl geri gönderme zaman, s tıkladı. 5. adım, altbilgi verileri kullanarak yeni bir kayıt eklemek gösterilmektedir.


[![GridView alt yeni bir kayıt eklemek için bir arabirim sağlar.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Şekil 10**: GridView alt yeni bir kayıt eklemek için bir arabirim sağlar ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>4. adım: bir ekleme düğmesini ekleme arabiriminde dahil.

Araç, yeni ürün s bilgileri girerek tamamladınız belirtmek için kullanıcı arabirimi şu anda ekleme altbilgi satır s eksik olduğundan bir ekleme düğmesini yere ekleme arabiriminde içerecek şekilde ihtiyacımız var. Bu varolan bir yerleştirilebilir `FooterTemplate` s veya ekleyebileceğiniz yeni bir sütun kılavuza bu amaç için. Bu öğreticide, let s yerleştirin Ekle düğmesini `ProductID` TemplateField s `FooterTemplate`.

Tasarımcıdan GridView s akıllı etiketinde Şablonları Düzenle bağlantısına tıklayın ve ardından `ProductID` alan s `FooterTemplate` aşağı açılan listeden. Düğmesi Web denetimi (veya bir LinkButton veya tercih ederseniz ImageButton) Kimliğini ayarlamak şablon eklemek `AddProduct`, kendi `CommandName` , eklenecek ve kendi `Text` Şekil 11'de gösterildiği gibi eklenecek özellik.


[![ProductID TemplateField s FooterTemplate Ekle düğmesini Yerleştir](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Şekil 11**: Düğme Ekle yerleştirin `ProductID` TemplateField s `FooterTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Bir tarayıcıda Sayfası Ekle düğmesini önceden bulunan sonra test edin. Geçersiz veri ekleme arabiriminde Ekle düğmesine tıklandığında, geri gönderme kısa circuited unutmayın ve (bkz. Şekil 12) geçersiz veri, ValidationSummary denetimi gösterir. Girilen uygun verilerle Ekle düğmesine tıklayın, geri göndermeye neden olur. Kayıt yok, ancak veritabanına eklenir. Biraz gerçekten INSERT gerçekleştirmek için kod yazma gerekecektir.


[![Ekleme arabiriminde geçersiz veri varsa Ekle düğmesi s geri gönderme kısa Circuited.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Şekil 12**: Düğme Ekle geri gönderme s'dir kısa Circuited ekleme arabiriminde geçersiz veri varsa ([tam boyutlu görüntüyü görmek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> Doğrulama denetimleri ekleme arabiriminde bir doğrulama grubuna atanmış olmadığından. Yalnızca kümesini sayfasında doğrulama denetimleri ekleme arabirimi olduğu sürece düzgün çalışır. Ancak, varsa, (örneğin, doğrulama denetimleri kılavuz s düzenleme arabiriminde) sayfasında diğer doğrulama denetimleri, doğrulama denetimleri ekleme, arabirim ve ekleme düğmesi `ValidationGroup` özellikleri aynı değer atanmalıdır so olarak için Bu denetimler belirli doğrulama grubuyla ilişkilendirin. Bkz: [doğrulama denetimleri ASP.NET 2.0 ayrıntıları](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) sayfasında düğmeler ve doğrulama denetimleri doğrulama gruplar halinde bölümleme hakkında daha fazla bilgi.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>5. adım: yeni bir kayıt ekleme`Products`tablo

GridView'ın yerleşik düzenleme özelliklerini kullanırken GridView otomatik olarak güncelleştirmeyi gerçekleştirmek için gerekli iş tüm işler. Özellikle, bu kopyalar düzenleme arabiriminden ObjectDataSource s parametrelerinde için girilen değerler güncelleştir düğmesine tıklandığında `UpdateParameters` toplama ve ObjectDataSource s çağırarak güncelleştirme kapalı kicks `Update()` yöntemi. GridView ekleme gibi yerleşik bir işlevi sağlamadığından ObjectDataSource s çağıran kod kodumuza `Insert()` yöntemi ve kopya ekleme gelen değerleri arabirim ObjectDataSource s `InsertParameters` koleksiyonu .

Ekle düğmesine tıkladıktan sonra bu INSERT mantık yürütülmelidir. Bölümünde açıklandığı gibi [ekleme ve GridView düğmeleri yanıtlama](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) öğretici, bir düğme, LinkButton veya GridView içinde ImageButton tıklandığında zaman GridView s `RowCommand` üzerinde geri gönderme olayı harekete geçirilir. Düğme, LinkButton veya ImageButton açıkça alt Satır Ekle düğmesini gibi eklendi olup olmadığını veya onu GridView tarafından otomatik olarak eklendiyse, bu olayı tetikler (sıralamayı etkinleştir seçildiğinde, her bir sütunun üst LinkButtons gibi veya LinkButtons sayfalama etkinleştir seçildiğinde disk belleği arabiriminde).

Bu nedenle, Ekle düğmesine tıklayarak kullanıcının yanıt vermesi için GridView s için bir olay işleyicisi oluşturmak ihtiyacımız `RowCommand` olay. Herhangi bir zamanda bu olay harekete beri *herhangi* düğme, LinkButton veya GridView içinde ImageButton tıklandığında, onu s biz yalnızca ekleme mantığıyla taktirde, önemli `CommandName` özelliği için olayişleyicieşlemelerigeçirilen`CommandName` Add (Ekle) düğmesini değeri. Geçerli veri doğrulama denetimleri rapor, ayrıca, biz de yalnızca devam etmelisiniz. Bunu yapabilmek için bir olay işleyicisi oluşturma `RowCommand` aşağıdaki kod ile olay:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Neden denetimi olay işleyicisi rahatsız merak `Page.IsValid` özelliği. Geçersiz veri ekleme arabiriminde sağlanırsa, geri gönderme kazanılan t atlanması? Kullanıcı JavaScript devre dışı veya istemci tarafı doğrulama mantığını aşmak için adımlar atmıştır sürece bu varsayımı doğrudur. Kısacası, bir hiçbir zaman kesin olarak istemci tarafı doğrulamasını yararlanmalıdır; Sunucu tarafı onay geçerliliğini verilerle çalışmaya başlamadan önce her zaman gerçekleştirilmelidir.


1. adımda oluşturduğumuz `ProductsDataSource` ObjectDataSource gibi kendi `Insert()` yöntemi eşlenmiş durumda `ProductsBLL` s sınıfı `AddProduct` yöntemi. Yeni kayıtta eklemek için `Products` tablo, biz yalnızca ObjectDataSource s çağırabilirsiniz `Insert()` yöntemi:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Şimdi `Insert()` yöntemi çağrılır, tüm kalan olan değerleri, parametreleri ekleme arabiriminden kopyalamak için geçirilen `ProductsBLL` s sınıfı `AddProduct` yöntemi. Geri gördüğümüz gibi [ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) öğretici, bu gerçekleştirilebilir ObjectDataSource s `Inserting` olay. İçinde `Inserting` denetimlerini programlı olarak başvurmak için ihtiyacımız olan olay `Products` GridView s alt satır ve değerlerine atama `e.InputParameters` koleksiyonu. Kullanıcı bırakarak gibi bir değer çıkarırsa `ReorderLevel` ihtiyacımız veritabanına eklenen değeri olması gerektiğini belirtmek için metin kutusu boş `NULL`. Bu yana `AddProducts` yöntemi kabul boş değer atanabilir veritabanı alanları için boş değer atanabilir türler, yalnızca null yapılabilir bir tür kullanın ve değerini ayarlamak `Nothing` kullanıcı girişini nerede atlanırsa durumda.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

İle `Inserting` tamamlandı olay işleyicisi, yeni kayıtlar eklenebilir `Products` GridView s altbilgi satır aracılığıyla veritabanı tablosu. Devam edin ve birkaç yeni ürünler eklemeyi deneyin.

## <a name="enhancing-and-customizing-the-add-operation"></a>Geliştirme ve özelleştirme işlemi ekleyin

Şu anda Ekle düğmesine tıklayın veritabanı tablosuna yeni bir kayıt ekleyen ancak görsel geri bildirim herhangi bir tür kaydı başarıyla eklendiğini sağlamaz. İdeal olarak, bir etiket Web denetimi veya istemci tarafı uyarı kutusu kendi ekleme başarılı bir şekilde tamamlandığını kullanıcıyı bilgilendirmek. Ben bunu bir alıştırma olarak için okuyucu bırakın.

Bu öğreticide kullanılan GridView herhangi bir sıralama düzeni listelenen ürünler için geçerli değildir ve verileri sıralamak son kullanıcı izin vermiyor. Sonuç olarak, kullanıcıların birincil anahtar alanı veritabanında olduğu gibi kayıtları sıralanır. Her yeni kayıt olduğundan bir `ProductID` son bir yeni bir ürün, sabitlenmiş Kılavuzu sonuna kadar her eklendiğinde, büyük değer. Bu nedenle, yeni bir kayıt ekledikten sonra kullanıcı GridView son sayfasına otomatik olarak göndermek isteyebilirsiniz. Bu kod aşağıdaki satırı ekleyerek çağrısından sonra gerçekleştirilebilir `ProductsDataSource.Insert()` içinde `RowCommand` olay işleyicisi, kullanıcı verileri GridView'a bağladıktan sonra sayfa son gönderilmesi gerektiğini belirtmek için:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` başlangıçta bir sayfa düzeyi Boolean değişkeni değeri atanır `False`. GridView s `DataBound` olay işleyicisi, `SendUserToLastPage` false ise `PageIndex` özelliğinin güncelleştirilmesinin son sayfayla kullanıcıya gönderilecek.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Nedeni `PageIndex` özelliği ayarlandığında `DataBound` olay işleyicisi (başlangıcı yerine sonundan `RowCommand` olay işleyicisi) çünkü zaman `RowCommand` olay işleyicisi size henüz yeni kayda ekleme ve harekete `Products` veritabanı tablosu. Bu nedenle `RowCommand` olay işleyicisinin son sayfa dizini (`PageCount - 1`) son sayfa dizini temsil *önce* yeni ürün eklendi. Eklenmekte olan ürünlerin çoğu için son sayfa dizini yeni ürünü ekledikten sonra aynıdır. Ancak, eklenen ürün sonuçları bir yeni son sayfa dizini, yanlış güncelleştirmemiz durumunda `PageIndex` içinde `RowCommand` olay işleyicisi sonra biz gidersiniz ikinci (yeni ürünü eklemeden önce son sayfa dizini) son sayfasına yeni son sayfa i aksine ndex. Bu yana `DataBound` olay işleyicisi, eklediğiniz yeni ürünü ve veri ayarlayarak kılavuza DataSet'e sonra ateşlenir `PageIndex` özelliği var. biliyoruz ki doğru son sayfa dizini alma.

Son olarak, bu öğreticide kullanılan GridView oldukça geniş yeni ürün eklemek için toplanması gereken alanların sayısı. Bu genişliği nedeniyle tercih edilen bir DetailsView s dikey düzeni olabilir. GridView s genel genişliği daha az girişleri toplamak tarafından indirgenebilir. Belki de biz t toplamak gerek ki `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları yeni bir ürün eklerken, bu durumda bu alanların GridView kaldırılamadı.

Toplanan verileri ayarlamak için şu iki yaklaşımdan birini kullanabilirsiniz:

- Kullanmaya devam `AddProduct` değerlerini bekliyor yöntemi `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları. İçinde `Inserting` olay işleyicisi, sabit kodlanmış sağlamak için varsayılan değerler ekleme arabiriminden kaldırılmış olan bu girdiler için kullanılacak.
- Yeni bir aşırı yüklemesini oluşturma `AddProduct` yönteminde `ProductsBLL` girdiler için kabul etmiyor sınıfı `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları. Ardından ASP.NET sayfasında bu yeni aşırı kullanılacak ObjectDataSource yapılandırın.

Her iki seçenek de eşit olarak çalışır. İçinde öğreticiler ikinci seçeneği için birden çok aşırı yükleme oluşturma kullandık `ProductsBLL` s sınıfı `UpdateProduct` yöntemi.

## <a name="summary"></a>Özet

GridView DetailsView ve FormView bulunan yerleşik ekleme özellikleri eksik, ancak biraz çaba ile alt bilgi satırına ekleme arabirim eklenebilir. Yalnızca içinde GridView altbilgi satırında görüntülenecek kümesi kendi `ShowFooter` özelliğini `True`. Alt bilgi satır içeriği için bir TemplateField alan dönüştürerek her alan için özelleştirilebilir ve ekleme ekleme arabirim için `FooterTemplate`. Bu öğreticide, gördüğümüz gibi `FooterTemplate` düğmeler, metin kutuları, DropDownList, onay kutuları, veri odaklı Web denetimleri (örneğin bir DropDownList) doldurmak için veri kaynağı denetimleri ve doğrulama denetimleri içerebilir. S kullanıcı girişinin toplanması için denetimlerin yanı sıra, bir düğme ekleyin, LinkButton veya ImageButton gereklidir.

Ne zaman Ekle düğmesine tıklandığında, ObjectDataSource s `Insert()` ekleme iş akışının başlatılacağı yöntemi çağrılır. ObjectDataSource ardından yapılandırılmış Ekle yöntemi çağırın ( `ProductsBLL` s sınıfı `AddProduct` bu öğreticideki yöntemi). Biz ObjectDataSource s arabirimi ekleme GridView s değerleri kopyalamalısınız `InsertParameters` çağrılmakta olan yöntemin INSERT önce koleksiyonu. Bu program aracılığıyla ObjectDataSource s ekleme arabirimi Web denetimlerinde başvurarak gerçekleştirilebilir `Inserting` olay işleyicisi.

Bu öğreticide, bizim göz GridView görünümünü geliştirme teknikleri tamamlar. Sonraki öğreticiler kümesini, PDF, Word belgeleri, görüntüler gibi ikili verileri ile çalışma ve benzeri işlemleri ve veri Web denetimleri inceleyeceksiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Bernadette Leigh oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](adding-a-gridview-column-of-checkboxes-vb.md)
