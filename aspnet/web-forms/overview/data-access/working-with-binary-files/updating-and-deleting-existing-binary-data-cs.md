---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Mevcut ikili verileri (C#) güncelleştirme ve silme | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde nasıl GridView denetiminde düzenlemek ve metin verilerini silmek kolaylaştırır gördük. Bu öğreticide nasıl GridView denetiminde de hale gör...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7ab615585da1fb324f0740c1626c70dd7e99df
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754182"
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Mevcut ikili verileri (C#) güncelleştirme ve silme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) veya [PDF olarak indirin](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Önceki öğreticilerde nasıl GridView denetiminde düzenlemek ve metin verilerini silmek kolaylaştırır gördük. Bu öğreticide nasıl GridView denetimi ayrıca, düzenlemek ve bu ikili veriler veritabanına kaydedilir ya da dosya sisteminde depolanan ikili verileri silmek mümkün kılar bakın.


## <a name="introduction"></a>Giriş

Son üç öğreticiler üzerinde size tam anlamıyla bir bit ikili verilerle çalışmak için işlev ekledik. Ekleyerek başlangıç bir `BrochurePath` sütuna `Categories` tablosundaki ve mimarisini buna uygun olarak güncelleştirildi. Kategorileri mevcut s tablosu ile çalışmak için veri erişim katmanı ve iş mantığı katmanı yöntemler de ekledik `Picture` sütunu ikili içerik s bir görüntü dosyasının tutar. Gösterilen kategori s Resimli bir indirme bağlantısı için Broşürü GridView ikili verileri sunmak için web sayfaları derledik bir `<img>` öğesi ve kullanıcıların Broşürü ve resim verilerini karşıya yükleme ve yeni kategori eklemek için bir DetailsView ekledik.

Uygulanacak kalan tek şey düzenleme ve biz GridView s yerleşik düzenleme kullanarak ve özellikleri silerek Bu öğreticide gerçekleştirilmesi mevcut kategorileri silme olanağı. Kullanıcı, bir kategori düzenlerken, isteğe bağlı olarak yeni bir resim yükleyin ya da var olan bir kullanmaya devam kategorisine sahip olacaktır. Broşürlerde, bunlar ya da mevcut Broşürü yeni Broşürü karşıya yükleyin veya kategorisi artık onunla ilişkili broşürlerde olduğunu belirtmek için kullanmayı da tercih edebilirsiniz. Let s başlayın!

## <a name="step-1-updating-the-data-access-layer"></a>1. adım: veri erişim katmanı güncelleştiriliyor

DAL otomatik olarak oluşturulmuş `Insert`, `Update`, ve `Delete` yöntemleri, ancak bu yöntemleri oluşturuldu göre `CategoriesTableAdapter` içermemesi s ana sorgu `Picture` sütun. Bu nedenle, `Insert` ve `Update` yöntemleri kategori s resmi için ikili verileri belirtmek için parametreleri içermez. Yaptığımız gibi [önceki öğretici](including-a-file-upload-option-when-adding-a-new-record-cs.md), güncelleştirmeye yönelik yeni bir TableAdapter yöntemi oluşturmak ihtiyacımız `Categories` ikili verileri belirtirken tablo.

Türü belirtilmiş veri kümesi'ni açın ve sağ Tasarımcısı'ndan `CategoriesTableAdapter` s üst bilgisi ve Sorgu Ekle bağlam menüsünden launche için TableAdapter sorgu Yapılandırma Sihirbazı'nı seçin. Bu sihirbaz, bize TableAdapter sorgusu veritabanına nasıl erişmeli isteyerek başlar. SQL deyimi Kullan'ı seçip İleri'ye tıklayın. Sonraki adım sorgu türü için oluşturulmasını ister. Size yeni bir kayıt eklemek için sorgu oluşturma re beri `Categories` tablo, GÜNCELLEŞTİRMEYİ seçin ve İleri'ye tıklayın.


[![GÜNCELLEŞTİRME seçeneğini belirleyin](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Şekil 1**: güncelleştirme seçeneğini belirleyin ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


Artık belirtmek ihtiyacımız `UPDATE` SQL deyimi. Sihirbaz otomatik olarak öneren bir `UPDATE` TableAdapter s ana sorguda karşılık gelen deyimi (güncelleştiren bir `CategoryName`, `Description`, ve `BrochurePath` değerler). Deyimi değiştirin böylece `Picture` sütundur ile birlikte dahil edilen bir `@Picture` parametresi, şu şekilde:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

Sihirbazın son ekran bize yeni TableAdapter yöntem adı ister. Girin `UpdateWithPicture` ve Son'a tıklayın.


[![Yeni bir TableAdapter yöntemi UpdateWithPicture adı](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Şekil 2**: yeni bir TableAdapter yöntem adı `UpdateWithPicture` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>2. adım: iş mantığı katmanı yöntemler ekleme

DAL güncelleştirmeye ek olarak, biz BLL yöntemlerini güncelleştirme, silme bir kategori içerecek şekilde güncelleştirmeniz gerekir. Bu sunum katmanı çağrılan yöntemlerdir.

Bir kategoriyi silmek için kullanabilir miyiz `CategoriesTableAdapter` otomatik olarak oluşturulan s `Delete` yöntemi. Aşağıdaki yöntemi ekleyin `CategoriesBLL` sınıfı:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Bu öğreticide, let s oluşturun bir kategori - ikili resim verileri bekliyor ve çağıran bir güncelleştirme için iki yöntem `UpdateWithPicture` yalnızca eklediğimiz için yöntem `CategoriesTableAdapter` kabul eden başka bir yalnızca `CategoryName`, `Description`ve `BrochurePath`kullanır ve değerleri `CategoriesTableAdapter` s otomatik olarak oluşturulan sınıfı `Update` deyimi. Bazı durumlarda, yeni resmi karşıya yükleme durumu kullanıcı gerekir, diğer alanları, birlikte kategori s resmi güncelleştirmek bir kullanıcı isteyebilirsiniz iki yöntemle arkasında stratejinin olur. Karşıya yüklenen resim s ikili veriler daha sonra kullanılabilir `UPDATE` deyimi. Diğer durumlarda, kullanıcı yalnızca güncelleştirme, örneğin, ad ve açıklama ilgilenebilecek. Ancak `UPDATE` ifadesi bekliyor, ikili veri `Picture` de sütun sonra size, d gerek de bu bilgiyi sağlamak. Bu veritabanına yeniden düzenlenmekte olan kayıt için resim verileri getirmek için fazladan bir seyahat gerektirir. Bu nedenle, şu iki istediğiniz `UPDATE` yöntemleri. Hangisinin kullanılacağını resim veri kategorisi güncelleştirilirken sağlanan temel alarak iş mantığı katmanı belirler.

Bunu kolaylaştırmak için iki yöntem için ekleme `CategoriesBLL` sınıfı, hem adlı `UpdateCategory`. İlk üç kabul `string` s, bir `byte` dizi ve bir `int` giriş olarak parametreler; ikinci yalnızca üç `string` s ve `int`. `string` Giriş parametreleridir s kategori adı, açıklama ve Broşürü dosya yolu `byte` dizidir kategori s resmi ikili içeriğini ve `int` tanımlayan `CategoryID` güncelleştirilecek kaydın. İlk aşırı yükleme geçilen ikinci if çağırır bildirimi `byte` dizi `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>3. adım: ekleme ve görünümü işlevselliğini kopyalama

İçinde [önceki öğretici](including-a-file-upload-option-when-adding-a-new-record-cs.md) adlı bir sayfa oluşturduk `UploadInDetailsView.aspx` GridView tüm kategorilerde listelenen ve sağlanan bir DetailsView sisteme yeni kategoriler ekleyecektir. Bu öğreticide size GridView düzenleme ve silme desteği içerecek şekilde genişletin. Gelen çalışmaya devam yerine `UploadInDetailsView.aspx`, let s, Bu öğretici s değişiklikleri bunun yerine koyun `UpdatingAndDeleting.aspx` aynı klasöre sayfasından `~/BinaryData`. Ve bildirim temelli biçimlendirme kopyalayıp yapıştırabilirsiniz gelen kod `UploadInDetailsView.aspx` için `UpdatingAndDeleting.aspx`.

Başlangıç açarak `UploadInDetailsView.aspx` sayfası. Tüm bildirim temelli söz dizimi içinde kopyalayın `<asp:Content>` Şekil 3'te gösterildiği gibi öğesi. Ardından, açık `UpdatingAndDeleting.aspx` ve bu biçimlendirme içinde yapıştırın, `<asp:Content>` öğesi. Benzer şekilde, koddan kopyalama `UploadInDetailsView.aspx` sayfasında s arka plan kod sınıfı `UpdatingAndDeleting.aspx`.


[![Bildirim temelli UploadInDetailsView.aspx biçimlendirmeden kopyalama](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Şekil 3**: bildirim temelli biçimlendirmeden kopyalama `UploadInDetailsView.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


Bildirim temelli işaretleme ve kod üzerinde kopyaladıktan sonra ziyaret `UpdatingAndDeleting.aspx`. Aynı çıktı ve aynı kullanıcı deneyimine sahip görmelisiniz olduğu gibi `UploadInDetailsView.aspx` önceki öğreticide sayfasından.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>4. adım: ObjectDataSource ve GridView destek silme ekleme

Geri ele aldığımız gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğreticide GridView yerleşik silme özellikleri sağlar ve bu yetenekler bir onay kutusu değer çizgisi sırasında etkinleştirilebilir temel kılavuz s veri kaynağı silme destekler. Şu anda ObjectDataSource GridView bağlı (`CategoriesDataSource`) silmeyi desteklemez.

Bu sorunu gidermek için sihirbazını başlatmak için veri kaynağı yapılandırma seçeneğinden üzerinde ObjectDataSource s akıllı etiket tıklayın. ObjectDataSource ile çalışmak üzere yapılandırıldı ilk ekran gösterilmektedir `CategoriesBLL` sınıfı. Sonraki basın. Şu anda yalnızca ObjectDataSource s `InsertMethod` ve `SelectMethod` özellikleri belirtilir. Ancak, sihirbaz otomatik olarak güncelleştir ve Sil sekmelerle açılan listelerde doldurulur `UpdateCategory` ve `DeleteCategory` yöntemleri, sırasıyla. Bunun nedeni, içinde `CategoriesBLL` Biz bu yöntemleri kullanarak işaretlenmiş sınıf `DataObjectMethodAttribute` güncelleştirme ve silme için varsayılan yöntemler olarak.

Şimdilik güncelleştirme s sekmesi açılır listede (hiçbiri) ayarlandı, ancak kümesine silme s sekmesi açılır listede bırakın `DeleteCategory`. Bu Sihirbazı'nı güncelleştirme desteği eklemek için adım 6'daki getireceğiz.


[![ObjectDataSource DeleteCategory yöntemi kullanmak üzere yapılandırma](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Şekil 4**: ObjectDataSource kullanılacak yapılandırma `DeleteCategory` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> Sihirbazı tamamladığınızda, Visual Studio Web verileri yeniden oluşturulacak alanları Yenile'yi ve anahtarları istiyorsanız, denetimleri isteyebilir. Evet'i seçerseniz, yaptığınız herhangi bir alan özelleştirme üzerine yazılacağından, Hayır'ı seçin.


ObjectDataSource artık için bir değer içerir, `DeleteMethod` özelliğinin yanı sıra bir `DeleteParameter`. Geri Çağırma yöntemlerini belirtmek için Sihirbazı kullanarak Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğini `original_{0}`, güncelleştirme ile sorunlara neden olur ve yöntem çağrıları silin. Bu nedenle, bu özelliği tamamen temizleyin veya Varsayılana Sıfırla `{0}`. ObjectDataSource bu özellik, bellek yenilemek ihtiyacınız varsa bkz [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğretici.

Sihirbaz tamamlandıktan ve düzelttikten sonra `OldValuesParameterFormatString`, ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

ObjectDataSource yapılandırdıktan sonra GridView s akıllı etiketinde silmeyi etkinleştir onay kutusunu işaretleyerek GridView'a silme özellikleri ekleyin. Bu bir CommandField GridView'a ekler, `ShowDeleteButton` özelliği `true`.


[![GridView içinde silmek için desteği etkinleştir](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Şekil 5**: GridView içinde silme için desteği etkinleştirme ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


Silme işlevini test etmek için bir dakikanızı ayırın. Bir yabancı anahtar arasında `Products` tablo s `CategoryID` ve `Categories` tablo s `CategoryID`, ilk sekiz kategorilerden herhangi biri silmeye çalışırsanız bir yabancı anahtar kısıtlaması ihlali özel durum alırsınız. Bu işlevleri test etmek için bir Broşürü ve resim sağlayan yeni bir kategori ekleyin. Şekil 6 üzerinde gösterilen benim test kategorisi adlı bir test Broşürü dosya içerir `Test.pdf` ve sınama resmi. Şekil 7, test kategorisi eklendikten sonra GridView gösterir.


[![Test kategori Broşürü ve görüntü ekleme](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Şekil 6**: Broşürü ve görüntü ile bir Test kategorisine ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![Test kategorisi ekledikten sonra GridView görüntülenir](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Şekil 7**: Test kategorisi ekledikten sonra GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


Visual Studio'da Çözüm Gezgini'ni yenileyin. Şimdi yeni bir dosya görmeniz gerekir `~/Brochures` klasöründe `Test.pdf` (bkz. Şekil 8).

Ardından, sayfanın geri gönderme neden Test kategorisi sıradaki Sil bağlantısını tıklayın ve `CategoriesBLL` s sınıfı `DeleteCategory` ateşlenmesine yöntemi. Bu DAL s çağıracağı `Delete` yöntemi, uygun neden `DELETE` veritabanına gönderilecek bildirimi. Veri ardından GridView'a DataSet'e ve biçimlendirme artık mevcut Test kategorisindeki istemciye geri gönderilir.

Silme iş akışı Test kategorisi kaydını başarıyla kaldırıldı. ancak `Categories` tablo, kendi Broşürü dosyasını web s sunucusu dosya sisteminde kaldırmadı. Çözüm Gezginini yenileyin ve göreceksiniz `Test.pdf` hala oturan `~/Brochures` klasör.


![Web sunucusu s dosya sisteminden Test.pdf dosyası silinmedi](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Şekil 8**: `Test.pdf` Web s sunucusu dosya sisteminde dosya silinmedi


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>5. adım: silinen kategori s Broşürü dosyasını kaldırılıyor

İkili verileri veritabanına dış depolama olumsuzlukları ilişkili veritabanı kaydı silindiğinde, bu dosyaları temizlemek için ek adımlar atılmalıdır biridir. GridView ve ObjectDataSource önce hem delete komutu gerçekleştirildikten sonra tetiklenen olayları sağlar. Biz aslında öncesi ve sonrası eylem olayları için olay işleyicileri oluşturmanız gerekir. Önce `Categories` kaydı silindiğinde, PDF dosyası s yolu belirlemek ihtiyacımız ancak biz t kategorisi kategori silinmez ve bazı özel durum durumunda silinmeden önce PDF silmek istiyorsanız ki.

GridView s [ `RowDeleting` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) ateşlenir ObjectDataSource s delete komutu çağrıldıktan önce açıkken kendi [ `RowDeleted` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) sonra ateşlenir. Aşağıdaki kodu kullanarak bu iki olayları için olay işleyicileri oluşturun:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

İçinde `RowDeleting` olay işleyicisi `CategoryID` satırının silinmesini GridView s yakaladı `DataKeys` bu olay işleyicisi erişilebilen koleksiyonuna `e.Keys` koleksiyonu. İleri `CategoriesBLL` s sınıfı `GetCategoryByCategoryID(categoryID)` silinmesini kaydı hakkında bilgi döndürmek için çağrılır. Varsa döndürülen `CategoriesDataRow` nesne sahip olmayan bir`NULL``BrochurePath` sayfa değişkenine depolandığını sonra değer `deletedCategorysPdfPath` böylece dosyanın içinde silinebilir `RowDeleted` olay işleyicisi.

> [!NOTE]
> Almak yerine `BrochurePath` ayrıntıları `Categories` içinde silinen kayıt `RowDeleting` olay işleyicisi alternatif olarak eklediğimiz `BrochurePath` GridView s `DataKeyNames` özelliği ve kayıt s değerinin erişilebilir aracılığıyla `e.Keys` koleksiyonu. Bunun yapılması biraz GridView s görünüm durumu boyutunu artırabilirsiniz ancak gereken kod miktarını azaltmak ve bir seyahat veritabanına kaydedin.


Temel alınan delete komutu s çağırıldı, ObjectDataSource GridView s sonra `RowDeleted` olay işleyicisi ateşlenir. Verileri silme hiçbir özel durum oluştu ve için bir değer yoksa `deletedCategorysPdfPath`, ardından PDF dosya sisteminden silindi. Bu ek bir kod resmi ile ilişkilendirilmiş kategori s ikili verileri temizlemek için gerekli olmadığını unutmayın. Bu s resim verilerini doğrudan veritabanında depolandığı için bu nedenle silme `Categories` satır, kategori s resim verilerini de siler.

İki olay işleyicileri ekledikten sonra bu test çalışmasını yeniden çalıştırın. Kategori siliniyor kendi ilişkili PDF da silinir.

Mevcut bir kayıt s ilişkili ikili verileri güncelleştirme, ilginç bazı zorluklar sağlar. Bu öğreticinin geri kalanında güncelleştirme özellikleri Broşürü ve resim ekleme halinde ilgili alır. 6. adım adım 7 resmi güncelleştirme sırasında ararken Broşürü bilgilerinin güncelleştirilmesi için teknikleri inceler.

## <a name="step-6-updating-a-category-s-brochure"></a>6. adım: bir kategori s Broşürü güncelleştiriliyor

Bölümünde açıklandığı gibi [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) öğreticide GridView, temel alınan veri kaynağı ise, bir onay kutusu değer çizgisi tarafından uygulanan yerleşik satır düzeyinde düzenleme destek sunar uygun şekilde yapılandırılmış. Şu anda `CategoriesDataSource` ObjectDataSource henüz yapılandırılmamış let s eklemek için destek, güncelleştirme eklenecek.

ObjectDataSource s Sihirbazı'ndan veri kaynağı yapılandırma bağlantısına tıklayın ve ikinci adıma geçin. Nedeniyle `DataObjectMethodAttribute` kullanılan `CategoriesBLL`, güncelleştirme aşağı açılan listesi ile otomatik olarak doldurulur `UpdateCategory` dört girdi parametrelerini kabul eden aşırı yükleme (tüm sütunlar için ancak `Picture`). Beş parametrelerle aşırı kullanmasını sağlayacak şekilde değiştirin.


[![ObjectDataSource resmi bir parametre içeren UpdateCategory yöntemi kullanmak üzere yapılandırma](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `UpdateCategory` için bir parametre içeren yöntem `Picture` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource artık için bir değer içerir, `UpdateMethod` özelliğinin yanı sıra karşılık gelen `UpdateParameter` s. 4. adımda da belirtildiği gibi Visual Studio ObjectDataSource s ayarlar `OldValuesParameterFormatString` özelliğini `original_{0}` veri kaynağı Yapılandırma Sihirbazı'nı kullanırken. Bu güncelleştirme ile sorunlara neden ve yöntem çağrıları Sil. Bu nedenle, bu özelliği tamamen temizleyin veya Varsayılana Sıfırla `{0}`.

Sihirbaz tamamlandıktan ve düzelttikten sonra `OldValuesParameterFormatString`, ObjectDataSource s bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

GridView s yerleşik düzenleme özelliklerini etkinleştirme GridView s akıllı etiket düzenlemeyi etkinleştir seçeneğini denetleyin. Bu ayarlar CommandField s `ShowEditButton` özelliğini `true`, sonuçta elde edilen ayrıca bir Düzenle düğmesi (ve düzenlenmekte olan satır için güncelleştirme ve İptal düğmeleri).


[![GridView'ın düzenleme desteği için yapılandırma](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Şekil 10**: GridView düzenleme desteği için yapılandırma ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve satır s Düzenle düğmeler birine tıklayın. `CategoryName` Ve `Description` BoundFields metin kutuları işlenir. `BrochurePath` TemplateField bulunmayan bir `EditItemTemplate`, gösterilmeye devam eder, `ItemTemplate` Broşürü bağlantı. `Picture` ImageField işleyen bir metin kutusu olarak ayarlanmış `Text` özelliği ImageField s değerinin atandığında `DataImageUrlField` değeri, bu durumda `CategoryID`.


[![GridView düzenleme bir arabirim için BrochurePath eksik.](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Şekil 11**: GridView düzenlemeyi arabirim eksik `BrochurePath` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Özelleştirme`BrochurePath`s düzenleme arabirimi

Düzenleme arabirim oluşturmak ihtiyacımız `BrochurePath` TemplateField, ya da kullanıcıya izin veren:

- Kategori s Broşürü olarak bırakın-olan
- Yeni bir Broşürü yükleyerek kategori s Broşürü güncelleştirin veya
- Kategori s Broşürü (kategorisi artık ilişkili bir Broşürü vardır durumda) tamamen kaldırın.

Biz de güncelleştirmeniz gerekiyor `Picture` ImageField s düzenleme arabirimi, ancak elde edeceğiniz için bu adım 7'de.

GridView s akıllı etiket Şablonları Düzenle bağlantısına tıklayın ve seçin `BrochurePath` TemplateField s `EditItemTemplate` aşağı açılan listeden. RadioButtonList Web denetim ekleme ayarı, bu şablon için kendi `ID` özelliğini `BrochureOptions` ve kendi `AutoPostBack` özelliğini `true`. Özellikler penceresinden üç noktaya tıklayın `Items` getirir özelliği `ListItem` Koleksiyonu Düzenleyicisi. Aşağıdaki üç seçenekle ekleme `Value` s 1, 2 ve 3, sırasıyla:

- Geçerli Broşürü kullanın
- Geçerli Broşürü Kaldır
- Yeni Broşürü karşıya yükleme

İlk kümesinden `ListItem` s `Selected` özelliğini `true`.


![RadioButtonList için üç ListItems Ekle](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Şekil 12**: üç ekleme `ListItem` RadioButtonList s


RadioButtonList adlı FileUpload denetim ekleme `BrochureUpload`. Ayarlama, `Visible` özelliğini `false`.


[![RadioButtonList ve FileUpload denetimi için EditItemTemplate Ekle](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Şekil 13**: RadioButtonList ve FileUpload denetimine ekleme `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Bu RadioButtonList kullanıcı için üç seçeneği sağlar. Yalnızca son seçenek, karşıya yükleme yeni Broşürü seçtiyseniz FileUpload denetim görüntülenir olur. Bunu gerçekleştirmek için bir olay işleyicisi RadioButtonList s için oluşturma `SelectedIndexChanged` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

RadioButtonList ve FileUpload denetimleri şablonu içinde olduğundan, biz biraz bu denetimleri programlı olarak erişmek için kod yazmanız gerekir. `SelectedIndexChanged` Olay işleyicisi, bir başvuru RadioButtonList geçirilir `sender` giriş parametresi. FileUpload denetim altına almak için Denetim ve kullanım RadioButtonList s üst almak ihtiyacımız `FindControl("controlID")` buradan yöntemi. RadioButtonList ve FileUpload denetimleri başvuru sahibiz sonra FileUpload denetim s `Visible` özelliği `true` yalnızca RadioButtonList s `SelectedValue` olduğu 3, eşittir `Value` karşıya yeni Broşürü için `ListItem`.

Yerinde şu kodla düzenleme arabirimini test etmek için bir dakikanızı ayırın. Bir satır için Düzenle düğmesine tıklayın. Başlangıçta, kullanım geçerli Broşürü seçeneği seçili olmalıdır. Seçilen dizin değiştirme geri göndermeye neden olur. Üçüncü seçenek belirlenirse, FileUpload denetim görüntülenir, aksi takdirde gizlenir. Düzenle düğmesine tıklandığında Şekil 14 düzenleme arabirimi gösterir. Karşıya yeni Broşürü seçeneği seçtikten sonra Şekil 15 arabirimini gösterir.


[![Seçeneği başlangıçta kullanımı geçerli Broşürü](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Şekil 14**: seçeneği belirlendiğinde başlangıçta kullanımı geçerli Broşürü ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Karşıya yeni Broşürü seçeneği görüntüler FileUpload denetim seçme](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Şekil 15**: karşıya yeni Broşürü seçeneği görüntüler FileUpload denetimi seçme ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Broşürü kaydetme dosya ve güncelleştirme`BrochurePath`sütun

GridView s güncelleştir düğmesine tıklandığında, `RowUpdating` olay harekete geçirilir. S güncelleştirme komut çağrıldığında ObjectDataSource ve GridView s `RowUpdated` olay harekete geçirilir. Gibi silme iş akışı ile hem de bu olayları olay işleyicilerini oluşturma ihtiyacımız var. İçinde `RowUpdating` olay işleyicisi, ihtiyacımız dayanarak hangi eylemin yapılacağını belirlemek `SelectedValue` , `BrochureOptions` RadioButtonList:

- Varsa `SelectedValue` 1, aynı kullanmaya devam etmek istediğimiz `BrochurePath` ayarı. Bu nedenle, ObjectDataSource s ayarlamak ihtiyacımız `brochurePath` varolan parametresi `BrochurePath` güncelleştirilmesini kayıt değeri. ObjectDataSource s `brochurePath` parametresi kullanılarak ayarlanabilir `e.NewValues["brochurePath"] = value`.
- Varsa `SelectedValue` 2'dir ve ardından kaydı s istiyoruz `BrochurePath` değerini `NULL`. Bu ObjectDataSource s ayarlayarak gerçekleştirilebilir `brochurePath` parametresi `Nothing`, bir veritabanında sonuçları `NULL` kullanılmakta `UPDATE` deyimi. Kaldırılmakta olan mevcut bir Broşürü dosyası varsa, biz varolan dosyayı silmeniz gerekir. Ancak, yalnızca bir özel durum yükseltmeden güncelleştirme tamamlandığında bunu istiyoruz.
- Varsa `SelectedValue` biz kullanıcı karşıya bir PDF dosyası emin olun ve ardından dosya sistemine Kaydet ve s kaydı güncelleştirmek istiyorsanız 3 olan `BrochurePath` sütun değeri. Ayrıca, değiştirilmekte olan mevcut bir Broşürü dosyası varsa, biz önceki dosyayı silmeniz gerekir. Ancak, yalnızca bir özel durum yükseltmeden güncelleştirme tamamlandığında bunu istiyoruz.

Ne zaman tamamlanması için gerekli olan adımları RadioButtonList s `SelectedValue` olan 3 DetailsView s tarafından Kullanılanlara neredeyse aynı `ItemInserting` olay işleyicisi. Yeni bir kategori kayıt ekledik, DetailsView denetiminde eklendiğinde bu olay işleyicisi yürütülür [önceki öğreticide](including-a-file-upload-option-when-adding-a-new-record-cs.md). Bu nedenle, bunu bize bu işlevleri ayrı yöntemlerde yeniden behooves. Özellikle, ı ortak işlevselliği iki yöntem taşındı:

- `ProcessBrochureUpload(FileUpload, out bool)` Giriş olarak bir FileUpload denetim örneği ve olup delete veya düzenleme işlemi devam edileceğine veya bunu bazı doğrulama hatası nedeniyle iptal edilip varsa belirten bir çıkış Boole değeri kabul eder. Bu yöntem, kaydedilen dosyanın yolunu döndürür veya `null` hiçbir dosya kaydedilmişse.
- `DeleteRememberedBrochurePath` Sayfa değişkeni yolu tarafından belirtilen dosyayı siler `deletedCategorysPdfPath` varsa `deletedCategorysPdfPath` değil `null`.

Bu iki yöntem için kod izler. Benzerlik arasında Not `ProcessBrochureUpload` ve DetailsView s `ItemInserting` olay işleyicisinden önceki öğretici. Bu öğreticide, bu yeni yöntemler kullanmayı DetailsView s olay işleyicileri miyim güncelleştirdik. DetailsView s olay işleyicileri için yapılan değişiklikleri görmek için Bu öğretici ile ilişkili kod indirin.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

GridView s `RowUpdating` ve `RowUpdated` olay işleyicilerini `ProcessBrochureUpload` ve `DeleteRememberedBrochurePath` yöntemleri, aşağıdaki kodun gösterdiği olarak:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Not nasıl `RowUpdating` olay işleyicisi dayalı olarak uygun bir eylem gerçekleştirmek için bir dizi koşul deyimlerini kullanır `BrochureOptions` RadioButtonList s `SelectedValue` özellik değeri.

Yerinde şu kodla bir kategoriyi düzenlemek ve bu, geçerli Broşürü kullanın, hiçbir Broşürü kullanın veya yeni bir tane karşıya yükleyin. Bir tane deneyin. Ayarlanan kesme noktaları `RowUpdating` ve `RowUpdated` olay işleyicileri'iş akışının bir fikir edinebilirsiniz.

## <a name="step-7-uploading-a-new-picture"></a>7. adım: yeni bir resim karşıya yükleme

`Picture` Değeriyle doldurulmuş bir metin kutusu olarak arabirimi işler düzenleme ImageField s kendi `DataImageUrlField` özelliği. Düzenleme iş akışı sırasında GridView parametre ObjectDataSource parametre adı ile ImageField s değerini geçirir `DataImageUrlField` özelliği ve parametre s değeri düzenleme arabiriminde metin kutusu içine girilen değer. Bu davranış, resim dosya sistemindeki bir dosya olarak kaydedildiğinde uygundur ve `DataImageUrlField` tam görüntü URL'sini içerir. Böyle durumlarda kullanıcı değiştirebilirsiniz ve veritabanına geri kaydettiniz metin kutusuna görüntü s URL'si düzenleme arabirimini görüntüler. Bu varsayılan arabirimi eklenmemişse t izin verildiyse kullanıcının yeni bir resim yükleyin, ancak bunları geçerli değer görüntünün URL'sini değiştirmek izin vermez. Bu öğreticide, ancak ImageField s varsayılan arabirimi düzenleme için yeterli değil `Picture` doğrudan veritabanında ikili veri depolanıyor ve `DataImageUrlField` özelliği ayrı tutma yalnızca `CategoryID`.

Müşterilerimize öğreticide ne bir kullanıcı bir ImageField sahip bir satır düzenlediğinde daha iyi anlamak için aşağıdaki örneği inceleyin: bir kullanıcı ile bir satır düzenler `CategoryID` neden 10 `Picture` ImageField textbox değeri 10 ile olarak işlemek için. Kullanıcı bu metin kutusundaki değeri 50 olarak değiştirir ve güncelleştir düğmesine tıkladığında hayal edin. Bir geri gönderme oluşur ve GridView başlangıçta adlı bir parametre oluşturur `CategoryID` 50 değerine sahip. Ancak, bu parametre GridView göndermeden önce (ve `CategoryName` ve `Description` parametreleri), değerler ekler `DataKeys` koleksiyonu. Bu nedenle, üzerine yazar `CategoryID` geçerli arka plandaki satır s ile parametre `CategoryID` 10 değeri. Kısacası, arabirimini düzenleme ImageField s herhangi bir etkisi olan düzenleme iş akışı Bu öğretici için sahip olduğundan ImageField s adlarını `DataImageUrlField` özelliği ve s kılavuz `DataKey` değeri, bir aynı.

ImageField veritabanı verileri temel alan bir görüntü kolaylaştırır, ancak biz textbox düzenleme arabiriminde sağlamak istediğiniz t ki. Bunun yerine, son kullanıcı kategorisi s resmi değiştirmek için kullanabileceğiniz bir FileUpload denetimi sunmak istiyoruz. Farklı `BrochurePath` değeri bu öğreticiler için biz ve karar her kategorinin resim sahip olmasını gerektirir. Bu nedenle, şu t, kullanıcının geçerli bir resim olarak bırakın ya da kullanıcı ilişkili resim olabilir ya da karşıya yeni bir resim olduğunu belirten gerek ki-olduğu.

ImageField s düzenleme arabirimini özelleştirme için biz bir TemplateField dönüştürmeniz gerekir. GridView s akıllı etiket sütunları Düzenle bağlantısına tıklayın, ImageField seçin ve bu alan dönüştürme TemplateField bağlantıya tıklayın.


![Bir TemplateField ImageField Dönüştür](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Şekil 16**: bir TemplateField ImageField Dönüştür


Bu şekilde bir TemplateField ImageField dönüştürme bir TemplateField iki şablonlarla oluşturur. Aşağıdaki bildirim temelli söz dizimi gösterildiği gibi `ItemTemplate` içeren bir görüntü Web ayarlanmış denetim `ImageUrl` özelliği ImageField s tabanlı veri bağlama söz dizimini kullanarak atandığı `DataImageUrlField` ve `DataImageUrlFormatString` özellikleri. `EditItemTemplate` TextBox içerir, `Text` özelliği tarafından belirtilen değere bağlı `DataImageUrlField` özelliği.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Güncelleştirilecek ihtiyacımız `EditItemTemplate` FileUpload denetimi kullanmak için. Bağlantı GridView s akıllı etiket üzerinde Şablonları Düzenle'ye tıklayın ve ardından `Picture` TemplateField s `EditItemTemplate` aşağı açılan listeden. Şablonda TextBox bunu görmeniz gerekir. Ardından, FileUpload denetim ayarı şablona araç kutusundan sürükleyin, `ID` için `PictureUpload`. Ayrıca kategori s resmini değiştirmek için yeni bir resim belirtin metin ekleyin. Aynı kategori s resmi tutmak için alanın şablonu için de boş bırakın.


[![EditItemTemplate için FileUpload denetim ekleme](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Şekil 17**: FileUpload denetimine ekleme `EditItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


Düzenleme arabirimi özelleştirdikten sonra bir tarayıcıda ilerleme durumunuzu görüntüleyin. Bir satır salt okunur modunda görüntülerken, kategori s görüntünün önceden, ancak işler FileUpload denetim metin olarak resim sütunu Düzenle düğmesine tıklayarak olarak gösterilir.


[![Düzenleme arabirimi FileUpload denetimi içerir.](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Şekil 18**: düzenleme arabirimi FileUpload denetimi içerir ([tam boyutlu görüntüyü görmek için tıklatın](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


ObjectDataSource çağırmak için yapılandırıldığını geri çağırma `CategoriesBLL` s sınıfı `UpdateCategory` resmi olarak ikili verileri girdi olarak kabul eden yöntemi bir `byte` dizi. Bu diziye sahipse bir `null` değeri, ancak diğer `UpdateCategory` aşırı çağrılır, hangi sorunların `UPDATE` değiştirdiği SQL deyimi `Picture` sütun, böylece s kategorisi geçerli bırakarak bozulmadan resim. GridView s, bu nedenle, `RowUpdating` ihtiyacımız programlı olarak başvurmak için olay işleyicisi `PictureUpload` FileUpload denetlemek ve bir dosya karşıya yüklendi belirleyin. Biri karşıya sonra yaptığımız *değil* için bir değer belirtmek istiyorsanız `picture` parametresi. Bir dosya karşıya yüklendi, diğer yandan, `PictureUpload` FileUpload denetim istediğimiz bir JPG dosyası olduğundan emin olun. Bunun ardından ikili içeriğini ObjectDataSource için gönderebiliriz `picture` parametresi.

Adım 6'da kullanılan koduyla burada zaten gereken kodun çoğu DetailsView s'te var gibi `ItemInserting` olay işleyicisi. Bu nedenle, ben ve yeni bir yöntem ortak işlevselliği yeniden düzenlenen `ValidPictureUpload`ve güncelleştirilmiş `ItemInserting` bu yöntemi kullanmak için olay işleyicisi.

GridView s başına aşağıdaki kodu ekleyin `RowUpdating` olay işleyicisi. Bu geçersiz resim dosyası karşıya yüklediyseniz Broşürü web sunucusu s dosya sistemine kaydetmek istediğiniz bu kod size t ki bu yana Broşürü dosyayı kaydeder kod önce gelen önemlidir.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

`ValidPictureUpload(FileUpload)` Yöntemi FileUpload denetim, tek bir giriş parametresi olarak alır ve karşıya yüklenen dosya JPG olduğundan emin olmak için karşıya yüklenen dosya s uzantısını denetler; yalnızca bir resim dosyası karşıya yüklediyseniz çağrılır. Dosya karşıya yüklendikten sonra resmi parametre ayarlı değil ve bu nedenle, varsayılan değeri kullanılıyorsa `null`. Bir resim yüklediyseniz ve `ValidPictureUpload` döndürür `true`, `picture` parametre yöntemi döndürürse karşıya yüklenen görüntünün ikili verileri atanmış `false`güncelleştirme iş akışını iptal edilir ve olay işleyicisi çıkıldı.

`ValidPictureUpload(FileUpload)` DetailsView s yeniden düzenlenmeden yöntemi kodu `ItemInserting` olay işleyicisi izler:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>8. adım: özgün kategorileri resimleri jpg formatından ile değiştirme

Özgün sekiz kategorileri resimleri sarmalanmış bir OLE üst bilgisinde bir bit eşlem dosyaları olduğunu hatırlayın. Varolan bir kaydı s resim düzenleme olanağı ekledik, bu bit eşlemler jpg formatından ile değiştirmek için bir dakikanızı ayırın. Geçerli kategori resimleri kullanmaya devam etmek istiyorsanız, aşağıdaki adımları uygulayarak bunları jpg formatından dönüştürebilirsiniz:

1. Bit eşlem resimleri sabit sürücünüze kaydedin. Ziyaret `UpdatingAndDeleting.aspx` sayfa tarayıcınızda ve ilk sekiz kategorilerin her birine yönelik ve resmin sağ resmi kaydetmeyi seçebilirsiniz.
2. Görüntü, görüntü tercih ettiğiniz düzenleyicide açın. Örneğin, Microsoft Paint'te kullanabilirsiniz.
3. Bit eşlem JPG resim olarak kaydedin.
4. Kategori s resim düzenleme arabiriminden JPG dosyasını kullanarak güncelleştirin.

Bir kategori düzenleme ve JPG resim karşıya sonra resmi tarayıcıda çünkü işlenmez `DisplayCategoryPicture.aspx` ilk sekiz kategorilerin resimleri ilk 78 baytlar sayfa şeridi oluşturma. Bu, OLE şeridi oluşturma üst bilgisi yapan kodunu kaldırarak düzeltin. Bunu yaptıktan sonra `DisplayCategoryPicture.aspx``Page_Load` olay işleyicisine aşağıdaki kodu sahip olmalıdır:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx` Ekleme ve arabirimleri düzenleme sayfası s biraz daha fazla iş kullanabilir. `CategoryName` Ve `Description` DetailsView ve GridView BoundFields TemplateField dönüştürülmesi. Bu yana `CategoryName` Project'in izin vermediği `NULL` değerleri bir RequiredFieldValidator eklenmelidir. Ve `Description` TextBox büyük olasılıkla dönüştürülmesi gereken çok satırlı TextBox'a. Ben bu Son dokunuşları bir alıştırma olarak sizin için bırakın.


## <a name="summary"></a>Özet

Bu öğreticide, ikili verilerle çalışma bizim göz tamamlar. Bu öğretici ve önceki üç nasıl ikili verileri gördüğümüz dosya sisteminde veya doğrudan veritabanı içinde depolanabilir. Bir kullanıcı, kendi sabit diskinizden bir dosya seçip burada dosya sisteminde depolanan veya yükleyebilir veritabanına eklenen web sunucusuna karşıya sistem ikili verileri sağlar. ASP.NET 2.0 sağlama böyle bir arabirim sürükle ve bırak kadar kolay hale getiren bir FileUpload denetimi içerir. Bununla birlikte, içinde belirtilenlerle [yüklenen dosyalar](uploading-files-cs.md) öğreticide FileUpload denetim, yalnızca nispeten küçük bir dosya karşıya yükler, ideal olarak bir megabayt geçmeyen için uygundur. Biz de düzenlemek ve var olan kayıtlardan ikili verileri silmek nasıl yanı sıra, karşıya yüklenen verileri temel alınan veri modeli ile ilişkilendirmek nasıl incelediniz.

Sonraki kümemizdeki öğretici çeşitli önbelleğe alma tekniklerini açıklar. Önbelleğe alma, bir uygulama s geliştirmek için bir yol sağlar pahalı işlemlerinin sonuçlarını almak ve bunları daha hızlı bir şekilde erişilebilir bir konumda depolayarak genel performansı.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murphy oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](including-a-file-upload-option-when-adding-a-new-record-cs.md)
> [İleri](uploading-files-vb.md)
