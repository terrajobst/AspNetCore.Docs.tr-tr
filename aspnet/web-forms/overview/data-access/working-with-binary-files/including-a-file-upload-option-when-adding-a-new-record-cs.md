---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Bir dosya dahil olmak üzere Karşıya Yükle seçeneği (C#) yeni kayıt eklerken | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, hem metin verileri girin ve ikili dosyaları karşıya yükleme kullanıcıya izin veren bir Web arabirimi oluşturma işlemi gösterilmektedir. Seçenekleri kullanılabilir t göstermek için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: c109c493875c08dc1057fc6a244f58806b6bae46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366112"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>(C#) yeni kayıt eklerken karşıya dosya yükleme seçeneği
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) veya [PDF olarak indirin](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Bu öğreticide, hem metin verileri girin ve ikili dosyaları karşıya yükleme kullanıcıya izin veren bir Web arabirimi oluşturma işlemi gösterilmektedir. Diğer dosya sisteminde depolanma ikili verileri depolamak için kullanılabilir seçenekleri göstermek için bir dosya veritabanında kaydedilir.


## <a name="introduction"></a>Giriş

Önceki iki öğreticilerde biz incelediniz dosyalarını istemciden web sunucusuna gönderin ve bu ikili verileri bir veri W sunma hakkında gördüğünüz için FileUpload denetim kullanmayı göz attınız uygulama s veri modeli ile ilişkili olan ikili verileri depolamak için kullanılan teknikleri EB denetimi. Biz ve henüz karşıya yüklenen veriler yine de veri modeli ile ilişkilendirme hakkında konuşacağız.

Bu öğreticide yeni bir kategori eklemek için bir web sayfası oluşturacağız. Kategori adı ve açıklaması için metin kutuları ek olarak, bu sayfa iki FileUpload denetimleri yeni bir kategori s resim için ve biri Broşürü eklemeniz gerekir. Karşıya yüklenen resim doğrudan yeni kayıttaki s depolanacak `Picture` sütun Broşürü kaydedilir ancak `~/Brochures` klasörü yeni kayıttaki s kaydettiğiniz dosyayı yoluyla `BrochurePath` sütun.

Bu yeni bir web sayfası oluşturmadan önce biz mimarisi güncelleştirmeniz gerekir. `CategoriesTableAdapter` s ana sorgu alamadı `Picture` sütun. Sonuç olarak, otomatik olarak oluşturulan `Insert` yöntemi yalnızca girişleri için sahip `CategoryName`, `Description`, ve `BrochurePath` alanları. Bu nedenle, ek bir yöntem için dört ister TableAdapter oluşturmak ihtiyacımız `Categories` alanları. `CategoriesBLL` İş mantığı katmanı sınıfında güncelleştirilmesi de gerekir.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>1. adım: Ekleme bir`InsertWithPicture`yöntemi`CategoriesTableAdapter`

Ne zaman oluşturduğumuz `CategoriesTableAdapter` geri [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) Öğreticisi, biz yapılandırılmış otomatik olarak oluşturmak için `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorgusuna dayalı. Ayrıca, biz TableAdapter yöntemleri oluşturulan DB doğrudan bir yaklaşım kullanmak istemiyorsunuz belirtildiği `Insert`, `Update`, ve `Delete`. Otomatik olarak oluşturulan bu yöntemleri yürütme `INSERT`, `UPDATE`, ve `DELETE` deyimleri ve sonuç olarak, ana sorgu tarafından döndürülen sütunlara göre giriş parametrelerini kabul edin. İçinde [yüklenen dosyalar](uploading-files-cs.md) biz genişletilmiş öğretici `CategoriesTableAdapter` kullanılacak ana sorgu s `BrochurePath` sütun.

Bu yana `CategoriesTableAdapter` s ana sorgu başvurmuyor `Picture` sütun, biz yeni bir kayıt eklemek ne için bir değer ile varolan bir kaydı güncelleştirme `Picture` sütun. Bu bilgileri yakalamak için ya da yeni bir yöntem de özellikle ikili veri içeren bir kayıt eklemek için kullanılan TableAdapter oluşturabiliriz ya da biz otomatik olarak oluşturulan özelleştirebilirsiniz `INSERT` deyimi. Otomatik olarak oluşturulan özelleştirme sorun `INSERT` deyimi, biz sihirbaz tarafından üzerine bizim özelleştirmeleri bağlantıyla karşılaşabilirsiniz. Örneğin, şu özelleştirilmiş Imagine `INSERT` kullanımını bildirimini `Picture` sütun. Bu TableAdapter s güncelleştirme `Insert` kategori s resim s ikili veriler için ek giriş parametresi dahil etmek için yöntemi. Ardından bir yöntem bu DAL yöntemi kullanın ve bir sunu katmanı aracılığıyla bu BLL yöntemi çağırmak için iş mantığı katmanı içinde oluşturabilir ve her şeyi son derece işe yarar. Diğer bir deyişle, sonraki açışınızda kadar biz TableAdapter'ı TableAdapter Yapılandırma Sihirbazı ile yapılandırılmış. Sihirbaz Tamamlandı hemen sonra bizim özelleştirmeleri `INSERT` deyimi üzerine, `Insert` yöntemi, eski forma geri dönmesi ve kodumuz artık derlenir!

> [!NOTE]
> Saklı yordamlar yerine geçici SQL deyimlerini kullanarak zamandır, bu sıkıntı getirir sorunu olmayan kullanır. Bir sonraki öğreticide, veri erişim katmanındaki saklı yordamlar yerine geçici SQL deyimlerini kullanarak inceleyeceksiniz.


Bu olası önlemek için bunun yerine yeni bir yöntem için TableAdapter oluşturma s otomatik olarak oluşturulan SQL deyimlerini özelleştirme yerine zahmetine katlanmadan olanak tanır. Adlı, bu yöntem `InsertWithPicture`, değerleri için kabul `CategoryName`, `Description`, `BrochurePath`, ve `Picture` sütunları ve yürütme bir `INSERT` tüm dört değer, yeni bir kayıt depolar deyimi.

Türü belirtilmiş veri kümesi'ni açın ve sağ Tasarımcısı'ndan `CategoriesTableAdapter` s üstbilgi ve bağlam menüsünden Sorgu Ekle'ı seçin. Bu, bize TableAdapter sorgusu veritabanına nasıl erişmeli isteyerek başlar TableAdapter sorgu Yapılandırma Sihirbazı başlatılır. SQL deyimi Kullan'ı seçip İleri'ye tıklayın. Sonraki adım sorgu türü için oluşturulmasını ister. Size yeni bir kayıt eklemek için sorgu oluşturma re beri `Categories` Tablo Ekle öğesini seçin ve İleri'ye tıklayın.


[![Ekle seçeneğini belirleyin](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Şekil 1**: Ekle seçeneğini belirleyin ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


Artık belirtmek ihtiyacımız `INSERT` SQL deyimi. Sihirbaz otomatik olarak öneren bir `INSERT` TableAdapter s ana sorguda karşılık gelen deyimi. Bu durumda, s bir `INSERT` ekler deyimi `CategoryName`, `Description`, ve `BrochurePath` değerleri. Güncelleştirme bildirimi böylece `Picture` sütundur ile birlikte dahil edilen bir `@Picture` parametresi, şu şekilde:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Sihirbazın son ekran bize yeni TableAdapter yöntem adı ister. Girin `InsertWithPicture` ve Son'a tıklayın.


[![Yeni bir TableAdapter yöntemi InsertWithPicture adı](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Şekil 2**: yeni bir TableAdapter yöntem adı `InsertWithPicture` ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>2. adım: iş mantığı katmanı güncelleştiriliyor

Sunu katmanı yalnızca iş mantığı katmanı ile arabirim yerine bu yana doğrudan veri erişim katmanına yere atlama, yeni oluşturduğumuz DAL yöntemini çağıran bir BLL yöntemi oluşturmak ihtiyacımız (`InsertWithPicture`). Bu öğreticide, bir yöntem oluşturma `CategoriesBLL` adlı sınıfı `InsertWithPicture` giriş olarak üç kabul eden `string` s ve `byte` dizisi. `string` Giriş parametreleri: s kategori adı, açıklama ve Broşürü dosya yolu için sırada `byte` kategori s resmi için ikili içerik dizisidir. Aşağıdaki kodda gösterildiği gibi bu BLL yöntemi karşılık gelen DAL yöntemini çağırır:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Türü belirtilmiş veri kümesi eklemeden önce kaydettiğinizden emin olun `InsertWithPicture` BLL için yöntemi. Bu yana `CategoriesTableAdapter` sınıf kodu otomatik olarak oluşturulan türü belirtilmiş veri kümesine bağlı, t don türü belirtilmiş veri kümesi için önce yaptığınız değişiklikleri kaydederseniz, `Adapter` t kazanılan özelliği bilmeniz hakkında `InsertWithPicture` yöntemi.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>3. adım: mevcut kategorileri ve ikili verileri listeleme

Bu öğreticide sisteme yeni bir kategori eklemek bir son kullanıcı veren bir sayfa yeni kategori için bir resim ve Broşürü sağlama oluşturacağız. İçinde [önceki öğretici](displaying-binary-data-in-the-data-web-controls-cs.md) GridView TemplateField ve ImageField ile her kategori s adını görüntülemek için kullandığımız resim, açıklama ve kendi Broşürü indirmek için bir bağlantı. Bu öğretici hem var olan tüm kategorileri listeler ve yenilerini oluşturulmasını sağlayan bir sayfa oluşturmak için bu işlevi çoğaltmak s olanak tanır.

Başlangıç açarak `DisplayOrDownload.aspx` gelen sayfasında `BinaryData` klasör. Kaynak görünümüne gidin ve içine yapıştırma GridView ve ObjectDataSource s bildirim temelli söz dizimini kopyalayın `<asp:Content>` öğesinde `UploadInDetailsView.aspx`. Ayrıca, rsquo unutmayın kopyalayabilirsiniz `GenerateBrochureLink` arka plan kod sınıfı yönteminden `DisplayOrDownload.aspx` için `UploadInDetailsView.aspx`.


[![DisplayOrDownload.aspx UploadInDetailsView.aspx bildirim temelli sözdizimine yapıştırın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Şekil 3**: gelen bildirim temelli söz dizimini kopyalayıp `DisplayOrDownload.aspx` için `UploadInDetailsView.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Bildirim temelli söz dizimi kopyaladıktan sonra ve `GenerateBrochureLink` üzerinden yönteme `UploadInDetailsView.aspx` sayfasında, her şeyi üzerinde doğru şekilde kopyalandığından emin olmak için bir tarayıcı aracılığıyla sayfada görüntüleyin. Kategori s resmi yanı sıra Broşürü indirmek için bir bağlantı içeren sekiz kategorileri listeleme GridView görmeniz gerekir.


[![Artık her kategorinin kendi ikili verilerle birlikte görmeniz gerekir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Şekil 4**:, gereken artık bkz her kategori boyunca kendi ikili verilerle ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>4. adım: Yapılandırma`CategoriesDataSource`için destek ekleme

`CategoriesDataSource` ObjectDataSource tarafından kullanılan `Categories` GridView şu anda sağlamaz veri ekleme olanağı. Bu veri kaynağı denetimi ile ekleme desteği için eşlemeniz gerekiyor, `Insert` , temel alınan nesnede bir yöntemi için yöntem `CategoriesBLL`. Özellikle, kendisine eşlemek istediğimiz `CategoriesBLL` geri 2. adımda eklediğimiz yöntem `InsertWithPicture`.

ObjectDataSource s akıllı etiketinde yapılandırma veri kaynağı bağlantısını tıklatarak başlatın. Birlikte çalışmak üzere yapılandırılmış veri kaynağı nesnesinin ilk ekran gösterilmektedir `CategoriesBLL`. Bu ayarı olarak bırakın-olduğu ve öncelikli veri yöntemleri tanımlamak ekranına İleri'yi tıklatın. Ekle sekmesine Taşı ve çekme `InsertWithPicture` aşağı açılan listeden yöntemi. Sihirbazı tamamlamak için Son'u tıklatın.


[![ObjectDataSource InsertWithPicture yöntemi kullanmak üzere yapılandırma](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Şekil 5**: kullanılacak ObjectDataSource yapılandırma `InsertWithPicture` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Sihirbazı tamamladığınızda, Visual Studio Web verileri yeniden oluşturulacak alanları Yenile'yi ve anahtarları istiyorsanız, denetimleri isteyebilir. Evet'i seçerseniz, yaptığınız herhangi bir alan özelleştirme üzerine yazılacağından, Hayır'ı seçin.


Sihirbazı tamamladıktan sonra ObjectDataSource artık için bir değer içerir, `InsertMethod` özelliği olarak `InsertParameters` dört Kategori sütunları için aşağıdaki bildirim temelli biçimlendirmesi olarak gösterilmektedir:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>5. adım: ekleme arabirimi oluşturma

İlk olarak ele [, bir genel bakış ekleme, güncelleştirme ve silme veri](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), DetailsView denetiminde ekleme destekleyen bir veri kaynağı denetimi ile çalışırken yararlanılabilir yerleşik bir ekleme arabirimi sağlar. Bu sayfa ekleme arabirimiyle kalıcı olarak işlenir GridView hızla yeni bir kategori eklemek bir kullanıcının yukarıda bir DetailsView denetimi eklemek s olanak tanır. Yeni bir kategori içinde DetailsView ekleme sırasında altındaki GridView otomatik olarak yenileyin ve yeni kategori görüntüle.

Bir DetailsView ayarı GridView yukarıda tasarımcıya Toolbox'tan sürükleyerek başlangıç alt `ID` özelliğini `NewCategory` ve Temizleme `Height` ve `Width` özellik değerlerini. DetailsView s akıllı etiketten varolan bağlama `CategoriesDataSource` ve ardından eklemeyi etkinleştir onay kutusunu işaretleyin.


[![DetailsView CategoriesDataSource için bağlama ve eklemeyi etkinleştir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Şekil 6**: DetailsView için bağlama `CategoriesDataSource` ve etkinleştirme ekleme ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


DetailsView ekleme kendi arabiriminde kalıcı olarak işlemek için ayarlanmış kendi `DefaultMode` özelliğini `Insert`.

DetailsView beş BoundFields olduğuna dikkat edin `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath` rağmen `CategoryID` çünkü BoundField ekleme arabiriminde değil işlenen kendi `InsertVisible` özelliği `false`. Bu BoundFields var çünkü bunlar tarafından döndürülecek olan sütunları `GetCategories()` ObjectDataSource verilerini almak için çağırır olan yöntem. Eklemek için ancak biz t için bir değer belirtmesine izin vermek istiyorsanız ki `NumberOfProducts`. Ayrıca, yanı sıra yeni kategori için bir resim karşıya Broşürü bir PDF dosyasını karşıya yükleme sağlamak ihtiyacımız var.

Kaldırma `NumberOfProducts` güncelleştirin ve DetailsView toptan BoundField'den `HeaderText` özelliklerini `CategoryName` ve `BrochurePath` BoundFields kategorisi ve Broşürü, sırasıyla. Ardından, dönüştürme `BrochurePath` BoundField bir TemplateField uygulamasına ve bu yeni TemplateField vererek yeni bir TemplateField resim ekleme bir `HeaderText` resmi değeri. Taşıma `Picture` BT'nin arasında bu nedenle TemplateField `BrochurePath` TemplateField ve CommandField.


![DetailsView CategoriesDataSource için bağlama ve eklemeyi etkinleştir](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Şekil 7**: DetailsView için bağlama `CategoriesDataSource` ve eklemeyi etkinleştir


Dönüştürdüyseniz `BrochurePath` TemplateField BoundField alanları Düzenle iletişim kutusu aracılığıyla bir TemplateField halinde içeren bir `ItemTemplate`, `EditItemTemplate`, ve `InsertItemTemplate`. Yalnızca `InsertItemTemplate` olan gerekli, ancak, bu nedenle diğer iki şablonları kaldırma çekinmeyin. Bu noktada, DetailsView s bildirim temelli söz dizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Broşürü ve resim alanları için FileUpload denetimleri ekleme

Şu anda, `BrochurePath` TemplateField s `InsertItemTemplate` bir TextBox içerir ancak `Picture` TemplateField hiçbir şablon içermiyor. Bu iki TemplateField s güncelleştirmek ihtiyacımız `InsertItemTemplate` s FileUpload denetimleri kullanın.

DetailsView s akıllı etiket Şablonları Düzenle seçeneğini belirleyin ve ardından `BrochurePath` TemplateField s `InsertItemTemplate` aşağı açılan listeden. TextBox kaldırın ve ardından FileUpload denetimi araç kutusundan şablona sürükleyin. S FileUpload denetimi ayarlama `ID` için `BrochureUpload`. Benzer şekilde, bir FileUpload denetimine ekleme `Picture` TemplateField s `InsertItemTemplate`. Bu FileUpload denetimi s ayarlama `ID` için `PictureUpload`.


[![InsertItemTemplate için FileUpload denetim ekleme](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Şekil 8**: FileUpload denetimine ekleme `InsertItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Bu eklemeler yaptıktan sonra iki TemplateField s bildirim temelli sözdizimi olacaktır:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Bir kullanıcı yeni bir kategori eklediğinde, resim ve Broşürü doğru dosya türü olduğunu emin olmak istiyoruz. Broşürlerde, kullanıcı bir PDF sağlamanız gerekir. Kullanıcının bir resim dosyası karşıya resim için ihtiyacımız ancak biz izin *herhangi* görüntü dosyası veya yalnızca resim dosyalarına GIF veya JPG formatından gibi belirli bir türün? Farklı dosya türleri için izin vermek için d genişletmek gerekiyor `Categories` dosya türü yakalar ve böylece bu tür istemciye gönderilen bir sütun eklemek için şema `Response.ContentType` içinde `DisplayCategoryPicture.aspx`. Biz t ki bu yana bu tür bir sütuna sahip, kullanıcıların yalnızca özel görüntü dosyası türü sağlamayı kısıtlamak akıllıca olur. `Categories` Tablo s var olan görüntülerden bit eşlemler, ancak jpg formatından web üzerinden sunulan görüntüler için daha uygun bir dosya biçimi.

Bir kullanıcı yanlış dosya türüne yükler, INSERT iptal edin ve sorun belirten bir ileti görüntülemek ihtiyacımız var. Bir etiket Web denetimi DetailsView altına ekleyin. Ayarlayın, `ID` özelliğini `UploadWarning`kullanıma temizleyin, `Text` özelliği ayarlamak `CssClass` uyarı özelliğini ve `Visible` ve `EnableViewState` özelliklerine `false`. `Warning` CSS sınıfı içinde tanımlanan `Styles.css` ve metin büyük, red, italik, kalın yazı tipinde işler.

> [!NOTE]
> İdeal olarak, `CategoryName` ve `Description` TemplateField ve özelleştirilmiş ekleme arabirimlerini BoundFields'in dönüştürülmesi. `Description` Arabirimi, örneğin, ekleme büyük olasılıkla daha uygun çok satırlı bir metin. Ve bu yana `CategoryName` sütun kabul etmez `NULL` değerleri, kullanıcı, yeni kategori s adı için bir değer sağlar emin olmak için bir RequiredFieldValidator eklenmelidir. Bu adımları bir alıştırma olarak okuyucu bırakılır. Kiracıurl [veri değişikliği arabirimini özelleştirme](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) veri değişikliği arabirimleri deneyimlerinizi ayrıntılı bir bakış için.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>6. adım: karşıya yüklenen Broşürü Web sunucusu s dosya sistemine kaydetme

Kullanıcı için yeni bir kategori değerler girer ve Ekle düğmesine tıkladığında, bir geri gönderme gerçekleşir ve açılan ekleme iş akışı. İlk olarak DetailsView s [ `ItemInserting` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) ateşlenir. Ardından, ObjectDataSource s `Insert()` yöntemi çağrılır, eklenen yeni bir kayıt içindeki sonuçlanır `Categories` tablo. Bundan sonra DetailsView s [ `ItemInserted` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) ateşlenir.

ObjectDataSource s önce `Insert()` yöntemi çağrıldığında, biz önce uygun dosya türleri kullanıcı tarafından yüklenen sağlayın ve ardından web sunucusu s dosya sistemine PDF Broşürü kaydetmeniz gerekir. DetailsView s için bir olay işleyicisi oluşturun `ItemInserting` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Olay işleyicisi başvurarak başlar `BrochureUpload` DetailsView s şablonlardan FileUpload denetimi. Ardından, broşürlerde karşıya yüklediyseniz, karşıya yüklenen dosya s uzantısı incelenir. Uzantı değilse. PDF, bir uyarı görüntülenir, ekleme işlemi iptal edildi ve olay işleyicisi yürütülmesini sonlandırır.

> [!NOTE]
> Karşıya yüklenen dosya s uzantısına bağlı olan karşıya yüklenen dosya bir PDF belgesi olduğundan emin olmanın bir sure-fire teknik değil. Kullanıcı uzantısı ile geçerli bir PDF belgesi olabilir `.Brochure`, olmayan PDF belgesi ve verilen bir `.pdf` uzantısı. Dosyanın s ikili içerik daha yaratacağı dosya türünü doğrulamak için programlı bir şekilde incelenmesi gerekir. Ancak, gibi kapsamlı bir yaklaşım düşünülecek çoğunlukla olur; Uzantı denetimi çoğu senaryo için yeterli olur.


Bölümünde açıklandığı gibi [yüklenen dosyalar](uploading-files-cs.md) öğretici, bir kullanıcı s karşıya yükleme, başka bir s geçersiz kılmaz için dosya sistemine kaydetme dosyaları dikkatli'nin alınması gerekir. Karşıya yüklenen dosya aynı adı kullanmak Bu öğretici için deneyeceğiz. Zaten mevcut değilse bir dosyada `~/Brochures` dizin o adla aynı dosya ancak biz ekleme sonuna bir sayı benzersiz bir ad bulunana kadar. Örneğin, kullanıcı adındaki Broşürü dosya yükler `Meats.pdf`, adlı bir dosya var. ancak `Meats.pdf` içinde `~/Brochures` klasörüne kaydedilen dosya adına değiştirmemizi `Meats-1.pdf`. Varsa, biz denenir `Meats-2.pdf`ve benzeri benzersiz bir dosya adı bulunana kadar.

Aşağıdaki kod [ `File.Exists(path)` yöntemi](https://msdn.microsoft.com/library/system.io.file.exists.aspx) belirtilen dosya adı ile bir dosya zaten var olup olmadığını belirlemek için. Bu durumda, çakışma bulunana kadar yeni dosya adlarını Broşürü denemeye devam eder.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Dosya geçerli bir dosya adı bulundu sonra dosya sistemini ve ObjectDataSource s kaydedilmesi gerekir `brochurePath``InsertParameter` değeri bu dosya adı veritabanına yazılır, böylece güncelleştirilmesi gerekiyor. Geri gördüğümüz gibi *yüklenen dosyalar* öğretici, dosya kaydedilebilir s FileUpload denetimini kullanarak `SaveAs(path)` yöntemi. ObjectDataSource s güncelleştirilecek `brochurePath` parametresi, kullanım `e.Values` koleksiyonu.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>7. adım: karşıya yüklenen resim veritabanına kaydetme

Karşıya yüklenen resim yeni depolamak için `Categories` kayıt, karşıya yüklenen ikili içerik ObjectDataSource s atamak ihtiyacımız `picture` DetailsView s parametresinde `ItemInserting` olay. Biz bu atama yapmadan önce Bununla birlikte, önce karşıya yüklenen resim JPG ve değil bir görüntü türü olmasını sağlamak ihtiyacımız var. Adım 6'da olduğu gibi s türünü belirlemek için karşıya yüklenen resim s dosya uzantısının kullanılması olanak tanır.

Sırada `Categories` tablosu verir `NULL` değerleri `Picture` sütun şu anda tüm kategorileri resim bulunur. Bu sayfadan yeni bir kategori eklerken bir resim sağlamak zorlamak s olanak tanır. Bir resmi karşıya yüklendi ve uygun bir uzantısı olduğundan emin olmak için aşağıdaki kodu denetler.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Bu kod yerleştirilmelidir *önce* adım 6 koddan böylece Broşürü dosyanın dosya sistemine kaydedilmeden önce resim karşıya yükleme ile ilgili bir sorun varsa, olay işleyicisi sonlandıracaktır.

Uygun bir dosya yüklediğiniz varsayılarak, karşıya yüklenen ikili içeriği aşağıdaki kod satırını resmi parametre s değeriyle atayın:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>Tam`ItemInserting`olay işleyicisi

Bütünlük açısından işte `ItemInserting` izlediğimizi olay işleyicisi:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>8. adım: Düzeltme`DisplayCategoryPicture.aspx`sayfası

Let s ekleme arabirimi test etmek için biraz alın ve `ItemInserting` geçtiğimiz birkaç adımda oluşturulan olay işleyicisi. Ziyaret `UploadInDetailsView.aspx` sayfa tarayıcısı ve bir kategori ekleyin, ancak resmi atlamak için girişim ya da olmayan JPG resim ya da bir PDF broşür belirtin. Tüm durumlarda, bir hata iletisi görüntülenir ve ekleme iş akışı iptal edildi.


[![Bir uyarı iletisi görüntülenen bir geçersiz dosya türü karşıya ise](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Şekil 9**: bir uyarı iletisi olup görüntülenen bir geçersiz dosya türü karşıya ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Bir kez Broşürü alanın boş bırakılması sayfasının resim karşıya yüklenecek gerektirir ve kazanılan t PDF olmayan veya JPG olmayan dosyaları kabul et, geçerli bir JPG resim ile yeni bir kategori eklemek doğrulanmıştır. Ekle düğmesine tıklandıktan sonra sayfanın geri gönderilir ve yeni bir kayıt eklenir `Categories` doğrudan veritabanında depolanan karşıya yüklenen görüntüyü s ikili içeriği içeren tablo. GridView güncelleştirilir ve yeni eklenen kategorisi için bir satır gösterir, ancak Şekil 10 gösterildiği gibi yeni kategori s resmi doğru işlenmez.


[![Yeni kategori s resim görüntülenmez](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Şekil 10**: resim görüntülenen yeni kategoriye s ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Yeni bir resim görüntülenmez neden olduğundan `DisplayCategoryPicture.aspx` belirtilen kategori s resim döndüren bir sayfa OLE üstbilginin bit eşlemler işlemek için yapılandırılır. Bu 78 bayt üst öğesinden yapılandırıldıktan `Picture` gönderilmeden önce s sütunu ikili içeriğini istemciye geri. Ancak yalnızca dosyamızı JPG dosyasını yeni kategori için bu OLE üst bilgisi yok. Bu nedenle, geçerli, gerekli bayt görüntü s ikili verileri kaldırılıyor.

Artık her iki bit eşlemler OLE üst bilgiler ve jpg formatından içinde olduğundan `Categories` tablo ihtiyacımız güncelleştirilecek `DisplayCategoryPicture.aspx` özgün sekiz kategorilerini şeridi oluşturma OLE üstbilgi ve bu yeni kategori kayıtlarını şeridi oluşturma atlar. Sonraki müşterilerimize öğreticide var olan kayıt s görüntüsünü güncelleştirmek nasıl inceleyeceğiz ve böylece jpg formatından oldukları tüm eski kategori resimler güncelleştireceğiz. Şu an için aşağıdaki kodu kullanın `DisplayCategoryPicture.aspx` yalnızca özgün sekiz kategorilerin için OLE üst bilgilerini gizlemek için:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Bu değişiklik, JPG Resmi artık doğru şekilde GridView içinde işlenir.


[![JPG görüntüleri yeni kategori için doğru şekilde oluşturulmasını](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Şekil 11**: JPG görüntüleri yeni kategorileri için doğru bir şekilde işlenen ([tam boyutlu görüntüyü görmek için tıklatın](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>9. adım: bir özel durum karşılaşıldığında Broşürü siliniyor

Web sunucusu s dosya sisteminde ikili verileri depolamanın en zorluklardan biri, veri modeli ve ikili verileri arasında bir bağlantı içermesidir. Bu nedenle, bir kayıt silindiğinde, dosya sisteminde karşılık gelen ikili veriler de kaldırılmalıdır. Bu da ekleme yaparken oyuna gelebilir. Aşağıdaki senaryoları düşünün: bir kullanıcı geçerli bir resim ve Broşürü belirterek yeni bir kategori ekler. Bir geri gönderme gerçekleşir Ekle düğmesine tıklayarak ve DetailsView s `ItemInserting` Broşürü web sunucusu s dosya sistemine kaydetme olay harekete geçirilir. Ardından, ObjectDataSource s `Insert()` yöntemi çağrılır, çağıran `CategoriesBLL` s sınıfı `InsertWithPicture` çağıran yöntem `CategoriesTableAdapter` s `InsertWithPicture` yöntemi.

Şimdi, veritabanını çevrimdışı olduğunda ne olacağını veya bir hata olup olmadığını `INSERT` SQL deyimi? Yeni kategori satır veritabanına eklenecek şekilde açıkça ekleme başarısız olur. Ancak yine de web sunucusu s dosya sisteminde oturan karşıya yüklenen Broşürü dosya sunuyoruz! Bu dosya ekleme iş akışı sırasında bir özel durumla karşılaşıldığında silinmesi gerekir.

Daha önce açıklandığı gibi [işleme BLL ve DAL düzeyi özel durumları bir ASP.NET sayfasında](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) derinliği, dayalı oluşturan çeşitli katmanları mimarisinin içinde gelen bir özel durum oluştuğunda öğretici. Sunu katmanı bir özel durum DetailsView s gerçekleşip gerçekleşmediğini belirleyebiliriz `ItemInserted` olay. Bu olay işleyicisi ObjectDataSource s değerlerini de sağlar. `InsertParameters`. Bu nedenle, bir olay işleyicisi için oluşturabiliriz `ItemInserted` denetler bir özel durum varsa ve bu durumda, olay ObjectDataSource s tarafından belirtilen dosyayı siler `brochurePath` parametresi:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Özet

İkili verileri içeren bir kayıt eklemek için web tabanlı bir arabirim sağlamak için gerçekleştirilmesi gereken adımlar vardır. İkili veriler doğrudan veritabanına depolanan büyük olasılıkla mimarisi güncelleştirmek ikili verileri nerede eklenmekte durumu işlemek için özel yöntemler ekleme ihtiyacınız vardır. Mimari güncelleştirildi sonra sonraki adım her ikili veri alanı için FileUpload denetim eklemek için özelleştirilmiş bir DetailsView kullanılarak gerçekleştirilebilir ekleme arabirimi oluşturuyor. Karşıya yüklenen veriler sonra web sunucusu s dosya sistemine kayıtlı veya bir veri kaynağı parametre DetailsView s atanan `ItemInserting` olay işleyicisi.

İkili verileri dosya sistemine kaydetme verilerini doğrudan veritabanına kaydetme değerinden daha fazla planlama yapmak gerekir. Başka bir s üzerine bir kullanıcı s karşıya yüklemeyi önlemek için bir adlandırma şeması seçilmelidir. Ayrıca, veritabanı ekleme başarısız olursa, karşıya yüklenen dosya silmek için ek adımlar atılmalıdır.

Artık bir Broşürü ve resim ancak sistemiyle yeni kategori ekleme olanağı henüz mevcut bir kategori s ikili verileri güncelleştirme veya silinmiş bir kategori için ikili verileri doğru şekilde kaldırma göz atmak ve sahibiz. Biz, sonraki öğreticide iki bu konuları ele alacağız.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Dave Gardner Teresa Murphy ve Bernadette Leigh yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](displaying-binary-data-in-the-data-web-controls-cs.md)
> [İleri](updating-and-deleting-existing-binary-data-cs.md)
