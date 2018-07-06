---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: İyimser eşzamanlılık (VB) uygulama | Microsoft Docs
author: rick-anderson
description: Birden çok kullanıcı verilerini düzenlemesini sağlayan bir web uygulaması için iki kullanıcı aynı verileri aynı anda düzenliyor olmanız riski yoktur. İçinde bu tutori...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 63b5a274103851b4b60c92d5fe46125cc4a1b0be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832972"
---
<a name="implementing-optimistic-concurrency-vb"></a>İyimser eşzamanlılık uygulama (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) veya [PDF olarak indirin](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Birden çok kullanıcı verilerini düzenlemesini sağlayan bir web uygulaması için iki kullanıcı aynı verileri aynı anda düzenliyor olmanız riski yoktur. Bu öğreticide size bu riski işlemek için iyimser eşzamanlılık denetimi uygulayacaksınız.


## <a name="introduction"></a>Giriş

Yalnızca kullanıcıların veri görüntülemesini sağlayan web uygulamaları için ya da, yalnızca verileri değiştirebilir bir tek kullanıcı eklemek için hiçbir tehdit yanlışlıkla başka birinin değişikliklerin üzerine iki eş zamanlı kullanıcı yoktur. Güncelleştirme veya veri silme birden çok kullanıcının web uygulamaları için ancak yoktur olası bir kullanıcının değişiklikleri başka bir eş zamanlı kullanıcının ile çakışır. İki kullanıcı aynı anda tek bir kaydı düzenleme yaparken herhangi bir yerde eşzamanlılık ilke değişiklikleri işlemeler kullanıcının son ilk tarafından yapılan değişiklikleri geçersiz kılar.

Örneğin, Jisun ve Vedat, iki kullanıcı hem de uygulamamızdaki ziyaretçiler, güncelleştirme ve silme ürünler aracılığıyla bir GridView denetimi izin verilen bir sayfasını ziyaret düşünün. Her ikisi de yaklaşık aynı zamanda GridView Düzenle düğmesine tıklayın. Jisun "Chai Çay" için ürün adını değiştirir ve güncelleştir düğmesine tıklar. Net sonucu olan bir `UPDATE` ayarlar veritabanına gönderilen bildirimi *tüm* ürünün güncelleştirilebilir alanlarının (Jisun yalnızca bir alan güncelleştirilmiş olmasa bile `ProductName`). Bu anda, veritabanı değerleri "Chai Çay", İçecekler, vb. belirli bu ürün için Exotic Liquids sağlayıcı kategorisi vardır. Ancak Vedat'ın ekranında GridView yine de ürün adı düzenlenebilir GridView satırında "Chai" gösterilir. Birkaç saniye Jisun'ın değişiklikleri işlendikten sonra Sam kategorisi için Çeşniler güncelleştirir ve güncelleştirme tıklar. Sonuçlanır bir `UPDATE` "Chai," ürün adına ayarlar veritabanına gönderilen deyimi `CategoryID` karşılık gelen İçecekler kategori kimliği ve benzeri. Ürün adı Jisun'ın değişikliklerin üzerine yazıldı. Şekil 1, grafik bu bir dizi olayı gösterilmektedir.


[![Ne zaman iki kullanıcı bir kayıt var. s olası bir kullanıcının değişiklikleri diğer tarafın üzerine yazmak için eşzamanlı olarak güncelleştirin](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Şekil 1**: ne zaman iki kullanıcı aynı anda güncelleştirme bir kaydı var. s olasılığı diğer tarafın üzerine yazma için bir kullanıcının değişiklikleri için ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image3.png))


Benzer şekilde, bir sayfa iki kullanıcıların ziyaret ettiği, bir kullanıcı başka bir kullanıcı tarafından silindiğinde bir kaydı güncelleştirme ortasında olabilir. Veya, bir kullanıcı bir sayfa yüklediğinde ve Sil düğmesine tıkladığınızda arasında başka bir kullanıcı, kaydın içeriğini değiştirilmiş olabilir.

Üç [eşzamanlılık denetimi](http://en.wikipedia.org/wiki/Concurrency_control) stratejileri kullanılabilir:

- **Hiçbir şey yapma** -eş zamanlı kullanıcı aynı kaydın değiştiriyorsanız, son kaydetme (varsayılan davranış) win olanak tanır.
- [**İyimser eşzamanlılık** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -eşzamanlılık çakışmaları every zorunluluğu, bu gibi çakışmaları ortaya çıkan olmaz; zaman büyük çoğunluğu olsa da olabilir bu nedenle, bir çakışma oluşursa, yalnızca hakkında bilgilendiren kullanıcı varsayılır, yaptıkları değişiklikleri aynı verileri başka bir kullanıcı tarafından değiştirildiğinden kaydedilemiyor
- **Kötümser eşzamanlılık** -eşzamanlılık çakışmaları olan sıradan bir hale ve yaptıkları değişiklikleri nedeniyle eş zamanlı etkinlik başka bir kullanıcının kayıtlı olmayan kullanıcılar durdurulmasını tolere olmaz söyledi varsayılır; Bu nedenle, bir kullanıcı bir kaydı güncelleştirme başladığında Kilitle , böylece düzenlemek veya kullanıcı kendi değişiklikleri işleme kadar o kaydı silmek diğer tüm kullanıcılar önleme

Tüm öğreticilerimizden şimdiye kadarki varsayılan eşzamanlılık çözümleme stratejisini kullanmış - yani biz win son yazma bildirdiniz. Bu öğreticide iyimser eşzamanlılık denetimi uygulamak nasıl inceleyeceğiz.

> [!NOTE]
> Bu öğretici serisinin kötümser eşzamanlılık örneklerde şu konuları olmaz. Kötümser eşzamanlılık gibi tarafından kilitlendiği için nadiren kullanılan yoksa düzgün relinquished, diğer kullanıcıların veri güncelleştirilmesini engelleyebilir. Örneğin, başka bir kullanıcı bir kullanıcı bir kaydı düzenlemek için kilitler ve bu kilit açma önce gün sonra bırakır, özgün kullanıcıya döndürür ve kendi güncelleştirme tamamlanana kadar bu kaydı güncelleştirmek mümkün olacaktır. Bu nedenle, kötümser eşzamanlılık kullanıldığı durumlarda, var. tipik ulaştıysanız, kilit iptal eden bir zaman aşımı Kullanıcı sipariş işlemi tamamlanırken kısa bir süre için belirli katılımcı Konumu Kilitle bilet satış Web sitelerinin, kötümser eşzamanlılık denetimi örneğidir.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>1. adım: Nasıl iyimser eşzamanlılık arayan uygulanır

İyimser eşzamanlılık denetimi, güncelleştirme veya silme işlemi başlatıldığında gibi kayıt güncelleştirildiğinde veya silindiğinde değerlerin aynı olduğundan emin olarak çalışır. Örneğin, düzenlenebilir bir GridView Düzenle düğmesine tıklandığında, kaydın değerleri veritabanından okunur ve metin kutuları ve diğer Web denetimleri görüntülenir. Bu özgün değerlerine GridView tarafından kaydedilir. Kullanıcı değişiklikleri yapar ve güncelleştir düğmesine tıkladığında sonra daha sonra yeni değerlerin toplamı orijinal değerleri iş mantığı katmanı ve veri erişim katmanına gönderilir. Veri erişim katmanı, veritabanı değerlerde yine de kullanıcı düzenlemeye başladığını orijinal değerleri aynıysa, yalnızca kaydı güncelleştirecek bir SQL deyimi yayımlamanız gerekir. Şekil 2, bu olayların sırasını gösterir.


[![Update veya Delete için başarılı olması, orijinal değerleri geçerli veritabanı değerlere eşit olmalıdır](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Şekil 2**: For Update veya Delete Succeed, özgün değer gerekir olması eşit geçerli veritabanı değerlerini ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image6.png))


İyimser eşzamanlılık uygulama çeşitli yaklaşımları vardır (bkz [Peter A. Bromberg](http://peterbromberg.net/)'s [Optmistic eşzamanlılık güncelleştirme mantığı](http://www.eggheadcafe.com/articles/20050719.asp) birçok seçenek kısa göz atmak için). ADO.NET türü belirtilmiş veri kümesi yalnızca bir onay kutusu değer çizgisi ile yapılandırılmış bir uygulamasını sağlar. TableAdapter bağdaştırıcısının türü belirtilmiş veri kümesinde bir TableAdapter artırmaktadır için iyimser eşzamanlılık etkinleştirilirken `UPDATE` ve `DELETE` özgün değerleri bir karşılaştırmasını içerecek şekilde deyimleri `WHERE` yan tümcesi. Aşağıdaki `UPDATE` deyimi, örneğin, güncelleştirmeleri adı ve ürünün fiyatı yalnızca geçerli veritabanı değerler GridView kaydında güncelleştirirken ilk olarak alınan değerlerle eşitse. `@ProductName` Ve `@UnitPrice` parametreleri ise kullanıcı tarafından girilen yeni değerleri içeren `@original_ProductName` ve `@original_UnitPrice` Düzenle düğmesine tıklandığında GridView yüklenen ilk değerleri içerir:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Bu `UPDATE` deyimi okunabilirlik için basitleştirilmiştir. Uygulamada, `UnitPrice` iade `WHERE` yan yana daha karmaşık olacaktır `UnitPrice` içerebilir `NULL` s ve denetimi if `NULL = NULL` her zaman False döndürür (Bunun yerine kullanmalısınız `IS NULL`).


Farklı bir arka plandaki kullanmanın yanı sıra `UPDATE` iyimser eşzamanlılık de kendi DB imzası değiştirir kullanmak için bir TableAdapter yapılandırma deyimi, doğrudan yöntemler. Geri çağırma ilk öğreticimize [ *veri erişim katmanını oluşturma*](../introduction/creating-a-data-access-layer-cs.md), giriş olarak parametre değerleri DB doğrudan yöntemler, skaler bir listesini kabul eder olduğunu (kesin türü belirtilmiş bir DataRow yerine veya DataTable örneği). İyimser eşzamanlılık, doğrudan DB kullanırken `Update()` ve `Delete()` yöntemleri orijinal değerleri de için giriş parametrelerini içerir. Ayrıca, batch kullanımından BLL kodda güncelleştirme desen ( `Update()` DataRow DataTables yerine ve skaler değerler kabul yöntemi aşırı yüklemeleri) de değiştirilmesi gerekir.

Bunun yerine bizim mevcut BT'nizi genişletin çok DAL'ın TableAdapter'ları (hangi uyum sağlayacak şekilde BLL değiştirmek istediğinde) iyimser eşzamanlılık kullanalım bunun yerine yeni yazılan adlı veri kümesi oluşturma `NorthwindOptimisticConcurrency`, hangi ekleyeceğiz için bir `Products` TableAdapter, İyimser eşzamanlılık kullanır. Oluşturacağımız bir `ProductsOptimisticConcurrencyBLL` DAL iyimser eşzamanlılığı desteklemek için uygun değişiklikleri olan iş mantığı katmanı sınıfı. Bu yapıyı düzenlenir sonra size ASP.NET sayfası oluşturmak hazır olacaksınız.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>2. adım: iyimser eşzamanlılık destekleyen veri erişim katmanını oluşturma

Yeni bir türü belirtilmiş veri kümesi oluşturmak için sağ `DAL` klasördeki `App_Code` klasörü ve adlı yeni bir veri kümesi Ekle `NorthwindOptimisticConcurrency`. İlk öğreticide gördüğümüz gibi Bunun yapılması yeni bir TableAdapter yazılan otomatik olarak TableAdapter Yapılandırma Sihirbazı Başlatılıyor veri kümesine, bu nedenle ekler. İlk ekranda, biz bağlanma - aynı Northwind veritabanı kullanarak bağlanmak için bir veritabanı belirtmeniz istenir `NORTHWNDConnectionString` ayarını `Web.config`.


[![Aynı Northwind veritabanına bağlanma](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Şekil 3**: aynı Northwind veritabanına bağlanma ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image9.png))


Ardından, verileri sorgulamak nasıl kullanılacağına biz istenir: geçici SQL deyimi, yeni bir saklı yordam veya varolan bir saklı yordam. Geçici SQL sorguları içinde bizim orijinal DAL kullandığından bu seçeneği burada da kullanın.


[![Geçici SQL deyimi kullanarak almak için verileri belirtin](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Şekil 4**: geçici SQL deyimi kullanarak almak için verileri belirtin ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image12.png))


Aşağıdaki ekranda, ürün bilgilerini almak için kullanılacak SQL sorgusunu girin. İçin kullanılan aynı tam SQL sorgusunu kullanalım `Products` TableAdapter gelen tüm döndürür bizim orijinal DAL `Product` ürünün Tedarikçi ve kategori adları ile birlikte sütunlar:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]


[![Özgün DAL aynı ürün TableAdapter SQL sorgudan kullanın](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Şekil 5**: gelen aynı SQL sorgusu kullanmanın `Products` özgün DAL, TableAdapter ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image15.png))


Sonraki ekrana taşımadan önce Gelişmiş Seçenekler düğmesine tıklayın. Bu TableAdapter kullanan iyimser eşzamanlılık denetimi için başka bir işlem yalnızca "iyimser eşzamanlılık kullan" onay kutusunu işaretleyin.


[![İyimser eşzamanlılık denetimi denetimi tarafından etkinleştir &quot;iyimser eşzamanlılığı kullanın&quot; onay kutusu](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Şekil 6**: etkinleştirme iyimser eşzamanlılık "iyimser eşzamanlılık kullan" onay kutusunu işaretleyerek denetimi ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image18.png))


Son olarak, TableAdapter, bir DataTable Doldur hem bir DataTable Döndür veri erişim desenlerini kullanması gerektiğini belirtirsiniz; aynı zamanda DB doğrudan yöntemler oluşturulması gerektiğini belirtir. İade için yöntem adını bir DataTable deseni GetData GetProducts, bizim orijinal DAL kullandığımız adlandırma kuralları yansıtmak için değiştirin.


[![Tüm veri erişim desenlerini yazılımınız TableAdapter sahip](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Şekil 7**: TableAdapter kullanan tüm veri erişim desenlerini sahip ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image21.png))


Sihirbazı tamamladıktan sonra veri kümesi Tasarımcısı türü kesin belirlenmiş bir içerecektir `Products` DataTable ve TableAdapter. DataTable nesnesinden yeniden adlandırmak için birkaç dakikanızı `Products` için `ProductsOptimisticConcurrency`, DataTable'nın başlık çubuğuna sağ tıklayıp bağlam menüsünden yeniden adlandır seçerek yapabilirsiniz.


[![Bir DataTable ve TableAdapter türü belirtilmiş veri kümesine eklendi](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Şekil 8**: bir DataTable ve TableAdapter türü belirtilmiş veri kümesi eklenmiştir ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image24.png))


Arasındaki farkları görmek için `UPDATE` ve `DELETE` arasında sorgular `ProductsOptimisticConcurrency` (hangi iyimser eşzamanlılık kullanır) TableAdapter ve (hangi değil) ürünleri TableAdapter, TableAdapter bağdaştırıcısında tıklayın ve Özellikler penceresine gidin. İçinde `DeleteCommand` ve `UpdateCommand` özelliklerin `CommandText` alt DAL'ın update veya delete ilgili yöntemler çağrıldığında veritabanına gönderilen gerçek SQL söz dizimi görebilirsiniz. İçin `ProductsOptimisticConcurrency` TableAdapter `DELETE` kullanılan deyimidir:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Oysa `DELETE` deyimi ürün nda TableAdapter bağdaştırıcısının özgün bizim DAL için çok basittir:


[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Gördüğünüz gibi `WHERE` yan tümcesinde `DELETE` deyimi için iyimser eşzamanlılık kullanan TableAdapter her biri arasında bir karşılaştırma `Product` tablo mevcut sütun değerleri ve orijinal değerleri zaman GridView (veya DetailsView veya FormView) son doldurulup doldurulmadığına. Tüm alanları dışındaki beri `ProductID`, `ProductName`, ve `Discontinued` olabilir `NULL` doğru karşılaştırma değerleri, ek parametreler ve denetimleri dahildir `NULL` değerler `WHERE` yan tümcesi.

ASP.NET sayfamızda, yalnızca güncelleştirme, silme ürün bilgileri sağlayacaktır herhangi bir ek DataTable iyimser eşzamanlılık özellikli veri kümesi için Bu öğreticide, ekleyeceğiz gerekmez. Ancak, yine de eklemek ihtiyacımız `GetProductByProductID(productID)` yönteme `ProductsOptimisticConcurrency` TableAdapter.

Bunu yapmak için TableAdapter bağdaştırıcısının başlık çubuğuna sağ tıklayın (alan hemen üstündeki `Fill` ve `GetProducts` yöntem adları) ve bağlam menüsünden Sorgu Ekle'ı seçin. Bu, TableAdapter sorgu Yapılandırma Sihirbazı başlatılır. Oluşturmak için TableAdapter bağdaştırıcısının ilk yapılandırma ile iyileştirilmiş gibi `GetProductByProductID(productID)` geçici SQL deyimi kullanarak yöntemini (bkz: Şekil 4). Bu yana `GetProductByProductID(productID)` yöntemi belirli bir ürünün ilgili bilgileri döndürür, bu sorgu olduğunu belirten bir `SELECT` sorgu satırlar döndüren türü.


[![Sorgu türü olarak işaretlemek bir &quot;satır döndüren SELECT&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Şekil 9**: sorgu türü olarak işaretlemek bir "`SELECT` satırları döndürür" ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image27.png))


Sonraki ekranda, TableAdapter bağdaştırıcısının varsayılan sorguyu ile önceden yüklenmiş kullanmak SQL sorgu için biz istenir. Yan tümce eklemek için var olan sorgu büyütmek `WHERE ProductID = @ProductID`Şekil 10'da gösterildiği gibi.


[![Ekleme bir WHERE yan tümcesi için belirli bir ürün kaydı önceden yüklenmiş sorguyu](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Şekil 10**: ekleme bir `WHERE` Pre-Loaded belirli bir ürün kaydı döndürmek için sorgu yan tümcesinin ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image30.png))


Son olarak, oluşturulan yöntemi adlarını değiştirmek `FillByProductID` ve `GetProductByProductID`.


[![Yöntemleri FillByProductID ve GetProductByProductID olarak yeniden adlandırın](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Şekil 11**: yöntemlere Yeniden Adlandır `FillByProductID` ve `GetProductByProductID` ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image33.png))


Bu Sihirbazı Tamamlandı, TableAdapter artık veri almak için iki yöntem içerir: `GetProducts()`, döndüren *tüm* ürünleri; ve `GetProductByProductID(productID)`, belirtilen ürün döndürür.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>3. adım: iyimser eşzamanlılık etkin DAL için bir iş mantığı katmanı oluşturma

Varolan müşterilerimizin `ProductsBLL` sınıfı toplu güncelleştirme ve DB doğrudan desenleri kullanma örnekleri vardır. `AddProduct` Yöntemi ve `UpdateProduct` her iki aşırı yüklemeleri kullanın tümleştirilmesidir toplu güncelleştirme deseni bir `ProductRow` TableAdapter bağdaştırıcısının güncelleştirme yöntemi örneği. `DeleteProduct` Yöntemi, diğer taraftan, TableAdapter bağdaştırıcısının DB doğrudan deseni kullanan `Delete(productID)` yöntemi.

Yeni `ProductsOptimisticConcurrency` TableAdapter, DB yöntemleri artık gerekli orijinal değerleri de ayrıca geçirilmesi doğrudan. Örneğin, `Delete` yöntemi artık bekliyor on giriş parametreleri: özgün `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, ve `Discontinued`. Bu ek girdi parametrelerinin değerleri kullanan `WHERE` yan tümcesi `DELETE` veritabanının geçerli değerleri kadar özgün olanları eşlerseniz belirtilen kayıt silme yalnızca veritabanına gönderilen ifadesi.

Yöntem imzası TableAdapter için bağdaştırıcısının while `Update` taşınmadığından toplu güncelleştirme deseninde kullanıldı yöntemi değiştirildi, özgün ve yeni değerlerini kaydetmek için gereken kod. Bu nedenle, yerine bizim varolan ile iyimser eşzamanlılık etkin DAL kullanma girişimi `ProductsBLL` sınıfında, müşterilerimize yeni bir DAL ile çalışmak için yeni bir iş mantığı katmanı sınıf oluşturalım.

Adlı bir sınıf ekleyin `ProductsOptimisticConcurrencyBLL` için `BLL` klasördeki `App_Code` klasör.


![BLL klasöre ProductsOptimisticConcurrencyBLL sınıfı Ekle](implementing-optimistic-concurrency-vb/_static/image34.png)

**Şekil 12**: ekleme `ProductsOptimisticConcurrencyBLL` BLL klasörüne sınıfı


Ardından, aşağıdaki kodu ekleyin `ProductsOptimisticConcurrencyBLL` sınıfı:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Not `NorthwindOptimisticConcurrencyTableAdapters` sınıf bildiriminin başlangıç yukarıda ifadesi. `NorthwindOptimisticConcurrencyTableAdapters` Ad alanı içerir `ProductsOptimisticConcurrencyTableAdapter` DAL'ın yöntemler sağlar sınıfını. Ayrıca sınıfın bildiriminden önce bulacaksınız `System.ComponentModel.DataObject` özniteliği bu sınıf ObjectDataSource sihirbazın aşağı açılan listesinde içermek için Visual Studio bildirir.

`ProductsOptimisticConcurrencyBLL`'S `Adapter` özelliği örneği hızlı erişim sağlar `ProductsOptimisticConcurrencyTableAdapter` sınıfı ve bizim orijinal BLL sınıflarda kullanılan deseni izler (`ProductsBLL`, `CategoriesBLL`, vb.). Son olarak, `GetProducts()` yöntemi yalnızca çağıran DAL ait `GetProducts()` yöntemi ve döndürür bir `ProductsOptimisticConcurrencyDataTable` nesnesi doldurulmuş ile bir `ProductsOptimisticConcurrencyRow` veritabanındaki her ürün kaydı için örneği.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>DB doğrudan desen ile iyimser eşzamanlılık kullanarak ürün siliniyor

İyimser eşzamanlılık kullanan bir DAL karşı DB doğrudan deseni kullanılırken, yöntemleri yeni ve özgün değerleri geçirilmelidir. Silme için yeni değerler yoktur, dolayısıyla yalnızca özgün değerlerine geçirilmesi. Bizim BLL ardından, biz özgün parametrelerin tümü girdi parametresi olarak kabul etmeniz gerekir. Başlayalım `DeleteProduct` yönteminde `ProductsOptimisticConcurrencyBLL` sınıfı DB doğrudan yöntemi kullanın. Bu, bu yöntem tüm on ürün veri alanları giriş parametresi olarak yararlanın ve aşağıdaki kodda gösterildiği gibi bu DAL için geçmesi gerektiğini anlamına gelir:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Kullanıcı Sil düğmesine tıkladığında veritabanında değerlerinin - GridView (veya DetailsView veya FormView içinde) en son yüklenen bu değerleri - özgün değerler farklıysa `WHERE` yan tümcesi olmaz eşleşme herhangi bir veritabanı kaydı ile hiçbir kayıt etkilenecek. Bu nedenle, TableAdapter bağdaştırıcısının `Delete` yöntemi döndürür `0` sayfalar ve BLL'ın `DeleteProduct` yöntemi döndürür `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Toplu güncelleştirme desen ile iyimser eşzamanlılık kullanarak ürün güncelleştiriliyor

TableAdapter bağdaştırıcısının daha önce belirtildiği gibi `Update` yöntemi ve toplu güncelleştirme desen için iyimser eşzamanlılık işe olup olmadığına bakılmaksızın aynı yöntem imzası vardır. Yani, `Update` yöntemi, bir dizi DataRow, DataTable ya da bir türü belirtilmiş veri kümesi bir DataRow bekliyor. Orijinal değerleri belirtmek için ek giriş parametresi vardır. DataTable özgün ve düzeltilmiş değerleri için kendi DataRow(s) izler mümkün olmasıdır. DAL, sorunlar, `UPDATE` deyimi, `@original_ColumnName` parametreleri ise DataRow nesnesinin özgün değerlerle doldurulur `@ColumnName` parametreleri DataRow nesnesinin değiştirilmiş değerlerle doldurulur.

İçinde `ProductsBLL` (bizim orijinal, olmayan-iyimser eşzamanlılık DAL kullanır) sınıfı kodumuz aşağıdaki olaylar dizisi gerçekleştirir, ürün bilgilerini güncelleştirmek için toplu güncelleştirme düzeni kullanırken:

1. Geçerli veritabanı ürün bilgilerini okuma bir `ProductRow` TableAdapter bağdaştırıcısının kullanarak `GetProductByProductID(productID)` yöntemi
2. İçin yeni değerler atayın `ProductRow` örneğinden 1. adım
3. TableAdapter bağdaştırıcısının çağrı `Update` tümleştirilmesidir yöntemi `ProductRow` örneği

Çünkü bu bir dizi adımdan, ancak doğru iyimser eşzamanlılık desteklemeyeceği `ProductRow` içinde doldurulmuş 1. adım, DataRow tarafından kullanılan özgün değerleri, şu anda mevcut olduğu anlamına doğrudan veritabanından doldurulur Veritabanı ve düzenleme işlemi başlangıcında GridView bağlanmış olanlar değil. Kullanarak bir iyimser eşzamanlılık-DAL etkin olduğunda, bunun yerine, alter için ihtiyacımız `UpdateProduct` yöntemi aşırı yüklemelerine aşağıdaki adımları kullanın:

1. Geçerli veritabanı ürün bilgilerini okuma bir `ProductsOptimisticConcurrencyRow` TableAdapter bağdaştırıcısının kullanarak `GetProductByProductID(productID)` yöntemi
2. Ata *özgün* değerler `ProductsOptimisticConcurrencyRow` örneğinden 1. adım
3. Çağrı `ProductsOptimisticConcurrencyRow` örneğinin `AcceptChanges()` DataRow geçerli değerleri "özgün" olduğunu bildirir yöntemi
4. Ata *yeni* değerler `ProductsOptimisticConcurrencyRow` örneği
5. TableAdapter bağdaştırıcısının çağrı `Update` tümleştirilmesidir yöntemi `ProductsOptimisticConcurrencyRow` örneği

Belirtilen ürün kaydı için tüm geçerli veritabanı değerlerini 1. adım okur. Bu adım, gereksiz `UpdateProduct` güncelleştiren aşırı yükleme *tüm* ürün sütun (Bu değerler 2. adımda yazılır), ancak burada yalnızca bir alt kümesini sütun değerleri geçirilir, olarak bu aşırı yüklemeler için gereklidir Giriş parametreleri. Özgün değerlerine atandığı sonra `ProductsOptimisticConcurrencyRow` örneği `AcceptChanges()` yöntemi çağrıldığında, geçerli bir DataRow değerleri, kullanılacak özgün değer olarak işaretler `@original_ColumnName` parametrelerinde `UPDATE` deyimi. Ardından, yeni parametre değerlerini atanan `ProductsOptimisticConcurrencyRow` ve son olarak, `Update` yöntemi çağrılır, DataRow içinde geçirme.

Aşağıdaki kodda gösterildiği `UpdateProduct` tüm ürün verileri kabul eden aşırı alanları olarak giriş parametreleri. Burada gösterilmeyen sırada `ProductsOptimisticConcurrencyBLL` sınıfı Bu öğretici ayrıca içerir, yüklemeye dahil bir `UpdateProduct` yalnızca ürün adını ve fiyat girdi parametresi olarak kabul eden aşırı yükleme.


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>4. adım: özgün ve yeni değerleri ASP.NET sayfasında BLL yöntemlere geçirirken

DAL ve BLL tam kalan tek şey sistemde yerleşik iyimser eşzamanlılık mantığı kullanabileceği bir ASP.NET sayfası oluşturmak için. Özellikle, Web denetimi (GridView, DetailsView veya FormView) verileri özgün değerleri ve ObjectDataSource her iki değer kümeleri için iş mantığı katmanı geçmelidir unutmamanız gerekir. Ayrıca, ASP.NET sayfasını eşzamanlılık ihlalleri düzgün bir şekilde işlemek için yapılandırılması gerekir.

Başlangıç açarak `OptimisticConcurrency.aspx` sayfasını `EditInsertDelete` klasörü ve GridView tasarımcıya ayarlama, ekleme, `ID` özelliğini `ProductsGrid`. GridView'ın akıllı etiketten adlı yeni bir ObjectDataSource oluşturmak için iyileştirilmiş `ProductsOptimisticConcurrencyDataSource`. İyimser eşzamanlılık destekleyen DAL kullanmak için bu ObjectDataSource istiyoruz olduğundan, bunu kullanacak şekilde yapılandırmanız `ProductsOptimisticConcurrencyBLL` nesne.


[![ObjectDataSource kullanması ProductsOptimisticConcurrencyBLL nesnesi](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Şekil 13**: ObjectDataSource kullanması `ProductsOptimisticConcurrencyBLL` nesne ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image37.png))


Seçin `GetProducts`, `UpdateProduct`, ve `DeleteProduct` sihirbazdaki açılır listelerden yöntemleri. UpdateProduct yöntemi için ürünün veri alanlarının tümünün kabul eden aşırı yüklemesini kullanın.

## <a name="configuring-the-objectdatasource-controls-properties"></a>ObjectDataSource denetim özelliklerini yapılandırma

Sihirbazı tamamladıktan sonra bildirim temelli biçimlendirme ObjectDataSource aşağıdaki gibi görünmelidir:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Gördüğünüz gibi `DeleteParameters` koleksiyonu içeren bir `Parameter` her on giriş parametreleri için örnek `ProductsOptimisticConcurrencyBLL` sınıfın `DeleteProduct` yöntemi. Benzer şekilde, `UpdateParameters` koleksiyonu içeren bir `Parameter` her giriş parametreleri için örnek `UpdateProduct`.

Veri değişikliği söz konusu önceki bu öğreticileri için biz ObjectDataSource kaldırarak `OldValuesParameterFormatString` bu özellik, BLL yöntemi geçirilmesi için eski (veya özgün) değerleri ve bunun yanı sıra yeni değerleri beklediğini belirtir. bu yana bu noktada özelliği. Ayrıca, bu özellik değeri, özgün değerlerine giriş parametresi adları gösterir. Özgün değerleri BLL geçiriyoruz beri yapmak *değil* bu özelliği kaldırın.

> [!NOTE]
> Değerini `OldValuesParameterFormatString` özelliği, özgün değerlerine beklediğiniz giriş parametre adları için BLL eşlenmelidir. Biz bu parametreleri adlı bu yana `original_productName`, `original_supplierID`ve benzeri bırakabilirsiniz `OldValuesParameterFormatString` özellik değeri olarak `original_{0}`. Ancak, BLL yöntemleri giriş parametreleri gibi adları olan, `old_productName`, `old_supplierID`ve benzeri güncelleştirmeniz gerekiyor `OldValuesParameterFormatString` özelliğini `old_{0}`.


ObjectDataSource orijinal değerleri doğru BLL yöntemlere geçirmek için sırayla yapılması gereken bir son özellik ayarı yoktur. ObjectDataSource sahip bir ['ınızı özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) için atanabilir [iki değerden birini](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -Varsayılan değer; Orijinal değerleri BLL yöntemleri özgün giriş parametreleri göndermez
- `CompareAllValues` -özgün değerlerine BLL yöntemlere; gönderme İyimser eşzamanlılık kullanırken bu seçeneği belirleyin.

Ayarlamak için bir dakikanızı ayırın `ConflictDetection` özelliğini `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>GridView'ın özellikleri ve alanları yapılandırma

Düzgün yapılandırılmış ObjectDataSource özelliklerle GridView ' ayarlamanın uygulamamızla dönelim. İlk olarak, düzenleme ve silme desteklemek için GridView istiyoruz düzenlemeyi etkinleştir ve silmeyi etkinleştir onay kutularını GridView'ın akıllı etiketinde tıklayın. Bu bir CommandField ekler, `ShowEditButton` ve `ShowDeleteButton` hem ayarlamak `true`.

Ne zaman bağlı `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView içeren bir alan her ürünün veri alanları. Böyle bir GridView düzenlenebilir, kullanıcı deneyimini herhangi bir şey ancak kabul edilebilir düzeyde olduğunda. `CategoryID` Ve `SupplierID` BoundFields işleme metin kutuları, kimlik numaraları olarak uygun bir kategori ve tedarikçi girmesini gerektiren. Olacaktır hiçbir sayısal alanlar ve ürün adı sağlandı ve birim fiyatı stoktaki birimleri, sırası ve sipariş düzeyi değerleri birimlerde uygun hem sayısal değerler ve büyüktür veya eşittir emin olmak için hiçbir doğrulama denetimleri için biçimlendirme sıfır olarak.

Açıkladığımız gibi *arabirimleri ekleme ve düzenleme için doğrulama denetimleri ekleme* ve *veri değişikliği arabirimini özelleştirme* öğreticiler, kullanıcı arabirimi özelleştirilebilir tarafından BoundFields TemplateField ile değiştiriliyor. Aşağıdaki yollarla bu GridView ve düzenleme arabirimiyle değiştirdiğiniz:

- Kaldırılan `ProductID`, `SupplierName`, ve `CategoryName` BoundFields
- Dönüştürülen `ProductName` BoundField bir TemplateField için ve bir RequiredFieldValidation denetimi eklediniz.
- Dönüştürülen `CategoryID` ve `SupplierID` BoundFields TemplateField için ve düzenleme arabirimi metin kutuları yerine DropDownList kullanacak şekilde ayarlanmış. İçinde bu TemplateField `ItemTemplates`, `CategoryName` ve `SupplierName` veri alanları görüntülenir.
- Dönüştürülen `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` BoundFields TemplateField ve eklenen CompareValidator denetimleri.

Önceki öğreticilerde, bu görevleri gerçekleştirmek üzere nasıl biz zaten anlatılmaktadır miyim yalnızca son bildirim temelli söz dizimi burada listeleyin ve uygulama yöntemi olarak bırakın.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Tam olarak çalışan bir örnek sahip yakın çok çalışıyoruz. Ancak, yedekleme işi ve bize sorunlara neden birkaç ıot'nin vardır. Ayrıca, yine de bir eşzamanlılık ihlali oluştuğunda kullanıcıyı uyarır bazı arabirimi ihtiyacımız var.

> [!NOTE]
> Doğru (Bu ardından için BLL geçirilir) ObjectDataSource orijinal değerleri geçirmek için sırasıyla bir veri Web denetimi için önemli olan GridView'ın `EnableViewState` özelliği `true` (varsayılan). Görünüm durumu devre dışı bırakırsanız, özgün değerler geri göndermede kaybolur.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>ObjectDataSource için doğru özgün değerlerini geçirme

Birkaç GridView yapılandırma yönteminiz ile ilgili sorunlar vardır. Varsa ObjectDataSource `ConflictDetection` özelliği `CompareAllValues` (olduğu gibi bizim), ObjectDataSource `Update()` veya `Delete()` yöntemleri GridView (veya DetailsView veya FormView) çağrılır, kopyalamak ObjectDataSource çalışır GridView'ın özgün değerlerine uygun kendi `Parameter` örnekleri. Geri bu işlem için bir grafik gösterimi Şekil 2'ye bakın.

Özellikle, GridView'ın özgün değerlerine GridView'a verileri her zaman bağlı iki yönlü veri bağlama ifadeleri değerler atanır. Bu nedenle, gerekli tüm gerekli orijinal değerleri çift yönlü veri bağlama yakalanır ve bir dönüştürülebilir biçiminde sağlanır.

Neden bu önemli olduğunu görmek için bir tarayıcıda sayfamızı ziyaret etmek için bir dakikanızı ayırarak. Beklendiği gibi bir düzenleme ve silme düğmesi en soldaki sütunda her bir ürün GridView listeler.


[![Ürünler GridView içinde listelenir](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Şekil 14**: ürünleri GridView içinde listelenir ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image40.png))


Herhangi bir ürünü için Sil düğmesine tıklarsanız bir `FormatException` oluşturulur.


[![Herhangi bir FormatException ürün sonuçlarında silinmeye çalışılıyor](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Şekil 15**: Any ürün sonuçları silme girişiminde bir `FormatException` ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image43.png))


`FormatException` ObjectDataSource özgün okuma girişiminde bulunduğunda tetiklenir `UnitPrice` değeri. Bu yana `ItemTemplate` sahip `UnitPrice` bir para birimi olarak biçimlendirilmiş (`<%# Bind("UnitPrice", "{0:C}") %>`), $19.95 gibi bir para birimi simgesi içerir. `FormatException` ObjectDataSource Bu dizeye dönüştürmeyi dener sistemdeki bir `decimal`. Bu sorunu aşmak için birçok seçenek sunuyoruz:

- Para birimi Biçimlendirmeyi Kaldır `ItemTemplate`. Diğer bir deyişle, yerine `<%# Bind("UnitPrice", "{0:C}") %>`, sadece kullanın `<%# Bind("UnitPrice") %>`. Bu dezavantajı, fiyat artık biçimlendirildiğinden emin olur.
- Görüntü `UnitPrice` bir para biriminde olarak biçimlendirilmiş `ItemTemplate`, ancak `Eval` bunu gerçekleştirmek için anahtar sözcüğü. Bu geri çağırma `Eval` tek yönlü bir bağlama gerçekleştirir. Hala sağlamak için ihtiyacımız `UnitPrice` hala bir iki yönlü veri bağlama deyiminde gerekir böylece özgün değerleri için değer `ItemTemplate`, ancak bu etiketin Web denetimde ayarlanmış yerleştirilebilir `Visible` özelliği `false`. Aşağıdaki biçimlendirmede ItemTemplate kullanabiliriz:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Para birimi Biçimlendirmeyi Kaldır `ItemTemplate`kullanarak `<%# Bind("UnitPrice") %>`. GridView'ın içinde `RowDataBound` olay işleyicisi, programlama yoluyla erişim etiket Web denetimi içinde `UnitPrice` değeri görüntülenir ve ayarlayın, `Text` biçimlendirilmiş sürüm özelliği.
- Bırakın `UnitPrice` bir para birimi olarak biçimlendirilmiş. GridView'ın içinde `RowDeleting` olay işleyicisi, varolan orijinalin yerine geçecek `UnitPrice` değeri ($19.95) kullanarak bir gerçek ondalık değer ile `Decimal.Parse`. Benzer bir şey yerine getirmeyi gördüğümüz `RowUpdating` olay işleyicisinde [ *işleme BLL ve DAL düzeyi özel durumları bir ASP.NET sayfasında* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) öğretici.

İkinci yaklaşımda Git seçtiniz my örneğin gizli etiket Web ekleme, denetim özelliği `Text` özelliği olan iki yönlü veri biçimlendirilmemiş bağımlı `UnitPrice` değeri.

Bu sorunun çözümüne sonra herhangi bir ürünü için Delete düğmeye yeniden tıklandığında deneyin. Bu süre elde edeceğiniz bir `InvalidOperationException` ObjectDataSource çalıştığında BLL's çağırmaya `UpdateProduct` yöntemi.


[![ObjectDataSource Gönder istediği giriş parametreleri olan bir yöntem bulunamıyor](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Şekil 16**: ObjectDataSource Gönder istediği giriş parametreleri olan bir yöntem bulunamıyor ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image46.png))


Özel durumun ileti bakarak, onu ObjectDataSource bir BLL çağrılacak istediğini işaretlenmemiştir `DeleteProduct` içeren yöntem `original_CategoryName` ve `original_SupplierName` giriş parametreleri. Bunun nedeni, `ItemTemplate` s `CategoryID` ve `SupplierID` TemplateField ile iki yönlü bir bağlama ifadeleri şu anda içeren `CategoryName` ve `SupplierName` veri alanları. Bunun yerine, dahil etmek ihtiyacımız `Bind` ifadelerle `CategoryID` ve `SupplierID` veri alanları. Bunu yapmak için var olan bağlama deyimleri ile değiştirin. `Eval` ifadeleri ve gizli etiket denetimleri ekleyin `Text` için ilişkili özellikleri `CategoryID` ve `SupplierID` gösterildiği gibi çift yönlü veri bağlama kullanarak veri alanları Aşağıda:


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Bu değişikliklerle başarıyla silin ve ürün bilgilerini düzenlemek sunmaktayız! Adım 5'te eşzamanlılık ihlali algılandı doğrulamak ne inceleyeceğiz. Ancak şimdilik güncelleştirme denemek için birkaç dakika sürebilir ve bu güncelleştirme ve silme tek bir kullanıcı için emin olmak için birkaç kayıt silme beklendiği gibi çalışır.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>5. adım: iyimser eşzamanlılık destek test

Eşzamanlılık ihlalleri algılanan (yerine doğrudan üzerine veri elde edilen) doğrulamak için bu sayfaya iki tarayıcı pencerelerini açmak gerekiyor. Her iki tarayıcı durumlarda ayrıntılarını Düzenle düğmesine tıklayın. Ardından, yalnızca bir tarayıcı, "Chai Çay" için adı değiştirin ve Güncelleştir'e tıklayın. Güncelleştirme, başarılı ve GridView yeni ürün adı olarak "Chai Çay" ile önceden düzenleme durumuna geri dönün.

Diğer tarayıcı penceresi örneğinde, ancak ürün adı metin kutusuna "Chai" görüntülenmeye devam eder. İkinci bir tarayıcı penceresi içinde güncelleştirme `UnitPrice` için `25.00`. İyimser eşzamanlılık desteği olmadan ikinci bir tarayıcı örneğinde Güncelleştir'i tıklatarak ürün adı için "böylece ilk tarayıcı örneği tarafından yapılan değişikliklerin üzerine geri Chai", değiştirirsiniz. İyimser eşzamanlılık işe, ancak ikinci bir tarayıcı örneğinde güncelleştir düğmesine tıklayarak sonuçlanır bir [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Bir eşzamanlılık ihlali algılandığında bir DBConcurrencyException oluşturulur](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Şekil 17**: bir eşzamanlılık ihlali algılandığında bir `DBConcurrencyException` oluşturulur ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image49.png))


`DBConcurrencyException` DAL'ın toplu güncelleştirme düzeni kullanılırken yalnızca oluşturulur. DB doğrudan deseni bir özel durum oluşturmaz, yalnızca satır etkilendiğini gösterir. Bunu açıklamak üzere; her iki tarayıcı örneğinin GridView önceden düzenleme durumlarını döndürür. Ardından, ilk tarayıcı örneğinde, Düzenle düğmesine tıklayın ve "Chai Çay" geri "Chai" için ürün adını değiştirmek ve Güncelleştir'e tıklayın. İkinci tarayıcı penceresinde ayrıntılarını Sil düğmesine tıklayın.

Delete tuşuna basarak, bağlı sayfanın geri gönderir, GridView ObjectDataSource çağırır `Delete()` yöntemi ve ObjectDataSource içine çağırır `ProductsOptimisticConcurrencyBLL` sınıfın `DeleteProduct` yöntemini özgün değerleri. Özgün `ProductName` ikinci tarayıcı örneği "Chai, geçerli eşleşmiyor Çay", değeri `ProductName` veritabanında değeri. Bu nedenle `DELETE` veritabanına verilen deyimi veritabanında hiçbir kayıt olduğundan sıfır satır etkiler, `WHERE` yan tümcesi karşılar. `DeleteProduct` Yöntemi döndürür `false` ve ObjectDataSource veri GridView'a DataSet'e bağlanır.

Son kullanıcının açısından Flash ekran için ikinci bir tarayıcı penceresi içinde Chai Çay Sil düğmesine tıklayarak neden ve, artık "Chai" (ilk tarayıcı tarafından yapılan ürün adı değişikliği olarak listeleniyor ancak geri dönmeyi üzerine ürün hala, yoktur Örnek). Kullanıcı yeniden Sil düğmesine tıklarsa, GridView özgün olarak silme başarılı `ProductName` değeri ("Chai") artık eşleştiğini veritabanında değerine sahip.

Her iki durumda, kullanıcı deneyimini gölgeden uzak idealdir. Biz açıkça kullanıcı nitty-gritty ayrıntılarını göstermek istemediğiniz `DBConcurrencyException` toplu güncelleştirme deseni kullanılırken özel durum. Ve DB doğrudan deseni kullanılırken kullanıcılar komutu başarısız oldu, ancak hiçbir hassas bir gösterge neden biraz kafa karıştırıcı bir davranıştır.

Bu iki sorun gidermek için bir güncelleştirme veya silme işlemi başarısız oldu neden bir açıklama sağlayın sayfadaki etiket Web denetimleri oluşturabilirsiniz. Toplu güncelleştirme desenini, belirleyebiliriz olup olmadığı bir `DBConcurrencyException` GridView'ın sonrası düzeyi olay işleyicisinde bir özel durum oluştu uyarı etiketi gerektiği şekilde görüntüleme. DB doğrudan yöntem için biz BLL yöntemin dönüş değerini inceleyebilirsiniz (olduğu `true` bir satır etkileniyorsa `false` aksi) ve gerektiği şekilde bir bilgilendirme iletisi görüntüler.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>6. adım: Bilgi iletilerini ekleme ve bunları bir eşzamanlılık ihlali karşılaşıldığında görüntüleme

Bir eşzamanlılık ihlali meydana geldiğinde sergilenen davranışı olup DAL'ın toplu güncelleştirme veya DB doğrudan desen kullanılan üzerinde bağlıdır. Müşterilerimize öğreticide her iki desenler, güncelleştirmek ve silmek için kullanılan DB doğrudan deseni için kullanılan toplu güncelleştirme deseni kullanır. Başlamak için iki etiket Web denetimi eşzamanlılık ihlali silmeye veya güncelleştirmeye veri çalışırken ortaya çıktığını açıklayan sayfamıza ekleyelim. Etiket denetimin ayarlayın `Visible` ve `EnableViewState` özelliklerine `false`; bu olanlar için nereye belirli sayfasını ziyaret dışında her sayfasını ziyaret edin gizlenecek açacak kendi `Visible` programlı olarak ayarlanırsa `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Ek olarak kendi `Visible`, `EnabledViewState`, ve `Text` özellikleri, ben ayrıca bildirimler'i `CssClass` özelliğini `Warning`, büyük, red, italik, kalın yazı tipinde görüntülenecek etiket neden olan kullanıcının. Bu CSS `Warning` sınıfın tanımlı ve Styles.css için eklenen geri *ekleme, güncelleştirme ve silme ile ilişkili olayları İnceleme* öğretici.

Bu etiketler ekledikten sonra Visual Studio tasarımcıda Şekil 18 benzer görünmelidir.


[![Sayfaya eklenen iki etiket denetimleri](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Şekil 18**: iki etiket denetimleri eklenmiş sayfa ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image52.png))


Bu etiketin Web kontroller varken, ne zaman bir eşzamanlılık ihlali, hangi uygun etiketin işaret oluştuğunu belirlemek nasıl incelemek hazırız `Visible` özelliği ayarlanabilir `true`, bilgilendirme iletisi görüntüleniyor.

## <a name="handling-concurrency-violations-when-updating"></a>Eşzamanlılık ihlalleri güncelleştirirken işleme

İlk toplu işlem kullanırken eşzamanlılık ihlalleri nasıl ele alınacağını göz deseni güncelleştirelim. Desen neden böyle ihlalleri batch'le güncelleştirmesinden bu yana bir `DBConcurrencyException` özel durum oluşturulmasına ASP.NET sayfamıza belirlemek için kod eklemek ihtiyacımız olmadığını bir `DBConcurrencyException` güncelleştirme işlemi sırasında özel durum oluştu. Bu nedenle, biz kaydını düzenleme başlatıldığında başka bir kullanıcı arasında aynı veri değiştirdiğinden, yaptıkları değişiklikleri kaydedilmedi kullanıcı açıklayan bir ileti görüntülenmelidir ve bunlar güncelleştir düğmesine tıklandığında.

İçinde gördüğümüz gibi *işleme BLL ve DAL düzeyi özel durumları bir ASP.NET sayfasında* öğretici, bu tür özel durumlar algılanabilir ve veri Web denetimin sonrası düzeyi olay işleyicilerindeki gizlendi. Bu nedenle, GridView için ait bir olay işleyicisi oluşturmak ihtiyacımız `RowUpdated` denetler olay bir `DBConcurrencyException` özel durumu oluştu. Bu olay işleyicisi, olay işleyicisini kodladıktan aşağıda gösterildiği gibi güncelleştirme işlemi sırasında başlatılan özel bir başvuru geçirilir:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Face, bir `DBConcurrencyException` özel durum, bu olay işleyicisi görüntüler `UpdateConflictMessage` etiket denetimini ve özel durumun işlenip gösterir. Bir kaydı güncelleştirilirken bir eşzamanlılık ihlali meydana geldiğinde bu yana değişiklikler başka bir kullanıcının aynı anda üzerlerine yerde şu kodla kullanıcının değişiklikler, kaybolur. Özellikle, GridView önceden düzenleme durumuna geri döndürülen ve geçerli veritabanı verilere bağlı. Bu, daha önce görünür değil diğer kullanıcının yaptığı değişiklikler sayesinde, GridView satır güncelleştirir. Ayrıca, `UpdateConflictMessage` etiket denetimi açıklamak için kullanıcı ne mi oldu. Bu olaylar dizisi şekil 19'ayrıntılı olarak verilmiştir.


[![Bir kullanıcı s eşzamanlılık ihlali yüz tanıma güncelleştirmeleri kayboluyor](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Şekil 19**: bir kullanıcı s güncelleştirmeleri eşzamanlılık ihlalinin karşılaştıkları kaybolur ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image55.png))


> [!NOTE]
> Alternatif olarak, GridView önceden düzenleme durumuna döndürmek yerine, biz GridView düzenleme durumuna ayarlayarak bırakabilir `KeepInEditMode` geçilen özelliği `GridViewUpdatedEventArgs` nesne true. Bu yaklaşımı benimsemeniz durumunda, ancak veri GridView rebind emin olun (çağırarak kendi `DataBind()` yöntemi) ve böylece düzenleme arabirimine yüklenen diğer kullanıcının yaptığı değerler. Bu öğretici ile indirilebilir kod sahip bu iki kod satırlarını `RowUpdated` olay işleyicisi açıklamalı kullanıma; yalnızca bu GridView sağlamak için kod satırlarını düzenleme modunda bir eşzamanlılık ihlali sonra kalan açıklamasını kaldırın.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Eşzamanlılık ihlallerini silerken yanıt

DB doğrudan desen ile karşılaşıldığında bir eşzamanlılık ihlali harekete geçirilen özel durum yoktur. Bunun yerine database deyimi yalnızca WHERE yan tümcesi bir kayıtla eşleşmiyor olarak hiçbir kayıt etkiler. Bunlar tam olarak bir kayıt etkilenmiş olup olmadığını gösteren bir Boole değeri döndürür, tüm BLL içinde oluşturulan veri değişikliği yöntemleri üretildi. Bu nedenle, bir kaydı silinirken bir eşzamanlılık ihlali oluşup olmadığını belirlemek için biz BLL'ın dönüş değerini inceleyebilirsiniz `DeleteProduct` yöntemi.

BLL yöntemi için dönüş değeri ile ObjectDataSource sonrası düzeyi olay işleyicileri incelenebilir `ReturnValue` özelliği `ObjectDataSourceStatusEventArgs` olay işleyicisine nesnesi geçirildi. Biz dönüş değeri saptanırken ilgilendiğiniz beri `DeleteProduct` yöntemi ihtiyacımız ObjectDataSource için bir olay işleyicisi oluşturmak `Deleted` olay. `ReturnValue` Özelliği türüdür `object` ve `null` bir özel durum oluştu ve yöntem bir değer döndürebilir önce kesildi. Bu nedenle, öncelikle, emin oluruz `ReturnValue` özelliği değil `null` ve bir Boole değeri. Bu onay geçirir, göstereceğiz varsayılarak `DeleteConflictMessage` , etiket denetimini `ReturnValue` olduğu `false`. Bu, aşağıdaki kodu kullanarak gerçekleştirilebilir:


[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Eşzamanlılık ihlali karşılaşıldığında, kullanıcının silme isteği iptal edildi. Sayfa ve he Sil düğmesine tıklandığında Bu kaydın saat arasındaki kullanıcı oluşan değişiklikleri yüklenen gösteren GridView yenilenir. Böyle bir ihlali transpires, `DeleteConflictMessage` etiketi gösterilir, açıklayan (bkz. Şekil 20) ne mi oldu.


[![Bir eşzamanlılık ihlali karşılaşıldığında bir kullanıcı s silme iptal edildi](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Şekil 20**: bir kullanıcı s eşzamanlılık ihlalinin karşılaşıldığında silme iptal edildi ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-vb/_static/image58.png))


## <a name="summary"></a>Özet

Birden çok, her bir uygulamada eşzamanlılık ihlalleri fırsatları mevcut güncelleştirmek veya veri silmek için eş zamanlı kullanıcı. Bu tür ihlalleri iki kullanıcı kişi son yazma "WINS'de" alır aynı verileri aynı anda güncelleştirdiğinizde, değişiklikleri üzerine diğer kullanıcının yaptığı değişiklikleri için hesaba değil ise. Alternatif olarak, geliştiriciler ya da iyimser veya kötümser eşzamanlılık denetimi uygulayabilirsiniz. İyimser eşzamanlılık denetimi eşzamanlılık ihlalleri daha azdır ve yalnızca güncelleştirme izin vermiyor veya bir eşzamanlılık ihlali olarak nitelendirilir komutu silme olduğunu varsayar. Kötümser eşzamanlılık denetimi ihlalleri sık ve update veya delete komutu yalnızca bir kullanıcının reddetme eşzamanlılık kabul edilebilir değil varsayar. Kötümser eşzamanlılık denetimi ile kaydı güncelleştirmek, böylece kilitliyken kayıt silme veya değiştirme başka bir kullanıcı engelleme kilitleme gerektirir.

. NET'te türü belirtilmiş veri kümesi için iyimser eşzamanlılık denetimini destekleyen işlevleri sağlar. Özellikle, `UPDATE` ve `DELETE` deyimleri veritabanına verilen tüm tablonun sütunlarını ekleyin, kaydın geçerli veriler özgün verilerle kullanıcıyla eşleştiğinde update veya delete yalnızca meydana gelir, böylelikle sağlayarak, sahip olduğunda kendi güncelleştirme veya silme gerçekleştirme. DAL, iyimser eşzamanlılığı desteklemek için yapılandırıldıktan sonra BLL yöntemleri güncelleştirilmesi gerekir. Ek olarak, BLL çağıran ASP.NET sayfasını ObjectDataSource orijinal değerleri kendi veri Web denetiminden alır ve BLL geçirir şekilde yapılandırılmalıdır.

Bu öğreticide gördüğümüz gibi iyimser eşzamanlılık denetimi, bir ASP.NET web uygulamasında uygulama BLL ve DAL güncelleştiriliyor ve ASP.NET sayfasında destek eklemeyi içerir. Olsun veya olmasın bu eklenen iş bir akıllıca zaman ve Emek yatırımdır, uygulamaya bağlıdır. Eş zamanlı kullanıcı verileri güncelleştirme seyrek sahip ya da güncelleştirmekte olduğunuz verilerin birbirlerinden farklı olduğundan, ardından eşzamanlılık denetimi bir anahtar sorun değildir. Ancak, düzenli olarak birden çok kullanıcı aynı verilerle çalışma sitenizde'iniz varsa, eşzamanlılık denetimi bir kullanıcının güncelleştirme ve silme farkında olmadan başka birinin üzerine yazmasını engellemek yardımcı olabilir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](customizing-the-data-modification-interface-vb.md)
> [İleri](adding-client-side-confirmation-when-deleting-vb.md)
