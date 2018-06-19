---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Veri erişim katmanın bağlantı ve komut düzeyi ayarlarını (VB) yapılandırma | Microsoft Docs
author: rick-anderson
description: TableAdapters yazılmış bir veri kümesi içinde otomatik olarak veritabanına bağlanırken, komutları veren ve sonuçları bir DataTable doldurma dikkatli olun...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: b73c6113e84e290025e5835781fa2f85587289b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877183"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Veri erişim katmanın bağlantı ve komut düzeyi ayarlarını (VB) yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) veya [PDF indirin](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapters yazılmış bir veri kümesi içinde otomatik olarak veritabanına bağlanırken, komutları veren ve sonuçları bir DataTable doldurma dikkatli olun. Ancak, bu ayrıntıları kendisini ve Bu öğreticide halletmeniz istediğinizde biz TableAdapter veritabanı bağlantısı ve komut düzeyini ayarlarında erişim öğrenin durumlar vardır.


## <a name="introduction"></a>Giriş

Eğitmen seri biz yazılan veri kümeleri katmanlı mimarimizin iş nesneleri ve veri erişim katmanı uygulamak için kullanılan. ' Da anlatıldığı gibi [ilk öğreticide](../introduction/creating-a-data-access-layer-vb.md), almak ve temel alınan veri değiştirmek için veritabanı ile iletişim kurmak için sarmalayıcıları TableAdapters görür ancak DataTables hizmet veri depoları olarak yazılan veri kümesi s. TableAdapters veritabanı ile çalışan söz konusu karmaşıklık şifreleyebilir ve bize veritabanına bağlanmak, bir komutu veya bir DataTable sonuçları doldurmak için kod yazma gereğini ortadan kaldırır.

Ne zaman TableAdapter derinliklerine burrow ve doğrudan ADO.NET nesneleriyle çalışan kod yazmak için ihtiyacımız zamanlar vardır. İçinde [kaydırma veritabanı değişiklikleri bir işlem içinde](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) öğretici, örneğin, eklediğimiz yöntemleri başına, yürütme ve ADO.NET işlemleri geri TableAdapter için. Bu yöntemler bir iç, kullanılan el ile oluşturulan `SqlTransaction` TableAdapter s atandı nesne `SqlCommand` nesneleri.

Bu öğreticide TableAdapter veritabanı bağlantısı ve komut düzeyini ayarlarında erişmek nasıl inceleyeceğiz. Özellikle, işlevsellik ekleyeceğiz `ProductsTableAdapter` etkinleştirir erişmek için temel alınan bağlantı dizesini ve zaman aşımı ayarları komutu.

## <a name="working-with-data-using-adonet"></a>ADO.NET kullanarak verileri ile çalışma

Microsoft .NET Framework sınıfları özellikle verilerle çalışmak için tasarlanmış sayısız içerir. İçinde bulunan bu sınıfların [ `System.Data` ad alanı](https://msdn.microsoft.com/library/system.data.aspx), denir *ADO.NET* sınıfları. ADO.NET şemsiyesi altındaki sınıfların bazıları için belirli bir bağlı *veri sağlayıcısı*. Bir veri sağlayıcısı akışı ADO.NET sınıfları ve temel alınan veri deposu bilgi sağlayan bir iletişim kanalı olarak düşünebilirsiniz. OleDb ve ODBC yanı sıra, belirli veritabanı sistemi için özel olarak tasarlanmıştır sağlayıcıları gibi genelleştirilmiş sağlayıcıları vardır. Örneğin, OleDb sağlayıcısı kullanarak bir Microsoft SQL Server veritabanına bağlanmak mümkün olsa da, tasarlandığı ve özellikle SQL Server için en iyi duruma getirilmiş olarak SqlClient sağlayıcısı çok daha verimli olur.

Program aracılığıyla verilere erişirken aşağıdaki düzeni yaygın olarak kullanılır:

1. Veritabanıyla bağlantı kurun.
2. Bir komutu yürütün.
3. İçin `SELECT` iş sorguları, sonuçta elde edilen kayıtlarıyla.

Bu adımların her biri gerçekleştirmek için ayrı ADO.NET sınıflarını vardır. Örneğin, SqlClient sağlayıcısı kullanarak bir veritabanına bağlanmak için kullanmak [ `SqlConnection` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Sorun için bir `INSERT`, `UPDATE`, `DELETE`, veya `SELECT` komutunu kullanın veritabanına [ `SqlCommand` sınıfı](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Dışında [kaydırma veritabanı değişiklikleri bir işlem içinde](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) öğretici, biz olmayan gerekiyordu herhangi bir alt düzey ADO.NET kod, kendisini, TableAdapters otomatik olarak üretilen kod için gereken işlevi içerdiğinden yazma veritabanına bağlanmak, komutları verin, verileri alabilir ve bu verileri DataTables doldurmak. Ancak, ne zaman bu alt düzey ayarlarını özelleştirmek için ihtiyacımız zamanlar olabilir. Sonraki birkaç adımda TableAdapters tarafından dahili olarak kullanılan ADO.NET nesneleri içine dokunun nasıl inceleyeceğiz.

## <a name="step-1-examining-with-the-connection-property"></a>1. adım: ile bağlantı özelliği inceleniyor

Her TableAdapter sınıfına sahip bir `Connection` veritabanı bağlantısı bilgilerini belirten özellik. Bu özellik s veri türü ve `ConnectionString` değeri TableAdapter Yapılandırma Sihirbazı'nda yaptığınız seçimlere göre belirlenir. Biz bir TableAdapter yazılmış bir veri kümesi eklediğinizde, bu sihirbaz bize veritabanı için isteyeceğini geri çağırma (bkz: Şekil 1) kaynağı. Bu ilk adımı aşağı açılan listesinde, herhangi bir sunucu Gezgini s veri bağlantıları veritabanlarında yanı sıra yapılandırma dosyasında belirtilen bu veritabanlarını içerir. Biz kullanmak istediğiniz veritabanı aşağı açılan listede yoksa yeni bağlantı düğmesini tıklatarak ve gerekli bağlantı bilgileri sağlayan yeni bir veritabanı bağlantısı belirtilebilir.


[![TableAdapter Yapılandırma Sihirbazı'nı ilk adımı](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Şekil 1**: TableAdapter Yapılandırma Sihirbazı'nın ilk adım ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Let s ele TableAdapter s kodunu incelemek için bir dakikanızı `Connection` özelliği. İçinde belirtildiği gibi [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğretici, biz görüntüleyebilir otomatik olarak oluşturulan TableAdapter kod sınıfı görünüm penceresine gidip aşağıya doğru uygun sınıf ayrıntılara ve üye adı çift.

Sınıf Görünümü penceresine Görünüm menüsüne giderek ve sınıf görünümü seçerek (veya Ctrl + Shift + C yazarak) gidin. Sınıf Görünümü pencerenin üst yarısı detayına gitmek `NorthwindTableAdapters` ad alanı seçip `ProductsTableAdapter` sınıfı. Bu görüntüler `ProductsTableAdapter` s üyeleri alt yarısında sınıfı Şekil 2'de gösterildiği gibi görünümün. Çift `Connection` kendi kodu görmek için özellik.


![Bağlantı özelliği, otomatik olarak oluşturulan kodu görüntülemek üzere Sınıf Görünümü'nde çift tıklatın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Şekil 2**: bağlantı özelliği, otomatik olarak oluşturulan kodu görüntülemek üzere Sınıf Görünümü'nde çift tıklatın


TableAdapter s `Connection` özelliği ve diğer bağlantısıyla ilgili kod aşağıdadır:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Ne zaman TableAdapter sınıfının örneği, üye değişkeni `_connection` eşittir `Nothing`. Zaman `Connection` özelliği erişildiğinde, onu önce bakar `_connection` üye değişkeni örneği. Değilse, varsa `InitConnection` yöntemi çağrıldığında, hangi başlatır `_connection` ve ayarlar kendi `ConnectionString` TableAdapter Yapılandırma Sihirbazı'nı s Birinci adımdaki belirtilen bağlantı dizesi değerine özelliği.

`Connection` Özelliği de atanabilen bir `SqlConnection` nesnesi. Bunun yapılması yeni ilişkilendirir `SqlConnection` her TableAdapter s nesnesiyle `SqlCommand` nesneleri.

## <a name="step-2-exposing-connection-level-settings"></a>2. adım: Bağlantı düzeyi ayarlarını gösterme

Bağlantı bilgilerini TableAdapter içinde şifrelenmiş olarak kalır ve diğer katmanlarda uygulama mimarisi için erişilebilir olmaması gerekir. Ancak, TableAdapter s bağlantı düzeyi bilgileri erişilebilir veya bir sorgu, kullanıcı veya ASP.NET sayfası özelleştirilebilir olması gerektiğinde senaryolar olabilir.

Genişletme s izin `ProductsTableAdapter` içinde `Northwind` dahil etmek için veri kümesi bir `ConnectionString` iş mantığı katmanı tarafından okuma veya TableAdapter tarafından kullanılan bağlantı dizesini değiştirmek için kullanılan özellik.

> [!NOTE]
> A *bağlantı dizesi* kullanmak için veritabanı, kimlik doğrulama kimlik bilgileri ve diğer veritabanıyla ilgili ayarları konumunu sağlayıcısı gibi veritabanı bağlantı bilgilerini belirleyen bir dizedir. Çeşitli veri depoları ve sağlayıcıları tarafından kullanılan bağlantı dizesi desenleri listesi için bkz: [ConnectionStrings.com](http://www.connectionstrings.com/).


' Da anlatıldığı gibi [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğretici, yazılan veri kümesi s otomatik olarak oluşturulan sınıflar uzatabilirsiniz kullanımı ile kısmi sınıflar. İlk olarak, yeni bir alt adlı projeye oluşturun `ConnectionAndCommandSettings` altındaki `~/App_Code/DAL` klasörü.


![ConnectionAndCommandSettings adlı bir alt klasör Ekle](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Şekil 3**: adlı bir alt klasör Ekle `ConnectionAndCommandSettings`


Adlı yeni bir sınıf dosyası ekleyin `ProductsTableAdapter.ConnectionAndCommandSettings.vb` ve aşağıdaki kodu girin:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Bu kısmi sınıfı ekler bir `Public` adlı özellik `ConnectionString` için `ProductsTableAdapter` okumak veya arka plandaki bağlantı TableAdapter s için bağlantı dizesini güncelleştirmek herhangi bir katman sağlayan sınıfı.

Bu parçalı sınıf ile oluşturulan (ve kaydedilen) açmak `ProductsBLL` sınıfı. Varolan yöntemleri birine gidin ve yazın `Adapter` ve ardından IntelliSense getirmek için dönem tuşuna basın. Yeni görmelisiniz `ConnectionString` özelliği okuma program aracılığıyla veya bu değerden BLL ayarlamak anlamına IntelliSense de kullanılabilir.

## <a name="exposing-the-entire-connection-object"></a>Tüm bağlantı nesnesi gösterme

Bu kısmi sınıfı temel alınan bağlantı nesnenin yalnızca bir özellik sunar: `ConnectionString`. Tüm bağlantı nesnesi TableAdapter sınırları ötesinde kullanılabilir hale getirmek istiyorsanız, bunun yerine değiştirebileceğiniz `Connection` özellik s koruma düzeyi. TableAdapter s gösterdi biz incelenmesi adım 1'de otomatik olarak oluşturulan kodu `Connection` özelliği olarak işaretli `Friend`, onu yalnızca aynı bütünleştirilmiş kodda sınıflar tarafından erişilebildiğinden anlamına gelir. Bu, ancak TableAdapter s değiştirilebilir `ConnectionModifier` özelliği.

Açık `Northwind` veri kümesi, tıklatıldığında `ProductsTableAdatper` Tasarımcısı'nda ve Özellikler penceresine gidin. Yazısını görürsünüz `ConnectionModifier` ve varsayılan değerini `Assembly`. Yapmak için `Connection` yazılan veri kümesi s derleme dışında değişiklik kullanılabilir özellik `ConnectionModifier` özelliğine `Public`.


[![Bağlantı özelliği s erişilebilirlik düzeyi ConnectionModifier özelliği aracılığıyla yapılandırılabilir](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Şekil 4**: `Connection` özelliği s erişilebilirlik düzeyi yapılandırılabilir aracılığıyla `ConnectionModifier` özelliği ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Veri kümesi kaydedin ve sonra geri dönüp `ProductsBLL` sınıfı. Daha önce varolan yöntemleri birine gidin ve yazın `Adapter` ve ardından IntelliSense getirmek için dönem tuşuna basın. Listenin içermelidir bir `Connection` özelliği, artık programlı olarak okuyabilir veya herhangi bir bağlantı düzeyi ayarı BLL atamak olduğunu anlamına gelir.

## <a name="step-3-examining-the-command-related-properties"></a>3. adım: komutu ile ilgili özellikler inceleniyor

Varsayılan olarak, otomatik olarak oluşturulan bir ana sorgu bir TableAdapter oluşur `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Bu ana sorgu s `INSERT`, `UPDATE`, ve `DELETE` deyimleri bir ADO.NET veri bağdaştırıcısı nesnesi olarak TableAdapter s kodda uygulanır `Adapter` özelliği. İle gibi kendi `Connection` özelliği `Adapter` özellik s veri türü, kullanılan veri sağlayıcısı tarafından belirlenir. Bu öğreticiler SqlClient sağlayıcısı kullandığından `Adapter` özelliği türüdür [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` özelliğinin türü üç özelliklerini `SqlCommand` için kullandığı sorunu `INSERT`, `UPDATE`, ve `DELETE` deyimleri:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` nesne belirli bir sorgu veritabanına göndermekten sorumludur ve gibi özelliklere sahiptir: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), geçici SQL deyimi veya saklı yordamı yürütmek için; içerir ve [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), koleksiyonu olduğu `SqlParameter` nesneleri. Geri gördüğümüz gibi [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğretici, bu komut nesneleri Özellikleri penceresinden özelleştirilebilir.

Kendi ana sorgu yanı sıra TableAdapter değişken çeşitli yöntemler içerebilir, çağrıldığında, veritabanı için belirtilen bir komut gönderme. Ana sorgu s komut nesne ve tüm ek yöntemleri için komut nesneleri TableAdapter s içinde depolanan `CommandCollection` özelliği.

Let s ele tarafından oluşturulan kodu bakmak için bir dakikanızı `ProductsTableAdapter` içinde `Northwind` veri kümesi bu iki özellik ve destekleyici üye değişkenleri ve yardımcı yöntemler için:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kodunu `Adapter` ve `CommandCollection` özellikleri yakından taklit eder, `Connection` özelliği. Özellikleri tarafından kullanılan nesneleri tutun üye değişkeni yok. Özellikler `Get` erişimciler Başlat karşılık gelen üye değişkeni olup olmadığını kontrol ederek `Nothing`. Bu durumda, başlatma yöntemini üye değişkeni örneği oluşturur ve çekirdek komutu ilgili özellikler atar adı verilir.

## <a name="step-4-exposing-command-level-settings"></a>4. adım: Komutu düzeyi ayarlarını gösterme

İdeal olarak, komut düzeyi bilgileri içinde veri erişim katmanı kapsüllenmiş kalmalıdır. Bu bilgiler mimarisinin diğer katmanlarda gerekli, ancak bu bir parçalı sınıf gibi bağlantı düzeyi ayarlarla gösterilebilir.

TableAdapter yalnızca tek bir sahip olduğundan `Connection` özelliği, bağlantı düzeyi ayarlarını gösterme kodunu oldukça basit. TableAdapter birden çok komut nesneleri - olabileceğinden komutu düzeyi ayarlarını değiştirirken şeyler biraz daha karmaşık bir `InsertCommand`, `UpdateCommand`, ve `DeleteCommand`, komut nesneleri değişken sayıda birlikte `CommandCollection` özellik. Komut düzeyi ayarlarını güncelleştirirken, bu ayarlar tüm komut nesneleri dağıtılmasını gerekir.

Örneğin, yürütmek için bir olağanüstü uzun sürdü TableAdapter içinde bazı sorgular olduğunu düşünün. TableAdapter bu sorguların birini yürütmek için kullanırken, biz s komut nesnesi artırmak isteyebilirsiniz [ `CommandTimeout` özelliği](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Bu özellik, komutun yürütülmesi için beklenecek saniye sayısını belirtir ve varsayılan olarak 30.

İzin vermek için `CommandTimeout` BLL tarafından ayarlanacak özellik ekleme aşağıdaki `Public` yönteme `ProductsDataTable` kısmi sınıf dosyası kullanılarak oluşturulan 2. adımda (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Bu yöntem, BLL veya sunu katmanı tüm komutları sorunlarını komut zaman aşımı ayarlamak için bu TableAdapter örneği tarafından çağırılabilir.

> [!NOTE]
> `Adapter` Ve `CommandCollection` özellikleri olarak işaretlenmiş `Private`, bunlar yalnızca erişilebilir TableAdapter içinde kodundan anlamına gelir. Farklı `Connection` özelliği, bu erişim değiştiricileri yapılandırılabilir değildir. Bu nedenle, diğer katmanlara mimarisinde komutu düzeyi özelliklerini ortaya gerekiyorsa sağlamak için yukarıda açıklanan parçalı sınıf yaklaşım kullanmanız gerekir bir `Public` yöntemi veya okuyan veya yazan özelliği `Private` nesneleri komutu.


## <a name="summary"></a>Özet

Veri erişim ayrıntılarını ve karmaşıklık kapsülleyen TableAdapters yazılmış bir veri kümesi içinde hizmet. TableAdapters kullanarak, biz veritabanına bağlanmak, bir komutu veya bir DataTable sonuçları doldurmak için ADO.NET kod yazma hakkında endişelenmeniz gerekmez. Bunu tüm otomatik olarak bize için gerçekleştirilir.

Ancak, ne zaman bağlantı dizesi veya varsayılan bağlantı ya da komut zaman aşımı değerlerini değiştirme gibi alt düzey ADO.NET özelliklerini özelleştirmek için ihtiyacımız zamanlar olabilir. TableAdapter otomatik olarak oluşturulan `Connection`, `Adapter`, ve `CommandCollection` özellikleri, ancak bunlar olan ya da `Friend` veya `Private`, varsayılan olarak. Bu iç bilgileri içerecek şekilde kısmi sınıflar kullanarak TableAdapter genişleterek verilebilen `Public` yöntemleri veya özellikleri. Alternatif olarak, TableAdapter s `Connection` özellik erişim değiştiricisi TableAdapter s yapılandırılabilir `ConnectionModifier` özelliği.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Burnadette Leigh, S ren Jacob Lauritsen Teresa Murphy ve Hilton Geisenow yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](working-with-computed-columns-vb.md)
> [sonraki](protecting-connection-strings-and-other-configuration-information-vb.md)
