---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Bildirim temelli parametreler (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide biz DetailsView denetiminde TemplateField görüntülenecek verileri seçmek için bir parametre için bir sabit kodlu değer kümesi kullanmak nasıl çalışılacağını.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: 01330d6c743fa9534cdb5dfa42bde5dbbe954c40
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839235"
---
<a name="declarative-parameters-vb"></a>Bildirim temelli parametreler (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) veya [PDF olarak indirin](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> Bu öğreticide biz DetailsView denetiminde TemplateField görüntülenecek verileri seçmek için bir parametre için bir sabit kodlu değer kümesi kullanmak nasıl çalışılacağını.


## <a name="introduction"></a>Giriş

İçinde [son öğretici](displaying-data-with-the-objectdatasource-vb.md) çağrılan bir ObjectDataSource denetimine bağlı GridView ve DetailsView FormView denetimleri ile verileri görüntüleme incelemiştik `GetProducts()` yönteminden `ProductsBLL` sınıfı. `GetProducts()` Yöntemi döndürür Northwind veritabanının kayıtların tümünün doldurulmuş kesin türü belirtilmiş DataTable `Products` tablo. `ProductsBLL` Sınıfı içeren ürün - döndüren yalnızca alt kümeleri için ek yöntemleri `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`, ve `GetProductsBySupplierID(supplierID)`. Bu üç yöntemi döndürülen ürün bilgileri nasıl filtreleme yapılacağını gösteren bir giriş parametresi bekler.

ObjectDataSource giriş parametrelerini beklediğiniz yöntemlerini çağırmak için ancak Biz bu parametrelerin değerleri nereden geldiğini belirtmelisiniz Bunu yapmak için kullanılabilir. Parametre değerleri, sabit kodlanmış olabilir veya bir çeşitli dahil olmak üzere dinamik kaynaklardan gelebilir: sorgu dizesi değerleri, oturum değişkenleri sayfasında veya başkalarının Web denetimin özellik değeri.

Bu öğretici için sabit kodlanmış bir değere ayarlanmış bir parametrenin nasıl kullanılacağını gösteren tarafından başlayalım. Özellikle, yani Chef Acı'nın Baharat olan karışımını, belirli bir ürün hakkındaki bilgileri gösteren sayfa bir DetailsView ekleme sırasında göz atacağız bir `ProductID` 5. Ardından, bir Web denetimine bağlı bir parametre yapmayı göreceğiz. Sonra bu ülkelerde ikamet sağlayıcıları listesini görmek için bir düğmeye tıklayın, bir ülkede yazın izin vermek için bir metin kutusu özellikle kullanacağız.

## <a name="using-a-hard-coded-parameter-value"></a>Bir parametre sabit kodlanmış değeri kullanarak

İlk örnekte, bir DetailsView denetimi için ekleyerek başlangıç `DeclarativeParams.aspx` sayfasını `BasicReporting` klasör. DetailsView'ın akıllı etiketten seçin &lt;yeni veri kaynağı&gt; açılır listeden listesi ve bir ObjectDataSource eklemek seçin.


[![Bir ObjectDataSource sayfasına ekleme](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Şekil 1**: bir ObjectDataSource sayfaya ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image3.png))


Bu, otomatik olarak ObjectDataSource denetim veri kaynağı Seç Sihirbazı'nı başlatır. Seçin `ProductsBLL` sihirbazının ilk ekranında sınıfı.


[![ProductsBLL sınıfı seçin](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Şekil 2**: seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image6.png))


Belirli bir ürün hakkındaki bilgileri görüntülemek istediğinden kullanmak istiyoruz `GetProductByProductID(productID)` yöntemi.


[![GetProductByProductID(productID) yöntemi](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Şekil 3**: seçin `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image9.png))


Bir parametreye seçtik yöntemi içerdiğinden, burada size parametresi için kullanılacak değeri tanımlar istenir Sihirbazı için bir daha fazla ekran yoktur. Sol taraftaki listenin tüm parametreler için seçilen yöntemi gösterir. İçin `GetProductByProductID(productID)` yalnızca bir tane `productID`. Sağ tarafta biz seçili parametresi için değer belirtebilirsiniz. Çeşitli olası kaynakları için parametre değeri parametre kaynak aşağı açılan listesi numaralandırır. Bir sabit kodlanmış değeri için 5 belirtmek istediğinden `productID` parametresi, parametre kaynağı yok olarak bırakın ve 5 DefaultValue metin kutusuna girin.


[![Bir Hard-Coded parametre değeri, 5 için kullanılacak parametre ProductID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Şekil 4**: A Hard-Coded parametre değeri, 5 için kullanılacak `productID` parametre ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image12.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra bildirim temelli biçimlendirme ObjectDataSource denetim içeren bir `Parameter` nesnesine `SelectParameters` koleksiyon içinde tanımlanan yöntem tarafından beklenen girdi parametrelerinin her `SelectMethod` özellik. Bu örnekte kullanıyoruz yöntemi yalnızca tek bir giriş parametre, bekliyor beri `parameterID`, burada yalnızca bir giriş yok. `SelectParameters` Koleksiyonu türetilen herhangi bir sınıf içeren `Parameter` sınıfını `System.Web.UI.WebControls` ad alanı. Temel parametresi sabit kodlanmış değerler için `Parameter` sınıfı kullanılır, ancak diğer parametresi için kaynak türetilmiş seçenekleri `Parameter` sınıfı kullanılır; kendi oluşturabilirsiniz [özel parametre türleri](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), gerekirse.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Bu noktada, kendi bilgisayarınızda bildirim temelli biçimlendirme izliyorsanız değerler için Mayıs görürsünüz `InsertMethod`, `UpdateMethod`, ve `DeleteMethod` özellikleri yanı `DeleteParameters`. ObjectDataSource veri kaynağı Seç Sihirbazı otomatik olarak yöntemlerinden belirtir `ProductBLL` böyle açıkça temizlenmiş sürece, bunlar yukarıdaki biçimlendirme dahil şekilde ekleme, güncelleştirme ve silme için kullanılacak.


Bu sayfayı ziyaret ederken Web denetimi veri ObjectDataSource çağıracağı `Select` yöntemini çağıracak `ProductsBLL` sınıfın `GetProductByProductID(productID)` 5 için sabit kodlanmış değeri kullanarak yöntemini `productID` giriş parametresi. Türü kesin belirlenmiş bir metodun döndüreceği `ProductDataTable` Chef Acı'nın Baharat karışımı hakkında bilgi içeren tek bir satır içeren nesne (ürünle `ProductID` 5).


[![Bilgi hakkında Chef Acı 's Baharat karışımı görüntülenir](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Şekil 5**: bilgi hakkında Chef Acı 's Baharat karışımı görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image15.png))


## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Parametre değeri Web denetimi özelliği değerini ayarlama

Sayfada Web denetim değerini temel ObjectDataSource parametre değerlerini de ayarlayabilirsiniz. Bunu göstermek için şimdi kullanıcı tarafından belirtilen bir ülkede bulunan sağlayıcıların tümünü listeleyen GridView sahip. Bu başlangıç sayfasına ülke adı kullanıcının girebileceği metin kutusu ekleyerek gerçekleştirmek için. Bu metin kutusu denetiminin ayarlamak `ID` özelliğini `CountryName`. Ayrıca, bir düğme Web denetimi ekleyin.


[![Bir metin kutusu Sayfa kimliği ülke adı ile ekleyin](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Şekil 6**: sayfası bir metin kutusu ekleme `ID` `CountryName` ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image18.png))


Ardından, yeni ObjectDataSource eklemek için GridView sayfası, gelen ve akıllı etiket ekleme seçin. Sağlayıcı bilgi Seç görüntülemek istiyoruz beri `SuppliersBLL` sihirbazın ilk ekranında bir sınıftan. İkinci ekranından çekme `GetSuppliersByCountry(country)` yöntemi.


[![GetSuppliersByCountry(country) yöntemi](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Şekil 7**: seçin `GetSuppliersByCountry(country)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image21.png))


Bu yana `GetSuppliersByCountry(country)` yöntemi giriş parametresi vardır, Sihirbazı bir kez daha parametre değeri seçmeye yönelik bir son ekran içerir. Bu süre parametresini kaynak denetim olarak ayarlayın. Bu sayfadaki denetimleri adlarıyla ControlId açılır listede doldurulur; seçin `CountryName` liste denetiminden. Ne zaman sayfa ilk ziyaret edildiğinde `CountryName` hiçbir şey görüntülenmez ve hiçbir sonuç döndürmedi metin kutusu boş olacaktır. DefaultValue metin kutusu, varsayılan olarak bazı sonuçları görüntülemek istiyorsanız, uygun şekilde ayarlayın.


[![Bir parametre adı: denetim değerine ayarlayın.](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Şekil 8**: parametre değerine `CountryName` denetim değerini ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image24.png))


ObjectDataSource bildirim temelli biçimlendirme ilk örneğimizde biraz farklıdır kullanarak bir [ControlParameter'da](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) standart yerine `Parameter` nesne. A `ControlParameter` belirtmek için ek özelliklere sahip `ID` Web denetimi ve özellik değeri'parametresi için kullanmayı (`PropertyName`). Veri Kaynağı Yapılandırma Sihirbazı'nı bir metin kutusu için biz büyük olasılıkla kullanmak istersiniz, belirlemek akıllı `Text` özelliği için parametre değeri. Ancak, Web denetiminden farklı özellik değerini kullanmak istiyorsanız, değiştirebileceğiniz `PropertyName` burada veya Sihirbazı'ndaki "Gelişmiş özellikleri göster" bağlantısına tıklayarak değeri.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Sayfa ilk defa ziyaret edildiğinde `CountryName` metin kutusu boştur. ObjectDataSource `Select` GridView ancak değerini metodunu çağırmak hala `Nothing` yöntemlere geçirilen `GetSuppliersByCountry(country)` yöntemi. TableAdapter dönüştürür `Nothing` veritabanına `NULL` değeri (`DBNull.Value`), ancak sorgu tarafından kullanılan `GetSuppliersByCountry(country)` yöntemi herhangi döndürmüyor şekilde yazılır, bu değerler bir `NULL` değer içinbelirtilen`@CategoryID`parametresi. Kısacası, yok sağlayıcıları döndürülür.

Ziyaretçi bir ülkede ancak girer ve bir geri gönderme ObjectDataSource's neden tedarikçileri Göster düğmesine tıklar sonra `Select` yöntemi yeniden, metin kutusu denetiminin geçirme `Text` olarak değer `country` parametresi.


[![Bu sağlayıcılardan Kanada gösterilir](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Şekil 9**: Bu Kanada sağlayıcıdan gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image27.png))


## <a name="showing-all-suppliers-by-default"></a>Varsayılan olarak tüm Üreticiler gösteriliyor

Bunun yerine yok sağlayıcıları ilk sayfayı görüntülerken Göster daha göstermek istiyoruz *tüm* tedarikçileri ülke adı metin kutusuna girerek listede aşağı küçültmek kullanıcının öncelikle,. Metin kutusu boş olduğunda `SuppliersBLL` sınıfın `GetSuppliersByCountry(country)` yöntemi geçirilir `Nothing` için kendi *`country`* giriş parametresi. Bu `Nothing` değeri sonra geçirilen DAL ait `GetSupplierByCountry(country)` yöntemi, burada da çevrilir veritabanına `NULL` değerini `@Country` aşağıdaki sorgu parametresi:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

İfade `Country = NULL` her zaman yanlış, hatta kayıtları için ayarlanmış döndürür `Country` sütununun bir `NULL` değeri; bu nedenle, hiçbir kayıt döndürülür.

Döndürülecek *tüm* Tedarikçiler ülke metin kutusu boş olduğunda, biz büyütmek `GetSuppliersByCountry(country)` BLL çağrılacak yöntem `GetSuppliers()` , ülke parametresinin yöntemi `Nothing` ve DAL's çağırmaya `GetSuppliersByCountry(country)` yöntemi, aksi takdirde. Bu, ülke parametresi eklendiğinde, hiçbir ülke belirtildiğinde tüm üreticiler ve tedarikçileri uygun alt kümesi döndüren bir etkisi olmaz.

Değişiklik `GetSuppliersByCountry(country)` yönteminde `SuppliersBLL` aşağıdaki sınıfı:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Bu değişikliğe `DeclarativeParams.aspx` sayfa ilk ziyaret edildiğinde sağlayıcıların tümünü gösterir (veya herhangi bir zamanda `CountryName` metin kutusu boşsa).


[![Artık gösterilen varsayılan olarak tüm tedarikçilerin olduğunu](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Şekil 10**: tüm tedarikçilerin olduğunu artık varsayılan olarak gösterilen ([tam boyutlu görüntüyü görmek için tıklatın](declarative-parameters-vb/_static/image30.png))


## <a name="summary"></a>Özet

Giriş parametreleriyle yöntemleri kullanmak için ObjectDataSource içinde parametrelerinin değerlerini belirtmek ihtiyacımız `SelectParameters` koleksiyonu. Farklı türde parametreler parametre değeri farklı kaynaklardan izin verir. Sabit kodlanmış bir değeri varsayılan parametre türü kullanır, ancak bir kolayca (ve bir kod satırı olmadan) gibi parametre değerlerini sorgu dizesi, oturum değişkenleri, tanımlama bilgileri ve Web denetimleri sayfasında bile kullanıcı tarafından girilen değerler elde edilebilir.

Bu öğreticide inceledik örnekler bildirim temelli parametre değerlerini kullanma gösterilmektedir. Ancak, geçerli tarih ve saat gibi kullanılabilir değil, bir parametre kaynağıyla kullanmak için gerektiğinde bir kez veya üç sitemizi üyelik, ziyaretçi kullanıcı Kimliğini kullanıyorsanız olabilir. Bu tür senaryolar için biz, temel alınan nesnenin yöntemini çağıran program aracılığıyla önce ObjectDataSource parametre değerlerini ayarlayabilirsiniz. Bunu gerçekleştirmek nasıl görüyoruz [sonraki öğreticiye](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Hilton Giesenow oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](displaying-data-with-the-objectdatasource-vb.md)
> [İleri](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
