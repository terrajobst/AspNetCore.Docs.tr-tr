---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: "Bir dosya karşıya yükleme seçeneği yeni bir kayıt (C#) eklerken | Microsoft Docs"
author: rick-anderson
description: "Bu öğretici hem metin verileri girin ve ikili dosyaları karşıya yükleme yapmasına izin veren bir Web arabirimi oluşturulacağını gösterir. Seçenekleri kullanılabilir t göstermek için..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: fcb791868e6af9eef1614d039d11ef5232b40af5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Yeni bir kayıt (C#) eklerken bir dosyayı karşıya yükleme seçeneği de dahil olmak üzere
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) veya [PDF indirin](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Bu öğretici hem metin verileri girin ve ikili dosyaları karşıya yükleme yapmasına izin veren bir Web arabirimi oluşturulacağını gösterir. Diğer dosya sisteminde barındırırken ikili verileri depolamak için kullanılabilir seçenekleri göstermek için bir dosya veritabanına kaydedilir.


## <a name="introduction"></a>Giriş

Önceki iki eğitimlerine biz dosyaları istemciden web sunucusuna göndermek ve bir veri W bu ikili verileri sunmak öğrendiniz dosya yükleme denetimi kullanmak nasıl Aranan uygulama s veri modeli ile ilişkili ikili verileri depolamak için teknikleri incelediniz EB denetim. Biz ve henüz karşıya yüklenen veriler yine de veri modeli ile ilişkilendirmek hakkında konuşun.

Bu öğreticide yeni bir kategori eklemek için bir web sayfasında oluşturacağız. Kategori s ad ve açıklama için metin kutuları ek olarak, iki dosya yükleme denetimleri yeni kategori s resmi için bir ve Broşürü için bir tane eklemek bu sayfayı gerekir. Karşıya yüklenen resim doğrudan yeni kayıttaki s depolanacak `Picture` sütun Broşürü kaydedilecek ancak `~/Brochures` yeni kayıt s kaydedilmiş dosyasının yolu klasörüyle `BrochurePath` sütun.

Bu yeni bir web sayfası oluşturmadan önce biz mimarisi güncelleştirmeniz gerekir. `CategoriesTableAdapter` s ana sorgu alamadı `Picture` sütun. Sonuç olarak, otomatik olarak oluşturulan `Insert` yöntemi yalnızca sahip girdileri `CategoryName`, `Description`, ve `BrochurePath` alanları. Bu nedenle, ek bir yöntem için dört ister TableAdapter oluşturmak ihtiyacımız `Categories` alanları. `CategoriesBLL` İş mantığı katmanı sınıfında güncelleştirilmesi de gerekir.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>1. adım: Ekleme bir`InsertWithPicture`yöntemi`CategoriesTableAdapter`

Ne zaman oluşturduğumuz `CategoriesTableAdapter` geri [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici, biz yapılandırılmış otomatik olarak oluşturmak için `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorguya dayalı. Ayrıca, biz yöntemleri oluşturulan DB doğrudan yaklaşım kullanmayı TableAdapter belirtildiği `Insert`, `Update`, ve `Delete`. Bu yöntem otomatik olarak oluşturulan yürütülemez `INSERT`, `UPDATE`, ve `DELETE` deyimleri ve sonuç olarak, ana sorgu tarafından döndürülen sütunlara göre giriş parametreleri kabul eder. İçinde [yüklenen dosyalar](uploading-files-cs.md) biz engagement'ta öğretici `CategoriesTableAdapter` kullanmak için ana sorgu s `BrochurePath` sütun.

Bu yana `CategoriesTableAdapter` s ana sorgu başvurmuyor `Picture` sütun, biz yeni bir kayıt eklemek ne için bir değer ile varolan bir kaydı güncelleştirmeye `Picture` sütun. Bu bilgileri yakalamak üzere ya da yeni bir yöntem özellikle ikili veri içeren bir kayıt eklemek için kullanılan TableAdapter içinde oluşturabilir veya biz otomatik olarak oluşturulan özelleştirebilirsiniz `INSERT` deyimi. Otomatik olarak oluşturulan özelleştirme sorun `INSERT` açıklamadır risk biz sihirbaz tarafından üzerine bizim özelleştirmeleri sahip. Örneğin, biz özelleştirilmiş düşünün `INSERT` kullanımını bildirimini `Picture` sütun. Bu TableAdapter s güncelleştirecektir `Insert` kategori s resim s ikili veriler için ek bir giriş parametresi dahil etmek için yöntem. Biz sonra bir yöntem bu DAL yöntemi kullanın ve sunu katmanı aracılığıyla bu BLL yöntemini çağırmak için iş mantığı katmanı olarak oluşturabilir ve her şeyi son derece çalışır. Diğer bir deyişle, zamana kadar biz TableAdapter TableAdapter Yapılandırma Sihirbazı ile yapılandırılmış. Sihirbazı Tamamlandı olarak bizim özelleştirmeleri `INSERT` deyimi üzerine, `Insert` yöntemi eski forma dönmek ve kodumuza artık derleme!

> [!NOTE]
> Geçici SQL deyimleri yerine saklı yordamları kullanma olduğunda, bu sıkıntı getirir sorunu olmayan kullanır. Bir sonraki öğretici, veri erişim katmanı'ndaki geçici SQL deyimleri yerine saklı yordamları kullanma inceleyeceksiniz.


Bu olasılığını önlemek için bunun yerine yeni bir yöntem için TableAdapter oluşturma s otomatik olarak oluşturulan SQL deyimlerini özelleştirme yerine çekiyorsanız olanak tanır. Adlı bu yöntem `InsertWithPicture`, değerleri kabul `CategoryName`, `Description`, `BrochurePath`, ve `Picture` sütunları ve yürütme bir `INSERT` tüm dört değerleri yeni kayıt depolayan deyimi.

Yazılan veri kümesi'ni açın ve Tasarımcısı'ndan sağ tıklayın `CategoriesTableAdapter` s üstbilgi ve bağlam menüsünden Sorgu Ekle'i seçin. Bu, bize nasıl TableAdapter sorgu veritabanına erişmek için kullanması isteyerek başlar TableAdapter sorgu Yapılandırma Sihirbazı başlatır. Use SQL deyimleri seçin ve İleri'yi tıklatın. Sonraki adım sorgu türü için oluşturulacak ister. Size yeni bir kayıt eklemek için bir sorgu oluşturma re itibaren `Categories` tablo, INSERT seçin ve İleri'yi tıklatın.


[![Ekle seçeneğini belirleyin](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Şekil 1**: Ekle seçeneğini belirleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Şimdi belirtmek ihtiyacımız `INSERT` SQL deyimi. Sihirbaz otomatik olarak öneren bir `INSERT` TableAdapter s ana sorguya karşılık gelen deyimi. Bu durumda, onu s bir `INSERT` ekler deyimi `CategoryName`, `Description`, ve `BrochurePath` değerleri. Update deyimi böylece `Picture` sütundur ile birlikte gelen bir `@Picture` parametresi şu şekilde:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Sihirbazın son ekranında bize yeni TableAdapter yöntem adı ister. Girin `InsertWithPicture` ve Son'u tıklatın.


[![Yeni TableAdapter yöntemi InsertWithPicture adı](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Şekil 2**: yeni TableAdapter yöntem adı `InsertWithPicture` ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>2. adım: iş mantığı katmanı güncelleştiriliyor

Sunu katmanı yalnızca iş mantığı katmanı ile arabirim yerine bu yana doğrudan veri erişim katmanı gitmesini atlayarak, yeni oluşturduğumuz DAL yöntemini çağıran bir BLL yöntemi oluşturmak ihtiyacımız (`InsertWithPicture`). Bu öğretici için bir yöntem oluşturma `CategoriesBLL` adlı sınıf `InsertWithPicture` giriş olarak üç kabul eden `string` s ve `byte` dizi. `string` Giriş parametreleri: s kategori adı, açıklama ve Broşürü dosya yolu, sırada `byte` dizidir kategori s resmi için ikili içerik. Aşağıdaki kodda gösterildiği gibi bu BLL yöntemi karşılık gelen DAL yöntemi çağırır:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Yazılan veri kümesi eklemeden önce kaydettiğinizden emin olun `InsertWithPicture` BLL yöntemi. Bu yana `CategoriesTableAdapter` sınıf kodu otomatik olarak oluşturulan yazılan veri kümesinde dayalı t tan yazılan veri kümesine değişikliklerinizi ilk kaydederseniz `Adapter` t won özelliği bilmeniz hakkında `InsertWithPicture` yöntemi.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>3. adım: mevcut kategorileri ve ikili verilerini listeleme

Bu öğreticide sisteme yeni bir kategori eklemek bir son kullanıcı veren bir sayfa resim ve Broşürü yeni kategori için sağlama oluşturacağız. İçinde [önceki öğretici](displaying-binary-data-in-the-data-web-controls-cs.md) biz GridView TemplateField ve ImageField ile her kategori s adını görüntülemek için kullanılan tanımlama, resmi ve onun Broşürü indirmek için bir bağlantı. Bu işlevselliği hem tüm mevcut kategorilerini listeler ve yenilerini oluşturulmasını sağlayan bir sayfa oluşturma, Bu öğretici için çoğaltma s olanak tanır.

Başlangıç açarak `DisplayOrDownload.aspx` gelen sayfa `BinaryData` klasör. Kaynak görünümüne gidin ve içinde yapıştırma GridView ve ObjectDataSource s tanımlayıcı sözdizimi, kopyalama `<asp:Content>` öğesinde `UploadInDetailsView.aspx`. Ayrıca, t tan unuttunuz üzerinden kopyalamak `GenerateBrochureLink` arka plandaki kod sınıfı yönteminden `DisplayOrDownload.aspx` için `UploadInDetailsView.aspx`.


[![DisplayOrDownload.aspx UploadInDetailsView.aspx bildirim temelli sözdizimine yapıştırın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Şekil 3**: bildirim temelli sözdiziminde kopyalayıp `DisplayOrDownload.aspx` için `UploadInDetailsView.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Bildirim temelli söz dizimini kopyaladıktan sonra ve `GenerateBrochureLink` üzerinden yönteme `UploadInDetailsView.aspx` sayfasında, her şeyi üzerinde doğru şekilde kopyalandığından emin olmak için bir tarayıcı aracılığıyla sayfasını görüntüleyin. Kategori s resmi yanı sıra Broşürü indirmek için bir bağlantı içeren sekiz kategorilerini liste GridView görmeniz gerekir.


[![İkili verileri birlikte her kategori görmelisiniz](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Şekil 4**:, gereken şimdi bkz her kategori boyunca ITS ikili verilerle ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>4. adım: Yapılandırma`CategoriesDataSource`desteği eklemek için

`CategoriesDataSource` ObjectDataSource tarafından kullanılan `Categories` GridView şu anda sağlamaz veri ekleme olanağı. Bu veri kaynağı denetimi ekleme desteklemek için eşlemek ihtiyacımız kendi `Insert` kendi nesnesini yönteminde yönteme `CategoriesBLL`. Özellikle, kendisine eşlemek istiyoruz `CategoriesBLL` geri 2. adımda, eklediğimiz yöntemi `InsertWithPicture`.

ObjectDataSource s akıllı etiket yapılandırma veri kaynağı bağlantısını tıklatarak başlatın. İlk ekranda birlikte çalışmak üzere yapılandırılmış veri kaynağı nesnesi gösterir `CategoriesBLL`. Bu ayarı olarak bırakın-olduğunu ve tanımlama veri yöntemleri ekranına öncelikli İleri'yi tıklatın. Taşıma Ekle sekmesi ve çekme `InsertWithPicture` aşağı açılan listeden yöntemi. Sihirbazı tamamlamak için Son'u tıklatın.


[![ObjectDataSource InsertWithPicture yöntemi kullanmak üzere yapılandırma](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanmak için yapılandırma `InsertWithPicture` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Sihirbazı tamamladığınızda, Visual Studio alanları Yenile'yi ve anahtarları istiyorsanız, Web verileri yeniden oluşturacak alanları denetimleri isteyebilir. Evet'i seçerseniz, alan özelleştirmeler yapmış olabileceğiniz üzerine yazar için Hayır'ı seçin.


Sihirbazı tamamladıktan sonra ObjectDataSource şimdi için bir değer içerir, `InsertMethod` özelliği yanı `InsertParameters` dört kategori sütunlar için aşağıdaki bildirim temelli biçimlendirme olarak gösterilmektedir:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>5. adım: ekleme arabirimi oluşturma

İlk olarak ele [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), DetailsView denetimi ekleme destekleyen bir veri kaynağı denetimi ile çalışırken, kullanılabilir yerleşik bir ekleme arabirim sağlar. Bu sayfaya hızla yeni bir kategori eklemek bir kullanıcının kalıcı olarak ekleme arabirimiyle kılacak GridView yukarıda DetailsView denetimini ekleme s olanak tanır. Yeni bir kategori DetailsView'da ekledikten sonra GridView altındaki otomatik olarak yenileyin ve yeni kategori görüntüler.

Araç kutusu ayarı GridView, yukarıda tasarımcıya'ndan bir DetailsView sürükleyerek başlangıç kendi `ID` özelliğine `NewCategory` ve Temizleme `Height` ve `Width` özellik değerleri. DetailsView s akıllı etiketten varolan bağlama `CategoriesDataSource` ve ardından ekleme etkinleştir onay kutusunu işaretleyin.


[![DetailsView CategoriesDataSource bağlayın ve eklemeyi etkinleştir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Şekil 6**: DetailsView'a bağlamak `CategoriesDataSource` ve etkinleştirme ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Kalıcı olarak ekleme arabirimiyle DetailsView'da işlemek için ayarlanmış kendi `DefaultMode` özelliğine `Insert`.

DetailsView beş BoundFields sahip olduğuna dikkat edin `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath` rağmen `CategoryID` çünkü BoundField ekleme arabiriminde değil çizilir kendi `InsertVisible` özelliği ayarlanmış `false`. Bu BoundFields var. tarafından döndürülecek olan sütunları olduklarından `GetCategories()` ObjectDataSource verilerini almak için çağırır olduğu yöntemi. Eklemek için ancak biz t kullanıcının için bir değer belirtmesine izin vermek istiyorsanız güncelleştireceğinizi `NumberOfProducts`. Ayrıca, biz bunları yanı sıra yeni kategori için bir resim karşıya Broşürü için bir PDF karşıya yüklemek izin vermeniz gerekir.

Kaldırma `NumberOfProducts` DetailsView değerlerinin ve ardından güncelleştirme BoundField'den `HeaderText` özelliklerini `CategoryName` ve `BrochurePath` BoundFields kategori ve Broşürü, sırasıyla. Ardından, dönüştürme `BrochurePath` bir TemplateField içine BoundField ve resim için yeni bir TemplateField eklemek bu yeni TemplateField vermiş bir `HeaderText` resmi değeri. Taşıma `Picture` onun arasında olacak şekilde TemplateField `BrochurePath` TemplateField ve CommandField.


![DetailsView CategoriesDataSource bağlayın ve eklemeyi etkinleştir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Şekil 7**: DetailsView'a bağlamak `CategoriesDataSource` ve eklemeyi etkinleştir


Dönüştürülürse `BrochurePath` TemplateField BoundField TemplateField alanları Düzenle iletişim kutusu üzerinden içine içeren bir `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate`. Yalnızca `InsertItemTemplate` olan gerekli, ancak, bu nedenle diğer iki şablonları Kaldır çekinmeyin. Bu noktada DetailsView s tanımlayıcı sözdizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Broşür ve resim alanları için dosya yükleme denetimler ekleme

Şu anda `BrochurePath` TemplateField s `InsertItemTemplate` bir metin kutusu içeren sırada `Picture` TemplateField şablonlardan içermiyor. Bu iki TemplateField s güncelleştirmek ihtiyacımız `InsertItemTemplate` s dosya yükleme denetimleri kullanın.

DetailsView s akıllı etiket Şablonları Düzenle seçeneğini seçin ve ardından `BrochurePath` TemplateField s `InsertItemTemplate` aşağı açılan listeden. TextBox kaldırın ve sonra dosya yükleme Denetim Araç Kutusu'ndan şablona sürükleyin. Dosya yükleme denetimini s ayarlama `ID` için `BrochureUpload`. Benzer şekilde, bir dosya yükleme denetimine ekleme `Picture` TemplateField s `InsertItemTemplate`. Bu dosya yükleme denetimini s ayarlama `ID` için `PictureUpload`.


[![InsertItemTemplate dosya yükleme denetim ekleme](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Şekil 8**: dosya yükleme denetimine ekleme `InsertItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Bu eklemeleri yaptıktan sonra iki TemplateField s Tanımlayıcı Sözdizimi olacaktır:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Bir kullanıcı yeni bir kategori eklediğinde, Broşürü ve resim doğru dosya türü olduğundan emin olmak istiyoruz. Broşürü, kullanıcı bir PDF sağlamanız gerekir. Bir görüntü dosyası karşıya yüklemek için kullanıcı ihtiyacımız için resim, ancak biz izin *herhangi* görüntü dosyası veya yalnızca görüntü dosyaları GIF veya JPG formatından gibi belirli bir türdeki? Farklı dosya türleri için izin vermek üzere d genişletmek ihtiyacımız `Categories` dosya türü yakalar ve böylece bu tür istemciye gönderilen bir sütun eklemek için şema `Response.ContentType` içinde `DisplayCategoryPicture.aspx`. Biz t güncelleştireceğinizi beri böyle bir sütun, kullanıcıların yalnızca belirli görüntü dosya türü sağlamayı kısıtlamak akıllıca olur. `Categories` s tablosunda var olan görüntüleri bit eşlemler, ancak jpg formatından web üzerinden sunulan görüntüleri için daha uygun bir dosya biçimi.

Dosya türü yanlış bir kullanıcı yüklerse, kimliğinizi Ekle iptal etme ve sorun belirten bir ileti görüntüleme gerekiyor. Bir etiket Web denetimi DetailsView altına ekleyin. Ayarlama, `ID` özelliğine `UploadWarning`, kullanıma açık kendi `Text` özelliği ayarlamak `CssClass` uyarı özelliğine ve `Visible` ve `EnableViewState` özelliklerine `false`. `Warning` CSS sınıfı tanımlanmış `Styles.css` ve bir büyük, kırmızı, italik, kalın yazı tipi metinde işler.

> [!NOTE]
> İdeal olarak, `CategoryName` ve `Description` TemplateFields ve özelleştirilmiş ekleme arabirimlerini BoundFields'in dönüştürülmesi. `Description` Arabirimi, örneğin, ekleme olasılıkla daha uygun çok satırlı metin kutusu. Ve bu yana `CategoryName` sütun kabul etmiyor `NULL` değerleri, kullanıcı için yeni bir kategori s ad değeri sağlar emin olmak için bir RequiredFieldValidator eklenmelidir. Bu adımları bir alıştırma olarak okuyucuya bırakılır. Geri başvurmak [veri değişikliği arabirimi özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) veri değişikliği arabirimleri program.cs'ye derinlemesine bir bakış için.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>6. adım: karşıya yüklenen Broşürü Web sunucusu s dosya sistemine kaydetme

Kullanıcı için yeni bir kategori değerleri girer ve Ekle düğmesine tıkladığında ekleme iş akışı açılan ve geri gönderimin oluşur. İlk olarak, DetailsView s [ `ItemInserting` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) etkinleşir. İleri, ObjectDataSource s `Insert()` yöntemi çağrıldığında, hangi eklenmekte olan yeni bir kayıt sonuçlanır `Categories` tablo. Bundan sonra DetailsView s [ `ItemInserted` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) etkinleşir.

ObjectDataSource s önce `Insert()` yöntemi çağrıldığında, biz önce uygun dosya türlerini kullanıcı tarafından yüklenen sağlayın ve ardından web sunucusu s dosya sistemine PDF Broşürü kaydetmeniz gerekir. DetailsView s için bir olay işleyicisi oluşturun `ItemInserting` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Olay işleyicisi başvurarak başlatır `BrochureUpload` DetailsView s şablonlardan dosya yükleme denetim. Ardından, bir Broşürü karşıya yüklediyseniz, karşıya yüklenen dosya s uzantısı incelenir. Uzantı değilse. PDF, sonra bir uyarı görüntülenir, ekleme iptal edilir ve olay işleyicisi yürütülmesi biter.

> [!NOTE]
> Karşıya yüklenen dosya s uzantısına bağlı karşıya yüklenen dosyanın bir PDF belgesini olduğunu sağlamaya yönelik bir sure-fire teknik değil. Kullanıcının geçerli bir PDF belgesini uzantılı olabilir `.Brochure`, veya olmayan PDF belgesini alınır ve verilen bir `.pdf` uzantısı. Dosya s ikili içerik daha conclusively dosya türünü doğrulamak için program aracılığıyla incelenmesi gerekir. Bu tür kapsamlı bir yaklaşım genellikle yine de gereğinden fazla bağlıdır; Uzantı denetimi çoğu senaryoları için yeterli olur.


' Da anlatıldığı gibi [yüklenen dosyalar](uploading-files-cs.md) öğretici, bu bir kullanıcı s karşıya yükleme başka bir s üzerine yazmaz şekilde dosya sistemine kaydetme dosyaları dikkatli'nin alınması gerekir. Bu öğretici için size aynı adı yüklenen dosya kullanmayı dener. Zaten varsa bir dosyada `~/Brochures` dizin, aynı dosya adıyla, ancak biz append sonunda bir sayı benzersiz bir ad bulunana kadar. Örneğin, kullanıcı adında bir Broşürü dosya karşıya yükleme `Meats.pdf`, adında bir dosya yoktur ancak `Meats.pdf` içinde `~/Brochures` klasörü, biz değiştirmek için kaydedilen dosya adı `Meats-1.pdf`. Varsa, biz deneyeceksiniz `Meats-2.pdf`ve benzeri benzersiz bir dosya adı bulunana kadar.

Aşağıdaki kod [ `File.Exists(path)` yöntemi](https://msdn.microsoft.com/en-us/library/system.io.file.exists.aspx) belirtilen dosya adıyla bir dosya zaten olup olmadığını belirlemek için. Bu durumda, yeni dosya adları Broşürü için çakışma bulunana kadar denemeye devam eder.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Dosya geçerli bir dosya adı bulundu sonra dosya sistemi ve ObjectDataSource s kaydedilmesi gerekiyor `brochurePath``InsertParameter` değeri bu dosya adı veritabanına yazılan böylece güncelleştirilmesi gerekiyor. Geri gördüğümüz gibi *yüklenen dosyalar* öğretici, dosyayı kaydedilebilir dosya yükleme denetimini s kullanarak `SaveAs(path)` yöntemi. ObjectDataSource s güncelleştirmek için `brochurePath` parametresi, kullanım `e.Values` koleksiyonu.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>7. adım: karşıya yüklenen resim veritabanına kaydetme

Karşıya yüklenen resim yeni depolamak için `Categories` kayıt, karşıya yüklenen ikili içerik ObjectDataSource s atamak ihtiyacımız `picture` DetailsView s parametresinde `ItemInserting` olay. Ancak, biz bu atama yapmadan önce biz öncelikle karşıya yüklenen resim JPG ve diğer olmayan bazı görüntü türü olduğundan emin olun gerekir. Adım 6 olduğu gibi türünü onaylaması için karşıya yüklenen resim s dosya uzantısını kullanan s olanak tanır.

Sırada `Categories` tablosu verir `NULL` değerleri `Picture` sütun, şu anda tüm kategorileri sahip bir resim. Bu sayfadan yeni bir kategori eklerken resim sağlamak için kullanıcı zorunlu s olanak tanır. Resim karşıya yüklendi ve uygun bir uzantısına sahip olduğundan emin olmak için aşağıdaki kodu denetler.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Bu kod yerleştirilmelidir *önce* adım 6 koddan böylece resim karşıya yükleme ile ilgili bir sorun varsa, Broşürü dosya için dosya sistemi kaydedilmeden önce olay işleyicisi sonlandırılacak.

Uygun bir dosyayı karşıya varsayılarak, karşıya yüklenen ikili içerik aşağıdaki kod satırını resim parametre s değeriyle atayın:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Tam`ItemInserting`olay işleyicisi

Bütünlük açısından işte `ItemInserting` okumalıdır olay işleyicisi:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>8. adım: Düzelttikten`DisplayCategoryPicture.aspx`sayfası

Let s ekleme arabirimin dışına sınamak için bir dakikanızı alın ve `ItemInserting` son birkaç adımda oluşturulan olay işleyicisi. Ziyaret `UploadInDetailsView.aspx` sayfa tarayıcı ve bir kategoriye Ekle, ancak resmi atlayın girişimi ya da olmayan JPG resim ya da bir PDF olmayan broşür belirtin. Bu durumların herhangi birinde, bir hata iletisi görüntülenir ve ekleme iş akışı iptal edildi.


[![Bir uyarı iletisi görüntülenir geçersiz bir dosya türü karşıya ise](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Şekil 9**: bir uyarı iletisi olup görüntülenen geçersiz bir dosya türü karşıya ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Bir kez Broşürü alan boş bırakılırsa sayfa karşıya yüklenecek resim gerektiriyorsa ve kazanılan t PDF olmayan veya JPG olmayan dosyaları kabul, geçerli bir JPG resim ile yeni bir kategori ekleyin doğrulanmıştır. Ekle düğmesine tıkladıktan sonra sayfanın geri gönderilir ve yeni bir kayıt eklenecek `Categories` doğrudan veritabanında depolanan karşıya yüklenen görüntü s ikili içeriğini içeren tablo. GridView güncelleştirilir ve yeni eklenen kategorisi için bir satır gösterir, ancak, Şekil 10 gösterildiği gibi yeni bir kategori s resim doğru işlenmez.


[![Yeni kategori resim gösterilmez s](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Şekil 10**: resim görüntülenmez yeni kategori s ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Yeni bir resim görüntülenmez neden olduğundan `DisplayCategoryPicture.aspx` belirtilen kategori s resim döndürür sayfa OLE üstbilginin bit eşlemler işlemek üzere yapılandırılmış. Bu 78 bayt üst öğesinden yapılandırıldıktan `Picture` gönderilmeden önce s sütunu ikili içeriği istemciye yedekleyin. Ancak biz yalnızca karşıya JPG dosyası yeni kategori için bu OLE üstbilgi yok; Bu nedenle, geçerli, gerekli bayt görüntü s ikili verileri kaldırılıyor.

Şimdi iki bit eşlemler OLE üstbilgileri ve jpg formatından içinde olduğundan `Categories` tablo ihtiyacımız güncelleştirmek `DisplayCategoryPicture.aspx` böylece özgün sekiz kategorilerini çıkarma OLE üstbilgi ve bu yeni kategori kayıtlarını çıkarma atlar. Bizim sonraki öğreticide biz var olan bir kayıt s görüntüsünü güncelleştirmek nasıl inceleyeceğiz ve jpg formatından; böylece tüm eski kategori resimler güncelleştirme yapacaksınız. Şimdilik, yine de aşağıdaki kodda kullanmak `DisplayCategoryPicture.aspx` OLE üst bilgileri yalnızca özgün sekiz kategorilerin Şerit için:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Bu değişiklikle, JPG resim artık doğru GridView işlenir.


[![Yeni kategoriler JPG görüntülerde doğru işlenmiş olan](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Şekil 11**: JPG görüntüleri yeni kategori için olan doğru çizilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>9. adım: bir özel durum karşısında Broşürü silme

Web sunucusu s dosya sisteminde ikili veri depolamanın zorluklardan biri, onu veri modeli ve ikili verileri arasında bir bağlantı kesme içermesidir. Bu nedenle, bir kaydı silindiğinde, dosya sistemindeki karşılık gelen ikili veriler ayrıca kaldırılmalıdır. Bu, de eklerken oyuna gelebilir. Aşağıdaki senaryoyu düşünün: geçerli resim ve Broşürü belirten yeni bir kategori bir kullanıcı ekler. Geri gönderimin oluştuğu için Ekle düğmesini tıklatın ve DetailsView s `ItemInserting` Broşürü web sunucusu s dosya sistemine kaydetme olay etkinleşir. İleri, ObjectDataSource s `Insert()` yöntemi çağrıldığında, çağıran `CategoriesBLL` s sınıfı `InsertWithPicture` çağırır yöntemi `CategoriesTableAdapter` s `InsertWithPicture` yöntemi.

Şimdi, veritabanı çevrimdışıysa ne olur veya bir hata olup olmadığını `INSERT` SQL deyimini? Yeni bir kategori satır veritabanına eklenecek şekilde açıkça ekleme başarısız olur. Ancak hala web sunucusu s dosya sisteminde durduğunu karşıya yüklenen Broşürü dosya sahibiz! Bu dosya ekleme iş akışı sırasında bir özel durum karşısında silinmesi gerekir.

Daha önce anlatıldığı gibi [işleme BLL - ve ASP.NET sayfası DAL düzeyi durumlar](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) onu kabarcıklanma yukarı çeşitli katmanından mimarisi derinliklerine içinde gelen bir özel durum olduğunda öğretici. Sunu katmanında, biz bir özel durum DetailsView s'den gerçekleşip gerçekleşmediğini belirleyebilirsiniz `ItemInserted` olay. Bu olay işleyicisi ObjectDataSource s değerlerini de sağlar `InsertParameters`. Bu nedenle, bir olay işleyicisi için oluşturabiliriz `ItemInserted` denetler bir özel durum ise ve bu durumda, olay siler ObjectDataSource s tarafından belirtilen dosya `brochurePath` parametre:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Özet

İkili veriler dahil kayıtları eklemek için web tabanlı bir arabirim sağlamak için gerçekleştirilmesi gereken adımlar vardır. İkili veriler doğrudan veritabanına depolanan olasılığını mimarisi güncelleştirmek ikili veri burada eklenmekte durumu işlemek için özel yöntemler ekleme gerekir demektir. Mimari güncelleştirilmiş sonra sonraki adım bir dosya yükleme denetimini her ikili veri alanı eklemek için özelleştirilmiş bir DetailsView kullanılarak gerçekleştirilebilir ekleme arabirimi oluşturuyor. Karşıya yüklenen veriler sonra web sunucusu s dosya sistemine kaydedilmiş veya bir veri kaynağı parametresi DetailsView s atanan `ItemInserting` olay işleyicisi.

İkili verileri dosya sistemine kaydetme verileri doğrudan veritabanına kaydetme'den daha fazla planlama gerektirir. Başka bir s üzerine bir kullanıcı s karşıya yükleme önlemek için bir adlandırma şeması seçilmelidir. Ayrıca, veritabanı ekleme başarısız olursa yüklenen dosya silmek için ek adımlar izlenmelidir.

Biz şimdi broşürlerde ve resim ancak biz sistemiyle yeni kategorileri eklemek için var olan bir kategori s ikili verileri güncelleştirmek nasıl ya da doğru olarak silinen bir kategori için ikili verileri kaldırmak nasıl henüz bakmak için ve sahipsiniz. Sonraki öğreticide Biz bu iki konuları ele alacağız.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Dave Gardner, Teresa Murphy ve Bernadette Leigh yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](displaying-binary-data-in-the-data-web-controls-cs.md)
[sonraki](updating-and-deleting-existing-binary-data-cs.md)
