---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: "Güncelleştirme ve silme Varolan ikili veriler (VB) | Microsoft Docs"
author: rick-anderson
description: "Önceki eğitimlerine nasıl GridView denetiminin düzenlemek ve metin verilerini silmek basit kolaylaştırır gördük. Bu öğreticide nasıl GridView denetimi de hale bakın …"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: e14b19f99e9f41c5a296d73ba689095a686794db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Güncelleştirme ve silme Varolan ikili veriler (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) veya [PDF indirin](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> Önceki eğitimlerine nasıl GridView denetiminin düzenlemek ve metin verilerini silmek basit kolaylaştırır gördük. Bu öğreticide nasıl GridView denetim Ayrıca, düzenlemek ve bu ikili veriler veritabanına kaydedilir ya da dosya sisteminde saklanan ikili veri silmek mümkün kılar bakın.


## <a name="introduction"></a>Giriş

Son üç öğreticileri üzerinden biz ullanıcı eklenen oldukça bit ikili verilerle çalışmak için işlevselliği. Ekleyerek başlatılan bir `BrochurePath` sütuna `Categories` tablo ve mimari buna göre güncelleştirilir. Ayrıca kategorileri s tablosunda var olan iş için veri erişim katmanı ve iş mantığı katmanı yöntemlerini eklediğimiz `Picture` bir görüntü dosyasının ikili içerik s içeren sütun. Web sayfaları gösterilen kategori s Resimli Broşürü için indirme bağlantısı GridView ikili verileri sunmak için oluşturduğumuz bir `<img>` öğesi ve yeni bir kategori ekleyin ve onun Broşürü ve resim veri yükleme yapmalarına izin vermek için bir DetailsView eklediniz.

Uygulanacak kalan tek şey düzenleme ve GridView s yerleşik düzenleme kullanarak ve özellikleri silinmesi Bu öğreticide gerçekleştirmek mevcut kategorileri silme olanağı. Bir kategori düzenlerken, kullanıcının isteğe bağlı olarak yeni bir resim karşıya yükleyebilir veya varolan bir kullanmaya devam kategorilerini mümkün olacaktır. Broşürü için bunlar ya da varolan Broşürü yeni Broşürü karşıya yüklemek veya kategorisi artık kendisiyle ilişkili bir Broşürü olduğunu belirtmek için kullanmayı da seçebilirsiniz. Let s başlayın!

## <a name="step-1-updating-the-data-access-layer"></a>1. adım: veri erişim katmanı güncelleştiriliyor

DAL otomatik olarak oluşturulan `Insert`, `Update`, ve `Delete` yöntemleri, ancak bu yöntemleri oluşturuldu göre `CategoriesTableAdapter` içermemesi s ana sorgu `Picture` sütun. Bu nedenle, `Insert` ve `Update` yöntemleri kategori s resmi için ikili verileri belirtmek için parametre içermiyor. Biz olduğu gibi [önceki öğretici](including-a-file-upload-option-when-adding-a-new-record-vb.md), güncelleştirme için yeni bir TableAdapter yöntemi oluşturmak ihtiyacımız `Categories` ikili veri belirtirken tablo.

Yazılan veri kümesi'ni açın ve Tasarımcısı'ndan sağ tıklayın `CategoriesTableAdapter` s üst bilgisi ve Sorgu Ekle bağlam menüsünden launche için TableAdapter sorgu Yapılandırma Sihirbazı'nı seçin. Bu sihirbaz, bize nasıl TableAdapter sorgu veritabanına erişmek için kullanması isteyerek başlar. Use SQL deyimleri seçin ve İleri'yi tıklatın. Sonraki adım sorgu türü için oluşturulacak ister. Size yeni bir kayıt eklemek için bir sorgu oluşturma re itibaren `Categories` tablo, GÜNCELLEŞTİRMEYİ seçin ve İleri'yi tıklatın.


[![GÜNCELLEŞTİRME seçeneğini belirleyin](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Şekil 1**: güncelleştirme seçeneğini belirleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Şimdi belirtmek ihtiyacımız `UPDATE` SQL deyimi. Sihirbaz otomatik olarak öneren bir `UPDATE` TableAdapter s ana sorguya karşılık gelen deyimi (güncelleştiren bir `CategoryName`, `Description`, ve `BrochurePath` değerleri). Deyimi değiştirin böylece `Picture` sütundur ile birlikte gelen bir `@Picture` parametresi şu şekilde:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Sihirbazın son ekranında bize yeni TableAdapter yöntem adı ister. Girin `UpdateWithPicture` ve Son'u tıklatın.


[![Yeni TableAdapter yöntemi UpdateWithPicture adı](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Şekil 2**: yeni TableAdapter yöntem adı `UpdateWithPicture` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>2. adım: iş mantığı katmanı yöntemler ekleme

DAL güncelleştirmeye ek olarak, biz güncelleştirme ve bir kategori silme yöntemleri dahil etmek için BLL güncelleştirmeniz gerekir. Bunlar sunu katmanı çağrılacak yöntemleridir.

Bir kategori silmek için biz kullanabilirsiniz `CategoriesTableAdapter` otomatik olarak oluşturulan s `Delete` yöntemi. Aşağıdaki yöntemi ekleyin `CategoriesBLL` sınıfı:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Bu öğretici için let s oluşturduğunuz bir kategori - ikili resim verilerinin bekler ve çağıran bir güncelleştirme için iki yöntem `UpdateWithPicture` yöntemi yalnızca eklediğimiz için `CategoriesTableAdapter` ve diğeri de kabul yalnızca `CategoryName`, `Description`ve `BrochurePath`değerleri ve kullandığı `CategoriesTableAdapter` otomatik olarak oluşturulan s sınıfı `Update` deyimi. İki yöntemi kullanarak arkasında stratejinin yeni resim karşıya durumda kullanıcı gerekir, diğer alanları, birlikte kategori s resmi güncelleştirmek bazı durumlarda, bir kullanıcının isteyebileceği ' dir. Karşıya yüklenen resim s ikili veriler, ardından kullanılabilir `UPDATE` deyimi. Diğer durumlarda, kullanıcı yalnızca güncelleştirme, söyleyin, ad ve açıklama ilgisini. Ancak `UPDATE` deyimi bekliyor ikili veriler için `Picture` de sütun ardından size d bu bilgileri de sağlamanız gereken. Bu resim verileri düzenlenmekte kayıt için geri getirmek için veritabanına ek bir seyahat gerektirir. Bu nedenle, biz iki istediğiniz `UPDATE` yöntemleri. İş mantığı katmanı resim veri kategorisi güncelleştirilirken sağlanan üzerinde hangisinin kullanılacağını göre belirler.

Bu kolaylaştırmak için iki yöntem ekleme `CategoriesBLL` sınıfı, hem adlı `UpdateCategory`. İlk üç kabul etmelidir `String` s, bir `Byte` diziye ve bir `Integer` giriş olarak parametreleri; ikinci yalnızca üç `String` s ve bir `Integer`. `String` Giriş parametreleri: s kategori adı, açıklama ve Broşürü dosya yolu, `Byte` dizidir kategori s resmi ikili içeriğini ve `Integer` tanımlayan `CategoryID` güncelleştirmek için kayıt. İlk aşırı yükleme geçilen ikinci if çağırır bildirimi `Byte` dizisi `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>3. adım: ekleme ve görünümü işlevi kopyalama

İçinde [önceki öğretici](including-a-file-upload-option-when-adding-a-new-record-vb.md) adlı bir sayfaya oluşturduğumuz `UploadInDetailsView.aspx` tüm kategorileri GridView içinde listelenen ve sisteme yeni kategoriler eklemek için bir DetailsView sağlanan. Bu öğreticide biz GridView düzenleme ve silme desteği içerecek şekilde genişletir. Gelen çalışmaya devam yerine `UploadInDetailsView.aspx`, let s yerine koyun Bu öğretici s değişiklikleri `UpdatingAndDeleting.aspx` aynı klasöre sayfasından `~/BinaryData`. Kopyalama ve bildirim temelli biçimlendirme yapıştırın ve gelen kod `UploadInDetailsView.aspx` için `UpdatingAndDeleting.aspx`.

Başlangıç açarak `UploadInDetailsView.aspx` sayfası. Tüm bildirim temelli söz dizimi içinde kopyalayın `<asp:Content>` Şekil 3'te gösterildiği gibi öğesi. Ardından, açık `UpdatingAndDeleting.aspx` ve bu biçimlendirmesi içinde yapıştırma kendi `<asp:Content>` öğesi. Benzer şekilde, koddan kopyalama `UploadInDetailsView.aspx` sayfasında s arka plandaki kod sınıfı `UpdatingAndDeleting.aspx`.


[![Kopya UploadInDetailsView.aspx bildirim temelli biçimlendirmeden](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Şekil 3**: bildirim temelli biçimlendirmeden kopyalama `UploadInDetailsView.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Bildirim temelli biçimlendirme ve kodun kopyaladıktan sonra ziyaret `UpdatingAndDeleting.aspx`. Aynı çıktı ve aynı kullanıcı deneyimini sahip görmelisiniz olduğu gibi `UploadInDetailsView.aspx` önceki öğretici sayfasından.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>4. adım: Destek ObjectDataSource ve GridView silme ekleme

Biz geri anlatıldığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) öğretici, GridView yerleşik silme yeteneği sağlar ve bu özellikler bir onay kutusu değer çizgilerinin varsa etkinleştirilebilir temel kılavuz s veri kaynağı silme destekler. Şu anda ObjectDataSource GridView bağlı (`CategoriesDataSource`) silme desteklemiyor.

Bu sorunu gidermek için Sihirbazı'nı başlatmak için veri kaynağını yapılandırma seçeneğinden üzerinde ObjectDataSource s akıllı etiket'ı tıklatın. ObjectDataSource çalışmak üzere yapılandırıldığı ilk ekran gösterilmektedir `CategoriesBLL` sınıfı. Sonraki ulaştı. Şu anda yalnızca ObjectDataSource s `InsertMethod` ve `SelectMethod` özellikleri belirtilir. Ancak, sihirbaz otomatik güncelleştirme ve silme sekmelerle açılan listelerde doldurulan `UpdateCategory` ve `DeleteCategory` yöntemleri, sırasıyla. Bunun nedeni, içinde `CategoriesBLL` Biz bu yöntemleri kullanarak işaretlenmiş sınıfı `DataObjectMethodAttribute` güncelleştirme ve silme için varsayılan yöntemi olarak.

Şimdilik, güncelleştirme sekmesini s aşağı açılan listede (hiçbiri) olarak ayarlayın ancak kümesine silme sekmesi s aşağı açılan listesi bırakın `DeleteCategory`. Bu sihirbaz güncelleştirme desteği eklemek için adım 6 için getireceğiz.


[![ObjectDataSource DeleteCategory yöntemi kullanmak üzere yapılandırma](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `DeleteCategory` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Sihirbazı tamamladığınızda, Visual Studio alanları Yenile'yi ve anahtarları istiyorsanız, Web verileri yeniden oluşturacak alanları denetimleri isteyebilir. Evet'i seçerseniz, alan özelleştirmeler yapmış olabileceğiniz üzerine yazar için Hayır'ı seçin.


ObjectDataSource şimdi için bir değer içerir, `DeleteMethod` özelliği yanı sıra bir `DeleteParameter`. Geri Çağırma yöntemlerini belirtmek için Sihirbazı'nı kullanarak Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğine `original_{0}`, hangi güncelleştirme sorunlara neden olur ve yöntem çağrılarına silin. Bu nedenle, bu özelliği tamamen temizleyin veya Varsayılana Sıfırla `{0}`. Bu ObjectDataSource özellikte belleğinizi yenilemek gerekirse bkz [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) Öğreticisi.

Sihirbazı tamamlama ve düzelttikten sonra `OldValuesParameterFormatString`, ObjectDataSource s bildirim temelli biçimlendirme şuna benzer görünmelidir:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

ObjectDataSource yapılandırdıktan sonra GridView s akıllı etiketinden Enable Deleting onay kutusunu işaretleyerek GridView silme özellikleri ekleyin. Bu bir CommandField GridView ekler, `ShowDeleteButton` özelliği ayarlanmış `True`.


[![GridView silmek için desteği etkinleştir](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Şekil 5**: GridView silme için desteği etkinleştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Out Sil işlevselliğini test etmek için bir dakikanızı ayırın. Bir yabancı anahtar arasındaki `Products` s tablosu `CategoryID` ve `Categories` s tablosu `CategoryID`, ilk sekiz kategorilerden herhangi biri silme girişimi olursa bir yabancı anahtar kısıtlaması ihlali özel durumu alır. Bu işlevselliği kullanıma test etmek için bir Broşürü ve resim sağlayan yeni bir kategori ekleyin. Şekil 6'da gösterilen my test kategorisi adında bir test Broşürü dosya içerir `Test.pdf` ve test resim. Test kategorisi eklendikten sonra Şekil 7 GridView gösterir.


[![Test kategori Broşürü ve görüntü ekleme](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Şekil 6**: broşür ve görüntü ile bir Test kategoriye Ekle ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Test kategorisi ekledikten sonra onu GridView görüntülenir](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Şekil 7**: Test kategorisi ekledikten sonra onu GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


Visual Studio'da Çözüm Gezgini'ne yenileyin. Yeni bir dosyada görmelisiniz `~/Brochures` klasörünü `Test.pdf` (bkz. Şekil 8).

Ardından, Test kategorisi satırda geri gönderme sayfası neden Sil bağlantısını tıklayın ve `CategoriesBLL` s sınıfı `DeleteCategory` tetiklenecek yöntemi. Bu DAL s çağıracağı `Delete` yöntemi, uygun neden `DELETE` veritabanına gönderilmek üzere deyimi. Veri sonra GridView DataSet'e ve biçimlendirme artık mevcut Test kategorisiyle istemciye geri gönderilir.

Silme iş akışı başarıyla kaldırıldı Test kategorisi kaydından sırada `Categories` tablo, web sunucusu s dosya sisteminden Broşürü dosya kaldırmadı. Çözüm Gezgini yenileyin ve göreceksiniz `Test.pdf` hala durduğunu `~/Brochures` klasör.


![Web sunucusu s dosya sisteminden Test.pdf dosyası silinmedi](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Şekil 8**: `Test.pdf` dosyası değil Web sunucusu s dosya sisteminden silindi


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>5. adım: silinen kategori s Broşürü dosya kaldırılıyor

İkili veriler veritabanına dış depolama olumsuzlukları ilişkili veritabanı kaydı silindiğinde, bu dosyaları temizlemek için ek adımlar izlenmelidir biridir. GridView ve ObjectDataSource önce hem delete komutu gerçekleştirildikten sonra yangın olayları sağlar. Biz gerçekte öncesi ve sonrası eylemi olayları için olay işleyicileri oluşturmanız gerekir. Önce `Categories` kaydı silinir, PDF dosyası s yolunu belirlemek ihtiyacımız ancak biz t kategori bazı istisna vardır ve kategori silinmemiş durumda silinmeden önce PDF silmek istediğinizden güncelleştireceğinizi.

GridView s [ `RowDeleting` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) ateşlenir ObjectDataSource s delete komutu çağrılmış önce while kendi [ `RowDeleted` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) sonra ateşlenir. Aşağıdaki kodu kullanarak bu iki olayları için olay işleyicileri oluşturun:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

İçinde `RowDeleting` olay işleyicisi `CategoryID` satırının silinmesini GridView s'den gerçekleşti `DataKeys` bu olay işleyici üzerinden erişilebilen koleksiyonu `e.Keys` koleksiyonu. Ardından, `CategoriesBLL` s sınıfı `GetCategoryByCategoryID(categoryID)` silinmesini kaydı hakkında bilgi döndürmek için çağrılır. Varsa döndürülen `CategoriesDataRow` nesne sahip olmayan bir`NULL``BrochurePath` sayfa değişkende depolanır sonra değer `deletedCategorysPdfPath` böylece dosyayı silinebilmesi için `RowDeleted` olay işleyicisi.

> [!NOTE]
> Alma yerine `BrochurePath` ayrıntıları `Categories` içinde silinen kayıt `RowDeleting` olay işleyicisi alternatif olarak eklediğimiz `BrochurePath` GridView s `DataKeyNames` özelliği ve kayıt s değeri erişilebilir aracılığıyla `e.Keys` koleksiyonu. Bunun yapılması biraz GridView s görünüm durumu boyutunu artırabilirsiniz ancak gereken kod miktarını azaltmak ve bir seyahat veritabanına kaydedin.


S temel delete komutu çağrılan, ObjectDataSource GridView s sonra `RowDeleted` olay işleyicisi etkinleşir. Verileri silme hiçbir özel durum oluştu ve için bir değer yoksa `deletedCategorysPdfPath`, sonra da PDF dosya sisteminden silindi. Bu ek kod kendi resimle ilişkili kategori s ikili verileri temizlemek için gerekli olmayan unutmayın. Bu s resim verileri doğrudan veritabanında depolandığından, bu nedenle silme `Categories` satır da bu kategori s resim verileri siler.

İki olay işleyicileri eklendikten sonra bu test çalışması yeniden çalıştırın. Kategori silerken ilişkili PDF de silinir.

Varolan bir kayıt s ilişkili ikili verileri güncelleştirme ilginç bazı zorluklar sağlar. Bu öğreticinin geri kalanında Broşürü ve resim güncelleştirme yetenekleri ekleme içine ilgili alır. 6. adım adım 7 resmi güncelleştirme sırasında ararken Broşürü bilgileri güncelleştirmek için teknikleri inceler.

## <a name="step-6-updating-a-category-s-brochure"></a>6. adım: bir kategori s Broşürü güncelleştiriliyor

GridView, bir genel bakış ekleme, güncelleştirme ve silme veri öğreticide açıklandığı gibi veri kaynağındaki uygun şekilde yapılandırılmışsa, bir onay kutusu değer çizgilerinin tarafından uygulanan yerleşik satır düzeyi düzenleme destek sunar. Şu anda `CategoriesDataSource` ObjectDataSource henüz yapılandırılmamış let s eklemek için güncelleştirme desteği, dahil etmek için.

ObjectDataSource s Sihirbazı'nı yapılandırma veri kaynağı bağlantısına tıklayın ve ikinci adıma geçebilirsiniz. Nedeniyle `DataObjectMethodAttribute` kullanılan `CategoriesBLL`, güncelleştirme aşağı açılan listesi ile otomatik olarak doldurulur `UpdateCategory` dört giriş parametreleri kabul aşırı yükleme (tüm sütunlar için ancak `Picture`). Bu beş parametrelerle aşırı kullanır şekilde değiştirin.


[![ObjectDataSource resmi için bir parametre içeren UpdateCategory yöntemi kullanmak üzere yapılandırma](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `UpdateCategory` için bir parametre içeren yöntem `Picture` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource şimdi için bir değer içerir, `UpdateMethod` özelliği yanı sıra karşılık gelen `UpdateParameter` s. Adım 4'te belirtildiği gibi Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğine `original_{0}` veri kaynağı Yapılandırma Sihirbazı'nı kullanırken. Bu güncelleştirme sorunlara neden ve yöntem çağrılarına silin. Bu nedenle, bu özelliği tamamen temizleyin veya Varsayılana Sıfırla `{0}`.

Sihirbazı tamamlama ve düzelttikten sonra `OldValuesParameterFormatString`, ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

GridView s yerleşik düzenleme özellikleri açın GridView s akıllı etiket düzenlemeyi etkinleştir seçeneğini denetleyin. Bu CommandField s ayarlar `ShowEditButton` özelliğine `True`, sonuçta elde edilen ayrıca bir Düzenle düğmesi (ve güncelleştirme ve İptal düğmeleri düzenlenmekte olan satır için).


[![Destek düzenlemeye GridView yapılandırın](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Şekil 10**: Destek düzenlemeye GridView yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve satır s Düzenle düğmeleri birine tıklayın. `CategoryName` Ve `Description` BoundFields metin kutuları işlenir. `BrochurePath` TemplateField eksik bir `EditItemTemplate`, göstermeye devam eder, `ItemTemplate` Broşürü bağlantı. `Picture` ImageField işler TextBox olarak özelliği `Text` özelliği ImageField s değerini atanan `DataImageUrlField` değeri, bu durumda `CategoryID`.


[![GridView düzenleme bir arabirim için BrochurePath eksik.](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Şekil 11**: bir düzenleme arabirimi GridView eksik `BrochurePath` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Özelleştirme`BrochurePath`s düzenleme arabirimi

Düzenleme arabirim oluşturmak ihtiyacımız `BrochurePath` TemplateField, bir kullanıcı ya da sağlar:

- Kategori s Broşürü olarak bırakın-olan
- Yeni bir Broşürü yükleyerek kategori s Broşürü güncelleştirmek veya
- Kategori s Broşürü (kategorisi artık ilişkili Broşürü sahip durumda) tamamen kaldırın.

Ayrıca güncelleştirmek ihtiyacımız `Picture` ImageField s düzenleme arabirimi, ancak alacaksınız için bunu adım 7'de.

GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve seçin `BrochurePath` TemplateField s `EditItemTemplate` aşağı açılan listeden. Bu şablona ayarlama, RadioButtonList Web denetim ekleme kendi `ID` özelliğine `BrochureOptions` ve kendi `AutoPostBack` özelliğine `True`. Özellikler penceresinden içinde üç noktaya tıklayın `Items` getirir özelliği `ListItem` Koleksiyonu Düzenleyicisi. Aşağıdaki üç seçenekle eklemek `Value` s 1, 2 ve 3, sırasıyla:

- Geçerli Broşürü kullanın
- Geçerli Broşürü Kaldır
- Yeni Broşürü karşıya yükle

İlk kümesinden `ListItem` s `Selected` özelliğine `True`.


![Üç ListItems RadioButtonList ekleme](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Şekil 12**: üç eklemek `ListItem` RadioButtonList s


Adlı bir dosya yükleme denetimi RadioButtonList ekleyin `BrochureUpload`. Ayarlama, `Visible` özelliğine `False`.


[![Bir RadioButtonList ve dosya yükleme denetimi EditItemTemplate ekleyin](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Şekil 13**: RadioButtonList ve dosya yükleme denetimine ekleme `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Bu RadioButtonList kullanıcı için üç seçenek sunar. Son seçenek, yeni Broşürü karşıya yükleme, yalnızca seçili dosya yükleme denetimi görüntülendiğinden emin olur. Bunu gerçekleştirmek için RadioButtonList s için bir olay işleyicisi oluşturun `SelectedIndexChanged` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

RadioButtonList ve dosya yükleme denetimleri bir şablonu içinde olduğundan, bir bit bu denetimlerini programlı olarak erişmek için kod yazmanız gerekir. `SelectedIndexChanged` Olay işleyicisi başvuru RadioButtonList olarak geçirilen `sender` giriş parametresi. Dosya yükleme denetimi almak için Denetim ve kullanım RadioButtonList s üst almak ihtiyacımız `FindControl("controlID")` buradan yöntemi. Dosya yükleme RadioButtonList ve dosya yükleme denetimleri başvuru sahibiz sonra s kontrol `Visible` özelliği ayarlanmış `True` yalnızca RadioButtonList s `SelectedValue` olduğu 3, eşittir `Value` karşıya yükleme yeni Broşürü için `ListItem`.

Yerinde bu kodu ile düzenleme arabirimin dışına test etmek için bir dakikanızı ayırın. Bir satır için Düzenle düğmesini tıklatın. Başlangıçta, geçerli Broşürü kullan seçeneği seçili olmalıdır. Seçilen dizin değiştirme geri gönderimin neden olur. Üçüncü seçenek belirlenirse, dosya yükleme denetim görüntülenir, aksi halde gizlenir. Düzenle düğmesi ilk tıklatıldığında Şekil 14 düzenleme arabirimi gösterir; Karşıya yükleme yeni Broşürü seçeneği seçtikten sonra Şekil 15 arabirimi gösterir.


[![Seçeneği seçili başlangıçta, kullanım geçerli Broşürü](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Şekil 14**: seçeneği belirlendiğinde başlangıçta, kullanım geçerli Broşürü ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Karşıya yükleme yeni Broşürü seçeneği görüntüler dosya yükleme denetim seçme](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Şekil 15**: karşıya yükleme yeni Broşürü seçeneği görüntüler dosya yükleme denetim seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Broşürü kaydetme dosya ve güncelleştirme`BrochurePath`sütun

GridView s güncelleştirme düğmesine tıklandığında, kendi `RowUpdating` olay etkinleşir. S güncelleştirme komutu çağrıldığında ObjectDataSource ve GridView s `RowUpdated` olay etkinleşir. Olay işleyicileri hem de bu olayları oluşturmak ihtiyacımız silme iş akışı ile gibi. İçinde `RowUpdating` olay işleyicisi ihtiyacımız temel hangi eylemin yapılacağını belirlemek `SelectedValue` , `BrochureOptions` RadioButtonList:

- Varsa `SelectedValue` 1, aynı kullanmaya devam etmek istiyoruz `BrochurePath` ayarı. Bu nedenle, ObjectDataSource s ayarlamak ihtiyacımız `brochurePath` varolan parametresi `BrochurePath` güncelleştirilmekte kaydın değeri. ObjectDataSource s `brochurePath` parametresi kullanılarak ayarlanabilir `e.NewValues["brochurePath"] = value`.
- Varsa `SelectedValue` 2 sonra s kayıt kümesinin istiyoruz `BrochurePath` değeri `NULL`. Bu ObjectDataSource s ayarlayarak gerçekleştirilebilir `brochurePath` parametresi `Nothing`, bir veritabanında sonuçları `NULL` kullanılmasını `UPDATE` deyimi. Kaldırılmakta olan mevcut bir Broşürü dosya varsa, var olan dosyayı silin gerekir. Ancak, yalnızca bir özel durum yükseltmeden Güncelleştirme tamamlandıktan, bunu yapmak istiyoruz.
- Varsa `SelectedValue` 3, biz kullanıcı karşıya bir PDF dosyası emin olun ve ardından dosya sistemine kaydetme ve kaydını s güncelleştirmek istiyorsanız, `BrochurePath` sütun değeri. Ayrıca, değiştirilmekte olan mevcut bir Broşürü dosyası ise, biz önceki dosya silmeniz gerekir. Ancak, yalnızca bir özel durum yükseltmeden Güncelleştirme tamamlandıktan, bunu yapmak istiyoruz.

Ne zaman tamamlanması için gerekli olan adımları RadioButtonList s `SelectedValue` olan 3 DetailsView s tarafından kullanılanlarla aynı neredeyse `ItemInserting` olay işleyicisi. Yeni bir kategori kayıt eklediğimiz içinde DetailsView denetiminden eklendiğinde bu olay işleyicisi yürütülür [önceki Öğreticisi](including-a-file-upload-option-when-adding-a-new-record-vb.md). Bu nedenle, onu işlevselliklere ayrı yöntemlerde yeniden düzenlemeniz için bize behooves. Özellikle, ı ortak işlevsellik iki yöntemlerin içine taşındı:

- `ProcessBrochureUpload(FileUpload, out bool)`Giriş olarak bir dosya yükleme denetim örneği ve olup silme veya düzenleme işlemi devam etmemelisiniz veya bazı doğrulama hatası nedeniyle iptal olması gerektiğini olmadığını belirten bir çıkış Boole değeri kabul eder. Bu yöntem kaydedilmiş dosyanın yolunu döndürür veya `null` hiçbir dosyasını kaydettiyseniz.
- `DeleteRememberedBrochurePath`Sayfa değişkenini yolda tarafından belirtilen dosyayı siler `deletedCategorysPdfPath` varsa `deletedCategorysPdfPath` değil `null`.

Bu iki yöntem için kod izler. Arasında benzerlik Not `ProcessBrochureUpload` ve DetailsView s `ItemInserting` önceki öğreticiden olay işleyicisi. Bu öğreticide t bu yeni yöntemlerini kullanmayı DetailsView s olay işleyicileri güncelleştirdiniz. DetailsView s olay işleyicileri için yapılan değişiklikleri görmek için Bu öğretici ile ilişkili kodu indirin.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s `RowUpdating` ve `RowUpdated` olay işleyicilerini kullanmak `ProcessBrochureUpload` ve `DeleteRememberedBrochurePath` aşağıdaki gösterildiği gibi kod yöntemleri:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Not nasıl `RowUpdating` olay işleyicisi göre uygun eylemi gerçekleştirmek için bir dizi koşullu deyimler kullanan `BrochureOptions` RadioButtonList s `SelectedValue` özellik değeri.

Yerinde bu kodu ile bir kategori düzenleyin ve sahip, geçerli Broşürü kullanın, hiçbir Broşürü kullanın veya yeni bir tane karşıya yükleyin. Bir tane deneyin. Kesme noktaları ayarlayın `RowUpdating` ve `RowUpdated` iş akışının bir fikir edinmek için olay işleyicileri.

## <a name="step-7-uploading-a-new-picture"></a>7. adım: yeni bir resim karşıya yükleme

`Picture` Değerinden doldurulan bir metin olarak arabirimi işler düzenleme ImageField s kendi `DataImageUrlField` özelliği. Düzenleme iş akışı sırasında GridView parametre ObjectDataSource s adlı parametreyi içeren ImageField s değerini geçirir `DataImageUrlField` özelliği ve parametre s değeri metin düzenleme arabirimi kutusuna girilen değer. Resmin dosya sisteminde dosya olarak kaydedildiğinde bu davranış uygundur ve `DataImageUrlField` görüntünün tam URL'sini içerir. Bu koşullara ile düzenleme arabirimi kullanıcı değiştirebilirsiniz ve veritabanına geri kaydetmiş metin kutusuna görüntü s URL'yi görüntüler. Bu varsayılan arabirimi içermiyor t izin verilen, yeni bir görüntü karşıya yüklemek kullanıcı, ancak bunları görüntünün URL'si geçerli değerinden değiştirmek izin vermez. Bu öğreticide, ancak ImageField s varsayılan arabirimi düzenleme için yeterli değil `Picture` ikili veriler doğrudan veritabanında depolanıyor ve `DataImageUrlField` özelliği ayrı tutma yalnızca `CategoryID`.

Bir kullanıcı bir ImageField sahip bir satır düzenler ne olur bizim öğreticide daha iyi anlamak için aşağıdaki örneği göz önünde bulundurun: bir kullanıcı bir satırla düzenler `CategoryID` neden 10 `Picture` 10 değeri ile textbox olarak işlemek için ImageField. Kullanıcı bu textbox değeri 50 olarak değiştirir ve Güncelleştir düğmesini tıklattığında düşünün. Geri gönderimin oluştuğu ve GridView başlangıçta adlı bir parametre oluşturur `CategoryID` 50 değerine sahip. Ancak, bu parametre GridView göndermeden önce (ve `CategoryName` ve `Description` parametreleri), gelen değerler ekler `DataKeys` koleksiyonu. Bu nedenle, bu raporun üzerine `CategoryID` geçerli arka plandaki satır s ile parametresi `CategoryID` 10 değeri. Kısacası, arabirimini düzenleme ImageField s herhangi bir etkisi düzenleme iş akışını Bu öğretici için sahip olduğundan ImageField s adlarını `DataImageUrlField` özelliği ve kılavuz s `DataKey` değeri olan bir aynı.

ImageField veritabanı verilerine dayalı bir görüntüyü kolaylaştırır, ancak biz t düzenleme arabiriminde textbox sağlamak istiyorsanız güncelleştireceğinizi. Bunun yerine, son kullanıcı kategorisi s resmi değiştirmek için kullanabileceğiniz bir dosya yükleme denetimi sunmaya istiyoruz. Aksine `BrochurePath` değeri, bu öğreticileri için biz ullanıcı karar gerektiren her kategoride bir resim olmalıdır. Bu nedenle, biz t ilişkili resim kullanıcı olabilir ya da karşıya yükleme yeni bir resim olduğunu belirtmek veya geçerli resmi olarak bırakın kullanıcı izin gerek güncelleştireceğinizi-değil.

ImageField s düzenleme arabirimini özelleştirmek için şu TemplateField'a dönüştürmeniz gerekir. GridView s akıllı etiket sütunları Düzenle bağlantısına tıklayın, ImageField seçin ve bu alan dönüştürme TemplateField bağlantıya tıklayın.


![ImageField TemplateField'a Dönüştür](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Şekil 16**: ImageField TemplateField'a Dönüştür


Bu şekilde TemplateField ImageField dönüştürme TemplateField iki şablonlarıyla oluşturur. Aşağıdaki bildirim temelli söz dizimini gösterildiği gibi `ItemTemplate` içeren bir görüntü Web özelliği kontrol `ImageUrl` özelliği ImageField s tabanlı databinding sözdizimini kullanarak atandığında `DataImageUrlField` ve `DataImageUrlFormatString` özellikleri. `EditItemTemplate` Metin kutusu içeren, `Text` özelliği tarafından belirtilen değere bağlı `DataImageUrlField` özelliği.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Güncelleştirmek ihtiyacımız `EditItemTemplate` dosya yükleme denetimi kullanmak için. S akıllı etiket Düzenle şablonlarındaki tıklatın GridView bağlantı ve ardından `Picture` TemplateField s `EditItemTemplate` aşağı açılan listeden. Şablonda bu kaldırmak metin kutusu görmeniz gerekir. Ardından, bir dosya yükleme denetimi ayarı şablonuna Araç Kutusu'ndan sürükleyin kendi `ID` için `PictureUpload`. Ayrıca kategori s resmi değiştirmek için yeni bir resim belirtin metin ekleyin. Kategori s resmi aynı tutmak için alan şablonu için de boş bırakın.


[![EditItemTemplate dosya yükleme denetim ekleme](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Şekil 17**: dosya yükleme denetimine ekleme `EditItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Düzenleme arabirimi özelleştirdikten sonra ilerleme durumunuzu bir tarayıcıda görüntülemek. Bir satır salt okunur modda görüntülerken, kategori s görüntü önceki ancak Düzenle düğmesini tıklatarak resim sütunu dosya yükleme denetimi ile metin olarak işleyen olarak gösterilir.


[![Bir dosya yükleme denetimi düzenleme arabirimi içerir](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Şekil 18**: bir dosya yükleme denetimi düzenleme arabirimi içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


ObjectDataSource çağırmak için yapılandırıldığını geri çağırma `CategoriesBLL` s sınıfı `UpdateCategory` resmi olarak için ikili verileri girdi olarak kabul yöntemi bir `Byte` dizi. Bu dizi ise `Nothing`, ancak, diğer `UpdateCategory` aşırı çağrılır, hangi sorunların `UPDATE` değişiklik SQL deyimini `Picture` sütun, böylece s kategori geçerli bırakarak resim bozulmadan. Bu nedenle, GridView s içinde `RowUpdating` ihtiyacımız programlı olarak başvurmak için olay işleyicisini `PictureUpload` dosya yükleme denetlemek ve bir dosyayı karşıya yüklenen belirleyin. Biri karşıya sonra bunu yapmadan *değil* için bir değer belirtmek için `picture` parametresi. Bir dosyayı karşıya yüklenen diğer yandan, `PictureUpload` dosya yükleme denetimi istiyoruz JPG dosyası olduğundan emin olmak. Bunun ardından ikili içeriğini ObjectDataSource için gönderebiliriz `picture` parametresi.

Adım 6'da kullanılan koduyla burada zaten gereken kod çoğunu DetailsView s var gibi `ItemInserting` olay işleyicisi. Bu nedenle, g ve yeni bir yöntemi, ortak işlevsellik bulunanad `ValidPictureUpload`ve güncelleştirilmiş `ItemInserting` bu yöntemi kullanmak için olay işleyicisi.

GridView s başlangıç için aşağıdaki kodu ekleyin `RowUpdating` olay işleyicisi. Bu geçersiz resim dosyası karşıya yüklediyseniz Broşürü web sunucusu s dosya sistemine kaydetmek istediğinizden Bu kod size t güncelleştireceğinizi beri Broşürü dosyası kaydeder kodu önce gelen önemlidir.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` Yöntemi, tek bir giriş parametresi olarak bir dosya yükleme denetimindeki alır ve yüklenen dosya JPG olduğundan emin olmak için yüklenen dosya s uzantısını denetler; yalnızca bir resim dosyası karşıya yüklediyseniz denir. Hiçbir dosya karşıya sonra resim parametresi ayarlanmamış ve bu nedenle, varsayılan değerini kullanır `Nothing`. Resim karşıya yüklediyseniz ve `ValidPictureUpload` döndürür `True`, `picture` parametre yöntem döndürüyorsa karşıya yüklenen görüntünün ikili veri atanan `False`, güncelleştirme iş akışını iptal edilir ve olay işleyicisi çıkıldı.

`ValidPictureUpload(FileUpload)` DetailsView s'den bulunanad yöntemi kodu `ItemInserting` olay işleyicisi izler:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>8. adım: özgün kategoriler resimleri jpg formatından ile değiştirme

Özgün sekiz kategorileri resimleri bir OLE üstbilgisinde Sarmalanan bit eşlem dosyaları olduğunu hatırlayın. Varolan bir kayıt s resmi düzenleme yeteneğini ekledik, bu bit eşlemler jpg formatından ile değiştirmek için bir dakikanızı ayırın. Geçerli kategori resimleri kullanmaya devam etmek istiyorsanız, bunları jpg formatından için aşağıdaki adımları gerçekleştirerek dönüştürebilirsiniz:

1. Bit eşlem görüntüleri sabit diskinize kaydedin. Ziyaret `UpdatingAndDeleting.aspx` sayfasında tarayıcınızda ve ilk sekiz kategorilerin her biri için görüntüye sağ tıklayın ve resim kaydetmek üzere seçim yapın.
2. Görüntü, seçim görüntü düzenleyicisinde açın. Örneğin, Microsoft Paint kullanabilirsiniz.
3. Bit eşlem JPG resim olarak kaydedin.
4. JPG dosyası kullanarak düzenleme arabirimi aracılığıyla kategori s resmi güncelleştirin.

Bir kategori düzenleme ve JPG resim karşıya sonra görüntüyü tarayıcıda çünkü işlemez `DisplayCategoryPicture.aspx` sayfa ilk sekiz kategorileri resimlerinin ilk 78 baytlar çıkarma. Bu, OLE üstbilgi çıkarma yapan kodunu kaldırarak düzeltin. Bunu yaptıktan sonra `DisplayCategoryPicture.aspx``Page_Load` olay işleyicisi, yalnızca aşağıdaki kodu olmalıdır:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Ekleme ve arabirimleri düzenleme sayfası s biraz daha fazla iş kullanır. `CategoryName` Ve `Description` BoundFields DetailsView ve GridView TemplateFields dönüştürülmesi. Bu yana `CategoryName` izin verme `NULL` değerleri, bir RequiredFieldValidator eklenmelidir. Ve `Description` TextBox büyük olasılıkla dönüştürülmesi gereken çok satırlı metin kutusuna. I bu son rötuşları bir alıştırma olarak sizin yerinize bırakın.


## <a name="summary"></a>Özet

Bu öğretici ikili verileri ile çalışma bizim göz tamamlar. Bu öğretici, önceki üç içinde nasıl ikili veri gördüğümüz dosya sistemindeki veya doğrudan veritabanında depolanabilir. Bir kullanıcı bir dosyayı kendi sabit sürücüden seçerek ve burada da yüklenebilir dosya sisteminde depolanan veya veritabanına eklenen web sunucusuna karşıya sistem ikili veriler sağlar. ASP.NET 2.0, böyle bir arabirim sürükle ve bırak kadar kolay sağlayan kılar bir dosya yükleme denetimi içerir. Bununla birlikte, içinde belirtilenlerle [yüklenen dosyalar](uploading-files-vb.md) öğretici, dosya yükleme denetimi, yalnızca görece küçük dosya yüklemeleri, ideal olarak bir megabayt geçmeyen için oldukça uygun. Biz de düzenlemek ve varolan kayıtlardan ikili verileri silmek nasıl yanı sıra temel alınan veri modeli ile karşıya yüklenen veriler ilişkilendirilecek nasıl incelediniz.

Bizim sonraki öğreticileri kümesini çeşitli önbelleğe alma teknikleri inceler. Önbelleğe alma, bir uygulama s geliştirmek için bir yol sağlar maliyeti yüksek işlemler sonuçlarından alma ve daha hızlı erişilebilecek bir konuma depolayarak genel performansı.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](including-a-file-upload-option-when-adding-a-new-record-vb.md)
