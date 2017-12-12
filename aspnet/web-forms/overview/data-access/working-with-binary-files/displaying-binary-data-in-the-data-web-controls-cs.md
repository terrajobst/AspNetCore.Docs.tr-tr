---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: "İkili verileri veri Web görüntüleme denetimleri (C#) | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide Web bir görüntü dosyası görünümünü ve 'İndirme' bağlantısının f sağlanmasını dahil olmak üzere bir sayfada, ikili verileri sunmak için seçenekleri ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 09482ef453e9e8efa4a2721b9fe628d2a58dd53c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Veri Web denetimleri (C#) ikili verileri görüntüleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) veya [PDF indirin](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> Bu öğreticide bir görüntü dosyası görünümünü ve 'İndirme' bağlantısı sağlamak için bir PDF dosyası dahil olmak üzere bir Web sayfasında ikili verileri sunmak için seçenekleri ele.


## <a name="introduction"></a>Giriş

Önceki öğreticide ikili veriler bir uygulama s temel alınan veri modeli ile ilişkilendirmek için iki teknikleri incelediniz ve web sunucusu s dosya sistemine tarayıcıdan dosyaları karşıya yüklemek için kullanılan dosya yükleme denetim. Biz ve henüz karşıya yüklenen ikili veriler veri modeli ile ilişkilendirmek bkz. Diğer bir deyişle, bir dosyayı karşıya ve dosya sistemine kaydettikten sonra dosya yolunu uygun veritabanı kaydında depolanması gerekir. Doğrudan veritabanında depolanan verileri, ardından karşıya yüklenen ikili veri dosya sistemine kaydedilmedi, ancak veritabanına eklenmesi gerekir.

Ancak, verileri veri modeli ile ilişkilendirme adresindeki Ara önce son kullanıcıya ikili veri sağlamak nasıl ilk bakmak s olanak tanır. Metin veri sunma kadar basit olmakla birlikte, ikili veri gösterilme? Bu, doğal olarak, ikili veri türüne bağlıdır. Görüntüleri için büyük olasılıkla görüntüyü istiyoruz; PDF için Microsoft Word belgelerini, ZIP dosyaları ve diğer indirme bağlantısı sağlayan ikili veri türleri büyük olasılıkla daha uygun.

İkili verileri kullanarak veri ilişkili metin verilerini yanında sunmak nasıl ele alacağız Bu öğreticide Web GridView ve DetailsView gibi denetler. Sonraki öğreticide biz veritabanı ile yüklenen bir dosya ilişkilendirme için uygulamamızla kapatmanız.

## <a name="step-1-providingbrochurepathvalues"></a>1. adım: Sağlama`BrochurePath`değerleri

`Picture` Sütununda `Categories` tablosu zaten çeşitli kategori görüntüleri için ikili veriler içerir. Özellikle, `Picture` her kayıt için sütun karlı, düşük kalite, 16 renkli bit eşlem resim ikili içeriğini tutar. Her kategori görüntü 172 piksel geniş ve 120 piksel uzunluğunda ve yaklaşık 11 KB kullanır. Daha fazla hangi s, ikili içeriği `Picture` sütununu içeren 78 baytlık [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) görüntü görüntülemeden önce atılması gereken üstbilgi. Northwind veritabanı Microsoft Access içinde kendi kökleri olduğundan bu üst bilgileri mevcuttur. Erişim'deki bu üst bilgisinde tacks OLE nesnesi veri türünü kullanarak ikili veri depolanır. Şimdilik, resmi görüntülemek için bu düşük kaliteli görüntüler üstbilgileri Şerit nasıl göreceğiz. Sonraki öğreticide biz kategori s güncelleştirmek için bir arabirim yapı `Picture` sütun ve gereksiz OLE üstbilgileri olmadan eşdeğer JPG görüntülerle OLE üstbilgileri kullanın Bu bit eşlem görüntüleri değiştirin.

Önceki öğreticide dosya yükleme denetiminin nasıl kullanıldığını gördünüz. Bu nedenle, devam edin ve web sunucusu s dosya sistemine Broşürü dosyaları ekleyin. Bunun yapılması, ancak güncelleştirilmediği `BrochurePath` sütununda `Categories` tablo. Sonraki öğreticide bunu gerçekleştirmek nasıl göreceğiz ancak şimdilik biz değerleri bu sütun için el ile belirtmeniz gerekir.

Bu öğretici s indirme yedi PDF Broşürü dosyalarında bulacaksınız `~/Brochures` klasörü, Deniz ürünleri dışında kategorilerin her biri için bir. Var senaryoları tüm kayıtları ikili veri burada ilişkilendirdiğiniz ne yapılacağını göstermek için Deniz ürünleri Broşürü ekleme atlanmış. Güncelleştirilecek `Categories` tablo bu değerlerle, sağ tıklayın `Categories` Sunucu Gezgini düğümden ve tablo verileri Göster'i seçin. Ardından, Şekil 1 gösterildiği gibi bir Broşürü sahip her kategori için Broşürü dosyaları için sanal yol girin. Deniz ürünleri kategorisi için hiçbir Broşürü olduğundan bırakın, `BrochurePath` s sütunu değeri olarak `NULL`.


[![El ile kategorileri tablo s BrochurePath sütunu için değerler girin](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Şekil 1**: değerlerini el ile girmeniz `Categories` s tablosu `BrochurePath` sütun ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>2. adım: GridView içinde broşürler için indirme bağlantısı sağlama

İle `BrochurePath` için sağlanan değerler `Categories` tablo, biz re her kategori kategori s Broşürü indirmek için bir bağlantı ile birlikte listeleyen GridView oluşturmak için hazır. Adım 4'te biz de kategori s görüntüyü görüntülemek için bu GridView genişletmeniz.

Başlangıç tasarımcısına Kutusu'ndan GridView sürükleyerek `DisplayOrDownloadData.aspx` sayfasındaki `BinaryData` klasör. GridView s ayarlamak `ID` için `Categories` ve yeni bir veri kaynağına bağlanmak GridView s akıllı etiket seçin. Özellikle, adlandırılmış bir ObjectDataSource bağlamak `CategoriesDataSource` verileri kullanarak alır `CategoriesBLL` s nesnesi `GetCategories()` yöntemi.


[![CategoriesDataSource adlı yeni bir ObjectDataSource oluşturma](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Şekil 2**: yeni ObjectDataSource adlandırılmış oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![ObjectDataSource CategoriesBLL sınıfını kullanmak için yapılandırma](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![GetCategories() yöntemi kullanarak kategorileri listesini alma](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Şekil 4**: listesi, kategorileri kullanarak almak `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak bir BoundField ekleyecektir `Categories` için GridView `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, ve `BrochurePath` `DataColumn` s. Bir tane kaldırmak `NumberOfProducts` bu yana BoundField `GetCategories()` s yöntemi sorgu bu bilgileri alamadı. Ayrıca kaldırmak `CategoryID` BoundField ve yeniden adlandırma `CategoryName` ve `BrochurePath` BoundFields `HeaderText` kategori ve Broşürü, özelliklerine sırasıyla. Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntüleme (bkz. Şekil 5). Sekiz kategorilerin her biri listelenir. Yedi kategori ile `BrochurePath` değerlere sahip `BrochurePath` içinde ilgili BoundField görüntülenen değeri. Sahip Deniz ürünleri bir `NULL` değerini kendi `BrochurePath`, boş bir hücre görüntüler.


[![Her kategori s adını, açıklamasını ve BrochurePath değerini listelenir](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Şekil 5**: her kategori s adı, açıklama ve `BrochurePath` değeri listelenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


Metnin görüntüleme yerine `BrochurePath` sütun istiyoruz Broşürü bir bağlantı oluşturmak. Bunu başarmak için kaldırmak `BrochurePath` BoundField ve HyperLinkField ile değiştirin. Yeni HyperLinkField s ayarlamak `HeaderText` Broşürü, özelliğine kendi `Text` görünüm Broşürü özelliğine ve kendi `DataNavigateUrlFields` özelliğine `BrochurePath`.


![Bir HyperLinkField BrochurePath için ekleme](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Şekil 6**: HyperLinkField için ekleme`BrochurePath`


Şekil 7'de gösterildiği gibi bu GridView için bağlantılar bir sütun ekler. Bir görünüm Broşürü bağlantıyı tıklatmak PDF doğrudan tarayıcıda görüntülemek veya bir PDF okuyucu yüklü olup olmadığını bağlı olarak dosyasını karşıdan yüklemek için kullanıcı ve tarayıcı s ayarlarını ister.


[![Bir kategori s Broşürü görünüm Broşürü bağlantıyı tıklatarak görüntülenebilir.](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Şekil 7**: görünümü Broşürü bağlantısına tıklayarak bir kategori s Broşürü görüntülenebilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![S Broşürü PDF kategori görüntülenir](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Şekil 8**: Kategori s Broşürü PDF görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Bir Broşürü olmadan kategorileri için Görünüm Broşürü metni gizleme

Şekil 7'de görüldüğü gibi `BrochurePath` HyperLinkField görüntüler kendi `Text` mi bağımsız olarak tüm kayıtlar için özellik değeri (Görünüm Broşürü) orada s olmayan bir`NULL` değerini `BrochurePath`. Doğal olarak, varsa `BrochurePath` olan `NULL`, bağlantı yalnızca metin olarak görüntülenir sonra Deniz ürünleri kategorisiyle olduğu gibi (geri Şekil 7'ye bakın). Metin görünümü Broşürü görüntüleme yerine, bu kategorilerin olmadan iyi olabilir bir `BrochurePath` değer Broşürü yok gibi bazı alternatif metin, görüntüler.

Bu davranışı sağlamak için içeriğe göre uygun çıktıyı yayar sayfa yöntemine yapılan bir çağrı aracılığıyla oluşturulur TemplateField kullanmak ihtiyacımız `BrochurePath` değeri. Biz bu tekniği biçimlendirme önce keşfedilen geri [kullanarak TemplateFields GridView denetiminde](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) Öğreticisi.

Seçerek TemplateField'a HyperLinkField kapatma `BrochurePath` HyperLinkField tıklayıp ardından üzerinde dönüştürme Bu alan TemplateField'a sütunları Düzenle iletişim kutusunda bağlantı.


![HyperLinkField TemplateField'a Dönüştür](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Şekil 9**: HyperLinkField TemplateField'a Dönüştür


Bu TemplateField ile oluşturacak bir `ItemTemplate` içeren bir köprü Web özelliği kontrol `NavigateUrl` özelliği bağlı `BrochurePath` değeri. Yöntemine yapılan bir çağrıyla bu biçimlendirme yerine `GenerateBrochureLink`, değeri geçen `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

Ardından, oluşturun bir `protected` ASP.NET yönteminde sayfa adında s arka plandaki kod sınıfı `GenerateBrochureLink` döndüren bir `string` ve kabul eden bir `object` giriş parametresi olarak.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Bu yöntem belirler geçilen `object` değeri olan bir veritabanı `NULL` ve bu durumda, kategori broşürlerde eksik olduğunu belirten bir ileti döndürür. Aksi durumda, yoksa bir `BrochurePath` değeri, onu bir köprü görüntülenen s. Unutmayın `BrochurePath` değer içine geçirilen s sunmak [ `ResolveUrl(url)` yöntemi](https://msdn.microsoft.com/en-us/library/system.web.ui.control.resolveurl.aspx). Geçilen bu metodu çözümler *url*değiştirerek `~` uygun sanal yolu ile karakter. Örneğin, uygulama köklü `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` döndürülecek `/Tutorial55/Brochures/Meat.pdf`.

Bu değişiklikler uygulandıktan sonra Şekil 10 sayfası gösterilir. Unutmayın Deniz ürünleri kategori s `BrochurePath` alan şimdi Broşürü yok metin görüntüler.


[![Bu kategoriler olmadan bir Broşürü için metin yok Broşürü kullanılabilir görüntülenir](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Şekil 10**: metin Broşürü yok Bu kategoriler olmadan bir broşürlerde görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>3. adım: Kategori s resim görüntülemek için bir Web sayfası ekleme

Bir kullanıcı bir ASP.NET sayfasının ziyaret ettiğinde, ASP.NET sayfası s HTML alırlar. Alınan HTML yalnızca metin ve ikili hiçbir veri içermiyor. Web sunucusunda ayrı kaynaklar olarak görüntüleri, ses dosyalarını, Macromedia Flash uygulamaları, katıştırılmış Windows Media Player videolar ve benzeri, gibi her türlü ek ikili verinin mevcut. HTML bu dosyaları başvurular içeriyor, ancak gerçek dosyaların içeriğini içermez.

Örneğin, HTML biçiminde `<img>` öğesi ile bir resim başvurmak için kullanılan `src` görüntü dosyasına işaret eden özniteliği şu şekilde:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Bir tarayıcıda bu HTML aldığında, ardından tarayıcıda görüntüler görüntü dosyası ikili içeriğini almak için web sunucusu için başka bir isteği yapar. Aynı kavram herhangi bir ikili veri geçerlidir. 2. adımda Broşürü sayfa s HTML biçimlendirmesini bir parçası olarak tarayıcıya gönderilmedi. Bunun yerine, işlenen HTML köprüler, tıklatıldığında, PDF belgesini doğrudan istemek tarayıcı nedeni, sağlanan.

Görüntülemek veya kullanıcıların veritabanı içinde bulunduğu ikili veri indirmesine izin ver, biz verileri ayrı bir web sayfası oluşturmak gerekir. Uygulamamız için doğrudan veritabanı kategorisi s resimde depolanan orada s yalnızca bir ikili veri alan. Bu nedenle, bir sayfa ihtiyacımız, çağrıldığında, belirli bir kategorideki görüntü verilerini döndürür.

Yeni bir ASP.NET sayfası Ekle `BinaryData` adlı klasörü `DisplayCategoryPicture.aspx`. Bunun yapılması, Select ana sayfa onay kutusunun işaretini kaldırın. Bu sayfayı bekliyor bir `CategoryID` döndürür s kategoriye ikili veri ve sorgu dizesi değeri `Picture` sütun. Bu sayfa ikili veriler ve hiçbir şey döndürdüğünden, HTML bölümdeki tüm biçimlendirme gerekmez. Bu nedenle, sol alt köşesinde kaynağı sekmesinde tıklayın ve tüm dışında yönelik sayfa s biçimlendirme kaldırmak `<%@ Page %>` yönergesi. Diğer bir deyişle, `DisplayCategoryPicture.aspx` s bildirim temelli biçimlendirme tek bir satırı oluşması:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Görürseniz `MasterPageFile` özniteliğini `<%@ Page %>` yönergesi, kaldırın.

Sayfa s arka plan kodu sınıfında aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Bu kod, okuma tarafından başlatır `CategoryID` adlı bir değişkende sorgu dizesi değeri `categoryID`. Ardından, bir çağrı aracılığıyla resim veriler alınır `CategoriesBLL` s sınıfı `GetCategoryWithBinaryDataByCategoryID(categoryID)` yöntemi. Bu verileri kullanarak istemciye döndürülen `Response.BinaryWrite(data)` yöntemi, ancak bu çağrılmadan önce `Picture` sütun değeri s OLE üstbilgisi kaldırılmalıdır. Bu oluşturarak gerçekleştirilir bir `byte` adlı dizi `strippedImageData` tam olarak 78 tutun karakter nedir değerinden `Picture` sütun. [ `Array.Copy` Yöntemi](https://msdn.microsoft.com/en-us/library/z50k9bft.aspx) veri kopyalamak için kullanılan `category.Picture` 78 konumunda üzerinden başlama `strippedImageData`.

`Response.ContentType` Özellik belirtir [MIME türü](http://en.wikipedia.org/wiki/MIME) böylece tarayıcı oluşturmak bildiği döndürülen içeriği. Bu yana `Categories` s tablosu `Picture` column bir bit eşlem resim, bit eşlem MIME türü kullanılan burada (görüntü/bmp). MIME türü atlarsanız, resim dosyası s ikili verileri içeriğine göre türü çıkarımını çünkü çoğu tarayıcısı görüntünün doğru görüntülenmeye devam eder. Ancak, mümkün olduğunda s MIME dahil akıllıca yazın. Bkz: [Internet Atanmış Numaralar Yetkilisi s Web sitesi](http://www.iana.org/) tam listesi için [MIME medya türleri](http://www.iana.org/assignments/media-types/).

Oluşturulan bu sayfayla ziyaret ederek belirli kategori s resim görüntülenebilir `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Şekil 11 gösterir görüntülenebilir Meşrubat kategori s resmi `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Resim görüntülenir İçecekler kategorisini s](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Şekil 11**: İçecekler kategorisini s resim görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


Ziyaret eden IF `DisplayCategoryPicture.aspx?CategoryID=categoryID`, nesne türü için 'türü için 'System.Byte []' System.DBNull' Unable okuyan bir özel durum almak için bu neden olabilecek iki şey vardır. İlk olarak, `Categories` s tablosu `Picture` sütunu izin `NULL` değerleri. `DisplayCategoryPicture.aspx` Sayfasında, ancak olduğunu varsayar var olmayan bir`NULL` değeri mevcut. `Picture` Özelliği `CategoriesDataTable` varsa doğrudan erişilemez bir `NULL` değeri. İzin vermek istiyorsanız, `NULL` değerleri `Picture` sütun, d aşağıdaki iki koşul dahil etmek istediğiniz:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

Yukarıdaki kod bazı görüntü dosyası adlı olmadığını s varsayar `NoPictureAvailable.gif` içinde `Images` kategorilerin resim olmadan görüntülemek istediğiniz klasör.

Bu özel durum, aynı zamanda, kaynaklanabilir `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` s yöntemi `SELECT` deyimi geçici SQL deyimlerini kullanıyorsanız ve önceden TableAdapter s için sihirbazı yeniden çalıştırın gerçekleşebilir geri ana sorgu s sütun listesine geri çevirdiniz ana sorgu. Emin olmak için onay `GetCategoryWithBinaryDataByCategoryID` s yöntemi `SELECT` deyimi hala içeren `Picture` sütun.

> [!NOTE]
> Her zaman `DisplayCategoryPicture.aspx` olan ziyaret veritabanı erişilir ve belirtilen kategori s resim veriler döndürülür. Kullanıcı son görüntülemediği bu yana kategori s resim edilmemiş t değiştirdiyseniz, yine de harcanan çabayı budur. Neyse ki, HTTP için izin verir *koşullu alır*. Koşullu bir GET ile boyunca HTTP isteği yapan istemcinin gönderdiği bir [ `If-Modified-Since` HTTP üstbilgisi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) tarih ve saat istemci en son web sunucusunda bu kaynak alınan sağlar. Web sunucusu yanıt verebilir içeriği bu tarih için belirtilen bu yana değişmemişse bir [değişiklik durum kodu (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) ve geri istenen kaynak s içeriği gönderirken atlayabilirsiniz. Kısacası, istemcinin en son erişilen beri bu değişiklik yapılmamış yoksa bir kaynak için içerik göndermek zorunda kalmadan web sunucusunda bu teknik kurtarır.


Bu davranışı uygulamak için eklemenizi gerektirir ancak, bir `PictureLastModified` sütuna `Categories` ne zaman yakalamak için tablo `Picture` sütun yanı sıra denetlemek için kodu güncelleştirilen son `If-Modified-Since` üstbilgi. Daha fazla bilgi için `If-Modified-Since` üstbilgi ve koşullu GET iş akışı bkz [HTTP koşullu GET RSS saldırganlar için](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) ve [A derin bakabilir HTTP isteklerinde gerçekleştirme ASP.NET sayfası](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>4. adım: bir GridView kategori resimleri görüntüleme

Belirli kategori s resim görüntülemek için bir web sayfası sahibiz, kullanarak görüntülemek [Görüntü Web denetimi](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) veya bir HTML `<img>` işaret öğesi `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Görüntüleri URL'si veritabanı veri tarafından belirlenir GridView ya da ImageField kullanarak DetailsView görüntülenebilir. ImageField içeren `DataImageUrlField` ve `DataImageUrlFormatString` HyperLinkField s gibi iş özellikleri `DataNavigateUrlFields` ve `DataNavigateUrlFormatString` özellikleri.

S büyütmek izin `Categories` GridView içinde `DisplayOrDownloadData.aspx` her kategori s resmi göstermek için bir ImageField ekleyerek düzenleyin. Yalnızca ImageField ekleyin ve ayarlayın, `DataImageUrlField` ve `DataImageUrlFormatString` özelliklerine `CategoryID` ve `DisplayCategoryPicture.aspx?CategoryID={0}`sırasıyla. Bu işleyen GridView sütununu oluşturacak bir `<img>` öğesi, `src` özniteliği başvuruları `DisplayCategoryPicture.aspx?CategoryID={0}`, {0} GridView satırı s burada değiştirilir `CategoryID` değeri.


![GridView bir ImageField ekleyin](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Şekil 12**: bir ImageField GridView ekleme


ImageField eklendikten sonra GridView s Tanımlayıcı Sözdizimi soothe gibi görünmelidir aşağıdaki:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Her bir kaydı kategorisi için bir resim şimdi nasıl içerdiğini unutmayın.


[![Her satır için kategori s resim görüntülenir](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Şekil 13**: her satır için görüntülenen resmi kategori s ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Özet

Bu öğreticide ikili verileri sunmak nasıl incelendi. Verilerin gösterilme veri türüne bağlıdır. PDF Broşürü dosyaları için biz kullanıcı görünümü Broşürü sunulan, bağlantı tıklatıldığında, kullanıcının doğrudan PDF dosyası aldı. Kategori s resmi için ilk almak ve ikili veri veritabanından bir sayfa oluşturduğunuz ve bu sayfa bir GridView her kategori s resim görüntülerken kullanılacak.

Şimdi görüyoruz ve arama biz ekleme, güncelleştirme ve silme ile ikili veriler veritabanında gerçekleştirme incelemek için hazır re ikili verileri görüntülemek nasıl. Sonraki öğreticide yüklenen dosya karşılık gelen veritabanı kaydıyla ilişkilendirmek nasıl inceleyeceğiz. Bundan sonra öğreticide varolan ikili verileri güncelleştirmek nasıl yanı sıra, ilişkili kaydı kaldırıldığında ikili verileri silmek nasıl göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Teresa Murphy ve Dave Gardner yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](uploading-files-cs.md)
[sonraki](including-a-file-upload-option-when-adding-a-new-record-cs.md)
