---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Ekleme, güncelleştirme ve silme (C#) ile ilişkili olayları İnceleme | Microsoft Docs
author: rick-anderson
description: Önce sırasında ve sonrasında bir ekleme meydana gelen olayları kullanarak inceleyeceğiz Bu öğreticide, güncelleştirme veya silme işlemi bir ASP.NET veri Web denetimi. W....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: cec25809038cbe6a114b0e36ef7265d84bbe9b92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363306"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Ekleme, güncelleştirme ve silme (C#) ile ilişkili olayları İnceleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) veya [PDF olarak indirin](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> Önce sırasında ve sonrasında bir ekleme meydana gelen olayları kullanarak inceleyeceğiz Bu öğreticide, güncelleştirme veya silme işlemi bir ASP.NET veri Web denetimi. Yalnızca ürün alanlarının alt kümesini güncelleştirmek için düzenleme arabirimini özelleştirme de göreceğiz.


## <a name="introduction"></a>Giriş

Yerleşik ekleme, düzenleme veya silme GridView, DetailsView veya FormView denetimlerin özelliklerini kullanırken, çeşitli adımları son kullanıcının yeni kayıt ekleme, güncelleştirme veya varolan bir kaydı silme işlemi tamamlandığında niteler. Açıkladığımız gibi [önceki öğreticide](an-overview-of-inserting-updating-and-deleting-data-cs.md), Düzenle düğmesini, güncelleştirme ve İptal düğmeleri ve metin kutuları BoundFields dönüştürün değiştirilir GridView satır düzenlendi. Son kullanıcı verileri güncelleştirir ve güncelleştirme tıkladığında sonra aşağıdaki adımları geri göndermede gerçekleştirilir:

1. GridView kendi ObjectDataSource doldurur `UpdateParameters` düzenlenen kaydın benzersiz tanımlayıcı alanları ile (aracılığıyla `DataKeyNames` özelliği) kullanıcı tarafından girilen değerler ile birlikte
2. GridView kendi ObjectDataSource çağırır `Update()` sırayla uygun arka plandaki nesneye yöntemi çağıran, yöntemi (`ProductsDAL.UpdateProduct`, önceki müşterilerimize öğreticide)
3. Artık güncel değişiklikleri içerir, temel alınan verileri GridView'a DataSet'e bağlanır

Bu adımlar dizisi sırasında olay sayısı yangın, bize Özel mantık eklemek için olay işleyicilerini oluşturma etkinleştirilmesi gerektiğinde. Örneğin, 1. adım, GridView'ın önce `RowUpdating` olay harekete geçirilir. Bazı doğrulama hatası varsa Biz bu noktada, güncelleştirme isteğini iptal edebilirsiniz. Zaman `Update()` yöntemi çağrılır, ObjectDataSource `Updating` olayı tetiklendiğinde, ekleme veya değerlerin herhangi birinin özelleştirme olanağı sağlayarak `UpdateParameters`. ObjectDataSource temel sonra nesnenin yöntemi ObjectDataSource yürütme tamamlandı `Updated` olayı oluşturulur. Bir olay işleyicisi için `Updated` olay kaç satır etkilendi ve olup olmadığı bir özel durum oluştu gibi güncelleştirme işlemiyle ilgili ayrıntıları inceleyin. Son olarak, 2. adım, GridView'ın sonra `RowUpdated` olay harekete geçirilir; yalnızca güncelleştirme işlemi hakkında ek bilgi gerçekleştirilen incelemek, bu olay can için bir olay işleyicisi.

Şekil 1 GridView güncelleştirirken bu dizi olayları ve adımları gösterilmektedir. Şekil 1 olay deseni ile GridView güncelleştirmeye benzersiz değil. Ekleme, güncelleştirme veya GridView verileri silme, veri Web denetimi hem ObjectDataSource öncesi ve sonrası düzeyi olayların aynı sırasını DetailsView veya FormView precipitates.


[![Bir dizi öncesi ve GridView verileri güncelleştirirken sonrası olayları tetikleme](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Şekil 1**:, A serisi öncesi ve sonrası olayları yangın olduğunda güncelleştirme verileri GridView ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))


Yerleşik eklemeden genişletmek için bu olayları kullanarak inceleyeceğiz Bu öğreticide, güncelleştirme ve silme özelliklerini ASP.NET veri Web denetler. Yalnızca ürün alanlarının alt kümesini güncelleştirmek için düzenleme arabirimini özelleştirme de göreceğiz.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>1. adım: bir ürünün güncelleştirme`ProductName`ve`UnitPrice`alanları

Önceki öğreticiden düzenleme arabirimlerde *tüm* dahil edilmesi gerekti salt okunur olmayan ürün alanları. GridView - bir alanı kaldırmak için olsaydık söyleyin `QuantityPerUnit` - veri Web denetimi verileri güncelleştirme ObjectDataSource ayarlamazsınız `QuantityPerUnit` `UpdateParameters` değeri. ObjectDataSource ardından geçecekse bir `null` içine değer `UpdateProduct` düzenlenen veritabanı kaydın değiştirirsiniz iş mantığı katmanı (BLL) yöntemini `QuantityPerUnit` sütununa bir `NULL` değeri. Benzer şekilde, gerekli bir alan, gibi `ProductName`, kaldırılır düzenleme arabiriminden güncelleştirme ile başarısız olur bir "*'ProductName' sütunu null değerlere izin vermiyor*" özel durum. ObjectDataSource çağırmak üzere yapılandırıldığından, bu davranışın nedeni olan `ProductsBLL` sınıfın `UpdateProduct` ürün alanların her biri için beklenen bir giriş parametresi yöntemi. Bu nedenle, ObjectDataSource `UpdateParameters` koleksiyon her yöntem bir giriş parametreleri için bir parametre yer.

Son kullanıcı, alanların bir alt kümesini yalnızca güncelleştirilecek sağlayan Web denetimi veri sağlamak istiyoruz sonra ya da eksik programlanarak ihtiyacımız `UpdateParameters` ObjectDataSource değerleri `Updating` olay işleyicisi oluşturun ve BLL yöntemi arayın yalnızca bir alt bekliyor. Bu ikinci yaklaşımda araştıralım.

Özellikle, görüntüleyen bir sayfa oluşturalım yalnızca `ProductName` ve `UnitPrice` alanlar içinde düzenlenebilir bir GridView. Bu GridView'ın düzenleme arabirimi yalnızca iki görüntülenen alanları güncelleştirmek kullanıcının sağlayacak `ProductName` ve `UnitPrice`. Bu düzenleme arabirimi yalnızca ürünün alanların bir alt kümesini sağlar ya da mevcut BLL's kullanan bir ObjectDataSource oluşturmak gerekir `UpdateProduct` yöntemi ve eksik ürün alan değerlerini programlı olarak ayarlanmış kendi `Updating` olay işleyicisi ya da biz yalnızca alt GridView içinde tanımlanan alanların bekliyor yeni bir BLL yöntemi oluşturmanız gerekir. Bu öğretici için Şimdi ikinci seçeneği kullanın ve bir aşırı yüklemesini oluşturma `UpdateProduct` yöntemi, yalnızca üç giriş parametreleri alan bir: `productName`, `unitPrice`, ve `productID`:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Gibi özgün `UpdateProduct` yöntemi, bu aşırı yüklemesini başlatır bir ürün veritabanında belirtilen sahip olup olmadığını kontrol ederek `ProductID`. Değilse, bunu döndürürse `false`, ürün bilgilerini güncelleştirme isteği başarısız olduğunu gösteren. Aksi takdirde, mevcut ürün kaydın güncelleştirmesi `ProductName` ve `UnitPrice` uygun şekilde alanları ve güncelleştirme TableAdpater'ın çağırarak işlemeler `Update()` tümleştirilmesidir yöntemi `ProductsRow` örneği.

İle bu ek olarak sunduğumuz `ProductsBLL` sınıfı, biz Basitleştirilmiş GridView arabirimi oluşturmak hazır. Açık `DataModificationEvents.aspx` içinde `EditInsertDelete` klasörü ve GridView sayfaya ekleyin. Yeni bir ObjectDataSource oluşturun ve bunu kullanacak şekilde yapılandırmanız `ProductsBLL` sınıfıyla birlikte kendi `Select()` yöntemi eşleme `GetProducts` ve kendi `Update()` yöntemi eşleme `UpdateProduct` yalnızca alan aşırı yüklemesini `productName`, `unitPrice`, ve `productID` giriş parametreleri. Şekil 2 ObjectDataSource eşlerken veri kaynağı Oluştur Sihirbazı'nı gösterir `Update()` yönteme `ProductsBLL` sınıf yeni `UpdateProduct` yöntemi aşırı yüklemesi.


[![ObjectDataSource Update() yöntemi için yeni UpdateProduct aşırı eşleme](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Şekil 2**: ObjectDataSource harita `Update()` yeni yönteme `UpdateProduct` aşırı yükleme ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))


Bizim örneğimizde başlangıçta yalnızca verileri düzenlemek için ancak eklemek veya kayıtları silme olanağı gerektirdiğinden ObjectDataSource açıkça belirtmek için bir dakikanızı ayırın `Insert()` ve `Delete()` yöntemleri olmamalıdır eşlenebilir herhangi birini `ProductsBLL` INSERT ve DELETE sekmeleri gidip (hiçbiri) aşağı açılan listeden seçerek sınıfın yöntemleri.


[![(Hiçbiri) INSERT ve DELETE sekmeler için aşağı açılan listeden seçin](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Şekil 3**: (hiçbiri) aşağı açılan listeden ekleme ve silme sekmeleri seçin ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))


Bu sihirbazı tamamladıktan sonra GridView'ın akıllı etiketinde düzenlemeyi etkinleştir onay kutusunu işaretleyin.

Veri Kaynağı Oluştur Sihirbazı'nı ve GridView'a bağlama tamamlanmasından ile Visual Studio, her iki denetim için bildirim temelli söz dizimi oluşturdu. ObjectDataSource bildirim temelli biçimlendirme ve aşağıda da gösterilen incelemek için kaynak görünümünü gidin:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

ObjectDataSource için eşleşme yok olduğundan `Insert()` ve `Delete()` yöntemleri vardır hiçbir `InsertParameters` veya `DeleteParameters` bölümler. Ayrıca, bu yana `Update()` yöntemi eşlenmiş durumda `UpdateProduct` yalnızca üç giriş parametrelerini kabul eden bir yöntemi aşırı yüklemesini `UpdateParameters` bölümü var. yalnızca üç `Parameter` örnekleri.

Unutmayın ObjectDataSource `OldValuesParameterFormatString` özelliği `original_{0}`. Veri Kaynağı Yapılandırma Sihirbazı'nı kullanırken, bu özellik Visual Studio tarafından otomatik olarak ayarlanır. Ancak, bizim BLL yöntemleri orijinal beklemiyoruz beri `ProductID` geçirilmesi, bu özellik ataması ObjectDataSource bildirim temelli söz dizimi tamamen kaldırmak için değer.

> [!NOTE]
> Yalnızca silerseniz `OldValuesParameterFormatString` Tasarım görünümünde, özelliği Özellikler penceresinden özellik değeri, bildirim temelli söz diziminde var olmaya devam edecek, ancak boş bir dize olarak ayarlayın. Kaldırabilir ya da özelliği tamamen bildirim temelli söz veya, Özellikler penceresinden değeri varsayılan olarak ayarlamak `{0}`.


ObjectDataSource yalnızca sahipken `UpdateParameters` ürün adı, fiyatı ve kimliği için Visual Studio BoundField veya CheckBoxField GridView içinde ürünün alanların her biri için eklemiştir.


[![GridView BoundField veya CheckBoxField ürünün alanların her biri için içerir](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Şekil 4**: GridView BoundField veya CheckBoxField ürünün alanların her biri için içerir ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))


Son kullanıcı, bir ürün düzenler ve kendi güncelleştir düğmesine tıkladığında, salt okunur olmayan alanlarla GridView numaralandırır. Ardından ObjectDataSource içinde karşılık gelen parametre değerini ayarlayan `UpdateParameters` kullanıcı tarafından girilen değer koleksiyonu. Karşılık gelen bir parametre değilse GridView bir koleksiyona ekler. Bizim GridView BoundFields ve tüm ürün alanları için CheckBoxFields içeriyorsa, bu nedenle, ObjectDataSource çağrılırken ayarlama sona erecek `UpdateProduct` tüm olgu rağmen bu parametre alan aşırı yüklemesini, ObjectDataSource bildirim temelli biçimlendirme (bkz: Şekil 5) yalnızca üç giriş parametrelerini belirtir. Salt okunur olmayan bir bileşimi varsa benzer şekilde, ürün için giriş parametrelerini karşılık gelmiyor GridView alanlarını bir `UpdateProduct` aşırı yükleme, güncellemeye çalışırken bir özel durum oluşturulur.


[![GridView olacak ObjectDataSource UpdateParameters koleksiyonuna parametre ekleyin](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Şekil 5**: GridView olacak parametreler ekleme ObjectDataSource `UpdateParameters` koleksiyon ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))


ObjectDataSource çağırır emin olmak için `UpdateProduct` yalnızca ürün adı, fiyatı ve kimliği, alan aşırı yüklemesini düzenlenebilir alanlar için sahip olmaya GridView kısıtlamak ihtiyacımız yalnızca `ProductName` ve `UnitPrice`. Bu diğer BoundFields ve CheckBoxFields, bu diğer alanları ayarlayarak kaldırarak gerçekleştirilebilir `ReadOnly` özelliğini `true`, veya ikisinin birleşimi. Bu öğretici için şimdi yalnızca hariç tüm GridView alanları Kaldır `ProductName` ve `UnitPrice` BoundFields, sonra GridView'ın bildirim temelli biçimlendirme görünür gibi:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Olsa da `UpdateProduct` aşırı giriş üç parametre bekliyor, yalnızca iki BoundFields bizim GridView sahibiz. Bunun nedeni, `productID` giriş parametresi bir birincil anahtar değeri ve değerin geçirilen `DataKeyNames` düzenlenen satır için özellik.

Bizim GridView ile birlikte `UpdateProduct` aşırı yükleme, kullanıcının herhangi bir ürün alanları kaybetmeden yalnızca adını ve ürünün fiyatı düzenlemenize izin verir.


[![Yalnızca ürün adını ve fiyat düzenleme arabirim sağlar.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Şekil 6**: yalnızca ürün adını ve fiyat düzenleme arabirimi sağlar ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))


> [!NOTE]
> Önceki öğreticide açıklandığı gibi s görünüm durumu GridView (varsayılan davranış) etkin oldukça önemlidir. GridView s ayarlarsanız `EnableViewState` özelliğini `false`, eş zamanlı kullanıcıların yanlışlıkla silme veya düzenleme kayıtları riskiyle karşılaşırsınız. Bkz: [uyarısı: eşzamanlılık sorunu ile ASP.NET 2.0 GridViews/DetailsView/FormViews bu düzenleme desteği ve/veya silme ve Whose görünüm durumu devre dışı](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) daha fazla bilgi için.


## <a name="improving-theunitpriceformatting"></a>Geliştirme`UnitPrice`biçimlendirme

Şekil 6 çalışır, gösterilen GridView örneği while `UnitPrice` alanı hiç biçimlendirilmemiş, herhangi bir para birimi olmayan bir fiyat ekranda kaynaklanan simgelerini ve dört ondalık basamağı varsa. Bir para birimi düzenlenemez satırlar için biçimlendirme uygulamak için ayarlamanız yeterlidir `UnitPrice` BoundField'ın `DataFormatString` özelliğini `{0:c}` ve kendi `HtmlEncode` özelliğini `false`.


[![UnitPrice'nın DataFormatString ve HtmlEncode özellikleri uygun şekilde ayarlayın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Şekil 7**: ayarlayın `UnitPrice`'s `DataFormatString` ve `HtmlEncode` özellikleri uygun şekilde ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))


Bu değişiklik, düzenlenemez satırları fiyat bir para birimi olarak Biçimlendir; düzenlenen satır, ancak yine de para birimi simgesi olmadan ve dört ondalık basamak değeri görüntüler.


[![Para birimi değerler artık biçimlendirilmiş Satırlar düzenlenemez:](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Şekil 8**: düzenlenemez satırlarıdır artık biçimlendirilmiş para birimi değerleri olarak ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))


Belirtilen biçimlendirme yönergeleri `DataFormatString` özelliği uygulanabilir düzenleme arabirimine BoundField'ın ayarlayarak `ApplyFormatInEditMode` özelliğini `true` (varsayılan değer `false`). Bu özelliği ayarlamak bir dakikanızı `true`.


[![UnitPrice BoundField'ın ApplyFormatInEditMode özelliğini true olarak ayarlayın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Şekil 9**: ayarlayın `UnitPrice` BoundField'ın `ApplyFormatInEditMode` özelliğini `true` ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))


Bu değişiklik, değeri ile `UnitPrice` düzenlenen görüntülenen satır ayrıca bir para birimi olarak biçimlendirilmiş.


[![Düzenlenen sıranın UnitPrice artık biçimlendirilmiş bir para birimi olarak değerdir](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Şekil 10**: düzenlenebilir sıranın `UnitPrice` değerdir artık biçimlendirilmiş bir para birimi olarak ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))


Gibi $19.00 oluşturur ancak, bir ürün metin kutusuna para birimi simgesi güncelleştiriliyor bir `FormatException`. GridView çalıştığında ObjectDataSource kullanıcı tarafından sağlanan değerleri atamak `UpdateParameters` dönüştüremedi olduğu koleksiyon `UnitPrice` içine "$19.00" dize `decimal` parametresi tarafından gerekli (bkz. Şekil 11). Bu sorunu gidermek için bir olay işleyicisi GridView için 's oluşturabiliriz `RowUpdating` olay ve kullanıcı tarafından sağlanan ayrıştırma `UnitPrice` para biçimli olarak `decimal`.

GridView'ın `RowUpdating` olay kabul eder, ikinci parametre olarak bir nesne türü [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), içeren bir `NewValues` sözlük kullanıcı tarafından sağlanan değerleri hazır tutan özelliklerinden biri olarak ObjectDataSource atanan `UpdateParameters` koleksiyonu. Biz varolan üzerine `UnitPrice` değerini `NewValues` ondalık bir değeri ile koleksiyon ayrıştırılmış kodu aşağıdaki satırlarla para birimi biçimi kullanarak `RowUpdating` olay işleyicisi:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Kullanıcı sağlamışsa bir `UnitPrice` değeri ("$19.00 gibi"), bu değer tarafından hesaplanan ondalık değer ile yazılır [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), ayrıştırma bir para birimi değeri. Bu tüm para birimi sembolleri, virgül, ondalık basamak ve benzeri durumunda ondalık doğru ayrıştırmaz ve kullanır [NumberStyles numaralandırma](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) içinde [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) ad alanı.

Şekil 11 gösterir iki sorun kullanıcı tarafından sağlanan para birimi sembolleri kaynaklanan `UnitPrice`, nasıl birlikte GridView'ın `RowUpdating` olay işleyicisi kullanılan tür girişi düzgün ayrıştırılamadı.


[![Düzenlenen sıranın UnitPrice artık biçimlendirilmiş bir para birimi olarak değerdir](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Şekil 11**: düzenlenebilir sıranın `UnitPrice` değerdir artık biçimlendirilmiş bir para birimi olarak ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>2. adım: yasaklanması`NULL UnitPrices`

İzin vermek için veritabanı yapılandırılırken `NULL` değerler `Products` tablonun `UnitPrice` sütunu istiyoruz belirleyerek söz konusu sayfa ziyaret ettiği önlemek bir `NULL` `UnitPrice` değeri. Diğer bir deyişle, bir kullanıcı girin başarısız olursa bir `UnitPrice` istiyoruz bu sayfadan belirtilen bir fiyat düzenlenen ürünlerden olmalıdır, kullanıcı bildiren bir ileti görüntülemek için veritabanına sonuçlarını kaydetmek yerine Ürün satır düzenlerken değeri.

`GridViewUpdateEventArgs` Nesnesi geçirildi GridView ait `RowUpdating` olay işleyici içerir bir `Cancel` özelliği, varsa kümesine `true`, bir güncelleştirme işlemini sonlandırır. Github'dan genişletmek `RowUpdating` ayarlamak için olay işleyicisi `e.Cancel` için `true` ve değilse nedenini açıklayan bir ileti görüntüler `UnitPrice` değerini `NewValues` koleksiyonu `null`.

Başlangıç sayfası için bir etiket Web denetimi ekleyerek `MustProvideUnitPriceMessage`. Bu etiket denetimi, kullanıcı belirtmek başarısız olursa görüntülenecek bir `UnitPrice` bir ürün güncelleştirirken değeri. Etiketin `Text` özelliğini, "Ürün için fiyat sağlamalısınız." Ayrıca, yeni bir CSS sınıfı oluşturmuş olduğunuz `Styles.css` adlı `Warning` aşağıdaki tanımıyla:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Son olarak, etiketin `CssClass` özelliğini `Warning`. Bu noktada Tasarımcı uyarı iletisi kırmızı, kalın, italik, çok büyük yazı tipi boyutu GridView yukarıda Şekil 12'de gösterildiği gibi göstermelidir.


[![Bir etiket GridView eklenmiştir.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Şekil 12**: bir etiket sahip olan eklenen yukarıda GridView ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))


Varsayılan olarak, bu etiket, olacak şekilde ayarlamanız gizlenmelidir, `Visible` özelliğini `false` içinde `Page_Load` olay işleyicisi:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Kullanıcı belirtmeden bir ürünü güncellemek deneyip denemeyeceğini `UnitPrice`, güncelleştirmeyi iptal eder ve uyarı etiketi görüntülemek istiyoruz. GridView'ın büyütmek `RowUpdating` olay işleyicisi aşağıdaki gibi:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Bir kullanıcı bir fiyat belirtmeden bir ürün kaydetmeye çalışırsa, güncelleştirme iptal edildi ve yararlı bir ileti görüntülenir. While veritabanı (ve iş mantığı) izin veren `NULL` `UnitPrice` s, bu belirli ASP.NET sayfası yok.


[![Bir kullanıcı UnitPrice boş ayrılamazsınız](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Şekil 13**: bir kullanıcı çıkamaz `UnitPrice` boş ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))


GridView'ın kullanmayı şimdiye gördük `RowUpdating` atanan ObjectDataSource parametre değerlerini programlı olarak değiştirmek için olay `UpdateParameters` de toplama güncelleştirme işlemi iptal etmek için nasıl tamamen. Bu kavramlar DetailsView ve FormView denetimlere aktarılır ve ekleme ve silme için de geçerlidir.

Bu görevler için olay işleyicileri ile ObjectDataSource düzeyinde de yapılabilir, `Inserting`, `Updating`, ve `Deleting` olayları. Bu olayları temel nesnenin ilişkili yöntemi çağrılmadan önce harekete ve giriş parametrelerini koleksiyonu değiştirmek veya yükseltebilir işlemi iptal etmek için bir son şans fırsatı sağlar. Bu üç olayları için olay işleyicileri türünde bir nesne geçirilir [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , ilgilenilen iki özelliğe sahiptir:

- [İptal](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), varsa kümesine `true`, gerçekleştirilen işlemin iptal eder
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), koleksiyonu olduğu `InsertParameters`, `UpdateParameters`, veya `DeleteParameters`olay işleyicisi için olup bağlı olarak `Inserting`, `Updating`, veya `Deleting` olay

ObjectDataSource düzeyinde parametre değerleri ile çalışma göstermek için şimdi sayfamızı yeni ürün eklemek kullanıcılara bir DetailsView içerir. Bu DetailsView hızlı bir şekilde veritabanına yeni ürün eklemek için bir arabirim sağlamak üzere kullanılır. Şimdi yeni bir ürün eklenmesine izin ver, yalnızca için değerleri girmesini tutarlı bir kullanıcı arabirimi tutmak `ProductName` ve `UnitPrice` alanları. Varsayılan olarak, DetailsView'ın ekleme arabiriminde sağlanan olmayan değerlere ayarlanacak bir `NULL` veritabanı değeri. Ancak, ObjectDataSource kullanabiliriz `Inserting` kısa süre içinde anlatıldığı gibi farklı varsayılan değerlerine eklemesine olay.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>3. adım: Yeni ürün eklemek için bir arabirim sağlar.

Bir DetailsView GridView yukarıda tasarımcıya araç kutusundan sürükleyin Temizle kendi `Height` ve `Width` özellikleri ve ObjectDataSource için zaten mevcut sayfada bağlarsınız. Bu BoundField veya CheckBoxField ürünün alanların her biri için ekler. Biz bu DetailsView yeni ürün eklemek için kullanmak istiyorsanız akıllı etiket ekleme Etkinleştir seçeneği denetlenecek gerekir; Ancak, bu seçeneği yoktur çünkü ObjectDataSource `Insert()` yöntemi, bir yönteme eşlenmez `ProductsBLL` sınıfı (veri kaynağı yapılandırma gördüğünüzde Şekil 3 Biz bu eşleme (hiçbiri) ayarını geri çağırma).

ObjectDataSource yapılandırmak için Sihirbazı başlatılıyor, akıllı etiketten veri kaynağı yapılandırma bağlantıyı seçin. İlk ekrana ObjectDataSource bağlı temel alınan nesnede değiştirmenize izin verir; izin kümesi için `ProductsBLL`. Sonraki ekranda, temel alınan nesnenin ObjectDataSource yöntemlerinden eşlemelere listeler. Biz açıkça, belirtilen olsa bile `Insert()` ve `Delete()` yöntemleri eşlenmemiş herhangi bir yöntem, INSERT ve DELETE sekmeleri giderseniz bir eşleştirme olduğunu görürsünüz. Bunun nedeni, `ProductsBLL`'s `AddProduct` ve `DeleteProduct` yöntemleri `DataObjectMethodAttribute` için varsayılan yöntemleri olduğunu belirtmek için özniteliği `Insert()` ve `Delete()`sırasıyla. Bu nedenle, bu her zaman açıkça belirtilen başka bir değer yoksa, Sihirbazı çalıştırmak ObjectDataSource Sihirbazı'nı seçer.

Bırakın `Insert()` işaret yöntemi `AddProduct` yöntemi, ancak yeniden silme sekmenin açılır listede (hiçbiri) ayarlayın.


[![Aşağı açılan liste ekleme sekmenin AddProduct yöntemi ayarlama](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Şekil 14**: Ekle sekmenin yanıdaki açılan listeyi `AddProduct` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))


[![DELETE sekmenin aşağı açılan listesi (yok)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Şekil 15**: silme sekmenin açılır listede (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))


Bu değişiklikleri yaptıktan sonra bildirim temelli söz dizimi ObjectDataSource içerecek şekilde genişletilir bir `InsertParameters` aşağıda gösterildiği gibi koleksiyon:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Sihirbazın geri eklenen artırarak algoritmanın yeniden çalıştırılması `OldValuesParameterFormatString` özelliği. Bu özellik, varsayılan değere ayarlayarak temizlemek için bir dakikanızı ayırın (`{0}`) veya bildirim temelli söz dizimi toptan kaldırılıyor.

ObjectDataSource ile ekleme özellikleriyle DetailsView'ın akıllı etiket artık eklemeyi etkinleştir onay kutusunu içerir; Tasarımcıya geri dönün ve bu seçeneği işaretleyin. Yalnızca iki BoundFields - sahip olacak şekilde DetailsView ardından, küçültmek `ProductName` ve `UnitPrice` - ve CommandField. Bu noktada DetailsView'ın bildirim temelli söz dizimi gibi görünmelidir:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

Şekil 16, bu noktada bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir. Gördüğünüz gibi DetailsView (Chai) ilk ürünün fiyatı ve adını listeler. İstediğimiz gibi ancak kullanıcının hızlı bir şekilde veritabanına yeni ürün eklemek bir yol sağlayan bir ekleme arabirimidir.


[![DetailsView şu anda işlenen salt okunur modda olduğundan](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Şekil 16**: DetailsView olduğundan şu anda işlenen salt okunur modda ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))


DetailsView ihtiyacımız ayarlamak için ekleme modunda göstermek için `DefaultMode` özelliğini `Inserting`. Bu ilk ziyaret edildiğinde ekleme modunda DetailsView işler ve var. yeni bir kayıt ekledikten sonra sürdürür. Şekil 17 gösterildiği gibi böyle bir DetailsView yeni bir kayıt eklemek için hızlı bir arabirim sağlar.


[![DetailsView hızlı bir şekilde yeni ürün eklemek için bir arabirim sağlar.](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Şekil 17**: DetailsView bir arabirim hızlı bir şekilde eklemek için yeni bir ürün sağlar ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))


Kullanıcı bir ürün adı ve Fiyat (örneğin, "GDB suyu" ve 1.99, Şekil 17 olduğu gibi) girip tıkladığında Ekle eşleştiğinde bir geri gönderme ensues ve veritabanına eklenen yeni bir ürün kaydındaki sonuçlanan ekleme iş akışı başlatır. DetailsView ekleme arabirimiyle ve GridView otomatik olarak DataSet'e veri kaynağına yeni ürün eklemek için Şekil 18'de gösterildiği gibi tutar.


![Ürün](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Şekil 18**: "GDB suyu" ürünü için veritabanı eklendi


DetailsView arabiriminden eksik ürün alanları, Şekil 18 içinde GridView göstermez ancak `CategoryID`, `SupplierID`, `QuantityPerUnit`ve benzeri atanan `NULL` veritabanı değerleri. Bu, aşağıdaki adımları uygulayarak görebilirsiniz:

1. Visual Studio sunucu Gezgini'nde Git
2. Genişletme `NORTHWND.MDF` veritabanı düğümü
3. Sağ `Products` veritabanı Tablo düğümü
4. Tablo verileri Göster'i seçin

Bu tüm kayıtları listeleyecek `Products` tablo. Şekil 19 gösterildiği gibi tüm müşterilerimize yeni ürünün sütunlarının dışında `ProductID`, `ProductName`, ve `UnitPrice` sahip `NULL` değerleri.


[![Ürün alanları sağlanmadı DetailsView içinde NULL değerler atanır](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Şekil 19**: ürün alanları sağlanmadı DetailsView içinde atanmış `NULL` değerleri ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))


Varsayılan değer dışında sağlamak isteyebilirsiniz `NULL` biri veya birkaçı için sütun değerleri, ya da çünkü `NULL` en iyi varsayılan seçeneği değil veya veritabanı sütununa izin `NULL` s. Bunu gerçekleştirmek için biz programlı olarak DetailsView'ın parametrelerinin değerlerini ayarlayabilirsiniz `InputParameters` koleksiyonu. Bu atama ya da olay işleyicisi DetailsView için 's yapılabilir `ItemInserting` olay veya ObjectDataSource `Inserting` olay. Biz zaten inceledik beri düzeyi denetim öncesi ve sonrası düzeyi olaylarını kullanarak veri Web, bu kez ObjectDataSource olayları kullanarak inceleyelim.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>4. adım: Değerler atamada`CategoryID`ve`SupplierID`parametreleri

Bu öğretici için şimdi bu arabirimi aracılığıyla yeni bir ürün eklerken uygulamamız için bunu atanmasını Imagine bir `CategoryID` ve `SupplierID` 1 değeri. Daha önce bahsedildiği gibi ObjectDataSource veri değişikliği işlemi sırasında Ateş öncesi ve sonrası düzeyi olay çifti sahiptir. Olduğunda, `Insert()` yöntemi çağrıldığında, ilk ObjectDataSource başlatır, `Inserting` olay, ardından yöntemini çağırır, kendi `Insert()` yöntemi için eşlenen ve son olarak yükseltir `Inserted` olay. `Inserting` Olay işleyicisi bize girdi parametrelerinin ince veya yükseltebilir işlemi iptal etmek için bir son fırsatı verir.

> [!NOTE]
> Bir gerçek yaşam uygulaması, büyük olasılıkla isteyeceğiniz için izin verin kullanıcı, üretici ve kategoriye belirtin veya bu değer için bunları bazı ölçütlere göre seçiyordu veya iş mantığı (yerine doğrudan bir kimliği 1'i seçerek). Ne olursa olsun, örnek giriş parametresi değeri ObjectDataSource önceden düzeyi olayından programlanarak nasıl ayarlanacağını gösterir.


ObjectDataSource için bir olay işleyicisi oluşturmak için birkaç dakikanızı `Inserting` olay. Olay işleyicinin ikinci girdi parametresi türü bir nesne olduğunu fark `ObjectDataSourceMethodEventArgs`, parametre koleksiyonunu erişmek üzere bir özelliğe sahip (`InputParameters`) ve işlemi iptal etmek için bir özellik (`Cancel`).


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

Bu noktada, `InputParameters` özelliği içeren ObjectDataSource `InsertParameters` DetailsView atanan değerleri ile koleksiyonu. Yalnızca bu parametrelerden birinin değeri değiştirmek için kullanın: `e.InputParameters["paramName"] = value`. Bu nedenle, ayarlanacak `CategoryID` ve `SupplierID` 1 değerine ayarlamak `Inserting` olay işleyicisi aşağıdaki gibi aramak için:


[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Bu saat (örneğin, GDB Soda), yeni bir ürün eklerken `CategoryID` ve `SupplierID` yeni ürünü sütunlarının 1 olarak ayarlayın (bkz. Şekil 20).


[![Yeni ürünler artık sahip kendi CategoryID ve SupplierID değerleri kümesi 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Şekil 20**: yeni ürünler artık sahip Their `CategoryID` ve `SupplierID` değerleri 1 olarak ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))


## <a name="summary"></a>Özet

Düzenleme, ekleme ve silme işlemi sırasında hem veri Web denetimi hem de ObjectDataSource öncesi ve sonrası düzeyi olayları bir dizi devam edin. Bu öğreticide önceden düzeyinde olaylar incelenir ve bu giriş parametrelerini özelleştirebilir veya veri değiştirme işlemi iptal etmek için tamamen hem veri Web denetimi ve ObjectDataSource olayları kullanmayı öğrendiniz. Sonraki öğreticide oluşturma ve sonrası düzeyi olayları için olay işleyicileri kullanılarak şu konuları inceleyeceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Jackie Goor ve Liz Shulok yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [İleri](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
