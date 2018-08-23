---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Dosyalar (VB) yükleme | Microsoft Docs
author: rick-anderson
description: İkili dosyaları (örneğin, Word veya PDF belgeleri) yüklemek kullanıcıların Web sitenize sunucunun dosya sisteminde bunlar burada depolanabilir öğrenin...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fdf7b351152d9c64b0ac4b1bc9d558962e704543
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754109"
---
<a name="uploading-files-vb"></a>Dosya Yükleme (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) veya [PDF olarak indirin](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Kullanıcıların (örneğin, Word veya PDF belgeleri) ikili dosyaları karşıya yüklemesine izin vermek Web sitenizde nerede bunlar sunucunun dosya sistemini veya veritabanı depolanabilir öğrenin.


## <a name="introduction"></a>Giriş

Tüm öğreticileri biz şu ana kadar incelenir ve özel metin verilerle çalıştığınız. Ancak, birçok uygulama, hem metin hem de ikili verileri yakalama veri modelleri vardır. Bir çevrimiçi dating sitesi, kullanıcıların kendi profili ile ilişkilendirmek için bir resim karşıya yüklemesine izin verebilir. Personel arama Web sitesine, kullanıcıların kendi sürdürme bir Microsoft Word veya PDF belgesi olarak karşıya yükleme sağlayabilir.

İkili veriler ile çalışmaya yeni zorluklar ekler. Biz, uygulama ikili verilerin depolanma şeklini karar vermeniz gerekir. Yeni kayıtlar ekleme için kullanılan arabirimi bilgisayarlarını bir dosyadan karşıya yüklemek izin vermek için güncelleştirilmesi gerekir ve görüntülemek veya ilişkili ikili verileri bir kayıt s karşıdan yüklemek için bir yol sağlamak için ek adımlar atılmalıdır. Bu öğretici ve sonraki üç Biz bu zorluklar hurdle nasıl hakkında bilgi edineceksiniz. Bu öğretici sonunda bir resim ve PDF Broşürü her kategoriyle ilişkilendiren tam olarak işlevsel bir uygulama derledik. Belirli Bu öğreticide biz ikili verileri depolamak için kullanılan farklı teknikleri bakmak ve etkinleştirme kullanıcıları, bilgisayarları bir dosyadan karşıya yüklemek keşfetmek ve web sunucusu s dosya sisteminde kaydettiniz.

> [!NOTE]
> Bir uygulama s veri modelinin bir parçası olan ikili veriler bazen olarak adlandırılır bir [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), ikili büyük nesne için bir kısaltma. BLOB terimi eşanlamlı olmasına rağmen bu öğreticilerde miyim terminolojisi ikili verileri kullanmayı seçtiniz.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>1. adım: ikili verileri Web sayfalarıyla çalışma oluşturma

İkili veriler için destek ekleme ile ilgili sorunları araştırmak başlamadan önce ilk ASP.NET sayfaları ve bu öğreticinin sonraki üç yapmamız gereken bizim Web sitesi projesi oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `BinaryData`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![İkili verilerle ilgili öğreticiler için ASP.NET sayfaları ekleme](uploading-files-vb/_static/image1.gif)

**Şekil 1**: ikili verilerle ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `BinaryData` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image2.png))


Son olarak, girişleri olarak bu sayfalar ekleme `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme Enhancing sonra eklemeniz GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, ikili veri öğreticiler ile çalışmak için artık öğeleri içerir.


![Site Haritası artık ikili verileri öğreticiler ile çalışmaya yönelik girişleri içerir](uploading-files-vb/_static/image3.gif)

**Şekil 3**: Site Haritası artık ikili verileri öğreticiler ile çalışmaya yönelik girişleri içerir


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>2. adım: ikili veri Store nerede karar verme

Uygulama s veri modeli ile ilişkili olan ikili verileri iki yerlerden biri depolanabilir: veritabanında; dosyasına bir başvuru ile web s sunucusu dosya sisteminde veya doğrudan veritabanı (bkz: Şekil 4). Her yaklaşımın kendi kümesi Artıları ve eksileri vardır ve merits daha ayrıntılı bir açıklaması.


[![İkili veriler dosya sisteminde veya doğrudan veritabanında depolanabilir](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Şekil 4**: ikili veriler, dosya sisteminde veya doğrudan veritabanında depolanabilir ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image4.png))


Bir resim her ürünle birlikte ilişkilendirmek için Northwind veritabanı genişletmek istedik düşünün. Web s sunucusu dosya sisteminde bu görüntü dosyalarını depolamak ve yola kaydetmek için bir seçenek olacaktır `Products` tablo. Bu yaklaşımda, d eklediğimiz bir `ImagePath` sütuna `Products` tablo türünde `varchar(200)`, belki de. Web s sunucusu dosya sisteminde bir kullanıcı bir resim ayrıntılarını karşıya, o resmi depolanabilir `~/Images/Tea.jpg`burada `~` uygulama s fiziksel yolunu temsil eder. Diğer bir deyişle, web sitesi fiziksel yola kökü belirtilmiş ise `C:\Websites\Northwind\`, `~/Images/Tea.jpg` eşdeğer olacaktır `C:\Websites\Northwind\Images\Tea.jpg`. Görüntü dosyasını karşıya yükledikten sonra d Chai kaydında güncelleştiriyoruz `Products` tablo böylece kendi `ImagePath` sütununa başvuruda yeni görüntüyü yolu. Kullanabiliriz `~/Images/Tea.jpg` veya yalnızca `Tea.jpg` tüm ürün görüntüleri uygulama s'te konulabilir karar verirseniz `Images` klasör.

Dosya sistemi ikili verileri depolamanın ana avantajları şunlardır:

- **Kolay uygulama** depolamak ve almak doğrudan veritabanında depolanan ikili verileri kısa bir süre içinde anlatıldığı gibi dosya sistemi üzerinden verilerle zaman çalışmaya değerinden biraz daha fazla kod içerir. Ayrıca, görüntülemek veya ikili verileri indirmek bir kullanıcı için sırayla onlar bu verileri için bir URL ile sunulmalıdır. Veri web sunucusu s dosya sisteminde bulunuyorsa, basit bir URL'dir. Veriler bir veritabanında depolanır ancak, bir web sayfasında almak ve dönüş verileri veritabanından oluşturulması gerekiyor.
- **İkili veriler için daha geniş erişim** ikili verileri diğer hizmetler veya uygulamalar, kendi veritabanındaki verileri çekme gerçekleştirilemiyor erişilebilir olması gerekebilir. Örneğin, her ürün ile ilişkili görüntü ayrıca kullanıcılara aracılığıyla kullanılabilir olması gerekebilir [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), bu durumda d dosya sisteminde ikili verileri depolamak istiyoruz.
- **Performans** ikili veriler dosya sisteminde depolanan, isteğe bağlı ve Ağ Tıkanıklığı veritabanı sunucusu ve web sunucusu arasında ikili veriler doğrudan veritabanında depolanıyorsa değerinden olacaktır.

Dosya sistemi ikili verileri depolamanın ana dezavantajı, veritabanındaki verileri ayırır ' dir. Uygulamasından bir kaydı silinirse `Products` tablo, ilişkili dosya web s sunucusu dosya sisteminde otomatik olarak silinmez. Dosya veya dosya sistemi silmeye ek bir kod kullanılmayan, yalnız bırakılmış dosyalarıyla dağınık hale gelecektir yazmanız gerekir. Ayrıca, veritabanını yedeklerken, biz dosya sistemine de ilişkili ikili veri yedeklemeleri yapmak emin olmanız gerekir. Veritabanını başka bir site veya sunucu yürütmelisiniz benzer zorlukları taşıma.

Alternatif olarak, ikili veriler doğrudan bir Microsoft SQL Server 2005 veritabanında türünde bir sütun oluşturarak depolanabilir `varbinary`. Gibi diğer değişken uzunluklu veri türleriyle tutulabilir ikili verileri en çok bu sütunda belirtebilirsiniz. Örneğin, ayrılan en fazla 5000 bayt kullanın `varbinary(5000)`; `varbinary(MAX)` yaklaşık 2 GB maksimum depolama boyutu için izin verir.

İkili verileri ve veritabanı kaydını arasında sıkı eşleştirme doğrudan veritabanında ikili verileri depolamanın ana avantajlarındandır. Bu, veritabanı yönetim görevleri, yedeklemeler veya farklı bir site veya sunucu için veritabanı taşıma gibi büyük ölçüde kolaylaştırır. Ayrıca, otomatik olarak bir kayıt silindiğinde, karşılık gelen ikili verileri siler. İkili verileri depolamanın veritabanında daha fazla Zarif avantajları da vardır. Bkz: [depolama ikili dosyaları doğrudan veritabanı kullanarak ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) daha ayrıntılı bir tartışma için.

> [!NOTE]
> Microsoft SQL Server 2000 ve önceki sürümlerinde, `varbinary` veri türü, 8000 bayt sayısı üst sınırına sahip. En fazla 2 GB ikili verileri depolamak için [ `image` veri türü](https://msdn.microsoft.com/library/ms187993.aspx) yerine kullanılması gerekir. Ek olarak `MAX` SQL Server 2005, ancak `image` veri türü kullanımdan kaldırıldı. Bunu s hala desteklenmektedir için geriye dönük uyumluluk, ancak Microsoft, duyurulan `image` veri türü SQL Server'ın gelecek bir sürümde kaldırılacak.


Eski bir veri modeli ile çalışıyorsanız görebileceğiniz `image` veri türü. Northwind veritabanı s `Categories` tablolu bir `Picture` kategorisi için bir görüntü dosyasının ikili verileri depolamak için kullanılan sütun. Microsoft Access ve SQL Server'ın önceki sürümlerinde, kök Northwind veritabanı olduğundan, bu sütun türüdür `image`.

Bu öğretici ve sonraki üç için iki yaklaşımı kullanacağız. `Categories` Tablosu zaten bir `Picture` kategorisi için bir görüntü ikili içeriğini depolamak için sütun. Ek bir sütun ekleyeceğiz `BrochurePath`web s sunucusu dosya sisteminde yazdırma kaliteli, parlak genel bakış kategorisi sağlamak için kullanılan bir PDF yola depolamak için.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>3. adım: Ekleme`BrochurePath`sütuna`Categories`tablo

Şu anda kategoriler tablosunu yalnızca dört sütun vardır: `CategoryID`, `CategoryName`, `Description`, ve `Picture`. Bu alanlara ek olarak, biz (varsa) için kategori s Broşürü işaret edecek yeni bir tane eklemeniz gerekir. Bu sütunu eklemek için sağ tıklayın, tablolara Sunucu Gezgini için detaya gidin `Categories` tablosu ve tablo tanımını Aç'ı seçin (bkz: Şekil 5). Sunucu Gezgini'ni görmüyorsanız, Görünüm menüsünden Sunucu Gezgini seçeneğini belirleyerek getirin veya Ctrl + Alt + S tuşlarına basın.

Yeni bir `varchar(200)` sütuna `Categories` adlı tablo `BrochurePath` ve verir `NULL` s Kaydet simgesine tıklayın (veya Ctrl + S isabet).


[![Kategorileri tablosuna BrochurePath sütun ekleme](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Şekil 5**: ekleme bir `BrochurePath` sütuna `Categories` tablo ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>4. adım: Mimari kullanacak biçimde güncelleştirme`Picture`ve`BrochurePath`sütunları

`CategoriesDataTable` Dört veri erişim katmanı (DAL) şu anda sahip `DataColumn` tanımlanan s: `CategoryID`, `CategoryName`, `Description`, ve `NumberOfProducts`. Ne zaman ilk olarak tasarladığımız Bu DataTable [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğreticide `CategoriesDataTable` yalnızca ilk üç sütun vardı; `NumberOfProducts` sütunu eklendi [kullanarak bir madde işaretli ana/ayrıntı Bir Ayrıntılar DataList'i ile ana kayıt listesi](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) öğretici.

Bölümünde açıklandığı gibi *veri erişim katmanını oluşturma*, iş nesneleri türü belirtilmiş veri kümesi DataTable olun. TableAdapter veritabanı ile iletişim ve iş nesnelerini sorgu sonuçlarıyla dolduruluyor sorumludur. `CategoriesDataTable` Tarafından doldurulur `CategoriesTableAdapter`, üç veri alma yöntemi vardır:

- `GetCategories()` TableAdapter s ana sorguyu yürütür ve döndürür `CategoryID`, `CategoryName`, ve `Description` tüm kayıtları alanlarının `Categories` tablo. Ana sorguda ne otomatik olarak oluşturulan tarafından kullanılır `Insert` ve `Update` yöntemleri.
- `GetCategoryByCategoryID(categoryID)` döndürür `CategoryID`, `CategoryName`, ve `Description` alanları kategorisi olan `CategoryID` eşittir *CategoryID*.
- `GetCategoriesAndNumberOfProducts()` -döndürür `CategoryID`, `CategoryName`, ve `Description` tüm kayıtlar için alanları `Categories` tablo. Ayrıca, her kategori ile ilişkili ürünleri sayısını döndürmek için sorgu kullanır.

Bildirimi bunlardan hiçbiri iade sorgular `Categories` tablo s `Picture` veya `BrochurePath` sütunların; ya da mu `CategoriesDataTable` sağlamak `DataColumn` bu alanlar için s. Resim ile çalışmak için ve `BrochurePath` özellikleri ihtiyacımız ilk eklemek üzere `CategoriesDataTable` ve ardından güncelleştirme `CategoriesTableAdapter` bu sütunların döndürülecek sınıfı.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Ekleme`Picture`ve`BrochurePath``DataColumn` s

Başlamak için bu iki sütunu ekleyerek `CategoriesDataTable`. Sağ `CategoriesDataTable` s Üstbilgi Ekle bağlam menüsünden seçin ve sonra sütun seçeneği. Bu yeni bir oluşturur `DataColumn` adlı DataTable `Column1`. Bu sütunun adını `Picture`. Özellikler penceresinde ayarlayın `DataColumn` s `DataType` özelliğini `System.Byte[]` (Bu bir seçenek aşağı açılan listesinde değil; içine yazmanız gerekir).


[![Bir DataColumn adlı, veri türü olan System.Byte [] resim oluşturma](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Şekil 6**: oluşturma bir `DataColumn` adlandırılmış `Picture` olan `DataType` olduğu `System.Byte[]` ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image8.png))


Başka bir `DataColumn` DataTable tablosuna adlandırma `BrochurePath` varsayılan kullanılarak `DataType` değeri (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Döndüren`Picture`ve`BrochurePath`TableAdapter değerleri

Bu iki ile `DataColumn` eklenen s `CategoriesDataTable`, biz güncelleştirmeye hazır re `CategoriesTableAdapter`. Biz hem ana TableAdapter sorgusunda döndürülen bu sütun değerleri olabilir, ancak bu geri her zaman ikili veri sunacağı `GetCategories()` yöntemi çağrıldı. Bunun yerine, let s güncelleştirme geri getirmek için ana TableAdapter sorgusu `BrochurePath` ve belirli bir kategoriye s döndüren bir ek veri alma yöntemi oluşturma `Picture` sütun.

Ana TableAdapter sorgu güncelleştirmek için sağ `CategoriesTableAdapter` s üstbilgi ve bağlam menüsünden yapılandırma seçeneğini kullanın. Bu tablo bağdaştırıcısı Yapılandırma Sihirbazı'nı, hangi biz getirir ve geçmiş öğreticiler bir süre içinde görülen. Geri getirmek için bu sorguyu güncelleyin `BrochurePath` ve Son'a tıklayın.


[![Güncelleştirme SELECT deyiminde BrochurePath ayrıca döndürülecek sütun listesi](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Şekil 7**: sütun listesinde güncelleştirmek `SELECT` aynı zamanda sonuç ifadesine `BrochurePath` ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image10.png))


Geçici SQL deyimleri için TableAdapter'ı kullanırken, ana sorgu sütunu listesi güncelleniyor sütun listesi için tüm güncelleştirmeleri `SELECT` sorgu TableAdapter yöntemleri. Anlamına `GetCategoryByCategoryID(categoryID)` yöntemi, döndürülecek güncelleştirildi `BrochurePath` biz amaçlanan olabilecek sütun. Ancak, sütun listesinde da güncelleştirilmiş `GetCategoriesAndNumberOfProducts()` yöntemi, her kategori için ürün sayısı veren kaldırmayı! Bu nedenle, bu yöntem s güncelleştirmek ihtiyacımız `SELECT` sorgu. Sağ `GetCategoriesAndNumberOfProducts()` yöntemini Yapılandır'ı seçin ve geri `SELECT` sorgu özgün değeri geri dön:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Ardından, belirli bir kategoriye s döndüren yeni bir TableAdapter yöntemi oluşturma `Picture` sütun değeri. Sağ `CategoriesTableAdapter` s üstbilgi ve TableAdapter sorgu Yapılandırma Sihirbazı'nı başlatmak için Sorgu Ekle seçeneğini belirleyin. Bu sihirbazın ilk adımı bize biz geçici SQL deyimi kullanarak verileri sorgulamak istiyorsanız, yeni bir saklı yordam veya mevcut bir ister. SQL deyimi Kullan'ı seçin ve İleri'ye tıklayın. Biz bir satır döndüren olduğundan, satır seçeneği ikinci adımda döndüren Seç.


[![SQL deyimi kullan seçeneğini seçin.](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Şekil 8**: SQL deyimi kullan seçeneğini seçin ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image12.png))


[![Sorgu kategoriler tablosundan bir kayıt döndürür beri Seç seçin satır döndüren](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Şekil 9**: Sorgu kategorileri tablosundan seçin satır döndüren seçin bir kaydı döndürür bu yana ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image14.png))


Üçüncü adımda, aşağıdaki SQL sorgusunu girin ve İleri'ye tıklayın:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

Son adım, yeni yöntemin adı seçmektir. Kullanım `FillCategoryWithBinaryDataByCategoryID` ve `GetCategoryWithBinaryDataByCategoryID` dolgu bir DataTable ve dönüş DataTable, sırasıyla desen. Sihirbazı tamamlamak için Son'u tıklatın.


[![TableAdapter s yöntemleri adlarını seçin](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Şekil 10**: TableAdapter s yöntemleri adlarını seçin ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Tablo bağdaştırıcısı sorgu Yapılandırma Sihirbazı'nı tamamladıktan sonra bir veri şeması yeni komut metni ana sorgunun şemasından farklı döndürür bildiren bir iletişim kutusu görebilirsiniz. Kısacası, sihirbaz dikkate alınarak TableAdapter s ana sorguda `GetCategories()` oluşturduğumuz olandan farklı bir şeması döndürür. Ancak, bu iletiyi yoksayabilirsiniz şekilde istediğimiz gibi budur.


Ayrıca, göz önünde bulundurun geçici SQL deyimlerini kullanarak ve daha sonraki bir noktada TableAdapter s ana sorgu zamanında değiştirmek için sihirbazı kullanın, onu değiştirir `GetCategoryWithBinaryDataByCategoryID` metodu s `SELECT` deyimi s sütun listesi yalnızca bu sütunları eklemek için Ana sorguda (diğer bir deyişle, kaldıracak `Picture` sorgudan sütunu). Döndürülecek sütun listesi el ile güncelleştirmeniz gerekecektir `Picture` sütun ne biz ile benzer `GetCategoriesAndNumberOfProducts()` Bu adımda yöntemi.

İki ekledikten sonra `DataColumn` s `CategoriesDataTable` ve `GetCategoryWithBinaryDataByCategoryID` yönteme `CategoriesTableAdapter`, yazılan veri kümesi Tasarımcısı'nda bu sınıfların Şekil 11'de ekran görüntüsü gibi görünmelidir.


![DataSet Designer yeni sütunlar ve yöntemi içerir.](uploading-files-vb/_static/image11.gif)

**Şekil 11**: DataSet Designer yeni sütunlar ve yöntemi içerir.


## <a name="updating-the-business-logic-layer-bll"></a>İş mantığı katmanı'nı (BLL) güncelleştiriliyor

Güncelleştirilmiş DAL ile kalan tek şey yeni bir yöntem eklemek için iş mantığı katmanı (BLL) artırmak için `CategoriesTableAdapter` yöntemi. Aşağıdaki yöntemi ekleyin `CategoriesBLL` sınıfı:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>5. adım: Web sunucusu istemciden bir dosyaya karşıya yükleme

Önerilmesine ikili verileri toplarken, bu verileri bir son kullanıcı tarafından sağlanır. Bu bilgileri yakalamak için kullanıcı bir dosyadan bilgisayarlarını web sunucusuna yükleyebildiğini olması gerekir. Karşıya yüklenen veriler daha sonra web sunucusu s dosya sistemi ve veritabanı dosyasına bir yol ekleme veya ikili içeriği doğrudan veritabanına yazma, dosyayı kaydetmeden gelebilir veri modeli ile tümleştirilmesi gerekir. Bu adımda bilgisayarlarını sunucuya dosyaları karşıya yükleme olanağı sağlamak nasıl şu konuları inceleyeceğiz. Sonraki öğreticide biz karşıya yüklenen dosya veri modeli ile tümleştirmek için uygulamamızla açacağım.

ASP.NET 2.0 yenilikler [FileUpload Web denetimi](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) kullanıcıların web sunucusuna bilgisayarlarından bir dosya göndermek bir mekanizma sağlar. FileUpload denetim olarak işler bir `<input>` öğesi olan `type` özniteliği olarak bir Gözat düğmesi ile TextBox'a tarayıcıları görüntüler dosyasını ayarlanır. Göz at düğmesine tıklatıp, kullanıcının bir dosya seçebileceği bir iletişim kutusunu açar. Formu yeniden gönderildiğinde Seçili dosyanın s içeriği geri gönderme ile birlikte gönderilir. Sunucu tarafında karşıya yüklenen dosya hakkındaki bilgileri FileUpload denetim s özellikleri erişilebilir.

Karşıya yükleme dosyaları göstermek için açık `FileUpload.aspx` sayfasını `BinaryData` klasöründe FileUpload Denetim Tasarımcısı araç kutusundan sürükleyin ve denetimi s ayarlama `ID` özelliğini `UploadTest`. Ardından, bir düğme Web denetim ayarı ekleyin, `ID` ve `Text` özelliklerine `UploadButton` ve sırasıyla seçili dosyasını karşıya yükleyin. Son olarak, Temizle, düğmenin altına bir etiket Web Denetimi yerleştirmek kendi `Text` özelliği ve kümesi kendi `ID` özelliğini `UploadDetails`.


[![ASP.NET sayfası için FileUpload denetim ekleme](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Şekil 12**: ASP.NET sayfası için FileUpload denetim ekleme ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image18.png))


Şekil 13, bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir. Göz at düğmesine tıklayarak bir dosya seçimi iletişim kutusunu bilgisayarlarını dosyasından kullanıcının getirir unutmayın. Bir dosyayı seçtikten sonra seçili dosyayı karşıya yükle düğmesine tıklayarak seçili dosya s ikili içerik web sunucusuna gönderen geri göndermeye neden olur.


[![Kullanıcı bilgisayarlarından sunucuya karşıya yüklenecek dosyayı seçebilirsiniz.](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Şekil 13**: kullanıcının bir dosya yüklemek için sunucu bilgisayarlarına seçebilirsiniz ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image20.png))


Geri gönderme, karşıya yüklenen dosya, dosya sistemine kaydedilebilir veya ikili verileri ile bir Stream doğrudan çalışılabilmesi. Bu örnekte, s oluşturmak istiyorum bir `~/Brochures` klasörü ve karşıya yüklenen dosyayı kaydedin. Başlangıç ekleyerek `Brochures` site kök dizininin bir alt klasörü. Ardından, bir olay işleyicisi oluşturun `UploadButton` s `Click` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Çeşitli özellikler, karşıya yüklenen veriler ile çalışmak için FileUpload denetim sağlar. Örneği için [ `HasFile` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) bir dosyayı kullanıcı tarafından yüklenen gösterir ancak [ `FileBytes` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) bayt dizisi karşıya yüklenen ikili verilere erişim sağlar. `Click` Olay işleyicisini başlatır sağlayarak bir dosyayı karşıya yüklendi. Bir dosya karşıya yüklediyseniz, etiket karşıya yüklenen dosya, bayt cinsinden boyutunu ve kendi içerik türü adını gösterir.

> [!NOTE]
> Kullanıcı kontrol edebilirsiniz bir dosya yükler emin olmak için `HasFile` özelliği ve bir uyarı görüntüler, s `False`, size kullanabilir veya [RequiredFieldValidator denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) bunun yerine.


S FileUpload `SaveAs(filePath)` karşıya yüklenen dosya belirtilen kaydeder *filePath*. *filePath* olmalıdır bir *fiziksel yolu* (`C:\Websites\Brochures\SomeFile.pdf`) yerine *sanal* *yolu* (`/Brochures/SomeFile.pdf`). [ `Server.MapPath(virtPath)` Yöntemi](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) sanal yolu alır ve karşılık gelen fiziksel yolu döndürür. Sanal yol işte `~/Brochures/fileName`burada *fileName* karşıya yüklenen dosya adıdır. Bkz: [kullanarak Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) sanal ve fiziksel yollarını ve kullanma hakkında daha fazla bilgi için `Server.MapPath`.

Tamamladıktan sonra `Click` olay işleyicisi sayfası tarayıcıda test etmek için bir dakikanızı ayırın. Gözat düğmesine tıklayın ve sabit diskinizden bir dosya seçin ve ardından seçili dosyayı karşıya yükle düğmesine tıklayın. Geri gönderme Seçili dosyanın içeriği web sunucusu ve ardından ona kaydetmeden önce dosya hakkındaki bilgileri görüntüler gönderir `~/Brochures` klasör. Dosyayı karşıya yükledikten sonra Visual Studio'ya geri dönün ve Çözüm Gezgini yenile düğmesine tıklayın. Yalnızca ~/Brochures klasörde karşıya dosya görmeniz gerekir!


[![Web sunucusuna dosya EvolutionValley.jpg karşıya yüklendi](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Şekil 14**: dosya `EvolutionValley.jpg` karşıya yüklendiğinden Web sunucusuna ([tam boyutlu görüntüyü görmek için tıklatın](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg ~/Brochures klasörüne kaydedildi](uploading-files-vb/_static/image15.gif)

**Şekil 15**: `EvolutionValley.jpg` için kaydedilmiş `~/Brochures` klasörü


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Karşıya yüklenen dosyaları dosya sistemine kaydetme ile ıot'nin

Web sunucusu s dosya sistemine dosya karşıya yükleme kaydedilirken ele alınması birkaç ıot'nin vardır. İlk olarak, burada s sorunu güvenlik. Bir dosyayı dosya sistemine kaydetmek için güvenlik bağlamı altında ASP.NET sayfasını yürütüyor yazma izinlerine sahip olmalıdır. ASP.NET Geliştirme Web sunucusu, geçerli kullanıcı hesabının bağlamında çalışır. Microsoft s Internet Information Services (IIS) web sunucusu olarak kullanıyorsanız, güvenlik bağlamı IIS yapılandırmasına ve sürümüne bağlıdır.

Dosya adlandırma geçici dosyaları dosya sistemine kaydetme bir diğer zorluk döner. Şu anda sayfamızı için karşıya yüklenen dosyaların tümünü kaydeder `~/Brochures` s istemci bilgisayarda dosyası olarak aynı adı kullanarak dizin. Kullanıcı bir ada sahip bir Broşürü yüklerse `Brochure.pdf`, dosya şu biçimde kaydedilecek: `~/Brochure/Brochure.pdf`. Ancak biraz daha sonra B kullanıcısı aynı adı sahip olan farklı Broşürü dosya ne yükler (`Brochure.pdf`)? Kod ile şimdi, kullanıcı s dosyası B kullanıcısı s karşıya yükleme ile üzerine yazılacak sahibiz.

Çeşitli dosya adı çakışmalarını çözmek için teknikler vardır. Zaten varsa aynı ada sahip bir dosya karşıya yasaklanmayacağını bir seçenektir. B kullanıcısı adlı bir dosya karşıya yükleme girişiminde bulunduğunda bu yaklaşımdaki `Brochure.pdf`, sistem değil, dosyayı kaydedin ve bunun yerine kullanıcı dosyayı yeniden adlandırın ve yeniden denemek için B bildiren bir ileti görüntüler. Olabilir benzersiz dosya adları kullanarak bu dosyayı kaydetmek için başka bir yaklaşımdır bir [genel benzersiz tanıtıcısı (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) veya birincil anahtar sütunları kayıt s veritabanı öğelerinden karşılık gelen değeri (karşıya yükleme ilişkili olduğu varsayılarak bir belirli bir satır veri modelindeki). Sonraki öğreticide daha ayrıntılı bir şekilde bu seçenekler şunları keşfedeceğiz.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>İkili veri çok büyük miktarlardaki ilgili zorlukları

Bu öğreticileri yakalanan ikili verilerin boyutu büyüklükteki olduğunu varsayalım. Daha büyük veya çok büyük miktarlardaki birkaç megabayt olan ikili veri dosyaları ile çalışma bu öğreticileri kapsamı dışındaki yeni zorluklara neden olur. Bu, aracılığıyla yapılandırılabilir olsa da örneğin, varsayılan ASP.NET 4 MB'tan fazla karşıya yükleme reddeder [ `<httpRuntime>` öğesi](https://msdn.microsoft.com/library/e1f13641.aspx) içinde `Web.config`. IIS çok kendi dosya karşıya yükleme boyutu sınırlamaları uygular. Bkz: [IIS karşıya dosya boyutu](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) daha fazla bilgi için. Ayrıca, büyük dosyaları karşıya yükleme işleminin süresi 110 ASP.NET isteği bekleyeceği saniye varsayılan aşabilir. Büyük dosyalarla çalışırken ortaya çıkan bellek ve performans sorunları vardır.

Büyük dosya yüklemeleri için FileUpload denetim zordur. Dosyanın s içeriği sunucuya gönderilen değerler gibi son kullanıcı alabilir, karşıya yükleme İlerliyor onaysız beklemeniz gerekir. Bu kadar çok sorun, birkaç saniye içinde karşıya yüklenebilir, ancak karşıya yüklemek için dakika sürebilir, daha büyük dosyalarla ilgili bir sorun olabilir, daha küçük dosyalar ile ilgilenirken değildir. Daha büyük karşıya işlemek için uygun yükleme denetimleri çeşitli üçüncü taraf dosya vardır ve bu satıcıların sunduğu birçok İlerleme göstergesi ve ActiveX daha parlak bir kullanıcı deneyimi yöneticileri karşıya sağlayın.

Uygulamanızın büyük dosyaları işlemek gerekiyorsa, dikkatli bir şekilde sorunları araştırın ve belirli gereksinimlerinize uygun çözümleri bulmak gerekir.

## <a name="summary"></a>Özet

İkili verileri yakalamak için gerekli bir uygulama oluşturmak, belirli zorluklar ortaya çıkarır. Biz bu öğreticide ilk iki incelediniz: ikili verileri depolamak konuma karar vermede ve bir web sayfası aracılığıyla ikili içerik yüklemek bir kullanıcı izin verme. Karşıya yüklenen veriler kaydını veritabanında ilişkilendirmek nasıl yanı sıra kendi metin veri alanları yanı sıra ikili verilerin nasıl görüntüleneceğini sonraki üç öğreticiler, göreceğiz.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Büyük değerli veri türlerini kullanma](https://msdn.microsoft.com/library/ms178158.aspx)
- [FileUpload denetim hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 FileUpload sunucu denetimi](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Dosya yüklemeleri koyu tarafında](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Teresa Murphy ve Bernadette Leigh yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](updating-and-deleting-existing-binary-data-cs.md)
> [İleri](displaying-binary-data-in-the-data-web-controls-vb.md)
