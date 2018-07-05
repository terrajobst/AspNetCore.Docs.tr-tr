---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: İkili verileri görüntüleme ve veri Web denetimleri (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide Web görünen görüntü dosyasının ve 'İndir' bağlantısına f hazırlama da dahil olmak üzere bir sayfada ikili verileri sunmak için seçenekleri şu konuları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d298ef328e951f235a6cfcd41b73fafefb0dfb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373113"
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>(VB) veri Web denetimlerinde ikili verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) veya [PDF olarak indirin](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> Bu öğreticide bir görüntü dosyasının görünen ve bir PDF dosyasının bir 'İndir' bağlantısına şartı dahil olmak üzere bir Web sayfasındaki ikili verileri sunmak için seçenekleri bakacağız.


## <a name="introduction"></a>Giriş

Önceki öğreticide ikili verileri bir uygulama s temel alınan veri modeli ile ilişkilendirmek için iki teknik incelediniz ve web sunucusu s dosya sisteminde bir tarayıcıdan dosyaları karşıya yüklemek için kullanılan FileUpload denetim. Biz henüz karşıya yüklenen ikili veriler veri modeli ile ilişkilendirmek nasıl görmek için Kaydet. Diğer bir deyişle, bir dosya karşıya yüklendi ve dosya sistemine kaydettikten sonra dosya yolunu uygun veritabanı kaydı depolanmalıdır. Doğrudan veritabanında depolanan veriler, ardından karşıya yüklenen ikili veriler dosya sistemine kaydedilmemiş, ancak veritabanına eklenmesi gerekir.

Ancak, verileri veri modeli ile ilişkilendirme sırasında göz atmadan önce son kullanıcıya ikili veri sağlamak nasıl bir ilk bakış s olanak tanır. Metin verileri sunmak yeterince basittir ancak ikili verileri nasıl gösterilir? Bu, Elbette, ikili veri türüne bağlıdır. Görüntüleri için büyük olasılıkla görüntüyü istiyoruz; PDF, Microsoft Word belgeleri, ZIP dosyaları ve diğer bir indirme bağlantısı sağlama, ikili veri türleri büyük olasılıkla daha uygun.

Bu öğreticide, verileri kullanarak ilişkili metin verilerini yanı sıra ikili verileri sunmak nasıl görüneceğini GridView ve DetailsView gibi Web denetler. Sonraki öğreticide biz bir karşıya yüklenen dosya veritabanı ile ilişkilendirmek için uygulamamızla açacağım.

## <a name="step-1-providingbrochurepathvalues"></a>1. adım: Sağlama`BrochurePath`değerleri

`Picture` Sütununda `Categories` tablosu zaten çeşitli kategori görüntüleri için ikili verileri içerir. Özellikle, `Picture` her kayıt için sütun karlı, düşük kaliteli, 16 renk bit eşlem görüntüsüne ikili içeriğini tutar. Her kategori görüntü, geniş ve 120 piksel yüksekliktir 172 pikseldir ve yaklaşık 11 KB kullanır. Daha fazla hangi s, ikili içeriği `Picture` sütununu içeren 78 baytlık [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) görüntü görüntülemeden önce atılması gerekir başlığı. Microsoft Access içinde kendi kök Northwind veritabanı olduğundan bu üst bilgi bilgileri mevcuttur. Erişimi'nde, bu başlığında tacks OLE nesnesi veri türünü kullanarak ikili veriler depolanır. Şimdilik, resim görüntülemek için bu düşük kaliteli görüntülerinden üstbilgileri Şerit nasıl göreceğiz. Bir sonraki öğreticide bir kategori s güncelleştirmek için bir arabirim oluşturacağız `Picture` sütun ve gereksiz OLE üst bilgileri olmadan eşdeğer JPG görüntüleri OLE üstbilgileri kullanmak bu bit eşlem resimleri değiştirin.

Önceki öğreticide FileUpload denetiminin nasıl kullanılacağını gördük. Bu nedenle, devam edin ve web sunucusu s dosya sistemine Broşürü dosyaları ekleyin. Bunun yapılması, ancak güncelleştirilmediği `BrochurePath` sütununda `Categories` tablo. Sonraki öğreticide bunun nasıl yapılacağını göreceğiz, ancak şimdilik biz el ile bu sütun için değer sağlamanız gereken.

Bu öğretici s indirme yedi PDF Broşürü dosyalarında bulabilirsiniz `~/Brochures` , Deniz ürünleri dışında kategorilerin her birine yönelik klasör. Kullanılamıyor.%n%nÇözüm tüm kayıtları ikili verileri nerede ilişkilendirdiğiniz senaryoları yapılacağını göstermek için Deniz ürünleri Broşürü ekleme atlanmış. Güncelleştirilecek `Categories` sağ tıklayın, bu değerleri ile tablo `Categories` Sunucu Gezgini düğümü ve tablo verilerini Göster'i seçin. Ardından Broşürü dosyalara olan Şekil 1 gösterildiği gibi bir Broşürü olan her kategori için sanal yol girin. Deniz ürünleri kategori için hiç Broşürü olduğundan, bırakın, `BrochurePath` s sütun değeri olarak `NULL`.


[![El ile kategorileri tablo s BrochurePath sütunu için değerler girin](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Şekil 1**: değerlerini el ile girin `Categories` tablo s `BrochurePath` sütun ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>2. adım: GridView içinde broşürler için indirme bağlantısı sağlama

İle `BrochurePath` için sağlanan değerler `Categories` tablo, biz re her kategorisi kategori s Broşürü indirmek için bir bağlantı ile birlikte listeleyen GridView oluşturmak için hazır. Adım 4'te biz de kategori s görüntüyü görüntülemek için bu GridView genişletin.

Başlangıç GridView Tasarımcısı araç kutusundan sürükleyip `DisplayOrDownloadData.aspx` sayfasını `BinaryData` klasör. GridView s ayarlamak `ID` için `Categories` GridView s akıllı etiket ile yeni bir veri kaynağına bağlamak seçin. Özellikle, bu adlı bir ObjectDataSource için bağlama `CategoriesDataSource` kullanarak verileri alır `CategoriesBLL` s nesnesi `GetCategories()` yöntemi.


[![CategoriesDataSource adlı yeni bir ObjectDataSource oluşturma](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Şekil 2**: adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![ObjectDataSource CategoriesBLL sınıfını kullanmak için yapılandırma](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![GetCategories() yöntemi kullanarak kategorileri listesi alınamıyor](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Şekil 4**: liste, kategorileri kullanarak almak `GetCategories()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak bir BoundField için ekler `Categories` GridView için `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath` `DataColumn` s. Devam edin ve kaldırma `NumberOfProducts` BoundField beri `GetCategories()` metodu s sorgu bu bilgileri alamadı. Kaldırılacak `CategoryID` BoundField ve yeniden adlandırma `CategoryName` ve `BrochurePath` BoundFields `HeaderText` kategorisi ve Broşürü, özellikleri sırasıyla. Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntüleme (bkz: Şekil 5). Sekiz kategorilerden her biri listelenir. Yedi kategorilerle `BrochurePath` değerlere sahip `BrochurePath` ilgili BoundField içinde görüntülenen değeri. Deniz ürünleri sahip bir `NULL` değerini kendi `BrochurePath`, boş bir hücreye görüntüler.


[![Her kategori adı, açıklama ve BrochurePath değer s listelenir](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Şekil 5**: her kategori s adı, açıklamayı ve `BrochurePath` değeri listelenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


Görüntüleme metnini yerine `BrochurePath` sütun, istediğimiz Broşürü bağlantı oluşturma. Bunu yapmak için kaldırmak `BrochurePath` BoundField bir HyperLinkField ile değiştirin. Yeni HyperLinkField s ayarlamak `HeaderText` özelliğini Broşürü, kendi `Text` görünümü Broşürü özelliğini ve kendi `DataNavigateUrlFields` özelliğini `BrochurePath`.


![Bir HyperLinkField BrochurePath için ekleyin](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Şekil 6**: bir HyperLinkField için ekleyin `BrochurePath`


Şekil 7 gösterildiği gibi bu GridView için bağlantılar içeren bir sütun ekler. Bir görünümü Broşürü bağlantıya tıklandığında ya da PDF doğrudan tarayıcınızda görüntülenir veya bir PDF okuyucu yüklü olup olmadığını bağlı olarak dosyayı indirmek için kullanıcı ve tarayıcı s ayarlarını soracak.


[![Bir kategori s Broşürü görünümü Broşürü bağlantıya tıklayarak görüntülenebilir.](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Şekil 7**: görünüm Broşürü bağlantıyı tıklatarak bir kategori s Broşürü görüntülenebilir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Kategori s Broşürü PDF görüntülenir](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Şekil 8**: Kategori s Broşürü PDF görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Broşürlerde olmadan kategorileri için metni görüntüle Broşürü gizleme

Şekil 7 gösterildiği gibi `BrochurePath` HyperLinkField görüntüler, `Text` özellik değeri (Görünüm Broşürü) bağımsız olarak tüm kayıtlar için orada s olmayan bir`NULL` değerini `BrochurePath`. Elbette, `BrochurePath` olduğu `NULL`, bağlantı yalnızca, metin görüntülenir Deniz ürünleri kategorili olduğu gibi (Şekil 7'ye tekrar bakın). Metin görünümü Broşürü görüntülemek yerine, bu kategorileri olmadan iyi olabilir bir `BrochurePath` değer Hayır Broşürü kullanılabilir gibi bazı alternatif metin görüntüleyin.

Bu davranışı sağlamak için içeriği göre uygun çıkış yayan bir sayfa yönteme bir çağrı aracılığıyla oluşturulan bir TemplateField kullanılacak gerekiyor `BrochurePath` değeri. Biz bu tekniği biçimlendirme ilk incelediniz geri [GridView denetiminde TemplateField kullanma](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) öğretici.

HyperLinkField seçerek bir TemplateField kapatma `BrochurePath` HyperLinkField ve dönüştürme bir TemplateField Bu alan ardından sütunları Düzenle iletişim kutusunda bağlantı.


![Bir TemplateField HyperLinkField Dönüştür](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Şekil 9**: bir TemplateField HyperLinkField Dönüştür


Bu bir TemplateField ile oluşturacak bir `ItemTemplate` içeren bir köprü Web ayarlanmış kontrol `NavigateUrl` özelliği bağlı `BrochurePath` değeri. Bu işaretleme yöntemine yapılan bir çağrıyla değiştirin `GenerateBrochureLink`, geçen değerini `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

Ardından, oluşturun bir `Protected` yöntemi ASP.NET sayfasında s arka plan kod sınıf adlı `GenerateBrochureLink` döndüren bir `String` ve kabul eden bir `Object` giriş parametresi olarak.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Bu yöntem belirler geçilen `Object` değeri olan bir veritabanı `NULL` ve bu durumda, kategori broşürlerde eksik belirten bir ileti döndürür. Aksi takdirde varsa bir `BrochurePath` değeri, s köprü görüntülenir. Unutmayın `BrochurePath` değer yöntemlere geçirilen s sunmak [ `ResolveUrl(url)` yöntemi](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Bu yöntemi, geçilen çözümler *url*, değiştirmeyi `~` karakter uygun sanal yol. Örneğin, uygulama köklü `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` döndüreceği `/Tutorial55/Brochures/Meat.pdf`.

Şekil 10, bu değişiklikler uygulandıktan sonra sayfada gösterilir. Unutmayın Deniz ürünleri kategori s `BrochurePath` alan artık yok Broşürü kullanılabilir metni görüntüler.


[![Bu kategorileri olmadan bir Broşürü için metin yok Broşürü kullanılabilir görüntülenir](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Şekil 10**: metin Hayır Broşürü kullanılabilir olanlar kategorileri olmadan bir broşürlerde görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>3. adım: Kategori s resim görüntülemek için bir Web sayfası ekleme

Bir kullanıcı bir ASP.NET sayfasını ziyaret ettiğinde, ASP.NET sayfası s HTML alırlar. Alınan HTML yalnızca metin ve tüm ikili verileri içermiyor. Web sunucusunda ayrı kaynaklar olarak görüntüleri, ses dosyaları, Macromedia Flash uygulamalar, katıştırılmış Windows Media Player videolar vb., gibi ek tüm ikili verileri yok. HTML, bu dosyalara başvuru içeriyor, ancak dosyaların asıl içeriğini içermez.

Örneğin, HTML biçiminde `<img>` öğesi ile bir resim başvurmak için kullanılan `src` görüntü dosyasına işaret eden bir öznitelik şu şekilde:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Bir tarayıcı bu HTML aldığında, ardından tarayıcıda görüntüler görüntü dosyasının ikili içeriğini almak için web sunucusuna başka bir istek gönderir. Aynı kavram, tüm ikili verileri için geçerlidir. Adım 2'de Broşürü sayfa s HTML biçimlendirmeyi bir parçası olarak tarayıcıya gönderilmedi. Bunun yerine, İşlenmiş HTML köprüler, tarayıcı, PDF belgesini doğrudan istemek neden tıklatıldığında, sağlanan.

Görüntülemek veya veritabanı içinde bulunan ikili verileri indirmek kullanıcılara izin vermek için veri döndüren ayrı bir web sayfası oluşturmak ihtiyacımız var. Uygulamamız için doğrudan veritabanı kategorisi s resmi depolanan orada s yalnızca bir ikili veri alanı. Bu nedenle, bir sayfa ihtiyacımız, çağrıldığında, belirli bir kategoriye ait görüntü verilerini döndürür.

Yeni bir ASP.NET sayfasına ekleme `BinaryData` adlı klasöre `DisplayCategoryPicture.aspx`. Bunun yapılması, Select ana sayfa onay kutusunu işaretlemeden bırakın. Bu sayfa bekliyor bir `CategoryID` döndürür s o kategorinin ikili veriler ve sorgu dizesi değeri `Picture` sütun. Bu sayfa, ikili verileri ve başka hiçbir şey döndürdüğünden, HTML bölümündeki tüm biçimlendirme gerekmez. Bu nedenle, sol alt köşedeki kaynak sekmesine tıklayın ve sayfanın s biçimlendirme dışında tümünü Kaldır `<%@ Page %>` yönergesi. Diğer bir deyişle, `DisplayCategoryPicture.aspx` s bildirim temelli biçimlendirme tek satırlık oluşmalıdır:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Görürseniz `MasterPageFile` özniteliğini `<%@ Page %>` yönergesi, bunu kaldırın.

Sayfa s arka plan kod sınıfında, aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Bu kod, okuyarak başlatır `CategoryID` adlı bir değişken querystring değerine `categoryID`. Ardından, resim verilerini çağrısıyla alınır `CategoriesBLL` s sınıfı `GetCategoryWithBinaryDataByCategoryID(categoryID)` yöntemi. Bu veri kullanarak istemciye döndürülen `Response.BinaryWrite(data)` yöntemi, ancak bu çağrılmadan önce `Picture` sütun değeri s OLE başlığı kaldırılmalıdır. Bu oluşturarak yapılabilir bir `Byte` adlı dizisi `strippedImageData` tam olarak 78 tutun karakter nedir değerinden `Picture` sütun. [ `Array.Copy` Yöntemi](https://msdn.microsoft.com/library/z50k9bft.aspx) veri kopyalamak için kullanılan `category.Picture` için üzerinden 78 konumdan başlayarak `strippedImageData`.

`Response.ContentType` Özellik belirtir [MIME türü](http://en.wikipedia.org/wiki/MIME) tarayıcı gerçekleştirmek üzere nasıl bilebilmesi döndürülen içerik. Bu yana `Categories` tablo s `Picture` sütun bit eşlem görüntüsüne, bit eşlem MIME türü kullanılan burada (görüntü/bmp). MIME türü atlarsanız, görüntü dosyası s ikili verilerin içeriğine göre türü çıkarsayabilir çoğu tarayıcısı görüntünün doğru şekilde görüntülenmeye devam eder. Ancak, mümkün olduğunda s MIME içerecek şekilde akıllıca yazın. Bkz: [Internet Atanmış Numaralar Yetkilisi Web sitesinden](http://www.iana.org/) tam listesi için [MIME medya türleri](http://www.iana.org/assignments/media-types/).

Oluşturulan bu sayfayla ederek belirli kategori s resmi görüntülenebilir `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Şekil 11 görüntülenebilir İçecekler kategori s resmi gösterir `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Görüntülenen resmi İçecekler kategorisindeki s](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Şekil 11**: İçecekler kategorisindeki s resim görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Eğer ziyaret edildiğinde, `DisplayCategoryPicture.aspx?CategoryID=categoryID`Unable 'System.Byte []' türü için ' System.DBNull' cast türündeki nesneye okuyan bir özel durum almak, bu neden olabilecek iki şey vardır. İlk olarak, `Categories` tablo s `Picture` sütunu izin `NULL` değerleri. `DisplayCategoryPicture.aspx` Sayfasında, ancak var olduğunu varsayar olmayan bir`NULL` değer mevcut. `Picture` Özelliği `CategoriesDataTable` varsa doğrudan erişilemez bir `NULL` değeri. İzin vermek istiyorsanız `NULL` değerleri `Picture` sütun, d aşağıdaki koşul eklemek istediğiniz:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

Yukarıdaki kod, bazı görüntü adlı dosya olmadığını s varsayar `NoPictureAvailable.gif` içinde `Images` resmi olmayan bu kategori için görüntülemek istediğiniz klasör.

Bu durum Ayrıca, kaynaklanabilir `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` metodu s `SELECT` deyimi geçici SQL deyimleri kullanıyorsanız ve önceden TableAdapter s için sihirbazı yeniden çalıştırın gerçekleşebilir geri ana sorgu s sütun listesine geri döndürüldü ana sorgu. Emin olmak için onay `GetCategoryWithBinaryDataByCategoryID` metodu s `SELECT` hala deyimi `Picture` sütun.

> [!NOTE]
> Her zaman `DisplayCategoryPicture.aspx` olan ziyaret edilen veritabanı erişilir ve belirtilen kategori s resim verileri döndürülür. Kullanıcının son görüntülemenizden sonra kategori s resim forumlarındaki t değiştirdiyseniz, yine de boşa giden çaba budur. Neyse ki, HTTP için izin verir *koşullu alır*. Koşullu bir Al ile boyunca HTTP isteği yapan istemcinin gönderdiği bir [ `If-Modified-Since` HTTP üstbilgisi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) istemci son bu kaynağı web sunucusu vm'sinden alındığı saat ve tarihi sağlar. Bu tarih belirtilen bu yana değişmemişse içerik web sunucusu ile yanıt verebilir bir [(304) durum kodu değiştirilmedi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) ve istenen kaynak s içerik geri göndererek atlayabilirsiniz. Kısacası, bu teknik, web sunucusu, istemcinin en son erişilen olduğundan, değiştirilmemiş yoksa bir kaynak için içerik göndermek zorunda üzerinizden alır.


Bu davranışı uygulamak ancak, eklemeniz gerekir. bir `PictureLastModified` sütuna `Categories` ne zaman yakalamak için tablo `Picture` sütun yanı sıra kodu denetlemek için son güncelleştirildi `If-Modified-Since` başlığı. Daha fazla bilgi için `If-Modified-Since` üstbilgi ve koşullu GET iş akışını görmek [koşullu HTTP RSS saldırganlar için alma](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) ve [A daha ayrıntılı incelemek, bir ASP.NET sayfasında HTTP isteklerini gerçekleştirmek](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>4. adım: Kategori resimleri GridView içinde görüntüleme

Belirli kategori s resim görüntülemek için bir web sayfası sahibiz, kullanarak görüntülemek [Görüntü Web denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) veya HTML `<img>` işaret eden öğe `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Görüntü URL'si veritabanı veri tarafından belirlenir GridView veya DetailsView ImageField kullanılarak görüntülenebilir. ImageField içeren `DataImageUrlField` ve `DataImageUrlFormatString` HyperLinkField s gibi çalışma özellikleri `DataNavigateUrlFields` ve `DataNavigateUrlFormatString` özellikleri.

S büyütmek izin `Categories` içinde GridView `DisplayOrDownloadData.aspx` her kategori s resmi gösterilecek bir ImageField ekleyerek. Yalnızca ImageField ekleme ve kendi `DataImageUrlField` ve `DataImageUrlFormatString` özelliklerine `CategoryID` ve `DisplayCategoryPicture.aspx?CategoryID={0}`sırasıyla. Bu işleyen GridView sütunu oluşturmak bir `<img>` öğesi olan `src` başvuruları öznitelik `DisplayCategoryPicture.aspx?CategoryID={0}`burada {0} GridView satır s ile değiştirilir `CategoryID` değeri.


![GridView'a bir ImageField Ekle](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Şekil 12**: bir ImageField GridView'a Ekle


ImageField ekledikten sonra GridView s Tanımlayıcı Sözdizimi soothe gibi görünmelidir aşağıdaki:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Her bir kaydı kategorisi için bir resim şimdi nasıl içerdiğini unutmayın.


[![S resmi kategori her satırı için görüntülenir.](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Şekil 13**: her satır için görüntülenen resmi kategori s ([tam boyutlu görüntüyü görmek için tıklatın](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Özet

Bu öğreticide, biz nasıl ikili verileri sunmak incelenir. Verilerin gösterilme veri türüne bağlıdır. PDF Broşürü dosyaları için biz kullanıcı görünümü Broşürü sunulan, tıklandığında bağlantı kullanıcı doğrudan PDF dosyasına sürdü. Kategori s resmi için almak ve ikili veri veritabanından bir sayfa ilk oluşturduğunuz ve sonra her kategori s resmi GridView içinde görüntülemek için bu sayfayı kullanılır.

Artık görüyoruz ve baktığı biz ekleme, güncelleştirme ve silme ile ikili veriler veritabanında gerçekleştirme incelemek için hazır re ikili verileri görüntülemek nasıl. Sonraki öğreticide karşıya yüklenen bir dosya, karşılık gelen veritabanı kaydıyla ilişkilendirmek nasıl şu konuları inceleyeceğiz. Mevcut ikili verileri güncelleştirme ve bunun yanı sıra kendi ilişkili kaydı kaldırıldığında ikili verileri silme, sonra öğreticide göreceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Teresa Murphy ve Dave Gardner yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](uploading-files-vb.md)
> [İleri](including-a-file-upload-option-when-adding-a-new-record-vb.md)
