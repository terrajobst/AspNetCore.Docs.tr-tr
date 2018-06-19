---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Ekleme, güncelleştirme ve silme (VB) ile ilişkili olay incelenerek | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, biz önce sırasında ve sonrasında bir ekleme meydana gelen olayları kullanarak inceleyeceğiz güncelleştirmek ya da silme işlemi ASP.NET veri Web denetimi. W....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c6a0ff85567b6e41a62feddc58672f38ad0d75b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889468"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Ekleme, güncelleştirme ve silme (VB) ile ilişkili olaylar inceleniyor
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) veya [PDF indirin](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> Bu öğreticide, biz önce sırasında ve sonrasında bir ekleme meydana gelen olayları kullanarak inceleyeceğiz güncelleştirmek ya da silme işlemi ASP.NET veri Web denetimi. Yalnızca bir alt ürün alanlarının güncelleştirmek için düzenleme arabirimi özelleştirmeyi de göreceğiz.


## <a name="introduction"></a>Giriş

Yerleşik ekleme, düzenleme veya silme GridView, DetailsView ya da FormView denetimlerinin özellikleri kullanırken adımları çeşitli son kullanıcı, yeni bir kayıt ekleme veya güncelleştirme veya varolan bir kayıt silme işlemi tamamlandıktan sonra gerçekleşen. Biz anlatıldığı gibi [önceki öğretici](an-overview-of-inserting-updating-and-deleting-data-vb.md), bir satır Düzenle düğmesini güncelleştir ve İptal düğmeleri ve metin kutuları içine BoundFields etkinleştirin ile değiştirilir GridView düzenlendiğinde. Son kullanıcı verileri güncelleştirir ve güncelleştirme tıkladığında sonra aşağıdaki adımları geri göndermede gerçekleştirilir:

1. GridView, ObjectDataSource's doldurur `UpdateParameters` düzenlenen kaydın benzersiz tanımlayıcı alan ile (aracılığıyla `DataKeyNames` özelliği) yanı sıra kullanıcı tarafından girilen değerler
2. GridView, ObjectDataSource's çağırır `Update()` sırayla nesnesini uygun yöntemi çağırır yöntemi (`ProductsDAL.UpdateProduct`, önceki öğreticimizi içinde)
3. Artık güncel değişiklikleri içerir, temel alınan veri GridView DataSet'e

Bu adımlar dizisi sırasında olay sayısı yangın, bize Özel mantık eklemek için olay işleyicileri oluşturmak etkinleştirme gerektiğinde. Örneğin, adım 1, GridView önce `RowUpdating` olay etkinleşir. Bazı doğrulama hatası varsa bu noktada, güncelleştirme isteği iptal etmemiz olabilir. Zaman `Update()` yöntemi çağrıldığında, ObjectDataSource's `Updating` ekleme veya değerlerin herhangi bir özelleştirme olanağı sağlayarak olay ateşlenir `UpdateParameters`. ObjectDataSource temel sonra nesnenin yöntemi yürütme, ObjectDataSource's tamamlandı `Updated` olayı oluşturulur. Bir olay işleyicisi için `Updated` olay satır sayısını etkilendi ve olsun veya olmasın bir özel durum oluştu gibi güncelleştirme işlemi hakkındaki ayrıntıları inceleyin. Son olarak, adım 2, GridView sonra `RowUpdated` olay ateşlenir; yalnızca güncelleştirme işlemi hakkında ek bilgi gerçekleştirilen incelemek için bu olay can olay işleyicisi.

Şekil 1 GridView güncelleştirirken bu adımları ve olayların dizi gösterilmektedir. Şekil 1 olay desende bir GridView güncelleştirmeye benzersiz değil. Ekleme, güncelleştirme veya GridView verileri silme, DetailsView ya da FormView veri Web denetimi ve ObjectDataSource öncesi ve sonrası düzeyi olayların aynı sırası precipitates.


[![GridView verilerde güncelleştirirken sonrası olayları yangın ve dizi öncesi](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Şekil 1**: in A Series öncesi ve sonrası olayları yangın zaman güncelleştirme verileri GridView ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


Biz yerleşik ekleme genişletmek için bu olayları kullanarak inceleyeceğiz Bu öğreticide, güncelleştirme ve silme ASP.NET veri özelliklerini Web denetler. Yalnızca bir alt ürün alanlarının güncelleştirmek için düzenleme arabirimi özelleştirmeyi de göreceğiz.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>1. adım: bir ürünün güncelleştirme`ProductName`ve`UnitPrice`alanları

Önceki öğretici düzenleme arabirimlerinden içinde *tüm* dahil edilmek üzere olan salt okunur olmayan ürün alanları. GridView - bir alanı kaldırmak için olsaydı söyleyin `QuantityPerUnit` - veri Web denetimi verileri güncelleştirme ObjectDataSource's ayarlamazsınız `QuantityPerUnit` `UpdateParameters` değeri. ObjectDataSource sonra bir değerinde geçip geçmeyeceğini `Nothing` içine `UpdateProduct` düzenlenen veritabanı kaydının değişeceğinden iş mantığı katmanı (BLL) yöntemi `QuantityPerUnit` sütununa bir `NULL` değeri. Benzer şekilde, gerekli bir alan varsa, aşağıdaki gibi `ProductName`, kaldırılır düzenleme arabiriminden güncelleştirme başarısız bir "*'ProductName' sütunu null değerlere izin vermiyor*" özel durum. ObjectDataSource çağırmak üzere yapılandırıldığından bu davranış nedeni olan `ProductsBLL` sınıfının `UpdateProduct` ürün alanların her biri için bir giriş parametresi beklenen yöntemi. Bu nedenle, ObjectDataSource's `UpdateParameters` koleksiyon yöntemi her giriş parametreleri için bir parametre yer.

Veri son kullanıcının yalnızca bir alt alanlarının güncelleştirme olanak sağlayan Web denetimi sağlamak istiyoruz sonra ya da program aracılığıyla eksik ayarlamak ihtiyacımız `UpdateParameters` ObjectDataSource'nın değerleri `Updating` olay işleyicisi veya oluşturun ve BLL yöntemini çağırın yalnızca bir alt bekliyor. Bu ikinci yaklaşımı inceleyelim.

Özellikle, görüntüleyen bir sayfa oluşturalım yalnızca `ProductName` ve `UnitPrice` düzenlenebilir GridView alanları. Bu GridView'ın düzenleme arabirimi yalnızca iki görüntülenen alanları güncelleştirmek kullanıcının sağlayacak `ProductName` ve `UnitPrice`. Bu düzenleme arabirimi yalnızca bir ürün alanlarının alt kümesini sağlar beri ya da varolan BLL's kullanan bir ObjectDataSource oluşturmak ihtiyacımız `UpdateProduct` yöntemi ve eksik ürün alan değerlerini programlı olarak ayarlanmış kendi `Updating` olayı işleyici veya biz yalnızca GridView tanımlanan alanları alt bekliyor yeni bir BLL yöntemi oluşturmanız gerekir. Bu öğretici için Şimdi ikinci seçeneği kullanın ve bir aşırı yüklemesini oluşturma `UpdateProduct` yöntemi, yalnızca üç giriş parametrelerinde geçen bir: `productName`, `unitPrice`, ve `productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Özgün gibi `UpdateProduct` yöntemi, bu aşırı yüklemeyi başlatır bir ürünün belirtilen veritabanında olup olmadığını kontrol ederek `ProductID`. Değilse, döndürürse `False`, ürün bilgilerini güncelleştirmeye yönelik isteği başarısız belirten. Aksi takdirde var olan ürün kaydın güncelleştirmeleri `ProductName` ve `UnitPrice` uygun şekilde alanları ve güncelleştirme TableAdpater's çağırarak tamamlar `Update()` tümleştirilmesidir yöntemi `ProductsRow` örneği.

Bu ek ile bizim `ProductsBLL` sınıfı, biz Basitleştirilmiş GridView arabirim oluşturmak için hazır. Açık `DataModificationEvents.aspx` içinde `EditInsertDelete` klasörü ve GridView sayfasına ekleyin. Yeni ObjectDataSource oluşturmak ve bunu kullanmak için yapılandırmak `ProductsBLL` ile sınıf kendi `Select()` yöntemi eşleme `GetProducts` ve kendi `Update()` yöntemi eşleme `UpdateProduct` yalnızca sürer aşırı `productName`, `unitPrice`, ve `productID` giriş parametreleri. Şekil 2 ObjectDataSource's eşlerken veri kaynağı Oluştur Sihirbazı'nı gösterir `Update()` yönteme `ProductsBLL` sınıfı yeni `UpdateProduct` yöntemi aşırı yüklemesini.


[![Yeni UpdateProduct aşırı yüklemesine map ObjectDataSource'nın Update() yöntemi](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Şekil 2**: ObjectDataSource's eşleme `Update()` yeni yönteme `UpdateProduct` aşırı yükleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


Bizim örneğimizde verileri düzenlemek için ancak eklemek veya kayıtları silme olanağı başlangıçta yalnızca gerekli olduğundan, ObjectDataSource's açıkça belirtmek için bir dakikanızı ayırın `Insert()` ve `Delete()` yöntemleri döndürmemelidir eşlenmesi herhangi birini `ProductsBLL` INSERT ve DELETE sekmelere gidip (hiçbiri) aşağı açılan listeden seçerek sınıfının yöntemleri.


[![(Hiçbiri) INSERT ve DELETE sekmeler için aşağı açılan listeden seçin](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Şekil 3**: (hiçbiri) aşağı açılan listeden Ekle ve Sil sekmeler için seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


Bu sihirbazı tamamladıktan sonra GridView'ın akıllı etiket gelen düzenlemeyi etkinleştir onay kutusunu işaretleyin.

Veri Kaynağı Oluştur Sihirbazı'nı ve GridView bağlaması tamamlanmasından ile Visual Studio hem denetimleri için tanımlayıcı sözdizimi oluşturdu. ObjectDataSource'nın bildirim temelli biçimlendirme, aşağıda gösterilen incelemek için kaynak görünümüne gidin:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

ObjectDataSource'nın için hiçbir eşleme olduğundan `Insert()` ve `Delete()` yöntemleri vardır hiçbir `InsertParameters` veya `DeleteParameters` bölümler. Ayrıca, bu yana `Update()` yöntemi eşleştirilir `UpdateProduct` yalnızca üç giriş parametreleri kabul yöntemi aşırı yüklemesini `UpdateParameters` bölümü var. yalnızca üç `Parameter` örnekleri.

Unutmayın ObjectDataSource's `OldValuesParameterFormatString` özelliği ayarlanmış `original_{0}`. Veri Kaynağı Yapılandırma Sihirbazı'nı kullanırken, bu özellik Visual Studio tarafından otomatik olarak ayarlanır. Ancak, bizim BLL yöntemleri özgün beklemeyen beri `ProductID` geçirilmesi, bu özellik ataması ObjectDataSource'nın bildirim temelli sözdiziminde tamamen kaldırmak için değer.

> [!NOTE]
> Yalnızca silerseniz `OldValuesParameterFormatString` özellik değeri Tasarım görünümünde, özellik Özellikler penceresinden bildirim temelli sözdiziminde var olmaya devam edecek, ancak boş bir dize olarak ayarlayın. Kaldırın veya özelliği tamamen tanımlayıcı sözdizimi veya, Özellikler penceresini değeri varsayılan olarak, Ayarla `{0}`.


ObjectDataSource yalnızca sahipken `UpdateParameters` ürün adı, fiyat ve kimliği için Visual Studio BoundField veya CheckBoxField GridView ürünün alanların her biri için ekledi.


[![GridView BoundField veya CheckBoxField ürünün alanların her biri için içerir](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Şekil 4**: GridView BoundField veya CheckBoxField ürünün alanların her biri için içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


Son kullanıcı bir ürün düzenler ve güncelleştirme düğmesine tıkladığında GridView salt okunur olmayan alanlarla numaralandırır. Ardından ObjectDataSource içinde 's karşılık gelen bir parametre değerini ayarlar `UpdateParameters` kullanıcı tarafından girilen değer koleksiyonu. GridView, karşılık gelen bir parametre değilse, bir koleksiyona ekler. Bizim GridView BoundFields ve tüm ürün alanlarının CheckBoxFields içeriyorsa, bu nedenle, ObjectDataSource çağırma yukarı sona erer `UpdateProduct` tüm olgusuna karşın bu parametre alan aşırı yüklemesini, ObjectDataSource's bildirim temelli biçimlendirme (bkz. Şekil 5) yalnızca üç giriş parametreleri belirtir. Salt okunur olmayan bir bileşimi ise benzer şekilde, ürün giriş parametreleri için karşılık gelmiyor GridView alanları bir `UpdateProduct` aşırı yükleme, bir özel durum güncellerken gerçekleştirilecektir.


[![GridView Will ObjectDataSource'nın UpdateParameters koleksiyonuna parametreleri ekleme](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Şekil 5**: GridView olacak parametreler ekleme ObjectDataSource's `UpdateParameters` koleksiyonu ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


ObjectDataSource çağırır emin olmak için `UpdateProduct` yalnızca ürün adı, fiyat ve kimliği, alan aşırı düzenlenebilir alanlar için zorunda GridView kısıtlamak ihtiyacımız yalnızca `ProductName` ve `UnitPrice`. Bu diğer BoundFields ve CheckBoxFields, bu diğer alanlar ayarlayarak kaldırarak gerçekleştirilebilir `ReadOnly` özelliğine `True`, veya iki şunların bir birleşimi. Bu öğretici için şimdi yalnızca dışındaki tüm GridView alanları Kaldır `ProductName` ve `UnitPrice` geçmesi GridView'ın bildirim temelli biçimlendirme görünür gibi BoundFields:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Olsa bile `UpdateProduct` aşırı bekliyor üç giriş parametreleri, yalnızca iki BoundFields bizim GridView sunuyoruz. Bunun nedeni, `productID` giriş parametresi bir birincil anahtar değer ve değeri geçirilen `DataKeyNames` düzenlenen satır özelliği.

Bizim GridView ile birlikte `UpdateProduct` aşırı yükleme, yalnızca ad ve bir ürünün fiyat herhangi diğer ürün alanlarının kaybetmeden düzenlemesine olanak tanır.


[![Arabirimi yalnızca ürün adı ve fiyat düzenleme olanağı sağlar](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Şekil 6**: yalnızca ürün adı ve fiyat düzenleme arabirimi sağlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> Önceki öğreticide anlatıldığı gibi s görünüm durumu olması GridView (varsayılan davranış) etkin oldukça önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğine `false`, yanlışlıkla silme veya düzenleme kayıtları eşzamanlı kullanıcı sahip riski. Bkz: [Uyarı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews Bu destek düzenleme ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/posts/10054.aspx) daha fazla bilgi için.


## <a name="improving-theunitpriceformatting"></a>Geliştirme`UnitPrice`biçimlendirme

Şekil 6 çalışır gösterilen GridView örnek while `UnitPrice` alan hiç biçimlendirilmemiş, bir para birimi eksik bir fiyat görüntü kaynaklanan simgelerini ve dört ondalık basamak vardır. Bir para birimi düzenlenemeyen satırları biçimlendirmesi uygulamak için yalnızca ayarlamak `UnitPrice` BoundField'ın `DataFormatString` özelliğine `{0:c}` ve kendi `HtmlEncode` özelliğine `False`.


[![UnitPrice'nın DataFormatString ve HtmlEncode özellikleri uygun şekilde ayarlayın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Şekil 7**: ayarlamak `UnitPrice`'s `DataFormatString` ve `HtmlEncode` uygun şekilde özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


Bu değişiklikle düzenlenemeyen satırları fiyat para birimi olarak biçimlendirmek; düzenlenen satır ancak hala para birimi simgesini olmadan ve dört ondalık değeri görüntüler.


[![Düzenlenemez satırları şimdi biçimlendirilmiş para birimi değerleridir](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Şekil 8**: düzenlenemez satırları olan şimdi biçimlendirilmiş para birimi değerleri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


Belirtilen biçimlendirme yönergeleri `DataFormatString` özelliği uygulanabilir düzenleme arabirimine BoundField's ayarlayarak `ApplyFormatInEditMode` özelliğine `True` (varsayılan `False`). Bu özelliği ayarlamak için bir dakikanızı ayırın `True`.


[![UnitPrice BoundField'ın ApplyFormatInEditMode özelliğini True olarak ayarlayın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Şekil 9**: ayarlamak `UnitPrice` BoundField'ın `ApplyFormatInEditMode` özelliğine `True` ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


Bu değişiklik, değeri ile `UnitPrice` düzenlenen görüntülenen satır aynı zamanda bir para birimi olarak biçimlendirilmiş.


[![Düzenlenen sıranın UnitPrice şimdi biçimlendirilmiş bir para birimi olarak değerdir](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Şekil 10**: düzenlenebilir satırın `UnitPrice` değerdir şimdi biçimlendirilmiş bir para birimi olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


$19.00 oluşturur gibi ancak, bir ürün metin kutusuna para birimi simgesini güncelleştirerek bir `FormatException`. GridView çalıştığında ObjectDataSource için kullanıcının kullanıcı tarafından sağlanan değer atamak `UpdateParameters` dönüştüremedi olduğu koleksiyonu `UnitPrice` içine "$19.00" dize `Decimal` parametresi tarafından gerekli (bkz. Şekil 11). Bu sorunu gidermek için bir olay işleyicisi GridView için 's oluşturabiliriz `RowUpdating` olay ve kullanıcı tarafından sağlanan ayrıştırma `UnitPrice` para birimi biçimli olarak `Decimal`.

GridView's `RowUpdating` olay kabul eder, ikinci parametre olarak türünde bir nesne [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), içeren bir `NewValues` sözlük kullanıcı tarafından sağlanan değerler hazır olmasını tutar özelliklerinden biri olarak ObjectDataSource için 's atanan `UpdateParameters` koleksiyonu. Biz varolan üzerine `UnitPrice` değeri `NewValues` koleksiyonu ondalık bir değeri ile Ayrıştırılmış kod aşağıdaki satırları para birimi biçimi kullanarak `RowUpdating` olay işleyicisi:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Kullanıcı sağladıysa bir `UnitPrice` değeri ("$19.00 gibi"), bu değer tarafından hesaplanan ondalık değeri ile üzerine [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), değeri bir para birimi olarak ayrıştırma. Bu ondalık herhangi para birimi simgeleri, virgül, ondalık basamak vb. durumunda doğru ayrıştırır ve kullandığı [NumberStyles numaralandırma](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) içinde [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) ad alanı.

Şekil 11 gösterir kullanıcı tarafından sağlanan para birimi simgelerini nedeni her iki sorun `UnitPrice`, nasıl birlikte GridView's `RowUpdating` olay işleyicisi, bu tür giriş doğru ayrıştırmak için kullanılabilir.


[![Düzenlenen sıranın UnitPrice şimdi biçimlendirilmiş bir para birimi olarak değerdir](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Şekil 11**: düzenlenebilir satırın `UnitPrice` değerdir şimdi biçimlendirilmiş bir para birimi olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>2. adım: yasaklanması`NULL UnitPrices`

İzin vermek için veritabanı yapılandırılırken `NULL` değerler `Products` tablonun `UnitPrice` sütun, biz isteyebilir belirleyerek bu belirli sayfasını ziyaret kullanıcıların önlemek bir `NULL` `UnitPrice` değeri. Diğer bir deyişle, girmek bir kullanıcı başarısız olursa bir `UnitPrice` sonuçları Biz bu sayfadan düzenlenen ürünlerden belirtilen bir fiyat olması gerekir, kullanıcı bildiren bir ileti görüntülemek istediğiniz veritabanına kaydetmek yerine bir ürün satır düzenlerken değeri.

`GridViewUpdateEventArgs` Nesnesi geçirildi GridView ait `RowUpdating` olay işleyiciyi içeriyor bir `Cancel` özelliği, varsa kümesine `True`, güncelleştirme işlemi sonlandırır. Şimdi genişletmek `RowUpdating` ayarlamak için olay işleyicisini `e.Cancel` için `True` ve değilse nedenini açıklayan bir ileti görüntüler `UnitPrice` değeri `NewValues` koleksiyonu değerine sahip `Nothing`.

Bir etiket Web denetimi adlı sayfasına eklemeye başlayın `MustProvideUnitPriceMessage`. Kullanıcı belirtmek başarısız olursa bu etiket denetimi görüntülenen bir `UnitPrice` bir ürün güncelleştirirken değeri. Etiketin ayarlamak `Text` "Ürün için bir fiyat sağlamalısınız." özelliği Ayrıca, yeni bir CSS sınıfı oluşturmuş olduğunuz `Styles.css` adlı `Warning` aşağıdaki tanımıyla:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Son olarak, etiketin ayarlamak `CssClass` özelliğine `Warning`. Bu noktada Tasarımcı uyarı iletisi bir red, kalın, italik, çok büyük yazı tipi boyutunu GridView yukarıda Şekil 12'de gösterildiği gibi göstermelidir.


[![Bir etiket GridView eklendi](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Şekil 12**: bir etiket sahip olan eklenen yukarıda GridView ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


Varsayılan olarak, bu etiket, olacak şekilde ayarlamanız gizleneceğini kendi `Visible` özelliğine `False` içinde `Page_Load` olay işleyicisi:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Kullanıcı bir ürünü belirtmeden güncelleştirmek dener `UnitPrice`, güncelleştirmeyi iptal eder ve uyarı etiketi görüntülemek istiyoruz. GridView's büyütmek `RowUpdating` şekilde olay işleyicisi:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Bir kullanıcı bir fiyat belirtmeden bir ürün kaydetmek çalışırsa, güncelleştirme iptal edilir ve yararlı bir ileti görüntülenir. While veritabanı (ve iş mantığı) izin veren `NULL` `UnitPrice` s, söz konusu ASP.NET sayfa desteklemez.


[![Bir kullanıcı UnitPrice boş bırakamazsınız](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Şekil 13**: olamaz bir kullanıcıyı bırakın `UnitPrice` boş ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


GridView's kullanma kadarki anlatıldığı `RowUpdating` program aracılığıyla ObjectDataSource için 's atanan parametre değerlerini değiştirmek için olay `UpdateParameters` koleksiyonu da iptal etmek için güncelleştirme işlem nasıl tamamen. Bu kavramlar DetailsView ve FormView denetimleri gerçekleştirmek ve ekleme ve silme için de geçerlidir.

Bu görevler için olay işleyicileri aracılığıyla ObjectDataSource düzeyinde de yapılabilir, `Inserting`, `Updating`, ve `Deleting` olaylar. Bu olaylar, temel alınan nesnenin ilişkili yöntemi çağrılmadan önce yangın ve giriş parametreleri koleksiyonunu Değiştir veya depolayabileceği işlemi iptal etmek için son fırsat fırsatı sağlar. Bu üç olayları için olay işleyicileri türünde bir nesneye iletilir [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , ilgilenilen iki özelliklere sahiptir:

- [İptal](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), varsa kümesine `True`, gerçekleştirilmekte olan işlemin iptal eder
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), koleksiyonu olduğu `InsertParameters`, `UpdateParameters`, veya `DeleteParameters`olay işleyicisi için olmasına bağlı olarak `Inserting`, `Updating`, veya `Deleting` olay

ObjectDataSource düzeyinde parametre değerleri ile çalışma göstermek için şimdi sayfamızı içinde yeni bir ürün eklemek kullanıcılara bir DetailsView içerir. Bu DetailsView hızlı bir şekilde veritabanına yeni bir ürün eklemek için bir arabirim sağlamak için kullanılır. Şimdi yeni bir ürün ekleme izin verdiğinizde yalnızca için değerleri girin kullanıcıya tutarlı bir kullanıcı arabirimi tutmak için `ProductName` ve `UnitPrice` alanları. Varsayılan olarak, DetailsView'un ekleme arabiriminde sağlanan olmayan bu değerleri ayarlanacak bir `NULL` veritabanı değeri. Ancak, biz ObjectDataSource's kullanabilirsiniz `Inserting` kısa süre içinde anlatıldığı gibi farklı varsayılan değerlere eklemesine olay.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>3. adım: Yeni ürünler eklemek için bir arabirim sağlayan

GridView, yukarıda tasarımcıya araç kutusundan bir DetailsView sürükleyin Temizle kendi `Height` ve `Width` özellikleri ve ObjectDataSource için zaten var sayfasında bağlarsınız. Bu bir BoundField veya CheckBoxField ürünün alanların her biri için ekler. Biz bu DetailsView yeni ürünler eklemek için kullanmak istediğiniz olduğundan, akıllı etiket etkinleştirmek ekleme seçeneğini denetlemek ihtiyacımız; Ancak, böyle bir seçenek yoktur çünkü ObjectDataSource's `Insert()` yöntemi olmayan bir yöntem eşleştirildiği `ProductsBLL` sınıfı (geri çağırma veri kaynağını yapılandırma gördüğünüzde Şekil 3 Biz bu eşleme (hiçbiri) ayarlayın).

ObjectDataSource yapılandırmak için Sihirbazı başlatılıyor, akıllı etiketten yapılandırma veri kaynağı bağlantısı seçin. İlk ekranda ObjectDataSource bağlı nesnesini değiştirmenize izin verir; ayarlamak için bırakın `ProductsBLL`. Sonraki ekranda temel alınan nesnenin ObjectDataSource'nın yöntemlerden eşlemelere listeler. Biz açıkça, belirtilen olsa bile `Insert()` ve `Delete()` yöntemleri değil eşlenmesi için herhangi bir yöntem, INSERT ve DELETE sekmelere giderseniz bir eşleştirme olduğunu görürsünüz. Bunun nedeni, `ProductsBLL`'s `AddProduct` ve `DeleteProduct` yöntemleri kullanın `DataObjectMethodAttribute` bunlar için varsayılan yöntemleri olduğunu belirtmek için öznitelik `Insert()` ve `Delete()`sırasıyla. Bu nedenle, açıkça belirtilen başka bir değer değilse, sihirbaz bu her çalıştırdığınızda ObjectDataSource Sihirbazı'nı seçer.

Bırakın `Insert()` işaret yöntemi `AddProduct` yöntemi, ancak yeniden DELETE sekmenin aşağı açılan listede (hiçbiri) olarak ayarlayın.


[![INSERT sekmenin aşağı açılan liste set AddProduct yöntemi](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Şekil 14**: Ekle sekmenin aşağı açılan liste kümesine `AddProduct` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![DELETE sekmenin aşağı açılan listede (hiçbiri) olarak ayarlayın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Şekil 15**: Sil sekmenin aşağı açılan listede (hiçbiri) olarak ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


Bu değişiklikleri yaptıktan sonra ObjectDataSource'nın bildirim temelli söz dizimini içerecek şekilde genişletilecek bir `InsertParameters` aşağıda gösterildiği gibi koleksiyon:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Geri eklenen sihirbazını yeniden `OldValuesParameterFormatString` özelliği. Bu özellik varsayılan değerine ayarlayarak temizlemek için bir dakikanızı ayırın (`{0}`) veya değerlerinin tümünü birlikte Tanımlayıcı Sözdizimi kaldırma.

Ekleme özelliklerinin sağlanması ObjectDataSource ile DetailsView'un akıllı etiket şimdi etkinleştirmek ekleme onay kutusu içerir; Tasarımcısına geri dönmek ve bu seçeneği işaretleyin. Ardından, yalnızca iki BoundFields - sahip olmasını DetailsView Karşılaştır `ProductName` ve `UnitPrice` - ve CommandField. Bu noktada DetailsView'un Tanımlayıcı Sözdizimi gibi görünmelidir:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Şekil 16 bir tarayıcı üzerinden bu noktada görüntülendiğinde bu sayfada görüntülenir. Gördüğünüz gibi DetailsView (Chai) ilk ürünün fiyatı ve adını listeler. Ne istiyoruz, ancak kullanıcının hızla yeni bir ürün veritabanına eklemek bir yol sağlayan bir ekleme arabirimdir.


[![Şu anda işlenen salt okunur modda DetailsView olduğu](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Şekil 16**: DetailsView olduğundan şu anda işlenen salt okunur modda ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


İhtiyacımız ayarlamak için kendi ekleme modunda DetailsView gösterebilmeniz `DefaultMode` özelliğine `Inserting`. Bu ilk sitesini ziyaret ettiğinizde ekleme modu DetailsView'da oluşturur ve bunu var. yeni bir kayıt ekledikten sonra tutar. Şekil 17 gösterildiği gibi böyle bir DetailsView yeni bir kayıt eklemek için hızlı bir arabirim sağlar.


[![DetailsView hızla yeni bir ürün eklemek için bir arabirim sağlar.](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Şekil 17**: DetailsView bir arabirim hızla eklemek için yeni bir ürün sağlar ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


Kullanıcı bir ürün adı ve Fiyat (örneğin, "Acme su" ve 1.99 şekil 17 olduğu gibi) girer ve INSERT tıkladığında geri gönderimin ensues ve veritabanına eklenen yeni bir ürün kaydı sonuçlanan ekleme iş akışı gönderir. DetailsView ekleme arabirimiyle ve GridView otomatik olarak DataSet'e kendi veri kaynağı için yeni ürün eklemek için Şekil 18'de gösterildiği gibi tutar.


![Ürün](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Şekil 18**: "Acme su" Ürün veritabanına eklendi


Şekil 18 GridView, DetailsView arabiriminden eksik ürün alanları göstermiyor sırada `CategoryID`, `SupplierID`, `QuantityPerUnit`ve benzeri atanan `NULL` veritabanı değerleri. Bu, aşağıdaki adımları gerçekleştirerek görebilirsiniz:

1. Visual Studio sunucu Gezgini'nde Git
2. Genişletme `NORTHWND.MDF` veritabanı düğümü
3. Sağ `Products` veritabanı Tablo düğümü
4. Tablo verileri göster seçin

Bu kayıtların tümünü listeleyecek `Products` tablo. Şekil 19 gösterildiği gibi tüm yeni bizim ürünün sütunlarının dışında `ProductID`, `ProductName`, ve `UnitPrice` sahip `NULL` değerleri.


[![Ürün alanları sağlanmamış DetailsView'da olan atanan NULL değerler](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Şekil 19**: ürün alanları sağlanmamış DetailsView'da atanır `NULL` değerleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


Biz dışında varsayılan bir değer sağlamak isteyebilirsiniz `NULL` biri veya birkaçı için sütun değerleri, ya da çünkü `NULL` en iyi varsayılan seçeneği değil veya veritabanı sütununa izin vermediği için `NULL` s. Bunu gerçekleştirmek için şu program aracılığıyla DetailsView'un parametrelerinin değerlerini ayarlayabilirsiniz `InputParameters` koleksiyonu. Bu atama ya da olay işleyicisi DetailsView'un için yapılabilir `ItemInserting` olay veya ObjectDataSource'nın `Inserting` olay. Biz zaten inceledik beri düzeyini denetleyen veri Web öncesi ve sonrası düzeyi olaylarını kullanarak, bu kez ObjectDataSource'nın olayları kullanarak inceleyelim.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>4. adım: İçin değerler atama`CategoryID`ve`SupplierID`parametreleri

Bu öğretici için şimdi bu arabirimi aracılığıyla yeni bir ürün eklerken uygulamamız için bunu atanması gerektiğini düşünün bir `CategoryID` ve `SupplierID` 1 değeri. Daha önce belirtildiği gibi ObjectDataSource veri değişikliği işlemi sırasında bu yangın öncesi ve sonrası düzeyi olayları çifti vardır. Zaman kendi `Insert()` yöntemi çağrıldığında, ObjectDataSource ilk başlatır, `Inserting` olay, ardından yöntemini çağırır, kendi `Insert()` yöntemi için eşlenen ve son olarak başlatır `Inserted` olay. `Inserting` Olay işleyicisi ortaya koymaktadır giriş parametreleri ince ayar veya depolayabileceği işlemi iptal etmek için bir son fırsat bize.

> [!NOTE]
> Sonra büyük olasılıkla isteyeceğiniz ya da let gerçek uygulama kullanıcı, kategori ve tedarikçi belirtin veya bu değer için bunları bazı ölçütlere göre seçmesi veya iş mantığı (doğrudan bir kimliği 1 seçmek yerine). Ne olursa olsun, örnek programlı olarak bir giriş parametresi değeri ObjectDataSource'nin önceden düzeyi olayından nasıl ayarlanacağı gösterilmiştir.


ObjectDataSource için 's olay işleyicisi oluşturmak için bir dakikanızı ayırın `Inserting` olay. Türünde bir nesne olay işleyicinin ikinci giriş parametresi fark `ObjectDataSourceMethodEventArgs`, parametre koleksiyonuna erişmek için bir özelliği vardır (`InputParameters`) ve işlemi iptal etmek için bir özellik (`Cancel`).


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

Bu noktada, `InputParameters` özelliği içeren ObjectDataSource's `InsertParameters` DetailsView atanan değerler koleksiyonu. Bu parametrelerden birinin değeri değiştirmek için yalnızca kullanın: `e.InputParameters("paramName") = value`. Bu nedenle, ayarlamak için `CategoryID` ve `SupplierID` 1 değerine ayarlamak `Inserting` olay işleyicisi şu şekilde görünür:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Bu zaman (örneğin, Acme Soda), yeni bir ürün eklerken `CategoryID` ve `SupplierID` yeni ürün sütunlarının 1 olarak ayarlayın (bkz. Şekil 20).


[![Yeni ürünleri artık yüklü kendi adlı kullanıcı, Categoryıd'si ve SupplierID değerleri kümesi 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Şekil 20**: yeni ürünler artık sahip Their `CategoryID` ve `SupplierID` değerlerini Ayarla 1 ([tam boyutlu görüntüyü görüntülemek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>Özet

Düzenleme, ekleme ve silme işlemi sırasında veri Web denetimi ve ObjectDataSource öncesi ve sonrası düzeyi olay sayısı ile devam edin. Bu öğreticide önceden düzeyi olayları incelenmesi ve bunlar giriş parametreleri özelleştirmek veya veri değişikliği işlemi iptal etmek için tamamen hem de veri Web denetimi ve ObjectDataSource'nın olayları nasıl kullanılacağını gördünüz. Sonraki öğreticide oluşturma ve sonrası düzeyi olayları için olay işleyicilerini kullanarak inceleyeceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Jackie Goor ve Liz Shulok yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [sonraki](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
