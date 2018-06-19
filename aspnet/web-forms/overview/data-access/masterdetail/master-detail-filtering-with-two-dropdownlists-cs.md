---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Ana/ayrıntı iki DropDownLists ile (C#) filtreleme | Microsoft Docs
author: rick-anderson
description: Bu öğretici, istenen üst ve iki üst recor seçmek için iki DropDownList denetimlerini kullanarak üçüncü bir katman eklemek için ana/ayrıntı ilişkisi genişletir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887258"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Ana/ayrıntı filtreleme iki DropDownLists ile (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) veya [PDF indirin](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Bu öğretici, istenen üst ve iki üst kayıtları seçmek için iki DropDownList denetimlerini kullanarak üçüncü bir katman eklemek için ana/ayrıntı ilişkisi genişletir.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](master-detail-filtering-with-a-dropdownlist-cs.md) biz kategorileri ve seçili kategorisine ait bu ürünleri gösteren GridView doldurulmuş tek bir DropDownList kullanılarak bir basit ana/ayrıntı raporu görüntülemek nasıl inceledi. Bu rapor düzeni iyi bir-çok ilişkisi vardır ve birden çok-çok ilişkileri içeren senaryoları için çalışması için kolayca genişletilebilir kayıtları görüntülenirken çalışır. Örneğin, bir sipariş girişi sistemi müşteriler, siparişler ve sipariş satır öğelerini karşılık gelen tabloları gerekir. Belirli bir müşteri birden fazla öğeden oluşan her düzende birden çok sipariş olabilir. Bu tür veriler, kullanıcıya iki DropDownLists ve GridView birlikte sunulabilir. İlk DropDownList ikinci veritabanıyla her müşteri için bir liste öğesi olurdu birinin içeriği Seçilen müşteri tarafından verilen siparişler bırakılıyor. GridView seçili sipariş satırı öğeleri listeler.

Northwind veritabanı dahil ederken kurallı müşteri / / sipariş ayrıntılarını bilgileri kendi `Customers`, `Orders`, ve `Order Details` tabloları, bu tablolar bizim mimarisinde yakalanan değil. Öte yandan, biz yine iki bağımlı DropDownLists kullanarak göstermeye. İlk DropDownList kategorileri ve ikinci seçili kategorisine ait ürünleri listeler. Bir DetailsView sonra seçili ürün ayrıntılarını listeler.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>1. adım: Oluşturma ve kategorilere DropDownList doldurma

İlk Amacımız kategorilerini listeler DropDownList eklemektir. Bu adımları ayrıntılı olarak önceki öğreticide incelenmesi, ancak bütünlük açısından buraya özetlenmiştir.

Açık `MasterDetailsDetails.aspx` sayfasındaki `Filtering` klasörünü kümesi sayfasına bir DropDownList Ekle kendi `ID` özelliğine `Categories`ve kendi akıllı etiket Configure Data Source bağlantıya tıklayın. Yeni bir veri kaynağı eklemek için veri kaynağı Yapılandırma Sihirbazı ' seçin.


[![Yeni bir veri kaynağı için DropDownList ekleyin](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Şekil 1**: yeni bir veri kaynağı için DropDownList ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


Doğal olarak, yeni veri kaynağı bir ObjectDataSource olmalıdır. Bu yeni ObjectDataSource ad `CategoriesDataSource` ve çağırma `CategoriesBLL` nesnesinin `GetCategories()` yöntemi.


[![CategoriesBLL sınıfını kullanmak seçin](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Şekil 2**: kullanımına seçin `CategoriesBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![ObjectDataSource GetCategories() yöntemi kullanmak üzere yapılandırma](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Şekil 3**: ObjectDataSource kullanılacak yapılandırma `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


ObjectDataSource yapılandırdıktan sonra hala hangi veri kaynağı alanı görüntüleneceğini belirlemek ihtiyacımız `Categories` DropDownList ve hangisinin liste öğesi için değer olarak yapılandırılmalıdır. Ayarlama `CategoryName` görüntü olarak alan ve `CategoryID` her liste öğesi için değer olarak.


[![CategoryName alan ve kullanım adlı kullanıcı, Categoryıd'si DropDownList görünen değere sahip](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Şekil 4**: DropDownList görüntü sahip `CategoryName` alan ve kullanım `CategoryID` değeri olarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


Bu noktada bir DropDownList denetimi sahibiz (`Categories`) kayıtlarla doldurulmuş `Categories` tablo. Kullanıcı yeni bir kategori DropDownList seçtiğinde 2. adımda oluşturmak için yapacağız DropDownList ürün yenilemek için gerçekleşmesi için bir geri gönderme istiyoruz. Bu nedenle, etkinleştirme AutoPostBack seçeneğinden denetleyin `categories` DropDownList'ın akıllı etiket.


[![İçin kategoriler DropDownList AutoPostBack'i etkinleştir](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Şekil 5**: AutoPostBack etkinleştirmek için `Categories` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>2. adım: bir ikinci DropDownList seçili kategorinin ürünleri görüntüleme

İle `Categories` DropDownList tamamlandı, seçilen kategoriye ait ürünlerin DropDownList görüntülemek için sonraki adım. Bunu başarmak için başka bir DropDownList adlı sayfasına ekleme `ProductsByCategory`. İle `Categories` DropDownList, oluşturmak için yeni bir ObjectDataSource `ProductsByCategory` adlı DropDownList `ProductsByCategoryDataSource`.


[![Yeni bir veri kaynağı için ProductsByCategory DropDownList ekleyin](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Şekil 6**: için yeni bir veri kaynağı Ekle `ProductsByCategory` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![ProductsByCategoryDataSource adlı yeni bir ObjectDataSource oluşturma](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Şekil 7**: yeni ObjectDataSource adlandırılmış oluşturma `ProductsByCategoryDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Bu yana `ProductsByCategory` seçili kategorisine ait yalnızca bu ürünleri görüntülenecek DropDownList ihtiyaçları olan çağırma ObjectDataSource `GetProductsByCategoryID(categoryID)` yönteminden `ProductsBLL` nesnesi.


[![ProductsBLL sınıfını kullanmak seçin](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Şekil 8**: kullanımına seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![ObjectDataSource GetProductsByCategoryID(categoryID) yöntemi kullanmak üzere yapılandırma](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


Değerini belirtmek ihtiyacımız sihirbazının son adımı *`categoryID`* parametresi. Bu parametre, seçili öğesini atamak `Categories` DropDownList.


[![Adlı kullanıcı, Categoryıd'si parametre değeri kategorileri DropDownList isteme](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Şekil 10**: çekme *`categoryID`* parametre değerinin `Categories` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Yapılandırılmış ObjectDataSource ile kalan tek şey hangi veri kaynağı alanlarını değeri DropDownList'ın öğeleri ve görüntü için kullanılan belirtmek için. Görüntü `ProductName` kullanın ve alan `ProductID` alan değeri olarak.


[![DropDownList'ın ListItems metin ve değer özellikleri için kullanılan veri kaynağı alanlarını belirtin](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Şekil 11**: veri kaynağı alanları kullanılan DropDownList için 's belirtin `ListItem` s' `Text` ve `Value` özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


ObjectDataSource ile ve `ProductsByCategory` DropDownList sayfamızı yapılandırılmış iki DropDownLists görüntülenir: ikinci seçili kategorisine ait bu ürünleri listeler sırada ilk tüm kategorilerini listeler. Kullanıcı ilk DropDownList yeni bir kategori seçtiğinde geri gönderimin olun ve ikinci DropDownList, yeni seçili kategorisine ait bu ürünleri gösteren DataSet'e. Şekil 12 ve 13 Göster `MasterDetailsDetails.aspx` bir tarayıcıdan görüntülendiğinde eylem.


[![İlk sayfasını ziyaret ettiğinizde, İçecekler kategorisini seçilir](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Şekil 12**: ilk sayfasını ziyaret ettiğinizde, İçecekler kategorisini seçilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Farklı bir kategori seçme yeni kategori ürünleri görüntüler](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Şekil 13**: farklı bir kategori görüntüler yeni kategori ürünleri seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Şu anda `productsByCategory` değiştirildiğinde DropDownList mu *değil* geri gönderimin neden. Ancak, seçilen ürün ayrıntıları (adım 3) görüntülemek için bir DetailsView eklediğimiz sonra gerçekleşmesi için bir geri gönderme istiyoruz. Bu nedenle, gelen AutoPostBack etkinleştir onay kutusunu işaretleyin `productsByCategory` DropDownList'ın akıllı etiket.


[![DropDownList productsByCategory AutoPostBack özelliğini etkinleştir](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Şekil 14**: AutoPostBack özelliğini etkinleştirmek `productsByCategory` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>3. adım: Seçili ürün için ayrıntıları görüntülemek için bir DetailsView kullanma

Son adım, seçilen ürün ayrıntılarını bir DetailsView'da görüntülemektir. Bunu başarmak eklemek için bir DetailsView sayfasına ayarlayın, `ID` özelliğine `ProductDetails`ve yeni ObjectDataSource oluşturun. Kendi verisinden çıkarmak için bu ObjectDataSource yapılandırma `ProductsBLL` sınıfının `GetProductByProductID(productID)` seçili değeri kullanılarak yöntemi `ProductsByCategory` değerini DropDownList *`productID`* parametresi.


[![ProductsBLL sınıfını kullanmak seçin](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Şekil 15**: kullanımına seçin `ProductsBLL` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![ObjectDataSource GetProductByProductID(productID) yöntemi kullanmak üzere yapılandırma](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Şekil 16**: ObjectDataSource kullanılacak yapılandırma `GetProductByProductID(productID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Parametre değeri ProductID ProductsByCategory DropDownList isteme](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Şekil 17**: çekme *`productID`* parametre değerinin `ProductsByCategory` DropDownList ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Kullanılabilir alanlar DetailsView'da görüntülemeyi seçebilirsiniz. Kaldırmak için tercih `ProductID`, `SupplierID`, ve `CategoryID` alanları ve kaldırılmasında ve geri kalan alanları biçimlendirilmiş. Ayrıca, t DetailsView'un temizlenmiş `Height` ve `Width` özellikleri, belirtilen boyutu kısıtlı sahip olmak yerine verileri için en iyi görüntü gerekli genişliği genişletmek DetailsView izin verme. Tam biçimlendirme aşağıda yer almaktadır:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Denemek için bir dakikanızı ayırın `MasterDetailsDetails.aspx` sayfasını bir tarayıcıda. İlk bakışta her şeyin istendiği gibi çalıştığını, ancak zarif bir sorun olduğu görünebilir. Yeni bir kategori seçtiğinizde `ProductsByCategory` DropDownList bu ürünler için seçilen kategori içerecek şekilde güncelleştirilir ancak `ProductDetails` DetailsView devam önceki ürün bilgilerini göstermek. DetailsView seçili kategorisi için farklı bir ürün seçerken güncelleştirilir. Yeterince baştan sona test ederseniz yeni kategoriler sürekli olarak seçerseniz, ayrıca, bulabilirsiniz (Meşrubat gelen seçme gibi `Categories` DropDownList sonra Condiments, ardından Confections) her bir kategori seçimi neden`ProductDetails`Yenilenmesi DetailsView.

Bu sorunu concretize yardımcı olmak için belirli bir örneğe bakalım. İlk sayfasını ziyaret ettiğinizde İçecekler kategorisini seçilir ve ilgili ürünler yüklenen `ProductsByCategory` DropDownList. Chai seçili ürün ve ayrıntılarını görüntülenen `ProductDetails` Şekil 18'de gösterildiği gibi DetailsView.


[![Seçili ürünün ayrıntıları bir DetailsView'da görüntülenir](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Şekil 18**: Seçili ürünün ayrıntıları bir DetailsView'da görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Condiments için Meşrubat kategori seçimi değiştirirseniz, geri gönderimin oluştuğu ve `ProductsByCategory` DropDownList buna göre güncelleştirilir, ancak DetailsView ayrıntılarını hala ayrıntılarını görüntüler.


[![Daha önce seçilen ürünün ayrıntıları hala görüntülenir](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Şekil 19**: daha önce seçilen ürünün ayrıntılar verilmektedir görüntülenmeye ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Listeden yeni bir ürün çekme DetailsView beklendiği gibi yeniler. Ürün değiştirdikten sonra yeni bir kategori seçerseniz DetailsView tekrar yenileyin olmaz. Ancak, yeni bir ürün seçmek yerine yeni bir kategori seçtiyseniz, DetailsView yeniler. Dünyadaki burada neler olup bittiğini?

Sayfanın yaşam döngüsü bir zamanlama sorunu sorunudur. Her bir sayfa çeşitli adımları kendi işleme olarak yoluyla ilerler istendi. Aşağıdaki adımlardan birini ObjectDataSource varsa denetleyin denetimleri kendi `SelectParameters` değerleri değişti. Veri Web denetimi bu nedenle, bağlı değilse ObjectDataSource görünümünü yenilemek için ihtiyaç duyduğu bilir. Örneğin, yeni bir kategori seçili olduğunda, `ProductsByCategoryDataSource` ObjectDataSource algılar, parametre değerlerinin değiştirilmiştir ve `ProductsByCategory` DropDownList rebinds kendisini ürünler seçilen kategori için alma.

Bu durumda ortaya ObjectDataSources değiştirilen parametrelerini denetleyin sayfa yaşam döngüsü noktasında oluştuğunu sorunudur *önce* ilişkili verileri Web denetimleri yeniden bağlama. Bu nedenle, yeni bir kategori seçerken `ProductsByCategoryDataSource` ObjectDataSource kendi parametre değerinde bir değişiklik algılar. Tarafından kullanılan ObjectDataSource `ProductDetails` DetailsView, ancak değil unutmayın herhangi bir değişikliğe çünkü `ProductsByCategory` DropDownList gerekiyor henüz DataSet'e. Yaşam döngüsü devamındaki `ProductsByCategory` DropDownList rebinds ürünler yeni seçilen kategori için kapmasını kendi ObjectDataSource için. Sırada `ProductsByCategory` DropDownList'ın değeri değişti, `ProductDetails` DetailsView'un ObjectDataSource, parametre değeri denetimi zaten yapılmış; bu nedenle, DetailsView önceki sonuçlarını görüntüler. Bu etkileşimi Şekil 20'de belirtilmiştir.


[![Değişiklikleri tazelemek DetailsView'un ObjectDataSource denetledikten sonra ProductsByCategory DropDownList değerini değiştirir](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Şekil 20**: `ProductsByCategory` DropDownList değer değişiklikleri sonra `ProductDetails` değişiklikleri DetailsView'un ObjectDataSource denetler ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


İhtiyacımız açıkça yeniden bağlamak için bu sorunu gidermek için `ProductDetails` sonra DetailsView `ProductsByCategory` DropDownList bağlı. Biz çağırarak gerçekleştirebilirsiniz `ProductDetails` DetailsView'un `DataBind()` yöntemi zaman `ProductsByCategory` DropDownList'ın `DataBound` olay etkinleşir. Aşağıdaki olay işleyici kodu eklemek `MasterDetailsDetails.aspx` sayfanın arka plandaki kod sınıfı (başvurun "[program aracılığıyla ObjectDataSource ait parametre değerleri ayarlanıyor](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" olay işleyicisi ekleme hakkında tartışma için):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Bu açık çağrısına sonra `ProductDetails` DetailsView'un `DataBind()` yöntemi eklenen, öğreticiyi beklendiği gibi çalışır. Şekil 21 vurgular nasıl bu değişti, önceki sorun düzenleyiciyle.


[![Tazelemek DetailsView açıkça yenilenir zaman ProductsByCategory DropDownList's veriye bağlı olay ateşlenir](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Şekil 21**: `ProductDetails` DetailsView olduğunu açıkça yenilendiğinde `ProductsByCategory` DropDownList'ın `DataBound` olay ateşlenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Özet

Ana/ayrıntı raporları için bir ideal kullanıcı arabirimi öğesi DropDownList gören ana ve ayrıntı kayıtları arasında bir-çok ilişkisi söz konusu olduğunda. Önceki öğreticide seçilen kategoriye göre görüntülenen ürünleri filtrelemek için tek bir DropDownList kullanmayı gördünüz. Bu öğreticide biz ürünleri GridView DropDownList ile değiştirilir ve bir DetailsView seçili ürün ayrıntılarını görüntülemek için kullanılır. Bu öğreticide açıklanan kavramlar, müşteriler, siparişler ve öğeleri sıralama gibi birden çok-çok ilişkileri içeren veri modelleri için kolayca genişletilebilir. Genel olarak, her bir-çok ilişkilerde "bir" varlıklar için her zaman bir DropDownList ekleyebilirsiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Giesenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](master-detail-filtering-with-a-dropdownlist-cs.md)
> [sonraki](master-detail-filtering-across-two-pages-cs.md)
