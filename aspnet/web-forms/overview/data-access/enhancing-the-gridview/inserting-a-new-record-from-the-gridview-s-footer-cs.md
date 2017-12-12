---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: "GridView'ın altbilginin (C#) yeni bir kayıt ekleme | Microsoft Docs"
author: rick-anderson
description: "GridView denetiminin verilerin yeni bir kayıt eklemek için yerleşik destek sağlamaz, ancak bu öğreticide içerecek şekilde GridView büyütmek gösterilmektedir bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b0208b4df0194abaf37f7f9ac66c9ce24c35d721
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>GridView'ın altbilginin (C#) yeni bir kayıt ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) veya [PDF indirin](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> GridView denetiminin verilerin yeni bir kayıt eklemek için yerleşik destek sağlamaz, ancak bu öğreticide ekleme arabirim içerecek şekilde GridView büyütmek gösterilmektedir.


## <a name="introduction"></a>Giriş

' Da anlatıldığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) Öğreticisi, GridView, DetailsView ve FormView Web denetimleri her yerleşik veri değişikliği özellikleri içerir. Bildirim temelli veri kaynağı denetimleri ile kullanıldığında, bu üç Web denetimleri hızla ve kolayca verileri - değiştirmek için yapılandırılabilir ve tek satırlık bir kod yazmaya gerek kalmadan senaryolarda. Ne yazık ki, yalnızca DetailsView ve FormView denetimleri yerleşik ekleme, düzenleme ve silme yetenekleri sağlar. GridView, düzenleme ve silme desteği yalnızca sunar. Ancak, biraz dirsek yağ ile biz GridView ekleme arabirim içerecek şekilde genişletebilirsiniz.

GridView ekleme özellikleri eklemek, biz nasıl yeni kayıtlarda eklenecek karar verme, ekleme arabirimi oluşturma ve yeni kayıt eklemek için kod yazma sorumludur. Bu öğreticide, biz GridView s altbilgi ekleme arabirim ekleme adresindeki görüneceğini (bkz. Şekil 1 ') satır. Altbilgi hücrenin her sütun için uygun veri toplama kullanıcı arabirimi öğesi (ürün s adı metin kutusuna, DropDownList tedarikçi, vb.) içerir. Ayrıca bir sütun ihtiyacımız bir Ekle düğmesi, tıklatıldığında, geri gönderimin neden ve yeni bir kayda eklemek `Products` altbilgi satırında sağlanan değerleri kullanarak tablo.


[![Altbilgi satırında yeni ürünler eklemek için bir arabirim sağlar.](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Şekil 1**: altbilgi satır ekleme yeni ürünler için bir arabirim sağlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>1. adım: Bir GridView ürün bilgilerini görüntüleme

Biz kendisini GridView s altbilgi ekleme arabirimi oluşturmayla ilgili önce veritabanında ürünleri listeler sayfa GridView ekleme s ilk odaklanmasına olanak tanır. Başlangıç açarak `InsertThroughFooter.aspx` sayfasındaki `EnhancedGridView` klasörü ve sürükleme GridView GridView s ayarı tasarımcıya araç `ID` özelliğine `Products`. Ardından, adlı yeni bir ObjectDataSource bağlamak için GridView s akıllı etiket kullanmak `ProductsDataSource`.


[![ProductsDataSource adlı yeni bir ObjectDataSource oluşturma](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış oluşturma `ProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


ObjectDataSource kullanmak için yapılandırma `ProductsBLL` s sınıfı `GetProducts()` ürün bilgisi almak için yöntem. Bu öğretici için düzenleme ve silme hakkında endişelenmeniz değil ve ekleme yetenekleri ekleme s odaklanmasına kesinlikle sağlar. Bu nedenle, Ekle sekmesinde aşağı açılan listesinde ayarlamak emin olun `AddProduct()` ve UPDATE ve DELETE sekmelerdeki açılan listeleri (hiçbiri) ayarlanır.


[![ObjectDataSource s INSERT() yönteme map AddProduct yöntemi](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Şekil 3**: eşleme `AddProduct` yöntemi ObjectDataSource s `Insert()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Güncelleştirme ve silme sekmeleri açılan listeleri (hiçbiri) ayarlayın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Şekil 4**: güncelleştirme ve silme sekmelerini aşağı açılan listeler (hiçbiri) olarak ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak alanları GridView karşılık gelen veri alanların her biri için ekleyeceksiniz. Şimdilik, tüm Visual Studio tarafından eklenen alanları bırakın. Biz geri dönün ve kaldırmak daha sonra Bu öğreticide bazı t değerleri güncelleştireceğinizi alanları yeni bir kayıt eklerken belirtilmesi gerekir.

Veritabanında yakın 80 ürünleri olduğundan, bir kullanıcının yeni bir kayıt eklemek için bu süreç boyunca tüm web sayfasının en alta kadar kaydırın kaydırmak gereklidir. Bu nedenle, ekleme arabirimi daha görünür ve erişilebilir kılmak ICollection s olanak tanır. Disk belleği üzerinde etkinleştirmek için yalnızca disk belleği etkinleştirme GridView s akıllı etiket gelen onay.

Bu noktada, GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Tüm ürün veri alanları disk belleğine alınan GridView görüntülenir](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Şekil 5**: tüm ürün veri alanları disk belleğine alınan GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>2. adım: altbilgi satır ekleme

Üstbilgi ve veri satırı birlikte GridView altbilgi satırı içerir. Üstbilgi ve altbilgi satırları GridView s değerleri bağlı olarak görüntülenen [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) ve [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) özellikleri. Altbilgi satırı göstermek için basitçe ayarlayın `ShowFooter` özelliğine `true`. Şekil 6 gösterildiği gibi ayarı `ShowFooter` özelliğine `true` kılavuza altbilgi satır ekler.


[![Altbilgi satırı görüntülemek için ShowFooter True olarak ayarlayın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Şekil 6**: görüntülenecek altbilgi satır kümesi `ShowFooter` için `True` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Altbilgi satırı koyu kırmızı arka plan rengi olduğuna dikkat edin. Bu DataWebControls oluşturulur ve tüm sayfalara uygulanan temayı kaynaklandığını geri [veri ObjectDataSource ile görüntüleme](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Öğreticisi. Özellikle, `GridView.skin` dosyası yapılandırır `FooterStyle` kullanır, özellik böyle `FooterStyle` CSS sınıfı. `FooterStyle` Sınıfı tanımlanmış `Styles.css` gibi:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Biz GridView s altbilgi satırı önceki eğitimlerine kullanarak keşfedilen Taşı. Gerekirse, yeniden bakın [özet bilgilerinde görüntüleme GridView'ın altbilgi](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) Yenileyici Öğreticisi.


Ayar sonra `ShowFooter` özelliğine `true`, çıkışı bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Şu anda altbilgi satır mevcut değil t herhangi bir metin veya Web denetimleri içerir. Adım 3'te uygun ekleme arabirimi içeren biz her GridView alan altbilgisi değiştireceksiniz.


[![Görüntülenen yukarıda disk belleği arabirimi denetimlerini boş altbilgi satırıdır](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Şekil 7**: Boş altbilgi satırıdır görüntülenen yukarıda disk belleği arabirimi denetimlerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>3. adım: altbilgi satırı özelleştirme

Geri [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) büyük ölçüde TemplateFields (aksine, BoundFields veya CheckBoxFields) kullanarak belirli bir GridView Sütun görüntüsünü özelleştirmek nasıl gördüğümüz öğretici; [ Veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) GridView içinde düzenleme arabirimini özelleştirmek için TemplateFields kullanarak Aranan. Geri çağırma bir TemplateField biçimlendirme, Web denetimleri ve Veri bağlamada sözdizimi karışımını tanımlayan bir dizi şablonları oluşur belirli satır türleri kullanılır. `ItemTemplate`, Örneğin, salt okunur satırlar için kullanılan şablon belirtir sırada `EditItemTemplate` düzenlenebilir satır için şablonu tanımlar.

İle birlikte `ItemTemplate` ve `EditItemTemplate`, TemplateField de içeren bir `FooterTemplate` altbilgi satırı içeriğini belirtir. Bu nedenle, her alan s arabirimine eklemek için gereken Web denetimleri ekleyebiliriz `FooterTemplate`. Başlatmak için tüm GridView alanları için TemplateFields dönüştürün. Bu her alan sol alt köşede seçerek ve bu alan dönüştürme TemplateField bağlantıya tıklayarak GridView s akıllı etiket, Edit Columns bağlantıyı tıklatarak yapılabilir.


![Her bir alan TemplateField'a Dönüştür](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Şekil 8**: her alanı TemplateField'a Dönüştür


Dönüştür'ü tıklatarak bu alana bir TemplateField eşdeğer TemplateField içine geçerli alan türü etkinleştirir. Örneğin, her BoundField TemplateField ile değiştirilmiştir bir `ItemTemplate` ilgili veri alanı görüntüleyen bir etiket içerir ve bir `EditItemTemplate` , verileri alan bir metin kutusunda görüntüler. `ProductName` BoundField aşağıdaki TemplateField biçimlendirme dönüştürülmüş:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Benzer şekilde, `Discontinued` CheckBoxField dönüştürülmüş TemplateField'a özelliği `ItemTemplate` ve `EditItemTemplate` bir onay kutusu Web denetimi içeren (ile `ItemTemplate` s devre dışı onay kutusu). Salt okunur `ProductID` BoundField, hem de etiket denetimi ile TemplateField içine dönüştürülmüş `ItemTemplate` ve `EditItemTemplate`. Kısacası, varolan bir GridView dönüştürme bir TemplateField içine varolan s alanı işlevlerden herhangi birini kaybetmeden daha özelleştirilebilir TemplateField geçmek için hızlı ve kolay bir yol alanıdır.

Bu yana GridView biz destek t içermiyor düzenleme ile çalışma re eşitleyerek kaldırmak ücretsiz `EditItemTemplate` her TemplateField yalnızca bırakarak `ItemTemplate`. Bunu yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Her bir GridView alan TemplateField'a dönüştürülür, biz uygun ekleme arabirimi her alan s girebilirsiniz `FooterTemplate`. Bazı alanlar ekleme arabirime sahip olmaz (`ProductID`, örneğin); başkalarının yeni ürün s bilgilerini toplamak için kullanılan Web denetimlerinde farklılık gösterir.

Düzenleme arabirimi oluşturmak üzere GridView s akıllı etiketten Şablonları Düzenle bağlantıyı seçin. Ardından, açılan listeden uygun alanını s seçin `FooterTemplate` ve uygun denetimi tasarımcıya araç çubuğuna sürükleyin.


[![Her alan s FooterTemplate uygun ekleme arabirimi ekleyin](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Şekil 9**: her alanı s uygun ekleme arabirimi eklemek `FooterTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


Aşağıdaki madde işaretli listede eklemek için ekleme arabirimi belirtme GridView alanları numaralandırır:

- `ProductID`yok.
- `ProductName`metin kutusu ekleyin ve ayarlayın, `ID` için `NewProductName`. Kullanıcı yeni ürün s adı için bir değer girdiğinden emin olmak için de bir RequiredFieldValidator ekleyin.
- `SupplierID`yok.
- `CategoryID`yok.
- `QuantityPerUnit`ayarı, bir metin kutusu ekleyin, `ID` için `NewQuantityPerUnit`.
- `UnitPrice`adlı bir TextBox ekleyin `NewUnitPrice` ve girilen değer sağlar CompareValidator değerinden büyük veya sıfıra eşit bir para birimi değeri.
- `UnitsInStock`bir metin kutusu kullanın, `ID` ayarlanır `NewUnitsInStock`. Girilen değer bir tamsayı değeri sıfırdan büyük veya sıfıra eşit olmasını sağlar CompareValidator içerir.
- `UnitsOnOrder`bir metin kutusu kullanın, `ID` ayarlanır `NewUnitsOnOrder`. Girilen değer bir tamsayı değeri sıfırdan büyük veya sıfıra eşit olmasını sağlar CompareValidator içerir.
- `ReorderLevel`bir metin kutusu kullanın, `ID` ayarlanır `NewReorderLevel`. Girilen değer bir tamsayı değeri sıfırdan büyük veya sıfıra eşit olmasını sağlar CompareValidator içerir.
- `Discontinued`ayarı, bir onay kutusu ekleyin, `ID` için `NewDiscontinued`.
- `CategoryName`bir DropDownList ekleyin ve ayarlayın, `ID` için `NewCategoryID`. Adlı yeni bir ObjectDataSource bağlamak `CategoriesDataSource` ve kullanacak şekilde yapılandırın `CategoriesBLL` s sınıfı `GetCategories()` yöntemi. DropDownList s sahip `ListItem` s görüntü `CategoryName` veri alanı, kullanarak `CategoryID` veri alan değerlerine olarak.
- `SupplierName`bir DropDownList ekleyin ve ayarlayın, `ID` için `NewSupplierID`. Adlı yeni bir ObjectDataSource bağlamak `SuppliersDataSource` ve kullanacak şekilde yapılandırın `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi. DropDownList s sahip `ListItem` s görüntü `CompanyName` veri alanı, kullanarak `SupplierID` veri alan değerlerine olarak.

Her doğrulama denetimleri temizleyin `ForeColor` özelliği böylece `FooterStyle` CSS sınıfı s Beyaz ön plan rengini kırmızı varsayılan yerine kullanılır. Aynı zamanda `ErrorMessage` ancak özelliği ayrıntılı bir açıklaması için belirlenen `Text` bir yıldız işareti özelliğine. İki satır sarmalamak ekleme arabirimi neden doğrulama denetimi s metin engelleyecek şekilde ayarlanmışsa `FooterStyle` s `Wrap` özelliğini her biri için false `FooterTemplate` doğrulama denetimi kullanma s. Son olarak, GridView ve kümesi altında ValidationSummary denetim ekleme kendi `ShowMessageBox` özelliğine `true` ve kendi `ShowSummary` özelliğine `false`.

Yeni bir ürün eklerken sağlamak ihtiyacımız `CategoryID` ve `SupplierID`. Bu bilgiler için altbilgi hücrelerindeki DropDownLists aracılığıyla yakalanan `CategoryName` ve `SupplierName` alanları. Bu alanları tersine kullanmak seçtiniz `CategoryID` ve `SupplierID` TemplateFields s kılavuzunda kullanıcı veri satırlarına olduğundan kimliği değerlerine yerine kategori ve tedarikçi adlarını görmeniz büyük olasılıkla daha ilgi. Bu yana `CategoryID` ve `SupplierID` değerleri şimdi yakalanır içinde `CategoryName` ve `SupplierName` alan s ekleme arabirimleri, biz kaldırabilirsiniz `CategoryID` ve `SupplierID` GridView gelen TemplateFields.

Benzer şekilde, `ProductID` yeni bir ürün eklerken kullanılmaz böylece `ProductID` TemplateField de kaldırılabilir. Bununla birlikte, bırakın s izin `ProductID` kılavuzunda alan. Metin kutuları, DropDownLists, onay kutularını ve ekleme arabirimini oluşturan doğrulama denetimleri ek olarak, biz ekleme ayrıca gerekir, düğme tıklatıldığında, yeni ürün veritabanına eklemek için mantığı gerçekleştirir. Adım 4'te Ekle düğmesi ekleme arabiriminde eklediğimiz `ProductID` TemplateField s `FooterTemplate`.

Çeşitli GridView alanları görünümünü iyileştirmek çekinmeyin. Örneğin, biçimlendirmek isteyebilirsiniz `UnitPrice` değerleri bir para birimi olarak Sağa Hizala `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` alanları ve güncelleştirme `HeaderText` TemplateFields değerleri.

Arabirimlerde ekleme slew oluşturduktan sonra `FooterTemplate` s, kaldırma `SupplierID`, ve `CategoryID` TemplateFields ve biçimlendirme ve bildirim temelli, GridView s TemplateFields hizalama aracılığıyla kılavuzunun estetik artırma Biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

GridView s altbilgi satırı artık bir tarayıcıdan görüntülendiğinde, tamamlanmış içerir. ekleme arabirim (bkz. Şekil 10). Bu noktada, ekleme arabirimi içermiyor t kullanıcının bu SEH s yeni ürün için girilen verilerini ve veritabanına yeni bir kayıt eklemek isteyen belirtmek bir yol ekleyin. Ayrıca, biz ve henüz altbilgi girilen verilerin yeni bir kayıt nasıl şekilde çevirir adres `Products` veritabanı. Adım 4 biz bakabilir ekleme arabirimi için Ekle düğmesini dahil etme ve kod yürütmek geri gönderme zaman onu tıklattınız s. Adım 5 altbilgi verileri kullanarak yeni bir kayıt eklemek nasıl gösterir.


[![GridView altbilgi yeni bir kayıt eklemek için bir arabirim sağlar.](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Şekil 10**: yeni bir kayıt eklemek için bir arabirim GridView altbilgi sağlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>4. adım: bir Ekle düğmesi ekleme arabirimde de dahil olmak üzere

Biz anlamına gelir, yeni ürün s bilgilerini girme tamamladınız belirtmek kullanıcı arabirimi şu anda ekleme altbilgi satır s eksik olduğundan Ekle düğmesini yere ekleme arabiriminde eklemeniz gerekir. Bu var olan birinde yerleştirilemedi `FooterTemplate` s veya biz ekleyebileceğiniz yeni bir sütun kılavuza bu amaç için. Bu öğretici için let s yerleştirin Ekle düğmesini `ProductID` TemplateField s `FooterTemplate`.

Tasarımcısından GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve ardından `ProductID` alan s `FooterTemplate` aşağı açılan listeden. Bir düğme Web denetimi (veya bir LinkButton veya tercih ederseniz ImageButton) Kimliğini ayarını şablona eklemek `AddProduct`, kendi `CommandName` , eklenecek ve kendi `Text` Şekil 11'de gösterildiği gibi eklenecek özellik.


[![Yerleştir ProductID TemplateField s FooterTemplate Ekle düğmesi](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Şekil 11**: Düğme Ekle yerleştirin `ProductID` TemplateField s `FooterTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Ekle düğmesi önceden dahil sonra sayfasını bir tarayıcıda çıkışı test edin. Ekleme arabiriminde geçersiz verilerle Ekle düğmesine tıklandığında, geri gönderme kısa circuited unutmayın ve ValidationSummary denetimi (bkz. Şekil 12) geçersiz veri gösterir. Girilen uygun verilerle geri gönderimin için Ekle düğmesini neden olur. Hiçbir kayıt ancak veritabanına eklenir. Biraz gerçekte Ekle gerçekleştirmek için kod yazma gerekecektir.


[![Ekleme arabiriminde geçersiz veri ise Ekle düğmesi s geri gönderme kısa Circuited.](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Şekil 12**: Ekle düğmesi geri gönderme s'dir kısa Circuited ekleme arabiriminde geçersiz veri ise ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> Doğrulama denetimleri ekleme arabiriminde bir doğrulama grubuna atanmış olmadığından. Doğrulama denetimleri sayfasında yalnızca kümesi ekleme arabirimi olduğu sürece düzgün çalışır. Ancak, varsa, diğer doğrulama denetimleri (örneğin, doğrulama denetimleri kılavuz s düzenleme arabiriminde) sayfasında, ekleme, doğrulama denetimleri arabirim ve düğmesi s eklemek `ValidationGroup` özellikleri aynı değeri atanmış so olarak için Bu denetimler belirli doğrulama grubuyla ilişkilendirin. Bkz: [ASP.NET 2. 0 doğrulama denetimleri Dissecting](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) doğrulama denetimleri ve düğmeleri sayfasında doğrulama gruplar halinde bölümleme hakkında daha fazla bilgi.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>5. adım: yeni bir kayıt ekleme`Products`tablosu

GridView yerleşik düzenleme özelliklerini kullanırken GridView otomatik olarak tüm güncelleştirme gerçekleştirmek için gerekli iş işler. Özellikle, bu düzenleme arabiriminden ObjectDataSource s parametre için girilen değerleri kopyalar güncelleştirme düğme tıklatıldığında `UpdateParameters` toplama ve ObjectDataSource s çağırma tarafından güncelleştirme kapalı kicks `Update()` yöntemi. GridView ekleme gibi yerleşik bir işlevi sağlamadığından ObjectDataSource s çağıran kodu uygulamanız gerekir `Insert()` yöntemi ve ekleme gelen değerleri arabirim ObjectDataSource s kopya `InsertParameters` koleksiyonu .

Ekle düğmesi tıklatıldıktan sonra bu Ekle mantığı yürütülmelidir. ' Da anlatıldığı gibi [ekleme ve GridView düğmeleri yanıtlama](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) düğme, LinkButton veya GridView içinde ImageButton tıklandığında verdiğinizde, GridView s öğretici, `RowCommand` geri gönderme üzerinde olay etkinleşir. Düğme, LinkButton veya ImageButton açıkça altbilgi satırında Ekle düğmesi gibi eklendi olup olmadığını veya onu GridView tarafından otomatik olarak eklendiyse, bu olay harekete (etkinleştirmek sıralama seçildiğinde, her bir sütunun üst LinkButtons gibi veya LinkButtons etkinleştirmek sayfalama seçildiğinde disk belleği arabiriminde).

Bu nedenle, için Ekle düğmesini kullanıcıya yanıt vermek için bir olay işleyicisi GridView s için oluşturmak ihtiyacımız `RowCommand` olay. Bu olay her tetikler. bu yana *herhangi* düğme, LinkButton veya GridView ImageButton tıklandığında, onu s biz yalnızca ekleme mantığı ile devam edin, önemli `CommandName` özelliğigeçirilenolayişleyicieşlemeleri`CommandName` Ekle düğmesi (Ekle) değeri. Geçerli veri doğrulama denetimleri rapor ayrıca, biz de yalnızca devam edin. Bunu yapabilmek için bir olay işleyicisi oluşturun `RowCommand` aşağıdaki kod ile olay:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Olay işleyicisi denetimi neden rahatsız merak ediyor olabilirsiniz `Page.IsValid` özelliği. Sonuçta, geçersiz veri ekleme arabiriminde sağladıysanız kazanılan t geri gönderme atlanması? Kullanıcı JavaScript devre dışı veya istemci tarafı doğrulama mantığını aşmak için adımlar atmıştır sürece bu doğru bir varsayılır. Kısacası, bir hiçbir zaman kesinlikle istemci tarafı doğrulamasını yararlanmalıdır; verilerle çalışmaya başlamadan önce her zaman bu geçerlilik için bir sunucu tarafı denetimi gerçekleştirilmesi gerekir.


1. adımda oluşturduğumuz `ProductsDataSource` ObjectDataSource şekilde kendi `Insert()` yöntemi eşleştirilir `ProductsBLL` s sınıfı `AddProduct` yöntemi. Yeni bir kayda eklemek için `Products` tablo, biz yalnızca ObjectDataSource s çağırabileceği `Insert()` yöntemi:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Şimdi `Insert()` yöntemi çağrılır, kalan tek şey ekleme arabiriminden parametreleri için değerleri kopyalamak için tüm geçirilecek `ProductsBLL` s sınıfı `AddProduct` yöntemi. Geri gördüğümüz gibi [ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) öğretici, bu gerçekleştirilebilir ObjectDataSource s `Inserting` olay. İçinde `Inserting` ihtiyacımız denetimlerini programlı olarak başvurmak için olay `Products` GridView s altbilgi satırın ve değerlerine atama `e.InputParameters` koleksiyonu. Bırakarak gibi bir değer kullanıcı çıkarırsa `ReorderLevel` ihtiyacımız veritabanına eklenen değer olması gerektiğini belirtmek için metin kutusu boş `NULL`. Bu yana `AddProducts` yöntemi boş değer atanabilir veritabanı alanları için boş değer atanabilir türler kabul eder, yalnızca null atanabilir bir tür kullanın ve değerini ayarlama `null` kullanıcı girişi atlanmış olduğu durumda.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

İle `Inserting` tamamlandı, olay işleyicisi yeni kayıtlar eklenebilir `Products` GridView s altbilgi satırı aracılığıyla veritabanı tablosu. Bir tane birkaç yeni ürünler eklemeyi deneyin.

## <a name="enhancing-and-customizing-the-add-operation"></a>Geliştirme ve özelleştirme ekleme işlemi

Şu anda Ekle düğmesini tıklatarak yeni bir kaydı veritabanı tablosuna ekler ancak görsel geribildirim herhangi bir tür kaydı başarıyla eklendi sağlamaz. İdeal olarak, bir etiket Web denetimi veya istemci tarafı uyarısı kutusu kendi ekleme başarıyla tamamlandı kullanıcıyı bilgilendirmek. I bunu bir alıştırma olarak okuyucuya bırakın.

Bu öğreticide kullanılan GridView herhangi sıralama düzeni listelenen ürünler için geçerli değildir ve verileri sıralamak son kullanıcı izin vermez. Sonuç olarak, kendi birincil anahtar alanı veritabanında olduğu gibi kayıtları sıralanır. Her yeni kayıt olduğundan bir `ProductID` son daha yeni bir ürün, sabitlenmiş kılavuz sonuna eklenen her zaman büyük değer. Bu nedenle, otomatik olarak yeni bir kayıt eklendikten sonra kullanıcı GridView son sayfasına göndermek isteyebilirsiniz. Bu çağrısından sonra aşağıdaki kod satırını ekleyerek gerçekleştirilebilir `ProductsDataSource.Insert()` içinde `RowCommand` kullanıcı verileri için GridView bağlandıktan sonra sayfa son gönderilmesi gerektiğini belirtmek için olay işleyicisi:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage`başlangıçta bir sayfa düzeyinde Boolean değişken değerini atanan `false`. GridView s `DataBound` olay işleyicisi varsa `SendUserToLastPage` false, `PageIndex` özelliği, kullanıcı son sayfasına göndermek üzere güncelleştirilir.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

Nedeni `PageIndex` özelliği ayarlanmış `DataBound` olay işleyicisi (tersine `RowCommand` olay işleyicisi) çünkü zaman `RowCommand` olay işleyicisi harekete Biz yeni kayda henüz eklemek ve `Products` veritabanı tablosu. Bu nedenle, içinde `RowCommand` olay işleyicisi son sayfa dizini (`PageCount - 1`) son sayfa dizini temsil eden *önce* yeni ürün eklendi. Eklenmekte olan ürünleri çoğunluğu için son sayfa dizini yeni ürün ekledikten sonra aynıdır. Ancak zaman eklenen ürün sonuçları yeni bir son sayfa dizini içinde biz yanlış güncelleştirirseniz `PageIndex` içinde `RowCommand` olay işleyicisi sonra biz gidersiniz ikinci (yeni ürün eklemeden önce son sayfa dizini) son sayfasına yeni son sayfa i aksine ndex. Bu yana `DataBound` olay işleyicisi harekete yeni ürün eklenir ve verileri ayarlayarak kılavuza DataSet'e sonra `PageIndex` özelliği var. biliyoruz biz doğru son sayfa dizini alma.

Son olarak, bu öğreticide kullanılan GridView yeni bir ürün eklemek için toplanması gereken alanlar sayısı nedeniyle oldukça geniş. Bu genişliği nedeniyle DetailsView s dikey düzen tercih edilen olabilir. GridView s genel genişliği daha az sayıda girdi toplayarak indirgenebilir. Belki de biz t toplamak gerek güncelleştireceğinizi `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları yeni bir ürün eklerken; bu durumda bu alanlar GridView kaldırılamaz.

Toplanan veriler ayarlamak için şu iki yaklaşımdan birini kullanabilirsiniz:

- Kullanmaya devam `AddProduct` için değeri bekler yöntemi `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları. İçinde `Inserting` olay işleyicisi varsayılan ekleme arabiriminden kaldırılmış olan bu girişleri için değerleri, sabit kodlanmış belirtin.
- Yeni bir aşırı yüklemesini oluşturma `AddProduct` yönteminde `ProductsBLL` girdileri kabul etmiyorum sınıfı `UnitsOnOrder`, `UnitsInStock`, ve `ReorderLevel` alanları. Sonra ASP.NET sayfasında ObjectDataSource bu yeni aşırı kullanacak şekilde yapılandırın.

Her iki seçenek de eşit olarak çalışır. İçinde öğreticileri ikinci seçeneği için birden çok aşırı oluşturma kullandık `ProductsBLL` s sınıfı `UpdateProduct` yöntemi.

## <a name="summary"></a>Özet

GridView DetailsView ve FormView bulunan yerleşik ekleme özellikleri eksik, ancak biraz çaba ile alt bilgi satırına ekleme arabirim eklenebilir. Yalnızca bir GridView altbilgi satırında görüntülenecek ayarlamak kendi `ShowFooter` özelliğine `true`. Altbilgi satır içerik alanı bir TemplateField dönüştürerek her alan için özelleştirilebilir ve ekleme ekleme arabirim için `FooterTemplate`. Bu öğreticide, gördüğümüz gibi `FooterTemplate` düğmeleri, metin kutuları, DropDownLists, onay kutularını, veri tabanlı Web denetimleri (örneğin, DropDownLists) doldurmak için veri kaynağı denetimleri ve doğrulama denetimleri içerebilir. Kullanıcı s girişi toplama denetimlerini yanı sıra, bir Ekle düğmesi, LinkButton veya ImageButton gereklidir.

Ne zaman Ekle düğmesine tıklandığında, ObjectDataSource s `Insert()` yöntemi ekleme iş akışını başlatmak için çağrılır. ObjectDataSource ardından yapılandırılmış Insert yöntemini çağıran ( `ProductsBLL` s sınıfı `AddProduct` bu öğreticideki yöntemi). Biz ObjectDataSource s arabirimine ekleme GridView s değerleri kopyalamalısınız `InsertParameters` çağrılan Insert yöntemini önce koleksiyonu. Bu program aracılığıyla ObjectDataSource s ekleme arabirimi Web denetimlerinde başvurarak gerçekleştirilebilir `Inserting` olay işleyicisi.

Bu öğretici GridView s görünümünü geliştirme tekniklerini bizim bakma tamamlar. Öğreticiler bir sonraki kümesini görüntüler, PDF, Word belgeleri gibi ikili verilerle çalışmak ve benzeri verilerini ve Web denetimleri inceleyeceksiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Bernadette Leigh oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](adding-a-gridview-column-of-checkboxes-cs.md)
[sonraki](adding-a-gridview-column-of-radio-buttons-vb.md)
