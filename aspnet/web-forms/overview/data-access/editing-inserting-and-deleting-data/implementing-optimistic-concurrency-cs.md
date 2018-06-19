---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: İyimser eşzamanlılık (C#) uygulama | Microsoft Docs
author: rick-anderson
description: Verileri düzenlemek birden çok kullanıcı sağlayan bir web uygulaması için iki kullanıcı aynı verileri aynı anda düzenleme riski yoktur. Bu tutori içinde...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 27441ea9343055b3139468036fc6f201c77667e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891054"
---
<a name="implementing-optimistic-concurrency-c"></a>Uygulama iyimser eşzamanlılık (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) veya [PDF indirin](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Verileri düzenlemek birden çok kullanıcı sağlayan bir web uygulaması için iki kullanıcı aynı verileri aynı anda düzenleme riski yoktur. Bu öğreticide Biz bu riski işlemek için iyimser eşzamanlılık denetimini uygulamanız.


## <a name="introduction"></a>Giriş

Yalnızca verileri görüntülemek kullanıcıların web uygulamaları için ya da kimin verileri değiştirebilir yalnızca tek bir kullanıcı eklemek için hiçbir tehdit yanlışlıkla başka birinin değişikliklerin üzerine iki eş zamanlı kullanıcı yoktur. Güncelleştirmek veya veri silmek birden çok kullanıcıların web uygulamaları için ancak yoktur olası bir kullanıcının değişikliklerin başka bir eş zamanlı kullanıcının ile artar. İki kullanıcı aynı anda tek bir kaydını düzenlerken herhangi bir yerde eşzamanlılık ilke olmadan kendi değişiklikleri kaydeder kullanıcının son ilk tarafından yapılan değişiklikleri geçersiz kılar.

Örneğin, iki kullanıcılar, Jisun ve Sam, her ikisi de güncelleştirme ve ürünleri GridView denetimi aracılığıyla silme ziyaretçileri izin uygulamamızdaki sayfasını ziyaret düşünün. Her ikisi de yaklaşık aynı zamanda GridView Düzenle düğmesini tıklatın. Jisun ürün adı "Chai Çay" değiştirir ve güncelleştirme düğmesine tıklar. Net sonucu olan bir `UPDATE` ayarlar veritabanına gönderilen deyimi *tüm* ürünün güncelleştirilebilir alanlarının (Jisun yalnızca bir alanın güncelleştirilmiş olmasa bile `ProductName`). Bu anda, veritabanı değerleri "Chai Çay," Meşrubat, vb. belirli bu ürün için Exotic Liquids sağlayıcı kategorisi vardır. Ancak, Sam'ın ekranında GridView hala ürün adı düzenlenebilir GridView satırda "Chai" gösterilir. Birkaç saniye sonra Jisun'ın değişiklikler kaydedildi Sam Condiments için kategori güncelleştirir ve güncelleştirme tıklar. Bunun bir `UPDATE` "Chai," ürün adına ayarlar veritabanına gönderilen deyimi `CategoryID` karşılık gelen Meşrubat kategori kimliği ve benzeri. Ürün adı Jisun'ın değişiklikleri üzerine yazıldı. Şekil 1 Bu dizi olayı grafik şeklinde gösterir.


[![İki kullanıcı aynı anda bir kaydı güncelleştirdiğinizde var. bir kullanıcıyı s olası diğer s üzerine yazmak için değiştirir](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Şekil 1**: olduğunda iki kullanıcılar aynı anda güncelleştirme bir kayıt var. s olası bir kullanıcıyı değiştirir üzerine yaz diğer s ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image3.png))


Bir sayfa ziyaret eden iki kullanıcılar, benzer şekilde, bir kullanıcı başka bir kullanıcı tarafından silindiğinde kayıt ortasında olabilir. Veya, bir kullanıcı bir sayfa yüklediğinde arasında Sil düğmesini tıklattığınızda, başka bir kullanıcı, kaydın içeriğini değiştirilmiş olabilir.

Üç [eşzamanlılık denetimi](http://en.wikipedia.org/wiki/Concurrency_control) stratejileri kullanılabilir:

- **Hiçbir şey yapma** -eşzamanlı kullanıcı aynı kaydı değiştiriyorsanız (varsayılan davranış) win son yürütme olanak tanır.
- [**İyimser eşzamanlılık** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -, olabilir ancak eşzamanlılık çakışıyor every şimdi ve daha sonra bu gibi çakışmaları olmaz ortaya; zaman çoğunluğu bu nedenle, bir çakışma çıkması durumunda yalnızca bildirmek kullanıcı varsayılır, yaptıkları değişiklikleri başka bir kullanıcı aynı veri değiştirildiğinden kaydedilemiyor
- **Eşzamanlılık** -eşzamanlılık çakışmaları olan sıradan bir hale ve kullanıcıların eklenmesini tolerans olmaz yaptıkları değişiklikleri nedeniyle başka bir kullanıcının eş zamanlı etkinlik kaydedilmiş doğru söylediyse varsayılır; Bu nedenle, bir kullanıcı kaydı güncelleme başladığında, kilitleme , böylece düzenleme veya kullanıcı, değişiklikleri uygulayan kadar bu kaydın silinmesi tüm diğer kullanıcıların önleme

Tüm öğreticilerimizi bugüne kadarki varsayılan eşzamanlılık çözümleme stratejisi kullanmış - yani, biz win son yazma izin. Bu öğreticide biz iyimser eşzamanlılık denetimi nasıl inceleyeceğiz.

> [!NOTE]
> Bu öğretici serisinde eşzamanlılık örnekleri ele olmaz. Eşzamanlılık gibi kilitler çünkü nadiren kullanılır değilse düzgün relinquished, diğer kullanıcıların verileri güncelleştirme engelleyebilir. Örneğin, bir kullanıcıyı düzenlemek için bir kayıt kilitler ve gün onu kilidini önce için ayrıldığında, başka bir kullanıcı özgün kullanıcıya döndürür ve kendi güncelleştirme tamamlanana kadar bu kaydı güncelleştirmek mümkün olacaktır. Bu nedenle, eşzamanlılık kullanıldığı durumlarda yoktur genellikle ulaştıysanız, kilit iptal eden bir zaman aşımı. Kullanıcı sipariş işlemini tamamlarken, kısa bir süre için belirli katılımcı Konumu Kilitle bilet satış Web siteleri, eşzamanlılık denetim örneğidir.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>1. adım: Nasıl iyimser eşzamanlılık arayan uygulanır

İyimser eşzamanlılık denetimini güncelleştirme veya silme işlemi başlattığınızda olduğu gibi güncelleştirildiğinde veya silindiğinde kayıt aynı değere sahip olduğundan emin olarak çalışır. Örneğin, düzenlenebilir GridView Düzenle düğmesine tıklandığında, kaydın değerleri veritabanından okunur ve metin kutuları ve diğer Web denetimleri görüntülenir. Bu orijinal değerleri GridView tarafından kaydedilir. Kullanıcı kendi değişiklikleri yapar ve güncelleştirme düğmesine tıklar sonra daha sonra özgün değerler artı yeni değerleri için iş mantığı katmanı ve veri erişim katmanı için gönderilir. Veri erişim katmanı değerler hala veritabanı kullanıcı düzenleme başladı özgün değerler özdeş ise, yalnızca kaydı güncelleştirecek bir SQL deyimi kesmeniz gerekir. Şekil 2 Bu olaylar dizisini gösterir.


[![Güncelleştirme veya silme için başarılı olması, özgün değerler geçerli veritabanı değerine eşit olmalıdır](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Şekil 2**: For Update veya Delete Succeed, orijinal değerleri gerekir olması eşit geçerli veritabanı değerlerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image6.png))


İyimser eşzamanlılık uygulamak için çeşitli yaklaşım vardır (bkz [Peter A. Bromberg](http://peterbromberg.net/)'s [Optmistic eşzamanlılık güncelleştirme mantığı](http://www.eggheadcafe.com/articles/20050719.asp) bir dizi seçenek kısa göz için). ADO.NET yazılan veri kümesi yalnızca bir onay kutusu değer çizgilerinin ile yapılandırılmış bir uygulamasını sağlar. Yazılan veri kümesinde bir TableAdapter TableAdapter's güçlendirir için iyimser eşzamanlılık etkinleştirme `UPDATE` ve `DELETE` tüm özgün değerler karşılaştırması içerecek şekilde deyimleri `WHERE` yan tümcesi. Aşağıdaki `UPDATE` deyimi, örneğin, güncelleştirmeleri adını ve bir ürünün fiyat yalnızca geçerli veritabanı değerler GridView kaydında güncelleştirirken başlangıçta alınan değerleri eşit ise. `@ProductName` Ve `@UnitPrice` parametreleri, ancak kullanıcı tarafından girilen yeni değerleri içermesi `@original_ProductName` ve `@original_UnitPrice` Düzenle düğmesi tıklatıldığında GridView başlangıçta yüklenen değerleri içerir:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Bu `UPDATE` deyimi okunabilirlik için basitleştirilmiştir. Uygulamada, `UnitPrice` iade `WHERE` yan yana daha karmaşık olacaktır `UnitPrice` içerebilir `NULL` s ve denetleniyor `NULL = NULL` her zaman False değerini döndürür (Bunun yerine kullanmalısınız `IS NULL`).


Farklı bir arka plandaki kullanmanın yanı sıra `UPDATE` iyimser eşzamanlılık Ayrıca kendi DB imzası değiştirir kullanmak için bir TableAdapter yapılandırma deyimi, doğrudan yöntemleri. Geri çağırma ilk öğreticimizi [ *veri erişim katmanı oluşturma*](../introduction/creating-a-data-access-layer-cs.md), DB doğrudan yöntemleri, skaler listesini kabul edildiği gibi giriş parametreleri değerleri (kesin türü belirtilmiş bir DataRow yerine veya DataTable örneği). İyimser eşzamanlılık, doğrudan DB kullanırken `Update()` ve `Delete()` yöntemleri, özgün değerleri de giriş parametrelerini içerir. Ayrıca, batch kullanılarak için BLL kodunda güncelleştirme deseni ( `Update()` DataRow DataTables yerine ve skaler değerler kabul yöntemi aşırı) de değiştirilmesi gerekir.

Bunun yerine bizim varolan genişletmek daha (hangi uyum sağlayacak şekilde BLL değiştirme istediğinde) iyimser eşzamanlılık kullanalım için DAL'ın TableAdapters bunun yerine yeni yazılan adlı bir veri kümesi oluşturma `NorthwindOptimisticConcurrency`, hangi ekleyeceğiz için bir `Products` TableAdapter, İyimser eşzamanlılık kullanır. Biz oluşturacaksınız bir `ProductsOptimisticConcurrencyBLL` iyimser eşzamanlılık DAL desteklemek için uygun değişiklikleri olan iş mantığı katmanı sınıfı. Bu geçişteki yerleştirilmiş sonra biz ASP.NET sayfası oluşturmak için hazır olacaktır.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>2. adım: iyimser eşzamanlılık destekleyen veri erişim katmanı oluşturma

Yeni yazılan veri kümesi oluşturmak için sağ `DAL` klasör içinde `App_Code` klasörü ve adlı yeni bir veri kümesi ekleyin `NorthwindOptimisticConcurrency`. İlk öğreticide gördüğümüz gibi bunun yeni bir TableAdapter yazılan TableAdapter Yapılandırma Sihirbazı'nı otomatik olarak başlatılması veri kümesi için bu nedenle ekleyin. İlk ekranda, biz bağlanmak-aynı Northwind veritabanı'na bağlanmak istediğiniz veritabanını belirtmek için istenir `NORTHWNDConnectionString` setting from `Web.config`.


[![Aynı Northwind veritabanına bağlan](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Şekil 3**: aynı Northwind veritabanına bağlanma ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image9.png))


Ardından, biz verilerin nasıl aldığını istenir: geçici SQL deyimi, yeni bir saklı yordam veya varolan bir saklı yordamı. Geçici SQL sorguları bizim özgün DAL kullandık olduğundan, bu seçenek burada da kullanın.


[![Geçici SQL deyimi kullanarak alınacak veri belirtin](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Şekil 4**: geçici SQL deyimi kullanarak alınacak veri belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image12.png))


Aşağıdaki ekranda ürün bilgisi almak için kullanılacak SQL sorgusu girin. İçin kullanılan aynı tam SQL sorgusunu kullanalım `Products` tüm döndürür bizim özgün DAL gelen TableAdapter `Product` ürünün sağlayıcısına ve kategori adları ile birlikte sütunlar:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Özgün DAL ürünleri TableAdapter aynı SQL sorgudan kullanın](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Şekil 5**: aynı SQL sorgusu kullanın `Products` özgün DAL TableAdapter ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image15.png))


Sonraki ekrana taşımadan önce Gelişmiş Seçenekler düğmesini tıklatın. Bu TableAdapter employ iyimser eşzamanlılık denetimini sağlamak için yalnızca "iyimser eşzamanlılık kullan" onay kutusunu işaretleyin.


[![Denetleme tarafından iyimser eşzamanlılık denetimini etkinleştir &quot;iyimser eşzamanlılık kullanmak&quot; onay kutusu](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Şekil 6**: iyimser eşzamanlılık "iyimser eşzamanlılık kullan" onay kutusunu işaretleyerek denetimi etkinleştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image18.png))


Son olarak, hem bir DataTable doldurun ve DataTable veri erişimi desenleri TableAdapter kullanması gerektiğini gösterir; aynı zamanda DB doğrudan yöntemleri oluşturulması gerektiğini belirtir. Dönüş için yöntem adını DataTable düzeni GetData bizim özgün DAL kullandık adlandırma kuralları yansıtmak amacıyla GetProducts değiştirin.


[![Tüm veri erişim desenlerini kullanan TableAdapter sahip](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Şekil 7**: TableAdapter kullanan tüm veri erişim düzenlere sahip ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image21.png))


Sihirbazı tamamladıktan sonra veri kümesi Tasarımcısı kesin türü belirtilmiş bir içerecektir `Products` DataTable ve TableAdapter. DataTable nesnesinden yeniden adlandırmak için bir dakikanızı ayırın `Products` için `ProductsOptimisticConcurrency`, DataTable nesnesinin başlık çubuğunda sağ tıklayıp bağlam menüsünden yeniden adlandır seçerek bunu yapabilirsiniz.


[![DataTable ve TableAdapter türü belirtilmiş veri kümesine eklenmiştir](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Şekil 8**: bir DataTable ve TableAdapter yazılan veri kümesi eklenmiştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image24.png))


Arasındaki farklar görmek için `UPDATE` ve `DELETE` arasında sorgular `ProductsOptimisticConcurrency` (hangi iyimser eşzamanlılık kullanır) TableAdapter ve (hangi değil) ürünleri TableAdapter, üzerinde TableAdapter tıklayın ve Özellikler penceresine gidin. İçinde `DeleteCommand` ve `UpdateCommand` özelliklerin `CommandText` alt DAL'ın güncelleştirme veya silme ilgili yöntem çağrıldığında, veritabanına gönderilen gerçek SQL söz dizimi görebilirsiniz. İçin `ProductsOptimisticConcurrency` TableAdapter `DELETE` kullanılan ifadesi:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Oysa `DELETE` ürün TableAdapter bizim özgün DAL içinde bildirimi çok basittir:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Gördüğünüz gibi `WHERE` yan tümcesinde `DELETE` iyimser eşzamanlılık kullanan TableAdapter deyimi içeren her biri arasında bir karşılaştırma `Product` tablo varolan sütun değerlerini ve özgün değerleri zaman GridView (veya DetailsView veya FormView) son doldurulmuş olduğu. Dışındaki tüm alanlar itibaren `ProductID`, `ProductName`, ve `Discontinued` olabilir `NULL` değerleri, ek parametreler ve denetimleri doğru Karşılaştırılacak dahil `NULL` değerler `WHERE` yan tümcesi.

ASP.NET sayfamızı yalnızca güncelleştirme ve silme ürün bilgileri sağlarız gibi biz herhangi bir ek DataTables iyimser eşzamanlılık özellikli veri kümesi için Bu öğretici için ekliyor olmaz. Ancak, yine eklemek ihtiyacımız `GetProductByProductID(productID)` yönteme `ProductsOptimisticConcurrency` TableAdapter.

Bunu gerçekleştirmek için TableAdapter'in başlık çubuğunda sağ tıklayın (yukarıda alanı sağ `Fill` ve `GetProducts` yöntemi adları) ve bağlam menüsünden Sorgu Ekle'i seçin. Bu TableAdapter sorgu Yapılandırma Sihirbazı'nı başlatır. Oluşturmak için bizim TableAdapter'ın ilk yapılandırma ile katılım gibi `GetProductByProductID(productID)` geçici SQL deyimi kullanarak yöntemi (Şekil 4'e bakın). Bu yana `GetProductByProductID(productID)` yöntemi, belirli bir ürünü hakkında bilgi verir, bu sorgu olduğunu gösteren bir `SELECT` sorgu satırları döndürür türü.


[![Sorgu türü olarak işaretlemek bir &quot;satırlar döndüren seçin&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Şekil 9**: sorgu türü olarak işaretlemek bir "`SELECT` satırlar döndüren" ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image27.png))


Sonraki ekranda, önceden yüklenmiş TableAdapter'ın varsayılan sorguyla kullanmak SQL sorgusu için biz istenir. Varolan sorguyu yan tümcesi içerecek şekilde büyütmek `WHERE ProductID = @ProductID`, Şekil 10'da gösterildiği gibi.


[![WHERE eklemek belirli ürün kaydı geri dönmek için önceden yüklenmiş sorguya yan tümcesi](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Şekil 10**: ekleme bir `WHERE` belirli bir ürün kaydı döndürülecek Pre-Loaded sorgu yan tümcesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image30.png))


Son olarak, oluşturulan yöntemi adlarını değiştirmek `FillByProductID` ve `GetProductByProductID`.


[![Yöntemleri FillByProductID ve GetProductByProductID olarak yeniden adlandırın](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Şekil 11**: yöntemlerini yeniden adlandırmak `FillByProductID` ve `GetProductByProductID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image33.png))


Bu sihirbaz tam, TableAdapter şimdi veri almak için iki yöntem içerir: `GetProducts()`, döndüren *tüm* ürünleri; ve `GetProductByProductID(productID)`, belirtilen ürün döndürür.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>3. adım: iyimser eşzamanlılık etkin DAL için bir iş mantığı katmanı oluşturma

Var olan bizim `ProductsBLL` sınıfı toplu güncelleştirme ve DB doğrudan desenleri kullanma örnekleri sahiptir. `AddProduct` Yöntemi ve `UpdateProduct` aşırı hem kullanın tümleştirilmesidir toplu güncelleştirme desen bir `ProductRow` TableAdapter'ın güncelleştirme yöntemi örneği. `DeleteProduct` Yöntemi, diğer yandan TableAdapter's çağırma DB doğrudan deseni kullanır `Delete(productID)` yöntemi.

Yeni `ProductsOptimisticConcurrency` yöntemleri şimdi gerektirir orijinal değerleri de ayrıca geçirilmesi TableAdapter, DB doğrudan. Örneğin, `Delete` yöntemi şimdi bekliyor on giriş parametreleri: özgün `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, ve `Discontinued`. Bu ek giriş parametreleri değerleri kullanan `WHERE` yan tümcesi `DELETE` veritabanının geçerli değerleri özgün olanları kadar eşlerseniz belirtilen kayıt silme yalnızca veritabanına gönderilen ifadesi.

While yöntemi imza TableAdapter için 's `Update` kurmadı toplu güncelleştirme desende kullanılacak yöntemi değiştirilmiş, özgün ve yeni değerleri kaydetmek için gereken kod sahiptir. Bu nedenle, yerine bizim var olan iyimser eşzamanlılık etkin DAL kullanma girişimi `ProductsBLL` sınıfı, bizim yeni DAL ile çalışmak için yeni bir iş mantığı katmanı sınıf oluşturalım.

Adlı bir sınıf ekleyin `ProductsOptimisticConcurrencyBLL` için `BLL` klasör içinde `App_Code` klasör.


![BLL klasörüne ProductsOptimisticConcurrencyBLL sınıfı ekleme](implementing-optimistic-concurrency-cs/_static/image34.png)

**Şekil 12**: eklemek `ProductsOptimisticConcurrencyBLL` BLL klasörüne sınıfı


Sonra aşağıdaki kodu ekleyin `ProductsOptimisticConcurrencyBLL` sınıfı:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Not `NorthwindOptimisticConcurrencyTableAdapters` sınıf bildiriminin başlangıç yukarıda deyimi. `NorthwindOptimisticConcurrencyTableAdapters` Ad alanında `ProductsOptimisticConcurrencyTableAdapter` DAL'ın yöntemleri sağlayan sınıf. Ayrıca sınıf bildirimi önce bulabilirsiniz `System.ComponentModel.DataObject` ObjectDataSource sihirbazın aşağı açılan listede bu sınıf eklemek için Visual Studio bildirir özniteliği.

`ProductsOptimisticConcurrencyBLL`'S `Adapter` özelliği örneği hızlı erişim sağlar `ProductsOptimisticConcurrencyTableAdapter` sınıfı ve özgün bizim BLL sınıflarda kullanılan deseni izler (`ProductsBLL`, `CategoriesBLL`, vb.). Son olarak, `GetProducts()` yöntemi yalnızca çağırır DAL ait `GetProducts()` yöntemi ve döndürür bir `ProductsOptimisticConcurrencyDataTable` nesnesi doldurulmuş ile bir `ProductsOptimisticConcurrencyRow` veritabanındaki her bir ürün kaydı için örneği.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>İyimser eşzamanlılık ile DB doğrudan düzeni kullanarak ürün silme

İyimser eşzamanlılık kullanan bir DAL karşı DB doğrudan deseni kullanırken, yöntemleri yeni ve özgün değerleri geçirilmelidir. Silme için yeni değerler yoktur, böylece yalnızca özgün değerlerine geçirilmesi. Bizim BLL, daha sonra biz tüm özgün parametrelerden biri giriş parametreleri olarak kabul etmeniz gerekir. Şimdi sahip `DeleteProduct` yönteminde `ProductsOptimisticConcurrencyBLL` sınıfı DB doğrudan yöntemi kullanın. Bu, bu yöntem giriş parametresi olarak tüm on ürün veri alanlarında almak ve bunları DAL aşağıdaki kodda gösterildiği gibi geçirmek gerektiği anlamına gelir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Kullanıcı Sil düğmesini tıklattığında veritabanındaki değerlerinden - GridView (veya DetailsView veya FormView) en son yüklenen bu değerleri - özgün değerler farklıysa `WHERE` yan tümcesi olmaz eşleştiğini herhangi bir veritabanı kaydının ve kayıt yok etkilenmez. Bu nedenle, TableAdapter's `Delete` yöntemi döndürür `0` ve BLL's `DeleteProduct` yöntemi döndürür `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Toplu güncelleştirme desen ile iyimser eşzamanlılık kullanarak ürün güncelleştiriliyor

TableAdapter'ın daha önce belirtildiği gibi `Update` yöntemi için toplu güncelleştirme düzeni sahip olup olmadığına iyimser eşzamanlılık işe bakılmaksızın aynı yöntem imzası. Yani, `Update` yöntemi bir DataRow satırının, DataRow, DataTable ya da yazılı DataSet dizisi bekliyor. Orijinal değerleri belirtmek için hiçbir ek giriş parametreleri yok. DataTable özgün ve değiştirilen değerleri için kendi DataRow(s) izler olasıdır. Ne zaman DAL sorunları kendi `UPDATE` deyimi, `@original_ColumnName` parametreleri, DataRow nesnesinin özgün değerlerle doldurulur, ancak `@ColumnName` parametreleri, DataRow nesnesinin değiştirilmiş değerlerle doldurulur.

İçinde `ProductsBLL` (bizim iyimser olmayan, özgün eşzamanlılık DAL kullanır) sınıfını kodumuza aşağıdaki olaylar dizisi gerçekleştirir ürün bilgilerini güncelleştirmek için toplu güncelleştirme düzeni kullanırken:

1. Geçerli veritabanı ürün bilgileri okuyun bir `ProductRow` TableAdapter's kullanarak örnek `GetProductByProductID(productID)` yöntemi
2. Yeni değerler atama `ProductRow` adım 1'den örneği
3. TableAdapter's çağrısı `Update` tümleştirilmesidir yöntemi `ProductRow` örneği

Bu adımlar, ancak dizi doğru iyimser eşzamanlılık olduğundan destek olmaz `ProductRow` içinde doldurulmuş 1. adım, DataRow tarafından kullanılan özgün değerleri, şu anda mevcut olduğu anlamına gelir doğrudan veritabanından doldurulur Veritabanı ve düzenleme işlemi başlangıcında GridView bağlanmış olanlar değil. Kullanarak bir iyimser eşzamanlılık-DAL etkinleştirildiğinde, bunun yerine, alter için ihtiyacımız `UpdateProduct` yöntemi aşırı aşağıdaki adımları kullanın:

1. Geçerli veritabanı ürün bilgileri okuyun bir `ProductsOptimisticConcurrencyRow` TableAdapter's kullanarak örnek `GetProductByProductID(productID)` yöntemi
2. Ata *özgün* değerler `ProductsOptimisticConcurrencyRow` adım 1'den örneği
3. Çağrı `ProductsOptimisticConcurrencyRow` örneğinin `AcceptChanges()` DataRow geçerli değerlerini "özgün" olduğunu bildirir yöntemi
4. Ata *yeni* değerler `ProductsOptimisticConcurrencyRow` örneği
5. TableAdapter's çağrısı `Update` tümleştirilmesidir yöntemi `ProductsOptimisticConcurrencyRow` örneği

Belirtilen ürün kaydı için 1. adım okuma tüm geçerli veritabanı değerleri. Bu adım, gereksiz `UpdateProduct` güncelleştirmeleri aşırı *tüm* ürün sütunların (Bu değerler olarak 2. adımda yazılır), ancak burada yalnızca bir alt kümesini sütun değerlerini içinde olarak geçirilir Bu aşırı yüklemeleri için gereklidir Giriş parametreleri. Özgün değerler atanmış sonra `ProductsOptimisticConcurrencyRow` örneği `AcceptChanges()` yöntemi çağrıldığında, geçerli DataRow değerleri, kullanılacak özgün değerler olarak işaretler `@original_ColumnName` parametrelerinde `UPDATE` deyimi. Ardından, yeni parametre değerlerini atanan `ProductsOptimisticConcurrencyRow` ve son olarak, `Update` yöntemi çağrıldığında, DataRow geçirme.

Aşağıdaki kodda gösterildiği `UpdateProduct` tüm ürün verilerini kabul eden aşırı alanları olarak giriş parametreleri. Burada gösterilmeyen sırada `ProductsOptimisticConcurrencyBLL` de Bu öğretici içermektedir yüklemeye dahil sınıfı bir `UpdateProduct` yalnızca ürün adı ve fiyat giriş parametreleri olarak kabul eden aşırı.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>4. adım: özgün ve yeni değerleri ASP.NET sayfasından BLL yöntemleri geçirme

DAL ve BLL tam kalan tek şey sisteme yerleşik iyimser eşzamanlılık mantığı kullanabileceği ASP.NET sayfası oluşturmak için. Özellikle, Web denetimi (GridView, DetailsView veya FormView) veri özgün değerlerini ve ObjectDataSource iki değer için iş mantığı katmanı geçmelidir unutmamanız gerekir. Ayrıca, ASP.NET sayfası eşzamanlılık ihlalleri kolaylıkla idare edecek yapılandırılması gerekir.

Başlangıç açarak `OptimisticConcurrency.aspx` sayfasındaki `EditInsertDelete` klasörü ve GridView ayarı Designer'a ekleyerek kendi `ID` özelliğine `ProductsGrid`. GridView'ın akıllı etiketten adlı yeni bir ObjectDataSource oluşturmak için kabul `ProductsOptimisticConcurrencyDataSource`. İyimser eşzamanlılık destekleyen DAL kullanmak için bu ObjectDataSource istiyoruz beri kullanacak şekilde yapılandırın `ProductsOptimisticConcurrencyBLL` nesnesi.


[![ObjectDataSource kullanması ProductsOptimisticConcurrencyBLL nesnesi](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Şekil 13**: ObjectDataSource kullanması `ProductsOptimisticConcurrencyBLL` nesne ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image37.png))


Seçin `GetProducts`, `UpdateProduct`, ve `DeleteProduct` Sihirbazı'nda açılan listelerden yöntemleri. UpdateProduct yöntemi için ürünün veri alanlarının tümünü kabul eden kullanın.

## <a name="configuring-the-objectdatasource-controls-properties"></a>ObjectDataSource denetimin özelliklerini yapılandırma

Sihirbazı tamamladıktan sonra ObjectDataSource'nın bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Gördüğünüz gibi `DeleteParameters` koleksiyonu içeren bir `Parameter` her on giriş parametrelerinde örneği `ProductsOptimisticConcurrencyBLL` sınıfının `DeleteProduct` yöntemi. Benzer şekilde, `UpdateParameters` koleksiyonu içeren bir `Parameter` her giriş parametrelerinde örneği `UpdateProduct`.

Veri değişikliği söz konusu bu önceki öğreticiler için biz ObjectDataSource's kaldırmayı tercih `OldValuesParameterFormatString` bu özellik BLL yöntemi geçirilmesi eski (veya özgün) değerlerinin yanı sıra yeni değerleri beklediğini belirtir beri bu noktada özelliği. Ayrıca, bu özellik değeri özgün değerler için giriş parametresi adları gösterir. Özgün değerleri BLL geçiriyoruz olduğundan *değil* bu özelliği kaldırın.

> [!NOTE]
> Değeri `OldValuesParameterFormatString` özelliği özgün değerler beklediğiniz giriş parametresi adları için BLL eşleme gerekir. Biz bu parametreler adlı bu yana `original_productName`, `original_supplierID`ve benzeri bırakabilirsiniz `OldValuesParameterFormatString` özellik değeri olarak `original_{0}`. Ancak, BLL yöntemleri giriş parametreleri gibi adları sahip, `old_productName`, `old_supplierID`ve benzeri güncellemeniz gerekir `OldValuesParameterFormatString` özelliğine `old_{0}`.


ObjectDataSource doğru özgün değerler BLL yöntemlere geçirmek için sırayla yapılmalıdır bir son özellik ayarı yoktur. ObjectDataSource sahip bir ['ınızı özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) için atanabilir [iki değerden birini](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -Varsayılan değer; Özgün değerler BLL yöntemleri özgün giriş parametreleri göndermez
- `CompareAllValues` -Özgün değerler BLL yöntemlere; Gönder İyimser eşzamanlılık kullanırken bu seçeneği seçin

Ayarlamak için bir dakikanızı ayırın `ConflictDetection` özelliğine `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>GridView'in özellikleri ve alanları yapılandırma

Düzgün yapılandırılmış ObjectDataSource'nin özellikleri ile GridView ayarına uygulamamızla dönelim. İlk olarak, düzenleme ve silme desteklemek için GridView istiyoruz beri GridView'ın akıllı etiket gelen düzenlemeyi etkinleştir ve Enable Deleting onay'ı tıklatın. Bu bir CommandField ekler, `ShowEditButton` ve `ShowDeleteButton` her ikisi de ayarlamak `true`.

Ne zaman bağlı `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView içeren bir alan her ürünün veri alanları. Bu tür bir GridView düzenlenebilir olsa da, kullanıcı deneyimini herhangi bir şey ancak kabul edilebilir. `CategoryID` Ve `SupplierID` BoundFields Tedarikçi ve uygun kategoriyi kimlik numaraları girmesini gerektiren kutularındaki sokacak. Olacaktır hiç sayısal alanlar ve ürün adı sağlandı ve birim fiyat stoktaki birimler, birimler sırası ve sipariş düzeyi değerleri üzerinde uygun her iki sayısal değerlerdir ve daha büyük veya eşit emin olmak için hiçbir doğrulama denetimleri için biçimlendirme sıfır olarak.

Biz anlatıldığı gibi *doğrulama denetimleri ekleme düzenleme ve ekleme arabirimleri için* ve *veri değişikliği arabirimi özelleştirme* öğreticileri, kullanıcı arabirimi özelleştirilebilir tarafından BoundFields TemplateFields ile değiştirin. Bu GridView ve düzenleme arabirimiyle aşağıdaki yollarla değiştirdim:

- Kaldırılan `ProductID`, `SupplierName`, ve `CategoryName` BoundFields
- Dönüştürülen `ProductName` bir TemplateField BoundField ve RequiredFieldValidation denetim eklenir.
- Dönüştürülen `CategoryID` ve `SupplierID` TemplateFields, BoundFields ve düzenleme arabirimi metin kutuları yerine DropDownLists kullanmak üzere ayarlanmış. Bu TemplateFields içinde `ItemTemplates`, `CategoryName` ve `SupplierName` veri alanları görüntülenir.
- Dönüştürülen `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` BoundFields TemplateFields ve eklenen CompareValidator kontrol eder.

Biz zaten önceki eğitimlerine bu görevleri gerçekleştirmek üzere nasıl incelenmesi olduğundan, ı yalnızca burada son Tanımlayıcı Sözdizimi listelemek ve uygulama olarak uygulama bırakın.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Tam olarak çalışan bir örnek olması yakın çok çalışıyoruz. Ancak, yedekleme işi ve bize sorunlara neden birkaç subtleties vardır. Ayrıca, hala bir eşzamanlılık ihlali oluştu yükleyen kullanıcının uyarıları bazı arabirimi ihtiyacımız var.

> [!NOTE]
> Doğru özgün değerler (hangi BLL sonra geçirilen) ObjectDataSource geçirilecek sırayla verileri Web denetimi için çok önemli, GridView's `EnableViewState` özelliği ayarlanmış `true` (varsayılan). Görünüm durumu devre dışı bırakırsanız, özgün değerler geri göndermede kaybolur.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>ObjectDataSource doğru orijinal değerleri geçirme

Birkaç GridView yapılandırılmış şekilde sorunları vardır. Varsa ObjectDataSource's `ConflictDetection` özelliği ayarlanmış `CompareAllValues` (olduğu gibi bizim), ne zaman ObjectDataSource's `Update()` veya `Delete()` yöntemleri GridView (veya DetailsView veya FormView) çağrılır, ObjectDataSource kopyalamayı dener GridView'ın özgün değerlere kendi uygun `Parameter` örnekleri. Geri bu işlem için bir grafik gösterimi Şekil 2'ye bakın.

Özellikle, GridView'ın özgün değerler her zaman veri GridView bağlı iki yönlü veri bağlaması deyimlerinde değerler atanır. Bu nedenle, gerekli tüm gerekli özgün değerler ile iki yönlü veri bağlaması yakalanır ve dönüştürülebilir biçiminde sağlanır.

Neden bu önemli olduğunu görmek için tarayıcıda sayfamızı ziyaret etmek için bir dakikanızı ayırın. GridView, beklendiği gibi bir düzenleme ve silme düğmesi en soldaki sütunda her ürünle listeler.


[![Ürünler GridView içinde listelenir](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Şekil 14**: ürünleri GridView içinde listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image40.png))


Herhangi bir ürünü için de Sil düğmesini tıklatın, bir `FormatException` oluşturulur.


[![Herhangi bir FormatException ürün sonuçlarında silinmeye çalışılıyor](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Şekil 15**: Sil herhangi ürün sonuçlarında çalışılırken bir `FormatException` ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image43.png))


`FormatException` ObjectDataSource özgün okuma girişiminde bulunduğunda tetiklenir `UnitPrice` değeri. Bu yana `ItemTemplate` sahip `UnitPrice` para birimi olarak biçimlendirilmiş (`<%# Bind("UnitPrice", "{0:C}") %>`), $19.95 gibi bir para birimi simgesini içerir. `FormatException` Bu dizeye dönüştürmek ObjectDataSource denediğinde oluşur bir `decimal`. Bu sorunu gidermek için biz dizi seçeneğiniz vardır:

- Biçimlendirmeyi para birimi kaldırmak `ItemTemplate`. Diğer bir deyişle, yerine `<%# Bind("UnitPrice", "{0:C}") %>`, yalnızca kullanmak `<%# Bind("UnitPrice") %>`. Bu dezavantajı, fiyat artık biçimlendirildiğinden emin olur.
- Görüntü `UnitPrice` bir para birimi olarak biçimlendirilmiş `ItemTemplate`, ancak `Eval` bunu gerçekleştirmek için anahtar sözcüğü. Sözcüğünün `Eval` tek yönlü databinding gerçekleştirir. Hala sağlamak ihtiyacımız `UnitPrice` nedenle yapmamız hala bir iki yönlü veri bağlaması deyiminde gereken özgün değerler için değer `ItemTemplate`, ancak bu bir etiket Web denetimi özelliği yerleştirilebilir `Visible` özelliği ayarlanmış `false`. Aşağıdaki biçimlendirmede ItemTemplate kullanıyoruz:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Biçimlendirmeyi para birimi kaldırmak `ItemTemplate`kullanarak `<%# Bind("UnitPrice") %>`. GridView's içinde `RowDataBound` olay işleyicisi, program aracılığıyla erişim etiket Web denetim içinde `UnitPrice` değeri görüntülenir ve ayarlama kendi `Text` biçimlendirilmiş sürüme özelliği.
- Bırakın `UnitPrice` para birimi olarak biçimlendirilmiş. GridView's içinde `RowDeleting` olay işleyicisi varolan özgün Değiştir `UnitPrice` değeri ($19.95) kullanarak bir gerçek ondalık değer ile `Decimal.Parse`. Benzer bir şey nasıl gördüğümüz `RowUpdating` olay işleyicisini [ *işleme BLL - ve ASP.NET sayfası DAL düzeyi durumlar* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) Öğreticisi.

İkinci yaklaşımda, Git seçtiğim my örneğin gizli etiket Web ekleme, denetim özelliği `Text` iki yönlü veri biçimlendirilmemiş için bağlı bir özelliktir `UnitPrice` değeri.

Bu sorunun çözümüne sonra herhangi bir ürün için de Sil düğmesini yeniden tıklatarak deneyin. Bu süre, alacaksınız bir `InvalidOperationException` ObjectDataSource çalıştığında BLL's çağrılacak `UpdateProduct` yöntemi.


[![ObjectDataSource Gönder istediği giriş parametreleri olan bir yöntem bulunamadı.](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Şekil 16**: ObjectDataSource Gönder istediği giriş parametreleri olan bir yöntem bulunamıyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image46.png))


Özel durumun ileti baktığınızda, onu ObjectDataSource bir BLL çağrılacak istediği işaretlenmemiştir `DeleteProduct` içeren yöntem `original_CategoryName` ve `original_SupplierName` giriş parametreleri. Bunun nedeni, `ItemTemplate` s için `CategoryID` ve `SupplierID` TemplateFields şu anda iki yönlü bağlama deyimleri içeren `CategoryName` ve `SupplierName` veri alanları. Bunun yerine, dahil etmek ihtiyacımız `Bind` deyimleri `CategoryID` ve `SupplierID` veri alanları. Bunu başarmak için var olan bağlama deyimleri ile değiştirin `Eval` deyimleri ve gizli etiket denetimleri ekleme `Text` özellikleri bağlı `CategoryID` ve `SupplierID` veri alanları gösterildiği gibi iki yönlü veri bağlamasını kullanma Aşağıda:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Bu değişikliklerle biz şimdi başarıyla silin ve ürün bilgileri düzenleyebilir! Adım 5'te biz eşzamanlılık ihlalleri algılandığını doğrulamak ne göreceğiz. Ancak şu an için güncelleştirmeyi deneyin için birkaç dakika sürebilir ve bu güncelleştirme ve silme için tek bir kullanıcı emin olmak için birkaç kayıtları silme beklendiği gibi çalışır.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>5. adım: iyimser eşzamanlılık destek test etme

Eşzamanlılık ihlalleri algılanan (yerine doğrudan üzerine yazmaya verilerdeki elde edilen) doğrulamak için Biz bu sayfaya iki tarayıcı pencerelerini açmanız gerekir. Her iki tarayıcı durumlarda ayrıntılarını Düzenle düğmesine tıklayın. Ardından, yalnızca bir tarayıcılar, "Chai Çay" adı değiştirin ve Güncelleştir'i tıklatın. Güncelleştirme başarılı ve GridView yeni ürün adı olarak "Chai Çay" ile önceden düzenleme durumuna geri dönün.

Diğer tarayıcı penceresini örneğinde, ancak, ürün adı metin kutusuna "Chai" görüntülenmeye devam eder. İkinci bir tarayıcı penceresi içinde güncelleştirme `UnitPrice` için `25.00`. İyimser eşzamanlılık desteği olmadan ikinci tarayıcı örneğinde Güncelleştir'i tıklatarak ürün adı "böylece ilk tarayıcı örneği tarafından yapılan değişikliklerin üzerine yazarak geri Chai", için değiştirirsiniz. İşe iyimser eşzamanlılık ile ancak ikinci tarayıcı örneği Güncelleştir düğmesini tıklatarak sonuçlanan bir [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Bir eşzamanlılık ihlali algılandığında bir DBConcurrencyException oluşturulur](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Şekil 17**: bir eşzamanlılık ihlali algılanırsa, bir `DBConcurrencyException` atılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image49.png))


`DBConcurrencyException` DAL'ın toplu güncelleştirme düzeni kullanılırken yalnızca atılır. DB doğrudan düzeni bir özel durum oluşturmaz, yalnızca hiçbir satır etkilendi gösterir. Bunu göstermek için her iki tarayıcı örnekleri GridView önceden düzenleme durumlarına geri dönün. Ardından, ilk tarayıcı örneğinde Düzenle düğmesini tıklatın ve "Chai" dön "Chai Çay" ürün adını değiştirmek ve Güncelleştir'i tıklatın. İkinci tarayıcı penceresinde ayrıntılarını Sil düğmesini tıklatın.

Delete tuşuna basarak, bağlı sayfaya geri gönderir, GridView ObjectDataSource's çağırır `Delete()` yöntemi ve ObjectDataSource çağırır içine `ProductsOptimisticConcurrencyBLL` sınıfının `DeleteProduct` boyunca özgün değerleri geçirme yöntemi. Özgün `ProductName` ikinci tarayıcı örneği "Chai hangi geçerli eşleşmiyor Çay", değeri `ProductName` değeri veritabanındaki değer. Bu nedenle `DELETE` veritabanına verilen deyimi etkiler sıfır satır veritabanında hiçbir kayıt olduğundan, `WHERE` yan tümcesi karşılar. `DeleteProduct` Yöntemi döndürür `false` ve ObjectDataSource'nın veri GridView DataSet'e bağlanır.

Son kullanıcının açısından, flash ekranın neden ikinci bir tarayıcı penceresinde Chai Çay için Sil düğmesini tıklatarak ve, şimdi "Chai" (ilk tarayıcı tarafından yapılan ürün adı değişikliği olarak listeleniyor ancak geri gelmelerini bağlı ürün hala, yok Örnek). Kullanıcı yeniden Sil düğmesine tıklarsa, GridView özgün olarak silme başarılı `ProductName` değeri ("Chai") artık eşleştiğini değeri veritabanındaki değer ile.

Her iki durumda, kullanıcı deneyimini gölgeden uzak idealdir. Biz açıkça kullanıcı nitty-gritty ayrıntılarını göster istemediğiniz `DBConcurrencyException` toplu güncelleştirme düzeni kullanırken, özel durum. Ve DB doğrudan düzeni kullanırken davranış kullanıcılar komutu başarısız oldu, ancak kesin bir gösterge neden olmadan vardı biraz karmaşıktır.

Bu iki sorunları çözmek için bir güncelleştirme veya silme işlemi başarısız oldu neden için bir açıklama sağlaması sayfasında etiket Web denetimleri oluşturabilirsiniz. Toplu güncelleştirme düzeni için belirleriz olsun veya olmasın bir `DBConcurrencyException` GridView'ın sonrası düzeyi olay işleyicisinde özel durum oluştu uyarı etiketi gerektiği gibi görüntüleme. DB doğrudan yöntemi için biz BLL yönteminin dönüş değeri inceleyebilirsiniz (olduğu `true` bir satır etkilenen, `false` aksi) ve gerektiği şekilde bir bilgilendirme iletisi görüntüler.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>6. adım: Bilgilendirici iletileri ekleme ve eşzamanlılık ihlali karşısında görüntüleme

Bir eşzamanlılık ihlali oluştuğunda sergilenen davranışı olup DAL'ın toplu güncelleştirme veya DB doğrudan desen kullanılan üzerinde bağlıdır. Öğreticimizi güncelleştirme ve silme için kullanılan DB doğrudan düzeni için kullanılan toplu güncelleştirme düzendeki iki desen kullanır. Başlamak için iki etiket Web denetimleri bir eşzamanlılık ihlali veri güncelleştirmek ya da silme girişimi sırasında oluştu açıklayan sayfamızı ekleyelim. Etiket denetimin ayarlamak `Visible` ve `EnableViewState` özelliklerine `false`; bu bunları nereye olanlar için belirli sayfasını ziyaret dışında her sayfasını ziyaret edin gizli olarak neden olacak kendi `Visible` özelliği ayarlanmış program aracılığıyla `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Ayar yanı sıra bunların `Visible`, `EnabledViewState`, ve `Text` özelliklerini ayrıca ayarlarım `CssClass` özelliğine `Warning`, büyük, kırmızı, italik, kalın yazı tipiyle görüntülenecek etiket neden olan kullanıcının. Bu CSS `Warning` sınıfı tanımlanan ve Styles.css için eklenen geri *ekleme, güncelleştirme ve silme ile ilişkili olay incelenerek* Öğreticisi.

Bu etiketler eklendikten sonra Visual Studio tasarımcıda Şekil 18'e benzer görünmelidir.


[![İki etiket denetimleri sayfasına eklenmiştir](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Şekil 18**: iki etiket denetimleri eklenmiştir sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image52.png))


Bu etiket Web kontroller varken, biz ne zaman bir eşzamanlılık ihlali, en uygun etiketin işaret oluştu belirleme incelemek hazır `Visible` özelliği ayarlanabilir `true`, bilgi iletisini görüntüleme.

## <a name="handling-concurrency-violations-when-updating"></a>Eşzamanlılık ihlalleri güncelleştirirken işleme

İlk önce toplu güncelleştirme düzeni kullanırken eşzamanlılık ihlalleri nasıl ele alınacağını konumundaki bakalım. Böyle ihlalleri yığın düzeni neden güncelleştirme bu yana bir `DBConcurrencyException` durum için özel durum kodu belirlemek için ASP.NET sayfamızı eklemek ihtiyacımız olup bir `DBConcurrencyException` güncelleştirme işlemi sırasında özel durum oluştu. Bu nedenle, biz kaydını düzenleme başlatıldığında başka bir kullanıcı arasında aynı veri değiştirdiğinden, yaptıkları değişiklikleri kaydedilmedi kullanıcı açıklayan bir ileti görüntülenmelidir ve güncelleştirme düğme tıklatıldığında.

İçinde gördüğümüz gibi *işleme BLL - ve ASP.NET sayfası DAL düzeyi durumlar* benzer özel durumların algılanabilir ve veri Web denetimin sonrası düzeyi olay işleyicileri gizlenen Öğreticisi. Bu nedenle, olay işleyici GridView için 's oluşturmak ihtiyacımız `RowUpdated` denetler olay bir `DBConcurrencyException` özel durum. Bu olay işleyicisi güncelleştirme işlemi sırasında başlatılan özel durumları başvuru olay işleyici kod aşağıda gösterildiği gibi geçirilir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

Face, bir `DBConcurrencyException` özel durum, bu olay işleyicisi görüntüler `UpdateConflictMessage` etiket denetimini ve özel durumun işlenip olduğunu gösterir. Bir eşzamanlılık ihlali bir kaydı güncelleştirirken oluştuğunda başka bir kullanıcının değişiklikleri aynı anda üzerlerine beri yerinde bu kodu ile kullanıcının değişiklikler, kaybolur. Özellikle, GridView önceden düzenleme durumuna geri döndürülen ve geçerli veritabanı veriye bağlı. Bu, daha önce görünür diğer kullanıcının değişiklikleriyle GridView satır güncelleştirir. Ayrıca, `UpdateConflictMessage` etiket denetimi açıklamak için kullanıcı yalnızca ne. Bu olaylar dizisi şekil 19'ayrıntılı olarak gösterilmiştir.


[![Bir kullanıcı s güncelleştirmeleri bir eşzamanlılık ihlali karşılaştıkları kayboluyor](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Şekil 19**: bir kullanıcı s güncelleştirmeleri bir eşzamanlılık ihlali karşılaştıkları kayboluyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Alternatif olarak, GridView önceden düzenleme durumuna döndürme yerine, biz GridView düzenleme durumundayken ayarlayarak bırakabilir `KeepInEditMode` geçilen özelliğinin `GridViewUpdatedEventArgs` nesne true. Bu yaklaşımı benimsemeniz durumunda, ancak GridView verileri rebind emin olun (çağırma tarafından kendi `DataBind()` yöntemi) böylece diğer kullanıcının değerleri düzenleme arabirimin içine yüklenir. Bu iki satır kod Bu öğretici ile İndirilebilecek koduna sahip `RowUpdated` olay işleyicisi geçersiz kılınan çıkışı; yalnızca bu GridView sağlamak için kod satırları düzenleme modunda bir eşzamanlılık ihlali sonra kalır açıklamadan çıkarın.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Eşzamanlılık ihlallerini silerken yanıt

DB doğrudan deseni, bir eşzamanlılık ihlali karşısında gerçekleştirilen herhangi bir özel durumu yok. Bunun yerine database deyimi yalnızca WHERE yan tümcesi ile herhangi bir kaydının eşleşmiyor olarak bağlı olarak, hiçbir kayıt etkiler. Bunlar tam olarak bir kayıt etkilenen olup olmadığını gösteren bir Boole değeri döndürür, tüm BLL oluşturulan veri değişikliği yöntemleri tasarlanmıştır. Bu nedenle, bir eşzamanlılık ihlali bir kaydı silinirken oluştu belirlemek için biz BLL'ın dönüş değeri inceleyebilirsiniz `DeleteProduct` yöntemi.

ObjectDataSource'nın sonrası düzeyi olay işleyicileri BLL yönteminin dönüş değeri incelenebilir `ReturnValue` özelliği `ObjectDataSourceStatusEventArgs` nesne olay işleyicisi geçirildi. Biz yönteminden döndürülen değer belirlerken ilgilendiğiniz beri `DeleteProduct` yöntemi, ihtiyacımız ObjectDataSource için 's olay işleyici oluşturmak `Deleted` olay. `ReturnValue` Özelliği türüdür `object` ve `null` bir özel durumu oluştu ve yöntemi bir değer döndürebilirsiniz önce kesildi. Bu nedenle, biz öncelikle emin `ReturnValue` özelliği `null` ve bir Boole değeri. Bu onay geçirir, gösteriyoruz varsayılarak `DeleteConflictMessage` Denetim etiketi `ReturnValue` olan `false`. Bu, aşağıdaki kodu kullanarak gerçekleştirilebilir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

Bir eşzamanlılık ihlali karşısında kullanıcının silme isteği iptal edildi. GridView, sayfa ve Sil düğmesine tıklandığında kendisinin arasındaki süre, kayıt için kullanıcı ortaya çıkan değişiklikleri yüklenen gösteren bir yenilenir. Bu tür bir ihlali transpires, `DeleteConflictMessage` etiket gösterilir, (bkz. Şekil 20) ne yalnızca açıklayan.


[![Bir kullanıcı s Delete karşısında bir eşzamanlılık ihlali iptal edildi](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Şekil 20**: Delete karşısında bir eşzamanlılık ihlali iptal bir kullanıcı s ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Özet

Eşzamanlılık ihlalleri olanaklarını mevcut birden çok, her uygulamada güncelleştirmek veya veri silmek için eşzamanlı kullanıcı. Bu tür ihlalleri iki kullanıcı aynı anda aktaranın son yazma "WINS'de" alır aynı veri güncelleştirdiğinizde, diğer kullanıcının üzerine değişiklikleri değişiklikleri için hesaba değil Alternatif olarak, geliştiricilerin ya da iyimser veya kötümser eşzamanlılık denetimi uygulayabilirsiniz. İyimser eşzamanlılık denetimini eşzamanlılık ihlalleri daha azdır ve yalnızca bir güncelleştirme izin vermez veya bir eşzamanlılık ihlali oluşturur komutu silme olduğunu varsayar. Eşzamanlılık denetimi ihlalleri sık ve yalnızca bir kullanıcının reddetme update veya delete komutu bu eşzamanlılık kabul edilebilir değil varsayar. Eşzamanlılık denetimi ile böylece değiştirme veya kilitliyken kayıt silme herhangi diğer kullanıcıların önleme kilitleme bir kaydın güncelleştirilmesini gerektirir.

.NET içinde yazılan veri kümesi iyimser eşzamanlılık denetimini desteklemek için işlevsellik sağlar. Özellikle, `UPDATE` ve `DELETE` deyimleri veritabanına verilen tüm tablonun sütunlarının içerir, böylece kaydın geçerli veriler özgün verilerle kullanıcı eşleşiyorsa, update veya delete yalnızca oluşacağını sağlar, sahip olduğunda kendi update veya delete gerçekleştiriliyor. DAL iyimser eşzamanlılık desteklemek üzere yapılandırılmış sonra BLL yöntemleri güncelleştirilmesi gerekir. Ayrıca, BLL çağıran ASP.NET sayfası ObjectDataSource kendi veri Web denetiminden özgün değerlerini alır ve BLL geçirir şekilde yapılandırılmalıdır.

Biz bu öğreticide gördüğünüz gibi bir ASP.NET web uygulamasında iyimser eşzamanlılık denetimini uygulama DAL ve BLL güncelleştirme ve ASP.NET sayfasında desteği ekleme içerir. Olsun veya olmasın bu eklenen akıllıca yatırımı zaman ve çaba iştir, uygulamaya bağlı olarak değişir. Verileri güncelleştirme eşzamanlı kullanıcı seyrek sahip ya da güncelleştirmekte olduğunuz veri birbirinden farklı ise, ardından eşzamanlılık denetimi bir anahtar sorun değildir. Ancak, düzenli olarak birden çok kullanıcı aynı verilerle çalışma sitenizdeki varsa, eşzamanlılık denetimi başka birinin üzerine farkında olmadan bir kullanıcının güncelleştirme ve silme önlenmesine yardımcı olabilir.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](customizing-the-data-modification-interface-cs.md)
> [sonraki](adding-client-side-confirmation-when-deleting-cs.md)
