---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Ek DataTable sütunları (VB) ekleme | Microsoft Docs
author: rick-anderson
description: Türü belirtilmiş veri kümesi oluşturmak için TableAdapter Sihirbazı'nı kullanarak, karşılık gelen DataTable ana veritabanında sorgu tarafından döndürülen sütunları içerir. Ancak orada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 84cb5f267a79fe7787ea23f1dc924e10e4d6fe13
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402791"
---
<a name="adding-additional-datatable-columns-vb"></a>Ek DataTable sütunları (VB) ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) veya [PDF olarak indirin](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Türü belirtilmiş veri kümesi oluşturmak için TableAdapter Sihirbazı'nı kullanarak, karşılık gelen DataTable ana veritabanında sorgu tarafından döndürülen sütunları içerir. Ancak DataTable ek sütunlar eklemek için gereken zamanı durumlar vardır. Bu öğreticide ek DataTable sütunları ihtiyacımız olduğunda saklı yordamlar neden önerilen öğrenin.


## <a name="introduction"></a>Giriş

Bir TableAdapter türü belirtilmiş veri kümesi eklerken, karşılık gelen DataTable s şema TableAdapter s ana sorgu tarafından belirlenir. Örneğin, ana sorguda veri alanlarını döndürürse *A*, *B*, ve *C*, DataTable adlı üç karşılık gelen sütunlara sahip olur *A*, *B*, ve *C*. Kendi ana sorguya ek olarak, TableAdapter, belki de bazı parametresine bağlı olarak verilerin bir alt kümesini döndüren ek sorgular içerebilir. Örneğin, ek olarak `ProductsTableAdapter` tüm ürünlerle ilgili bilgileri döndürür, s ana sorguda da içerdiği gibi yöntemleri `GetProductsByCategoryID(categoryID)` ve `GetProductByProductID(productID)`, sağlanan bir parametresini temel alan belirli bir ürün bilgileri döndürür.

TableAdapter s ana sorguda yansıtacak DataTable s şemasına sahip modelin iyi TableAdapter s yöntemlerin tümü, aynı veya daha az veri alanlarını ana sorguda belirtilenlerden döndürmesi durumunda çalışır. Bir TableAdapter yöntemi ek veri alanlarını döndürmek gerekiyorsa, ardından biz DataTable s şema uygun şekilde genişletmeniz gerekir. İçinde [ana/ayrıntı bir Ayrıntılar DataList'i ile bir madde işaretli liste, ana kayıtları kullanarak](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) bir yönteme ekledik öğretici `CategoriesTableAdapter` döndürülen `CategoryID`, `CategoryName`, ve `Description` tanımlanan veri alanları Ana sorguda artı `NumberOfProducts`, her kategori ile ilişkili ürün sayısı bildirilen bir ek veri alanı. El ile yeni bir sütun ekledik `CategoriesDataTable` yakalamak üzere `NumberOfProducts` veri alanı değeri bu yeni yöntemi.

Bölümünde açıklandığı gibi [yüklenen dosyalar](../working-with-binary-files/uploading-files-vb.md) geçici SQL deyimlerini ve veri alanları, ana sorgu tam olarak eşleşmiyor yöntemleri TableAdapter ile eğitmen, harika bakım gerçekleştirilen. TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın, böylece kendi veri alan listesi ana sorguda eşleşir, tüm TableAdapter s yöntemleri güncelleştirir. Sonuç olarak, özelleştirilmiş sütun listesi ile herhangi bir yöntem ana sorgu s sütun listesine dönmek ve beklenen verileri döndürmek değil. Bu sorun, saklı yordamları kullanarak ortaya çıkan değil.

Bu öğreticide size ek sütunlar eklemek için bir DataTable s şemayı genişletmek konuları ele alınacaktır. Kırılganlığının da artacağını geçici SQL deyimlerini kullanarak TableAdapter nedeniyle, bu öğreticide saklı yordamlarını kullanırız. Başvurmak [oluşturma yeni saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) ve [kullanarak mevcut saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) öğreticileri hakkında daha fazla bilgi için saklı yordamları kullanmak için bir TableAdapter yapılandırma.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>1. adım: Ekleme bir`PriceQuartile`sütun`ProductsDataTable`

İçinde *oluşturma yeni saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters* adlı bir türü belirtilmiş DataSet oluşturduğumuz öğretici `NorthwindWithSprocs`. Bu veri kümesi şu anda iki DataTable içerir: `ProductsDataTable` ve `EmployeesDataTable`. `ProductsTableAdapter` Aşağıdaki üç yöntemi vardır:

- `GetProducts` -tüm kayıtları döndüren ana sorguda `Products` tablo
- `GetProductsByCategoryID(categoryID)` -Belirtilen tüm ürünleriyle döndürür *CategoryID*.
- `GetProductByProductID(productID)` -belirli bir ürün belirli döndürür *ProductID*.

Tüm ana sorgu ve iki ek yöntem aynı veri alanları, yani tüm sütunların kümesini iade `Products` tablo. Bağlantılı hiçbir alt sorgular vardır veya `JOIN` ilgili verileri çekerek s `Categories` veya `Suppliers` tablolar. Bu nedenle, `ProductsDataTable` her alan için karşılık gelen bir sütunda `Products` tablo.

Bu öğreticide, let s eklemek için bir yöntem `ProductsTableAdapter` adlı `GetProductsWithPriceQuartile` tüm ürünleri döndürür. Standart ürün veri alanlara ek olarak `GetProductsWithPriceQuartile` da içerecek bir `PriceQuartile` veri alanı, hangi dörtte altında düştüğünde ürün s fiyatı gösterir. Örneğin, bu ürünlerin, fiyatlarıdır en pahalı %25 olur bir `PriceQuartile` olanlar, Fiyatlar % alt 25 kalan değeri 4 olsa 1 değeri. Bu bilgiler döndürmek için saklı yordam oluşturma hakkında endişe önce ancak ilk güncelleştirmek ihtiyacımız `ProductsDataTable` tutmak için bir sütun içerecek şekilde `PriceQuartile` sonuçları zaman `GetProductsWithPriceQuartile` yöntemi kullanılır.

Açık `NorthwindWithSprocs` veri kümesi üzerinde sağ tıklayın ve `ProductsDataTable`. Bağlam menüsünden Ekle'yi seçin ve ardından sütun seçin.


[![Yeni bir sütun için ProductsDataTable Ekle](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Şekil 1**: yeni bir sütun ekleyin `ProductsDataTable` ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image3.png))


Bu tür Column1 adlı DataTable tablosuna yeni bir sütun ekler `System.String`. Bu sütun s adı PriceQuartile ve kendi tür güncelleştirmek ihtiyacımız `System.Int32` 1 ile 4 arasında bir sayı tutmak için kullanılacağından. Yeni eklenen sütun seçtiğinizde `ProductsDataTable` ve Özellikler penceresinde ayarlayın `Name` PriceQuartile özelliğini ve `DataType` özelliğini `System.Int32`.


[![Yeni sütun adı ve veri türü özellikleri ayarlayın](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Şekil 2**: yeni bir sütun s ayarlamak `Name` ve `DataType` özellikleri ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image6.png))


Şekil 2 gösterildiği gibi olup sütundaki değerleri sütun otomatik artış sütunu ise benzersiz olması gerektiği gibi ayarlanabilir ek özellikleri vardır, olsun veya olmasın veritabanı `NULL` değerleri izin verilir ve benzeri. Bu değerleri değerlerinde bırakın.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>2. adım: Oluşturma`GetProductsWithPriceQuartile`yöntemi

Şimdi `ProductsDataTable` içerecek biçimde güncelleştirildi `PriceQuartile` sütun duyuyoruz oluşturmaya hazır `GetProductsWithPriceQuartile` yöntemi. Tableadapter'a sağ tıklayıp bağlam menüsünden Sorgu Ekle seçerek başlatın. Bu ilk bize biz geçici SQL deyimleri ya da yeni veya mevcut bir saklı yordam kullanmak isteyip istemediğinizi dair ister TableAdapter sorgu Yapılandırma Sihirbazı getirir. T ki henüz fiyat dörtte verileri döndüren bir saklı yordamı sahip olduğundan, bizim için bu saklı yordam oluşturmak TableAdapter izin s olanak tanır. Yeni saklı yordam oluştur seçeneğini belirleyin ve İleri'ye tıklayın.


[![Söyleyin bizim için saklı yordam oluşturmak için TableAdapter Sihirbazı](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Şekil 3**: saklı yordam için bize oluşturmak için TableAdapter Sihirbazı'nı isteyin ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image9.png))


Şekil 4'te gösterilen sonraki ekranda, sihirbaz bize ne tür bir sorgu eklemek için sorar. Bu yana `GetProductsWithPriceQuartile` yöntemi tüm sütunları ve kayıtları döndürür `Products` tablo, satırları seçeneği ve İleri'ye döndüren seçin.


[![Bir SELECT deyimi birden fazla satırları döndürür Sorgumuzu olacaktır](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Şekil 4**: Mız sorgu olacak bir `SELECT` deyimi birden fazla satırları döndürür ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image12.png))


Sonraki biz istenir `SELECT` sorgu. Sihirbazı'na aşağıdaki sorguyu girin:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Yukarıdaki sorguda, SQL Server 2005 s yeni kullanan [ `NTILE` işlevi](https://msdn.microsoft.com/library/ms175126.aspx) sonuçları, burada grupları tarafından belirlenir dört gruplara bölmek için `UnitPrice` değerleri azalan düzende sıralanır.

Ne yazık ki, Sorgu Oluşturucu nasıl ayrıştıracağını bilmez `OVER` anahtar sözcüğü ve yukarıdaki sorguda ayrıştırılırken bir hata görüntülenir. Bu nedenle, yukarıdaki sorguda doğrudan metin kutusunda sihirbazın Sorgu Oluşturucusu kullanmadan girin.

> [!NOTE]
> Diğer sıralama işlevlerini NTILE ve SQL Server 2005 s hakkında daha fazla bilgi için bkz: [Microsoft SQL Server 2005 ile sıralanmış sonuçları döndüren](http://www.4guysfromrolla.com/webtech/010406-1.shtml) ve [sıralaması işlevler bölümü](https://msdn.microsoft.com/library/ms189798.aspx) gelen [SQL Server 2005 Çevrimiçi Kitaplar](https://msdn.microsoft.com/library/ms189798.aspx).


Girdikten sonra `SELECT` sorgu ve İleri'ye tıklama, sihirbaz sorar bize onu oluşturacaktır saklı yordamı için bir ad sağlayın. Yeni saklı yordam adı `Products_SelectWithPriceQuartile` ve İleri'ye tıklayın.


[![Saklı yordam Products_SelectWithPriceQuartile adı](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Şekil 5**: saklı yordam adı `Products_SelectWithPriceQuartile` ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image15.png))


Son olarak, biz TableAdapter metotları adı istenir. Bir DataTable hem dolgu bırakın ve bir DataTable işaretli ve ad yöntemleri dönüş `FillWithPriceQuartile` ve `GetProductsWithPriceQuartile`.


[![Bitiş adı TableAdapter s yöntemleri ve tıklayın](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Şekil 6**: TableAdapter s yöntemleri ve Son'a tıklayın ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image18.png))


İle `SELECT` belirtilen sorgu ve saklı yordam ve TableAdapter yöntemleri adlı, Sihirbazı tamamlamak için Son'u tıklatın. Bu noktada bir veya iki sihirbazından olduğunu belirten bir uyarı alabilirsiniz `OVER` SQL yapıyı veya ifadesi desteklenmiyor. Bu uyarıların yoksayılabilir.

Sihirbazı tamamladıktan sonra TableAdapter içermelidir `FillWithPriceQuartile` ve `GetProductsWithPriceQuartile` yöntemleri ve veritabanı adında bir saklı yordamı içermelidir `Products_SelectWithPriceQuartile`. TableAdapter aslında bu yeni bir yöntem içermiyor ve saklı yordam doğru veritabanına eklendiğini doğrulamak için bir dakikanızı ayırın. Saklı yordam deneyin saklı yordamlar klasörü sağ tıklatın ve yenileme seçerek görmüyorsanız veritabanı denetlenirken.


![Yeni bir yöntem TableAdapter bağdaştırıcısına eklenen olduğunu doğrulayın](adding-additional-datatable-columns-vb/_static/image19.png)

**Şekil 7**: yeni bir yöntem TableAdapter bağdaştırıcısına eklenen olduğunu doğrulayın


[![Veritabanı Products_SelectWithPriceQuartile içerdiğinden emin olun depolanan yordamı](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Şekil 8**: veritabanı içeren emin `Products_SelectWithPriceQuartile` saklı yordam ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Geçici SQL deyimleri yerine saklı yordamlar kullanmanın avantajları TableAdapter yapılandırma sihirbazını yeniden çalıştırarak saklı yordamlar sütunu listeler değiştirmeyeceğini biridir. Bu, Tableadapter'a sağ tıklayıp bağlam-sihirbazını başlatmak için menüsünden yapılandırma seçeneğini seçmenize ve sonra tamamlamak için Son'u tıklayıp doğrulayın. Ardından, Görünüm ve veritabanı için Git `Products_SelectWithPriceQuartile` saklı yordamı. Not, sütun listesi değiştirilmedi. Kullandığımız geçici SQL deyimleri, TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırmak bu sorgu s sütun listesi'NTILE deyimi tarafından kullanılan bir sorgu böylece kaldırma ana sorgu sütunu listesi ile eşleşecek şekilde Döndürülmüş `GetProductsWithPriceQuartile` yöntemi.


Veri erişim katmanı s `GetProductsWithPriceQuartile` yöntemi çağrıldığında, TableAdapter'ı yürütür `Products_SelectWithPriceQuartile` bir satır ekler ve saklı yordamı `ProductsDataTable` her kaydı döndürdü. Saklı yordam tarafından döndürülen veri alanlarını eşlendiğine `ProductsDataTable` s sütunları. Olduğundan bir `PriceQuartile` veri alan bir saklı yordamdan döndürülen değeri atanır `ProductsDataTable` s `PriceQuartile` sütun.

Sorgularının döndürmeyen bu TableAdapter yöntemleri için bir `PriceQuartile` veri alanı `PriceQuartile` sütunu s değerdir tarafından belirtilen değeri kendi `DefaultValue` özelliği. Şekil 2 gösterildiği gibi bu değer kümesine `DBNull`, varsayılan. Farklı bir varsayılan değer tercih ediyorsanız, ayarlamanız yeterlidir `DefaultValue` özelliği uygun şekilde. Yalnızca, emin `DefaultValue` değeri geçerli s sütununda verilen `DataType` (yani, `System.Int32` için `PriceQuartile` sütun).

Bu noktada size ek bir sütun DataTable tablosuna eklemek için gerekli adımları gerçekleştirdiniz. Bu ek sütun beklendiği gibi çalıştığını doğrulamak için her ürün adı, fiyatı ve fiyat dörtte görüntüleyen bir ASP.NET sayfası oluşturma s olanak tanır. Bunu yapmadan önce ilk iş mantığı katmanı DAL s çağıran bir yöntem içerecek şekilde güncelleştirmek ihtiyacımız `GetProductsWithPriceQuartile` yöntemi. Biz BLL ardından, adım 3'te güncelleştirin ve sonra adım 4'te ASP.NET sayfası oluşturun.

## <a name="step-3-augmenting-the-business-logic-layer"></a>3. adım: iş mantığı katmanı deneyimlerinizi

Yeni kullandığımız önce `GetProductsWithPriceQuartile` yöntemi sunu katmanı, biz öncelikle karşılık gelen bir yöntemi için BLL eklemeniz gerekir. Açık `ProductsBLLWithSprocs` sınıf dosyası ve aşağıdaki kodu ekleyin:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Gibi diğer veri alma yöntemlerine `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` yöntemi yalnızca bir DAL çağırır s karşılık gelen `GetProductsWithPriceQuartile` yöntemi ve sonuçları döndürür.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>4. adım: bir ASP.NET Web sayfasında fiyat dörtte bilgilerini görüntüleme

BLL eklenmesiyle tamamlamak biz re her ürün için fiyat dörtte gösteren bir ASP.NET sayfası oluşturmak için hazır. Açık `AddingColumns.aspx` sayfasını `AdvancedDAL` klasörü ve ayar Tasarımcısı araç kutusundan sürükleyip GridView kendi `ID` özelliğini `Products`. İsteğe bağlı olarak GridView s akıllı etiketten adlı yeni bir ObjectDataSource bağlama `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLLWithSprocs` s sınıfı `GetProductsWithPriceQuartile` yöntemi. Bu salt okunur bir kılavuz olacağından, güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image25.png))


[![Ürün bilgisi almak GetProductsWithPriceQuartile yöntemi](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Şekil 10**: ürün bilgilerini almak `GetProductsWithPriceQuartile` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image28.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak BoundField veya CheckBoxField GridView'a her yöntem tarafından döndürülen veri alanlarını ekler. Bu veri alanları biridir `PriceQuartile`, eklediğimiz için sütun olduğu `ProductsDataTable` adım 1.

Kaldırma GridView s alanları düzenleme dışındaki tüm `ProductName`, `UnitPrice`, ve `PriceQuartile` BoundFields. Yapılandırma `UnitPrice` değerini bir para birimi olarak Biçimlendir ve BoundField `UnitPrice` ve `PriceQuartile` BoundFields sağ ve orta-hizalanmış, sırasıyla. Son olarak, kalan BoundFields güncelleştirme `HeaderText` ürün, fiyat ve fiyat dörtte özellikleri sırasıyla. Ayrıca, GridView s akıllı etiket sıralama etkinleştir onay denetleyin.

Bu değişikliklerden sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Şekil 11, bir tarayıcıdan ziyaret edildiğinde bu sayfada görüntülenir. Başlangıçta, ürünleri göre fiyatı her ürünle birlikte uygun bir atanan azalan düzende sıralanır, Not `PriceQuartile` değeri. Elbette bu verileri diğer ölçütlere göre yine de ürün s sıralamasına göre fiyat yansıtan fiyat dörtte sütun değeri ile sıralanabilir (bkz. Şekil 12).


[![Ürünleri fiyatları göre sıralanır.](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Şekil 11**: ürünleri fiyatları göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image31.png))


[![Ürün adlarına göre sıralanır.](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Şekil 12**: ürün adlarına göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Böylece göre ürün satırları renkli birkaç kod satırıyla, biz GridView büyütmek, `PriceQuartile` değeri. Biz bu ürünlerin içinde ilk dörtte bir açık yeşil, ikinci dörtte açık bir sarı penceresindekilerle renk ve VS. Bu işlevsellik eklemek için birkaç dakikanızı geçmenizi öneriyoruz. GridView biçimlendirme şirket bilgilerinizi tazelemeniz gerekiyorsa başvurun [özel biçimlendirme sırasında verileri](../custom-formatting/custom-formatting-based-upon-data-vb.md) öğretici.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternatif bir yaklaşım - başka bir TableAdapter oluşturma

Bu öğreticide, ana sorgudan il dışındaki veri alanları döndüren bir TableAdapter için yöntem ekleme yaparken gördüğümüz gibi DataTable öğesine karşılık gelen sütunlara ekleyebiliriz. Bu tür bir yaklaşım, ancak yalnızca az sayıda farklı veri alanları döndüren yöntemler TableAdapter içinde varsa ve bu alternatif veri alanları ana sorgudan çok farklı değil ise iyi çalışır.

DataTable tablosuna sütun eklemek yerine, bunun yerine başka bir TableAdapter farklı veri alanları döndüren ilk TableAdapter yöntemleri içeren veri kümesine ekleyebilirsiniz. Bu öğretici için eklemek yerine `PriceQuartile` sütuna `ProductsDataTable` (Bu yalnızca kullanıldığı tarafından `GetProductsWithPriceQuartile` yöntemi), ek bir TableAdapter adlı veri kümesini ekledik `ProductsWithPriceQuartileTableAdapter` kullanılan `Products_SelectWithPriceQuartile` depolanan yordam, ana sorgu olarak. Fiyat dörtte ile ürün bilgilerini almak için gereken ASP.NET sayfaları kullandığınız `ProductsWithPriceQuartileTableAdapter`, belirtmiyor o kullanmaya devam edebilir ancak `ProductsTableAdapter`.

Yeni bir TableAdapter'ı ekleyerek, DataTables untarnished kalır ve kendilerine ait sütunların TableAdapter s yöntemleriyle tarafından döndürülen veri alanlarını tam olarak yansıtır. Ancak, ek TableAdapter'ları yinelenen görevleri ve işlevselliği ortaya çıkarabilir. Örneğin, bu, ASP.NET sayfaları görüntülenen `PriceQuartile` sütun da INSERT, sağlama güncelleştir ve Sil desteği, `ProductsWithPriceQuartileTableAdapter` olması gerekir, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikler düzgün yapılandırılmış. Bu özellikler yansıtmak sırada `ProductsTableAdapter` s, bu yapılandırma, fazladan bir adım tanıtır. Ayrıca, iki şekilde güncelleştirmek için silin veya aracılığıyla veritabanına - bir ürün eklemek artık var. `ProductsTableAdapter` ve `ProductsWithPriceQuartileTableAdapter` sınıfları.

Bu öğretici için indirme içeren bir `ProductsWithPriceQuartileTableAdapter` sınıfını `NorthwindWithSprocs` veri kümesi bu alternatif bir yaklaşım gösterilmektedir.

## <a name="summary"></a>Özet

Çoğu senaryoda, bir TableAdapter yöntemleri tümünün aynı alan veri kümesini döndürür, ancak bazı durumlarda belirli bir yöntem veya iki ek bir alan döndürmek için ne zaman gerekebilir. Örneğin, [ana/ayrıntı bir Ayrıntılar DataList'i ile bir madde işaretli liste, ana kayıtları kullanarak](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) bir yönteme ekledik öğretici `CategoriesTableAdapter` döndürülen ana sorgu s veri alanlara ek olarak, bir `NumberOfProducts` , alan Her kategori ile ilişkili ürün sayısı bildirdi. Bu öğreticide, yöntem ekleme sırasında olan incelemiştik `ProductsTableAdapter` döndürülen bir `PriceQuartile` ana sorgu s veri alanlara ek olarak alan. Ek veri yakalamak için karşılık gelen sütunlara DataTable öğesine ihtiyacımız alanları TableAdapter s yöntemlerle döndürdü.

El ile DataTable tablosuna sütun ekleme planlıyorsanız, TableAdapter saklı yordamları kullanmanız önerilir. TableAdapter geçici SQL deyimleri kullanırsa, istediğiniz zaman ana sorgu tarafından döndürülen veri alanlarını veri alan listeleri dönmek yöntemlerin tümü, TableAdapter Yapılandırma Sihirbazı'nı çalıştırılır. Bu sorun, saklı yordamlar için önerilir ve Bu öğreticide kullanılan neden olduğu genişletilmez.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Randy Etikan, Jacky Goor Bernadette Leigh ve Hilton Giesenow yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](updating-the-tableadapter-to-use-joins-vb.md)
> [İleri](working-with-computed-columns-vb.md)
