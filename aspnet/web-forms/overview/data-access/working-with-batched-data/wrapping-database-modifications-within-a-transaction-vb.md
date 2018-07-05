---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Veritabanı değişikliklerini bir işlemin (VB) içinde sarmalama | Microsoft Docs
author: rick-anderson
description: Bu öğretici, güncelleştirme, silme ve verileri toplu ekleme sırasında görünen dört ilk bölümüdür. Bu öğreticide veritabanı işlemleri nasıl izin bilgi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: cf87dd0e677a73dc81cc430157be22b01ff0f7a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390168"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>Veritabanı değişikliklerini bir işlemin (VB) içinde sarmalama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) veya [PDF olarak indirin](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Bu öğretici, güncelleştirme, silme ve verileri toplu ekleme sırasında görünen dört ilk bölümüdür. Bu öğreticide veritabanı işlemleri batch değişiklikleri tüm adımları başarılı veya başarısız adımların tümünü sağlar bir atomik işlem olarak gerçekleştirilebilmesi nasıl izin öğrenin.


## <a name="introduction"></a>Giriş

İle başlayarak gördüğümüz gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) öğreticide GridView yerleşik destek sağlar satır düzeyinde düzenleme ve silme için. Fare birkaç tıklama ile bir satır kod yazmadan zengin veri değişikliği arabirimini düzenleme ve silme tek satır başına temelinde ile içerik sürece oluşturmak mümkündür. Ancak, belirli senaryolarda bu yetersiz ve kullanıcılar düzenleme veya bir toplu kayıt silme olanağı sağlamak için ihtiyacımız.

Örneğin, en web tabanlı e-posta istemcilerinin bir kılavuz her ileti listelemek için bulunduğu her satır bir onay e-posta s bilgilerini (konu, gönderen vb.) birlikte içerdiği kullanın. Bu arabirim, kullanıcının birden çok ileti bunları denetleniyor ve ardından Seçili iletileri Sil düğmesini tıklatarak silmesine izin verir. Kullanıcıların yaygın olarak birçok farklı kayıtlara Düzenle burada durumlarda arabirimini düzenleme toplu idealdir. Kullanıcının'ı zorlamak yerine düzenlemek, kendi değişiklik yapın ve değiştirilmesi gereken her bir kayıt için Güncelleştir'e tıklayın, her satır, düzenleme arabirimiyle arabirimini düzenleme toplu işler. Kullanıcı, hızlı bir şekilde değiştirilmesi gereken satır kümesini değiştirin ve sonra bir Tümünü Güncelleştir düğmesine tıklayarak bu değişiklikleri kaydedebilirsiniz. Bu öğretici kümesinde ekleme, düzenleme ve veri toplu silme arabirimler oluşturma inceleyeceğiz.

Toplu işlemleri gerçekleştirirken, bazı işlemlerin başarılı olması için batch'te başkalarının çalışırken için bunu mümkün olup olmayacağını belirlemek önemli s başarısız. İlk seçilen kaydı başarıyla silindi, ancak ikinci, örneğin bir yabancı anahtar kısıtlaması ihlali nedeniyle başarısız olursa ne olacağını Interface - silme toplu göz önünde? İlk kayıt s silme geri alınması veya silinen kalmasına ilk kayıt için kullanılabilir?

Toplu işlem olarak kabul edilmesini isterseniz bir [atomik işlem](http://en.wikipedia.org/wiki/Atomic_operation), bir yere ya da tüm adımları başarılı veya tüm adımları başarısız, ardından veri erişim katmanı için destek eklemek için genişletilmesi gerekir [veritabanı işlem](http://en.wikipedia.org/wiki/Database_transaction). Veritabanı işlemleri yaparak kararlılık kümesinin garanti `INSERT`, `UPDATE`, ve `DELETE` deyimleri işlem terimdir altında yürütülen ve çoğu tüm modern veritabanı sistemleri tarafından desteklenen bir özelliğidir.

Bu öğreticide veritabanı işlemleri kullanmak için DAL genişletmek ne inceleyeceğiz. Sonraki öğreticilerde toplu ekleme, güncelleştirme ve silme arabirimleri uygulayan web sayfaları inceleyeceksiniz. Let s başlayın!

> [!NOTE]
> Bir toplu işlemde verileri değiştirirken, kararlılık her zaman gerekli değildir. Bazı senaryolarda, bazı veri değişikliklerinin başarılı olması için kabul edilebilir olabilir ve diğer aynı toplu iş zamanı gibi başarısız bir web tabanlı e-posta istemcisinden e-postalar bir dizi siliniyor. Orada s silme yoluyla bir veritabanı hatası midway işliyorsa, s hatasız işlenen kayıtları silinen kalmasını büyük olasılıkla kabul edilebilir. Bu gibi durumlarda, DAL veritabanı işlemleri desteklemek için değiştirilmesi gerekmez. Kararlılık çok önemli olduğu diğer toplu işlem senaryo vardır, ancak. Bir müşteri kendi para bir banka hesabından diğerine geçtiğinde, iki işlem gerçekleştirilmelidir: fon ilk hesabından kesilen ve ardından ikinci eklenir. Banka başarılı bir ilk adım olması gerektiğini unutmayın değil ancak ikinci adım başarısız olsa da, müşterilerinin anlaşılır şekilde upset olacaktır. Bu öğreticide çalışabilir ve toplu ekleme kullanarak, güncelleştirme ve biz aşağıdaki üç öğreticilerde oluşturuyor olacaksınız arabirimleri silme düşünmüyorsanız bile, veritabanı işlemleri desteklemek için DAL için iyileştirmeler uygulamak geçmenizi öneriyoruz.


## <a name="an-overview-of-transactions"></a>İşlemleri genel bakış

Çoğu veritabanı desteği dahil *işlemleri*, tek bir mantıksal birimde iş gruplandırılmasını birden çok veritabanı komutlarını etkinleştir. Bir işlemi oluşturan veritabanı komutlarını tüm komutları başarısız olur veya tüm başarılı anlamına gelen atomik olması sağlanır.

Genel olarak, işlem şu biçimi kullanarak SQL deyimleri uygulanır:

1. Bir işlem başlangıcını gösterir.
2. İşlemi oluşturan SQL deyimlerini yürütün.
3. 2. adım, geri alma işlemi deyimlerinden birinde bir hata varsa.
4. Tüm 2. adım deyimlerinin hatasız tamamlanırsa, işlem kaydedilemiyor.

Oluşturmak için kullanılan SQL deyimleri yürütme ve işlem girilebilir el ile saklı yordamlar SQL komut dosyası yazma veya oluşturma, geri alma ya da ADO.NET veya sınıfları kullanarak programlama yoluyla anlamına gelir [ `System.Transactions` ad alanı](https://msdn.microsoft.com/library/system.transactions.aspx). Bu öğreticide yalnızca inceleyeceğiz ADO.NET kullanarak işlemleri yönetme. Bir sonraki öğreticide aynı zamanda SQL deyimlerini oluşturmak, geri alma ve işlem yürüten şunları keşfedeceğiz veri erişim katmanı, saklı yordamlar kullanma atacağız. Bu arada, başvurun [yönetme işlemleri SQL Server saklı yordamları](http://www.4guysfromrolla.com/webtech/080305-1.shtml) daha fazla bilgi için.

> [!NOTE]
> [ `TransactionScope` Sınıfı](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) içinde `System.Transactions` ad alanı, program aracılığıyla bir işlem kapsamında bir dizi deyim sarmalamak geliştiricilerin sağlar ve birden çok ilgili karmaşık işlemleri için desteği içerir iki farklı veritabanları ya da Microsoft SQL Server veritabanı, Oracle veritabanı ve bir Web hizmeti gibi veri depolarında bile heterojen türleri gibi kaynakları. Ben ve karar ADO.NET işlemleri yerine Bu öğretici için kullanılacak `TransactionScope` sınıfı ADO.NET veritabanı işlemleri için ve çoğu durumda, daha belirli olduğundan, çok daha az kaynak yoğundur. Ayrıca, belirli senaryolar altında `TransactionScope` sınıfı Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) kullanır. Yapılandırma, uygulama ve performans sorunlarını çevreleyen MSDTC yerine özelleştirilmiş ve gelişmiş bir konu kolaylaştırır ve bu öğreticileri kapsamı dışında.


ADO.NET SqlClient sağlayıcı ile çalışırken, işlem yapılan bir çağrıyla başlatılır [ `SqlConnection` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), döndüren bir [ `SqlTransaction` nesne](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Düzenini işlem içine yerleştirilir veri değişikliği deyimleri bir `try...catch` blok. Bir deyimde bir hata oluşması halinde `try` engelleme, yürütme aktarmalarıyla `catch` nerede işlem gerçekleştirilen adımların geri alınması aracılığıyla blok `SqlTransaction` s nesnesi [ `Rollback` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Tüm deyimler çağrısı başarıyla tamamlanırsa `SqlTransaction` s nesnesi [ `Commit` yöntemi](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) sonunda `try` blok hareketi tamamlar. Aşağıdaki kod parçacığını bu deseni gösterilmektedir. Bkz: [işlemle veritabanı tutarlılığını sürdürme](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) ek sözdizimi ve ADO.NET ile işlemleri kullanma örnekleri için.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Varsayılan olarak, TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi, işlem kullanmayın. İşlemler için destek sağlamak üzere bir dizi bir işlem kapsamında veri değişikliği deyim gerçekleştirmek için yukarıdaki desen kullanan ek yöntemleri eklemek için TableAdapter sınıflarını genişletmek ihtiyacımız var. Kısmi sınıflar bu yöntemler eklemek için nasıl kullanılacağını adım 2'de göreceğiz.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>1. adım: toplu verileri Web sayfalarıyla çalışma oluşturma

Veritabanı işlemleri desteklemek için DAL artırmak nasıl araştırma başlamadan önce öncelikle Bu öğretici için ihtiyacımız ASP.NET web sayfaları ve izleyen üç oluşturmak için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `BatchData` ve ardından her bir sayfayla ilişkili aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Şekil 1**: SqlDataSource ile ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi ile `Default.aspx` kullanacağı `SectionLevelTutorialListing.ascx` bölümü içinde öğreticileri listelemek için kullanıcı denetimi. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Son olarak, girişleri olarak bu dört sayfalar ekleme `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme özelleştirme sonra eklemeniz Site Haritası `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık toplu veri öğreticiler ile çalışmaya yönelik öğeleri içerir.


![Site Haritası artık toplu veri öğreticiler ile çalışmaya yönelik girişleri içerir](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Şekil 3**: Site Haritası artık toplu veri öğreticiler ile çalışmaya yönelik girişleri içerir


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>2. adım: Veritabanı işlemleri desteklemek için veri erişim katmanı güncelleştiriliyor

İlk öğreticide geri ele aldığımız gibi [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md), DataTables ve TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi de bizim DAL oluşur. DataTable veri tutarak TableAdapter veritabanı DataTables ve benzeri için yapılan değişikliklerle güncelleştirmek için DataTable içine veritabanından verileri okuyamadı işlevselliği sağlar. Ben toplu güncelleştirme ve DB-doğrudan adlandırılan, verileri güncelleştirmek için TableAdapter bağdaştırıcıları iki deseni sağladığını unutmayın. Toplu güncelleştirme deseni, bir veri kümesi, DataTable ya da DataRow koleksiyonunu TableAdapter geçirilir. Bu veriler numaralandırılana ve her biri için eklenen, değiştirilecek veya satır, silinecek `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` yürütülür. DB-doğrudan desenle TableAdapter yerine sütun ekleme, güncelleştirme veya tek bir kaydı silmek için gerekli değerleri geçirilir. DB doğrudan deseni yöntem uygun yürütmek için geçilen değerleri ardından kullanır. `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` deyimi.

Kullanılan güncelleştirme Düzen bağımsız olarak, otomatik olarak oluşturulan TableAdapter yöntemleri işlemleri kullanmayın. Varsayılan olarak her INSERT, update veya delete TableAdapter tarafından gerçekleştirilen tek bir ayrı işlem olarak kabul edilir. Örneğin, DB doğrudan deseni BLL bazı kod tarafından on kayıt veritabanına eklemek için kullanılan olduğunu düşünün. Bu kod, TableAdapter s çağıracak `Insert` on kez yöntemi. İlk beş ekler başarılı, ancak altıncı tek bir özel durum ile sonuçlandı. eklenen ilk beş kaydı veritabanında kalır. Benzer şekilde, toplu güncelleştirme desenini ekler gerçekleştirmek için kullanılırsa, güncelleştirme ve silme için eklenen, değiştirilebilir ve ilk birkaç değişiklik başarılı oldu, ancak daha sonraki bir önceki söz konusu değişiklikler hatayla karşılaşıldı, bir DataTable tablosundaki satırları silindi, Tamamlanan veritabanında kalır.

Belirli senaryolarda kararlılık değişiklikleri bir dizi emin olmak istiyoruz. Size gereken el ile genişletmek TableAdapter yürütülen yeni yöntemler ekleyerek bunu sağlamak için `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` s genel bir işlem altında. İçinde [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md) kullanarak incelemiştik [kısmi sınıflar](http://en.wikipedia.org/wiki/Partial_type) türü belirtilmiş veri kümesi içindeki DataTable genişletmek için. Bu teknik, TableAdapter'ları ile de kullanılabilir.

Türü belirtilmiş veri kümesi `Northwind.xsd` bulunan `App_Code` s klasörü `DAL` alt. Bir alt klasöre oluşturma `DAL` adlı klasöre `TransactionSupport` ve adlı yeni bir sınıf dosyası ekleyin `ProductsTableAdapter.TransactionSupport.vb` (bkz: Şekil 4). Bu dosya, kısmi uygulanması tutacak `ProductsTableAdapter` veri değişiklikleri kullanarak bir işlem gerçekleştirmek için yöntemler içerir.


![TransactionSupport adlı bir klasör ve ProductsTableAdapter.TransactionSupport.vb adlı bir sınıf dosyası ekleyin](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Şekil 4**: adlı bir klasör ekleme `TransactionSupport` ve adlı bir sınıf dosyası `ProductsTableAdapter.TransactionSupport.vb`


Aşağıdaki kodu girin `ProductsTableAdapter.TransactionSupport.vb` dosyası:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial` Burada sınıf bildirimindeki anahtar sözcüğü gösterir içinde eklenen üyeler eklenecek olan derleyici `ProductsTableAdapter` sınıfını `NorthwindTableAdapters` ad alanı. Not `Imports System.Data.SqlClient` deyimini dosyanın üst. TableAdapter SqlClient sağlayıcısı kullanacak şekilde yapılandırıldıktan sonra dahili olarak kullandığı bir [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) veritabanına komutlarının vermek için nesne. Sonuç olarak, kullanılacak ihtiyacımız `SqlTransaction` işlemi başlatamadığı için sınıf ve işleyin veya geri alma. Microsoft SQL Server farklı bir veri deposu kullanıyorsanız, uygun sağlayıcıyı kullanmak gerekir.

Bu yöntemler, geri alma, başlatmak için gereken yapı taşlarını sağlar ve bir işlem kaydedilemiyor. İşaretlenmiş olsa `Public`, bunları içinden kullanılacak etkinleştirme `ProductsTableAdapter`, DAL başka bir sınıf veya başka bir katman mimarisinde BLL gibi. `BeginTransaction` İç TableAdapter s açılır `SqlConnection` (gerekirse), işlem başlar ve atar `Transaction` özelliği ve işlem iç ağa bağlanan `SqlDataAdapter` s `SqlCommand` nesneleri. `CommitTransaction` ve `RollbackTransaction` çağrı `Transaction` s nesnesi `Commit` ve `Rollback` yöntemlerini sırasıyla iç kapatmadan önce `Connection` nesne.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>3. adım: Güncelleştirme ve verileri, bir işlemin genel altında silmek için yöntemler ekleme

Bu yöntemleri tam, biz re hazır yöntemleri eklemek için `ProductsDataTable` ya da bir işlemin genel altındaki komutları bir dizi gerçekleştirmek BLL. Aşağıdaki yöntemi güncelleştirmek için toplu güncelleştirme desen kullanan bir `ProductsDataTable` kullanarak bir işlem örneği. Çağırarak bir hareket başlatır `BeginTransaction` yöntemi ve kullandığı bir `Try...Catch` veri değişikliği ifadeleri oluşturmak için blok. Çağrı `Adapter` s nesnesi `Update` yöntemde özel durum, yürütme transfer edeceğini için `catch` burada işlem geri alınacak bloğu ve yeniden oluşturulan özel durum. Sözcüğünün `Update` yöntemini uygulayan toplu güncelleştirme deseni sağlanan satırlarını numaralandırarak `ProductsDataTable` ve gerekli gerçekleştirme `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` s. Bu komutları sonuçları hata herhangi biri varsa, işlem geri, işlem s ömrü boyunca yapılan önceki değişiklikler geri alınır. Gereken `Update` deyimini hatasız tamamlamak, işlem sunabilen kararlıdır.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Ekleme `UpdateWithTransaction` yönteme `ProductsTableAdapter` sınıfı kısmi class içinde aracılığıyla `ProductsTableAdapter.TransactionSupport.vb`. Alternatif olarak, bu yöntem iş mantığı katmanı s eklenemedi `ProductsBLL` birkaç küçük söz dizimi değişikliğiyle sınıfı. Yani, anahtar sözcüğü `Me` içinde `Me.BeginTransaction()`, `Me.CommitTransaction()`, ve `Me.RollbackTransaction()` ile değiştirilmesi gerekecek `Adapter` (sözcüğünün `Adapter` bir özelliğin adı `ProductsBLL` türü `ProductsTableAdapter`).

`UpdateWithTransaction` Yöntemi toplu güncelleştirme deseni kullanır, ancak bir dizi DB doğrudan çağrıları aşağıdaki yöntemi gösterildiği gibi bir işlem kapsamı içinde de kullanılabilir. `DeleteProductsWithTransaction` Yöntemi giriş olarak kabul bir `List(Of T)` türü `Integer`, hangi `ProductID` s silinemedi. Yöntem çağrısı aracılığıyla işlem başlatır `BeginTransaction` ve daha sonra `Try` engelleme, DB doğrudan deseni çağırma sağlanan listede ilerler `Delete` yöntemi her `ProductID` değeri. Çağrıları varsa `Delete` başarısız denetimi aktarılır `Catch` blok burada işlem geri alındı ve yeniden oluşturulan özel durum. Tüm çağrılar, `Delete` işlem, daha sonra başarılı. Bu yönteme ekleme `ProductsBLL` sınıfı.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Birden çok TableAdapter'ları arasında uygulanan işlemler

Bu öğreticide incelenir işlemle ilgili kodu birden çok deyime karşı sağlar `ProductsTableAdapter` atomik işlem kabul edilmesi için. Ancak, birden çok değişiklik farklı veritabanı tablolarında atomik olarak gerçekleştirilecek ihtiyacım olursa ne? Örneği için bir kategori silerken biz öncelikle geçerli ürünlerinden için başka bir kategori atamak isteyebilirsiniz. Kategori siliniyor ve ürünleri elemanına bu iki adımı, bir atomik işlem olarak yürütülmelidir. Ancak `ProductsTableAdapter` yalnızca değiştirme yöntemlerini içeren `Products` tablo ve `CategoriesTableAdapter` yalnızca değiştirme yöntemlerini içeren `Categories` tablo. Nasıl bir işlem hem TableAdapters olabilir?

Bir seçenek olan bir yöntem eklemek için `CategoriesTableAdapter` adlı `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` ve hem ürünleri yeniden atar ve saklı yordam içinde tanımlanan bir işlem kapsamında kategoriyi siler saklı bir yordam çağırma bu yöntem. Başlamak için işleme nasıl ve saklı yordamlarda geri alma işlemleri bir sonraki öğreticide inceleyeceğiz.

Yardımcı bir sınıf içeren bir DAL oluşturmak için başka bir seçenektir `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` yöntemi. Bu yöntem bir örneğini oluşturacak `CategoriesTableAdapter` ve `ProductsTableAdapter` ve sonra bu iki TableAdapters ayarlayın `Connection` aynı özellikleri `SqlConnection` örneği. AT o noktada iki TableAdapters birini başlatmak işlem çağrısı ile `BeginTransaction`. Kategori siliniyor ve ürünleri yeniden atama için TableAdapter yöntemleri içinde çağrılmasına bir `Try...Catch` blok hareket kaydedilmiş veya geri gerektiği şekilde alındı.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>4. adım: Ekleme`UpdateWithTransaction`iş mantığı katmanı yöntemi

3. adımda eklediğimiz bir `UpdateWithTransaction` yönteme `ProductsTableAdapter` DAL içinde. Biz BLL için karşılık gelen bir yöntemi eklemeniz gerekir. Sunu katmanını çağırmak için doğrudan DAL aşağı çağırabilir sırada `UpdateWithTransaction` yöntemi, bu öğreticileri genişletmeniz sunu katmanını gelen DAL korunmasını sağlar katmanlı bir mimari tanımlamak. Bu nedenle, bu yaklaşım devam etmek için bize behooves.

Açık `ProductsBLL` sınıf dosyası ve adlı bir yöntem ekleyin `UpdateWithTransaction` yalnızca çağıran karşılık gelen DAL yöntemi gösteriyor. Ayrıca iki yeni yöntemleri artık olmalıdır `ProductsBLL`: `UpdateWithTransaction`, hangi yeni eklediğiniz, ve `DeleteProductsWithTransaction`, adım 3'te eklenmiştir.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Bu yöntemleri dahil etmeyin `DataObjectMethodAttribute` çoğu diğer yöntemleri için atanan öznitelik `ProductsBLL` çünkü Biz bu yöntemleri doğrudan sınıflardan ASP.NET sayfaları arka plan kod yürütmesini. Bu geri çağırma `DataObjectMethodAttribute` yöntemleri ObjectDataSource s'te yapılandırma veri kaynağı Sihirbazı ve hangi sekme (SELECT, UPDATE, INSERT veya DELETE) altında görünmelidir bayrağı için kullanılır. Toplu düzenleme veya silme için herhangi bir yerleşik destek GridView eksik olduğundan, biz program aracılığıyla bu yöntemleri çağırmak yerine kod gerektirmeyen bildirim temelli bir yaklaşım kullanmak zorunda kalırsınız.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>5. adım: Veritabanı verileri sunu katmanını atomik olarak güncelleştiriliyor.

Let s işlem kayıtları toplu güncelleştirme yapılırken etkisini göstermeye GridView tüm ürünleri listeler ve bir düğmeyi Web içeren bir kullanıcı arabirimi oluşturma, denetim tıklandığında ürünleri yeniden atar `CategoryID` değerleri. Özellikle, ilk birden çok ürünlerin geçerli bir kalmayacak şekilde kategorisi yeniden atama ilerleyeceğine dair `CategoryID` başkalarının kullanılamıyor.%n%nÇözüm çalışırken değer atanmış var olmayan bir `CategoryID` değeri. Biz bir ürünle veritabanını güncellemek çalışırsanız, `CategoryID` var olan bir kategori s eşleşmiyor `CategoryID`, yabancı anahtar kısıtlaması ihlali ortaya çıkar ve bir özel durum oluşturulur. Bir yabancı anahtar kısıtlaması ihlali oluşturulan özel durum işlemi kullanarak önceki neden olur, bu örnekte ne görüyoruz olan geçerli `CategoryID` değişiklik geri alınamaz. Ancak, bir işlem kullanmadığınız durumlarda ilk kategorileri için yapılan değişiklikleri kalır.

Başlangıç açarak `Transactions.aspx` sayfasını `BatchData` klasörü ve GridView tasarımcı araç kutusundan sürükleyin. Ayarlama, `ID` için `Products` ve isteğe bağlı olarak, akıllı etiketten adlı yeni bir ObjectDataSource bağlama `ProductsDataSource`. ObjectDataSource kendi verileri çekmek için yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Bu salt okunur GridView olması, bu nedenle açılan listeler, ekleme, güncelleştirme ayarlayın ve sekmeleri (hiçbiri) SİLİN ve Son'a tıklayın.


[![ObjectDataSource s ProductsBLL sınıfı GetProducts yöntemi kullanmak üzere yapılandırma](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Şekil 6**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio BoundFields ve ürün veri alanları için bir CheckBoxField oluşturur. Tüm dışında bu alanlardan kaldırmak `ProductID`, `ProductName`, `CategoryID`, ve `CategoryName` ve yeniden adlandırma `ProductName` ve `CategoryName` BoundFields `HeaderText` ürün ve kategorisi, özellikleri sırasıyla. Akıllı etiket etkinleştirme disk belleği seçeneği işaretleyin. Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirmeyi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Ardından, yukarıdaki GridView üç düğme Web denetimleri ekleyin. İlk düğmeyi s metin özelliği Kılavuzu Yenile ve ikinci s kategorileri (işlem) ile değiştirmek için değiştirme kategorileri (olmadan işlem) üçüncü bir s ayarlayın.


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

Bu noktada Visual Studio Tasarım görünümünde, Şekil 7'de gösterilen ekran şuna benzemelidir.


[![Sayfa GridView ve üç düğme Web denetimleri içerir.](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Şekil 7**: sayfa GridView ve üç düğme Web denetimleri içeriyor ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Her üç düğme s için olay işleyicileri oluşturma `Click` olayları ve aşağıdaki kodu kullanın:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Yenile düğmesini s `Click` rebinds yalnızca GridView verileri olay işleyicisini çağırarak `Products` GridView s `DataBind` yöntemi.

İkinci olay işleyicisi ürünleri yeniden atar `CategoryID` s ve BLL veritabanı gerçekleştirmek için yeni işlem yöntemden bir işlemin genel altında güncelleştirmeleri kullanır. Unutmayın her ürün s `CategoryID` rasgele olarak aynı değeri olarak ayarlanır, `ProductID`. Bu ürünlerin sahip olduğundan bu ince ilk için az sayıda ürün çalışır `ProductID` geçerli eşlemek için durum değerleri `CategoryID` s. Ancak bir kez `ProductID` çok fazla büyümesini s başlatmak, bu içerik olarak farklı çakışmasını `ProductID` s ve `CategoryID` s artık geçerlidir.

Üçüncü `Click` olay işleyicisi ürünlerin güncelleştirmeleri `CategoryID` gönderir ancak güncelleştirme aynı şekilde s kullanarak veritabanına `ProductsTableAdapter` s varsayılan `Update` yöntemi. Bu `Update` yöntemi bir işlem içinde komut dizisi kaydırılacak, önce ilk karşılaşılan yabancı anahtar kısıtlaması ihlali hatası Bu değişiklikler için açık kalır.

Bu davranış göstermek için bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Başlangıçta ilk sayfasında veri Şekil 8'de gösterildiği gibi görmeniz gerekir. Ardından, değişiklik kategorileri (ile işlem) düğmesine tıklayın. Bu bir geri göndermeye neden olur ve tüm ürünleri güncelleştirme denemesi `CategoryID` değerleri, ancak bir yabancı anahtar kısıtlaması ihlali neden olur (bkz. Şekil 9).


[![Ürünleri alınabilir GridView içinde görüntülenir.](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Şekil 8**: ürünleri alınabilir GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Bir yabancı anahtar kısıtlaması ihlali kategorileri sonuçları yeniden atama](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Şekil 9**: bir yabancı anahtar kısıtlaması ihlali kategorileri sonuçları yeniden atama ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


Şimdi, tarayıcı s geri düğmesine basın ve ardından Kılavuzu yenile düğmesine tıklayın. Veri yenileme sırasında tam aynı çıktı Şekil 8'de gösterildiği gibi görmeniz gerekir. Diğer bir deyişle, hatta bazı ürünler rağmen `CategoryID` s için yasal değiştirilmiş değerlere olan ve geri yabancı anahtar kısıtlaması ihlali meydana geldiğinde veritabanındaki güncelleştirilmiş, bunlar alındı.

Şimdi değiştirmek kategorileri (olmadan işlem) düğmesini tıklatarak deneyin. Bu aynı yabancı anahtar kısıtlaması ihlali hatasına neden olur (bkz. Şekil 9), ancak bu sefer bu ürünlerin, `CategoryID` değerleri için yasal bir değiştirildi değeri değil geri alınacak. Tarayıcı s geri düğmesine ve ardından Kılavuzu yenile düğmesine basın. Şekil 10 gösterildiği gibi `CategoryID` s ilk sekiz ürün yeniden. Örneğin, Şekil 8'de Chang sahip bir `CategoryID` 1, ancak Şekil 10 BT s'te 2'ye yeniden.


[![Bazı ürünler CategoryID değerler güncelleştirildi ancak diğerleri olan değil olan](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Şekil 10**: bazı ürünleri `CategoryID` değerleri değildi güncelleştirildi ancak diğerleri olan ([tam boyutlu görüntüyü görmek için tıklatın](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Özet

Varsayılan olarak, TableAdapter s yöntemleri bir işlem kapsamında yürütülen veritabanı deyimleri kaydırma değil, ancak ile biraz iş oluşturacak yöntemleri, işleme ve bir işlem geri alma ekleyebiliriz. Bu öğreticide oluşturduğumuz üç tür yöntemler `ProductsTableAdapter` sınıfı: `BeginTransaction`, `CommitTransaction`, ve `RollbackTransaction`. Bu yöntemleri ile birlikte kullanma gördüğümüz bir `Try...Catch` veri değişikliği deyimleri bir dizi atomik yapmak için blok. Özellikle, oluşturduğumuz `UpdateWithTransaction` yönteminde `ProductsTableAdapter`, sağlanan satırlarını gerekli değişiklikleri gerçekleştirmek için toplu güncelleştirme deseni kullanan `ProductsDataTable`. Ekledik `DeleteProductsWithTransaction` yönteme `ProductsBLL` kabul BLL sınıfta bir `List` , `ProductID` DB doğrudan deseni yöntemini çağırır ve değerleri, giriş olarak `Delete` her `ProductID`. Bir işlem oluşturarak ve ardından veri değişikliği deyimleri içinde yürütülen her iki yöntem de başlangıç bir `Try...Catch` blok. Bir özel durum oluşursa, işlem geri alınır, aksi takdirde taahhüt eder.

5. adım, bir işlem kullanmak ihmal toplu güncelleştirmeler ve işlem Toplu güncelleştirmelerin etkisini gösterilmektedir. Sonraki üç öğreticilerde biz Bu öğreticide düzenlenir foundation sınayabilmesi ve toplu güncelleştirme, silme ve ekler gerçekleştirmek için kullanıcı arabirimleri oluşturun.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Veritabanı Tutarlılığı işlemle](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Saklı yordam SQL Server'da işlemleri yönetme](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Artık daha kolay işlemler: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope ve DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Oracle veritabanı işlemleri,. NET'te kullanma](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Dave Gardner Hilton Giesenow ve Teresa Murphy yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](batch-inserting-cs.md)
> [İleri](batch-updating-vb.md)
