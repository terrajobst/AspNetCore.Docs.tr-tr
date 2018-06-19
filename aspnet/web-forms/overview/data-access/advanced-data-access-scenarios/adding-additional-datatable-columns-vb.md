---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Ek DataTable sütunlar (VB) ekleme | Microsoft Docs
author: rick-anderson
description: Yazılan veri kümesi oluşturmak için TableAdapter Sihirbazı'nı kullanırken, karşılık gelen DataTable ana veritabanı sorgusunun döndürdüğü sütunları içerir. Ancak orada...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: b51089057ad1e14901cb09589534d6e575261c3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879055"
---
<a name="adding-additional-datatable-columns-vb"></a>Ek DataTable sütunlar (VB) ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) veya [PDF indirin](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Yazılan veri kümesi oluşturmak için TableAdapter Sihirbazı'nı kullanırken, karşılık gelen DataTable ana veritabanı sorgusunun döndürdüğü sütunları içerir. Ancak ek sütunlar eklenecek DataTable gerektiği zaman durumlar vardır. Bu öğreticide ek DataTable sütunlar ihtiyacımız zaman neden saklı yordamlar için önerilen öğrenin.


## <a name="introduction"></a>Giriş

Bir TableAdapter yazılmış bir veri kümesi eklerken, karşılık gelen DataTable s şema TableAdapter s ana sorgu tarafından belirlenir. Örneğin, ana sorgu veri alanları döndürürse *A*, *B*, ve *C*, DataTable adında üç karşılık gelen sütunlara sahip olur *A*, *B*, ve *C*. Kendi ana sorgu yanı sıra bir TableAdapter belki de, bazı parametresi temelinde verilerin bir alt kümesini döndüren ek sorgular içerebilir. Örneğin, ek olarak `ProductsTableAdapter` tüm ürünlerle ilgili bilgileri döndürür, s ana sorgu ayrıca içerdiği yöntemler gibi `GetProductsByCategoryID(categoryID)` ve `GetProductByProductID(productID)`, sağlanan bir parametresine bağlı olarak belirli bir ürün bilgi döndürür.

İyi tüm TableAdapter s yöntemleri aynı veya daha az veri alanları ana sorguya belirtilenlerden döndürürse TableAdapter s ana sorgu yansıtacak DataTable s şemasına sahip modelin çalışır. TableAdapter yöntemi ek veri alanlarını döndürmek gerekirse, ardından biz DataTable s şema uygun şekilde genişletmeniz gerekir. İçinde [ana/Ayrıntılar DataList ile bir madde işaretli listesi, ana kayıtları kullanarak ayrıntı](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) bir yönteme eklediğimiz öğretici `CategoriesTableAdapter` döndürülen `CategoryID`, `CategoryName`, ve `Description` tanımlanan veri alanları artı ana sorgu `NumberOfProducts`, her kategori ile ilişkili ürün sayısına bildirilen bir ek veri alanı. El ile yeni bir sütun eklediğimiz `CategoriesDataTable` yakalamak üzere `NumberOfProducts` veri alanı değeri bu yeni yönteminden.

' Da anlatıldığı gibi [yüklenen dosyalar](../working-with-binary-files/uploading-files-vb.md) geçici SQL deyimlerini kullanın ve veri alanları ana sorgu tam olarak eşleşmiyor yöntemleri TableAdapters eğitmen, çok dikkatli gerçekleştirilecek. TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın, böylece kendi veri alan listesi ana sorgu eşleşir, tüm TableAdapter s yöntemleri güncelleştirir. Sonuç olarak, özelleştirilmiş sütun listesi ile herhangi bir yöntem ana sorgu s sütun listesine dönmek ve beklenen verileri iade değil. Bu sorun, saklı yordamları kullanırken çıkabilecek değil.

Bu öğreticide biz ek sütunlar eklenecek DataTable s şemayı genişletmek ne arar. Kırılganlığının da artacağını geçici SQL deyimlerini kullanırken TableAdapter nedeniyle, bu öğreticide saklı yordamlar kullanacağız. Başvurmak [oluşturma yeni saklı yordamlar için yazılan veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) ve [kullanarak var olan saklı yordamlar için yazılan veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) öğreticileri hakkında daha fazla bilgi için saklı yordamlar kullanmak için bir TableAdapter yapılandırma.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>1. adım: Ekleme bir`PriceQuartile`sütunu`ProductsDataTable`

İçinde *oluşturma yeni saklı yordamlar için yazılan veri kümesi s TableAdapters* yazılan adlı bir veri oluşturduğumuz Öğreticisi `NorthwindWithSprocs`. Bu veri kümesi şu anda iki DataTables içeriyor: `ProductsDataTable` ve `EmployeesDataTable`. `ProductsTableAdapter` Aşağıdaki üç yöntemi vardır:

- `GetProducts` -tüm kayıtları döndürür ana sorgu `Products` tablosu
- `GetProductsByCategoryID(categoryID)` -Belirtilen tüm ürünleri verir *adlı kullanıcı, Categoryıd'si*.
- `GetProductByProductID(productID)` -Belirtilen belirli ürünle döndürür *ProductID*.

Tüm ana sorgu ve iki ek yöntemleri veri alanları, yani tüm sütunları aynı kümesini döndürür `Products` tablo. Hiçbir ilişkili alt sorgulara vardır veya `JOIN` ilgili verileri çekme s `Categories` veya `Suppliers` tabloları. Bu nedenle, `ProductsDataTable` her alan için karşılık gelen bir sütuna sahip `Products` tablo.

Bu öğretici için let s eklemek için bir yöntem `ProductsTableAdapter` adlı `GetProductsWithPriceQuartile` tüm ürünleri döndürür. Standart ürün veri alanlara ek `GetProductsWithPriceQuartile` de içerecek bir `PriceQuartile` altında hangi DÖRTTEBİRLİK ürün s fiyat döner gösteren veri alanı. Örneğin, bu ürünler, fiyatlarıdır en pahalı % 25'nde olacaktır bir `PriceQuartile` olanlar, Fiyatlar kalan alt %25 4'ün bir değer olsa 1 değeri. Bu bilgileri döndürmek için saklı yordam oluşturma hakkında endişelenmeniz önce ancak ilk güncelleştirmek ihtiyacımız `ProductsDataTable` tutmak için bir sütun eklemek için `PriceQuartile` sonuçları zaman `GetProductsWithPriceQuartile` yöntemi kullanılır.

Açık `NorthwindWithSprocs` veri kümesi ve sağ `ProductsDataTable`. Bağlam menüsünden Ekle'yi seçin ve ardından sütun seçin.


[![ProductsDataTable için yeni bir sütun ekleyin](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Şekil 1**: yeni bir sütun ekleyin `ProductsDataTable` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image3.png))


Bu tür Column1 adlı DataTable yeni bir sütun ekler `System.String`. Bu sütun s adı PriceQuartile ve onun tür güncelleştirmek ihtiyacımız `System.Int32` 1 ile 4 arasında bir sayı tutmak için kullanılacağından. Yeni eklenen sütununda seçin `ProductsDataTable` ve Özellikler penceresinden ayarlayın `Name` PriceQuartile özelliğine ve `DataType` özelliğine `System.Int32`.


[![Yeni sütun adı ve veri türü özellikleri ayarlayın](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Şekil 2**: yeni bir sütun s ayarlamak `Name` ve `DataType` özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image6.png))


Şekil 2'de görüldüğü gibi olup sütundaki değerlerin sütun otomatik artış sütunu ise benzersiz olmalıdır gibi ayarlanabilir ek özellikleri vardır, desteklemediğini veritabanı `NULL` değerleri izin verilir ve benzeri. Varsayılan değerlerine ayarlayın bu değerleri bırakın.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>2. adım: Oluşturma`GetProductsWithPriceQuartile`yöntemi

Şimdi `ProductsDataTable` içerecek şekilde güncelleştirilmiş `PriceQuartile` sütun, biz oluşturmak için hazır `GetProductsWithPriceQuartile` yöntemi. TableAdapter üzerinde sağ tıklayıp bağlam menüsünden Sorgu Ekle seçerek başlatın. Bu ilk biz geçici SQL deyimlerini ya da yeni veya varolan bir saklı yordamı kullanmak isteyip istemediğinizi konusunda bize ister TableAdapter sorgu Yapılandırma Sihirbazı açar. T güncelleştireceğinizi henüz fiyat DÖRTTEBİRLİK veri döndüren bir saklı yordam sahip olduğundan, bu saklı yordam bize oluşturmak TableAdapter izin s olanak tanır. Oluştur Yeni saklı yordam seçeneğini seçin ve İleri'yi tıklatın.


[![TableAdapter bize için saklı yordam oluşturmak için sihirbazın isteyin](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Şekil 3**: TableAdapter saklı yordam için bize oluşturmak için sihirbazın isteyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image9.png))


Şekil 4'te gösterilen sonraki ekranda sihirbaz bize ne tür eklemek için sorgu sorar. Bu yana `GetProductsWithPriceQuartile` yöntemi, tüm sütunları ve kayıtları döndürecektir `Products` tablo, satır seçeneği ve İleri'yi tıklatın döndüren seçin.


[![Bizim sorgu bir SELECT deyimi bu döndürür birden çok satır olacaktır](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Şekil 4**: Mız sorgu bir `SELECT` deyimi bu döndürür birden çok satır ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image12.png))


Sonraki biz istenir `SELECT` sorgu. Aşağıdaki Sorgu Sihirbazı'na girin:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Yukarıdaki sorguda SQL Server 2005 s yeni kullanan [ `NTILE` işlevi](https://msdn.microsoft.com/library/ms175126.aspx) burada gruplar tarafından belirlenir dört gruplar halinde sonuçları bölmek için `UnitPrice` azalan şekilde sıralanmış değerleri.

Ne yazık ki, Sorgu Oluşturucusu ayrıştırmayı bilmez `OVER` anahtar sözcüğü ve yukarıdaki sorguda ayrıştırılırken bir hata görüntülenir. Bu nedenle, yukarıdaki sorguda doğrudan Sihirbazı'nda textbox Sorgu Oluşturucusu kullanmadan girin.

> [!NOTE]
> Diğer derecelendirme işlevleri NTILE ve SQL Server 2005 s hakkında daha fazla bilgi için bkz: [Microsoft SQL Server 2005'te derece sonuçları döndüren](http://www.4guysfromrolla.com/webtech/010406-1.shtml) ve [sıralaması işlevler bölümüne](https://msdn.microsoft.com/library/ms189798.aspx) gelen [SQL Server 2005 Çevrimiçi Kitaplar](https://msdn.microsoft.com/library/ms189798.aspx).


Girdikten sonra `SELECT` sorgu ve İleri'yi tıklatmadan, sihirbaz sorar bize onu oluşturacaktır saklı yordam için bir ad sağlayın. Yeni bir saklı yordam adı `Products_SelectWithPriceQuartile` ve İleri'yi tıklatın.


[![Saklı yordam Products_SelectWithPriceQuartile adı](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Şekil 5**: saklı yordam adı `Products_SelectWithPriceQuartile` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image15.png))


Son olarak, biz TableAdapter yöntemleri adı istenir. Her iki dolgu DataTable bırakın ve bir teslim DataTable onay kutularını ve ad yöntemleri döndürür `FillWithPriceQuartile` ve `GetProductsWithPriceQuartile`.


[![Bitiş TableAdapter s adı yöntemleri ve tıklatın](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Şekil 6**: TableAdapter s yöntemleri ve Son'u tıklatın adı ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image18.png))


İle `SELECT` sorgu belirtilen ve saklı yordam ve TableAdapter yöntemleri adlı, Sihirbazı tamamlamak için Son'u tıklatın. Bu noktada bir uyarı ya da iki olduğunu belirten Sihirbazı'ndan alabilirsiniz `OVER` SQL yapısını veya deyimi desteklenmiyor. Bu uyarı yoksayılabilir.

Sihirbazı tamamladıktan sonra TableAdapter içermelidir `FillWithPriceQuartile` ve `GetProductsWithPriceQuartile` yöntemleri ve veritabanı adlı bir saklı yordam içermelidir `Products_SelectWithPriceQuartile`. TableAdapter bu yeni yöntemin gerçekten içermiyor ve saklı yordam veritabanına doğru eklendiğini doğrulamak için bir dakikanızı ayırın. Saklı yordam deneyin saklı yordamlar klasöre sağ tıklayıp yenileme seçme görmüyorsanız veritabanı denetlenirken.


![Yeni bir yöntem için TableAdapter eklendiğini doğrulayın](adding-additional-datatable-columns-vb/_static/image19.png)

**Şekil 7**: yeni bir yöntem için TableAdapter eklendiğini doğrulayın


[![Veritabanı Products_SelectWithPriceQuartile içerdiğinden emin olun saklı yordamı](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Şekil 8**: veritabanını içeren emin `Products_SelectWithPriceQuartile` saklı yordam ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Geçici SQL deyimleri yerine saklı yordamlar kullanmanın avantajları TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırmak saklı yordamlar sütunu listeler değiştirmez biridir. Bu, üzerinde TableAdapter sağ tıklayarak, İçerik-Sihirbazı'nı başlatmak için menüsünden yapılandırma seçeneğini seçmenize ve tamamlamak için Son'u tıklatarak doğrulayın. Ardından, veritabanı görünümü'a gidin ve `Products_SelectWithPriceQuartile` saklı yordamı. Sütun listesi değiştirilmedi unutmayın. Kullandığımızı geçici SQL deyimlerini, TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırmak bu sorgu s sütun listesi böylece NTILE deyimi tarafından kullanılan sorguyu kaldırma ana sorgu sütun listesiyle eşleşen Döndürülmüş `GetProductsWithPriceQuartile` yöntemi.


Veri erişim katmanı s `GetProductsWithPriceQuartile` yöntemi çağrıldığında, TableAdapter yürütür `Products_SelectWithPriceQuartile` saklı yordamı ve bir satır ekler `ProductsDataTable` her kayıt döndürülen. Saklı yordam tarafından döndürülen veri alanları eşlendiği `ProductsDataTable` s sütun. Olduğundan bir `PriceQuartile` verileri alan bir saklı yordamdan döndürülen değeri atandığı `ProductsDataTable` s `PriceQuartile` sütun.

Sorgularının döndürmeyen bu TableAdapter yöntemleri için bir `PriceQuartile` veri alanı, `PriceQuartile` s sütunu değerdir ile belirtilen değer, `DefaultValue` özelliği. Şekil 2'de görüldüğü gibi bu değer ayarlanırsa `DBNull`, varsayılan değer. Farklı bir varsayılan değer tercih ediyorsanız, yalnızca ayarlamak `DefaultValue` özelliği uygun şekilde. Yalnızca olduğundan emin olun `DefaultValue` değeri geçerli s sütunu verilen `DataType` (yani, `System.Int32` için `PriceQuartile` sütun).

Bu noktada biz DataTable tablosuna ek bir sütun eklemek için gerekli adımları gerçekleştirdiniz. Bu ek sütun beklendiği gibi çalıştığını doğrulamak için her ürün adı, fiyat ve fiyat DÖRTTEBİRLİK görüntüleyen bir ASP.NET sayfası oluşturma s olanak tanır. Bunu yapmadan rağmen önce DAL s çağıran bir yöntemi dahil etmek için iş mantığı katmanı güncelleştirmek ihtiyacımız `GetProductsWithPriceQuartile` yöntemi. Biz BLL sonra adım 3'te güncelleştirin ve sonra adım 4'te ASP.NET sayfası oluşturun.

## <a name="step-3-augmenting-the-business-logic-layer"></a>3. adım: iş mantığı katmanı program.cs'ye

Yeni kullanırız önce `GetProductsWithPriceQuartile` yöntemi sunu katmanı, biz öncelikle karşılık gelen bir yöntemi BLL eklemeniz gerekir. Açık `ProductsBLLWithSprocs` sınıf dosya ve aşağıdaki kodu ekleyin:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Gibi diğer veri alma yöntemleri `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` yöntemi yalnızca DAL çağırır s karşılık gelen `GetProductsWithPriceQuartile` yöntemi ve sonuçlarını döndürür.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>4. adım: bir ASP.NET Web sayfası fiyat DÖRTTEBİRLİK bilgilerini görüntüleme

BLL eklenmesiyle tamamlamak biz re her ürün için fiyat DÖRTTEBİRLİK gösteren ASP.NET sayfası oluşturmak için hazır. Açık `AddingColumns.aspx` sayfasındaki `AdvancedDAL` klasörü ve sürükleme GridView ayarı tasarımcıya araç kendi `ID` özelliğine `Products`. İsteğe bağlı olarak GridView s akıllı etiketten adlı yeni bir ObjectDataSource bağlama `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLLWithSprocs` s sınıfı `GetProductsWithPriceQuartile` yöntemi. Bu salt okunur bir kılavuz olacağından, güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image25.png))


[![Ürün bilgisini almak GetProductsWithPriceQuartile yöntemi](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Şekil 10**: ürün bilgilerini almak `GetProductsWithPriceQuartile` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image28.png))


Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio otomatik olarak bir BoundField veya CheckBoxField GridView yöntemi tarafından döndürülen veri alanların her biri için ekler. Bu veri alanları biri `PriceQuartile`, eklediğimiz için sütun olduğu `ProductsDataTable` 1. adımda.

Kaldırma GridView s alanları düzenleyin dışındaki tüm `ProductName`, `UnitPrice`, ve `PriceQuartile` BoundFields. Yapılandırma `UnitPrice` değer bir para birimi olarak biçimlendirmek ve BoundField `UnitPrice` ve `PriceQuartile` BoundFields sağa ve merkezi-hizalı, sırasıyla. Son olarak, kalan BoundFields güncelleştirme `HeaderText` ürün, fiyat ve fiyat DÖRTTEBİRLİK özelliklerine sırasıyla. Ayrıca, GridView s akıllı etiketinden sıralama etkinleştir onay kutusunu işaretleyin.

Bu değişiklikler sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Şekil 11 bir tarayıcıdan sitesini ziyaret ettiğinizde bu sayfada görüntülenir. Başlangıçta, ürünleri göre kendi fiyatı uygun bir atanan her ürünle azalan düzende sıralanır, Not `PriceQuartile` değeri. Kuşkusuz bu verileri diğer ölçütlere göre hala fiyat göre ürün s sıralamasını yansıtacak şekilde fiyat DÖRTTEBİRLİK sütun değeri ile sıralanabilir (bkz. Şekil 12).


[![Ürünler kendi fiyatlarını göre sıralanır](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Şekil 11**: ürünleri fiyatları tarafından sıralanan ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image31.png))


[![Ürünler, adlarına göre sıralanır](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Şekil 12**: ürünleri, adlarına göre sıralanmış ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Böylece göre ürün satırları renkli birkaç kod satırıyla, biz GridView büyütmek kendi `PriceQuartile` değeri. Biz bu ürünler ilk DÖRTTEBİRLİK bir açık yeşil, ikinci DÖRTTEBİRLİK açık sarı de, renk ve benzeri olabilir. Bu işlevsellik eklemek için bir dakikanızı ayırın size öneririz. GridView biçimlendirme üzerinde Yenileyici gerekiyorsa, başvurun [özel biçimlendirme göre bağlı verileri](../custom-formatting/custom-formatting-based-upon-data-vb.md) Öğreticisi.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Alternatif bir yaklaşım - başka bir TableAdapter oluşturma

Ana sorgu tarafından yazılmış olanlar dışındaki veri alanları döndüren bir TableAdapter için yöntem ekleme, bu öğreticide, gördüğümüz gibi karşılık gelen sütunlara DataTable tablosuna ekleyebilirsiniz. Bu tür bir yaklaşım, ancak yalnızca az sayıda farklı veri alanlarını döndürmek TableAdapter yöntemlerinde varsa ve bu alternatif veri alanlarını çok ana sorgudan farklı değil ise iyi çalışır.

DataTable tablosuna sütun ekleme yerine başka bir TableAdapter farklı veri alanları döndüren ilk TableAdapter yöntemleri içeren veri kümesi için bunun yerine ekleyebilirsiniz. Bu öğretici için eklemek yerine `PriceQuartile` sütuna `ProductsDataTable` (burada yalnızca kullanıldığı tarafından `GetProductsWithPriceQuartile` yöntemi), ek bir TableAdapter adlı veri ekledik `ProductsWithPriceQuartileTableAdapter` kullanılan `Products_SelectWithPriceQuartile` depolanan yordam, ana sorgu olarak. Fiyat DÖRTTEBİRLİK ile ürün bilgilerini almak için gereken ASP.NET sayfaları kullandığınız `ProductsWithPriceQuartileTableAdapter`, belirtmiyor o kullanmaya devam edebilir ancak `ProductsTableAdapter`.

Yeni bir TableAdapter ekleyerek DataTables untarnished kalır ve bunların sütunları tam olarak TableAdapter s yöntemleriyle tarafından döndürülen veri alanları yansıtma. Ancak, ek TableAdapters yinelenen görevleri ve işlevselliği ortaya çıkarabilir. Örneğin, bu, ASP.NET sayfaları görüntülenen `PriceQuartile` sütun ayrıca ekleme, sağlama güncelleştir ve Sil desteği, `ProductsWithPriceQuartileTableAdapter` olması gerekir, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri düzgün yapılandırılmış. Bu özellikleri yansıtma sırada `ProductsTableAdapter` s, bu yapılandırma, ek bir adım tanıtır. Ayrıca, vardır güncelleştirmek için iki yol silin veya aracılığıyla veritabanına - ürün ekleme artık `ProductsTableAdapter` ve `ProductsWithPriceQuartileTableAdapter` sınıfları.

Bu öğretici için indirme içeren bir `ProductsWithPriceQuartileTableAdapter` sınıfını `NorthwindWithSprocs` bu alternatif bir yaklaşım gösterilmektedir veri kümesi.

## <a name="summary"></a>Özet

Çoğu senaryoda, tüm bir TableAdapter yöntemlere veri alanları aynı kümesini döndürür, ancak belirli bir yöntem veya iki ek bir alan döndürmek için ne zaman gerekebilir zamanlar. Örneğin, [ana/Ayrıntılar DataList ile bir madde işaretli listesi, ana kayıtları kullanarak ayrıntı](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) bir yönteme eklediğimiz öğretici `CategoriesTableAdapter` döndürülen ana sorgu s veri alanları ek olarak, bir `NumberOfProducts` , alan bildirilen her kategoriyle ilişkili ürünleri sayısı. Bu öğreticide bir yöntem ekleme biz Aranan `ProductsTableAdapter` döndürülen bir `PriceQuartile` ana sorgu s veri alanlara ek alan. Ek veri yakalamak için karşılık gelen sütunlara DataTable tablosuna eklemek ihtiyacımız alanları TableAdapter s yöntemlerle döndürdü.

Sütunları DataTable tablosuna el ile eklemeyi planlıyorsanız, TableAdapter saklı yordamları kullanmanız önerilir. Geçici SQL deyimlerini TableAdapter kullanıyorsa, her zaman tüm ana sorgu tarafından döndürülen veri alanları için veri alan listeleri geri yöntemleri TableAdapter Yapılandırma Sihirbazı'nı çalıştırılır. Bu sorunu saklı yordamlar için önerilen ve Bu öğreticide kullanılan neden olduğu kapsamaz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Randy Etikan, Mersin Goor, Bernadette Leigh ve Hilton Giesenow yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](updating-the-tableadapter-to-use-joins-vb.md)
> [sonraki](working-with-computed-columns-vb.md)
