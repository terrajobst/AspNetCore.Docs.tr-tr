---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Veri değişikliği işlevselliğini sınırlama (VB) kullanıcı tabanlı | Microsoft Docs
author: rick-anderson
description: Verileri düzenleme olanağı tanıyan bir web uygulamasında, farklı kullanıcı hesapları, farklı veri düzenleme ayrıcalıklara sahip olmayabilir. Bu öğreticide inceleyeceğiz nasıl t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 23e23c288ccceab7f7e1c07aa9a902bef4045de0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836823"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Kullanıcıya (VB) bağlı veri değişikliği işlevselliğini sınırlama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) veya [PDF olarak indirin](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> Verileri düzenleme olanağı tanıyan bir web uygulamasında, farklı kullanıcı hesapları, farklı veri düzenleme ayrıcalıklara sahip olmayabilir. Bu öğreticide ziyaret kullanıcıya bağlı veri değişikliği özellikleri dinamik olarak ayarlamak nasıl inceleyeceğiz.


## <a name="introduction"></a>Giriş

Web uygulamalarını birkaç kullanıcı hesaplarını ve farklı seçenekler, raporlar ve oturum açmış kullanıcıya bağlı işlevler sağlar. Örneğin, öğreticilerimizden ile belki - kendi şirket adı gibi tedarikçi bilgilerle birlikte ürünlerini - adlarını ve birim başına miktarı hakkında site ve güncelleştirme genel bilgiler oturum açmak için tedarikçi şirketlerden kullanıcıların istiyoruz, Adres, s kişi bilgileri ve benzeri. Ayrıca, biz Şirketimiz kişilerden için oturum açın ve ürün bilgileri, stoktaki birim düzeyi yeniden sıralama vb. gibi güncelleştirme bazı kullanıcı hesapları eklemek isteyebilirsiniz. Web uygulamamız da (oturum kişiler) ziyaret etmek anonim kullanıcılar izin verebilir ancak bunları veri görüntülemek için sınırlandırır. Böyle bir kullanıcı hesabı sistem ile yerinde, ekleme, düzenleme ve silme özellikleri şu anda oturum açmış kullanıcı için uygun sunmak için ASP.NET sayfalarımızın biz veri Web denetimleri tercih etmelisiniz.

Bu öğreticide ziyaret kullanıcıya bağlı veri değişikliği özellikleri dinamik olarak ayarlamak nasıl inceleyeceğiz. Özellikle, düzenlenebilir bir DetailsView sağlayıcısı tarafından sağlanan olduğu ürünleri listeler GridView yanı sıra, üreticiler bilgileri görüntüleyen bir sayfa oluşturacağız. Bizim şirketten sayfasını ziyaret ederek kullanıcı ise yapabilirler: herhangi bir sağlayıcı s bilgi; görüntüleyin kullanıcının adresini Düzenle; bilgileri için sağlayıcı tarafından sağlanan herhangi bir ürünü ve düzenleyin. Ancak, kullanıcı belirli bir şirketten ise yalnızca yapabilir görüntüleyin ve kendi adres bilgilerini düzenlemek ve yalnızca kullanımdan olarak işaretlenen değil ürünlerini düzenleyebilirsiniz.


[![Bir kullanıcıdan şirket portalımız tedarikçi s bilgileri düzenleyebilirsiniz](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Şekil 1**: bir kullanıcı bilgileri Mız şirket olabilir Düzenle herhangi tedarikçi s ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Bir kullanıcıdan bir belirli tedarikçi Can yalnızca görüntüleme ve düzenleme bilgilerine](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Şekil 2**: bir kullanıcı bir belirli tedarikçi olabilir yalnızca görünümü ve düzenleme bilgilerine ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Let s başlayın!

> [!NOTE]
> ASP.NET 2.0 s üyelik sistemi oluşturma, yönetme ve kullanıcı hesapları doğrulanıyor için standartlaştırılmış, Genişletilebilir bir platform sağlar. Üyelik Sistemi incelemesini bu öğreticileri kapsamı dışında olduğundan, bu öğreticide bunun yerine "üyelik belirli bir üretici veya Şirketimiz oldukları seçmek anonim ziyaretçiler vererek fakes". Üyelik hakkında daha fazla bilgi için bkz my [İnceleme ASP.NET 2.0 s üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) makale serisi.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>1. adım: kullanıcı erişim haklarını belirtmek izin verme

Gerçek web uygulamasında, bir kullanıcı s hesabı bilgilerini Şirketimiz için veya belirli bir üretici için çalışan ve kullanıcı siteye oturum açtıktan sonra bu bilgileri ASP.NET sayfalarımızın programlı olarak erişilebilir olup olmadığını içerir. Bu bilgiler, kullanıcı düzeyi hesabı bilgilerini profil sistemi aracılığıyla veya bazı özel araçlarla olarak ASP.NET 2.0 s rolleri sistem yakalanan.

Bu öğreticinin amacı, oturum açmış kullanıcıya bağlı veri değiştirme özelliklerini ayarlayarak göstermektir ve gösterimi ASP.NET 2.0 s üyelik, roller ve profil sistemleri için tasarlanmamıştır olduğundan, bir çok basit mekanizması belirlemek için kullanacağız Özellikleri sayfası - içinden kullanıcı belirtebilirsiniz, görüntüleyebilir ve tedarikçileri bilgi veya hiçbirini, alternatif olarak, hangi Düzenle gerektiğini bir DropDownList ziyaret kullanıcı için belirli tedarikçi s bilgileri görüntüleyebilir ve düzenleyebilirsiniz. Kullanıcı Filiz görüntüleyebilir ve düzenleme tüm sağlayıcı bilgileri (varsayılan) olduğunu gösteriyorsa, kendisi tüm Üreticiler sayfasında, herhangi tedarikçi s adresi bilgileri düzenleyin ve seçili sağlayıcısı tarafından sağlanan herhangi bir ürün için birim başına miktarı ve adını düzenleyin. Kullanıcının kendisi yalnızca görüntüleyebilir ve belirli tedarikçi, ancak sonra kendisi yalnızca ayrıntıları ve ürünleri için bir üretici görüntüleyebilir ve yalnızca düzenleme güncelleştirmesi adı ve miktar olan bu ürünlere yönelik birim bilgileri başına gösteriyorsa *değil* kullanımdan kaldırıldı.

Bu öğreticide ilk adımımız, bu DropDownList oluşturmak ve sistemde Tedarikçiler ile doldurmak için ise. Açık `UserLevelAccess.aspx` sayfasını `EditInsertDelete` klasöründe bir DropDownList ekleyin, `ID` özelliği `Suppliers`ve bu DropDownList adlı yeni bir ObjectDataSource bağlama `AllSuppliersDataSource`.


[![AllSuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Şekil 3**: adlı yeni bir ObjectDataSource oluşturma `AllSuppliersDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Tüm Üreticiler dahil etmek için bu DropDownList istiyoruz olduğundan, çağrılacak ObjectDataSource yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi. ObjectDataSource s de emin `Update()` yöntemi eşlenmiş durumda `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi, bu ObjectDataSource da kullanılacak ekleyeceğiz 2. adımda DetailsView tarafından.

ObjectDataSource sihirbazını tamamladıktan sonra yapılandırarak adımları tamamlamak `Suppliers` DropDownList bunu gösterir şekilde `CompanyName` veri alanı `SupplierID` değeri olarak her veri alanı `ListItem`.


[![Üreticiler DropDownList CompanyName ve SupplierID veri alanlarını kullanmak için yapılandırma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Şekil 4**: yapılandırma `Suppliers` kullanılacak DropDownList `CompanyName` ve `SupplierID` veri alanlarını ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


Bu noktada, DropDownList veritabanında tedarikçileri şirket adlarını listeler. Ancak, biz de DropDownList "Tüm Üreticiler Göster/Düzenle" seçeneğine eklemeniz gerekir. Bunu gerçekleştirmek için ayarlanmış `Suppliers` DropDownList s `AppendDataBoundItems` özelliğini `true` ve ardından eklemek bir `ListItem` olan `Text` özelliği "Tüm Üreticiler Göster/Düzenle" ve değeri `-1`. Bu bildirim temelli işaretleme veya tasarımcı aracılığıyla doğrudan özellikler penceresine gidip DropDownList s üç noktaya tıklayarak eklenebilir `Items` özelliği.

> [!NOTE]
> Kiracıurl [ *ana/ayrıntı filtreleme ile bir DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) Tümünü Seç öğesi bir sınırlama DropDownList ekleme hakkında daha ayrıntılı bir açıklaması için öğretici.


Sonra `AppendDataBoundItems` özelliğini ayarlayın ve `ListItem` eklediğiniz DropDownList s bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Şekil 5, bir tarayıcıdan görüntülendiğinde geçerli ilerlememizin ekran görüntüsü gösterilmektedir.


[![Tüm ListItem yanı sıra, her bir sağlayıcı için bir Göster tedarikçileri DropDownList içerir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Şekil 5**: `Suppliers` DropDownList içeren bir Tümünü Göster `ListItem`, artı bir her üretici için ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Kullanıcı Seçimi hemen değiştirildikten sonra kullanıcı arabirimini güncelleştirmek istiyoruz, ayarlanması `Suppliers` DropDownList s `AutoPostBack` özelliğini `true`. 2. adımda DropDownList seçim temel alınarak supplier(s) bilgilerini gösteren bir DetailsView denetimi oluşturacağız. Ardından, adım 3'te bu DropDownList s için bir olay işleyicisi oluşturacağız `SelectedIndexChanged` içinde ekleyeceğiz DetailsView için uygun üretici bilgilerini bağlayan bir kod üzerinde seçili sağlayıcı tabanlı olay.

## <a name="step-2-adding-a-detailsview-control"></a>2. adım: bir DetailsView denetimi ekleme

Sağlayıcı bilgileri görüntülemek için bir DetailsView kullanın s olanak tanır. Kimler görüntüleyebilir veya tüm Üreticiler Düzenle kullanıcıdan DetailsView, disk belleği, kullanıcının aynı anda sağlayıcı bilgileri bir kaydı adım izin destekler. Kullanıcı için belirli bir üretici çalışırsa, ancak DetailsView yalnızca belirli üretici s bilgileri gösterir ve disk belleği arabirimi dahil edilmez. Her iki durumda da kullanıcının tedarikçi s Adres, şehir ve ülke alanları düzenlemesine izin vermek DetailsView gerekir.

Sayfanın bir DetailsView eklemek `Suppliers` DropDownList, ayarla, `ID` özelliğini `SupplierDetails`ve öğeyi `AllSuppliersDataSource` ObjectDataSource önceki adımda oluşturulan. Ardından, DetailsView s akıllı etiketinde etkinleştirme sayfalama ve düzenlemeyi etkinleştir onay kutularını işaretleyin.

> [!NOTE]
> T don akıllı DetailsView s Düzenlemeyi Etkinleştir seçeneği görürseniz ObjectDataSource s eşleşmiyor çünkü s etiketlemek `Update()` yönteme `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi. Geri dönüp bu yapılandırma değişikliği, düzenlemeyi etkinleştir seçeneği DetailsView s akıllı etiket görünmesi gereken yapmak için bir dakikanızı ayırın.


Bu yana `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi yalnızca dört parametre - kabul `supplierID`, `address`, `city`, ve `country` -DetailsView s BoundFields değiştirin böylece `CompanyName` ve `Phone` BoundFields salt okunurdur. Ayrıca, kaldırma `SupplierID` BoundField toptan. Son olarak, `AllSuppliersDataSource` ObjectDataSource şu anda sahip kendi `OldValuesParameterFormatString` özelliğini `original_{0}`. Bu özellik ayarı bildirim temelli söz dizimi özelliği tamamen kaldırmak veya varsayılan değere ayarlamak için bir dakikanızı ayırın `{0}`.

Yapılandırdıktan sonra `SupplierDetails` DetailsView ve `AllSuppliersDataSource` ObjectDataSource, biz aşağıdaki bildirim temelli biçimlendirme olacaktır:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Bu noktada DetailsView aracılığıyla belleğine alınabilen ve s seçili sağlayıcı adresi bilgilerini, yapılan seçim bağımsız olarak güncelleştirilebilir `Suppliers` DropDownList (bkz. Şekil 6).


[![Tüm Üreticiler bilgileri görüntüleyebilir ve adresini güncelleştirildi](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Şekil 6**: Any tedarikçileri bilgileri görüntülenebilir ve kendi adres güncelleştirildi ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>3. adım: Yalnızca seçili sağlayıcı s bilgileri görüntüleme

Sayfamızı bilgi olup belirli bir üretici gelen seçildi bağımsız olarak tüm Üreticiler için şu anda görüntüler `Suppliers` DropDownList. Yalnızca seçili sağlayıcı Tedarikçi bilgilerini görüntülemek için biz sayfamızı, belirli bir üretici hakkındaki bilgileri alır bir başka bir ObjectDataSource eklemeniz gerekir.

Adlandırma Ekle sayfası için yeni ObjectDataSource `SingleSupplierDataSource`. Akıllı etiketinde, veri kaynağı yapılandırma bağlantısına tıklayın ve bu kullanın `SuppliersBLL` s sınıfı `GetSupplierBySupplierID(supplierID)` yöntemi. Olduğu gibi `AllSuppliersDataSource` ObjectDataSource, sahip `SingleSupplierDataSource` ObjectDataSource s `Update()` yöntemi eşlenen `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi.


[![SingleSupplierDataSource ObjectDataSource GetSupplierBySupplierID(supplierID) yöntemi kullanmak üzere yapılandırma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Şekil 7**: yapılandırma `SingleSupplierDataSource` kullanılacak ObjectDataSource `GetSupplierBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Ardından, biz yeniden parametre kaynağını belirtmek için istemde `GetSupplierBySupplierID(supplierID)` metodu s `supplierID` giriş parametresi. Bilgi DropDownList, kullanım seçili sağlayıcı için gösterilecek istediğinden `Suppliers` DropDownList s `SelectedValue` parametre kaynağı olarak özelliği.


[![Üreticiler DropDownList satýrýnSupplierID parametre kaynağı kullanın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Şekil 8**: kullanım `Suppliers` DropDownList olarak `supplierID` parametre kaynağı ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Eklenen bile bu ikinci ObjectDataSource ile DetailsView denetimi şu anda her zaman kullanmak üzere yapılandırılmış `AllSuppliersDataSource` ObjectDataSource. DetailsView bağlı olarak tarafından kullanılan veri kaynağı ayarlamak için mantığı eklemek ihtiyacımız `Suppliers` DropDownList öğe seçildi. Bunu yapmak için oluşturun bir `SelectedIndexChanged` tedarikçileri DropDownList için olay işleyicisi. Bu en bir kolayca DropDownList Tasarımcısı'nda çift tıklayarak oluşturabilirsiniz. Bu olay işleyicisi, hangi veri kaynağının kullanılacağını belirlemek gereken ve DetailsView verileri yeniden bağlamanız gerekir. Bu, aşağıdaki kod ile gerçekleştirilir:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Olay işleyicisi, "Tüm Üreticiler Göster/Düzenle" seçeneğini seçili olmadığını belirleyerek başlar. Bu durumda, bu ayarlar `SupplierDetails` DetailsView s `DataSourceID` için `AllSuppliersDataSource` ve ayarlayarak kullanıcı tedarikçileri kümesindeki ilk kaydı döndürür `PageIndex` özelliğinin 0. Ancak, belirli bir üretici DropDownList, DetailsView s kullanıcının seçtiği ise `DataSourceID` atandığı `SingleSuppliersDataSource`. Bağımsız olarak hangi veri kaynağı kullanılır, `SuppliersDetails` modu salt okunur moda geri dönüldü ve veri DetailsView için yapılan bir çağrıyla DataSet'e `SuppliersDetails` denetim s `DataBind()` yöntemi.

"Tüm Üreticiler Göster/Düzenle" seçeneği belirtildi, bu durumda disk belleği arabirimi aracılığıyla sağlayıcıların tümünü görüntülenebilir sürece bu olay işleyicisi ile yerinde DetailsView denetiminde seçili tedarikçi, artık gösterir. Şekil 9, "Tüm Üreticiler Göster/Düzenle" seçeneği seçili sayfada gösterilir; disk belleği arabirimi mevcut olduğunu ziyaret edin ve herhangi bir sağlayıcı güncelleştirmek kullanıcının unutmayın. Şekil 10 sayfada seçilen Ma Ahmet sağlayıcı ile gösterilir. Yalnızca master Ahmet s bilgileri, bu durumda görüntülenebilir ve düzenlenebilir.


[![Tüm Üreticiler bilgileri görüntülenebilir ve düzenlenebilir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Şekil 9**: tüm Üreticiler bilgileri görüntülenebilir ve düzenlenen ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Yalnızca seçili sağlayıcı s bilgileri görüntülenebilir ve düzenlenebilir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Şekil 10**: yalnızca seçili sağlayıcı s bilgi Viewed ve düzenlenen ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Bu öğretici için DropDownList ve DetailsView denetimi s `EnableViewState` ayarlanmalıdır `true` (varsayılan) çünkü DropDownList s `SelectedIndex` ve DetailsView s `DataSourceID` Geri göndermeler arasında özellik s değişiklikleri anımsanacak.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>4. adım: bir düzenlenebilir GridView tedarikçileri ürünleri listeleme

DetailsView tam sonraki adımımız seçili sağlayıcı tarafından sağlanan bu ürünlerin listeleyen bir düzenlenebilir GridView eklemektir. Düzenlemeler yalnızca bu GridView sağlamalıdır `ProductName` ve `QuantityPerUnit` alanları. Ayrıca, bu sayfasını ziyaret ederek kullanıcı belirli bir tedarikçiden varsa bu ürünlerin güncelleştirmeleri yalnızca sağlamalıdır *değil* kullanımdan kaldırıldı. İlk olarak bir aşırı yüklemesini eklemek için gereken bunu sağlamak için `ProductsBLL` s sınıfı `UpdateProducts` alır yöntemi yalnızca `ProductID`, `ProductName`, ve `QuantityPerUnit` alanları girdi olarak. Biz bu nedenle izin eklenmesi kod burada yalnızca bakmak s ve bu süreçte birçok öğreticilerde önceden basamaklı `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Oluşturulan, bu aşırı yükleme ile biz re GridView denetiminde ve onun ilişkili ObjectDataSource eklemek için hazır. Sayfaya yeni GridView ekleyin, kendi `ID` özelliğini `ProductsBySupplier`ve adlı yeni bir ObjectDataSource kullanacak şekilde yapılandırma `ProductsBySupplierDataSource`. Seçili sağlayıcı tarafından bu ürünlerin listelemek için bu GridView istiyoruz beri kullanın `ProductsBLL` s sınıfı `GetProductsBySupplierID(supplierID)` yöntemi. Ayrıca harita `Update()` yeni yönteme `UpdateProduct` oluşturduğumuz aşırı yükleme.


[![Yeni oluşturduğunuz UpdateProduct aşırı yüklemesini kullanın ObjectDataSource yapılandırın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `UpdateProduct` aşırı yükleme, yeni oluşturduğunuz ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Biz re parametre kaynağını seçmeniz istenir `GetProductsBySupplierID(supplierID)` metodu s `supplierID` giriş parametresi. Ürün kullanımı gibi DetailsView seçili sağlayıcı için gösterilecek istediğinden `SuppliersDetails` DetailsView denetiminde s `SelectedValue` parametre kaynağı olarak özelliği.


[![SuppliersDetails DetailsView s SelectedValue özelliği parametre kaynağı olarak kullanın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Şekil 12**: kullanım `SuppliersDetails` DetailsView s `SelectedValue` parametre kaynağı olarak özelliği ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


GridView'a döndürerek, kaldırmak dışında GridView alanların tümünü `ProductName`, `QuantityPerUnit`, ve `Discontinued`, işaretlenmesi `Discontinued` CheckBoxField salt okunur. Ayrıca, akıllı etiket s GridView düzenlemeyi etkinleştir seçeneğini denetleyin. Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme GridView ve ObjectDataSource aşağıdakine benzer görünmelidir:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Bizim önceki ObjectDataSources, bu s ile `OldValuesParameterFormatString` özelliği `original_{0}`, neden olacak sorunları s ürün adı veya birim başına miktarı güncellemeye çalışırken. Bu özelliği tamamen bildirim temelli söz dizimi kaldırın veya varsayılan, ayarına `{0}`.

Bu yapılandırma tamamlandı, sayfamız şimdi GridView içinde seçili sağlayıcı tarafından sağlanan olduğu ürünleri listeler. (bkz. Şekil 13). Şu anda *herhangi* s ürün adı veya birim başına miktar güncelleştirilebilir. Ancak Biz bu işlevselliğin artık üretilmeyen ürünler için belirli bir sağlayıcı ile ilişkili kullanıcılar için kullanılamaz, bizim sayfa mantıksal güncelleştirmeniz gerekir. Biz, adım 5'te bu son parçası üstesinden.


[![Seçili sağlayıcısı tarafından sağlanan ürünleri görüntülenir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Şekil 13**: seçili sağlayıcı tarafından sağlanan ürünleri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Ek olarak bu düzenlenebilir GridView `Suppliers` DropDownList s `SelectedIndexChanged` GridView bir salt okunur duruma döndürmek için olay işleyicisi güncelleştirilmelidir. Farklı bir sağlayıcı, ürün bilgilerini düzenleme sırasında ortasında seçiliyse, aksi takdirde, yeni sağlayıcı için GridView karşılık gelen dizin de düzenlenemez. Bunu önlemek için GridView s ayarlamanız yeterlidir `EditIndex` özelliğini `-1` içinde `SelectedIndexChanged` olay işleyicisi.


Ayrıca, s görünüm durumu GridView (varsayılan davranış) etkin önemli olduğunu hatırlayın. GridView s ayarlarsanız `EnableViewState` özelliğini `false`, eş zamanlı kullanıcıların yanlışlıkla silme veya düzenleme kayıtları riskiyle karşılaşırsınız. Bkz: [uyarısı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews bu düzenleme desteği ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx) daha fazla bilgi için.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>5. adım: Düzenleme kullanımdan ürünleri olduğunda göster/Düzenle tüm Üreticiler seçili için izin verme

Sırada `ProductsBySupplier` GridView tam işlevsel, şu anda çok fazla erişim belirli bir tedarikçiden olan kullanıcılarla verir. İş kurallarımızın bu kullanıcılar artık üretilmeyen ürünler güncelleştiremezsiniz olmamalıdır. Bunu zorunlu kılmak için biz gizle (devre dışı bırakmak bu sayfa bir kullanıcı tarafından bir tedarikçiden ziyaret edildiğinde artık üretilmeyen ürünler GridView satırlardaki Düzenle düğmesini veya).

GridView s için bir olay işleyicisi oluşturun `RowDataBound` olay. Kullanıcı, bu öğreticide, üreticiler DropDownList s denetleyerek belirlenebilir belirli bir sağlayıcı ile ilişkili olup olmadığını belirlemek ihtiyacımız bu olay işleyicisinde `SelectedValue` özellik - varsa, s -1, ardından kullanıcı bir şey diğer belirli bir sağlayıcı ile ilişkili. Bu kullanıcılar için daha sonra ürün kullanımdan olup olmadığını belirlemek ihtiyacımız var. Biz gerçek bir başvuru alın `ProductRow` örneği, GridView satır bağlanan `e.Row.DataItem` bölümünde açıklandığı gibi özellik [ *GridView s altbilgi özeti bilgilerini görüntüleme* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) öğretici. Ürün kullanımdan kaldırılmıştır, Düzenle düğmesini GridView s önceki öğreticide açıklanan teknikleri kullanarak CommandField programlı bir başvuru almak [ *ekleme istemci tarafı doğrulama zaman silme* ](adding-client-side-confirmation-when-deleting-vb.md). Biz size ardından gizleyebilir veya düğmeyi devre dışı bir başvurusu oluşturduktan sonra.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Bu sayfa bir kullanıcı olarak belirli bir tedarikçiden üretilmeyen bu ürünlerin ziyaret olmadığı durumlarda düzenlenemez, bu olay ile işleyici, yerinde bu ürünler için Düzenle düğmesini gizlidir. Örneğin, Chef Acı s Baharat karışımı New Orleans Cajun Delights üretici için kullanımdan kaldırılan bir üründür. Bu belirli bir sağlayıcı için sayfasını ziyaret ederek, bu ürün için Düzenle düğmesini görüş gizli (bkz. Şekil 14). Ancak, "Göster/Düzenle tüm Üreticiler" kullanarak ziyaret ederken Düzenle düğmesi kullanılabilir (bkz: Şekil 15) olur.


[![Tedarikçi belirli kullanıcılar için Chef Acı s Baharat karışımı Düzenle düğmesi gizlenir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Şekil 14**: Chef Acı s Baharat karışımı Düzenle düğmesi gizlenir tedarikçi özgü kullanıcı ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Chef Acı s Baharat karışımı Düzenle düğmesini göster/Düzenle tüm Üreticiler kullanıcılar için görüntülenir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Şekil 15**: Göster/Düzenle tüm Üreticiler kullanıcıları için Chef Acı s Baharat karışımı görüntülenir için Düzenle düğmesini ([tam boyutlu görüntüyü görmek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>İş mantığı katmanı erişim haklarını denetleme

Bu öğreticide ASP.NET sayfası bakımından hangi bilgileri kullanıcının görebileceği tüm mantığı işler ve hangi ürünleri kendisinin güncelleştirebilirsiniz. İdeal olarak, bu mantık ayrıca iş mantığı katmanı mevcut olacaktır. Örneğin, `SuppliersBLL` s sınıfı `GetSuppliers()` (tüm Üreticiler döndüren) yöntemi, o anda oturum açmış kullanıcı olduğundan emin olmak için bir onay içerebilir *değil* belirli bir sağlayıcı ile ilişkili. Benzer şekilde, `UpdateSupplierAddress` yöntemi, o anda oturum açmış kullanıcı ya da Şirketimiz için çalışan (ve bu nedenle tüm Üreticiler adres bilgilerini güncelleştirmesine) emin olmak için bir onay içerebilir veya verisini güncelleştiriliyor sağlayıcı ile ilişkilendirilir.

Müşterilerimize öğreticide s kullanıcı haklarını bir DropDownList BLL sınıfları erişemiyor sayfasında belirlenir çünkü tür BLL katmanı denetimler içermiyordu. Üyelik sistemini veya (Windows kimlik doğrulaması gibi), ASP.NET tarafından sağlanan çıkış-hazır kimlik doğrulama düzenleri birini kullanırken, şu anda oturum açmış kullanıcının s ve rolleri bilgileri böylece erişim yapmadan BLL erişilebilir hakları sunu ve BLL katmanları olası denetler.

## <a name="summary"></a>Özet

Kullanıcı hesapları sağlamak en siteler üzerinde oturum açmış olan kullanıcının dayalı veri değişikliği arabirimini özelleştirme gerekir. Yönetici olmayan kullanıcılar yalnızca güncelleştiriliyor veya kendilerini oluşturan kayıtları silmek için sınırlı olabilir ancak yönetim kullanıcılarının herhangi bir kayıt silip mümkün olabilir. Her senaryo, veri Web denetimleri, ObjectDataSource, olabilir ve iş mantığı katmanı sınıfları eklemek veya oturum açmış kullanıcıya dayanarak bazı işlevlere reddetmek için genişletilebilir. Bu öğreticide Şirketimiz için çalışan veya kullanıcı belirli bir sağlayıcı ile ilişkili olmasına bağlı olarak görüntülenebilir ve düzenlenebilir verileri sınırlamak nasıl gördük.

Bu öğreticiyi bizim inceleme, ekleme, güncelleştirme ve silme FormView GridView ve DetailsView denetimlerini kullanarak verileri sonlandırır. Sonraki öğretici ile başlayarak, sayfalama ve sıralama desteği eklemek için uygulamamızla açacağım.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-client-side-confirmation-when-deleting-vb.md)
