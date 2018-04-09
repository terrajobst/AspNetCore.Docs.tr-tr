---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Veritabanı değişiklikleri (C#) bir işlem içinde sarmalama | Microsoft Docs
author: rick-anderson
description: Bu güncelleştirme, silme ve toplu veri ekleme görünen dört ilk öğreticidir. Bu öğreticide nasıl veritabanı işlemleri izin bilgi edinin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a3f8ec2de7b9259e4bb83f4346bde8abfd643fb4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Bir işlem (C#) içinde kaydırma veritabanı değişiklikleri
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) veya [PDF indirin](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Bu güncelleştirme, silme ve toplu veri ekleme görünen dört ilk öğreticidir. Bu öğreticide nasıl veritabanı işlemleri tüm adımları başarılı veya tüm adımları başarısız sağlayan atomik bir işlem olarak gerçekleştirilebilmesi toplu değişikliklere izin ver öğrenin.


## <a name="introduction"></a>Giriş

İle başlayan gördüğümüz gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğretici, GridView desteği sağlar. yerleşik satır düzeyi düzenleme ve silme için. Fare birkaç tıklama ile bir satır kod yazmadan zengin veri değişikliği arabirimi düzenleme ve silme bir satır başına temelinde ile içerik sürece oluşturmak mümkündür. Ancak, bazı senaryolarda bu yetersiz ve kullanıcılara düzenleme veya bir toplu kayıt silme olanağı sağlamak ihtiyacımız.

Örneğin, e-posta istemcileri en web tabanlı bir kılavuz her ileti listelemek için burada her satır bir onay e-posta s bilgileri (konu, gönderen ve benzeri) ile birlikte içeren kullanın. Bu arabirim kullanıcının birden fazla ileti bunları denetleniyor ve ardından Seçili iletileri Sil düğmesini tıklatarak silmesine izin verir. Arabirim düzenleme toplu kullanıcıların yaygın olarak birçok farklı kaydını düzenlemek burada durumlarda idealdir. Kullanıcının'ı zorlama yerine düzenleme, kullanıcıların değişiklik ve değiştirilmesi gereken her kayıt için Güncelleştir'i tıklatın, her satırın düzenleme arabirimiyle arabirimini düzenleme toplu işler. Kullanıcı, hızlı bir şekilde değiştirilmesi gereken satır kümesini değiştirin ve Tümünü Güncelleştir düğmesini tıklatarak bu değişiklikleri kaydedin. Bu öğreticiler kümesinde biz ekleme, düzenleme ve veri toplu silme arabirimlerin nasıl oluşturulacağı inceleyeceğiz.

Toplu işlemleri gerçekleştirirken, bazı işlemlerin başarılı olması için Toplu işlemdeki diğer while için bunu mümkün olup olmaması gerektiğini belirlemek önemli s başarısız. İlk seçili kaydı başarıyla silindi, ancak İkincisi, söyleyin, yabancı anahtar kısıtlaması ihlali nedeniyle başarısız olursa ne olacağını arabirimi - silme toplu göz önünde bulundurun? İlk kaydı s silme geri alınması yoksa ilk kaydı silinen kalması kabul edilebilir mı?

Toplu işlem göreceğini isteyip istemediğinizi bir [atomik işlemi](http://en.wikipedia.org/wiki/Atomic_operation), bir burada ya da tüm adımlar başarılı veya tüm adımları başarısız ve veri erişim katmanı için destek eklemek için Genişletilebilir gerekiyor [veritabanı İşlemler](http://en.wikipedia.org/wiki/Database_transaction). Veritabanı işlemleri garanti kümesinin kararlılık `INSERT`, `UPDATE`, ve `DELETE` deyimleri işlem şemsiyesi yürütülen ve çoğu tüm modern veritabanı sistemleri tarafından desteklenen bir özelliğidir.

Bu öğreticide biz veritabanı işlemleri kullanmak için DAL genişletmek ne göreceğiz. Sonraki öğreticilerde uygulayan web sayfaları ekleme, güncelleştirme ve silme arabirimleri toplu için inceleyeceksiniz. Let s başlayın!

> [!NOTE]
> Bir toplu işlemde verileri değiştirirken kararlılık her zaman gerekli değildir. Bazı senaryolarda, bazı veri değişikliklerinin başarılı olması için kabul edilebilir olabilir ve diğerleri aynı toplu iş zaman gibi başarısız bir web tabanlı e-posta istemcisinden e-postaları kümesi siliniyor. Bir veritabanı hatası yarıda silme aracılığıyla orada s işlem, bu s hatasız işlenen kayıtları silinen kalmasını büyük olasılıkla kabul edilebilir. Böyle durumlarda, DAL veritabanı işlemleri desteklemek için değişiklik gerekmez. Kararlılık önemli olduğu diğer toplu işlem senaryolar, ancak vardır. Bir müşteri kendi fon bir banka hesabından diğerine taşındığında, iki işlem gerçekleştirilmelidir: fon ilk hesabından kesinti ve ikinci eklenir. Banka başarılı ilk adımı sahip şunları aklınızda değil ancak ikinci adım başarısız olsa da, müşterilerinin understandably upset olacaktır. Bu öğreticide çalışma ve geliştirmeler ekleme toplu kullanarak, güncelleştirme ve biz aşağıdaki üç öğreticilerde derleme arabirimleri silme düşünmüyorsanız bile, veritabanı işlemleri desteklemek için DAL uygulamak için önerilir.


## <a name="an-overview-of-transactions"></a>İşlemleri genel bakış

Çoğu veritabanları için destek içerir *işlemleri*, tek bir mantıksal birimde iş gruplandırılmasını birden çok veritabanı komutları etkinleştirin. Bir işlem oluşturan veritabanı komutları tüm komutları başarısız olur veya tüm başarılı olur anlamına gelen, atomik olması garanti.

Genel olarak, işlemleri şu biçimi kullanarak SQL deyimlerini uygulanır:

1. Bir işlem başlangıcını gösterir.
2. İşlem oluşturan SQL deyimlerini yürütün.
3. Adım 2 ', geri alma işlemi deyimleri birinde bir hata varsa.
4. Adım 2'deyimleri tümünün hatasız tamamlarsanız, işlem uygulayın.

Oluşturmak için kullanılan SQL deyimlerini yürütün ve işlem girilebilir el ile saklı yordamlar SQL komut dosyası yazma veya oluşturma sırasında geri veya ADO.NET ya da sınıflarını kullanarak program aracılığıyla anlamına gelir [ `System.Transactions` ad alanı](https://msdn.microsoft.com/library/system.transactions.aspx). Bu öğreticide yalnızca inceleyeceğiz ADO.NET kullanarak işlemleri yönetme. Sonraki öğreticide saklı yordamlar aynı zamanda biz oluşturma, geri alma ve işlem yürüten SQL deyimlerini ele alacağız veri erişim katmanı kullanmak nasıl ele alacağız. Bu arada, başvurun [SQL Server saklı yordamlar yönetme işlemlerinde](http://www.4guysfromrolla.com/webtech/080305-1.shtml) daha fazla bilgi için.

> [!NOTE]
> [ `TransactionScope` Sınıfı](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) içinde `System.Transactions` ad alanı programlı olarak bir dizi deyimi bir işlem kapsamı içinde sarmalamak için geliştiricilere olanak sağlar ve birden çok ilgili karmaşık işlemleri için destek içerir iki farklı veritabanları veya bir Microsoft SQL Server veritabanı, bir Oracle veritabanına ve bir Web hizmeti gibi veri depolarına bile heterojen türleri gibi kaynakları. I ullanıcı karar yerine Bu öğretici için ADO.NET hareketleri kullanmak `TransactionScope` sınıfı ADO.NET veritabanı işlemleri için ve çoğu durumda, daha özel olduğundan çok daha az kaynak yoğundur. Ayrıca, bazı senaryolarla `TransactionScope` sınıfı, Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) kullanır. Yapılandırma, uygulama ve performans sorunlarını çevresindeki MSDTC yerine özel ve gelişmiş bir konu kolaylaştırır ve bu öğreticileri kapsamı dışında.


ADO.NET SqlClient sağlayıcısı ile çalışırken, işlemleri çağrısıyla başlatılan [ `SqlConnection` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), döndüren bir [ `SqlTransaction` nesne](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Oluşma şekli işlem içinde yerleştirilir veri değişikliği deyimleri bir `try...catch` bloğu. Bir deyimi içinde bir hata oluşursa, `try` engellemek, yürütme aktarımları için `catch` burada hareket geri aracılığıyla blok `SqlTransaction` s nesnesi [ `Rollback` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Tüm ifadeler başarıyla tamamlanması için bir çağrı varsa `SqlTransaction` s nesnesi [ `Commit` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) sonunda `try` blok işlem kaydeder. Aşağıdaki kod parçacığını bu deseni gösterilmektedir. Bkz: [Bakımı veritabanı tutarlılığını hareketlerle](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) ek sözdizimi ve örnekleri işlemleri ADO.NET ile kullanma.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Varsayılan olarak, yazdığınız kümesindeki TableAdapters işlemleri kullanmayın. Bir işlem kapsamı içinde veri değişikliği bildirimlerini bir dizi gerçekleştirmek için yukarıdaki desenini kullanan ek yöntemleri dahil etmek için TableAdapter sınıfları büyütmek için ihtiyacımız işlemleri için destek sağlamak için. Kısmi sınıflar bu yöntemleri eklemek için nasıl kullanılacağını adım 2'de göreceğiz.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>1. adım: toplu veri Web sayfalarıyla çalışan oluşturma

Veritabanı işlemleri desteklemek için DAL büyütmek üzere nasıl keşfetme başlamadan önce öncelikle Bu öğretici için ihtiyacımız ASP.NET web sayfaları ve izleyin ve üç oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `BatchData` ve her bir sayfa ile ilişkilendirme aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![ASP.NET sayfaları için SqlDataSource ile ilgili öğreticiler ekleme](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Şekil 1**: SqlDataSource ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer bir klasörleri olduğu gibi `Default.aspx` kullanacağı `SectionLevelTutorialListing.ascx` öğreticileri, bölüm içindeki listelemek için kullanıcı denetimi. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Son olarak, bu dört sayfaları girişlere ekleyin `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirmeyi özelleştirme sonra eklemeniz Site Haritası `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık toplu veri öğreticileri çalışmak için öğeleri içerir.


![Site Haritasını toplu veri öğreticileri çalışmak için girişleri şimdi içerir.](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Şekil 3**: Site Haritası toplu veri öğreticileri çalışmak için girişleri şimdi içerir.


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>2. adım: Veritabanı işlemleri desteklemek için veri erişim katmanı güncelleştiriliyor

Biz geri ilk öğreticide anlatıldığı gibi [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md), bizim DAL yazılan veri kümesini DataTables ve TableAdapters oluşur. DataTables veri tutarken TableAdapters DataTables ve benzeri yapılan değişikliklerle veritabanını güncelleştirmek için DataTables içine veritabanından veri okumak için işlevsellik sağlar. TableAdapters t için toplu güncelleştirme ve DB doğrudan olarak adlandırılan verileri güncelleştirmek için iki desenleri sağlamak geri çağırma. Toplu güncelleştirme desenle TableAdapter DataSet, DataTable ya da DataRow koleksiyonu geçirilir. Bu veri numaralandırılır ve her biri için eklenen, değiştirilen veya satır, silinen `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` yürütülür. DB doğrudan desenle TableAdapter yerine sütun ekleme, güncelleştirme veya tek bir kaydını silmek için gerekli değerleri geçirilir. DB doğrudan düzeni yöntemi sonra uygun yürütmek için geçirilen bu değerleri kullanır `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` deyimi.

Kullanılan güncelleştirme Düzen bağımsız olarak işlemleri otomatik olarak oluşturulan TableAdapters yöntemleri kullanmayın. Varsayılan olarak her INSERT, update veya delete TableAdapter tarafından gerçekleştirilen tek ayrık bir işlem olarak kabul edilir. Örneğin, DB doğrudan düzeni tarafından BLL bazı kodda on kayıt veritabanına eklemek için kullanılan olduğunu düşünün. Bu kod TableAdapter s çağırırdı `Insert` yöntemi on kez. İlk beş eklemeleri başarılı, ancak altıncı tek bir özel durumla sonuçlandı, ilk beş eklenen kayıtlar veritabanında kalır. Benzer şekilde, toplu güncelleştirme düzeni ekler gerçekleştirmek için kullanılırsa, güncelleştirme ve silme için eklenen, değiştirilen ve bir DataTable tablosundaki satırları ilk çeşitli değişiklikler başarılı oldu, ancak daha sonraki bir önceki bu değişiklikleri bir hata ile karşılaştı silindiği, Tamamlanan veritabanında kalır.

Bazı senaryolarda kararlılık değişiklikleri bir dizi arasında emin olmak istiyoruz. Biz el ile genişlemelidir TableAdapter yürütme yeni yöntemleri ekleyerek bunu gerçekleştirmek için `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` bir işlem şemsiyesi altındaki s. İçinde [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md) kullanarak Aranan [kısmi sınıflar](http://en.wikipedia.org/wiki/Partial_type) yazılan DataSet içindeki DataTable işlevselliğini genişletmek için. Bu teknik TableAdapters ile de kullanılabilir.

Yazılan veri kümesi `Northwind.xsd` bulunan `App_Code` s klasörü `DAL` alt klasörü. Bir alt klasör oluşturun `DAL` adlı klasörü `TransactionSupport` ve adlı yeni bir sınıf dosyası ekleyin `ProductsTableAdapter.TransactionSupport.cs` (Şekil 4'e bakın). Bu dosya kısmi uygulanması tutacak `ProductsTableAdapter` bir işlemi kullanarak veri değişiklikleri gerçekleştirmek için yöntemler içerir.


![TransactionSupport adlı bir klasör ve ProductsTableAdapter.TransactionSupport.cs adlı bir sınıf dosyası ekleme](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Şekil 4**: adlı bir klasör ekleme `TransactionSupport` ve adlı bir sınıf dosyası `ProductsTableAdapter.TransactionSupport.cs`


Aşağıdaki kodu girin `ProductsTableAdapter.TransactionSupport.cs` dosyası:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

`partial` Burada sınıf bildirimindeki anahtar sözcüğü gösterir eklenecek içinde eklenen üye olan derleyici `ProductsTableAdapter` sınıfını `NorthwindTableAdapters` ad alanı. Not `using System.Data.SqlClient` deyimini dosyanın üst. TableAdapter SqlClient sağlayıcısı kullanacak şekilde yapılandırıldıktan sonra dahili olarak kullandığı bir [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) kendi komutları veritabanına vermek için nesne. Sonuç olarak, kullanılacak ihtiyacımız `SqlTransaction` işlem başlamak için sınıf ve ardından onu yürütme veya geri alma için. Microsoft SQL Server dışındaki bir veri deposu kullanıyorsanız, uygun sağlayıcıyı kullanmanız gerekir.

Bu yöntemler, geri alma, başlatmak için gereken yapı taşlarını sağlar ve bir işlem uygulayın. Bunlar işaretlenmiş `public`, bunları içinden kullanılacak etkinleştirme `ProductsTableAdapter`, DAL başka bir sınıf ya da mimarisinde BLL gibi başka bir katmandan. `BeginTransaction` İç TableAdapter s açılır `SqlConnection` (gerekirse), işlem başlar ve atar `Transaction` özelliği, hareket iç ağa bağlayan `SqlDataAdapter` s `SqlCommand` nesneleri. `CommitTransaction` ve `RollbackTransaction` çağrısı `Transaction` s nesnesi `Commit` ve `Rollback` yöntemleri, sırasıyla iç kapatmadan önce `Connection` nesnesi.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>3. adım: Güncelleştirme ve bir işlem Şemsiyesi altındaki verilerini silmek için yöntemler ekleme

Bu yöntemler tam, biz re hazır yöntemleri eklemek için `ProductsDataTable` veya bir dizi komut bir işlem şemsiyesi altındaki gerçekleştirmek BLL. Aşağıdaki yöntemi güncelleştirmek için toplu güncelleştirme desen kullanan bir `ProductsDataTable` bir işlem kullanarak örneği. Çağırarak bir işlem başlatır `BeginTransaction` yöntemi ve kullandığı bir `try...catch` veri değişikliği deyimleri vermek için blok. Varsa çağrısı `Adapter` s nesnesi `Update` yöntemde bir özel durum, yürütme transfer edeceğini için `catch` burada işlem geri alınacak bloğu ve yeniden oluşturulan özel durum. Sözcüğünün `Update` yöntemi sağlanan satırlarını numaralandırarak toplu güncelleştirme deseni uygular `ProductsDataTable` ve gerekli gerçekleştirme `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` s. Bu komutları sonuçlar hata herhangi biri olursa, işlem geri, işlem s ömrü boyunca yapılan önceki değişiklikler geri alındı. Gereken `Update` deyimini tamamlamak hatasız, işlem tamamının gerçekleştirilir.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Ekleme `UpdateWithTransaction` yönteme `ProductsTableAdapter` sınıfı kısmi sınıfında üzerinden `ProductsTableAdapter.TransactionSupport.cs`. Alternatif olarak, bu yöntem iş mantığı katmanı s eklenemedi `ProductsBLL` birkaç küçük söz dizimi değişikliklerle sınıfı. Yani, anahtar sözcüğü bu `this.BeginTransaction()`, `this.CommitTransaction()`, ve `this.RollbackTransaction()` ile değiştirilmeleri gerekir `Adapter` (sözcüğünün `Adapter` bir özellik adı `ProductsBLL` türü `ProductsTableAdapter`).

`UpdateWithTransaction` Yöntemi toplu güncelleştirme deseni kullanır, ancak bir dizi DB doğrudan çağrısı aşağıdaki gösterildiği gibi yöntemi bir işlem kapsamı içinde de kullanılabilir. `DeleteProductsWithTransaction` Yöntemi giriş olarak kabul eden bir `List<T>` türü `int`, hangi `ProductID` silmek için s. Yöntem çağrısı aracılığıyla işlemi başlatan `BeginTransaction` , daha sonra `try` engellemek, DB doğrudan düzeni çağırma sağlanan listede tekrarlanan `Delete` yöntemi her `ProductID` değeri. Yapılan çağrıların varsa `Delete` başarısız denetim için aktarılır `catch` burada işlem geri bloğu ve yeniden oluşturulan özel durum. Tüm çağrılar, `Delete` işlem sonra başarılı. Bu yönteme eklemek `ProductsBLL` sınıfı.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Birden çok TableAdapters üzerinden uygulanan işlemler

Bu öğreticide incelenmesi işlem ilgili kod karşı birden çok deyime sağlar `ProductsTableAdapter` atomik bir işlem olarak kabul edilmesi için. Ancak ne birden fazla değişiklik yapılması farklı veritabanı tablolarını otomatik olarak gerçekleştirilmesi gerekir? Örneği için bir kategori silerken, ilk geçerli ürünlerinden başka bir kategoriye atamak istiyoruz. Ürünleri yeniden atama ve kategori silindiğinde bu iki adımı atomik bir işlem olarak yapılmalıdır. Ancak `ProductsTableAdapter` yalnızca değiştirme yöntemlerini içeren `Products` tablo ve `CategoriesTableAdapter` yalnızca değiştirme yöntemleri içerir `Categories` tablo. Nasıl bir işlem hem TableAdapters olabilir?

Bir seçenek olan bir yöntem eklemek için `CategoriesTableAdapter` adlı `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` ve bu yöntem ürünleri yeniden atar hem de kategori saklı yordam içinde tanımlanan bir işlem kapsamı içinde siler bir saklı yordam çağrısı. Başlamak için Yürüt nasıl ve saklı yordamlar geri alma işlemlerinde gelecekteki bir öğreticide inceleyeceğiz.

İçeren DAL bir yardımcı sınıfı oluşturmak için başka bir seçenektir `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` yöntemi. Bir örneğini oluşturur. Bu yöntem `CategoriesTableAdapter` ve `ProductsTableAdapter` ve ardından bu iki TableAdapters `Connection` aynı özelliklerine `SqlConnection` örneği. AT o noktadan iki TableAdapters birini başlatmak çağrısıyla işlem `BeginTransaction`. Ürünleri yeniden atama ve kategoriyi silmek için TableAdapters yöntemleri içinde çağrılan bir `try...catch` işlem yürütülmeli veya geri gerektiğinde with bloğu.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>4. adım: Ekleme`UpdateWithTransaction`iş mantığı katmanı yöntemi

Adım 3'te eklediğimiz bir `UpdateWithTransaction` yönteme `ProductsTableAdapter` DAL içinde. Biz BLL karşılık gelen bir yöntemi eklemeniz gerekir. Sunu katmanı çağırmak için doğrudan DAL aşağıya doğru çağırabilirsiniz sırada `UpdateWithTransaction` yöntemi, bu öğreticileri genişletmeniz sunu katmandan DAL sahibi olma gereksiniminden kurtarır katmanlı bir mimari tanımlamak. Bu nedenle, bu yaklaşım devam etmemizi behooves.

Açık `ProductsBLL` sınıf dosya ve adlı bir yöntem ekleyin `UpdateWithTransaction` yalnızca çağıran karşılık gelen DAL yöntemi aşağı kaydırın. Ayrıca, iki yeni yöntemleri şimdi olmalıdır `ProductsBLL`: `UpdateWithTransaction`, hangi yeni eklediğiniz, ve `DeleteProductsWithTransaction`, adım 3'te eklendi.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Bu yöntemleri dahil etmeyin `DataObjectMethodAttribute` çoğu diğer yöntemleri için atanan öznitelik `ProductsBLL` Biz bu yöntemlerini doğrudan sınıflardan ASP.NET sayfaları arka plan kodu çağırma çünkü sınıfı. Sözcüğünün `DataObjectMethodAttribute` hangi yöntemlerin ObjectDataSource s yapılandırma veri kaynağı Sihirbazı ve hangi sekmesinde (seçin, güncelleştirme, ekleme veya silme) altında görünmelidir bayrak için kullanılır. GridView düzenleme veya silme toplu için herhangi bir yerleşik destek eksik olduğundan, biz bu yöntemleri program aracılığıyla çağırmak yerine Kodsuz bildirim temelli yaklaşımı kullanmak zorunda kalırsınız.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>5. adım: Sunu katmanı veritabanı verilerinden otomatik olarak güncelleştiriliyor

Let s işlem kayıtları toplu güncelleştirirken etkisini anlamak için tüm ürünler GridView, listeler ve düğmesi Web içeren bir kullanıcı arabirimi oluşturma tıklatıldığında kontrol eden, ürünleri yeniden atar `CategoryID` değerleri. İlk birkaç ürünleri geçerli bir kalmayacak şekilde özel olarak, kategori yeniden atama ilerleyeceğini `CategoryID` başkalarının var durumdayken değeri atanmış mevcut olmayan `CategoryID` değeri. Veritabanı olan bir ürün güncelleştirme denemesi biz, `CategoryID` var olan bir kategori s eşleşmiyor `CategoryID`, yabancı anahtar kısıtlaması ihlali ortaya çıkar ve özel durum oluşturuldu. Bir işlem özel durum yabancı anahtar kısıtlaması ihlali oluştu kullanarak önceki neden olur, bu örnekte ne göreceğiz olan geçerli `CategoryID` değişiklikler geri alındı. Ancak, bir işlem kullanmadığınızda, ilk kategorileri için yapılan değişiklikleri kalmaya devam edecek.

Başlangıç açarak `Transactions.aspx` sayfasındaki `BatchData` klasörü ve araç tasarımcıya GridView sürükleyin. Ayarlama, `ID` için `Products` ve akıllı etiketten adlı yeni bir ObjectDataSource bağlayın `ProductsDataSource`. ObjectDataSource kendi verisinden isteyecek şekilde yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu salt okunur GridView olması, bu nedenle güncelleştirme, ekleme, açılan listeleri ayarlamak ve sekmeleri (hiçbiri) silecek ve bu Son'u tıklatın.


[![Şekil 5: ObjectDataSource s ProductsBLL sınıfı GetProducts yöntemi kullanmak üzere yapılandırma](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Şekil 5**: Şekil 5: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Şekil 6**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve ürün veri alanları için CheckBoxField oluşturur. Dışında bu alanların tümünü Kaldır `ProductID`, `ProductName`, `CategoryID`, ve `CategoryName` ve yeniden adlandırma `ProductName` ve `CategoryName` BoundFields `HeaderText` özellikleri ürün ve kategorisi, sırasıyla. Akıllı etiketten etkinleştirmek disk belleği seçeneği işaretleyin. Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Ardından, yukarıdaki GridView üç düğme Web denetimleri ekleyin. Kılavuzu Yenile, ikinci s kategorilerine (işlem) ile değiştirin ve üçüncü bir s kategorileri (olmadan işlem) değiştirmek için ilk düğme s metin özelliğini ayarlayın.


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

Bu noktada Visual Studio Tasarım görünümünde Şekil 7'de gösterilen ekran benzer görünmelidir.


[![Sayfa GridView ve üç düğme Web denetimleri içeriyor](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Şekil 7**: sayfası GridView ve üç düğme Web denetimleri içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Her üç düğme s için olay işleyicileri oluşturma `Click` olaylar ve aşağıdaki kodu kullanın:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

Yenile düğmesini s `Click` rebinds yalnızca GridView verileri olay işleyicisi çağrılarak `Products` GridView s `DataBind` yöntemi.

İkinci olay işleyicisi ürünleri yeniden atar `CategoryID` s ve BLL veritabanı gerçekleştirmek için yeni işlem yönteminden güncelleştirmeleri bir işlem şemsiyesi kullanır. Unutmayın her ürünün s `CategoryID` rasgele aynı değere ayarlanır kendi `ProductID`. Bu ürünler olduğundan bu ince ilk birkaç ürünleri çalışır `ProductID` geçerli eşlemek için durum değerleri `CategoryID` s. Ancak bir kez `ProductID` çok fazla büyümesini s başlatmak, bu içerik olarak farklı çakışmasını `ProductID` s ve `CategoryID` s artık uygular.

Üçüncü `Click` olay işleyicisi güncelleştirmeleri ürünleri `CategoryID` gönderir ancak güncelleştirme s aynı şekilde kullanarak veritabanı `ProductsTableAdapter` s varsayılan `Update` yöntemi. Bu `Update` yöntemi bir işlem içinde komutları dizi sarmalamak değil, bu değişiklikleri önce ilk karşılaşılan yabancı anahtar kısıtlaması ihlali hatası yapılan şekilde korunur.

Bu davranış göstermek için bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Başlangıçta verileri'nın ilk sayfasında Şekil 8'de gösterildiği gibi görmeniz gerekir. Ardından, değişiklik kategorileri (ile işlem) düğmesine tıklayın. Bu geri gönderimin neden ve tüm ürünleri güncelleştirme denemesi `CategoryID` değerleri, ancak bir yabancı anahtar kısıtlaması ihlali neden olur (bkz. Şekil 9).


[![Bir disk belleğine alınabilir GridView ürünleri görüntülenir](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Şekil 8**: ürünler, bir disk belleğine alınabilir GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Bir yabancı anahtar kısıtlaması ihlali kategorileri sonuçlarında yeniden atama](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Şekil 9**: bir yabancı anahtar kısıtlaması ihlali kategorileri sonuçlarında yeniden atama ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Şimdi, tarayıcı s geri düğmesine basın ve ardından Kılavuzu Yenile düğmesini tıklatın. Veri yenileme sırasında tam aynı çıktı Şekil 8'de gösterildiği gibi görmeniz gerekir. Diğer bir deyişle, hatta bazı ürünler olsa `CategoryID` s yasal için değiştirilmiş değerleri olan ve geri yabancı anahtar kısıtlaması ihlali oluştu zaman veritabanında güncelleştirilen, bunlar alındı.

Şimdi değiştir kategorileri (olmadan işlem) düğmesini tıklatarak deneyin. Bu aynı yabancı anahtar kısıtlaması ihlali hataya neden olur (bkz. Şekil 9), ancak bu sefer bu ürünler, `CategoryID` değerleri için yasal değiştirildi değeri değil geri alınacak. Tarayıcı s geri düğmesine ve ardından Kılavuzu yenile düğmesine basın. Şekil 10 gösterildiği gibi `CategoryID` ilk sekiz ürünlerin s yeniden. Örneğin, Şekil 8'de izleme sahip bir `CategoryID` 1, ancak Şekil 10 BT s 2'ye yeniden atandığında.


[![Bazı ürünler adlı kullanıcı, Categoryıd'si değerler güncelleştirildi ancak başkalarının olan değil olan](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Şekil 10**: bazı ürünler `CategoryID` değerleri değildi güncelleştirilmiş sırada başkalarının olan ([tam boyutlu görüntüyü görüntülemek için tıklatın](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Özet

Varsayılan olarak, TableAdapter s yöntemleri bir işlem kapsamı içinde yürütülen veritabanı deyimleri sarmalamak değil, ancak küçük bir iş oluşturacak yöntemleri, kaydetme ve geri alma bir işlem ekleyebiliriz. Bu öğreticide, üç tür yöntem oluşturduğumuz `ProductsTableAdapter` sınıfı: `BeginTransaction`, `CommitTransaction`, ve `RollbackTransaction`. Bu yöntemleri ile birlikte kullanmak nasıl gördüğümüz bir `try...catch` bir dizi veri değişikliği deyimi atomik yapmak için blok. Özellikle, oluşturduğumuz `UpdateWithTransaction` yönteminde `ProductsTableAdapter`, sağlanan satırlarını gerekli değişiklikleri yapmak için toplu güncelleştirme desen kullanan `ProductsDataTable`. Ayrıca eklediğimiz `DeleteProductsWithTransaction` yönteme `ProductsBLL` kabul BLL sınıfında bir `List` , `ProductID` değerleri kendi giriş ve DB doğrudan düzeni yöntemini çağırır `Delete` her `ProductID`. Bir işlem oluşturarak ve veri değişikliği deyimleri içinde yürütülen her iki yöntem Başlat bir `try...catch` bloğu. Bir özel durum oluşursa, işlem geri alındı, aksi takdirde taahhüt eder.

Adım 5 işlem toplu güncelleştirmeleri bir işlem kullanılacak ihmal toplu güncelleştirmeler karşı etkisini gösterilmiştir. Sonraki üç eğitimlerine biz Bu öğreticide yerleştirilmiş foundation temel yapı ve toplu güncelleştirme, silme ve eklemeleri gerçekleştirmek için kullanıcı arabirimleri oluşturun.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Veritabanı tutarlılığını işlemleri ile koruma](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Saklı yordamlar SQL Server'daki işlemleri yönetme](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Kolaylaştırılmış işlemler: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope ve DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Oracle veritabanı işlemleri .NET ile kullanma](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Dave Gardner, Hilton Giesenow ve Teresa Murphy yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](batch-updating-cs.md)
