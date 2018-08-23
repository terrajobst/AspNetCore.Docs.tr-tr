---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: (C#) SqlDataSource ile iyimser eşzamanlılık uygulama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide iyimser eşzamanlılık denetiminin temel bilgileri gözden geçirin ve ardından SqlDataSource denetimi kullanarak nasıl keşfedin.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f2590e8e7712d719eb89403ef839f03066a93d2b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757272"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>(C#) SqlDataSource ile iyimser eşzamanlılık uygulama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) veya [PDF olarak indirin](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> Bu öğreticide iyimser eşzamanlılık denetiminin temel bilgileri gözden geçirin ve ardından SqlDataSource denetimi kullanarak nasıl keşfedin.


## <a name="introduction"></a>Giriş

Önceki öğreticide biz ekleme, güncelleştirme ve silme SqlDataSource denetimi olanağı ekleme incelenir. Kısacası, bu özellikleri sağlamak için buna karşılık gelen belirtmek ihtiyacımız `INSERT`, `UPDATE`, veya `DELETE` s denetimi SQL deyiminde `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` uygun birlikte özellikleri parametrelerinde `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları. Bu özellikler ve Koleksiyonlar el ile belirtilebilir, ancak veri kaynağı Yapılandırma Sihirbazı'nı s Gelişmiş düğmesine bir Oluştur sunar `INSERT`, `UPDATE`, ve `DELETE` otomatik oluşturur-Bu deyimler deyimleri onay kutusuna bağlı olarak `SELECT` deyimi.

Generate birlikte `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusunu Gelişmiş SQL oluşturma seçenekleri iletişim kutusu içeren iyimser eşzamanlılık seçeneğini kullanın (bkz. Şekil 1). Bu onay kutusu işaretlendiğinde, `WHERE` otomatik olarak oluşturulan yan tümcelerinde `UPDATE` ve `DELETE` deyimleri, yalnızca güncelleştirme gerçekleştirmek için değiştirildiğinde veya silme temel alınan veritabanı veri forumlarındaki t kullanıcı beri değiştirilmiş, en son yüklenen verileri kılavuza.


![İyimser eşzamanlılık destek Gelişmiş ekleyebilirsiniz SQL oluşturma iletişim kutusu seçenekleri](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Şekil 1**: iyimser eşzamanlılık destek Gelişmiş ekleyebilirsiniz SQL oluşturma iletişim kutusu seçenekleri


Geri [iyimser eşzamanlılık uygulama](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) öğretici iyimser eşzamanlılık denetimi ve ObjectDataSource için ekleme temelleri biz incelenir. Bu öğreticide biz iyimser eşzamanlılık denetiminin hakkında temel bilgiler rötuşlama ve ardından SqlDataSource kullanarak nasıl keşfetmek.

## <a name="a-recap-of-optimistic-concurrency"></a>İyimser eşzamanlılık bir özeti

Birden çok izin veren web uygulamaları için Düzenle veya aynı verileri silmek için eşzamanlı kullanıcıların bir kullanıcı başka bir s değişikliklerin yanlışlıkla üzerine olasılığı vardır. İçinde [iyimser eşzamanlılık uygulama](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) öğretici miyim sağlanan aşağıdaki örnekte:

İki kullanıcı, Jisun ve Vedat, hem de ziyaretçiler, güncelleştirme ve silme ürünler aracılığıyla bir GridView denetimi izin verilen bir uygulamada bir sayfasını ziyaret düşünün. Her ikisi de Düzenle düğmesini ayrıntılarını aynı anda tıklayın. Jisun Chai Çay için ürün adını değiştirir ve güncelleştir düğmesine tıklar. Net sonucu olan bir `UPDATE` ayarlar veritabanına gönderilen bildirimi *tüm* ürün s güncelleştirilebilir alanların (Jisun yalnızca bir alan güncelleştirilmiş olmasa bile `ProductName`). Bu anda, veritabanında Chai Çay, İçecekler, kategori vb. Bu belirli bir ürün için tedarikçi Exotic Liquids değerleri var. Ancak, Sam s ekranında GridView hala ürün adı düzenlenebilir GridView satırında Chai gösterilir. Birkaç saniye Jisun s değişiklikleri işlendikten sonra Sam kategorisi için Çeşniler güncelleştirir ve güncelleştirme tıklar. Sonuçlanır bir `UPDATE` Chai için ürün adını ayarlar veritabanına gönderilen deyimi `CategoryID` karşılık gelen Çeşniler kategori kimliği ve benzeri. Ürün adı Jisun s değişikliklerin üzerine yazıldı.

Şekil 2, bu etkileşim gösterilmektedir.


[![İki kullanıcı aynı anda bir kayıt güncelleştirdiğinizde var. bir kullanıcıyı s potansiyeli diğer s üzerine yazmak için değişir](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Şekil 2**: olduğunda iki kullanıcılar aynı anda güncelleştirme bir kaydı var. s olası bir kullanıcıyı değiştirir üzerine yaz diğer s ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Bu senaryo unfolding gelen, çeşit önlemek için [eşzamanlılık denetimi](http://en.wikipedia.org/wiki/Concurrency_control) uygulanmalıdır. [İyimser eşzamanlılık](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) odak bu öğreticinin olabilir ancak eşzamanlılık çakışmalarını every zorunluluğu gibi çakışmaların kazanılan t süresi büyük çoğunluğu ortaya varsayımına çalışır. Bir çakışma oluşursa, bu nedenle, iyimser eşzamanlılık denetimi yalnızca kullanıcının aynı verileri başka bir kullanıcı tarafından değiştirildiğinden, değişiklikleri can t kaydedilmesi bildirir.

> [!NOTE]
> Burada, çok fazla eşzamanlılık çakışma olur ya da bu gibi çakışmaları fazla değilseniz varsayılır uygulamalar için ardından kötümser eşzamanlılık denetimi yerine kullanılabilir. Kiracıurl [iyimser eşzamanlılık uygulama](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) kötümser eşzamanlılık denetimi hakkında daha kapsamlı bir tartışma için öğretici.


İyimser eşzamanlılık denetimi, güncelleştirme veya silme işlemi başlatıldığında gibi kayıt güncelleştirildiğinde veya silindiğinde değerlerin aynı olduğundan emin olarak çalışır. Örneğin, düzenlenebilir bir GridView Düzenle düğmesine tıklandığında, kayıt s değerleri veritabanından okunur ve metin kutuları ve diğer Web denetimleri görüntülenir. Bu özgün değerlerine GridView tarafından kaydedilir. Daha sonra kullanıcı değişiklikleri yapar ve güncelleştir düğmesine tıkladığında `UPDATE` kullanılan deyimi özgün değerlerine yanı sıra yeni değerleri dikkate alın ve kullanıcı düzenlemeye başladığını orijinal değerleri yalnızca temel alınan veritabanı kaydı güncelleştirin yine de veritabanında değerleri aynıdır. Şekil 3, bu olayların sırasını gösterir.


[![Update veya Delete için başarılı olması, orijinal değerleri geçerli veritabanı değerlere eşit olmalıdır](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Şekil 3**: For Update veya Delete Succeed, özgün değer gerekir olması eşit geçerli veritabanı değerlerini ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


İyimser eşzamanlılık uygulama çeşitli yaklaşımları vardır (bkz [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic eşzamanlılık güncelleştirme mantığı](http://www.eggheadcafe.com/articles/20050719.asp) birçok seçenek kısa göz atmak için). SqlDataSource (veya ADO.NET yazılan bizim veri erişim katmanındaki kullanılan veri kümelerine göre) kullanılan bir teknik artırmaktadır `WHERE` tüm orijinal değerleri karşılaştırması içerecek şekilde yan tümcesi. Aşağıdaki `UPDATE` deyimi, örneğin, güncelleştirmeleri adı ve ürünün fiyatı yalnızca geçerli veritabanı değerler GridView kaydında güncelleştirirken ilk olarak alınan değerlerle eşitse. `@ProductName` Ve `@UnitPrice` parametreleri ise kullanıcı tarafından girilen yeni değerleri içeren `@original_ProductName` ve `@original_UnitPrice` Düzenle düğmesine tıklandığında GridView yüklenen ilk değerleri içerir:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Bu öğreticide anlatıldığı gibi SqlDataSource ile iyimser eşzamanlılık denetimini etkinleştirme olarak bir onay kutusunu işaretlemek kadar kolaydır.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>1. adım: iyimser eşzamanlılık destekleyen bir SqlDataSource oluşturma

Başlangıç açarak `OptimisticConcurrency.aspx` gelen sayfasında `SqlDataSource` klasör. SqlDataSource denetimi ayarları Tasarımcısı araç kutusundan sürükleyin, `ID` özelliğini `ProductsDataSourceWithOptimisticConcurrency`. Ardından, Denetim s akıllı etiket yapılandırmak veri kaynağı bağlantısından tıklayın. Sihirbazın ilk ekranından ile çalışmayı tercih `NORTHWINDConnectionString` ve İleri'ye tıklayın.


[![NORTHWINDConnectionString çalışmak için seçin](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Şekil 4**: çalışmak tercih `NORTHWINDConnectionString` ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


Bu örneğin ekleyeceğiz, kullanıcıların düzenlemesine olanak sağlayan GridView `Products` tablo. Bu nedenle, Select deyimini ekran yapılandırma seçin `Products` tablosunu aşağı açılan listeden seçip `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` sütunları, Şekil 5'te gösterildiği gibi.


[![Ürünleri tablo, ProductID, ProductName, UnitPrice ve artık sağlanmayan sütunları döndürür.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Şekil 5**: gelen `Products` tablo, iade `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` sütunları ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Sütunları seçtikten sonra Gelişmiş SQL oluşturma seçenekleri iletişim kutusunu Getir için Gelişmiş düğmesine tıklayın. Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` deyimleri ve iyimser eşzamanlılık onay kutularını kullanın ve Tamam'a tıklayın (Şekil 1'e bir ekran için geri bakın). İleri'yi tıklatarak Sihirbazı tamamlayın ve ardından son.

Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra ortaya çıkan incelemek için bir dakikanızı ayırarak `DeleteCommand` ve `UpdateCommand` özellikleri ve `DeleteParameters` ve `UpdateParameters` koleksiyonları. Bunu yapmanın en kolay yolu, sayfa s bildirim temelli söz dizimi görmek için sol alt köşesine kaynağı sekmesinde tıklayın sağlamaktır. Burada bulacaksınız bir `UpdateCommand` değeri:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Yedi parametrelerinde ile `UpdateParameters` koleksiyonu:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Benzer şekilde, `DeleteCommand` özelliği ve `DeleteParameters` koleksiyonu, aşağıdaki gibi görünmelidir:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Deneyimlerinizi yanı sıra `WHERE` yan tümcelerini `UpdateCommand` ve `DeleteCommand` özellikleri (ve karşılık gelen parametre koleksiyonlara ek parametreler ekleyerek), iyimser eşzamanlılık seçeneği iki diğer ayarlar kullanımı seçme Özellikler:

- Değişiklikleri [ `ConflictDetection` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) gelen `OverwriteChanges` (varsayılan) için `CompareAllValues`
- Değişiklikleri [ `OldValuesParameterFormatString` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) gelen {0} (varsayılan) için özgün\_ {0} .

Ne zaman Web denetimi veri çağırır SqlDataSource s `Update()` veya `Delete()` yöntemi, özgün değerleri geçirir. Varsa SqlDataSource s `ConflictDetection` özelliği `CompareAllValues`, özgün bu değerleri komutu eklenir. `OldValuesParameterFormatString` Bu özgün değer parametreler için kullanılan adlandırma deseni özelliği sağlar. Özgün veri kaynağı Yapılandırma Sihirbazı'nı kullanan\_ {0} ve özgün her parametre adları `UpdateCommand` ve `DeleteCommand` özellikleri ve `UpdateParameters` ve `DeleteParameters` koleksiyonları uygun şekilde.

> [!NOTE]
> Biz yetenekleri ekleme SqlDataSource denetimi s kullanmayan re düşünüyorsanız bu yana kaldırmak ücretsiz `InsertCommand` özelliği ve kendi `InsertParameters` koleksiyonu.


## <a name="correctly-handlingnullvalues"></a>Doğru şekilde`NULL`değerleri

Ne yazık ki, Genişletilmiş `UPDATE` ve `DELETE` ifadeleri otomatik olarak oluşturulan iyimser eşzamanlılık kullanırken veri kaynağı Yapılandırma Sihirbazı tarafından yapmak *değil* içeren kayıtlar ile çalışmak `NULL` değerleri. Nedenini görmek için bizim SqlDataSource s göz önünde bulundurun. `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice` Sütununda `Products` tablo olabilir `NULL` değerleri. Belirli bir kayıt varsa bir `NULL` değerini `UnitPrice`, `WHERE` yan tümcesi bölümü `[UnitPrice] = @original_UnitPrice` olacak *her zaman* False olarak değerlendirilemiyor çünkü `NULL = NULL` her zaman false değerini döndürür. Bu nedenle, kayıtları içeren `NULL` değerler düzenlenemez veya silinmiş olarak `UPDATE` ve `DELETE` deyimleri `WHERE` yan tümceleri, güncelleştirmek veya silmek için herhangi bir satır t dönüş kazandı.

> [!NOTE]
> Bu hata, 2004'ın Haziran Microsoft'a ilk raporlandı [SqlDataSource hatalı SQL deyimleri oluşturan](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) ve ASP.NET'in bir sonraki sürümünde düzeltilen bağlarsanız zamanlandı.


Bu sorunu gidermek için el ile güncelleştirmeniz sahibiz `WHERE` yan tümce hem de `UpdateCommand` ve `DeleteCommand` özelliklerini **tüm** olabilir sütunlar `NULL` değerleri. Genel olarak, değişiklik `[ColumnName] = @original_ColumnName` için:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Bu değişikliği doğrudan Özellikler penceresinden veUpdateQuery veya DeleteQuery seçeneği aracılığıyla bildirim temelli bir işaretleme veya UPDATE aracılığıyla yapılabilir ve özel bir SQL deyimi veya saklı yordam verilerini Yapılandır seçeneğinde belirtin sekmelerde Sil Kaynağı Sihirbazı. Yeniden, bu değişiklik yapılması *her* sütununda `UpdateCommand` ve `DeleteCommand` s `WHERE` içerebilir yan tümcesi `NULL` değerleri.

Bu örneğimizde için uygulama sonuçları aşağıdaki değişiklik `UpdateCommand` ve `DeleteCommand` değerleri:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>2. adım: GridView düzenleme ve silme seçenekleri ekleme

SqlDataSource ile iyimser eşzamanlılığı desteklemek için yapılandırılmış kalan tek şey bu eşzamanlılık denetimi yararlanan sayfasına bir veri Web denetimi eklemek için. Bu öğretici için iki Düzen sağlayan GridView ekleme ve silme işlevlerini sağlar. Bunu gerçekleştirmek için ayarlama ve Tasarımcısı araç kutusundan GridView sürükleyin, `ID` için `Products`. GridView s akıllı etiketten için bağlama `ProductsDataSourceWithOptimisticConcurrency` 1. adımda eklenen SqlDataSource denetimi. Son olarak, akıllı etiket düzenlemeyi etkinleştir ve silmeyi etkinleştir seçeneklerden denetleyin.


[![GridView SqlDataSource için bağlama ve düzenleme ve silme etkinleştir](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Şekil 6**: GridView SqlDataSource ve düzenlemeyi etkinleştir ve silme için bağlama ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


GridView ekledikten sonra onun görünümünü kaldırarak yapılandırma `ProductID` BoundField değiştirme, `ProductName` BoundField s `HeaderText` ürün ve güncelleştirme özelliğini `UnitPrice` BoundField böylece kendi `HeaderText` özelliği yalnızca fiyat. İdeal olarak, d biz düzenleme arabirimi için bir RequiredFieldValidator içerecek şekilde geliştirmek `ProductName` değer ve bir CompareValidator `UnitPrice` (s düzgün biçimlendirilmiş bir sayısal değer sağlamak üzere) değeri. Başvurmak [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) arabirimini düzenleme GridView s özelleştirme bir daha derinlemesine bakış Öğreticisi.

> [!NOTE]
> GridView ' SqlDataSource için geçirilen orijinal değerleri olduğundan s görünüm durumu etkinleştirilmelidir GridView Görünüm durumu depolanır.


GridView bu değişiklikleri yaptıktan sonra GridView ve SqlDataSource bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

İyimser eşzamanlılık denetimi iş başında görmek için iki tarayıcı penceresi açın ve yük `OptimisticConcurrency.aspx` hem de sayfa. İlk ürün hem tarayıcılarda düzenleme düğmelerini tıklayın. Bir tarayıcıda, ürün adı değiştirin ve Güncelleştir'e tıklayın. Tarayıcı geri gönderilir ve GridView düzenlemiş kaydı için yeni ürün adını gösteren önceden düzenleme moduna döndürür.

İkinci bir tarayıcı penceresi içinde fiyatı (ancak özgün değeri olarak ürün adını bırakın) değiştirin ve Güncelleştir'e tıklayın. Geri gönderme, kılavuz, önceden düzenleme moduna döner, ancak fiyat değişikliği kaydedilmedi. İkinci tarayıcı eski fiyat yeni ürün adıyla aynı değeri ilk olarak gösterir. İkinci bir tarayıcı penceresi içinde yapılan değişiklikler kayboldu. Hiçbir özel durum veya bir eşzamanlılık ihlali yalnızca oluştuğunu belirten ileti haliyle Ayrıca, değişiklikler yerine sessizce kayboldu.


[![İkinci bir tarayıcı penceresi değişiklikleri sessizce kayboldu](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Şekil 7**: ikinci tarayıcı penceresi olan sessizce kayıp değişiklikleri ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Neden ikinci tarayıcı s değişiklikleri yapılan değil neden nedeni `UPDATE` deyimi s `WHERE` yan tümcesi tüm kayıtları süzer filtrelenmiş ve bu nedenle hiçbir satırı etkilemeyen. S bakmak istiyorum `UPDATE` deyimi yeniden:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

İkinci bir tarayıcı penceresi kaydı güncelleştirir, orijinal ürün adı belirtilen `WHERE` yan tümcesi eklenmemişse t eşleme ile mevcut ürün adı (ilk tarayıcı tarafından değiştirilmesinden bu yana). Bu nedenle, deyim `[ProductName] = @original_ProductName` yanlış döndürür ve `UPDATE` herhangi bir kayıt etkilemez.

> [!NOTE]
> Delete aynı şekilde çalışır. İki tarayıcı penceresi ile açık, belirli bir ürün olan bir düzenleme ve ardından değişiklikleri kaydetme başlatın. Bir tarayıcıda değişiklikleri kaydettikten sonra diğer aynı ürün için Sil düğmesine tıklayın. İçinde orijinal değerleri don t eşleşme olduğundan `DELETE` deyimi s `WHERE` yan tümcesi silme sessizce başarısız olur.


Son kullanıcı s açısından ikinci bir tarayıcı penceresi içinde güncelleştir düğmesine tıkladıktan sonra önceden düzenleme moduna kılavuz döndürür ancak değişikliklerini kayboldu. Bununla birlikte, burada s hiçbir görsel geri bildirim, değişiklikleri etmedi t takılıyor. İdeal olarak, kullanıcı s değişiklikleri eşzamanlılık ihlalinin kaybolması durumunda, biz d bildirmek ve belki de düzenleme modunda kılavuz tutun. Bunu gerçekleştirmek nasıl ilişkilendirildiğine baktık s olanak tanır.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>3. adım: Bir eşzamanlılık ihlali ne zaman oluştuğunu belirleme

Eşzamanlılık ihlalinin bir yaptığı değişiklikleri reddettiğinde olduğundan, bir eşzamanlılık ihlali oluştuğunda kullanıcıyı uyarmak iyi olurdu. Let s kullanıcıyı uyarmak için bir etiket Web denetimi adlı sayfanın üst kısmına ekleyin `ConcurrencyViolationMessage` olan `Text` özelliği şu iletiyi görüntüler: güncelleştirme veya başka bir kullanıcı tarafından aynı anda güncelleştirilen bir kaydı silme girişiminde bulundunuz. Lütfen diğer kullanıcının yaptığı değişiklikleri gözden geçirin ve ardından güncelleştirmeyi yeniden veya silin. Etiket denetimi s ayarlama `CssClass` bir CSS sınıfı olan uyarı özelliğini tanımlanan `Styles.css` kırmızı, italik, kalın ve büyük yazı tipiyle metni görüntüleyen. Son olarak, ' % s'etiketi s ayarlamak `Visible` ve `EnableViewState` özelliklerine `false`. Bu etiketi yalnızca biz açıkça ayarlandığı geri dışında gizlenir, `Visible` özelliğini `true`.


[![Uyarı görüntüleyecek şekilde sayfasına bir etiket denetimi ekleme](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Şekil 8**: sayfaya uyarı görüntüleyecek şekilde bir etiket denetimi ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Gerçekleştirirken bir update veya delete, GridView s `RowUpdated` ve `RowDeleted` olay işleyicileri yangın kendi veri kaynağı denetimi istenen update veya delete işlemi gerçekleştirdikten sonra. Bu olay işleyicileri işlemi kaç satır etkilendiğini belirleyebiliriz. Sıfır satır etkilendi, biz görüntülemek istediğiniz `ConcurrencyViolationMessage` etiketi.

Bir olay işleyicisi her ikisi için oluşturmak `RowUpdated` ve `RowDeleted` olayları ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

Her iki olay işleyicileri biz denetleyin `e.AffectedRows` özelliği ve 0 eşittir verilirse `ConcurrencyViolationMessage` etiket s `Visible` özelliğini `true`. İçinde `RowUpdated` olay işleyicisi, biz de isteyin ayarlayarak düzenleme modunda kalmayı GridView `KeepInEditMode` özelliği true. Bunun yapılması, bir kullanıcı s veri düzenleme arabirimine yüklenmesi kılavuza veriler yeniden bağlamak ihtiyacımız var. Bu GridView s çağrılarak gerçekleştirilir `DataBind()` yöntemi.

Şekil 9, bu iki olay işleyicileri ile gösterildiği gibi bir eşzamanlılık ihlali oluştuğunda çok belirgin bir ileti görüntülenir.


[![Bir eşzamanlılık ihlali karşılaşıldığında bir ileti görüntülenir.](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Şekil 9**: eşzamanlılık ihlalinin karşılaşıldığında bir ileti görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Özet

Bir web uygulaması oluştururken burada birden çok, eşzamanlı kullanıcılar aynı verileri düzenleme eşzamanlılık denetimi seçeneklerini göz önünde bulundurmanız önemlidir. Varsayılan olarak, ASP.NET veri Web denetimleri ve veri kaynağı denetimleri herhangi bir eşzamanlılık denetimi dağıtıyorsunuz değil. Bu öğreticide gördüğümüz gibi SqlDataSource ile iyimser eşzamanlılık denetimi uygulamak görece hızlı ve kolay. Ekleme, genişletilmiş için SqlDataSource çalıştırabilirsiniz çoğunu işler `WHERE` otomatik olarak oluşturulan yan tümceyi `UPDATE` ve `DELETE` ancak deyimleri olan birkaç ıot'nin işlemede `NULL` anlatıldığı gibi sütun değeri Doğru şekilde `NULL` değerleri bölümü.

Bu öğreticiyi SqlDataSource bizim incelenmesi sonlandırır. ObjectDataSource ile katmanlı mimari verilerle çalışmaya kalan öğreticilerimizden döndürür.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [İleri](querying-data-with-the-sqldatasource-control-vb.md)
