---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Ekleme, güncelleştirme ve silme (C#) SqlDataSource ile veri | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde ObjectDataSource Denetimi, ekleme, güncelleştirme ve verileri silmek için nasıl izin öğrendiniz. SqlDataSource denetimi t destekler...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c52fcf746d80899d7ea568c8110c4dfa610224c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755891"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Ekleme, güncelleştirme ve silme (C#) SqlDataSource ile veri
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) veya [PDF olarak indirin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Önceki öğreticilerde ObjectDataSource Denetimi, ekleme, güncelleştirme ve verileri silmek için nasıl izin öğrendiniz. SqlDataSource denetimi aynı işlemleri destekler ancak farklı bir yaklaşımdır ve Bu öğreticide, ekleme, güncelleştirme ve verileri silmek için SqlDataSource yapılandırma işlemi gösterilmektedir.


## <a name="introduction"></a>Giriş

Bölümünde açıklandığı gibi [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), yerleşik güncelleştirme GridView denetiminde sağlar ve DetailsView ve FormView denetimleri ekleme içerirken silme özellikleri desteği ile birlikte düzenleme ve silme işlevleri. Bu veri değişikliği özellikleri veri kaynağı denetimi bir satır kod yazılması gerek kalmadan doğrudan takılabilir. [Bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) ekleme, güncelleştirme ve silme ile FormView GridView ve DetailsView denetimlerini kolaylaştırmak için ObjectDataSource kullanma incelenir. Alternatif olarak, SqlDataSource ObjectDataSource yerine kullanılabilir.

Ekleme, güncelleştirme ve silme, biz INSERT gerçekleştirmek için çağrılacak nesne katmanı yöntemlerini belirtmek için güncelleştirme veya silme eylemi ObjectDataSource ile desteklemek için geri çağırma. Sağlamak için ihtiyacımız SqlDataSource ile `INSERT`, `UPDATE`, ve `DELETE` SQL deyimleri (veya saklı yordamları) yürütmek için. Bu öğreticide anlatıldığı gibi bu deyimler el ile oluşturulabilir veya SqlDataSource s veri kaynağı Yapılandırma Sihirbazı tarafından otomatik olarak oluşturulabilir.

> [!NOTE]
> Bu yana ve zaten ekleme, düzenleme ve DetailsView, GridView yeteneklerini silme ele almıştık ve FormView denetler, bu öğreticide işlemlerini desteklemek için SqlDataSource denetimi yapılandırma üzerinde odaklanır. Bu özellikler düzenleme, ekleme ve silme veri öğreticiler GridView DetailsView ve FormView dönüşü içinde uygulamaya tazelemek istiyorsanız başlayarak [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>1. adım: Belirtme`INSERT`,`UPDATE`, ve`DELETE`deyimleri

Olarak seçmeliyiz SqlDataSource denetimden verileri almak üzere son iki öğreticilerde görülen ve iki özelliği ayarlayın:

1. `ConnectionString`, ne, sorgu göndermek için veritabanı belirtir ve
2. `SelectCommand`, geçici SQL deyiminin veya saklı yordam adı sonuçları döndürmek için yürütülecek belirtir.

İçin `SelectCommand` parametreleriyle değerleri, değerler SqlDataSource s belirtilen parametre `SelectParameters` koleksiyonu ve ortak parametre kaynak değerlerini sabit kodlu değer içerebilir (querystring alanları, oturum değişkenleri Web denetim değerleri ve VS.) ya da programlı bir şekilde atanabilir. SqlDataSource denetlemek s `Select()` yöntemi Web denetimi veri çağrılması programlama yoluyla veya otomatik olarak veritabanına bir bağlantı kurulur, sorgu parametresi değerler atanır ve için Kapat komutunu shuttled Veritabanı. Sonuçlar daha sonra bir veri kümesi veya DataReader, ' s denetimin değerine bağlı olarak döndürülen `DataSourceMode` özelliği.

Verileri seçme birlikte SqlDataSource denetimi ekleme, güncelleştirme ve verileri sağlayarak silmek için kullanılabilir `INSERT`, `UPDATE`, ve `DELETE` SQL deyimlerinde epey benzer. Yalnızca Ata `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri `INSERT`, `UPDATE`, ve `DELETE` SQL deyimleri yürütmek için. (Her zaman en göründükleri gibi) ifadeler parametrelere sahip olup, içine dahil `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları.

Bir kez bir `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` değer belirtilmiş, karşılık gelen verileri Web denetimi s akıllı etiket ekleme etkinleştirmek, düzenlemeyi etkinleştir ya da silmeyi etkinleştir seçeneği kullanılabilir olur. Let s bunu göstermek için bir örnekten ele `Querying.aspx` oluşturduğumuz sayfa [SqlDataSource denetimi ile veri sorgulama](querying-data-with-the-sqldatasource-control-cs.md) öğretici ve ayırma özellikleri içerecek şekilde silin.

Başlangıç açarak `InsertUpdateDelete.aspx` ve `Querying.aspx` gelen sayfaları `SqlDataSource` klasör. Üzerinde Tasarımcısından `Querying.aspx` sayfasında, ilk örnekte SqlDataSource ve GridView seçin ( `ProductsDataSource` ve `GridView1` denetimleri). İki denetimi seçtikten sonra düzenleme menüsüne gidin ve Kopyala'yı seçin (veya Ctrl + C yalnızca isabet). Ardından, tasarımcısına Git `InsertUpdateDelete.aspx` denetimlerinde yapıştırın. Sonra iki denetim e taşınmış `InsertUpdateDelete.aspx`, test bir tarayıcıda sayfası. Değerlerini görmelisiniz `ProductID`, `ProductName`, ve `UnitPrice` tüm kayıtları için sütunları `Products` veritabanı tablosu.


[![Tüm ürünleri, sıralı ProductID listelenir](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Şekil 1**: tüm ürünleri, göre sıralanmış listelenen `ProductID` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>SqlDataSource s ekleme`DeleteCommand`ve`DeleteParameters`özellikleri

Bu noktada yalnızca tüm kayıtları döndüren bir SqlDataSource sahibiz `Products` tablo ve bu verileri işleyen GridView. Hedefimiz, kullanıcının ürünleri GridView aracılığıyla silmek izin vermek için bu örneği genişletmek sağlamaktır. SqlDataSource denetimi s değerlerini belirtmek için ihtiyacımız bunu sağlamak için `DeleteCommand` ve `DeleteParameters` özellikleri ve GridView silme destekleyecek şekilde yapılandırın.

`DeleteCommand` Ve `DeleteParameters` özellikleri çeşitli şekillerde belirtilebilir:

- Bildirim temelli söz dizimi aracılığıyla
- Tasarımcıda Özellikler penceresinden
- Özel bir SQL deyimi belirtin veya saklı yordam ekranından veri kaynağı Yapılandırma Sihirbazı
- Görünüm ekranında veri kaynağı Yapılandırma Sihirbazı'nı bir tablo belirtin sütunlarından Gelişmiş düğmesi aracılığıyla, otomatik olarak oluşturacağını `DELETE` kullanılan SQL deyimi ve parametre koleksiyonu `DeleteCommand` ve `DeleteParameters` özellikleri

Otomatik olarak nasıl inceleyeceğiz `DELETE` deyimi 2. adımda oluşturmuştunuz. Veri Kaynağı Yapılandırma Sihirbazı'nı veya bildirim temelli söz dizimi seçeneği da çalışır ancak şimdilik s Tasarımcısı'nda Özellikler penceresini kullanın olanak tanır.

Tasarımcıda gelen `InsertUpdateDelete.aspx`, tıklayarak `ProductsDataSource` SqlDataSource ve ardından Özellikler penceresini açın (Görünüm menüsünden, Özellikler penceresinde seçin veya yalnızca, F4 isabet). Üç nokta kümesi getirecek DeleteQuery özelliği seçin.


![DeleteQuery özelliğini Özellikler penceresinden seçin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Şekil 2**: DeleteQuery özelliğini Özellikler penceresinden seçin


> [!NOTE]
> SqlDataSource eklenmemişse t bir DeleteQuery özellik vardır. Bunun yerine, DeleteQuery birleşimidir `DeleteCommand` ve `DeleteParameters` özellikleri ve Özellikler penceresinde Tasarımcı penceresinden görüntülerken yalnızca listelenir. Kaynak Görünümü Özellikler penceresinde bakıyorsanız bulabilirsiniz `DeleteCommand` özelliği bunun yerine.


Komut ve parametre Düzenleyicisi iletişim kutusunu açmak için DeleteQuery özelliğinde üç noktayı (bkz. Şekil 3) kutusuna tıklayın. Bu iletişim kutusunda belirttiğiniz `DELETE` SQL deyimi ve parametreleri belirtin. Aşağıdaki sorguyu girin `DELETE` komut textbox (ya da el ile veya tercih ederseniz, Sorgu Oluşturucusu'nu kullanarak):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Ardından, eklemek için parametreleri Yenile düğmesini `@ProductID` parametresini aşağıdaki parametrelerin listesi.


[![DeleteQuery özelliğini Özellikler penceresinden seçin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Şekil 3**: DeleteQuery özelliğini Özellikler penceresinden seçin ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Yapmak *değil* (bırakın, parametresi None kaynak) Bu parametre için bir değer sağlayın. Silme desteği GridView'a eklediğinizde GridView otomatik olarak bu parametre değerini kullanarak sağlayacak kendi `DataKeys` olan Sil düğmesine tıklanana satır koleksiyonu.

> [!NOTE]
> Kullanılan parametre adı `DELETE` sorgu *gerekir* adı ile aynı olması `DataKeyNames` GridView, DetailsView veya FormView değeri. Diğer bir deyişle, parametreyi `DELETE` deyimi kullanılamıyor.%n%nÇözüm adlı `@ProductID` (yerine, örneğin `@ID`), Ürünler tablosu (ve bu nedenle GridView DataKeyNames değer) birincil anahtar sütunu adı olduğundan `ProductID`.


Parametre adı ve `DataKeyNames` değeri eklenmemişse t eşleşme GridView otomatik olarak atayamazsınız parametresi değerinden `DataKeys` koleksiyonu.

Komut ve parametre Düzenleyicisi iletişim kutusuna delete ile ilgili bilgileri girdikten sonra Tamam'a tıklayın ve ortaya çıkan bildirim temelli biçimlendirme incelemek için kaynak görünümüne gidin:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Ek unutmayın `DeleteCommand` özelliği yanı sıra `<DeleteParameters>` bölümü ve adlı parametre nesnesini `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>GridView'ın silmek için yapılandırma

İle `DeleteCommand` özelliği eklendi, GridView s akıllı etiket artık Silmeyi Etkinleştir seçeneği içerir. Devam edip bu onay kutusunu işaretleyin. Bölümünde açıklandığı gibi [bir genel bakış, güncelleştirme ve silme ekleme,](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), GridView ile bir CommandField eklemek bu neden olur, `ShowDeleteButton` özelliğini `true`. Bir tarayıcıdan sayfayı ziyaret edildiğinde 4 gösterir, Şekil gibi Sil düğmesini dahil edilir. Bu sayfa, bazı ürünler silerek test edin.


[![Her GridView Satır Sil düğmesini artık içerir.](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Şekil 4**: her GridView satırında bir Sil düğmesini artık içerir ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Sil düğmesini tıklatarak, bağlı bir geri gönderme gerçekleşir, GridView atar `ProductID` parametre değeri, `DataKeys` olan Sil düğmesine tıklandığını ve SqlDataSource s çağırır satır için koleksiyon değeri `Delete()` yöntemi. SqlDataSource denetimi ardından veritabanına bağlanır ve yürüten `DELETE` deyimi. GridView geri alma ve görüntüleme (kod artık tam olarak silinen kayıt içerir) ürünleri geçerli kümesini SqlDataSource için ardından rebinds.

> [!NOTE]
> GridView kullandığından, `DataKeys` SqlDataSource parametreleri doldurmak için koleksiyon, önemli s, GridView s `DataKeyNames` birincil anahtar ve, oluşturan sütunları özelliğinin ayarlanması SqlDataSource s `SelectCommand` döndürür Bu sütunlar. Ayrıca, bu parametre SqlDataSource s'te adı önemli s `DeleteCommand` ayarlanır `@ProductID`. Varsa `DataKeyNames` özelliği ayarlı değil veya parametre adlandırılmamış `@ProductsID`Sil düğmesine tıklanarak geri göndermeye neden olur, ancak kazanılan t gerçekte herhangi bir kaydı silin.


Şekil 5, bu etkileşim grafik olarak gösterir. Kiracıurl [ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) ekleme, güncelleştirme ve silme verilerden Web denetimi ile ilgili olaylar zincirini üzerinde daha ayrıntılı bir açıklaması için öğretici.


![GridView Sil düğmesini tıklatarak SqlDataSource s Delete() yöntemi çağırır](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Şekil 5**: GridView Sil düğmesini tıklatarak çağırır SqlDataSource s `Delete()` yöntemi


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>2. adım: Otomatik olarak oluşturma`INSERT`,`UPDATE`, ve`DELETE`deyimleri

Denetlenen 1. adım olarak `INSERT`, `UPDATE`, ve `DELETE` SQL deyimleri, Özellikler penceresinde veya denetim s bildirim temelli söz dizimi belirtilebilir. Ancak, bu yaklaşım, biz el ile SQL deyimlerini el ile ve sıkıcı ve hataya olabilen yazma gerektirir. Neyse ki, veri kaynağı Yapılandırma Sihirbazı için bir seçenek sağlar `INSERT`, `UPDATE`, ve `DELETE` deyimleri görüntüleme ekranı tablosunu belirtin sütunlarından kullanırken otomatik olarak oluşturulur.

Bu otomatik oluşturma seçeneği keşfedin s olanak tanır. Tasarımcıda bir DetailsView eklemek `InsertUpdateDelete.aspx` ve kendi `ID` özelliğini `ManageProducts`. Ardından, yeni bir veri kaynağı oluşturun ve adlı bir SqlDataSource oluşturmak DetailsView s akıllı etiketten seçin `ManageProductsDataSource`.


[![ManageProductsDataSource adlı yeni bir SqlDataSource oluşturma](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Şekil 6**: adlı yeni bir SqlDataSource oluşturma `ManageProductsDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'ndan kullanmayı tercih `NORTHWINDConnectionString` bağlantı dizesi ve İleri'ye tıklayın. Select deyimi ekran yapılandırma belirtin sütunları seçili bir tablo veya Görünüm radyo düğmesini bırakın ve çekme `Products` aşağı açılan listeden bir tablo. Seçin `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` onay kutusu listesi sütunları.


[![Ürünleri tabloyu kullanarak ProductID, ProductName, UnitPrice ve kullanımdan kaldırılan sütunlar döndürür](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Şekil 7**: kullanarak `Products` tablo, iade `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` sütunları ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Otomatik olarak oluşturmak için `INSERT`, `UPDATE`, ve `DELETE` seçili tablo ve sütunlara göre deyimleri Gelişmiş düğmesine tıklayın ve Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu.


![Generate INSERT, UPDATE ve DELETE deyimleri onay kutusunu işaretleyin](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Şekil 8**: Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` onay deyimleri


Generate `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu yalnızca olacaktır checkable seçili olan tablonun birincil anahtar varsa ve birincil anahtar sütunu (veya sütun) geri dönen sütunlar listesinde yer. Seçilebilir duruma kullanım iyimser eşzamanlılık onay kutusunu Oluştur `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu iade, büyütmek `WHERE` sonuç olarak yan tümceleri `UPDATE` ve `DELETE` deyimleri iyimser eşzamanlılık denetimi sağlamak için. Şu an için bu onay kutusunu işaretlemeden bırakın; sonraki öğreticide SqlDataSource denetimi ile iyimser eşzamanlılık inceleyeceğiz.

Generate denetledikten sonra `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusunu Select deyimini yapılandırma ekranına geri dönmek ve ardından İleri'yi için Tamam'a tıklayın ve ardından, veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için son. Sihirbazı tamamladığınızda, Visual Studio BoundFields için DetailsView ekleyeceksiniz `ProductID`, `ProductName`, ve `UnitPrice` sütunları ve için bir CheckBoxField `Discontinued` sütun. Böylece bu sayfasını ziyaret ederek kullanıcı ile ürünleri geçebilirsiniz DetailsView s akıllı etiketten sayfalama Etkinleştir seçeneği işaretleyin. Ayrıca DetailsView s Temizle `Width` ve `Height` özellikleri.

Akıllı etiket etkinleştirme ekleme, düzenlemeyi etkinleştir ve silmeyi etkinleştir seçenekleri olduğuna dikkat edin. SqlDataSource değerlerini içeren olmasıdır, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand`aşağıdaki bildirim temelli söz dizimi gösterildiği gibi:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

SqlDataSource denetimi için otomatik olarak ayarlanmış değerleri nasıl aldığını unutmayın, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Başvurulan sütun kümesini `InsertCommand` ve `UpdateCommand` özelliklerini de temel alınmıştır `SELECT` deyimi. Diğer bir deyişle, yerine *her* ürünleri sütununda `InsertCommand` ve `UpdateCommand`, yalnızca belirtilen sütun `SelectCommand` (daha az `ProductID`, çünkü atlanırsa, s bir [ `IDENTITY` sütun](http://www.sqlteam.com/item.asp?ItemID=102)düzenlendiğinde değeri değiştirilemez ve otomatik olarak atanır eklenirken). Ayrıca, her parametre için `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri içinde karşılık gelen parametre yok `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları.

DetailsView s veri değişikliği özelliklerini etkinleştirme için etkinleştirme ekleme, düzenlemeyi etkinleştir ve akıllı etiketinde silmeyi etkinleştir seçeneklerinde denetleyin. Bu bir CommandField ile ekler, `ShowInsertButton`, `ShowEditButton`, ve `ShowDeleteButton` özelliklerini ayarlamak `true`.

Bir tarayıcıda sayfasını ziyaret edin ve düzenleme, silme ve DetailsView içinde bulunan yeni düğmeler dikkat edin. Düzenle düğmesine tıklayarak, her BoundField görüntüler düzenleme moduna DetailsView kapatır, `ReadOnly` özelliği `false` (varsayılan) olarak bir metin kutusu ve bir onay kutusu olarak CheckBoxField.


[![DetailsView s varsayılan düzenleme arabirimi](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Şekil 9**: DetailsView s varsayılan düzenleme arabirimine ([tam boyutlu görüntüyü görmek için tıklatın](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Benzer şekilde, şu anda seçili ürünü silmek ya da sisteme yeni bir ürün ekleyin. Bu yana `InsertCommand` deyimi yalnızca çalışır `ProductName`, `UnitPrice`, ve `Discontinued` sütunları, diğer sütunları olan ya da `NULL` veya ekleme sırasında veritabanı tarafından atanan varsayılan değer. ObjectDataSource ile durumunda olduğu gibi `InsertCommand` t ki sütunları izin herhangi bir veritabanı tablosu eksik `NULL` s ve rsquo; < varsayılan değerine sahip, bir SQL hatası meydana gelir yürütmeye çalışılırken `INSERT` deyimi.

> [!NOTE]
> Ekleme ve düzenleme arabirimleri DetailsView s herhangi bir tür özelleştirme ya da doğrulama yoksundur. Doğrulama denetimleri ekleme veya arabirimleri özelleştirmek için bunları TemplateField için BoundFields dönüştürmeniz gerekir. Başvurmak [arabirimleri ekleme ve düzenleme için doğrulama denetimleri ekleme](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) ve [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) daha fazla bilgi için öğreticiler.


Ayrıca, güncelleştirme ve silme için kullandığı geçerli ürün s DetailsView akılda tutulması `DataKey` yalnızca var ise değer `DataKeyNames` özelliği yapılandırılır. Düzenleme veya silme etkisinin görünüyorsa emin `DataKeyNames` özelliği ayarlanmış.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Otomatik olarak SQL deyimleri oluşturma sınırlamaları

Generate beri `INSERT`, `UPDATE`, ve `DELETE` deyimleri seçeneği, yalnızca kullanılabilir, daha karmaşık sorgular için bir tablodaki sütunların çekme kendi yazmak yapacağı `INSERT`, `UPDATE`, ve `DELETE` Adım 1'de yaptığımız gibi deyimler. Yaygın olarak, SQL `SELECT` deyimlerini `JOIN` verileri için bir veya daha fazla arama tabloları görüntülemek amacıyla geri getirmek için s (geri getirme gibi `Categories` tablo s `CategoryName` ürün bilgileri görüntülerken, alan). Aynı zamanda, biz düzenlemek, güncelleştirme veya çekirdek tabloya veri eklemek kullanıcı izin vermek isteyebilirsiniz (`Products`, bu durumda).

Sırada `INSERT`, `UPDATE`, ve `DELETE` deyimleri el ile girilebilir, aşağıdaki zamandan tasarruf ipucu göz önünde bulundurun. Böylece geri veri yalnızca çeker SqlDataSource başlangıçta Kurulum `Products` tablo. Veri Kaynağı Yapılandırma Sihirbazı'nı s belirtin sütunlar bir tablo veya Görünüm ekrandan kullanabilir, böylece otomatik olarak oluşturabilirsiniz `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Ardından Özellikler penceresinden SelectQuery yapılandırmak Sihirbazı tamamladıktan sonra seçin (veya alternatif olarak, veri kaynağı Yapılandırma Sihirbazı, ancak kullanım özel bir SQL deyimi belirtin veya saklı yordam seçeneği geri dönme). Ardından update `SELECT` bildirimini `JOIN` söz dizimi. Bu teknik otomatik olarak oluşturulan SQL deyimlerini zamandan tasarruf avantajlarını sunar ve için daha fazla özelleştirilmiş sağlayan `SELECT` deyimi.

Otomatik olarak oluşturmanın bir diğer sınırlandırma `INSERT`, `UPDATE`, ve `DELETE` deyimler olduğu, sütunları `INSERT` ve `UPDATE` deyimleri tarafından döndürülecek olan sütunları temel `SELECT` deyimi. Güncelleştirin veya daha fazla veya daha az alanları ancak eklemek gerekebilir. Örneğin, adım 2'deki örnekte, belki de olmasını istiyoruz `UnitPrice` BoundField salt okunur. Bu durumda, paylaşılmamalıdır t görünür `UpdateCommand`. Veya, biz GridView görünmeyen bir tablo alanın değerini ayarlamak isteyebilirsiniz. Örneğin, yeni bir eklerken kayıt istiyoruz `QuantityPerUnit` TODO için değer ayarlayın.

Bu tür özelleştirmeleri gerekiyorsa, bunları el ile Özellikler penceresinde, özel bir SQL deyimi belirtin ya da saklı yordam seçeneği sihirbazda ya da bildirim temelli söz dizimi aracılığıyla aracılığıyla yapmak gerekir.

> [!NOTE]
> Veriler karşılık gelen alanlara sahip olmayan parametreler eklemeyi Web denetimi, bu parametreleri değerleri değerlerde bir şekilde atanması gerekir aklınızda bulundurun. Bu değerler olabilir: doğrudan kodlanmış `InsertCommand` veya `UpdateCommand`; bazı önceden tanımlanmış kaynaktan (sorgu dizesi, oturum durumu, Web denetimleri sayfa vb.); gelebilir veya önceki öğreticide gördüğümüz gibi programlı olarak atanabilir.


## <a name="summary"></a>Özet

Kendi yerleşik ekleme, düzenleme ve silme özelliklerini kullanmak için Web denetimlerini sırayla veriler için bağlı olan veri kaynak denetimi bu işlevselliğin sağlaması gerekir. SqlDataSource için diğer bir deyişle `INSERT`, `UPDATE`, ve `DELETE` için SQL deyimleri atanmalıdır `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Bu özellikler ve karşılık gelen parametreleri koleksiyonları el ile eklenen veya veri kaynağı Yapılandırma Sihirbazı otomatik olarak oluşturulur. Bu öğreticide size her iki tekniği incelenir.

İçinde ObjectDataSource ile iyimser eşzamanlılık kullanarak incelenmesi [iyimser eşzamanlılık uygulama](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) öğretici. SqlDataSource denetimi iyimser eşzamanlılık desteği de sağlar. Adım 2'de otomatik olarak oluştururken belirtildiği gibi `INSERT`, `UPDATE`, ve `DELETE` deyimleri, sihirbaz teklif iyimser eşzamanlılık seçeneğini kullanın. SqlDataSource ile iyimser eşzamanlılık kullanma sonraki öğreticide anlatıldığı gibi değiştirir `WHERE` yan tümcelerinde `UPDATE` ve `DELETE` değerleri için diğer sütunları veri son başlatıldığından beri değiştirilmiş t haven emin olmak için deyimleri sayfada görüntülenir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [İleri](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
