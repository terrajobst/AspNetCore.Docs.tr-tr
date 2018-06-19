---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Veri değişikliği işlevlerini sınırlama tabanlı kullanıcı (VB) | Microsoft Docs
author: rick-anderson
description: Verileri düzenleme olanağı sağlayan bir web uygulamasında farklı kullanıcı hesapları farklı veri düzenleme ayrıcalıklara sahip olmayabilir. Bu öğreticide biz inceleyeceğiz nasıl t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887879"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Kullanıcıya (VB) bağlı bir sınırlama veri değişikliği işlevi
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) veya [PDF indirin](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> Verileri düzenleme olanağı sağlayan bir web uygulamasında farklı kullanıcı hesapları farklı veri düzenleme ayrıcalıklara sahip olmayabilir. Bu öğreticide biz nasıl ziyaret kullanıcıyı temel alarak veri değişikliği özellikleri dinamik olarak ayarlanacağını inceleyeceğiz.


## <a name="introduction"></a>Giriş

Web uygulamalarının sayısı, kullanıcı hesaplarını destekler ve farklı seçenekler, raporları ve üzerinde oturum açmış olan kullanıcının tabanlı işlevsellik sağlar. Örneğin, öğreticilerimizi ile biz kullanıcıların belki - kullanıcıların şirket adı gibi tedarikçi bilgilerle birlikte site ve güncelleştirme genel bilgilerini ürünlerini - adlarını ve birim başına miktar oturum açmak için tedarikçi şirketlerden izin vermek isteyebilirsiniz, Adres, s kişi bilgileri ve benzeri. Ayrıca, biz böylece oturum ve stok birimlerde sipariş düzeyi vb. gibi ürün bilgilerini güncelleştirmek için şirketimizin kişilerden bazı kullanıcı hesapları eklemek isteyebilirsiniz. Web uygulamamız da (oturum kişiler), ziyaret etmek anonim kullanıcılar sağlayabilir, ancak bunları verileri görüntülemek için sınırlandırır. Böyle bir kullanıcı hesabı sistemiyle yerinde, biz Web denetimleri veri ekleme, düzenleme ve silme özellikleri şu anda oturum açmış kullanıcı için uygun sunmak için bizim ASP.NET sayfalarında istersiniz.

Bu öğreticide biz nasıl ziyaret kullanıcıyı temel alarak veri değişikliği özellikleri dinamik olarak ayarlanacağını inceleyeceğiz. Özellikle, sağlayıcısı tarafından sağlanan ürünleri listeler GridView birlikte düzenlenebilir bir DetailsView içinde Üreticiler bilgileri görüntüleyen bir sayfa oluşturacağız. Sayfasını ziyaret kullanıcı bizim şirketten ise, yönetici şunları yapabilir: herhangi bir tedarikçi s bilgi; görüntüleyin kendi adresi düzenleyin; sağlayıcısı tarafından sağlanan herhangi bir ürün bilgileri ve düzenleyin. Ancak, kullanıcı belirli bir şirketten ise yalnızca yapabilir görüntülemek ve kendi adres bilgilerini düzenle ve devam etmeyen olarak işaretlenen değil, ürünlerinin yalnızca düzenleyebilirsiniz.


[![Şirketimizin kullanıcıdan tedarikçi s bilgileri düzenleyebilirsiniz](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Şekil 1**: bir kullanıcı Mız şirket olabilir Düzenle herhangi tedarikçi s bilgi ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Belirli tedarikçi Can yalnızca görüntüleme ve düzenleme bilgilerini kullanıcıdan](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Şekil 2**: bir kullanıcıdan bir belirli tedarikçi olabilir yalnızca görüntüleme ve düzenleme bilgilerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Let s başlayın!

> [!NOTE]
> ASP.NET 2.0 s üyelik sistemi oluşturma, yönetme ve kullanıcı hesaplarını doğrulama için standartlaştırılmış, Genişletilebilir bir platform sağlar. Bir üyelik sistemi incelendiğinde bu öğreticileri kapsamında olduğundan, Bu öğretici bunun yerine "üyelik belirli bir üretici veya şirketimizin oldukları seçmek anonim ziyaretçiler vererek fakes". Üyelik hakkında daha fazla bilgi için bkz my [inceleniyor ASP.NET 2.0 s üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) makalesi serisi.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>1. adım: kullanıcı erişim haklarını belirtmek izin verme

Gerçek dünya web uygulamasında, bir kullanıcı s hesabı bilgilerini şirketimizin veya belirli bir üretici çalışılan ve kullanıcı siteye oturum açtıktan sonra bu bilgileri bizim ASP.NET sayfaları programlı olarak erişilebilir olan olup olmadığını içerir. Bu bilgiler, kullanıcı düzeyinde hesap bilgilerini profil sistemi aracılığıyla veya bazı özel araçlarla olarak ASP.NET 2.0 s rolleri sisteminde yakalanan.

Bu öğreticinin amacı oturum açan kullanıcıyı temel alarak veri değişikliği özellikleri ayarlama göstermektir ve gösterimi ASP.NET 2.0 s üyelik, roller ve profil sistemleri için tasarlanmamıştır olduğundan, bir çok basitçe mekanizması belirlemek için kullanacağız özellikleri sayfasını - alınacağı kullanıcı belirtebilirsiniz bunlar görebilir ve hiçbir sağlayıcılardan bilgi veya alternatif olarak, ne olup olmayacağını DropDownList ziyaret kullanıcı için belirli tedarikçi s bilgilerini görüntüleyin ve düzenleyin. Kullanıcı kendisinin görüntülemek ve böylelikle tüm tedarikçi bilgileri (varsayılan) Düzenle gösteriyorsa, aynen tüm tedarikçileri sayfasında, tüm tedarikçi s adresi bilgileri düzenleyin ve adını ve seçili sağlayıcısı tarafından sağlanan herhangi bir ürün için birim başına miktar düzenleyin. Kullanıcı kendisi yalnızca görüntüleyebilirsiniz ve belirli tedarikçi, ancak, sonra bunları yalnızca ayrıntılar ve ürünleri için bir üretici görüntüleyebilir ve yalnızca düzenleme güncelleştirme adı ve birim bilgileri, bu ürünleri için başına miktar gösteriyorsa *değil* devam etmez.

Bizim ilk Bu öğreticide daha sonra bu DropDownList oluşturup sistemde sağlayıcılarla doldurmak için bir adımdır. Açık `UserLevelAccess.aspx` sayfasındaki `EditInsertDelete` klasöründe bir DropDownList ekleyin, `ID` özelliği ayarlanmış `Suppliers`ve bu DropDownList adlı yeni bir ObjectDataSource bağlama `AllSuppliersDataSource`.


[![AllSuppliersDataSource adlı yeni bir ObjectDataSource oluşturma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Şekil 3**: yeni ObjectDataSource adlandırılmış oluşturma `AllSuppliersDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Tüm tedarikçileri dahil etmek için bu DropDownList istiyoruz beri çağrılacak ObjectDataSource yapılandırma `SuppliersBLL` s sınıfı `GetSuppliers()` yöntemi. ObjectDataSource s de emin `Update()` yöntemi eşleştirilir `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi, bu ObjectDataSource da tarafından kullanılır biz ekleme 2. adımda DetailsView.

ObjectDataSource Sihirbazı'nı tamamladıktan sonra yapılandırarak adımları `Suppliers` DropDownList onu gösterir şekilde `CompanyName` veri alanı `SupplierID` değeri olarak her veri alanı `ListItem`.


[![Üreticiler DropDownList ŞirketAdı ve SupplierID veri alanlarını kullanacak şekilde yapılandırma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Şekil 4**: yapılandırmak `Suppliers` DropDownList kullanılacak `CompanyName` ve `SupplierID` veri alanları ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


Bu noktada, DropDownList veritabanındaki tedarikçileri şirket adlarını listeler. Ancak, biz de DropDownList "Tüm tedarikçileri Göster/Düzenle" seçeneğine eklemeniz gerekir. Bunu gerçekleştirmek için ayarlanmış `Suppliers` DropDownList s `AppendDataBoundItems` özelliğine `true` ve ardından ekleyin bir `ListItem` , `Text` özelliktir "Göster/Düzenle tüm tedarikçileri" ve değeri `-1`. Bu doğrudan bildirim temelli biçimlendirme veya Tasarımcısı aracılığıyla Properties penceresine gidip DropDownList s elipsler tıklayarak eklenebilir `Items` özelliği.

> [!NOTE]
> Geri başvurmak [ *ana/ayrıntı filtreleme ile bir DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) veriye bağlı DropDownList Tümünü Seç öğesi ekleme hakkında daha ayrıntılı bilgi için Öğreticisi.


Sonra `AppendDataBoundItems` özelliğini ayarlayın ve `ListItem` eklenen, DropDownList s bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Şekil 5 bizim geçerli ilerleme ekran görüntüsü bir tarayıcıdan görüntülendiğinde gösterir.


[![Üreticiler DropDownList bir gösterisi, tüm LISTITEM yanı sıra, her sağlayıcı için bir içerir.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Şekil 5**: `Suppliers` DropDownList içeren bir Tümünü Göster `ListItem`, artı bir her sağlayıcı için ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Kullanıcı kendi seçimi hemen değiştikten sonra kullanıcı arabirimini güncelleştirme istiyoruz, ayarlama `Suppliers` DropDownList s `AutoPostBack` özelliğine `true`. 2. adımda DropDownList seçimine göre supplier(s) bilgilerini gösteren DetailsView denetimini oluşturacağız. Ardından, adım 3'te bu DropDownList s için bir olay işleyicisi oluşturacağız `SelectedIndexChanged` olay, hangi hesabına ekleyeceğiz uygun üretici bilgilerini DetailsView'a bağlar kodu göre seçilen sağlayıcısına.

## <a name="step-2-adding-a-detailsview-control"></a>2. adım: DetailsView denetim ekleme

Sağlayıcı bilgileri görüntülemek için bir DetailsView kullanın s olanak tanır. Kimin görüntüleyebilir ve tüm tedarikçileri düzenleyebilirsiniz kullanıcıdan DetailsView, disk belleği, kullanıcının aynı anda sağlayıcı bilgileri bir kaydı adım izin destekler. Kullanıcı için belirli bir üretici çalışırsa, ancak DetailsView yalnızca belirli üretici s bilgileri gösterir ve disk belleği arabirimi dahil edilmez. Her iki durumda da DetailsView tedarikçi s adresi, şehir ve ülke alanları düzenlemek izin vermek gerekiyor.

Sayfanın bir DetailsView eklemek `Suppliers` DropDownList, ayarlayın, `ID` özelliğine `SupplierDetails`ve onu bağladıktan `AllSuppliersDataSource` ObjectDataSource önceki adımda oluşturulan. Ardından, DetailsView s akıllı etiketinden etkinleştirmek disk belleği ve düzenlemeyi etkinleştir onay denetleyin.

> [!NOTE]
> T tan bir akıllı DetailsView s düzenlemeyi etkinleştir seçeneğini görürseniz ObjectDataSource s eşleşmiyor çünkü s etiketlemek `Update()` yönteme `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi. Geri dönüp bu yapılandırma değişikliği, düzenlemeyi etkinleştir seçeneği DetailsView s akıllı etiket görünmesi gereken yapmak için bir dakikanızı ayırın.


Bu yana `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi yalnızca dört parametre - kabul `supplierID`, `address`, `city`, ve `country` -DetailsView s BoundFields değiştirmek için `CompanyName` ve `Phone` BoundFields salt okunurdur. Ayrıca, kaldırma `SupplierID` BoundField değerlerinin. Son olarak, `AllSuppliersDataSource` ObjectDataSource şu anda sahip kendi `OldValuesParameterFormatString` özelliğini `original_{0}`. Bu özellik ayarı Tanımlayıcı Sözdizimi değerlerinin tümünü birlikte kaldırmak veya varsayılan değerine ayarlamak için bir dakikanızı ayırın `{0}`.

Yapılandırdıktan sonra `SupplierDetails` DetailsView ve `AllSuppliersDataSource` ObjectDataSource, biz aşağıdaki bildirim temelli biçimlendirme olacaktır:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Bu noktada DetailsView aracılığıyla belleğine alınabilen ve seçili tedarikçi s adres bilgilerini, yapılan seçim bağımsız olarak güncelleştirilebilir `Suppliers` DropDownList (bkz. Şekil 6).


[![Tüm Üreticiler bilgileri görüntüleyebilir ve adresini güncelleştirildi](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Şekil 6**: Any Üreticiler bilgileri görüntülenebilir ve ITS adresi güncelleştirildi ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>3. adım: Yalnızca seçili sağlayıcı s bilgilerini görüntüleme

Sayfamızı şu anda gelen belirli bir sağlayıcı olup seçilmiş bağımsız olarak tüm satıcılara ait bilgileri görüntüler `Suppliers` DropDownList. Yalnızca seçili sağlayıcı Tedarikçi bilgilerini görüntülemek için şu sayfamızı, belirli bir üretici ilgili bilgileri alır bir başka bir ObjectDataSource eklemeniz gerekir.

Adlandırma sayfasına yeni ObjectDataSource ekleme `SingleSupplierDataSource`. Kendi akıllı etiket yapılandırma veri kaynağı bağlantısını tıklayın ve sahip kullanmak `SuppliersBLL` s sınıfı `GetSupplierBySupplierID(supplierID)` yöntemi. İle `AllSuppliersDataSource` ObjectDataSource, sahip `SingleSupplierDataSource` ObjectDataSource s `Update()` yöntemi eşlenmiş `SuppliersBLL` s sınıfı `UpdateSupplierAddress` yöntemi.


[![SingleSupplierDataSource ObjectDataSource GetSupplierBySupplierID(supplierID) yöntemi kullanmak üzere yapılandırma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Şekil 7**: yapılandırmak `SingleSupplierDataSource` ObjectDataSource kullanılacak `GetSupplierBySupplierID(supplierID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Ardından, biz re parametre kaynağı belirtmesi istenir `GetSupplierBySupplierID(supplierID)` s yöntemi `supplierID` giriş parametresi. DropDownList, kullanım seçili Tedarikçi bilgilerini göstermek istiyoruz beri `Suppliers` DropDownList s `SelectedValue` özelliği parametre kaynağı olarak.


[![Üreticiler DropDownList SupplierID parametre kaynağı kullanın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Şekil 8**: kullanım `Suppliers` olarak DropDownList `supplierID` parametre kaynağı ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Eklenen bile bu ikinci ObjectDataSource ile DetailsView denetimi şu anda her zaman kullanmak üzere yapılandırılmış `AllSuppliersDataSource` ObjectDataSource. Bağlı olarak DetailsView tarafından kullanılan veri kaynağı ayarlamak için mantığı eklemek ihtiyacımız `Suppliers` DropDownList öğe seçti. Bunu başarmak için Oluştur bir `SelectedIndexChanged` Üreticiler DropDownList için olay işleyicisi. Bu, DropDownList Tasarımcısı'nda çift tıklatarak en kolay oluşturulabilir. Bu olay işleyicisi kullanmak için hangi veri kaynağı belirlemek gereken ve DetailsView verileri yeniden bağlamanız gerekir. Bu, aşağıdaki kod ile gerçekleştirilir:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Olay işleyicisi "Tüm tedarikçileri Göster/Düzenle" seçeneğini seçili olmadığını belirleyerek başlar. Olduysa, ayarlar `SupplierDetails` DetailsView s `DataSourceID` için `AllSuppliersDataSource` ve ayarlayarak kullanıcı Üreticiler kümesindeki ilk kaydı döndürür `PageIndex` özelliğinin 0. Ancak, kullanıcı belirli bir üretici DropDownList, DetailsView s seçtiyse, `DataSourceID` atandığı `SingleSuppliersDataSource`. Bağımsız olarak hangi veri kaynağı kullanılır, `SuppliersDetails` modu salt okunur moda geri döndürüldü ve veriler için bir çağrı tarafından DetailsView'a DataSet'e `SuppliersDetails` denetim s `DataBind()` yöntemi.

"Tüm tedarikçileri Göster/Düzenle" seçeneğini seçildi, bu durumda disk belleği arabirimi aracılığıyla sağlayıcıların tümünü görüntülenebilir sürece yerinde bu olay işleyicisi ile DetailsView denetimi seçili Tedarikçi şimdi gösterir. Şekil 9 sayfası "Göster/Düzenle tüm tedarikçileri" seçeneği belirlenmiş gösterir; disk belleği arabirimi varsa, ziyaret edin ve tüm tedarikçi güncelleştirmek kullanıcının unutmayın. Şekil 10 sayfa seçili Ma Ahmet tedarikçi ile gösterilir. Yalnızca Ma Ahmet s bilgileri, bu durumda görüntülenebilir ve düzenlenebilir.


[![Tüm Üreticiler bilgileri görüntülenebilir ve düzenlenebilir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Şekil 9**: tüm Üreticiler bilgileri görüntülenebilir ve düzenlenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Yalnızca seçili sağlayıcı s bilgileri görüntülenebilir ve düzenlenebilir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Şekil 10**: yalnızca seçili sağlayıcı s bilgi Viewed ve düzenlenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> Bu öğretici için DropDownList ve DetailsView s kontrol `EnableViewState` ayarlanmalıdır `true` (varsayılan) çünkü DropDownList s `SelectedIndex` ve DetailsView s `DataSourceID` Geri göndermeler arasında özellik s değişiklikleri anımsanacak.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>4. adım: bir düzenlenebilir GridView Üreticiler ürünlerinde listeleme

DetailsView tam bizim sonraki adım seçili sağlayıcı tarafından sağlanan bu ürünleri listeler düzenlenebilir bir GridView eklemektir. Bu GridView yalnızca düzenlemelere izin vermeli `ProductName` ve `QuantityPerUnit` alanları. Ayrıca, bu sayfasını ziyaret kullanıcı listesinden belirli bir tedarikçi ise, bu ürünleri için güncelleştirmeleri yalnızca sağlamalıdır *değil* devam etmez. İlk olarak bir aşırı yüklemesini eklemek için gerekir ki bunu gerçekleştirmek için `ProductsBLL` s sınıfı `UpdateProducts` yönteminin alan içinde yalnızca `ProductID`, `ProductName`, ve `QuantityPerUnit` alanlar girişleri olarak. Size bu süreçte, çok sayıda eğitimlerine önceden adım adım ve bu nedenle eklenmesi kod burada yalnızca bakmak s izin `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Oluşturulan, bu aşırı ile biz re GridView denetimini ve onun ilişkili ObjectDataSource eklemek için hazır. Sayfaya yeni GridView ekleyin, Ayarla kendi `ID` özelliğine `ProductsBySupplier`ve adlı yeni bir ObjectDataSource kullanacak şekilde yapılandırın `ProductsBySupplierDataSource`. Seçili sağlayıcı tarafından bu ürün listesi için bu GridView istiyoruz, kullanamadı `ProductsBLL` s sınıfı `GetProductsBySupplierID(supplierID)` yöntemi. Ayrıca eşleyebilir `Update()` yöntemi yeni `UpdateProduct` yeni oluşturduğumuz aşırı.


[![ObjectDataSource yeni oluşturduğunuz UpdateProduct aşırı kullanmak için yapılandırma](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `UpdateProduct` oluşturduğunuz aşırı yükleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Biz re parametre kaynağı seçmesi istenir `GetProductsBySupplierID(supplierID)` s yöntemi `supplierID` giriş parametresi. DetailsView, kullanım seçili sağlayıcı için ürünleri göstermek istiyoruz beri `SuppliersDetails` DetailsView denetimi s `SelectedValue` özelliği parametre kaynağı olarak.


[![SuppliersDetails DetailsView s SelectedValue özelliği parametre kaynağı olarak kullanın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Şekil 12**: kullanım `SuppliersDetails` DetailsView s `SelectedValue` özelliği parametre kaynağı olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


GridView için döndürerek, kaldırmak için dışında GridView alanların tümünü `ProductName`, `QuantityPerUnit`, ve `Discontinued`, işaretlenmesi `Discontinued` CheckBoxField salt okunur olarak. Ayrıca, GridView s akıllı etiket düzenlemeyi etkinleştir seçeneğini denetleyin. Bu değişiklikleri yaptıktan sonra bildirim temelli biçimlendirme GridView ve ObjectDataSource aşağıdakine benzer görünmelidir:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Bu s önceki bizim ObjectDataSources olduğu gibi `OldValuesParameterFormatString` özelliği ayarlanmış `original_{0}`, neden olacak sorunları ürün s adı veya birim başına miktar güncelleştirmeye çalışırken. Bu özellik tanımlayıcı sözdizimi tamamen kaldırın veya kendi varsayılan olarak ayarla `{0}`.

Bu yapılandırma tamamlandı, sayfamızı şimdi GridView seçili sağlayıcı tarafından sağlanan ürünleri listeler (bkz. Şekil 13). Şu anda *herhangi* ürün s adı veya birim başına miktar güncelleştirilebilir. Ancak, belirli bir sağlayıcı ile ilişkili kullanıcıları için devam etmeyen ürünleri için böyle işlevselliğini yasaklanmış sayfa mantığımızı güncelleştirmeye ihtiyacımız. Biz bu son parçası adım 5'te üstesinden gelmek.


[![Seçili sağlayıcı tarafından sağlanan ürünleri görüntülenir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Şekil 13**: seçili sağlayıcı tarafından sağlanan ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Bu düzenlenebilir GridView eklenmesi ile `Suppliers` DropDownList s `SelectedIndexChanged` olay işleyicisi GridView salt okunur bir duruma döndürmek için güncelleştirilmesi gerekir. Farklı bir sağlayıcı, ürün bilgilerini düzenleme sırada ortasında seçilirse, aksi takdirde, yeni tedarikçi GridView karşılık gelen dizininde de düzenlenebilir. Bunu önlemek için yalnızca GridView s ayarlamak `EditIndex` özelliğine `-1` içinde `SelectedIndexChanged` olay işleyicisi.


Ayrıca, s görünüm durumu olması GridView (varsayılan davranış) etkin önemli olduğunu hatırlayın. GridView s ayarlarsanız `EnableViewState` özelliğine `false`, yanlışlıkla silme veya düzenleme kayıtları eşzamanlı kullanıcı sahip riski. Bkz: [Uyarı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews Bu destek düzenleme ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx) daha fazla bilgi için.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>5. adım: Dönüştürülmesine ürünleri olduğunda göster/Düzenle tüm tedarikçileri seçili için düzenleme izin verme

Sırada `ProductsBySupplier` GridView tam olarak işlevsel olduğunu, şu anda çok fazla erişim listesinden belirli bir tedarikçi bu kullanıcılar için verir. Bizim iş kuralları tür kullanıcılar artık kullanımda olmayan ürünleri güncelleştirmek mümkün olmamalıdır. Bu zorlamak için biz gizle (devre dışı sayfa bir kullanıcı tarafından listesinden bir tedarikçi ziyaret edildiğinde devam etmeyen ürünlerle GridView satırları Düzenle düğmesini veya).

GridView s için bir olay işleyicisi oluşturun `RowDataBound` olay. Kullanıcı Bu öğretici için Üreticiler DropDownList s denetleyerek belirlenebilir, belirli bir sağlayıcı ile ilişkili olup olmadığını belirlemek ihtiyacımız bu olay işleyicisi `SelectedValue` özelliği - varsa onu s -1, sonra kullanıcı bir şey diğer belirli bir sağlayıcı ile ilişkili. Böyle kullanıcıları için biz sonra ürün kesilir olup olmadığını belirlemeniz gerekir. Biz gerçek başvuru yakalayın `ProductRow` örneği bağlı GridView satır `e.Row.DataItem` anlatıldığı gibi özelliği [ *özet bilgilerinde görüntüleme GridView s altbilgi* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) öğretici. Ürün kesilir, önceki öğreticide açıklanan teknikleri kullanarak CommandField GridView s Düzenle düğmesini programlı başvuru yakalayın [ *ekleme istemci-tarafı onay olduğunda silme* ](adding-client-side-confirmation-when-deleting-vb.md). Biz sonra gizleyebilir veya düğmesini devre dışı bir başvuru sahibiz sonra.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Bu sayfa bir kullanıcı olarak listesinden belirli bir tedarikçi üretilmeyen bu ürünlerden ziyaret olmadığında düzenlenebilir, bu olay ile işleyicisini yerde, bu ürünler için Düzenle düğmesini gizlenir. Örneğin, Chef Anton s Baharat karışımı New Orleans Cajun Delights tedarikçi için devam etmeyen bir üründür. Bu belirli tedarikçi için sitesini ziyaret ettiğinde bu ürün için Düzenle düğmesini görüş gizli (Şekil 14 bakın). Ancak, "Göster/Düzenle tüm tedarikçileri" kullanarak ziyaret Düzenle düğmesi kullanılabilir (bkz. Şekil 15) olur.


[![Üretici-belirli kullanıcılar için Chef Anton s Baharat karışımı için Düzenle düğmesini gizli](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Şekil 14**: tedarikçi özgü Chef Anton s Baharat karışımı için Düzenle düğmesini gizli kullanıcıları için ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Chef Anton s Baharat karışımı için Düzenle düğmesini göster/Düzenle tüm Üreticiler kullanıcılar için görüntülenir](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Şekil 15**: Göster/Düzenle tüm Üreticiler kullanıcıları için Baharat karışımı görüntülenir Chef Anton s için Düzenle düğmesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>İş mantığı katmanı erişim haklarını denetleme

Bu öğreticide ASP.NET sayfası kullanıcı görebileceğiniz hangi bilgi göre tüm mantığı işler ve hangi ürünleri kendisinin güncelleştirebilirsiniz. İdeal olarak, bu mantığı ayrıca iş mantığı katmanında mevcut olacaktır. Örneğin, `SuppliersBLL` s sınıfı `GetSuppliers()` tüm tedarikçileri döndürür) yöntemi (şu anda oturum açmış kullanıcı olduğundan emin olmak için bir onay içerebilir *değil* belirli bir sağlayıcı ile ilişkili. Benzer şekilde, `UpdateSupplierAddress` yöntemi ya da şirketimizin için çalışılan (ve dolayısıyla tüm Üreticiler adres bilgilerini güncelleştirebilirsiniz) şu anda oturum açmış kullanıcı emin olmak için bir onay içerebilir veya verisini güncelleştiriliyor tedarikçi ile ilişkilendirilir.

Bizim öğreticide BLL sınıfları erişemiyor sayfasında DropDownList s kullanıcı hakları saptanır çünkü bu tür BLL katman denetimleri içermiyordu. Üyelik sistemi ya da (Windows kimlik doğrulaması gibi), ASP.NET tarafından sağlanan genişletme-yepyeni kimlik doğrulama şemasını birini kullanırken, şu anda oturum açmış kullanıcı s ve rolleri bilgi böylece bu erişimin yapma BLL erişilebilir hakları sunu ve BLL Katmanlar olası denetler.

## <a name="summary"></a>Özet

Kullanıcı hesapları sağlar sitelerinin çoğu oturum açmış olan kullanıcının tabanlı veri değişikliği arabirimi özelleştirmeniz gerekir. Yönetici olmayan kullanıcıların yalnızca güncelleştirme veya kendilerini oluşturulan kayıtları silme sınırlı olabilir ancak silin ve herhangi bir kayıt düzenlemek yönetici kullanıcılar açabilir. Her senaryo, veriler Web denetimleri, ObjectDataSource, olabilir ve iş mantığı katmanı sınıfları eklemek veya oturum açan kullanıcıyı temel alarak belirli işlevleri reddetmek için genişletilebilir. Bu öğreticide kullanıcının belirli bir sağlayıcı ile ilişkili olmasına veya şirketimizin için çalışılan bağlı olarak görüntülenebilir ve düzenlenebilir verileri sınırlamak nasıl gördük.

Bu öğreticiyi ekleme, güncelleştirme ve GridView, DetailsView ve FormView denetimlerini kullanarak verileri silme bizim İnceleme sonlandırır. Sonraki öğretici ile başlayarak, biz disk belleği ve Destek sıralama eklemek için uygulamamızla kapatmanız.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](adding-client-side-confirmation-when-deleting-vb.md)
