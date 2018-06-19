---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: ObjectDataSource (VB) ile verileri görüntüleme | Microsoft Docs
author: rick-anderson
description: Bu öğretici havi olmadan önceki öğreticideki oluşturulan BLL kaynağından alınan veri bağlayabilirsiniz bu denetimi kullanarak ObjectDataSource Denetimi bakan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: ec3be56e1bb4294402351ff05e9209fe97510748
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877352"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>ObjectDataSource (VB) ile verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) veya [PDF indirin](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Bu öğretici bir satırlık bir kod yazmak zorunda kalmadan önceki öğreticide oluşturduğunuz BLL kaynağından alınan veri bağlayabilirsiniz bu denetimi kullanarak ObjectDataSource Denetimi bakan!


## <a name="introduction"></a>Giriş

Bizim uygulama mimarisi ve Web sitesi sayfa düzeni ile tam, biz çeşitli ortak verileri - ve Raporlama ile ilgili görevleri gerçekleştirmek nasıl keşfetmeye başlamak hazırsınız. Program aracılığıyla veri DAL ve BLL ASP.NET sayfası Web denetiminde veri nasıl bağlanacağını önceki eğitimlerine gördük. Verinin Web denetimi atama bu söz dizimini `DataSource` verileri görüntüleme ve denetimin çağırma özelliğine `DataBind()` yöntemi olan ASP.NET 1.x uygulamalarında kullanılan düzen ve 2.0 uygulamalarınızda kullanılacak devam edebilirsiniz. Ancak, ASP.NET 2.0'ın yeni veri kaynağı denetimleri verilerle çalışmak için bildirim temelli bir yolunu sunar. Bu denetimleri kullanarak bir satırlık bir kod yazmak zorunda kalmadan önceki öğreticide oluşturduğunuz BLL kaynağından alınan veri bağlayabilirsiniz!

ASP.NET 2.0 beş yerleşik veri kaynağı denetimleri ile birlikte gelen [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), ve [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) kendi oluşturabilirsiniz ancak [özel veri kaynağı denetimleri](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp)gerekirse. Eğitmen uygulamamız için bir mimari geliştirdik olduğundan, biz ObjectDataSource bizim BLL sınıfları karşı kullanırsınız.


![ASP.NET 2.0 beş yerleşik veri kaynağı denetimleri içerir](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Şekil 1**: ASP.NET 2.0, beş yerleşik veri kaynağı denetimleri içerir


ObjectDataSource, başka bir nesne ile çalışmak için bir proxy olarak görev yapar. ObjectDataSource yapılandırmak için bu nesne ve nasıl yöntemlerinden ObjectDataSource için 's eşleme temel belirttiğimiz `Select`, `Insert`, `Update`, ve `Delete` yöntemleri. Ardından bu nesnesini belirtilen ve yöntemlerinden ObjectDataSource için 's eşlenen sonra biz Web denetimi veri ObjectDataSource bağlayabilirsiniz. ASP.NET Web denetimleri, GridView, DetailsView, RadioButtonList ve DropDownList, diğerleri de dahil olmak üzere birçok verilerle birlikte gönderilir. Sayfa yaşam döngüsü sırasında Web denetimi veri kendi ObjectDataSource's çağırarak yapabiliriz bağlı için veri erişim gerekebilir `Select` yöntemi Web denetimi verilerini destekleyen ekleme, güncelleştirme veya silme çağrıları için yapılabilir; kendi ObjectDataSource'nın `Insert`, `Update`, veya `Delete` yöntemleri. Aşağıdaki diyagramda gösterildiği gibi bu çağrıları ObjectDataSource uygun temel alınan nesnenin yöntemleri tarafından yönlendirilir.


[![ObjectDataSource bir Proxy olarak görev yapar.](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Şekil 2**: Proxy olarak hizmet veren ObjectDataSource ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


ObjectDataSource ekleme yöntemleri çağırmak için kullanılabilir olsa da, güncelleştirme ya da verileri silme şimdi yalnızca veri döndüren odaklanın; Gelecekteki öğreticileri ObjectDataSource ve veri verileri değiştirme Web denetimleri kullanarak inceleyeceksiniz.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>1. adım: Ekleme ve ObjectDataSource Denetimi yapılandırma

Başlangıç açarak `SimpleDisplay.aspx` sayfasındaki `BasicReporting` klasörü, Tasarım görünümüne geçin ve sonra bir ObjectDataSource Denetimi Araç sayfanın Tasarım yüzeyine sürükleyin. Tüm biçimlendirme oluşturmadığı ObjectDataSource tasarım yüzeyine gri kutu olarak görüntülenir; yalnızca belirtilen bir nesneden bir yöntemini çağırarak verilere erişiyor. ObjectDataSource tarafından döndürülen verileri, verileri GridView, DetailsView, FormView ve benzerleri gibi Web denetimi tarafından görüntülenebilir.

> [!NOTE]
> Alternatif olarak, öncelikle, Web denetimi veri sayfasına ekleyin ve ardından, akıllı etiketten &lt;yeni veri kaynağı&gt; aşağı açılan listeden seçeneği.


ObjectDataSource'nın nesnesini ve o nesnenin yöntemleri ObjectDataSource için 's nasıl eşleme belirtmek için ObjectDataSource'nın akıllı etiket yapılandırma veri kaynağı bağlantısını tıklayın.


[![Tıklatın akıllı etiket veri kaynağı bağlantısını yapılandırın](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Şekil 3**: Akıllı etiket yapılandırma veri kaynağı bağlantısından tıklayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Bu veri kaynağı Yapılandırma Sihirbazı'nı açar. İlk olarak, biz ObjectDataSource çalışmak için nesne belirtmeniz gerekir. "Yalnızca veri bileşenlerini göster" onay kutusu işaretli değilse bu ekranda açılan listesinden, ile donatılmış nesneleri listeler `DataObject` özniteliği. Şu anda bizim listesi TableAdapters yazılan veri kümesi ve önceki öğreticide oluşturduğumuz BLL sınıfları içerir. Eklenecek unuttuysanız, `DataObject` özniteliği iş mantığı katmanı sınıfları, onları bu listede göremezsiniz. Bu durumda, BLL sınıfları (yanı sıra diğer sınıflarda yazılan veri kümesi DataTables, DataRow ve benzeri) içermelidir tüm nesneleri görüntülemek için "yalnızca veri bileşenlerini göster" onay kutusunun işaretini kaldırın.

Bu ilk ekranda seçin `ProductsBLL` sınıf aşağı açılan listeden ve İleri'yi tıklatın.


[![ObjectDataSource Denetimi ile kullanılacak nesnesi belirtin](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Şekil 4**: ObjectDataSource Denetimi ile kullanılacak nesnesi belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


Sihirbazın sonraki ekranında ObjectDataSource çağıracağı yöntemi seçmenizi ister. Aşağı açılan önceki ekranından seçili nesnesindeki dönüş verileri bu yöntemleri listelenmiştir. Burada gösteriliyor `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, ve `GetProductsBySupplierID`. Seçin `GetProducts` tıklayın ve açılan liste yönteminden son (eklediyseniz `DataObjectMethodAttribute` için `ProductBLL`ait önceki öğretici, bu seçenek gösterildiği gibi yöntemleri varsayılan olarak seçilir).


[![SELECT sekmesinden veriyor yöntemini seçin.](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Şekil 5**: yöntemi için veri döndürme seçin sekmesinden seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>ObjectDataSource el ile yapılandırma

ObjectDataSource'nın veri kaynağı Yapılandırma Sihirbazı'nı kullanır nesnesi belirtin ve hangi yöntemlerin nesnesinin çağrılan ilişkilendirmek için hızlı bir yolunu sunar. Ancak, özellikleri, Özellikler penceresini üzerinden veya doğrudan bildirim temelli biçimlendirmede aracılığıyla ObjectDataSource yapılandırabilirsiniz. Basitçe `TypeName` kullanılmak üzere temel nesne türü özelliğine ve `SelectMethod` için veriler alınırken çağrılacak yöntem.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Sihirbaz yalnızca Geliştirici oluşturulan sınıfları listeler gibi ObjectDataSource el ile yapılandırmak için gerektiğinde zamanlar olabilir veri kaynağı Yapılandırma Sihirbazı'nı tercih ederseniz. ObjectDataSource .NET Framework sınıf gibi bağlamak istiyorsanız [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx), kullanıcı hesabı bilgilerini erişmek için veya [Directory sınıfı](https://msdn.microsoft.com/library/system.io.directory.aspx) dosya sistemi bilgileri ile çalışmak için ObjectDataSource'nin özellikleri el ile ayarlamanız gerekir.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>2. adım: veri Web denetim ekleme ve ObjectDataSource bağlama

ObjectDataSource sayfaya eklenen ve yapılandırılmış sonra biz ObjectDataSource tarafından 's döndürülen verileri görüntülemek için sayfanın veri Web denetimleri eklemek hazır `Select` yöntemi. Herhangi bir veri Web denetimi bir ObjectDataSource bağlanabilir; GridView, DetailsView ve FormView ObjectDataSource'nın verileri görüntüleme konumundaki bakalım.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>ObjectDataSource GridView bağlama

GridView Denetim araç kutusuna ekleme `SimpleDisplay.aspx`'s tasarım yüzeyi. GridView'ın akıllı etiketten 1. adımda eklediğimiz ObjectDataSource Denetimi seçin. Bu otomatik olarak bir BoundField ObjectDataSource ait veri tarafından döndürülen her bir özellik için GridView oluşturacaktır `Select` yöntemi (yani, ürünleri DataTable tarafından tanımlanan Özellikler).


[![GridView sayfaya eklenen ve ObjectDataSource bağlı](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Şekil 6**: A GridView eklenmiştir ObjectDataSource bağlanan ve sayfa ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Özelleştirme yeniden düzenleyebilir veya Akıllı Etiket Sütunları Düzenle seçeneğini tıklatarak GridView'ın BoundFields kaldırın.


[![GridView'ın BoundFields düzenleme sütunlar iletişim kutusu üzerinden yönetme](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Şekil 7**: GridView's BoundFields aracılığıyla Düzenle sütunları Yönet iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Kaldırma GridView'ın BoundFields değiştirmek için bir dakikanızı ayırın `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` BoundFields. Yalnızca BoundField sol alt listesinden seçin ve silme (bunları kaldırmak için kırmızı X) düğmesine tıklayın. Ardından, BoundFields yeniden böylece `CategoryName` ve `SupplierName` BoundFields önünde `UnitPrice` bu BoundFields seçerek ve yukarı ok tıklatarak BoundField. Ayarlama `HeaderText` kalan BoundFields özelliklerini `Products`, `Category`, `Supplier`, ve `Price`sırasıyla. Ardından, sahip `Price` BoundField BoundField's ayarlayarak para birimi olarak biçimlendirilmiş `HtmlEncode` özelliğini false olarak ayarlayın ve kendi `DataFormatString` özelliğine `{0:c}`. Son olarak, yatay olarak Hizala `Price` sağındaki ve `Discontinued` merkezi aracılığıyla onay kutusu `ItemStyle` / `HorizontalAlign` özelliği.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![GridView'ın BoundFields özelleştirilmiş](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Şekil 8**: GridView's BoundFields özelleştirilmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Tema için tutarlı bir görünüm kullanma

Herhangi bir denetim düzeyi stil ayarını kaldırmak bu öğreticileri çabalar, bunun yerine geçişli stil sayfası kullanarak bir dış dosyada mümkün olduğunca tanımlı. `Styles.css` Dosyasını içeren `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, ve `AlternatingRowStyle` verilerin görünümünü dikte için kullanılması gereken CSS sınıfları Web bu öğreticileri kullanılan denetimler. Bunu başarmak için biz GridView's ayarlayabilirsiniz `CssClass` özelliğine `DataWebControlStyle`ve kendi `HeaderStyle`, `RowStyle`, ve `AlternatingRowStyle` özelliklerin `CssClass` özellikleri uygun şekilde.

Biz bu ayarlarsanız `CssClass` ihtiyacımız açıkça her veriler için bu özellik değerlerini ayarlamak için Web denetimi özelliklerini Web denetimi için öğreticilerimizi eklendi. GridView için DetailsView, CSS ile ilgili varsayılan özelliklerini tanımlamak için daha kolay yönetilebilir bir yaklaşım olduğundan ve bir tema kullanarak FormView denetler. Bir tema denetim düzeyi özellik ayarları, görüntüler ve sayfalar için ortak bir görünüm zorlamak için bir site genelindeki uygulanabilir CSS sınıfları koleksiyonudur.

Bizim tema herhangi bir görüntü veya CSS dosyaları içermez (stil bırakacağız `Styles.css` olarak-web uygulaması kök klasöründe tanımlanmış olan), ancak iki görünümler içerir. Bir dış Web denetimi için varsayılan özelliklerini tanımlayan bir dosyadır. Özellikle, biz görünüm dosyası GridView ve DetailsView denetimleri için varsayılan belirten sahip olacaksınız `CssClass`-ilgili özellikler.

Adlı projenize yeni bir görünüm dosyası eklemeye başlayın `GridView.skin` Çözüm Gezgini'nde proje adına sağ tıklayarak ve Yeni Öğe Ekle seçme.


[![GridView.skin adlı bir görünüm dosyası ekleme](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Şekil 9**: kaplama adlı dosya ekleme `GridView.skin` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Bir tema yerleştirilecek gerekir, bulunan dış görünüm dosyaları `App_Themes` klasör. Böyle bir klasörü henüz olduğundan, Visual Studio bizim ilk kaplama eklerken ABD oluşturmak Lütfen sunacaktır. Oluşturmak için Evet'i tıklatın `App_Theme` klasörü ve yeni yerleştirin `GridView.skin` dosya vardır.


[![Visual Studio App_Themes klasörünü oluşturmak istiyorum](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Şekil 10**: Visual Studio oluşturma izin `App_Theme` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Bu yeni bir tema oluşturacak `App_Themes` GridView kaplama dosyasıyla adlı klasörü `GridView.skin`.


![GridView tema eklendi App_Theme klasörüne sahip](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Şekil 11**: GridView tema sahip eklenmiş `App_Theme` klasörü


GridView tema için DataWebControls yeniden adlandırın (GridView klasörüne sağ tıklayın `App_Theme` klasörü ve yeniden adlandırma seçin). Ardından, aşağıdaki biçimlendirme içine girin `GridView.skin` dosyası:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Bu varsayılan özelliklerini tanımlar `CssClass`-DataWebControls Tema kullanan herhangi bir sayfayı içindeki tüm GridView için ilgili özellikler. Başka bir dış veri size kısa süre içinde kullanmaya başlayacağınız Web denetimi DetailsView için ekleyelim. Yeni bir kaplama adlı DataWebControls tema eklemek `DetailsView.skin` ve aşağıdaki biçimlendirmeyi ekleyin:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Tanımlanan bizim tema ile ASP.NET sayfamızı temayı uygulamak son adımdır. Sayfa tarafından temelinde veya bir Web sitesindeki tüm sayfalar için bir tema uygulanabilir. Bu tema Web sitesindeki tüm sayfalar için kullanalım. Bunu başarmak için aşağıdaki biçimlendirme eklemek `Web.config`'s `<system.web>` bölümü:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Tüm olan İşte bu kadar! `styleSheetTheme` Ayarını gösterir Tema içinde belirtilen özellikleri gerektiğini *değil* denetim düzeyinde belirtilen özellikler geçersiz kılar. Tema Ayarları denetim ayarlarını trump belirtmek için kullanın `theme` yerine özniteliği `styleSheetTheme`; ne yazık ki, tema ayarları Visual Studio Tasarım görünümünde görüntülenmez. Başvurmak [ASP.NET temalar ve dış genel bakış](https://msdn.microsoft.com/library/ykzx33wh.aspx) ve [sunucu tarafı stilleri kullanarak Temalar](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) temalar ve dış; daha fazla bilgi için bkz: [nasıl yapılır: ASP.NET temaları uygulamak](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) hakkında daha fazla bilgi için bir tema kullanmak için bir sayfa yapılandırma.


[![GridView ürün adı, kategori, tedarikçi, fiyat ve devam etmeyen bilgileri görüntüler](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Şekil 12**: Ürün adı, kategori, tedarikçi, fiyat ve dönüştürülmesine bilgi GridView görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Bir kayıt birer birer DetailsView'da görüntüleme

GridView, bağlı olduğu veri kaynağı denetimi tarafından döndürülen her kayıt için bir satır görüntüler. Biz aynı anda tek bir kayıt veya yalnızca bir kayıt görüntülemek istediğinizde zamanlar vardır. [DetailsView denetimi](https://msdn.microsoft.com/library/s3w1w7t4.aspx) bir HTML olarak işlemeye bu işlevselliği sunduğunu `<table>` iki sütun ve her bir sütun veya denetime bağlı özelliği için bir satır. Bir tek kayıt 90 derece ile GridView olarak DetailsView düşünebilirsiniz.

DetailsView denetimini eklemeye başlayın *yukarıda* GridView `SimpleDisplay.aspx`. Ardından, aynı ObjectDataSource denetimine GridView olarak bağlayın. GridView ile bir BoundField ObjectDataSource tarafından 's döndürülen nesnedeki her özellik için DetailsView eklenir gibi `Select` yöntemi. DetailsView'un BoundFields yatay yerine dikey olarak yerleştirilir, yalnızca farktır.


[![Sayfaya bir DetailsView ekleyin ve ObjectDataSource bağlayın](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Şekil 13**: bir DetailsView sayfasına ekleyin ve ObjectDataSource bağlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


GridView gibi DetailsView'un BoundFields ObjectDataSource tarafından döndürülen verilerin daha özelleştirilmiş bir görünümünü sağlamak için tweaked. Şekil 14 sonra kendi BoundFields DetailsView gösterir ve `CssClass` özellikleri görünümünü GridView örneğe benzer şekilde yapmak için yapılandırılmıştır.


[![Tek bir kaydını DetailsView gösterir](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Şekil 14**: tek bir kaydını DetailsView gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


DetailsView yalnızca kendi veri kaynağı tarafından döndürülen ilk kaydı görüntüler unutmayın. Tüm kayıtlar bir seferde bir adım adım kullanıcıya izin vermek için disk belleği DetailsView için etkinleştirmelisiniz. Bunu yapmak için Visual Studio'ya geri dönün ve DetailsView'un akıllı etiketinde etkinleştirmek disk belleği onay kutusunu işaretleyin.


[![Disk belleği DetailsView denetimini etkinleştir](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Şekil 15**: etkinleştirmek disk belleği DetailsView denetiminde ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Etkin disk belleği ile DetailsView ürünlerden birinin görüntülemek olanak tanır.](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Şekil 16**: disk belleği etkin DetailsView ürünlerden birinin görüntülemek izin verir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Biz gelecekte öğreticileri disk belleği hakkında daha fazla konuşun.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Aynı anda bir kaydı göstermek için daha esnek bir düzeni

DetailsView ObjectDataSource döndürülen her kaydın nasıl gösterir oldukça rigid ' dir. Biz verilerin daha esnek bir görünümünü isteyebilirsiniz. Örneğin, ürün adı, kategori, tedarikçi, fiyat ve devam etmeyen bilgi ayrı bir satırda gösteren yerine, ürün adını gösterir ve içinde fiyatı istiyoruz bir `<h4>` , görünen kategori ve tedarikçi bilgilerle başlık ad ve daha küçük bir yazı tipi boyutu fiyatına aşağıda. Ve biz değerleri yanındaki özellik adları (ürün, kategori ve benzeri) göstermek önemli değil.

[FormView denetim](https://msdn.microsoft.com/library/fyf1dk77.aspx) bu düzeyde özelleştirme sağlar. (GridView ve DetailsView gibi) alanlarını kullanarak yerine FormView Web denetimleri, statik HTML bir karışımını için izin şablonları kullanır ve [databinding sözdizimi](http://www.15seconds.com/issue/040630.htm). ASP.NET tarafından yineleyici denetimiyle tanıdık 1.x, düşündüğünüz FormView tek bir kaydı göstermek için yineleyici olarak.

FormView denetimine ekleme `SimpleDisplay.aspx` sayfanın Tasarım yüzeyi. Başlangıçta, en azından, denetimin sağlamak ihtiyacımız bize bildiren FormView gri bir blok olarak görüntüler `ItemTemplate`.


[![FormView gereken bir ItemTemplate içerir](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Şekil 17**: FormView içermelidir bir `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


FormView doğrudan bir veri kaynağı denetimi varsayılan oluşturacak FormView'ın akıllı etiket aracılığıyla bağlayabilirsiniz `ItemTemplate` otomatik olarak (ile birlikte bir `EditItemTemplate` ve `InsertItemTemplate`, ObjectDatatSource denetimin `InsertMethod` ve `UpdateMethod` özellikler ayarlanır). Ancak, bu örnek için şimdi verileri FormView bağlamak ve belirtin, `ItemTemplate` el ile. Başlangıç FormView's ayarlayarak `DataSourceID` özelliğine `ID` ObjectDataSource Denetimi `ObjectDataSource1`. Ardından, oluşturun `ItemTemplate` ürünün adını ve fiyat görüntüler böylece bir `<h4>` öğesi ve altında kategori ve taşıyıcı adları bir küçük yazı tipi boyutu.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![İlk ürün (Chai) bir özel biçiminde görüntülenir](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Şekil 18**: ilk ürün (Chai) bir özel biçiminde görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>` Databinding sözdizimi aşağıdaki gibidir. `Eval` Yöntemi FormView denetimine bağlanan geçerli nesne için belirtilen özelliğinin değerini döndürür. Out Alex Homer'ın makale onay [Basitleştirilmiş ve genişletilmiş veri bağlama sözdiziminde ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) databinding ayrıntıları hakkında daha fazla bilgi için.

DetailsView gibi FormView ObjectDataSource döndürülen ilk kaydı yalnızca gösterir. FormView aynı anda tek ürünleri üzerinden adım ziyaretçilerin izin vermek için disk belleği etkinleştirebilirsiniz.

## <a name="summary"></a>Özet

Bir ASP.NET 2. 0'ın ObjectDataSource Denetimi sayesinde kod satırı yazmadan erişme ve verileri bir iş mantığı katmanından görüntüleme gerçekleştirilebilir. ObjectDataSource sınıfının belirtilen bir yöntemi çağırır ve sonuçları döndürür. Bu sonuç, veri ObjectDataSource bağlı Web denetimi görüntülenebilir. Bu öğreticide ObjectDataSource GridView, DetailsView ve FormView denetimleri bağlama sırasında arama.

Şu ana kadar ObjectDataSource parametresiz yöntemini çağırmak için nasıl kullanılacağını yalnızca gördük, ancak ne bekliyor bir yöntemi çağırmak istiyoruz giriş gibi parametreleri, `ProductBLL` sınıfının `GetProductsByCategoryID(categoryID)`? Bir veya daha fazla parametre bekliyor bir yöntemi çağırmak için Biz bu parametreler için değerler belirtmek üzere ObjectDataSource yapılandırmanız gerekir. Bunu gerçekleştirmek nasıl göreceğiz bizim [sonraki öğretici](declarative-parameters-vb.md).

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kendi veri kaynağı denetimleri oluşturma](https://msdn.microsoft.com/library/ms364049.aspx)
- [ASP.NET 2.0 GridView örnekleri](https://msdn.microsoft.com/library/aa479339.aspx)
- [Basitleştirilmiş ve genişletilmiş veri ASP.NET 2.0 sözdizimi bağlama](http://www.15seconds.com/issue/040630.htm)
- [ASP.NET 2.0 temalar](http://www.odetocode.com/Articles/423.aspx)
- [Temalar kullanarak sunucu tarafı stiller](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Nasıl yapılır: ASP.NET temaları program aracılığıyla uygulama](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [sonraki](declarative-parameters-vb.md)
