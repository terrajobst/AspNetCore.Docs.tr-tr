---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Ekleme ve GridView (VB) düğmelere yanıtlama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide bir şablona hem GridView veya DetailsView denetiminin alanlara özel düğmeler ekleme inceleyeceğiz. Özellikle, bui gerekir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 58b570c897810eeaa182a201616a182c02e9d92c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Ekleme ve düğmelere GridView (VB) yanıt verme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) veya [PDF indirin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> Bu öğreticide bir şablona hem GridView veya DetailsView denetiminin alanlara özel düğmeler ekleme inceleyeceğiz. Özellikle, biz tedarikçileri sayfasında kullanıcıya izin veren bir FormView içeren bir arabirim yapı.


## <a name="introduction"></a>Giriş

Birçok raporlama senaryolarına rapor verilerine salt okunur erişimi içeren sırada bu eylemleri gerçekleştirme yeteneğini dahil etmek için raporları değil seyrek s tabanlı görüntülenen verileri. Genellikle Bu raporda görüntülenen her bir kayıtla düğme, LinkButton veya ImageButton Web denetim ekleme söz konusu, tıklatıldığında geri gönderimin neden olur ve bazı sunucu tarafı kodu çağırır. Düzenleme ve kayıt kayıt temelinde verileri silme en yaygın örnektir. Aslında, başlayarak gördüğümüz gibi [genel bakış, ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) GridView, DetailsView ve FormView denetimleri bu tür işlevselliği olmadan destekleyebilir kadar sık öğretici, düzenleme ve silme tek satırlık bir kod yazmak için gerekir.

Ayrıca düzenleme ve silme düğmeleri, GridView, DetailsView ve FormView için denetimler de düğmeler, LinkButtons veya ImageButtons içerir, tıklatıldığında, bazı özel sunucu tarafı mantığını gerçekleştirirler. Bu öğreticide bir şablona hem GridView veya DetailsView denetiminin alanlara özel düğmeler ekleme inceleyeceğiz. Özellikle, biz tedarikçileri sayfasında kullanıcıya izin veren bir FormView içeren bir arabirim yapı. Belirli bir sağlayıcı için FormView tıkladıysanız, tüm ilişkili ürünlerini dönüştürülmesine olarak işaretler, bir düğme Web denetimi birlikte tedarikçi hakkında bilgi gösterir. Ayrıca, GridView fiyat artırmak ve indirim fiyat, tıkladıysanız, yükseltmek veya ürün s azaltmak düğmeler içeren her satırın seçili sağlayıcı tarafından sağlanan bu ürünleri listeler `UnitPrice` 10 oranında (bkz. Şekil 1).


[![FormView ve GridView özel eylemleri gerçekleştiren düğmeleri içerir](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Şekil 1**: her iki FormView ve GridView içeren düğmeleri, özel eylemleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))


## <a name="step-1-adding-the-button-tutorial-web-pages"></a>1. adım: düğme öğretici Web sayfaları ekleme

Özel düğmeler eklemek nasıl tümleştirildiği incelenmektedir önce öncelikle Bu öğretici için yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `CustomButtons`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki iki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `CustomButtons.aspx`


![Özel düğmeler ilgili öğreticileri için ASP.NET sayfaları ekleme](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Şekil 2**: özel düğmeler ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `CustomButtons` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Şekil 3**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))


Son olarak, girişleri olarak sayfaları eklemek `Web.sitemap` dosya. Özellikle, sıralama ve disk belleği sonra aşağıdaki biçimlendirme eklemeniz `<siteMapNode>`:


[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık düzenleme, ekleme ve öğreticiler silmek için öğeler içerir.


![Site Haritasını giriş özel düğmeler öğretici için artık içerir.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Şekil 4**: Site Haritası giriş özel düğmeler öğretici için artık içerir.


## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>2. adım: tedarikçileri listeler FormView ekleme

Bu öğretici ile tedarikçileri listeler FormView ekleyerek başlamanıza s olanak tanır. Girişte açıklandığı gibi bu FormView GridView tedarikçi tarafından sağlanan ürünleri gösteren tedarikçileri sayfadan kullanıcıya izin verir. Ayrıca, bu FormView bir düğmesi bulunur, tıklatıldığında, tüm tedarikçi s ürünleri dönüştürülmesine olarak işaretler. Biz kendisini FormView özel düğme ekleme ile ilgili önce üretici bilgilerini görüntüler ilk yalnızca FormView oluşturun, böylece s olanak tanır.

Başlangıç açarak `CustomButtons.aspx` sayfasındaki `CustomButtons` klasör. Araç kümesi ve Tasarımcısı üzerine sürükleyerek bir FormView sayfasına ekleme kendi `ID` özelliğine `Suppliers`. FormView s akıllı etiketten adlı yeni bir ObjectDataSource oluşturmak için kabul `SuppliersDataSource`.


[![SuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Şekil 5**: yeni ObjectDataSource adlandırılmış oluşturma `SuppliersDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))


Bu yeni ObjectDataSource gelen sorgular gibi yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi (bkz. Şekil 6). Bu FormView (hiçbiri) güncelleştirme sekmesinde aşağı açılan listeden seçeneğini üretici bilgilerini seçin güncelleştirmek için bir arabirim sağlamadığından.


[![S GetSuppliers() yöntemi SuppliersBLL sınıfını kullanmak için veri kaynağını yapılandırma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Şekil 6**: kullanmak için veri kaynağını yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio oluşturacak bir `InsertItemTemplate`, `EditItemTemplate`, ve `ItemTemplate` FormView için. Kaldırma `InsertItemTemplate` ve `EditItemTemplate` ve değiştirme `ItemTemplate` böylece yalnızca tedarikçi s şirket adını ve telefon numarasını görüntüler. Son olarak, disk belleği FormView desteği akıllı etiket gelen sayfalama etkinleştir onay kutusu işaretlendiğinde Aç (veya ayarlayarak kendi `AllowPaging` özelliğine `True`). Bu değişikliklerden sonra sayfa s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

Şekil 7 bir tarayıcıdan görüntülendiğinde CustomButtons.aspx sayfası gösterilir.


[![FormView ŞirketAdı ve şu anda seçili Tedarikçi telefon alanlardan listeler](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Şekil 7**: FormView listeler `CompanyName` ve `Phone` şu anda seçili tedarikçi alanlardan ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))


## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>3. adım: Seçili tedarikçi s ürünleri listeler GridView ekleme

Tüm ürünleri Durdur düğmesini FormView s şablona ekleyebilmeniz önce GridView seçili sağlayıcı tarafından sağlanan ürünleri listeler FormView altına ekleyin. s olanak tanır. Bunu başarmak eklemek için GridView sayfasına ayarlayın, `ID` özelliğine `SuppliersProducts`, ve adlı yeni bir ObjectDataSource ekleme `SuppliersProductsDataSource`.


[![SuppliersProductsDataSource adlı yeni bir ObjectDataSource oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Şekil 8**: yeni ObjectDataSource adlandırılmış oluşturma `SuppliersProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))


S ProductsBLL sınıfını kullanmak için bu ObjectDataSource yapılandırma `GetProductsBySupplierID(supplierID)` yöntemi (bkz. Şekil 9). Bu GridView ayarlanması için bir ürün s fiyatı izin verir, ancak won t düzenleme veya özellikleri GridView silme yerleşik kullanıyor olabilir. Bu nedenle, biz güncelleştirme, ekleme ve silme sekmeleri ObjectDataSource s için aşağı açılan listesine (hiçbiri) ayarlayabilirsiniz.


[![S GetProductsBySupplierID(supplierID) yöntemi ProductsBLL sınıfını kullanmak için veri kaynağını yapılandırma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Şekil 9**: kullanmak için veri kaynağını yapılandırma `ProductsBLL` s sınıfı `GetProductsBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))


Bu yana `GetProductsBySupplierID(supplierID)` yöntemi giriş parametresi kabul eder, bu parametre değeri kaynak için bize ObjectDataSource Sihirbazı'nı ister. İçinde geçirmek için `SupplierID` değer FormView seçin, parametre kaynak aşağı açılan liste denetimi ve ControlId aşağı açılan listesine ayarlayın `Suppliers` (FormView kimliği 2. adımda oluşturulan).


[![SupplierID Üreticiler FormView denetiminden parametre gelmesi gerektiğini belirtin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Şekil 10**: belirtmek *`supplierID`* gereken parametre gelen `Suppliers` FormView denetimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))


ObjectDataSource Sihirbazı'nı tamamladıktan sonra GridView BoundField veya CheckBoxField ürün s veri alanların her biri için içerir. Let s kırpma Bu aşağı göstermek için yalnızca `ProductName` ve `UnitPrice` ile birlikte BoundFields `Discontinued` CheckBoxField; Ayrıca, s biçimi izin `UnitPrice` BoundField sağlayacak şekilde kendi metin para birimi olarak biçimlendirilir. GridView ve `SuppliersProductsDataSource` ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki biçimlendirme benzer görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

Bu noktada öğreticimizi kullanıcının bir üretici en üstünde FormView seçim ve alt GridView aracılığıyla üretici tarafından sağlanan ürünleri görüntülemek için bir ana/ayrıntı raporu görüntüler. Şekil 11, Tokyo Traders tedarikçi FormView seçerken bu sayfayı ekran görüntüsü gösterilmektedir.


[![Seçili sağlayıcı s ürünleri GridView görüntülenir](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Şekil 11**: Seçili tedarikçi s ürünleri GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))


## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>4. adım: DAL ve BLL yöntemleri için bir sağlayıcının tüm ürünleri durdurmaya oluşturma

Düğme için FormView eklemeden önce tıklatıldığında, tüm sona erdirir tedarikçi s ürünlerin, önce DAL ve bu eylem gerçekleştirir BLL için bir yöntem eklemek ihtiyacımız. Özellikle, bu yöntem olarak adlandırılacaktır `DiscontinueAllProductsForSupplier(supplierID)`. FormView s düğmesine tıklandığında, biz bu yöntemi, iş mantığı katmanı geçirme seçili tedarikçi s çağırma `SupplierID`; BLL sonra verecek karşılık gelen veri erişim katmanı yönteme çağıracaktır bir `UPDATE` deyimi Belirtilen sağlayıcı s ürünleri sona erdirir veritabanı.

Biz önceki öğreticilerimizi yaptığınız gibi bir aşağıdan yukarıya yaklaşım DAL yöntemi, ardından BLL yöntemi oluşturma ve son olarak ASP.NET sayfasında işlevselliği uygulama başlayarak kullanacağız. Açık `Northwind.xsd` yazılan kümesinde `App_Code/DAL` klasörü ve yeni bir yöntem ekleyin `ProductsTableAdapter` (sağ tıklayın `ProductsTableAdapter` ve Sorgu Ekle'ı seçin). Bunun yapılması bize yeni yöntemi ekleme işleminde size kılavuzluk eder TableAdapter sorgu Yapılandırma Sihirbazı getirir. Bizim DAL yöntemi geçici SQL deyimi kullandığınızı belirterek başlatın.


[![Geçici SQL deyimi kullanarak DAL yöntemi oluşturma](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Şekil 12**: DAL yöntemini kullanarak bir geçici SQL deyimi oluşturmak ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))


Ardından, sihirbaz bize ne tür oluşturmak için sorgu aldığını ister. Bu yana `DiscontinueAllProductsForSupplier(supplierID)` yöntemi güncellemeniz gerekir `Products` ayarı veritabanı tablosu `Discontinued` için belirtilen tarafından sağlanan tüm ürünleri 1 alan *`supplierID`*, verileri güncelleştiren bir sorgu oluşturmak ihtiyacımız.


[![Güncelleştirme sorgu türünü seçin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Şekil 13**: güncelleştirme sorgu türünü seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))


TableAdapter s varolan sonraki sihirbaz ekranında sağlar `UPDATE` tanımlanan alanların her biri güncelleştirmeleri deyimi `Products` DataTable. Bu sorgu metni aşağıdaki deyimiyle değiştirin:


[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Bu sorgu girme ve son Sihirbazı ekran için yeni bir yöntem s ad ister İleri tıklatarak sonra kullanın `DiscontinueAllProductsForSupplier`. Son düğmesine tıklayarak Sihirbazı tamamlayın. Veri kümesi Tasarımcısı döndürme bağlı yeni bir yöntemi görmelisiniz `ProductsTableAdapter` adlı `DiscontinueAllProductsForSupplier(@SupplierID)`.


[![Yeni DAL yöntemi DiscontinueAllProductsForSupplier adı](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Şekil 14**: yeni DAL yöntem adı `DiscontinueAllProductsForSupplier` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))


İle `DiscontinueAllProductsForSupplier(supplierID)` veri erişim katmanı'ndaki oluşturulan bizim sonraki görev yöntemidir oluşturmak için `DiscontinueAllProductsForSupplier(supplierID)` iş mantığı katmanı yöntemi. Bunu gerçekleştirmek için açık `ProductsBLL` sınıf dosya ve aşağıdakileri ekleyin:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Bu yöntem yalnızca aşağı çağırır `DiscontinueAllProductsForSupplier(supplierID)` sağlanan geçirme DAL yönteminde *`supplierID`* parametre değeri. Bir sağlayıcının belirli koşullar altında kullanımdan kaldırılacak için s ürünleri yalnızca izin verilen tüm iş kuralları olsaydı, bu kurallar burada BLL uygulanmalıdır.

> [!NOTE]
> Farklı `UpdateProduct` içinde overloads `ProductsBLL` sınıfı, `DiscontinueAllProductsForSupplier(supplierID)` yöntem imzası içermez `DataObjectMethodAttribute` özniteliği (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Bu önleyen `DiscontinueAllProductsForSupplier(supplierID)` UPDATE sekmesi ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı'nı s aşağı açılan listeden yöntemi. T ve biz çağırma çünkü bu öznitelik atlanmış `DiscontinueAllProductsForSupplier(supplierID)` ASP.NET sayfamızı olay işleyicisinde doğrudan yönteminden.


## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>5. adım: Ekleme bir FormView tüm ürünleri düğmesine Durdur

İle `DiscontinueAllProductsForSupplier(supplierID)` DAL ve BLL yönteminde tamamlandı, seçili sağlayıcı FormView s düğmesi Web denetim eklemek için tüm ürünleri Durdur olanağı eklemek için son adım `ItemTemplate`. Let s Durdur tüm ürünleri düğme metni tedarikçi s telefon numarasıyla aşağıda böyle bir düğme ekleme ve bir `ID` özellik değerinin `DiscontinueAllProductsForSupplier`. FormView s akıllı etiket Şablonları Düzenle bağlantıya tıklayarak bu düğme Web denetimi Tasarımcısı aracılığıyla ekleyebilirsiniz (bkz. Şekil 15), veya doğrudan Tanımlayıcı Sözdizimi.


[![Ekleme bir FormView s ItemTemplate için tüm ürünleri düğmesi Web Denetimi Durdur](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Şekil 15**: Durdur tüm ürünleri düğmesi Web denetim FormView s ekleme `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))


Ne zaman düğmesine tıklandığında sayfasında, bir geri gönderme ensues kullanıcı ziyaret ve FormView s tarafından [ `ItemCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) etkinleşir. Bu düğmeye tıklandığında yanıt özel kod yürütmek için bu olay için bir olay işleyicisi oluşturabilirsiniz. , Yine de anlamak `ItemCommand` olay ateşlenir *herhangi* düğmesi, LinkButton veya ImageButton Web denetimi FormView içinde tıklattınız. Yani, kullanıcı FormView diğerine bir sayfadan diğerine taşındığında `ItemCommand` olay ateşlenir; kullanıcı yeni, düzenleme, tıklar veya ekleme, güncelleştirme veya silme destekleyen bir FormView sildiğinizde aynı anlama.

Bu yana `ItemCommand` olay işleyicisi ihtiyacımız tüm ürünleri Durdur düğmesini tıklandığını belirleme için bir yol veya başka bir düğme ise, hangi düğmeye tıklandığında bağımsız olarak etkinleşir. Bunu başarmak için biz düğmesi Web denetimi s ayarlayabilirsiniz `CommandName` tanımlayan bir değer özelliği. Ne zaman düğmesine tıklandığında, bu `CommandName` değeri içine geçirilir `ItemCommand` tüm ürünleri Durdur düğmesine tıklandığında düğmesi olup olmadığını belirlemek için bize etkinleştirme olay işleyicisi. Durdur tüm ürünleri düğmesi s ayarlamak `CommandName` DiscontinueProducts özelliğine.

Son olarak, kullanıcının gerçekten seçili tedarikçi s ürünleri durdurmaya istediği emin olmak için bir istemci-tarafı Onayla iletişim kutusunu kullanın s olanak tanır. İçinde gördüğümüz gibi [ekleme istemci-tarafı onay olduğunda silme](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) öğretici, bu gerçekleştirilebilir JavaScript bir bit. Özellikle düğme Web denetimi s OnClientClick özelliğini ayarlayın `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Bu değişiklikleri yaptıktan sonra FormView s tanımlayıcı sözdizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Ardından, FormView s için bir olay işleyicisi oluşturun `ItemCommand` olay. Bu olay işleyicisi biz önce tüm ürünleri Durdur düğmesini tıklandığını olup olmadığını belirlemeniz gerekir. Bu nedenle, biz örneği oluşturmak istiyorsanız `ProductsBLL` sınıfı ve çağırma kendi `DiscontinueAllProductsForSupplier(supplierID)` tümleştirilmesidir yöntemi `SupplierID` seçili FormView biri:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Unutmayın `SupplierID` FormView geçerli seçili tedarikçi, FormView s kullanılarak erişilebilir [ `SelectedValue` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). `SelectedValue` Özelliği, ilk veri anahtar FormView görüntülenmesini kaydının değerini döndürür. FormView s [ `DataKeyNames` özelliği](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), verileri anahtar değerlerini alanları çekilen, veri belirten otomatik olarak ayarlandığı `SupplierID` ObjectDataSource FormView geri bağlanırken Visual Studio tarafından Adım 2'de.

İle `ItemCommand` oluşturulan, olay işleyicisi sayfayı test etmek için bir dakikanızı alın. Cooperativa de Quesos için Gözat 'Las Cabras' tedarikçi (bunu s benim için FormView beşinci tedarikçi). Bu sağlayıcı her ikisi de olan iki ürünler, Queso Cabrales ve Queso Manchego La Pastora sağlar *değil* devam etmez.

Imagine that Cooperativa de Quesos 'Las Cabras' has gone out of business and therefore its products are to be discontinued. Tıklatın tüm ürünleri düğmesi durdur. Bu istemci-tarafı Onayla iletişim kutusu görüntüler (bkz. Şekil 16) kutusunda.


[![Cooperativa de Quesos Las Cabras kaynakları iki etkin ürünler](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Şekil 16**: Cooperativa de Quesos Las Cabras kaynakları iki etkin ürünler ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))


İstemci-tarafı Onayla iletişim kutusunda Tamam'ı tıklatırsanız, form gönderme geri gönderimin içinde neden devam FormView s `ItemCommand` olayını ateşle. Çağrılırken, oluşturduğumuz olay işleyicisi sonra yürütecek `DiscontinueAllProductsForSupplier(supplierID)` yöntemi ve Queso Cabrales ve Queso Manchego La Pastora ürünleri sona erdirme.

GridView s görünüm durumu devre dışı bıraktıysanız, GridView temel alınan veri deposuna her geri göndermede DataSet'e ve bu nedenle hemen (bkz. Şekil 17), bu iki ürün artık üretilmeyen yansıtacak şekilde güncelleştirilir. Ancak GridView Görünüm durumuna devre dışı bıraktığınız değil, bu değişikliği yaptıktan sonra GridView verileri yeniden el ile bağlamanız gerekir. Bunu başarmak için yalnızca GridView s çağırmaya `DataBind()` yöntemi çağırma hemen sonra `DiscontinueAllProductsForSupplier(supplierID)` yöntemi.


[![Tüm ürünleri Durdur düğmesine tıkladıktan sonra tedarikçi s ürün buna göre güncelleştirilir değil](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Şekil 17**: tedarikçi s ürün tüm ürünleri Durdur düğmesine Tıklandıktan sonra buna göre güncelleştirilir değil ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))


## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>6. adım: bir UpdateProduct aşırı ürün s fiyat ayarlamak için iş mantığı katmanı oluşturma

Gibi FormView Durdur tüm ürünleri düğmesi artırma ve GridView üründe fiyat azaltmak için düğmeler eklemek için biz öncelikle uygun veri erişim katmanı ve iş mantığı katmanı yöntemleri eklemeniz gerekir. Biz DAL tek ürün satırda güncelleştiren bir yöntem zaten sahip olduğundan, biz için yeni bir aşırı oluşturarak bu tür işlevselliği sağlayabilen `UpdateProduct` BLL yöntemi.

Bizim geçmiş `UpdateProduct` aşırı ürün alanlarının bileşimi skaler giriş değerleri olarak aldıysanız ve sonra belirtilen ürün için yalnızca bu alanları güncelleştirdiniz. Bu aşırı yüklemesi için Biz bu standart olandan biraz farklılık ve bunun yerine ürünün s geçirin `ProductID` ve ayarlamak için yüzdeyle `UnitPrice` (yeni geçirme aksine ayarlanmış `UnitPrice` kendisini). Bu yaklaşım ASP.NET sayfası arka plan kodu sınıfında yazmak için ihtiyacımız kodu basitleştirir, biz güncelleştireceğinizi olduğundan t sahip geçerli ürün s belirleme ile rahatsız `UnitPrice`.

`UpdateProduct` Bu öğretici aşağıda gösterilen için aşırı yükleme:


[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Bu aşırı belirtilen ürün DAL s ile ilgili bilgileri alır `GetProductByProductID(productID)` yöntemi. Ardından bakar olup olmadığını s ürün `UnitPrice` bir veritabanı atanan `NULL` değeri. İse, fiyat sol değiştirilmemiş. Ancak, varsa, olmayan bir`NULL` `UnitPrice` değeri, yöntem s ürün güncelleştirmeleri `UnitPrice` belirtilen yüzde (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>7. adım: GridView artırma ve azaltma düğmeleri ekleme

GridView (ve DetailsView) hem de alanlarının bir koleksiyonu oluşur. BoundFields, CheckBoxFields ve TemplateFields ek olarak, ASP.NET adından da anlaşılacağı gibi her satır için düğmesi, LinkButton ya da ImageButton olan bir sütun olarak işleyen ButtonField içerir. Benzer şekilde tıklatarak FormView *herhangi* GridView disk belleği düğmeleri, düzenleme veya silme düğmeleri, sıralama düğmeleri ve benzeri içinde düğmesini geri gönderimin neden olur ve GridView s başlatır [ `RowCommand` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx).

ButtonField sahip bir `CommandName` her düğmeleri için belirtilen değer atar özelliği `CommandName` özellikleri. FormView ile gibi `CommandName` değeri tarafından kullanılan `RowCommand` hangi düğmenin tıklandığını belirleme için olay işleyicisi.

Let s GridView düğme metni fiyat + 10 biriyle iki yeni ButtonFields eklemek % ve diğer fiyat -10 metinle %. Bu ButtonFields eklemek için GridView s akıllı etiket Düzenle sütunları bağlantısından tıklayın, sol üst listesinden ButtonField alan türü seçin ve Ekle düğmesine tıklayın.


![GridView iki ButtonFields ekleyin](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Şekil 18**: iki ButtonFields GridView ekleme


İlk iki GridView alanlar olarak görünmesini sağlayacak şekilde iki ButtonFields taşıyın. Ardından, ayarlayın `Text` özelliklerini + %10 fiyat bu iki ButtonFields ve fiyat -10% ve `CommandName` özellikleri IncreasePrice ve DecreasePrice, sırasıyla. Varsayılan olarak, bir ButtonField, sütunu düğmelerinin LinkButtons işler. Bu, ancak ButtonField s değiştirilebilir [ `ButtonType` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx). Normal düğmeler işlenen bu iki ButtonFields sahip s sağlar; Bu nedenle, `ButtonType` özelliğine `Button`. Bu değişiklikler yapıldıktan sonra Şekil 19 alanları iletişim kutusu gösterir; Aşağıdaki GridView s bildirim temelli biçimlendirme olur.


![ButtonFields metin, CommandName ve ButtonType özelliklerini yapılandırın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Şekil 19**: ButtonFields yapılandırma `Text`, `CommandName`, ve `ButtonType` özellikleri


[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Oluşturulan bu ButtonFields ile son adım GridView s için bir olay işleyicisi oluşturmaktır `RowCommand` olay. Çünkü harekete varsa bu olay işleyicisi bir fiyat ya da + %10 veya fiyat -10% düğmeler tıklandığında, gereksinimlerini belirlemek için `ProductID` , düğme tıklandığını ve ardından çağırma satır için `ProductsBLL` s sınıfı `UpdateProduct` uygun geçirme yöntemi `UnitPrice` yüzde ayarlama ile birlikte `ProductID`. Aşağıdaki kod, şu görevleri gerçekleştirir:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Belirlemek için `ProductID` satır, fiyat + 10 % veya fiyat -10% düğmesine tıklandığında, GridView s başvurun ihtiyacımız `DataKeys` koleksiyonu. Bu koleksiyon belirtilen alanların değerlerini tutan `DataKeyNames` her GridView satır özelliği. GridView s itibaren `DataKeyNames` özelliğinin ayarlandığı ProductID için Visual Studio tarafından ObjectDataSource GridView için bağlanırken `DataKeys(rowIndex).Value` sağlar `ProductID` için belirtilen *rowIndex*.

ButtonField içinde otomatik olarak geçirir *rowIndex* , düğmesine tıklanana aracılığıyla satırın `e.CommandArgument` parametresi. Bu nedenle, belirlemek için `ProductID` satır, fiyat + 10 % veya fiyat -10% düğmesine tıklandığında, kullanırız: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Olarak tüm ürünleri Durdur düğmesini tıklatıp GridView s görünüm durumu devre dışı bıraktıysanız GridView temel alınan veri deposuna her geri göndermede DataSet'e ve bu nedenle hemen tıklatarak öğesinden oluşan bir fiyat değişikliği yansıtacak şekilde güncelleştirilir düğmelerin ya da. Ancak GridView Görünüm durumuna devre dışı bıraktığınız değil, bu değişikliği yaptıktan sonra GridView verileri yeniden el ile bağlamanız gerekir. Bunu başarmak için yalnızca GridView s çağırmaya `DataBind()` yöntemi çağırma hemen sonra `UpdateProduct` yöntemi.

Şekil 20 sayfa büyükanne Kelly's yerimizle tarafından sağlanan ürünleri görüntülerken gösterir. Şekil %21 düğmesi iki kez büyükanne'nın Böğürtlen yayılan ve fiyat -10% düğmesi için bir kez Northwoods Cranberry Sosu tıklatıldıktan + 10 fiyat sonra sonuçları gösterir.


[![GridView + 10 fiyatına dahildir % ve fiyat -10% düğmeleri](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Şekil 20**: GridView içeren fiyat + %10 ve % düğmeleri fiyat -10 ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))


[![Birinci ve üçüncü ürün fiyatlar fiyat + 10 güncelleştirildi % ve fiyat -10% düğmeleri](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Şekil 21**: birinci ve üçüncü ürün güncelleştirildi + 10 fiyat üzerinden fiyatları % ve % düğmeleri fiyat -10 ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))


> [!NOTE]
> Düğmeler, LinkButtons veya kendi TemplateFields eklenen ImageButtons ayrıca GridView (ve DetailsView) olabilir. GridView s BoundField ile tıklatıldığında, bu düğmeleri geri gönderimin anlamına şekilde yükseltme `RowCommand` olay. Ne zaman ekleme düğmeleri bir TemplateField, ancak s düğmesi `CommandArgument` ButtonFields kullanırken olduğu gibi satırın dizini otomatik olarak ayarlanmadı. Satır dizini içinde tıklandığını düğmesinin belirlemeniz gerekiyorsa `RowCommand` olay işleyicisi s düğmesi el ile ayarlamanız gerekir `CommandArgument` kendi bildirim temelli söz dizimini kullanarak kodu gibi TemplateField içinde bir özellik:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.


## <a name="summary"></a>Özet

GridView, DetailsView ve FormView denetimleri tüm düğmeler, LinkButtons veya ImageButtons içerebilir. Bu tür düğme tıklatıldığında, geri gönderimin neden ve yükseltmek `ItemCommand` FormView ve DetailsView denetimlerinde olay ve `RowCommand` GridView olayı. Bu verileri Web denetimleri kayıtlarını düzenleme veya silme gibi ortak komutu ile ilgili eylemler işlemek için yerleşik işlevselliğe sahiptir. Ancak, aynı zamanda geçebiliriz kullanım düğmeleri, tıklatıldığında, kendi özel kod yürütülmekte olan yanıt.

Bunu gerçekleştirmek için bir olay işleyicisi oluşturmak ihtiyacımız `ItemCommand` veya `RowCommand` olay. Bu olay işleyicisi biz önce gelen kontrol `CommandName` değeri hangi düğmenin tıklandığını belirleme ve sonra uygun özel eylemi gerçekleştirin. Bu öğreticide düğmeleri ve ButtonFields tüm ürünleri için belirtilen bir üretici sona erdirme veya artırmak veya belirli bir ürünü fiyat 10 oranında azaltmak için nasıl kullanılacağı gördük.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-and-responding-to-buttons-to-a-gridview-cs.md)
