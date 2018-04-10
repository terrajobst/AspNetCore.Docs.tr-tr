---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Dosyalar (C#) | Microsoft Docs
author: rick-anderson
description: İkili dosyaları (örneğin, Word veya PDF belgesini) yüklemek kullanıcıların Web sitenize sunucusunun dosya sisteminde bunlar burada depolanabilir öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c758e94311817d01b17d27083733f805caf600f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="uploading-files-c"></a>Dosyalar (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) veya [PDF indirin](uploading-files-cs/_static/datatutorial54cs1.pdf)

> İkili dosyaları (örneğin, Word veya PDF belgesini) yüklemek kullanıcıların Web sitenize burada bunlar sunucunun dosya sistemini veya veritabanı depolanabilir öğrenin.


## <a name="introduction"></a>Giriş

Tüm öğreticileri biz kadarki incelenmesi ve özel olarak metin verilerle çalışılan. Ancak, birçok uygulama, metin ve ikili veri yakalama veri modelleri vardır. Çevrimiçi dating site kullanıcıların kendi profiliyle ilişkilendirmek için bir resim yüklemesine olanak sağlayabilir. Personel arama bir Web sitesi, kullanıcıların kendi Sürdür Microsoft Word veya PDF belgesi olarak karşıya sağlayabilir.

İkili veriler ile çalışma yeni zorluklarla ekler. İkili veriler uygulamada nasıl depolandığını karar vermelisiniz. Kendi bilgisayarından bir dosyayı karşıya yüklemeyi izin vermek için güncelleştirilecek yeni kayıtları eklemek için kullanılan arabirimi var ve görüntülemek ya da ikili veri kaydı s karşıdan yüklemek için bir yol ilişkili sağlamak için ek adımlar izlenmelidir. Bu öğretici ve sonraki üç Biz bu zorluklar hurdle nasıl ele alacağız. Bu öğreticiler sonunda bir resim ve PDF Broşürü her kategori ile ilişkilendirir tam olarak işlevsel bir uygulama oluşturduğumuz. Belirli Bu öğreticide biz ikili veri depolamak için farklı teknikleri bakın ve kullanıcıların kendi bilgisayarından bir dosyayı karşıya yüklemek nasıl keşfetmek ve web sunucusu s dosya sisteminde kaydettiğiniz.

> [!NOTE]
> Bir uygulama s veri modelinin parçası olan ikili veri bazen olarak adlandırılır bir [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), ikili büyük nesne kısaltması. Terim BLOB eşanlamlıdır rağmen bu öğreticileri ı terminolojisi ikili veri kullanmayı seçtiniz.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>1. adım: ikili veri Web sayfalarıyla çalışan oluşturma

İkili veriler için desteği ekleme ile ilgili sorunları araştırmak başlamadan önce öncelikle Bu öğretici ve sonraki üç yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `BinaryData`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![İkili veri ilişkili öğreticileri için ASP.NET sayfaları ekleme](uploading-files-cs/_static/image1.gif)

**Şekil 1**: ikili veri ilişkili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `BinaryData` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image2.png))


Son olarak, bu sayfaları girişlere eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme Enhancing sonra eklemeniz GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Sol menü öğelerini ikili veri öğreticileri çalışmak için şimdi içerir.


![Site Haritasını ikili veri öğreticileri çalışmak için girişleri şimdi içerir.](uploading-files-cs/_static/image3.gif)

**Şekil 3**: Site Haritası ikili veri öğreticileri çalışmak için girişleri şimdi içerir.


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>2. adım: ikili verileri depolanacağı karar verme

Uygulama s veri modeli ile ilişkili ikili verileri iki yerde birinde depolanabilir: veritabanında; depolanan dosyasına bir başvuru ile web sunucusu s dosya sisteminde veya doğrudan veritabanı içinde (Şekil 4'e bakın). Her iki yaklaşımın Artıları ve eksileri kendi kümesine sahiptir ve daha ayrıntılı bir tartışma merits.


[![İkili veriler dosya sisteminde veya doğrudan veritabanında depolanan](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Şekil 4**: ikili veriler, dosya sistemindeki veya doğrudan veritabanında depolanabilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image4.png))


Biz her ürünle birlikte bir resim ilişkilendirilecek Northwind veritabanı genişletmek istediğinizi düşünelim. Bu görüntü dosyaları web sunucusu s dosya sisteminde depolayın ve yolu kaydetmek için bir seçenek olacaktır `Products` tablo. Bu yaklaşımda, d eklediğimiz bir `ImagePath` sütuna `Products` tablo türü `varchar(200)`, belki de. Web sunucusu s dosya sistemi üzerinde bir kullanıcının bir resim ayrıntılarını karşıya olduğunda, o resmi depolanabilir `~/Images/Tea.jpg`, burada `~` uygulama s fiziksel yolu temsil eder. Diğer bir deyişle, web sitesi fiziksel yolda kökü ise `C:\Websites\Northwind\`, `~/Images/Tea.jpg` eşdeğer olacaktır `C:\Websites\Northwind\Images\Tea.jpg`. Görüntü dosyasını karşıya yüklemeden sonra d Chai kaydında güncelleştiriyoruz `Products` tablo böylece kendi `ImagePath` sütununa başvuruda yeni resmin yolu. Biz kullanabilirsiniz `~/Images/Tea.jpg` veya yalnızca `Tea.jpg` tüm ürün görüntüleri uygulama s konulabilir karar verirseniz `Images` klasör.

Dosya sisteminde ikili veri depolamanın ana avantajları şunlardır:

- **Uygulama kolaylığı** kısa süre içinde anlatıldığı gibi doğrudan veritabanında depolanan ikili verileri alma ve depolamada dosya sistemi üzerinden verilerle zaman çalışma değerinden biraz daha fazla kod içerir. Ayrıca, sırayla görüntülemek veya ikili veri yüklemek bir kullanıcı için bunlar bu verileri URL'yi sunulmalıdır. Verileri web sunucusu s dosya sisteminde bulunuyorsa, basit bir URL'dir. Veriler veritabanında depolanır ancak, bir web sayfasında almak ve dönüş verileri veritabanından oluşturulması gerekir.
- **İkili veriler için daha geniş erişim** ikili verileri diğer hizmetler veya uygulamalar, veritabanından veri çekme olamaz olanları erişilebilir olması gerekebilir. Örneğin, her ürünle ilişkili görüntüleri de aracılığıyla kullanıcılara kullanılabilir olması gereken [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), bu durumda d dosya sisteminde ikili verileri depolamak istiyoruz.
- **Performans** dosya sisteminde saklanan ikili verileri, veritabanı sunucusu ve web sunucusu arasında isteğe bağlı ve Ağ Tıkanıklığı ikili veriler doğrudan veritabanı içinden depolanıyorsa değerinden olacaktır.

Dosya sisteminde ikili veri depolamanın ana dezavantajı, veritabanındaki verileri ayrıştırır ' dir. Uygulamasından bir kaydı silinirse `Products` tablo, web sunucusu s dosya sisteminde ilişkili dosya otomatik olarak silinmez. Dosya veya dosya sistemi silmek için ek kod kullanılmayan, yalnız bırakılmış dosyalarıyla kalabalık hale yazmanız gerekir. Ayrıca, veritabanını yedeklerken, biz dosya sistemine de ilişkili ikili veri yedeklemeleri yapmak emin olmanız gerekir. Veritabanını başka bir site veya sunucu üzerinden benzer zorluklar taşıma.

Alternatif olarak, ikili veriler doğrudan bir Microsoft SQL Server 2005 veritabanında türünde bir sütun oluşturarak depolanabilir `varbinary`. Gibi diğer değişken uzunlukta veri türleriyle, maksimum uzunluktaki tutulabilir ikili veriler bu sütunda belirtebilirsiniz. Örneğin, en çok ayrılacak 5.000 bayt kullanmak `varbinary(5000)`; `varbinary(MAX)` en fazla depolama boyutunu için yaklaşık 2 GB sağlar.

İkili veriler doğrudan veritabanında saklamak ana avantajı ikili verileri ve veritabanı kaydını arasında sıkı bağ olmasıdır. Bu büyük ölçüde veritabanı yedeklemeleri veya farklı bir siteye veya sunucu için veritabanını taşıma gibi yönetim görevlerini basitleştirir. Ayrıca, otomatik olarak kayıt silme karşılık gelen ikili verileri siler. İkili veriler veritabanında saklamak daha fazla Zarif avantajları vardır. Bkz: [depolama ikili dosyaları doğrudan veritabanı kullanarak ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) daha ayrıntılı bir tartışma için.

> [!NOTE]
> Microsoft SQL Server 2000 ve önceki sürümlerde, `varbinary` veri türüne sahip bir üst sınır 8.000 bayt. 2 GB ikili veri depolamak için [ `image` veri türü](https://msdn.microsoft.com/library/ms187993.aspx) yerine kullanılmalıdır. Eklenmesi ile `MAX` SQL Server 2005, ancak `image` veri türü kullanım dışı bırakıldı. Bu s hala desteklenmektedir için geriye dönük uyumluluk, ancak Microsoft, duyurdu `image` veri türü SQL Server'ın gelecek bir sürümünde kaldırılacak.


Eski bir veri modeli ile çalışıyorsanız, görebilirsiniz `image` veri türü. Northwind veritabanı s `Categories` tablolu bir `Picture` kategorisi için bir görüntü dosyasının ikili verileri depolamak için kullanılan sütun. Northwind veritabanı, Microsoft Access ve SQL Server'ın önceki sürümlerinde, kökleri olduğundan, bu sütun türünde `image`.

Bu öğretici ve sonraki üç için her iki yaklaşımın kullanacağız. `Categories` Tablosu zaten bir `Picture` kategorisi için görüntü ikili içeriğini depolamak için sütun. Ek bir sütun ekleyeceğiz `BrochurePath`, kategori yazdırma kalitesi, zarif bir bakış sağlamak için kullanılan web sunucusu s dosya sisteminde bir PDF yoluna depolamak için.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>3. adım: Ekleme`BrochurePath`sütuna`Categories`tablosu

Şu anda yalnızca dört sütun kategorileri tablo yok: `CategoryID`, `CategoryName`, `Description`, ve `Picture`. Bu alanların yanı sıra, biz (varsa), kategori s Broşürü işaret edecek yeni bir tane eklemeniz gerekir. Bu sütun eklemek için Sunucu Gezgini tablolara detaya, sağ tıklayın gidin `Categories` tablo ve açık tablo tanımı seçin (bkz. Şekil 5). Sunucu Gezgini görmüyorsanız, Görünüm menüsünden Sunucu Gezgini seçeneği belirleyerek kutusunu açın veya Ctrl + Alt + S ulaştı.

Yeni bir ekleme `varchar(200)` sütuna `Categories` adlı tablo `BrochurePath` ve verir `NULL` s ve Kaydet simgesine tıklayın (veya Ctrl + S isabet).


[![Kategoriler tabloya BrochurePath sütun ekleme](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Şekil 5**: ekleme bir `BrochurePath` sütuna `Categories` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>4. adım: mimarisi kullanımı için güncelleştirme`Picture`ve`BrochurePath`sütunlar

`CategoriesDataTable` Veri erişim düzeyi (DAL) şu anda dört sahip `DataColumn` tanımlanan s: `CategoryID`, `CategoryName`, `Description`, ve `NumberOfProducts`. Ne zaman biz ilk olarak tasarlanan bu DataTable [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici, `CategoriesDataTable` yalnızca ilk üç sütun içeriyor; `NumberOfProducts` sütun eklenen [ana/ayrıntı kullanarak bir madde işaretli Ana Ayrıntılar DataList kayıtlarıyla listesi](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Öğreticisi.

' Da anlatıldığı gibi *veri erişim katmanı oluşturma*, iş nesneleri yazılan veri kümesinde DataTables olun. TableAdapters ile iletişim kurmak ve sorgu sonuçlarının iş nesnelerle doldurmak için sorumludur. `CategoriesDataTable` Tarafından doldurulur `CategoriesTableAdapter`, üç veri alma yöntemleri vardır:

- `GetCategories()` TableAdapter s ana sorgu yürütür ve döndürür `CategoryID`, `CategoryName`, ve `Description` tüm kayıtları alanlarının `Categories` tablo. Ana sorgu ne otomatik olarak oluşturulan tarafından kullanılır `Insert` ve `Update` yöntemleri.
- `GetCategoryByCategoryID(categoryID)` döndürür `CategoryID`, `CategoryName`, ve `Description` kategori alanlarını, `CategoryID` eşittir *adlı kullanıcı, Categoryıd'si*.
- `GetCategoriesAndNumberOfProducts()` -döndürür `CategoryID`, `CategoryName`, ve `Description` tüm kayıtlar için alanları `Categories` tablo. Ayrıca her kategoriyle ilişkili ürünleri sayısını döndürmek için bir alt sorgu kullanır.

Hiçbiri return sorgular bildirimi `Categories` s tablosu `Picture` veya `BrochurePath` sütunların; ya da mu `CategoriesDataTable` sağlamak `DataColumn` bu alanlar için s. Resimle çalışması için ve `BrochurePath` özelliklerini ihtiyacımız ilk için düzenli olarak eklemek `CategoriesDataTable` ve ardından güncelleştirme `CategoriesTableAdapter` bu sütunları döndürülecek sınıfı.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Ekleme`Picture`ve`BrochurePath``DataColumn` s

Bu iki sütun eklemeye başlayın `CategoriesDataTable`. Sağ `CategoriesDataTable` s üstbilgi bağlam menüsünden Ekle'yi seçin ve ardından sütun seçeneği belirtin. Bu yeni bir oluşturacak `DataColumn` adlı DataTable tablosundaki `Column1`. Bu sütunu yeniden adlandırmak `Picture`. Özellikler penceresinden ayarlamak `DataColumn` s `DataType` özelliğine `System.Byte[]` (Bu aşağı açılan listesinde bir seçenek değil; yazmanız gerekir).


[![Veri türü olan System.Byte [] DataColumn adlı resim oluşturma](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Şekil 6**: oluşturmak bir `DataColumn` adlandırılmış `Picture` , `DataType` olan `System.Byte[]` ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image8.png))


Başka bir tane eklemek `DataColumn` DataTable tablosuna adlandırma `BrochurePath` varsayılan kullanılarak `DataType` değeri (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Döndürme`Picture`ve`BrochurePath`TableAdapter değerleri

Bu iki ile `DataColumn` eklenen s `CategoriesDataTable`, biz güncelleştirmek için hazır re `CategoriesTableAdapter`. Biz bu sütun değerleri ana TableAdapter sorgusunda döndürülen her ikisi de olabilir, ancak bu geri her zaman ikili veri sunacağı `GetCategories()` yöntem çağrıldığı. Bunun yerine, let s güncelleştirin geri getirmek için ana TableAdapter sorgu `BrochurePath` ve belirli bir kategoriye s döndüren bir ek veri alma yöntemi oluşturma `Picture` sütun.

Ana TableAdapter sorgu güncelleştirmek için sağ `CategoriesTableAdapter` s üstbilgi ve bağlam menüsünden yapılandırma seçeneğini belirleyin. Bu tablo bağdaştırıcısı Yapılandırma Sihirbazı'nı, hangi biz getirir ve geçmiş öğreticileri sayısında görülen. Geri getirmek için sorguyu güncelleştirmek `BrochurePath` ve Son'u tıklatın.


[![Güncelleştirme ayrıca BrochurePath dönmek için SELECT deyiminde sütun listesi](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Şekil 7**: sütun listesinde güncelleştirmek `SELECT` aynı zamanda sonuç ifadesine `BrochurePath` ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image10.png))


Geçici SQL deyimleri için TableAdapter kullanırken, ana sorgu sütun listesinde güncelleştirme sütun listesi için tüm güncelleştirmeleri `SELECT` sorgu TableAdapter yöntemleri. Anlamına `GetCategoryByCategoryID(categoryID)` yöntemi, geri dönmek için güncelleştirilmiştir `BrochurePath` biz hedeflenen olabilir sütun. Ancak, sütun listesinde de güncelleştirilmiş `GetCategoriesAndNumberOfProducts()` yöntemi, her kategori için ürünleri sayısı döndüren kaldırmayı! Bu nedenle, bu yöntem s güncelleştirmek ihtiyacımız `SELECT` sorgu. Sağ `GetCategoriesAndNumberOfProducts()` yöntemi Yapılandır'ı seçin ve geri `SELECT` özgün değeri geri sorguya:


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Ardından, belirli bir kategoriye s döndürür yeni bir TableAdapter yöntemi oluşturun `Picture` sütun değeri. Sağ `CategoriesTableAdapter` s üstbilgi ve TableAdapter sorgu Yapılandırma Sihirbazı'nı başlatmak için Sorgu Ekle seçeneği seçin. Bu sihirbaz ilk adımı bize biz geçici SQL deyimi kullanarak verileri sorgulamak istiyorsanız, yeni bir saklı yordam veya mevcut bir sorar. Use SQL deyimleri seçin ve İleri'yi tıklatın. Biz bir satır döndüren olduğundan, satır seçeneği ikinci adımdan döndüren Seç.


[![Use SQL deyimleri seçeneği seçin](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Şekil 8**: Use SQL deyimleri seçeneği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image12.png))


[![Sorgu kategoriler tablosundan bir kayıt döndürür olduğundan, Seç seçin, satırları döndürür](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Şekil 9**: Sorgu kategoriler tablosundan seçin satırlar döndüren seçin bir kaydı döndürür bu yana ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image14.png))


Üçüncü adım, aşağıdaki SQL sorgusunu girin ve İleri'yi tıklatın:


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Son adım, yeni yöntemin adını seçmektir. Kullanım `FillCategoryWithBinaryDataByCategoryID` ve `GetCategoryWithBinaryDataByCategoryID` dolgu DataTable ve Return DataTable, sırasıyla düzenleri. Sihirbazı tamamlamak için Son'u tıklatın.


[![TableAdapter s yöntemleri için adlar seçin](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Şekil 10**: TableAdapter s yöntemleri için adlar seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image16.png))


> [!NOTE]
> Tablo bağdaştırıcısı sorgu Yapılandırma Sihirbazı'nı tamamladıktan sonra yeni komut metni ile şema ana sorgu şemasından farklı döndürüldüğünü bildiren bir iletişim kutusu görebilirsiniz. Sihirbaz, kısa, belirtmeye TableAdapter s ana sorgu `GetCategories()` yeni oluşturduğumuz olandan farklı bir şema döndürür. Ancak bu iletiyi göz ardı edebilirsiniz şekilde ne istiyoruz, budur.


Ayrıca, göz önünde bulundurmanız geçici SQL deyimlerini kullanarak ve sihirbazın sonraki bir noktada TableAdapter s ana sorgu zamanında değiştirmek için kullanıyorsanız, bunu değiştirecek `GetCategoryWithBinaryDataByCategoryID` s yöntemi `SELECT` yalnızca bu sütunları içerecek şekilde deyimi s sütun listesi ana sorgu (diğer bir deyişle, kaldıracak `Picture` sorgu sütundan). Döndürülecek sütun listesi el ile güncelleştirmeniz gerekecektir `Picture` sütun, ne ile yaptığımız için benzer `GetCategoriesAndNumberOfProducts()` Bu adımda yöntemi.

İki ekledikten sonra `DataColumn` s `CategoriesDataTable` ve `GetCategoryWithBinaryDataByCategoryID` yönteme `CategoriesTableAdapter`, bu sınıfları yazılan veri kümesi Tasarımcısı'nda Şekil 11 ekran görüntüsüne benzer görünmelidir.


![Veri kümesi Tasarımcısı yöntemi ve yeni sütunlar içerir](uploading-files-cs/_static/image11.gif)

**Şekil 11**: veri kümesi Tasarımcısı yöntemi ve yeni sütunlar içerir


## <a name="updating-the-business-logic-layer-bll"></a>İş mantığı katmanı (BLL) güncelleştiriliyor

Güncelleştirilmiş DAL ile kalan tek şey bir yöntem için yeni dahil etmek için iş mantığı katmanı (BLL) büyütmek üzere `CategoriesTableAdapter` yöntemi. Aşağıdaki yöntemi ekleyin `CategoriesBLL` sınıfı:


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>5. adım: istemci Web sunucusuna bir dosyanın karşıya yükleme

Görmemeleri ikili verileri toplarken, bu verileri bir son kullanıcı tarafından sağlanır. Bu bilgileri yakalamak için kullanıcı kendi bilgisayarından bir dosyayı web sunucusuna yüklemek gerekir. Karşıya yüklenen veriler ardından web sunucusu s dosya sistemi ve veritabanı dosyasında bir yol ekleme veya ikili içeriği doğrudan veritabanına yazma dosyayı kaydetmeden olduğu anlamına gelebilir veri modeli ile tümleştirilmesi gerekir. Bu adımda kendi bilgisayarından bir dosya sunucusuna yüklemek bir kullanıcı izin vermek nasıl inceleyeceğiz. Sonraki öğreticide biz yüklenen dosya veri modeli ile tümleştirmek için uygulamamızla kapatmanız.

ASP.NET 2.0 yenilikler [dosya yükleme Web denetimi](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) kullanıcıların bir dosya bilgisayarlarından web sunucusuna göndermek bir mekanizma sağlar. Dosya yükleme denetimi olarak işleyen bir `<input>` öğesi, `type` özniteliği bir Gözat düğmesi kutusuyla olarak tarayıcıları görüntüler dosya olarak ayarlanmış. Gözat düğmesini tıklatarak kullanıcı bir dosyayı seçebileceğiniz bir iletişim kutusunu açar. Formun geri gönderildiğinde, seçilen dosya s içeriği geri gönderme birlikte gönderilir. Sunucu tarafında yüklenen dosya hakkında bilgi dosya yükleme denetim özelliklerinde erişilebilir.

Karşıya yükleme dosyaları göstermek için açık `FileUpload.aspx` sayfasındaki `BinaryData` klasör, dosya yükleme denetimini tasarımcıya araç çubuğuna sürükleyin ve s denetimini ayarlama `ID` özelliğine `UploadTest`. Ardından, ayarı düğmesi Web denetim ekleme kendi `ID` ve `Text` özelliklerine `UploadButton` ve seçilen dosya, sırasıyla yükleyin. Son olarak, Temizle düğmesini altındaki bir etiket Web denetimi yerleştirin, `Text` özelliği ve kümesi kendi `ID` özelliğine `UploadDetails`.


[![Dosya Yükleme denetim ASP.NET sayfası ekleme](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Şekil 12**: ASP.NET sayfası dosya yükleme denetim ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image18.png))


Şekil 13 bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir. Gözat düğmesini tıklatarak bir dosya seçimi iletişim kutusunu, kullanıcının kendi bilgisayardan dosya çekme izin getirir unutmayın. Bir dosya seçildiğinde seçilen dosyasını karşıya yükle düğmesi geri seçilen dosya s ikili içerik web sunucusuna gönderen gönderimin neden olur.


[![Kullanıcı bilgisayarlarından sunucuya yüklemek için bir dosya seçebilirsiniz](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Şekil 13**: kullanıcı bir dosyayı karşıya yükleme için sunucu bilgisayarlarına seçebilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image20.png))


Geri yüklenen dosya için dosya sistemi kaydedilebilir veya ikili verileri ile doğrudan bir akış çalışılabilecek. Bu örnekte, oluşturma s izin bir `~/Brochures` klasörü ve karşıya yüklenen dosyasını kaydedin. Başlangıç ekleyerek `Brochures` kök dizinin bir alt site klasörüne. Ardından, bir olay işleyicisi oluşturun `UploadButton` s `Click` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Dosya yükleme denetimi çeşitli karşıya yüklenen verilerle çalışmak için özellikler sunar. Örneğin, [ `HasFile` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) bir dosyayı kullanıcı tarafından karşıya yüklenen olup olmadığını gösterir sırada [ `FileBytes` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) bir bayt dizisi karşıya yüklenen ikili verilere erişim sağlar. `Click` Olay işleyicisini başlatır bir dosyayı karşıya sağlayarak. Bir dosyayı karşıya yüklediyseniz, etiket karşıya yüklenen dosyanın, bayt boyutunda ve, içerik türü adını gösterir.

> [!NOTE]
> Kullanıcı kontrol edebilirsiniz bir dosya yükler emin olmak için `HasFile` özelliği ve bir uyarı görüntüler, s `false`, veya kullanabilirsiniz [RequiredFieldValidator denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) yerine.


Dosya Yükleme s `SaveAs(filePath)` karşıya yüklenen dosyanın belirtilen kaydeder *filePath*. *filePath* olmalıdır bir *fiziksel yolu* (`C:\Websites\Brochures\SomeFile.pdf`) yerine bir *sanal* *yolu* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) sanal yolu alır ve kendi karşılık gelen fiziksel yolu döndürür. Sanal yol işte `~/Brochures/fileName`, burada *fileName* yüklenen dosya adıdır. Bkz: [kullanarak Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) sanal ve fiziksel yollarını ve kullanma hakkında daha fazla bilgi için `Server.MapPath`.

Tamamladıktan sonra `Click` olay işleyicisi sayfasını bir tarayıcıda çıkışı test etmek için bir dakikanızı ayırın. Gözat düğmesine tıklayın ve sabit sürücünüzden bir dosya seçin ve ardından seçili dosyasını karşıya yükle düğmesine tıklayın. Geri gönderme seçilen dosyanın içeriğini ardından ona kaydetmeden önce dosya hakkındaki bilgileri görüntüler web sunucusuna Gönder `~/Brochures` klasör. Dosyayı karşıya yükledikten sonra Visual Studio'ya geri dönün ve Çözüm Gezgini'nde Yenile düğmesini tıklatın. Yalnızca ~/Brochures klasöründe karşıya dosya görmeniz gerekir!


[![Dosya EvolutionValley.jpg Web sunucusuna karşıya yüklendi](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Şekil 14**: File `EvolutionValley.jpg` edilmiş yükledi Web sunucusuna ([tam boyutlu görüntüyü görüntülemek için tıklatın](uploading-files-cs/_static/image22.png))


![EvolutionValley.jpg ~/Brochures klasörüne kaydedildi](uploading-files-cs/_static/image15.gif)

**Şekil 15**: `EvolutionValley.jpg` konumuna kaydedildi `~/Brochures` klasörü


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Karşıya yüklenen dosyaların dosya sistemine kaydetme ile subtleties

Dosyaları karşıya yükleme için web sunucusu s dosya sisteminde kaydedilirken ele alınması gereken birkaç subtleties vardır. İlk olarak, burada s sorunu güvenlik. Bir dosyayı dosya sistemine kaydetmek için güvenlik bağlamı altında ASP.NET sayfası yürütüyor yazma izinlerine sahip olmalıdır. ASP.NET Geliştirme Web sunucusu, geçerli kullanıcı hesabının bağlamında çalışır. Microsoft s Internet Information Services (IIS) web sunucusu olarak kullanıyorsanız, güvenlik bağlamı IIS ve yapılandırmasıyla sürümüne bağlıdır.

Dosya adlandırma geçici dosyaları dosya sistemine kaydetme, başka bir güçlükle döner. Şu anda sayfamızı karşıya yüklenen dosyaların tamamını kaydeder `~/Brochures` s istemci bilgisayardaki dosyası olarak aynı adı kullanarak dizin. Kullanıcı bir ada sahip bir Broşürü yüklerse, `Brochure.pdf`, dosya olarak kaydedilmiş `~/Brochure/Brochure.pdf`. Ancak ne süre sonraki kullanıcı B aynı filename sahip olan bir farklı Broşürü dosyayı yükler (`Brochure.pdf`)? Kodla Şimdi s dosyası kullanıcı B s karşıya yükleme ile geçersiz kılınır kullanıcı sahibiz.

Dosya adı çakışmalarını çözmek için teknikleri mevcuttur. Zaten var., varsa aynı ada sahip bir dosyayı karşıya yüklemeyi engelle bir seçenektir. Kullanıcı B adlı bir dosyayı karşıya yüklemeyi denediğinde bu yaklaşımı `Brochure.pdf`, sistem değil, dosyayı kaydedin ve bunun yerine kullanıcı dosyayı yeniden adlandırın ve yeniden denemek için B bildiren bir ileti görüntüler. Olabilir bir benzersiz dosya adını kullanarak dosyayı kaydetmek için başka bir yaklaşımdır bir [genel benzersiz tanıtıcısı (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) veya kayıt s birincil anahtar sütunları veritabanı karşılık gelen değerden (karşıya yükleme ilişkili olduğu varsayılarak bir belirli bir satır veri modelindeki). Sonraki öğreticide Biz bu seçenekleri daha ayrıntılı ele alacağız.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>İkili veri çok büyük miktarlarda ile ilgili sorunları

Bu öğreticiler yakalanan ikili veri boyutu uygun olduğunu varsayın. Çok büyük miktarda birkaç megabayt ikili veri dosyaları ile çalışma veya daha büyük bu öğreticileri kapsamı dışındaki yeni zorluklar getirir. Bu aracılığıyla yapılandırılabilir olsa da örneğin, varsayılan ASP.NET 4 MB'den daha yüklemeleri reddeder [ `<httpRuntime>` öğesi](https://msdn.microsoft.com/library/e1f13641.aspx) içinde `Web.config`. IIS kendi dosya karşıya yükleme boyutu sınırlamaları çok uygular. Bkz: [IIS karşıya dosya boyutu](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) daha fazla bilgi için. Ayrıca, büyük dosyaları karşıya yükleme için geçen süre 110 ASP.NET isteği için bekleyeceği saniye varsayılan aşabilir. Büyük dosyaları ile çalışırken ortaya çıkan bellek ve performans sorunları vardır.

Büyük dosya karşıya dosya yükleme denetimi zordur. Sunucuya dosya s içeriğini gönderilen gibi son kullanıcı kendi karşıya yükleme işlendiğini tüm onaysız patiently beklemeniz gerekir. Bu kadar çok sorun, birkaç saniye içinde yüklenebilir, ancak bir sorun karşıya yüklemek için dakika sürebilir büyük dosyalarla ilgilenirken olabilir daha küçük dosyalar ile ilgilenirken değildir. Büyük yüklemeleri işlemek için daha uygundur karşıya yükleme denetimi üçüncü taraf dosya çeşitli vardır ve bu satıcılardan çoğunu İlerleme göstergesi ve ActiveX çok daha zarif bir kullanıcı deneyimi sunmak yöneticileri karşıya sağlayın.

Büyük dosyaları işlemek için uygulamanız gereken dikkatle sorunları araştırmak ve belirli gereksinimlerinize göre uygun çözüm bulmak gerekir.

## <a name="summary"></a>Özet

İkili verileri yakalamak için gereken uygulama oluşturma belirli zorluklar getirir. Bu öğreticide biz ilk iki incelediniz: ikili verileri depolanacağı karar verme ve bir kullanıcının bir web sayfası aracılığıyla ikili içerik yüklemek için izin verme. Sonraki üç öğreticileri kendi metin veri alanları yanında ikili verileri görüntülemek nasıl yanı sıra veritabanındaki bir kaydı karşıya yüklenen veriler ilişkilendirmek nasıl göreceğiz.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Değer büyük veri türlerini kullanma](https://msdn.microsoft.com/library/ms178158.aspx)
- [Dosya yükleme denetimi QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 dosya yükleme sunucu denetimi](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Dosya koyu tarafında yükler](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve Bernadette Leigh yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](displaying-binary-data-in-the-data-web-controls-cs.md)
