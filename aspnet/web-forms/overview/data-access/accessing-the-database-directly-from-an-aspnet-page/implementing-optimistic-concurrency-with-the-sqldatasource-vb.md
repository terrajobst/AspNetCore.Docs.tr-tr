---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: "İyimser eşzamanlılık SqlDataSource (VB) ile uygulama | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide iyimser eşzamanlılık denetimini essentials gözden geçirin ve SqlDataSource denetimi kullanarak uygulama keşfedin."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 603aaa35a533fc8853ea72fc9be05ca82b213049
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>İyimser eşzamanlılık SqlDataSource (VB) ile uygulama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) veya [PDF indirin](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> Bu öğreticide iyimser eşzamanlılık denetimini essentials gözden geçirin ve SqlDataSource denetimi kullanarak uygulama keşfedin.


## <a name="introduction"></a>Giriş

Önceki öğreticide ekleme, güncelleştirme ve silme SqlDataSource denetimi yetenekleri ekleme incelendi. Kısacası, bu özellikler sağlamak için biz buna karşılık gelen belirtmek gereken `INSERT`, `UPDATE`, veya `DELETE` s denetim SQL deyiminde `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` uygun birlikte özellikleri Parametrelerde `InsertParameters`, `UpdateParameters`, ve `DeleteParameters` koleksiyonları. Bu özellikler ve Koleksiyonlar el ile belirtilebilir, ancak veri kaynağı Yapılandırma Sihirbazı'nı s Gelişmiş düğmesi bir Generate sunar `INSERT`, `UPDATE`, ve `DELETE` otomatik oluşturur-bu deyimleri deyimleri onay kutusuna bağlı olarak `SELECT` deyimi.

Generate birlikte `INSERT`, `UPDATE`, ve `DELETE` deyimleri onay kutusu, Gelişmiş SQL oluşturma seçenekleri iletişim kutusu kullan iyimser eşzamanlılık seçeneği içerir (bkz: Şekil 1). İşaretlendiğinde, `WHERE` otomatik olarak oluşturulur yan tümcelerinde `UPDATE` ve `DELETE` deyimleri yalnızca güncelleştirme gerçekleştirmek için değiştirildiğinde veya delete temel alınan veritabanı veri edilmemiş t bu yana kullanıcı değiştirilmiş, en son yüklenen veri kılavuza.


![İyimser eşzamanlılık destek Gelişmiş ekleyebilirsiniz SQL oluşturma seçenekleri iletişim kutusu](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Şekil 1**: iyimser eşzamanlılık destek Gelişmiş ekleyebilirsiniz SQL oluşturma seçenekleri iletişim kutusu


Geri [uygulama iyimser eşzamanlılık](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) biz incelenmesi iyimser eşzamanlılık denetimini ve ObjectDataSource ekleme temelleri öğretici. Bu öğreticide biz iyimser eşzamanlılık denetimini essentials üzerinde rötuşlama ve SqlDataSource kullanarak uygulama keşfedin.

## <a name="a-recap-of-optimistic-concurrency"></a>İyimser eşzamanlılık, bir özeti

Birden fazla, web uygulamaları için düzenlemek veya aynı veri silmek için eşzamanlı kullanıcıların bir kullanıcı başka bir s değişiklikleri yanlışlıkla üzerine olasılığı vardır. İçinde [uygulama iyimser eşzamanlılık](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) ı sağlanan aşağıdaki örnek öğretici:

Jisun ve Sam, iki kullanıcı hem de ziyaretçileri güncelleştirmek ve ürünleri GridView denetimini üzerinden silmek izin verilen bir uygulamada sayfasını ziyaret düşünün. Her ikisi de Düzenle düğmesini ayrıntılarını aynı anda tıklatın. Jisun ürün adı Chai Çay değiştirir ve güncelleştirme düğmesine tıklar. Net sonucu olan bir `UPDATE` ayarlar veritabanına gönderilen deyimi *tüm* ürün s güncelleştirilebilir alanlarının (Jisun yalnızca bir alanın güncelleştirilmiş olmasa bile `ProductName`). Bu anda, veritabanı Chai Çay, Meşrubat, kategori ve benzeri belirli bu ürün için tedarikçi Exotic Liquids değerlere sahip. Ancak, Sam s ekranında GridView hala ürün adı düzenlenebilir GridView satırda Chai gösterilir. Birkaç saniye sonra Jisun s değişiklikler kaydedildi Sam Condiments için kategori güncelleştirir ve güncelleştirme tıklar. Bunun bir `UPDATE` Chai için ürün adını ayarlar veritabanına gönderilen deyimi `CategoryID` karşılık gelen Condiments kategori kimliği ve benzeri. Ürün adı Jisun s değişiklikler üzerine yazıldı.

Şekil 2 bu etkileşimi gösterilmektedir.


[![İki kullanıcı aynı anda bir kaydı güncelleştirdiğinizde var. bir kullanıcıyı s olası diğer s üzerine yazmak için değiştirir](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Şekil 2**: olduğunda iki kullanıcılar aynı anda güncelleştirme bir kayıt var. s olası bir kullanıcıyı değiştirir üzerine yaz diğer s ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Bu senaryoyu unfolding gelen, bir formu engellemek için [eşzamanlılık denetimi](http://en.wikipedia.org/wiki/Concurrency_control) uygulanmalıdır. [İyimser eşzamanlılık](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) Bu öğreticinin odak olabilir ancak eşzamanlılık çakışması every şimdi yapıp gibi çakışmaları won t zaman çoğunluğu ortaya varsayımına çalışır. Bir çakışma çıkması durumunda, bu nedenle, iyimser eşzamanlılık denetimini yalnızca kullanıcı başka bir kullanıcı aynı veri değiştirildiğinden kendi değişiklikleri can t kaydedilmesi bildirir.

> [!NOTE]
> Burada, birçok eşzamanlılık çakışması olur veya bu gibi çakışmaları tolerable değilseniz varsayılır uygulamalar için ardından eşzamanlılık denetim yerine kullanılabilir. Geri başvurmak [uygulama iyimser eşzamanlılık](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) eşzamanlılık denetimi hakkında daha kapsamlı bir tartışma için Öğreticisi.


İyimser eşzamanlılık denetimini güncelleştirme veya silme işlemi başlattığınızda olduğu gibi güncelleştirildiğinde veya silindiğinde kayıt aynı değere sahip olduğundan emin olarak çalışır. Örneğin, düzenlenebilir GridView Düzenle düğmesine tıklandığında, kayıt s değerleri veritabanından okunur ve metin kutuları ve diğer Web denetimleri görüntülenir. Bu orijinal değerleri GridView tarafından kaydedilir. Daha sonra kullanıcı kendi değişiklikleri yapar ve güncelleştirme düğmesine tıklar sonra `UPDATE` kullanılan ifadesi özgün değerler artı yeni değerleri dikkate alın ve kullanıcı düzenleme başladı orijinal değerleri yalnızca temel alınan veritabanı kaydını güncelleştirin. Veritabanı değerlerde yine aynıdır. Şekil 3 olayların bu sırası gösterilmektedir.


[![Güncelleştirme veya silme için başarılı olması, özgün değerler geçerli veritabanı değerine eşit olmalıdır](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Şekil 3**: For Update veya Delete Succeed, orijinal değerleri gerekir olması eşit geçerli veritabanı değerlerini ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


İyimser eşzamanlılık uygulamak için çeşitli yaklaşım vardır (bkz [Peter A. Bromberg](http://peterbromberg.net/) s [Optmistic eşzamanlılık güncelleştirme mantığı](http://www.eggheadcafe.com/articles/20050719.asp) bir dizi seçenek kısa göz için). SqlDataSource tarafından (yanı sıra ADO.NET yazılan bizim veri erişim katmanı kullanılan veri kümelerindeki tarafından) kullanılan teknik güçlendirir `WHERE` tüm orijinal değerleri karşılaştırması içerecek şekilde yan tümcesi. Aşağıdaki `UPDATE` deyimi, örneğin, güncelleştirmeleri adını ve bir ürünün fiyat yalnızca geçerli veritabanı değerler GridView kaydında güncelleştirirken başlangıçta alınan değerleri eşit ise. `@ProductName` Ve `@UnitPrice` parametreleri, ancak kullanıcı tarafından girilen yeni değerleri içermesi `@original_ProductName` ve `@original_UnitPrice` Düzenle düğmesi tıklatıldığında GridView başlangıçta yüklenen değerleri içerir:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Bu öğreticide anlatıldığı gibi SqlDataSource ile iyimser eşzamanlılık denetimini etkinleştirme bir onay kutusu denetimi olarak kadar basittir.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>1. adım: iyimser eşzamanlılık destekleyen bir SqlDataSource oluşturma

Başlangıç açarak `OptimisticConcurrency.aspx` gelen sayfa `SqlDataSource` klasör. Araç kutusu tasarımcıya Ayarları'ndan bir SqlDataSource denetimi sürükleyin kendi `ID` özelliğine `ProductsDataSourceWithOptimisticConcurrency`. Ardından, Denetim s akıllı etiket yapılandırma veri kaynağı bağlantısını tıklayın. Sihirbazın ilk ekranından ile çalışmayı tercih `NORTHWINDConnectionString` ve İleri'yi tıklatın.


[![NORTHWINDConnectionString ile çalışacak biçimde seçin](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Şekil 4**: ile çalışacak biçimde seçin `NORTHWINDConnectionString` ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


Biz ekleme düzenlemek kullanıcıların sağlayan GridView bu örneğin `Products` tablo. Bu nedenle, Select deyimi ekran yapılandırma seçin `Products` tablo aşağı açılan listeden ve seçin `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` Şekil 5'te gösterildiği gibi sütun.


[![Ürünler tablosundan ProductID, ProductName, UnitPrice ve devam etmeyen sütunları döndürür](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Şekil 5**: gelen `Products` tablo, iade `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` sütunlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Sütunları seçtikten sonra Gelişmiş SQL oluşturma seçenekleri iletişim kutusunu Getir için Gelişmiş düğmesini tıklatın. Generate denetleyin `INSERT`, `UPDATE`, ve `DELETE` deyimleri iyimser eşzamanlılık onay kutularını kullanın ve Tamam'ı tıklatın (geri için ekran görüntüsü Şekil 1'e bakın). İleri'yi tıklatarak Sihirbazı tamamlayın ve ardından son.

Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra elde edilen incelemek için bir dakikanızı ayırın `DeleteCommand` ve `UpdateCommand` özellikleri ve `DeleteParameters` ve `UpdateParameters` koleksiyonları. Bunu yapmanın en kolay yolu, kaynak sekmesinde sayfa s Tanımlayıcı Sözdizimi görmek için sol alt köşesinde bulunan tıklatın olmaktır. Bul bir `UpdateCommand` değeri:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Yedi parametrelerinde ile `UpdateParameters` koleksiyonu:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Benzer şekilde, `DeleteCommand` özelliği ve `DeleteParameters` koleksiyonu, aşağıdaki gibi görünmelidir:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Program.cs'ye yanı sıra `WHERE` yan tümcelerinde `UpdateCommand` ve `DeleteCommand` özellikleri (ve ek parametrelerle ilgili parametre koleksiyonuna ekleme), iyimser eşzamanlılık seçenek iki diğer ayarlar kullanım seçme Özellikler:

- Değişiklikleri [ `ConflictDetection` özelliği](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) gelen `OverwriteChanges` (varsayılan) için`CompareAllValues`
- Değişiklikleri [ `OldValuesParameterFormatString` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) {0} (varsayılan) özgün\_{0}.

Ne zaman veri Web denetimi çağırır SqlDataSource s `Update()` veya `Delete()` yöntemi, özgün değerleri geçirir. Varsa SqlDataSource s `ConflictDetection` özelliği ayarlanmış `CompareAllValues`, özgün bu değerleri komutu eklenir. `OldValuesParameterFormatString` Özelliği bu özgün değer parametreler için kullanılan adlandırma deseni sağlar. Veri Kaynağı Yapılandırma Sihirbazı'nı özgün kullanır\_{0} ve özgün her parametre adları `UpdateCommand` ve `DeleteCommand` özellikleri ve `UpdateParameters` ve `DeleteParameters` koleksiyonları buna göre.

> [!NOTE]
> Biz yetenekleri ekleme SqlDataSource denetimi s kullanmayan re eşitleyerek beri kaldırmak ücretsiz `InsertCommand` özelliği ve kendi `InsertParameters` koleksiyonu.


## <a name="correctly-handlingnullvalues"></a>Doğru şekilde işlememesi`NULL`değerleri

Ne yazık ki, Genişletilmiş `UPDATE` ve `DELETE` deyimleri iyimser eşzamanlılık kullanırken veri kaynağı Yapılandırma Sihirbazı tarafından otomatik olarak yapmak *değil* içeren kayıtları ile çalışmak `NULL` değerleri. Nedenini görmek için bizim SqlDataSource s göz önünde bulundurun. `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice` Sütununda `Products` tablo olabilir `NULL` değerleri. Belirli bir kayıt varsa, bir `NULL` değerini `UnitPrice`, `WHERE` yan tümcesi bölümü `[UnitPrice] = @original_UnitPrice` olacak *her zaman* çünkü False olarak değerlendirmek `NULL = NULL` her zaman False değerini döndürür. Bu nedenle, kayıtları içeren `NULL` değerleri düzenlenmesine veya silinmiş olarak `UPDATE` ve `DELETE` deyimleri `WHERE` yan tümceleri won t return güncelleştirmek veya silmek için herhangi bir satır.

> [!NOTE]
> Bu hata, Haziran 2004 ' ilk Microsoft'a bildirildi [SqlDataSource oluşturur yanlış SQL deyimlerini](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) ve bağlarsanız ASP.NET bir sonraki sürümde sabit planlanır.


Bu sorunu gidermek için el ile güncelleştirmek sahibiz `WHERE` yan tümceleri hem de `UpdateCommand` ve `DeleteCommand` özelliklerini **tüm** olabilir sütunlar `NULL` değerleri. Genel olarak, değiştirmeniz `[ColumnName] = @original_ColumnName` için:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Bu değişikliği doğrudan Özellikler penceresini veUpdateQuery veya DeleteQuery seçeneklerinden yoluyla bildirim temelli işaretleme veya UPDATE aracılığıyla yapılabilir ve özel bir SQL deyimi veya saklı yordam verilerini Yapılandır seçeneğinde belirt sekmeleri SİLİN Kaynak Sihirbazı. Yeniden, bu değişiklik yapılması gereken *her* sütununda `UpdateCommand` ve `DeleteCommand` s `WHERE` içerebilir yan tümcesi `NULL` değerleri.

Bu bizim örnek uygulama sonuçları aşağıdaki değiştiren `UpdateCommand` ve `DeleteCommand` değerler:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>2. adım: Düzenle ve Sil seçenekleriyle GridView ekleme

İyimser eşzamanlılık desteklemek üzere yapılandırılmış SqlDataSource ile kalan tek şey verilerin Web denetimi bu eşzamanlılık denetim yararlanan eklemek için. Bu öğretici için her iki düzenleme sağlayan GridView ekleme ve silme işlevlerinin s olanak tanır. Bunu gerçekleştirmek için araç kümesi ve Tasarımcısı üzerine GridView sürükleyin kendi `ID` için `Products`. GridView s akıllı etiketten kendisine bağlamak `ProductsDataSourceWithOptimisticConcurrency` SqlDataSource denetimi adım 1'de eklendi. Son olarak, akıllı etiket düzenlemeyi etkinleştir ve Enable Deleting seçeneklerinden denetleyin.


[![GridView SqlDataSource bağlama ve düzenleme ve silme etkinleştir](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Şekil 6**: GridView SqlDataSource ve düzenlemeyi etkinleştir ve silme için bağlamak ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


GridView ekledikten sonra görünümünü kaldırarak yapılandırma `ProductID` değiştirme BoundField `ProductName` BoundField s `HeaderText` ürün ve güncelleştirme özelliği `UnitPrice` BoundField böylece kendi `HeaderText` özelliği yalnızca fiyat. İdeal olarak, biz d RequiredFieldValidator için eklenecek düzenleme arabirimi geliştirmek `ProductName` değeri ve bir CompareValidator `UnitPrice` (s düzgün şekilde biçimlendirilmiş bir sayısal değer sağlamak için) değeri. Başvurmak [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) arabirimini düzenleme GridView s özelleştirme daha derinlemesine bir bakış için Öğreticisi.

> [!NOTE]
> GridView SqlDataSource geçirilen özgün değerler olduğundan s görünüm durumu etkinleştirilmelidir GridView Görünüm durumu depolanır.


GridView bu değişiklikleri yaptıktan sonra GridView ve SqlDataSource bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

İyimser eşzamanlılık denetimini çalışırken görmek için iki tarayıcı penceresi açın ve yük `OptimisticConcurrency.aspx` hem de sayfa. Her iki tarayıcılarda ilk ürün Düzenle düğmeleri tıklayın. Bir tarayıcıda, ürün adı değiştirin ve Güncelleştir'i tıklatın. Tarayıcı geri gönderilir ve GridView yalnızca düzenlenen kayıt için yeni ürün adı gösteren önceden düzenleme moduna döner.

İkinci bir tarayıcı penceresi fiyatı (ancak ürün adı özgün değeri olarak bırakın) değiştirin ve Güncelleştir'i tıklatın. Geri gönderme, önceden düzenleme moduna kılavuz verir, ancak fiyat değişikliği kaydedilmedi. İkinci tarayıcı eski fiyat yeni ürün adıyla aynı değeri ilk olarak gösterir. İkinci bir tarayıcı penceresi içinde yapılan değişiklikler kayboldu. Hiçbir özel durum veya bir eşzamanlılık ihlali yalnızca oluştuğunu belirten ileti haliyle Ayrıca, değişiklikler yerine sessizce kayboldu.


[![İkinci bir tarayıcı penceresi değişiklikleri sessizce kayboldu](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Şekil 7**: ikinci tarayıcı penceresi olan sessizce kayıp değişiklikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Neden ikinci tarayıcı s değişiklikleri değil taahhüt neden nedeni, `UPDATE` deyimi s `WHERE` yan tümcesi tüm kayıtları filtre ve bu nedenle hiçbir satırı etkilemedi. S bakmak izin `UPDATE` deyimi yeniden:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

İkinci bir tarayıcı penceresi kaydı güncelleştirdiğinde, özgün ürün adı belirtilen `WHERE` yan tümcesi mevcut değil t eşleme (ilk tarayıcı tarafından değiştirilmesinden bu yana) var olan bir ürün adı ile. Bu nedenle, deyim `[ProductName] = @original_ProductName` False döndürür ve `UPDATE` herhangi bir kayıt etkilemez.

> [!NOTE]
> Delete aynı şekilde çalışır. İki tarayıcı pencerelerini ile açık, belirli bir ürün biriyle düzenleme ve değişiklikleri kaydetme başlatın. Bir tarayıcıda değişiklikler kaydedildikten sonra diğer aynı ürün için de Sil düğmesini tıklatın. İçinde orijinal değerleri tan t eşleşme beri `DELETE` deyimi s `WHERE` sessizce silme yan tümcesi başarısız olur.


Son kullanıcı s açısından ikinci tarayıcı penceresinde Güncelleştir düğmesini tıklattıktan sonra Kılavuz önceden düzenleme moduna döner ancak bunların değişiklikler kayboldu. Ancak, burada s kendi değişiklikleri etmedi t takılıyor hiçbir visual geri bildirim. İdeal olarak, bir kullanıcı s değişiklikleri eşzamanlılık ihlalinin kaybolursa d size bildirmek ve, belki de kılavuz düzenleme modunda tutun. Bunu gerçekleştirmek ne Ara s olanak tanır.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>3. adım: Bir eşzamanlılık ihlali ne zaman oluştuğunu belirleme

Bir eşzamanlılık ihlali bir yaptığı değişiklikler reddeder olduğundan, bir eşzamanlılık ihlali oluştuğunda kullanıcıyı uyarmak iyi olacaktır. Let s kullanıcıyı uyarmak için bir etiket Web denetimi adlı sayfanın üst kısmına ekleyin `ConcurrencyViolationMessage` , `Text` özelliği aşağıdaki ileti görüntülenir: güncelleştirme veya aynı anda başka bir kullanıcı tarafından güncelleştirilen bir kaydı silme girişiminde bulunuldu. Lütfen diğer kullanıcının değişiklikleri gözden geçirin ve ardından, güncelleştirme Yinele veya silin. Etiket denetimi s ayarlamak `CssClass` bir CSS sınıfı uyarı özelliğine tanımlanan `Styles.css` kırmızı, italik, kalın ve büyük yazı tipiyle metni görüntüler. Son olarak, etiketin s ayarlamak `Visible` ve `EnableViewState` özelliklerine `False`. Bu etiket burada açıkça ayarlarız yalnızca bu Geri göndermeler dışında gizlenir kendi `Visible` özelliğine `True`.


[![Etiket denetimi uyarı görüntüleyecek şekilde sayfasına ekleme](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Şekil 8**: uyarı görüntüleyecek şekilde sayfasına bir etiket denetimi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Gerçekleştirirken bir güncelleştirme veya silme, GridView s `RowUpdated` ve `RowDeleted` olay işleyicileri yangın kendi veri kaynağı denetimi istenen güncelleştirme veya silme işlemi gerçekleştirdikten sonra. Bu olay işleyicileri işlemi satır sayısını etkilendi öğeleri belirleyebilirsiniz. Sıfır satır etkilendi, biz görüntülemek istiyorsanız `ConcurrencyViolationMessage` etiketi.

Her ikisi için bir olay işleyicisi oluşturun `RowUpdated` ve `RowDeleted` olayları ve aşağıdaki kodu ekleyin:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

Her iki olay işleyicileri biz denetleyin `e.AffectedRows` özelliği ve 0 eşitse, ayarlayın `ConcurrencyViolationMessage` etiket s `Visible` özelliğine `True`. İçinde `RowUpdated` olay işleyicisi, biz de istemeniz ayarlayarak düzenleme modunda kalmayı GridView `KeepInEditMode` özelliğinin true. Bunu yaparken, böylece diğer kullanıcı s verileri düzenleme arabirimine yüklenen veri kılavuza rebind seçmeliyiz. Bu GridView s çağırarak gerçekleştirilir `DataBind()` yöntemi.

Şekil 9, bu iki olay işleyicileri ile gösterildiği gibi bir eşzamanlılık ihlali oluştuğunda çok belirgin bir ileti görüntülenir.


[![Bir eşzamanlılık ihlali karşısında bir ileti görüntülenir](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Şekil 9**: bir eşzamanlılık ihlali karşısında bir ileti görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Özet

Bir web uygulaması oluştururken, burada birden çok, eşzamanlı kullanıcılar aynı veri düzenleme eşzamanlılık denetim seçeneklerini göz önünde bulundurmak önemlidir. Varsayılan olarak, veri kaynağı denetimleri ve Web denetimleri ASP.NET veri herhangi bir eşzamanlılık denetimi uygulamadığınız değil. Bu öğreticide gördüğümüz gibi iyimser eşzamanlılık denetimini SqlDataSource ile uygulama görece hızlı ve kolay olur. Ekleme, engagement'ta için SqlDataSource legwork çoğunu işleme `WHERE` otomatik olarak oluşturulur yan tümcelerini `UPDATE` ve `DELETE` deyimleri ancak var olan birkaç subtleties işlemedeki `NULL` anlatıldığı gibi sütun değeri Doğru şekilde işlememesi `NULL` değerleri bölümü.

Bu öğretici SqlDataSource bizim incelendiğinde sonlanır. ObjectDataSource ve katmanlı mimarisi kullanarak verilerle çalışmak için kalan öğreticilerimizi döndürür.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Önceki](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
