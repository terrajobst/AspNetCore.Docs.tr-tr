---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Ekleme, güncelleştirme ve SqlDataSource (C#) ile verileri silme | Microsoft Docs
author: rick-anderson
description: Önceki eğitimlerine nasıl ObjectDataSource denetimine ekleme, güncelleştirme ve verileri silme izin öğrendiniz. SqlDataSource denetimi t destekliyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 25dab0292aefa183a1abc2615a7ba8e7a512346d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Ekleme, güncelleştirme ve SqlDataSource (C#) ile verileri silme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) veya [PDF indirin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Önceki eğitimlerine nasıl ObjectDataSource denetimine ekleme, güncelleştirme ve verileri silme izin öğrendiniz. SqlDataSource denetimi aynı işlemleri destekler ancak farklı bir yaklaşımdır ve bu öğreticinin eklemek, güncelleştirmek ve verileri silmek için SqlDataSource yapılandırma gösterir.


## <a name="introduction"></a>Giriş

' Da anlatıldığı gibi [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), yerleşik güncelleştirme GridView denetimi sağlar ve DetailsView ve FormView denetimleri ekleme dahil ederken silme özellikleri, destek ile birlikte düzenleme ve silme işlevselliği. Bu veri değişikliği özellikleri veri kaynağı denetimi yazılması gereken kod satırı olmadan doğrudan takılabilir. [Bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) ekleme, güncelleştirme ve silme ile GridView, DetailsView ve FormView denetimleri kolaylaştırmak için ObjectDataSource kullanılarak incelenmesi. Alternatif olarak, SqlDataSource ObjectDataSource yerine kullanılabilir.

Ekleme, güncelleştirme ve silme, biz Ekle gerçekleştirmek için çağrılacak nesne katmanı yöntemlerini belirtmek için güncelleştirme veya silme eylemi ObjectDataSource ile desteklemek için geri çağırma. Sağlamak ihtiyacımız SqlDataSource ile `INSERT`, `UPDATE`, ve `DELETE` SQL deyimleri (veya saklı yordamlar) yürütmek için. Bu öğreticide anlatıldığı gibi bu deyimleri el ile oluşturulabilir veya SqlDataSource s veri kaynağı Yapılandırma Sihirbazı tarafından otomatik olarak oluşturulabilir.

> [!NOTE]
> Biz bu yana ullanıcı zaten ekleme, düzenleme ve GridView, DetailsView, yeteneklerini silme ele alınan ve FormView denetimleri, Bu öğretici işlemlerini desteklemek için SqlDataSource denetimi yapılandırma hakkında odaklanır. Bu özellikler düzenleme, ekleme ve silme veri öğreticileri GridView, DetailsView ve FormView dönün içinde uygulanmasına tazelemek gerekiyorsa, başlayarak [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>1. adım: Belirtme`INSERT`,`UPDATE`, ve`DELETE`deyimleri

Biz için ihtiyacımız SqlDataSource denetimden veri almak için son iki eğitimlerine görülen ve iki özellikleri ayarlayın:

1. `ConnectionString`, belirten ne sorguya göndermek için veritabanı ve
2. `SelectCommand`, geçici SQL deyimi veya sonuçları döndürmek için saklı yordam adı belirtir.

İçin `SelectCommand` parametrelerle değerleri, değerler SqlDataSource s belirtilen parametre `SelectParameters` koleksiyonu ve sabit kodlanmış değerler ortak parametre kaynak içerebilir (querystring alanları, oturum değişkenleri Web denetimi değerlerini ve vb.) veya programlı olarak atanabilir. SqlDataSource denetlemek s `Select()` yöntemi verilerden Web denetimi çağrılan program aracılığıyla veya otomatik olarak veritabanına bir bağlantı oluşturulur, parametre değerlerini sorguya atanır ve için Kapat komutunu shuttled Veritabanı. Sonuçlar daha sonra bir veri kümesi veya DataReader, olarak s denetiminin değerine bağlı olarak döndürülür `DataSourceMode` özelliği.

Verileri seçme birlikte SqlDataSource denetimi ekle, Güncelleştir ve sağlayarak verilerini silmek için kullanılabilir `INSERT`, `UPDATE`, ve `DELETE` benzer şekilde SQL deyimlerinde. Yalnızca Ata `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri `INSERT`, `UPDATE`, ve `DELETE` SQL deyimlerini yürütmek için. (Her zaman en göründükleri gibi) deyimleri parametreler varsa, içine dahil `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları.

Bir kez bir `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` değeri belirtilen, karşılık gelen verilerin Web denetimi s akıllı etiket etkinleştirmek ekleme, düzenlemeyi etkinleştir veya etkinleştirmek silme seçeneğinde kullanılabilir hale gelecektir. Let s bunu göstermek için bir örnek uygulamanız `Querying.aspx` oluşturduğumuz içinde sayfa [SqlDataSource denetimi sorgulama verilerle](querying-data-with-the-sqldatasource-control-cs.md) öğretici ve ayırma özellikleri içerecek şekilde silin.

Başlangıç açarak `InsertUpdateDelete.aspx` ve `Querying.aspx` gelen sayfaları `SqlDataSource` klasör. Üzerinde Tasarımcısından `Querying.aspx` sayfasında, ilk örnekte SqlDataSource ve GridView seçin ( `ProductsDataSource` ve `GridView1` denetimleri). İki denetim seçtikten sonra düzenleme menüsüne gidin ve Kopyala'yı seçin (veya Ctrl + C yalnızca isabet). Ardından, tasarımcısına Git `InsertUpdateDelete.aspx` denetimlerinde yapıştırın. İki denetimleri için taşıdığınız sonra `InsertUpdateDelete.aspx`, test sayfasını bir tarayıcıda çıkışı. Değerlerini görmelisiniz `ProductID`, `ProductName`, ve `UnitPrice` tüm kayıtları için sütunları `Products` veritabanı tablosu.


[![Tüm ürünleri, ProductID tarafından sıralanan listelenir](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Şekil 1**: tüm ürünleri, göre sıralanmış listelenen `ProductID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>SqlDataSource s ekleme`DeleteCommand`ve`DeleteParameters`özellikleri

Bu noktada yalnızca tüm kayıtları döndürür SqlDataSource sahibiz `Products` tablo ve bu verileri işleyen GridView. Amacımız GridView aracılığıyla ürünleri silme kullanıcıya izin vermek için bu örneği genişletmek için ' dir. İhtiyacımız SqlDataSource denetimi s değerlerini belirtmek için bunu gerçekleştirmek için `DeleteCommand` ve `DeleteParameters` özellikleri ve GridView silme destekleyecek şekilde yapılandırın.

`DeleteCommand` Ve `DeleteParameters` çeşitli yollarla özellikler belirtilebilir:

- Tanımlayıcı Sözdizimi
- Tasarımcıda Özellikler penceresinden
- Özel bir SQL ifadesi belirtin veya saklı yordam ekranından veri kaynağı Yapılandırma Sihirbazı
- Veri Kaynağı Yapılandırma Sihirbazı'nda Görünüm ekranın bir tablodan belirt sütunlardaki Gelişmiş düğmesi hangi gerçekte otomatik olarak oluşturacağını `DELETE` kullanılan SQL deyimi ve parametre koleksiyonu `DeleteCommand` ve `DeleteParameters` özellikleri

Biz nasıl otomatik olarak inceleyeceğiz `DELETE` 2. adımda oluşturulan deyimini. Veri Kaynağı Yapılandırma Sihirbazı'nı veya tanımlayıcı sözdizimi seçeneği da çalışır ancak şu an için Tasarımcısı'nda Özellikler penceresini kullanın s olanak tanır.

Tasarımcıda gelen `InsertUpdateDelete.aspx`, tıklayın `ProductsDataSource` SqlDataSource ve Özellikler penceresini açın (Görünüm menüsünden Özellikler penceresi seçin veya yalnızca ulaştı. F4). Üç nokta ayarlama getirecek DeleteQuery özelliğini seçin.


![Özellikler penceresinden DeleteQuery özelliği seçin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Şekil 2**: Özellikler penceresinden DeleteQuery özelliği seçin


> [!NOTE]
> SqlDataSource içermiyor t DeleteQuery özelliğe sahip. Bunun yerine, DeleteQuery birleşimidir `DeleteCommand` ve `DeleteParameters` özellikleri ve Özellikler penceresinde Tasarımcı penceresinden görüntülerken yalnızca listelenir. Kaynak Görünümü Özellikleri penceresinde bakıyorsanız bulacaksınız `DeleteCommand` özelliği yerine.


Komut ve parametre Düzenleyicisi iletişim kutusunu açmak için DeleteQuery özelliğinde üç nokta (bkz. Şekil 3) kutusuna tıklayın. Bu iletişim kutusunda belirttiğiniz `DELETE` SQL deyimini ve parametreleri belirtin. Aşağıdaki sorguyu girin `DELETE` komutu textbox (ya da el ile veya tercih ederseniz Sorgu Oluşturucusu'nu kullanarak):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Ardından, eklemek için parametreleri Yenile düğmesini tıklatın `@ProductID` aşağıdaki parametrelerin listesi için parametre.


[![Özellikler penceresinden DeleteQuery özelliği seçin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Şekil 3**: Özellikler penceresinden DeleteQuery özelliğini seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Yapmak *değil* Bu parametre (bırakın, parametresinin None kaynağı) için bir değer girin. Silme desteği için GridView eklediğinizde GridView otomatik olarak bu parametre değeri kullanılarak sağlayacak kendi `DataKeys` , Sil düğmesine tıklanana satır için koleksiyonu.

> [!NOTE]
> Kullanılan parametre adı `DELETE` sorgu *gerekir* adı ile aynı `DataKeyNames` GridView, DetailsView ya da FormView değeri. Diğer bir deyişle, parametresinde `DELETE` deyimi adlı var `@ProductID` (yerine, söyleyin, `@ID`), Ürünler tablosuna (ve bu nedenle GridView DataKeyNames değerinde) birincil anahtar sütunu adı olduğundan `ProductID`.


Parametre adı ve `DataKeyNames` değeri içermiyor t eşleşme GridView otomatik olarak atayamazsınız parametre değerini `DataKeys` koleksiyonu.

Komut ve parametre Düzenleyicisi iletişim kutusuna delete ilgili bilgileri girdikten sonra Tamam'ı tıklatın ve sonuçta elde edilen bildirim temelli biçimlendirme incelemek için kaynak görünümüne gidin:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Eklenmesi Not `DeleteCommand` özelliği yanı sıra `<DeleteParameters>` bölümü ve adlı parametre nesne `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>GridView silmek için yapılandırma

İle `DeleteCommand` özelliği eklendi, GridView s akıllı etiket şimdi etkinleştirmek silme seçeneği içerir. Devam etmek ve bu onay kutusunu işaretleyin. ' Da anlatıldığı gibi [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), bu CommandField ile eklemek GridView neden olur, `ShowDeleteButton` özelliğini `true`. Sayfa bir tarayıcıdan ziyaret edildiğinde 4 gösterir, Şekil gibi bir Delete düğmesi bulunur. Bu sayfa, bazı ürünler silerek sınayın.


[![Her GridView Satır Sil düğmesini artık içerir](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Şekil 4**: her GridView Satır Sil düğmesini artık içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Sil düğmesine tıkladığınızda, üzerine geri gönderimin oluştuğu, GridView atar `ProductID` parametre değeri, `DataKeys` olan Sil düğmesini tıklandığını ve SqlDataSource s çağırır satır için koleksiyon değerini `Delete()` yöntemi. SqlDataSource denetimi sonra veritabanına bağlanır ve yürütür `DELETE` deyimi. GridView, daha sonra geri alamazsınız ve (içeren artık yalnızca silinen kayıt) ürünleri geçerli kümesini görüntüleme SqlDataSource için rebinds.

> [!NOTE]
> GridView kullandığından, `DataKeys` SqlDataSource parametreleri doldurmak için koleksiyon, s önemli, GridView s `DataKeyNames` özelliği ayarlanmış birincil anahtar ve, oluşturan sütunlara SqlDataSource s `SelectCommand` döndürür Bu sütun. Ayrıca, bu parametre SqlDataSource s adı önemli s `DeleteCommand` ayarlanır `@ProductID`. Varsa `DataKeyNames` özelliği ayarlı değil veya parametre adlandırılmamış `@ProductsID`de Sil düğmesini tıklatarak geri gönderimin neden olur, ancak kazanılan t gerçekte herhangi bir kayıt silme.


Şekil 5 bu etkileşimi grafik şeklinde gösterir. Geri başvurmak [ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) ekleme, güncelleştirme ve silme verilerden Web denetimi ile ilişkili olayların üzerinde daha ayrıntılı bir tartışma için Öğreticisi.


![GridView Sil düğmesini tıklatarak SqlDataSource s Delete() yöntemi çağırır](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Şekil 5**: GridView Sil düğmesini tıklatarak çağırır SqlDataSource s `Delete()` yöntemi


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>2. adım: Otomatik olarak üretmek`INSERT`,`UPDATE`, ve`DELETE`deyimleri

Adım 1'i denetlenen olarak `INSERT`, `UPDATE`, ve `DELETE` SQL deyimlerini Özellikler penceresini veya denetimi s tanımlayıcı sözdizimi ile belirtilebilir. Ancak, bu yaklaşım, biz el ile SQL deyimlerini el ile ve sıkıcı ve hatalara eğilimli olabilen yazma gerektirir. Neyse ki, veri kaynağı Yapılandırma Sihirbazı'nı sağlamak için bir seçenek sağlar `INSERT`, `UPDATE`, ve `DELETE` deyimleri bir tablo görünümü ekranının belirt sütunlarından kullanırken otomatik olarak oluşturulur.

Bu otomatik oluşturma seçeneği keşfedin s olanak tanır. Tasarımcıda bir DetailsView eklemek `InsertUpdateDelete.aspx` ve kendi `ID` özelliğine `ManageProducts`. Ardından, yeni bir veri kaynağı oluşturun ve adlı bir SqlDataSource oluşturmak DetailsView s akıllı etiketten seçin `ManageProductsDataSource`.


[![ManageProductsDataSource adlı yeni bir SqlDataSource oluşturma](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Şekil 6**: yeni SqlDataSource adlandırılmış oluşturma `ManageProductsDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı kullanmayı tercih `NORTHWINDConnectionString` bağlantı dizesini değiştirin ve İleri'yi tıklatın. Select deyimi ekran yapılandırma belirt sütunları seçili bir tablo veya Görünüm radyo düğmesini bırakın ve çekme `Products` aşağı açılan listesinden tablo. Seçin `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` onay kutusu listesi sütunlarından.


[![Ürünler tabloyu kullanarak ProductID, ProductName, UnitPrice ve devam etmeyen sütunları döndürür](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Şekil 7**: kullanarak `Products` tablo, iade `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` sütunlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Otomatik olarak oluşturmak için `INSERT`, `UPDATE`, ve `DELETE` seçili tablo ve sütun göre deyimleri Gelişmiş düğmesini tıklatın ve Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu.


![Generate INSERT, UPDATE ve DELETE deyimlerini onay kutusunu denetleyin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Şekil 8**: Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu


Generate `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu yalnızca olacaktır checkable seçili olan tablonun birincil anahtarı varsa ve birincil anahtar sütunu (veya sütunlar) döndürülen sütunları listede yer. Seçilebilir duruma kullanım iyimser eşzamanlılık checkbox Generate `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusunu kontrol edildiyse, büyütmek `WHERE` elde edilen içinde yan tümceleri `UPDATE` ve `DELETE` deyimleri iyimser eşzamanlılık denetimi sağlamak için. Şu an için bu onay kutusunun işaretini kaldırın; sonraki öğreticide biz SqlDataSource denetimi ile iyimser eşzamanlılık inceleyeceğiz.

Generate denetledikten sonra `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusunu Select deyimi yapılandırma ekrana dönün ve ardından İleri'yi tıklatın Tamam tıklayın ve ardından, veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın. Sihirbazı tamamladığınızda, Visual Studio BoundFields için DetailsView ekleyecek `ProductID`, `ProductName`, ve `UnitPrice` sütunlar ve CheckBoxField için `Discontinued` sütun. Böylece bu sayfasını ziyaret kullanıcı ürünleri üzerinden geçebilirsiniz DetailsView s akıllı etiketten etkinleştirmek disk belleği seçeneği işaretleyin. Ayrıca DetailsView s temizleyin `Width` ve `Height` özellikleri.

Akıllı etiket etkinleştirme ekleme, düzenlemeyi etkinleştir ve Enable Deleting seçenekleri olduğuna dikkat edin. SqlDataSource değerlerini içeren Bunun nedeni, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand`gibi aşağıdaki bildirim temelli söz dizimini gösterir:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

SqlDataSource denetimi için otomatik olarak ayarlanan değerlerinin nasıl dolmadığı Not kendi `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Başvurulan sütun kümesini `InsertCommand` ve `UpdateCommand` özellikleri temel de `SELECT` deyimi. Diğer bir deyişle, sahip olmak yerine *her* ürünleri sütununda `InsertCommand` ve `UpdateCommand`, yalnızca belirtilen sütun `SelectCommand` (daha az `ProductID`, çünkü atlanmış onu s bir [ `IDENTITY` sütun](http://www.sqlteam.com/item.asp?ItemID=102), düzenlendiğinde değeri değiştirilemez ve hangi otomatik olarak atanır eklenirken). Ayrıca, her parametre için `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` içinde karşılık gelen parametre özellikleri `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları.

DetailsView s veri değişikliği özellikleri açın, etkinleştirme ekleme, düzenlemeyi etkinleştir ve akıllı etiket Enable Deleting seçeneklerinde denetleyin. Bu CommandField ile ekler, `ShowInsertButton`, `ShowEditButton`, ve `ShowDeleteButton` özelliklerini ayarlamak `true`.

Bir tarayıcıda sayfasını ziyaret edin ve düzenleme, silme ve yeni düğmeler DetailsView'da dahil not edin. Her BoundField görüntüler düzenleme moduna kapatır DetailsView Düzenle düğmesini tıklatarak, `ReadOnly` özelliği ayarlanmış `false` (varsayılan) bir metin kutusu ve bir onay olarak CheckBoxField olarak.


[![DetailsView s varsayılan düzenleme arabirimi](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Şekil 9**: DetailsView s düzenleme arabirimi varsayılan ([tam boyutlu görüntüyü görüntülemek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Benzer şekilde, şu anda seçili ürün silin veya yeni bir ürün sisteme ekleyin. Bu yana `InsertCommand` deyimi yalnızca çalışır `ProductName`, `UnitPrice`, ve `Discontinued` sütunları, diğer sütunları olan ya da `NULL` veya INSERT bağlı veritabanı tarafından atanan kendi varsayılan değeri. ObjectDataSource ile durumunda olduğu gibi `InsertCommand` t güncelleştireceğinizi sütunları izin herhangi bir veritabanı tablosunun eksik `NULL` s ve tan t sahip varsayılan değer, bir SQL hatası ortaya çıkar gerçekleştirilmeye çalışılırken `INSERT` deyimi.

> [!NOTE]
> Ekleme ve düzenleme arabirimleri DetailsView s özelleştirme veya doğrulama herhangi bir tür yoksundur. Doğrulama denetimleri ekleme ya da arabirimler özelleştirmek için TemplateFields için BoundFields dönüştürmeniz gerekir. Başvurmak [doğrulama denetimleri ekleme düzenleme ve ekleme arabirimleri için](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) ve [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) öğreticileri daha fazla bilgi için.


Ayrıca, güncelleştirme ve silme için DetailsView geçerli ürün s kullandığını unutmayın `DataKey` yalnızca var ise değer `DataKeyNames` özelliği yapılandırılır. Düzenleme veya silme etkisinin görünüyorsa emin `DataKeyNames` özelliği ayarlanmış.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Otomatik olarak SQL deyimleri oluşturma sınırlamaları

Generate itibaren `INSERT`, `UPDATE`, ve `DELETE` deyimleri seçenek, yalnızca kullanılabilir kendi yazmak daha karmaşık sorgular için bir tablodaki sütunların çekme yapacağı bir dönemde `INSERT`, `UPDATE`, ve `DELETE` 1. adımda yaptığımız gibi deyimleri. Yaygın olarak, SQL `SELECT` bildirimlerini kullanmak `JOIN` bir veya daha fazla arama tabloları için verileri görüntülemek amacıyla geri getirmek için s (geri getirme gibi `Categories` s tablosu `CategoryName` alan ürün bilgileri görüntülerken). Aynı anda biz düzenlemek, güncelleştirmek ya da çekirdek tabloya veri ekleme kullanıcıya izin vermek isteyebilirsiniz (`Products`, bu durumda).

Sırada `INSERT`, `UPDATE`, ve `DELETE` deyimleri el ile girilebilir, aşağıdaki zaman kazandıran ipucu göz önünde bulundurun. Böylece veri yalnızca geri çeker SqlDataSource başlangıçta Kurulum `Products` tablo. Bir tablo veya Görünüm ekran veri kaynağı Yapılandırma Sihirbazı'nı s belirt sütunlarından kullanabilir, böylece otomatik olarak oluşturabilir `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Daha sonra Özellikler penceresinde SelectQuery yapılandırmak için Sihirbazı tamamladıktan sonra'yı seçin (veya alternatif olarak, veri kaynağı Yapılandırma Sihirbazı, ancak kullanım özel bir SQL ifadesi belirtin veya saklı yordam seçeneği geri dönme). Ardından güncelleştirme `SELECT` bildirimini `JOIN` sözdizimi. Bu teknik otomatik olarak oluşturulan SQL deyimlerini zaman kazandıran avantajları sunar ve daha özelleştirilmiş için verir `SELECT` deyimi.

Otomatik olarak oluşturmanın bir diğer sınırlandırma `INSERT`, `UPDATE`, ve `DELETE` deyimleri yapan, sütunları `INSERT` ve `UPDATE` deyimleri tarafından döndürülen sütunlara bağlı `SELECT` deyimi. Güncelleştirme veya daha fazla veya daha az alan, ancak eklemek gerekebilir. Örneğin, adım 2'deki bir örnekte, belki de olmasını istiyoruz `UnitPrice` BoundField salt okunur. Bu durumda, onu shouldn t görünür `UpdateCommand`. Veya biz GridView görünmeyen bir tablo alanı değerini ayarlamak isteyebilirsiniz. Örneğin, yeni bir eklerken kayıt istiyoruz `QuantityPerUnit` değerini ayarlamak için Yapılacaklar.

Bu gibi özelleştirmeler gerekirse, bunları el ile ya da Özellikler penceresini, özel bir SQL ifadesi belirtin veya saklı yordam seçeneği sihirbazda ya da bildirim temelli söz dizimi aracılığıyla aracılığıyla olmanız gerekir.

> [!NOTE]
> Denetim verileri karşılık gelen alanlara sahip olmayan parametreler eklemeyi Web olduğunda, bu parametre değerleri bazı şekilde değerler atanması gerekir aklınızda bulundurun. Bu değerler olabilir: doğrudan kodlanmış `InsertCommand` veya `UpdateCommand`; bazı önceden tanımlanmış kaynağından (sorgu dizesi, oturum durumu, Web denetimlerini sayfası vb.); gelebilir veya önceki öğreticide gördüğümüz gibi programlı olarak atanabilir.


## <a name="summary"></a>Özet

Kendi yerleşik ekleme, düzenleme ve silme özellikleri kullanmak için Web denetimlerini sırayla verileri için bu tür işlevselliği için bağlı veri kaynağı denetimi sağlaması gerekir. SqlDataSource, bunun anlamı `INSERT`, `UPDATE`, ve `DELETE` SQL deyimlerini atanmalıdır `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Bu özellikler ve ilgili parametreleri koleksiyonları el ile eklenmiş veya veri kaynağı Yapılandırma Sihirbazı'nı otomatik olarak oluşturulur. Bu öğreticide her iki tekniği incelendi.

ObjectDataSource iyimser eşzamanlılık kullanarak incelenmesi [uygulama iyimser eşzamanlılık](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Öğreticisi. SqlDataSource denetimi de iyimser eşzamanlılık destek sağlar. Adım 2'de otomatik olarak oluşturulurken belirtildiği gibi `INSERT`, `UPDATE`, ve `DELETE` deyimleri, sihirbazın kullan iyimser eşzamanlılık seçeneği sunar. Sonraki öğreticide anlatıldığı gibi iyimser eşzamanlılık ile SqlDataSource kullanarak değiştirir `WHERE` yan tümcelerinde `UPDATE` ve `DELETE` diğer sütunların değerlerini verileri son başlatıldığından beri değiştirilen t başlattıysanız emin olmak için deyimleri sayfada görüntülenir.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [sonraki](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
