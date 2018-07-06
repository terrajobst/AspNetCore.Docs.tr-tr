---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: (VB) ObjectDataSource ile verileri görüntüleme | Microsoft Docs
author: rick-anderson
description: Bu öğreticide havi olmadan önceki öğreticide oluşturulan BLL hizmetinden alınan verileri bağlayabilirsiniz bu denetimi kullanarak ObjectDataSource Denetimi bakan...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 759403bf175cfb2d44cc53ab497c55c354733f15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832730"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>(VB) ObjectDataSource ile verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) veya [PDF olarak indirin](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Bu öğreticide, bir satır kod yazmanıza gerek kalmadan önceki öğreticide oluşturulan BLL hizmetinden alınan verileri bağlayabilirsiniz bu denetimi kullanarak ObjectDataSource Denetimi, görünüyor!


## <a name="introduction"></a>Giriş

Bizim uygulama mimarisi ve Web sitesi sayfa düzeni ile tam, çeşitli veri ve raporlama ilgili genel görevleri yerine getirmeyi keşfetmeye başlamak hazırız. Önceki öğreticilerde biz program aracılığıyla veri BLL ve DAL Web denetimi bir ASP.NET sayfasında veri bağlamak öğrendiniz. Bu sözdizimi veri Web denetimi atama `DataSource` verileri görüntüleme ve ardından denetimin arama özelliğini `DataBind()` yöntemi olan ASP.NET 1.x uygulamalarında kullanılan desen ve 2.0 uygulamalarınızda kullanılacak devam edebilirsiniz. Ancak, ASP.NET 2.0'ın yeni veri kaynağı denetimleri verileri ile çalışma için bildirim temelli bir yöntemini sunar. Bu denetimleri kullanarak bir satır kod yazmanıza gerek kalmadan önceki öğreticide oluşturulan BLL hizmetinden alınan verileri bağlayabilirsiniz!

ASP.NET 2.0 beş yerleşik veri kaynağı denetimleri ile birlikte gelen [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), ve [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) kendi oluşturabilirsiniz ancak [özel veri kaynağı denetimleri](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), gerekirse. Öğretici uygulamamız için bir mimari geliştirdik beri ObjectDataSource bizim BLL sınıfları karşı kullanacağız.


![ASP.NET 2.0, beş yerleşik veri kaynağı denetimleri içerir.](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Şekil 1**: ASP.NET 2.0 beş yerleşik veri kaynaklarını içerir


ObjectDataSource, başka bir nesne ile çalışmak için bir proxy olarak görev yapar. ObjectDataSource yapılandırmak için bu nesne ve metotlarını ObjectDataSource nasıl eşleştiği temel belirttiğimiz `Select`, `Insert`, `Update`, ve `Delete` yöntemleri. Ardından bu nesnesini belirtilen ve ObjectDataSource metotlarını eşlenen sonra biz Web denetimi veri ObjectDataSource bağlayabilirsiniz. ASP.NET, çoğu veri GridView ve DetailsView, RadioButtonList ve DropDownList, diğerlerinin yanı sıra dahil olmak üzere, Web denetimleri ile birlikte gelir. Sayfa yaşam döngüsü sırasında Web denetimi verileri kendi ObjectDataSource çağırarak yapacaktır bağlı olarak veri erişimi gerekebilir `Select` yöntemi Web denetimi veri destekler ekleme, güncelleştirme veya silme çağrıları için yapılabilir; kendi ObjectDataSource `Insert`, `Update`, veya `Delete` yöntemleri. Aşağıdaki diyagramda gösterildiği gibi bu çağrılar ObjectDataSource uygun temel alınan nesnenin yöntemlerine göre yönlendirilir.


[![ObjectDataSource bir Proxy olarak görev yapar.](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Şekil 2**: Proxy olarak hizmet veren ObjectDataSource ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


ObjectDataSource ekleme yöntemlerini çağırmak için kullanılabilse de, güncelleştirme veya veri silme şimdi yalnızca veri döndüren üzerinde odaklanın; ObjectDataSource ile verileri değiştiren veri Web denetimleri sonraki öğreticiler inceleyeceksiniz.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>1. adım: Ekleme ve ObjectDataSource Denetimi yapılandırma

Başlangıç açarak `SimpleDisplay.aspx` sayfasını `BasicReporting` klasöründe Tasarım görünümüne geçiş yapın ve ardından sayfanın Tasarım yüzeyine araç kutusundan bir ObjectDataSource denetimi sürükleyin. Tüm biçimlendirme üretmediği ObjectDataSource tasarım yüzeyinde gri bir kutu olarak görünür; yalnızca belirtilen bir nesneden bir yöntemi çağırarak verileri erişir. Bir ObjectDataSource tarafından döndürülen veriler bir veri GridView DetailsView, FormView ve benzeri gibi Web denetimi tarafından görüntülenebilir.

> [!NOTE]
> Alternatif olarak, öncelikle, sayfaya Web denetimi veri eklemek ve ardından, akıllı etiketinde &lt;yeni veri kaynağı&gt; aşağı açılan listeden seçeneği.


ObjectDataSource nesnesini ve nesnenin yöntemleri ObjectDataSource nasıl eşleştiğini belirtmek için akıllı etiket ObjectDataSource yapılandırma veri kaynağı bağlantısından tıklayın.


[![Tıklayın veri kaynağı bağlantısından akıllı etiket yapılandırma](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Şekil 3**: Akıllı etiket yapılandırmak veri kaynağı bağlantısından tıklayın ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Bu veri kaynağı Yapılandırma Sihirbazı ' getirir. İlk olarak biz ObjectDataSource ile çalışmak için nesne belirtmeniz gerekir. "Yalnızca veri bileşenleri Göster" onay kutusu işaretli değilse bu ekranı aşağı açılan listede yalnızca ile donatılmış nesneleri listeler `DataObject` özniteliği. Şu anda listemize TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi ve önceki öğreticide oluşturduğumuz BLL sınıfları içerir. Eklemeyi unuttuysanız `DataObject` özniteliği iş mantığı katmanı sınıfları, bunları bu listesinde görmezsiniz. Bu durumda, BLL sınıfları (yanı sıra diğer sınıfların türü belirtilmiş veri kümesi DataTable, DataRow ve benzeri) içermelidir tüm nesneleri görüntülemek için "veri bileşenleri Göster" onay kutusunun işaretini kaldırın.

Bu ilk ekranında seçin `ProductsBLL` sınıfı aşağı açılan listeden ve İleri'ye tıklayın.


[![ObjectDataSource Denetimi ile kullanılacak nesneyi belirtin](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Şekil 4**: kullanılacak nesnesi ile ObjectDataSource Denetimi belirtin ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Sihirbazın sonraki ekranında ObjectDataSource çağıracağı yöntemi seçmenizi ister. Aşağı açılan önceki ekranından seçilen nesneyi veri döndüren bu yöntemler listelenmiştir. Burada görürüz `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, ve `GetProductsBySupplierID`. Seçin `GetProducts` yöntemi açılan liste ve son (eklediyseniz `DataObjectMethodAttribute` için `ProductBLL`ait önceki öğreticide, bu seçenek gösterildiği gibi yöntemler, varsayılan olarak seçilir).


[![Veri döndüren SELECT sekmesinden yöntemini seçin.](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Şekil 5**: yöntemi için veri döndüren seçin sekmesinden seçin ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>ObjectDataSource el ile yapılandırma

ObjectDataSource veri kaynağı Yapılandırma Sihirbazı'nı kullanır nesnesi belirtin ve nesnenin yöntemleri çağrılmadan ilişkilendirmek için hızlı bir yol sunar. Ancak, Özellikler penceresinde veya doğrudan bildirim temelli biçimlendirmede özellikleri aracılığıyla ObjectDataSource yapılandırabilirsiniz. Ayarlamanız yeterlidir `TypeName` kullanılabilmesi için temel alınan nesnenin türü özelliğini ve `SelectMethod` için veriler alınırken çağrılacak yöntem.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Sihirbaz yalnızca geliştirici tarafından oluşturulan sınıflar listeler gibi ObjectDataSource el ile yapılandırmak için gerektiğinde zamanlar olabilir veri kaynağı Yapılandırma Sihirbazı'nı tercih ettiğiniz bile. ObjectDataSource .NET Framework sınıfında gibi bağlamak istiyorsanız [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx), kullanıcı hesabı bilgilerini erişmeye veya [Directory sınıfı](https://msdn.microsoft.com/library/system.io.directory.aspx) dosya sistemi bilgileri ile çalışmak için ObjectDataSource özellikleri el ile ayarlamanız gerekir.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>2. adım: veri Web denetim ekleme ve ObjectDataSource için bağlama

ObjectDataSource sayfasına eklenen ve yapılandırılmış sonra ObjectDataSource tarafından döndürülen verileri görüntülemek için sayfanın veri Web denetimleri ekleme hazırız `Select` yöntemi. Herhangi bir veri Web denetimi için bir ObjectDataSource bağlanabilir; bir GridView, DetailsView ve FormView ObjectDataSource veri görüntüleme bakalım.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>GridView ObjectDataSource için bağlama

Bir GridView denetimi için araç kutusundan ekleme `SimpleDisplay.aspx`'s tasarım yüzeyi. GridView'ın akıllı etiketten 1. adımda eklediğimiz ObjectDataSource Denetimi seçin. Bu otomatik olarak bir BoundField ObjectDataSource döndürülen veriler tarafından her bir özellik için GridView oluşturacaktır `Select` yöntemi (yani, ürünleri DataTable tarafından tanımlanan tüm özellikler).


[![GridView sayfaya eklenen ve ObjectDataSource için bağlı](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Şekil 6**: A GridView eklendi ObjectDataSource bağlanan ve sayfa ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Özelleştirme, yeniden düzenleyebilir veya Akıllı Etiket Sütunları Düzenle seçeneğini tıklayarak GridView'ın BoundFields kaldırın.


[![GridView'ın BoundFields sütunları Düzenle iletişim kutusu aracılığıyla yönetme](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Şekil 7**: GridView'ın BoundFields aracılığıyla Düzenle sütunları Yönet iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


GridView'ın BoundFields kaldırma, değiştirme için bir dakikanızı ayırın `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` BoundFields. Yalnızca sol alt listeden BoundField seçin ve (bunları kaldırmak için kırmızı X) Sil düğmesine tıklayın. Ardından, BoundFields yeniden böylece `CategoryName` ve `SupplierName` BoundFields önünde `UnitPrice` bu BoundFields seçip Yukarı oka tıklayarak BoundField. Ayarlama `HeaderText` için kalan BoundFields özelliklerini `Products`, `Category`, `Supplier`, ve `Price`sırasıyla. Ardından, sahip `Price` BoundField BoundField'ın ayarlayarak bir para birimi olarak biçimlendirilmiş `HtmlEncode` özelliğini False olarak ve kendi `DataFormatString` özelliğini `{0:c}`. Son olarak, yatay Hizala `Price` sağa ve `Discontinued` onay kutusu Merkezi'nde `ItemStyle` / `HorizontalAlign` özelliği.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![GridView'ın BoundFields özelleştirilmiş](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Şekil 8**: GridView'ın BoundFields özelleştirilmiş ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Tema için tutarlı bir görünüm kullanma

Herhangi bir denetim düzeyi stil ayarları kaldırmak bu öğreticileri çaba, bunun yerine geçişli stil sayfaları kullanarak tanımlanan bir harici dosyada mümkün olduğunda. `Styles.css` Dosyasını içeren `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, ve `AlternatingRowStyle` verilerin görünümünü dikte için kullanılması gereken CSS sınıfları Web bu öğreticilerde kullanılan denetimler. Bunu yapmak için biz GridView'ın ayarlayabilirsiniz `CssClass` özelliğini `DataWebControlStyle`ve onun `HeaderStyle`, `RowStyle`, ve `AlternatingRowStyle` özelliklerin `CssClass` özellikleri uygun şekilde.

Bunlar ayarlarsanız `CssClass` açıkça her veriler için bu özellik değerlerini ayarlamak basitçe hatırlanan #içeren biz Web denetimi özelliklerini Web denetimi için öğreticilerimizi eklendi. GridView için DetailsView, CSS ile ilgili varsayılan özelliklerini tanımlamak için daha kolay yönetilebilir bir yaklaşım olan ve bir tema kullanarak FormView denetler. Bir tema, bir denetim düzeyi özellik ayarları, görüntüler ve sayfalar için genel bir görünüm zorlamak için bir site arasında uygulanabilir CSS sınıfları koleksiyonudur.

Bizim tema tüm görüntüler veya CSS dosyaları içermez (stil sayfası bırakacağız `Styles.css` olarak-web uygulaması kök klasöründe tanımlanmış olan), ancak iki css'li dış görünümler içerir. Bir dış Web denetimi için varsayılan özelliklerini tanımlayan bir dosyadır. Özellikle, biz varsayılan gösteren GridView ve DetailsView denetimlerini için bir dış görünüm dosyası gerekir `CssClass`-ilgili özellikler.

Adlı projenize yeni bir dış görünüm dosyası ekleyerek başlangıç `GridView.skin` Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle.


[![GridView.skin adlı bir dış görünüm dosyası Ekle](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Şekil 9**: dış görünüm dosyası adlandırılmış ekleme `GridView.skin` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Bir tema yerleştirilecek ihtiyacınız olan bulunur dış görünüm dosyaları `App_Themes` klasör. Böyle bir klasörü henüz yoksa bu yana ilk bizim kaplama eklerken bizim için oluşturmak Visual Studio genişliğinizin sunacaktır. Oluşturmak için Evet'i tıklatın `App_Theme` klasörü ve yeni yerleştirin `GridView.skin` dosya vardır.


[![Visual Studio App_Themes klasörünü oluşturmak istiyorum](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Şekil 10**: Visual Studio oluşturma izin `App_Theme` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Bu yeni bir tema oluşturur `App_Themes` GridView ile dış görünüm dosyası adında klasör `GridView.skin`.


![GridView tema App_Theme klasöre olan eklendi](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Şekil 11**: GridView tema sahip eklenmiş `App_Theme` klasörü


GridView tema DataWebControls için yeniden adlandırın (GridView klasöre sağ tıklayarak `App_Theme` klasörü ve Yeniden Adlandır seçeneğini belirleyin). Ardından, aşağıdaki biçimlendirme içine girin `GridView.skin` dosyası:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Bu varsayılan özelliklerini tanımlar `CssClass`-DataWebControls Tema kullanan herhangi bir sayfa içinde herhangi bir GridView için ilgili özellikler. Başka bir dış DetailsView için kısa bir süre sonra kullanacağız Web denetimi veri ekleyelim. DataWebControls tema adlı yeni bir dış görünüm Ekle `DetailsView.skin` ve aşağıdaki işaretlemeyi ekleyin:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Tanımlanan bizim tema ile ASP.NET sayfamızı temayı uygulamak son adımdır. Bir tema, bir sayfa tarafından temelinde veya bir Web sitesindeki tüm sayfalar için uygulanabilir. Bu tema Web sitesindeki tüm sayfalar için kullanalım. Bunu yapmak için aşağıdaki biçimlendirme eklemek `Web.config`'s `<system.web>` bölümü:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Tüm İşte bu kadar kolay! `styleSheetTheme` Ayarını gösterir Tema içinde belirtilen özellikleri gerektiğini *değil* denetimi düzeyinde belirtilen özellikleri geçersiz kıl. Tema Ayarları denetim ayarları trump belirtmek için kullanın `theme` yerine özniteliği `styleSheetTheme`; ne yazık ki, tema ayarları, Visual Studio Tasarım görünümünde görünmez. Başvurmak [ASP.NET temaları ve dış genel bakış](https://msdn.microsoft.com/library/ykzx33wh.aspx) ve [sunucu tarafı stilleri kullanarak Temalar](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) temalar ve dış; daha fazla bilgi için bkz. [nasıl yapılır: ASP.NET temaları uygulamak](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) hakkında daha fazla bilgi bir tema kullanmak için bir sayfa yapılandırma.


[![GridView ürün adı, kategori, tedarikçi, fiyat ve artık sağlanmayan bilgiler görüntüler.](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Şekil 12**: GridView ürün adı, kategori, tedarikçi, fiyat ve kullanımdan bilgi görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>DetailsView zamanında bir kayıt görüntüleme

GridView, bağlı olduğu veri kaynak denetimi tarafından döndürülen her kayıt için bir satır görüntüler. Ne zaman bir kerede tek bir kayıt veya tek bir kaydı görüntülemek istiyoruz zamanlar vardır. [DetailsView denetiminde](https://msdn.microsoft.com/library/s3w1w7t4.aspx) bir HTML olarak işlemeye, bu işlevler sunan `<table>` iki sütun ve her bir sütun veya denetime bağlı özelliği için bir satır. DetailsView bir tek kayıt 90 derece ile GridView olarak düşünebilirsiniz.

Bir DetailsView denetimi ekleyerek başlangıç *yukarıda* içinde GridView `SimpleDisplay.aspx`. Ardından, aynı ObjectDataSource denetimine GridView olarak bağlayın. GridView ile bir BoundField DetailsView ObjectDataSource tarafından döndürülen nesnedeki her özellik için eklenecek gibi `Select` yöntemi. DetailsView'ın BoundFields yatay yerine dikey olarak yerleştirilir, tek fark.


[![Bir DetailsView sayfaya ekleyin ve ObjectDataSource için bağlama](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Şekil 13**: bir DetailsView sayfaya ekleyin ve ObjectDataSource için bağlama ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


GridView gibi DetailsView'ın BoundFields ObjectDataSource tarafından döndürülen veriler daha özelleştirilmiş bir görünümünü sağlamak için tweaked. Şekil 14 sonra kendi BoundFields DetailsView gösterir ve `CssClass` özellikleri görünümünü GridView örneğe benzer hale getirmek için yapılandırılmış olması.


[![Tek bir kaydı DetailsView gösterir](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Şekil 14**: tek bir kaydı DetailsView gösterir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


DetailsView yalnızca kendi veri kaynağı tarafından döndürülen ilk kaydı gösterdiğini unutmayın. Tüm kayıtlar teker teker adım adım kullanıcıya izin vermek için disk belleği DetailsView için etkinleştirmelisiniz. Bunu yapmak için Visual Studio'ya geri dönün ve DetailsView'ın akıllı etiketinde sayfalama etkinleştir onay kutusunu işaretleyin.


[![DetailsView denetiminde ICollection](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Şekil 15**: disk belleği etkinleştirme DetailsView denetiminde ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Disk belleği etkin olan DetailsView ürünlerden birini görüntülemesini sağlar.](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Şekil 16**: disk belleği etkin DetailsView ürünlerden birini görüntülemesini sağlar. ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Gelecekte öğreticiler sayfalama hakkında daha fazla konuşacağız.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Bir kerede bir kaydı göstermek için daha esnek bir düzen

DetailsView oldukça katı ObjectDataSource döndürülen her kaydın nasıl bunu gösterir. Biz verilerin daha esnek bir görünüm isteyebilirsiniz. Örneğin, ayrı bir satırda ürün adı, kategori, tedarikçi, fiyat ve artık sağlanmayan bilgiler gösteriliyor yerine, ürün adı gösterip fiyat, istediğimiz bir `<h4>` , kategori ve tedarikçi bilgiler görünen başlık Aşağıdaki ad ve fiyat daha küçük bir yazı tipi boyutu. Ve şu değerleri yanında bulunan özellik adları (ürün, kategori vb.) göstermek önemsemez.

[FormView denetim](https://msdn.microsoft.com/library/fyf1dk77.aspx) bu özelleştirme düzeyini sağlar. Alanlar (GridView ve DetailsView yaptığı gibi) kullanmak yerine FormView izin veren bir karışımını statik HTML Web denetimleri için şablonları kullanır ve [veri bağlama söz dizimi](http://www.15seconds.com/issue/040630.htm). ASP.NET tarafından Repeater denetimiyle ilgili bilgi sahibi olduğunuz 1.x, düşündüğünüz FormView tek bir kaydı göstermek için bir yineleyici olarak.

Bir FormView'da denetimine ekleme `SimpleDisplay.aspx` sayfanın Tasarım yüzeyi. İlk başta, en azından, denetimin sağlamak için ihtiyacımız bize bildiren FormView gri bir blok olarak görüntüler `ItemTemplate`.


[![FormView gereken bir ItemTemplate içerir](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Şekil 17**: FormView içermelidir bir `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


FormView doğrudan veri kaynak denetimi varsayılan bir FormView akıllı etiket üzerinden bağlayabilirsiniz `ItemTemplate` otomatik olarak (ile birlikte bir `EditItemTemplate` ve `InsertItemTemplate`, ObjectDatatSource denetimin `InsertMethod` ve `UpdateMethod` özellikleri ayarlanır). Ancak, bu örnek için şimdi FormView için verilere bağlayın ve belirtin, `ItemTemplate` el ile. Başlangıç FormView ayarlayarak `DataSourceID` özelliğini `ID` ObjectDataSource Denetimi, `ObjectDataSource1`. Ardından, oluşturma `ItemTemplate` ürün adı ve fiyat gösterir, böylece bir `<h4>` öğesi ve altında kategori ve shipper adları bir küçük yazı tipi boyutu.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![İlk ürün (Chai) bir özel biçiminde görüntülenir.](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Şekil 18**: ilk ürün (Chai), bir özel biçiminde görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>` Veri bağlama söz dizimi. `Eval` Yöntemi FormView denetime bağlı geçerli nesne için belirtilen özelliğin değerini döndürür. Alex Homer'ın makaleyi inceleyin [Basitleştirilmiş ve genişletilmiş veri bağlama söz dizimi ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) ınit'in olumlu ve veri bağlama, daha fazla bilgi için.

DetailsView gibi FormView yalnızca ObjectDataSource döndürülen ilk kaydı gösterir. FormView ürünler tek bir adım adım ziyaretçilerin izin vermek için disk belleği etkinleştirebilirsiniz.

## <a name="summary"></a>Özet

Erişme ve iş mantığı katmanı verileri görüntüleyen bir ASP.NET 2.0'ın ObjectDataSource Denetimi sayesinde kod satırı yazmadan gerçekleştirilebilir. ObjectDataSource, bir sınıfın belirtilen bir yöntemi çağırır ve sonuçları döndürür. Bu sonuç için ObjectDataSource bağlı olduğu Web denetimi veri görüntülenebilir. Bu öğreticide FormView GridView ve DetailsView denetimlerini ObjectDataSource için bağlama sırasında incelemiştik.

Şu ana kadar bir parametresiz yöntemini çağırmak için ObjectDataSource kullanma yalnızca gördük, ancak ne bekliyor bir yöntemi çağırmak istediğimiz parametreleri gibi giriş `ProductBLL` sınıfın `GetProductsByCategoryID(categoryID)`? Bir veya daha fazla parametre bekliyor bir yöntem çağırmak için bu parametrelerin değerleri belirtmek için ObjectDataSource yapılandırmalısınız. Bunu gerçekleştirmek nasıl görüyoruz bizim [sonraki öğreticiye](declarative-parameters-vb.md).

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kendi veri kaynağı denetimleri oluşturma](https://msdn.microsoft.com/library/ms364049.aspx)
- [ASP.NET 2.0 GridView örnekleri](https://msdn.microsoft.com/library/aa479339.aspx)
- [Basitleştirilmiş ve genişletilmiş veri söz dizimi ASP.NET 2.0 bağlama](http://www.15seconds.com/issue/040630.htm)
- [ASP.NET 2.0 temalar](http://www.odetocode.com/Articles/423.aspx)
- [Sunucu tarafı stilleri Temalar kullanma](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Nasıl yapılır: ASP.NET temaları program aracılığıyla uygulama](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Hilton Giesenow oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [İleri](declarative-parameters-vb.md)
