---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Bildirim temelli parametreleri (VB) | Microsoft Docs
author: rick-anderson
description: "Bu öğreticide biz parametresi sabit kodlanmış bir değerine ayarlanmış bir DetailsView denetiminde görüntülenecek verileri seçmek için nasıl kullanılacağını gösteren."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: ea1aed2b76eb4196196f8a800c0bdb891bceda91
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="declarative-parameters-vb"></a>Bildirim temelli parametreleri (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) veya [PDF indirin](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Bu öğreticide biz parametresi sabit kodlanmış bir değerine ayarlanmış bir DetailsView denetiminde görüntülenecek verileri seçmek için nasıl kullanılacağını gösteren.


## <a name="introduction"></a>Giriş

İçinde [son öğretici](displaying-data-with-the-objectdatasource-vb.md) çağrılan bir ObjectDataSource denetimine bağlı GridView, DetailsView ve FormView denetimleri ile verileri görüntüleme Aranan `GetProducts()` yönteminden `ProductsBLL` sınıfı. `GetProducts()` Yöntemi döndürür tüm Northwind veritabanı kayıtlarını doldurulan kesin türü belirtilmiş bir DataTable `Products` tablo. `ProductsBLL` Sınıfı içerir ürünlerin - döndürmeyi yalnızca alt kümeleri için ek yöntemleri `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, ve `GetProductsBySupplierID(supplierID)`. Bu üç yöntem döndürülen ürün bilgileri filtrelemek nasıl gösteren bir giriş parametresi bekler.

ObjectDataSource giriş parametreleri beklediğiniz yöntemleri çağırmak için ancak Biz bu parametreler için değerler alınacağı yeri belirtmelisiniz Bunu yapmak için kullanılabilir. Parametre değerleri sabit kodlanmış olabilir veya bir çeşitli dahil olmak üzere dinamik kaynaklardan gelebilir: sorgu dizesi değerleri, oturum değişkenleri, bir Web denetimi sayfasında veya başkalarının özellik değeri.

Bu öğretici için sabit kodlanmış bir değere ayarlayın parametresinin nasıl kullanılacağını gösteren tarafından başlayalım. Biz öğesine Cennet Baharat olan karışımı, belirli bir ürün bilgilerini görüntüler sayfaya bir DetailsView ekleme özellikle de göreceğiz bir `ProductID` 5. Ardından, bir Web denetimine bağlı bir parametre ayarlama göreceğiz. Özellikle, daha sonra bu ülkelerde ikamet Üreticiler listesini görmek için düğmesini tıklatın, bir ülkede yazın kullanıcı izin vermek için bir metin kutusu kullanacağız.

## <a name="using-a-hard-coded-parameter-value"></a>Bir sabit kodlanmış parametre değeri kullanma

İlk örnek için bir denetimini ekleyerek başlayın `DeclarativeParams.aspx` sayfasındaki `BasicReporting` klasör. DetailsView'un akıllı etiketten seçin &lt;yeni veri kaynağı&gt; açılan dan listesinde ve bir ObjectDataSource eklemek için seçin.


[![Bir ObjectDataSource sayfasına ekleme](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Şekil 1**: bir ObjectDataSource sayfasına ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image3.png))


Bu, otomatik olarak ObjectDataSource denetimin veri kaynağı Seç Sihirbazı'nı başlatır. Seçin `ProductsBLL` Sihirbazı'nın ilk ekranından sınıfı.


[![ProductsBLL sınıfı seçin](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Şekil 2**: seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image6.png))


Belirli bir ürünü hakkındaki bilgileri görüntülemek istediğinden kullanmak istiyoruz `GetProductByProductID(productID)` yöntemi.


[![GetProductByProductID(productID) yöntemini seçin.](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Şekil 3**: seçin `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image9.png))


Bir parametre seçtik yöntemi içerir olduğundan, burada biz parametresi için kullanılacak değeri tanımlamak istenir sihirbazın bir daha fazla ekran yoktur. Soldaki listeden seçilen yöntemi için parametreler tümünün gösterir. İçin `GetProductByProductID(productID)` yalnızca bir tane `productID`. Sağ tarafta biz seçilen parametre için değer belirtebilirsiniz. Çeşitli olası kaynakları parametre değeri parametre kaynak açılan listesini numaralandırır. 5 için sabit kodlanmış bir değerini belirtmek istediğinden `productID` parametre, parametre kaynağı hiçbiri olarak bırakın ve 5 DefaultValue metin kutusuna girin.


[![Bir Hard-Coded parametre değeri, 5 için kullanılacak parametre ProductID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Şekil 4**: A Hard-Coded parametre değeri, 5 için kullanılacak `productID` parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image12.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra ObjectDataSource denetimin bildirim temelli biçimlendirme içeren bir `Parameter` nesnesinde `SelectParameters` her tanımlanan yöntemi tarafından beklenen giriş parametreleri için koleksiyon `SelectMethod` özellik. Biz bu örnekte, kullanmakta olduğunuz yöntemi yalnızca bir tek giriş parametre bekler beri `parameterID`, burada yalnızca bir giriş yok. `SelectParameters` Koleksiyonu türetilen herhangi bir sınıf içerebilir `Parameter` sınıfını `System.Web.UI.WebControls` ad alanı. Temel sabit kodlanmış parametre değerleri için `Parameter` sınıfı kullanılır, ancak kaynak için başka bir parametre türetilmiş seçenekleri `Parameter` sınıfı kullanılır; Ayrıca kendi oluşturabilirsiniz [özel parametre türleri](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11)gerekirse.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Kendi bilgisayarınızda bildirim temelli biçimlendirme izlemekte, bu noktada için değerler arasında may olup `InsertMethod`, `UpdateMethod`, ve `DeleteMethod` özellikleri yanı `DeleteParameters`. ObjectDataSource'nın veri kaynağı Seç Sihirbazı otomatik olarak yöntemleri belirtir `ProductBLL` olanlar çıkışı açıkça temizlenmiş sürece, bunlar yukarıdaki biçimlendirme dahil böylece ekleme, güncelleştirme ve silme için kullanılacak.


Bu sayfayı ziyaret ederken Web denetimi veri ObjectDataSource's çağıracağı `Select` çağıracak yöntemi `ProductsBLL` sınıfının `GetProductByProductID(productID)` 5 için sabit kodlanmış değeri kullanılarak yöntemi `productID` giriş parametresi. Kesin türü belirtilmiş bir yöntem döndürülecek `ProductDataTable` Cennet Baharat karışımı hakkında bilgi içeren tek bir satır içeren nesne (ürünle `ProductID` 5).


[![Bilgi hakkında Chef Cennet Baharat karışımı görüntülenir](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Şekil 5**: bilgi hakkında Chef Cennet Baharat karışımı görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Parametre değeri bir Web denetimi özellik değerini ayarlama

ObjectDataSource'nın parametre değerleri de ayarlayabilirsiniz sayfasında Web denetimi değere göre. Bunu göstermek için şimdi tüm kullanıcı tarafından belirtilen bir ülkede bulunan sağlayıcıların listeleyen GridView sahip. Kullanıcı bir ülke adı girebileceği sayfasına metin kutusu ekleyerek bu başlangıç gerçekleştirmek için. Bu metin kutusu denetiminin ayarlamak `ID` özelliğine `CountryName`. Ayrıca bir düğme Web denetimi ekleyin.


[![Metin kutusu Sayfa kimliği adı: ile ekleyin](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Şekil 6**: sayfasıyla metin kutusu ekleyin `ID` `CountryName` ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image18.png))


Ardından, yeni ObjectDataSource eklemek için GridView sayfası, gelen ve akıllı etiket ekleme seçin. Biz tedarikçi bilgi seçin görüntülemek istediğiniz beri `SuppliersBLL` sihirbazın ilk ekranından sınıfı. İkinci ekranından çekme `GetSuppliersByCountry(country)` yöntemi.


[![GetSuppliersByCountry(country) yöntemini seçin.](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Şekil 7**: seçin `GetSuppliersByCountry(country)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image21.png))


Bu yana `GetSuppliersByCountry(country)` yöntemi giriş parametresi vardır, Sihirbazı bir kez daha parametre değeri seçmeye yönelik bir son ekran içerir. Bu süre, parametre kaynak denetimine ayarlayın. Bu sayfadaki denetimlerin adlarıyla ControlId aşağı açılan liste doldurulur; seçin `CountryName` listeden denetim. Ne zaman ilk sayfası `CountryName` hiç sonuç döndürmedi ve hiçbir şey görünmez metin kutusu boş olacaktır. Varsayılan olarak bazı sonuçları görüntülemek istiyorsanız, DefaultValue textbox uygun şekilde ayarlayın.


[![Bir parametre adı: denetim değerine ayarlayın](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Şekil 8**: parametre değeri ayarlamak `CountryName` denetim değeri ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image24.png))


ObjectDataSource'nın bildirim temelli biçimlendirme ilk örneğimizde biraz farklıdır kullanarak bir [ControlParameter'da](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) standart yerine `Parameter` nesnesi. A `ControlParameter` belirtmek için ek özellikler sahip `ID` Web denetimi ve parametre için kullanılacak özellik değeri (`PropertyName`). Veri Kaynağı Yapılandırma Sihirbazı'nı bir metin kutusu biz büyük olasılıkla kullanmak istersiniz, belirlemek akıllı `Text` özelliği parametre değeri. Ancak, Web denetiminden farklı özellik değerini kullanmak istiyorsanız, değiştirebileceğiniz `PropertyName` burada veya Sihirbazı'ndaki "Göster Gelişmiş Özellikler" bağlantısına tıklayarak değer.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Sayfa ilk kez ziyaret eden `CountryName` metin kutusu boştur. ObjectDataSource's `Select` yöntemi GridView, ancak değeri tarafından hala çağrılan `Nothing` içine geçirilen `GetSuppliersByCountry(country)` yöntemi. TableAdapter dönüştürür `Nothing` bir veritabanı içine `NULL` değeri (`DBNull.Value`), ancak tarafından kullanılan sorguyu `GetSuppliersByCountry(country)` yöntemi herhangi döndürmez şekilde yazılır, bu değerler bir `NULL` değeri içinbelirtilen`@CategoryID`parametresi. Kısacası, hiçbir Üreticiler döndürülür.

Ziyaretçi bir ülkede ancak girer ve bir geri gönderme ObjectDataSource's neden Üreticiler Göster düğmesini tıklattığında sonra `Select` yöntemi yeniden, metin kutusu denetiminin geçirme `Text` olarak değer `country` parametresi.


[![Bu sağlayıcıdan Kanada gösterilir](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Şekil 9**: olanlar Kanada sağlayıcıdan gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Varsayılan olarak tüm tedarikçileri gösterme

Bunun yerine tedarikçileri hiçbiri ilk sayfa görüntülerken Göster daha göstermek istiyoruz *tüm* ilk, metin kutusuna bir ülke adı girerek listede aşağı Karşılaştır arkasından üreticiler. Metin kutusu boş olduğunda `SuppliersBLL` sınıfının `GetSuppliersByCountry(country)` yöntemi geçirilir `Nothing` için kendi  *`country`*  giriş parametresi. Bu `Nothing` değeri sonra geçirilir DAL ait `GetSupplierByCountry(country)` yöntemi, burada da çevrilir veritabanına `NULL` değerini `@Country` aşağıdaki sorgu parametresinde:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

İfade `Country = NULL` her zaman yanlış bile kayıtlarını özelliği döndürür `Country` sütununda bir `NULL` değeri; bu nedenle, hiçbir kayıt döndürdü.

Döndürülecek *tüm* ülke metin kutusu boş olduğunda Üreticiler ki büyütmek `GetSuppliersByCountry(country)` çağrılacak BLL yönteminde `GetSuppliers()` , ülke parametresinin yöntemi `Nothing` ve DAL's çağırmak için `GetSuppliersByCountry(country)` yöntemi, aksi takdirde. Bu ülke parametresi dahil edildiğinde, hiçbir ülke belirtildiğinde tüm tedarikçileri ve uygun bir alt kümesiyle Üreticiler döndüren etkisi olmaz.

Değişiklik `GetSuppliersByCountry(country)` yönteminde `SuppliersBLL` şu sınıfı:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Bu değişikliği `DeclarativeParams.aspx` sayfa ilk sitesini ziyaret ettiğinizde sağlayıcıların tümünü gösterir (veya her `CountryName` metin kutusu boş).


[![Şimdi gösterilen varsayılan olarak tüm tedarikçilerin olduğunu](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Şekil 10**: tüm tedarikçilerin olduğunu şimdi gösterilen varsayılan olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Özet

Giriş parametreleriyle yöntemlerini kullanabilmeniz için parametreler için değerler ObjectDataSource içinde 's belirtmek ihtiyacımız `SelectParameters` koleksiyonu. Farklı kaynaklardan gelen parametre değeri parametre farklı türlerini sağlar. Varsayılan parametre türü bir sabit kodlu değer kullanır, ancak kolayca (ve bir kod satırı olmadan) gibi parametre değerlerini querystring, oturum değişkenleri, tanımlama bilgilerini ve Web denetimleri sayfasında bile kullanıcı tarafından girilen değerlerinden elde edilebilir.

Bu öğreticide inceledik örnekler bildirim temelli parametre değerleri kullanmak nasıl gösterilmiştir. Ancak, zaman zaman geçerli tarih ve saat gibi kullanılabilir olmayan bir parametre kaynağı kullanması için ihtiyacımız olması veya, sitemizi üyeliği, ziyaretçi kullanıcı Kimliğini kullanıyorsanız olabilir. Gibi senaryolar için temel alınan nesnenin yönteminin çağrılması biz program aracılığıyla ObjectDataSource önce parametre değerlerini ayarlayabilirsiniz. Bunu gerçekleştirmek nasıl göreceğiz [sonraki öğretici](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](displaying-data-with-the-objectdatasource-vb.md)
[sonraki](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
