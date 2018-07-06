---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Türü belirtilmiş DataSet'in TableAdapter'ları için (C#) saklı yordamlarını kullanarak mevcut | Microsoft Docs
author: rick-anderson
description: Önceki öğreticide Biz yeni saklı yordamlar üretmesini TableAdapter Sihirbazı'nı kullanmayı öğrendiniz. Bu öğreticide size bilgi nasıl aynı TableAdapter...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 3057b7c296c82b3db740d9fd345aaa222ca31328
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828876"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Mevcut saklı yordamları kullanma türü belirtilmiş DataSet'in TableAdapter'ları için (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) veya [PDF olarak indirin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> Önceki öğreticide Biz yeni saklı yordamlar üretmesini TableAdapter Sihirbazı'nı kullanmayı öğrendiniz. Bu öğreticide aynı TableAdapter Sihirbazı ile mevcut saklı yordamları nasıl çalışabileceğini öğrenin. Biz de el ile yeni saklı yordamlar eklemek öğrenin.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) erişim verileri yerine geçici SQL deyimleri için saklı yordamları kullanmak için türü belirtilmiş veri kümesi s TableAdapter'ın nasıl yapılandırılabilir gördük. Özellikle, otomatik olarak bu saklı yordamlar oluşturma TableAdapter Sihirbazı'nı ne incelenir. ASP.NET 2.0 için eski bir uygulama bağlantı noktası oluşturma veya var olan bir veri modeli çerçevesinde bir ASP.NET 2.0 Web sitesi oluştururken, veritabanı zaten ihtiyacımız saklı yordamları içeriyor yüksektir. Alternatif olarak, el ile veya otomatik oluşturur, saklı yordamlar TableAdapter Sihirbazı dışında bazı aracı ile saklı yordamlar oluşturmayı tercih edebilirsiniz.

Bu öğreticide biz varolan saklı yordamları kullanmak için TableAdapter yapılandırma konuları ele alınacaktır. Northwind veritabanı yalnızca küçük bir yerleşik saklı yordamlar olduğundan, ayrıca el ile veritabanına yoluyla Visual Studio ortamını yeni saklı yordamlar eklemek için gerekli olan adımları atacağız. Let s başlayın!

> [!NOTE]
> İçinde [veritabanı değişikliklerini bir işlemin içinde sarmalama](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) öğretici işlemleri desteklemek için TableAdapter yöntemleri eklenmiştir (`BeginTransaction`, `CommitTransaction`, vb.). Alternatif olarak, işlem gerektiren hiçbir veri erişim katmanı kodda değişiklik tamamen bir saklı yordam içinde yönetilebilir. Bu öğreticide, bir saklı yordam s bilgilerinin kapsamındaki bir işlem yürütmek için kullanılan T-SQL komutlarını şunları keşfedeceğiz.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>1. adım: Saklı yordamları Northwind veritabanına ekleme

Visual Studio, bir veritabanı için yeni saklı yordamlar eklemek kolaylaştırır. Let s tüm sütunları döndürür Northwind veritabanına yeni bir saklı yordam Ekle `Products` tablosu belirli bir sahip olanlar için `CategoryID` değeri. Sunucu Gezgini penceresinden Northwind veritabanı - veritabanı diyagramları, tablolar, görünümler ve benzeri - klasörlerinde görüntülenen şekilde genişletin. Önceki öğreticide gördüğümüz gibi saklı yordamlar klasörü veritabanı s mevcut saklı yordamları içerir. Yeni bir saklı yordam eklemek, saklı yordamlar klasörü sağ tıklatın ve bağlam menüsünden Yeni saklı yordam Ekle seçeneğini seçin.


[![Saklı yordamlar klasörü sağ tıklatın ve yeni bir saklı yordam Ekle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Şekil 1**: saklı yordamları klasörü sağ tıklatın ve yeni bir saklı yordam Ekle ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Şekil 1 gösterildiği gibi yeni saklı yordam Ekle seçeneğini belirleyerek bir komut penceresi Visual Studio'da saklı yordam oluşturmak için gereken SQL komut dosyası ana hat ile açılır. Bu betik tazeleyin ve hangi noktada saklı yordamı veritabanına eklenir. Bunu yürütme bizim işi var.

Aşağıdaki betiği girin:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Çalıştırıldığında, bu komut, Northwind veritabanına adlı yeni bir saklı yordam ekler `Products_SelectByCategoryID`. Bu saklı yordamı tek bir giriş parametre kabul eder (`@CategoryID`, türü `int`) ve tüm alanların eşleşen bu ürünlere yönelik döndürür `CategoryID` değeri.

Bunu yürüttüğünüzden `CREATE PROCEDURE` betik ve saklı yordamı veritabanına ekleme, araç çubuğunda Kaydet simgesine tıklayın veya Ctrl + S isabet. Bunu, saklı yordamlar klasörünü yenilemeler yaptıktan sonra yeni oluşturulan gösteren saklı yordamı. Ayrıca, komut penceresinde gelen subtlety değiştirecek `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` için `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Yeni bir saklı yordam, veritabanına ekler sırada `ALTER PROCEDURE` mevcut olanı güncelleştirir. Betik başlangıcını değiştiğinden `ALTER PROCEDURE`, saklı yordamları değiştirme giriş parametreleri veya SQL deyimlerini ve Kaydet simgesine tıklayarak güncelleştirecek saklı yordamı bu değişikliklerle.

Şekil 2, Visual Studio yüklendikten sonra gösterir `Products_SelectByCategoryID` saklı yordam kaydedildi.


[![Saklı yordam Products_SelectByCategoryID veritabanına eklenen](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Şekil 2**: saklı yordam `Products_SelectByCategoryID` eklendiğini veritabanına ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>2. adım: varolan bir saklı yordam kullanmak için TableAdapter yapılandırma

Şimdi `Products_SelectByCategoryID` saklı yordam eklendiğini veritabanına yapabiliriz concigure bizim veri erişim katmanı yöntemlerinden biri çağrıldığında Bu saklı yordamı kullanın. Özellikle, ekleyeceğiz bir `GetProducstByCategoryID(categoryID)` yönteme `ProductsTableAdapter` içinde `NorthwindWithSprocs` çağırır türü belirtilmiş DataSet `Products_SelectByCategoryID` saklı yordamı oluşturduğumuz.

Başlangıç açarak `NorthwindWithSprocs` veri kümesi. Sağ `ProductsTableAdapter` ve TableAdapter sorgu Yapılandırma Sihirbazı'nı başlatmak için Sorgu Ekle öğesini seçin. İçinde [önceki öğretici](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) TableAdapter bizim için yeni bir saklı yordam oluşturmak için biz seçimi yaptıysanız. Bu öğreticide, ancak varolan yeni TableAdapter yöntemi wire istiyoruz `Products_SelectByCategoryID` saklı yordamı. Bu nedenle, mevcut saklı yordam seçeneğini kullanın Sihirbazı s ilk adımda seçin ve İleri'ye tıklayın.


[![Saklı yordam seçeneği mevcut kullanımı seçin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Şekil 3**: saklı yordamı seçeneği var olanı Kullan'ı seçin ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


Aşağıdaki ekranda açılan listesini s veritabanı saklı yordamlar doldurulmuş sağlar. Bir saklı yordam seçerek, sol ve sağda (varsa) döndürülen veri alanlarını, giriş parametreleri listeler. Seçin `Products_SelectByCategoryID` saklı yordamı listeden ve İleri'ye tıklayın.


[![Products_SelectByCategoryID çekme depolanan yordamı](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Şekil 4**: çekme `Products_SelectByCategoryID` saklı yordam ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


Sonraki ekranda bize ne tür veriler saklı yordam tarafından döndürülen ister ve TableAdapter s yöntemi tarafından döndürülen tür bizim yanıt burada belirler. Örneğin, tablosal veri döndürdüğünü belirtmek, metodun döndüreceği bir `ProductsDataTable` örneği saklı yordam tarafından döndürülen kayıt ile doldurulur. Buna karşılık, bu saklı yordamı tek bir değer döndüren belirtmek, TableAdapter döndürür bir `object` saklı yordam tarafından döndürülen ilk kaydın ilk sütunundaki değeri atanır.

Bu yana `Products_SelectByCategoryID` saklı yordam, belirli bir kategoriye ait, ilk yanıt - tablo verisi-'ı seçin ve İleri'ye tıklayın, tüm ürünler döndürür.


[![Saklı yordam tablo verisi döndüren belirtin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Şekil 5**: saklı yordam tablo verisi döndüren belirtin ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Kalan tek şey yöntemi desenleri kullanmak için bu yöntemlerin adlarına göre ardından göstermek için. Bir DataTable ve dönüş DataTable seçenekleri kullanıma, ancak yeniden adlandırmak için yöntemleri iki dolgu bırakın `FillByCategoryID` ve `GetProductsByCategoryID`. Sonra sihirbazın gerçekleştireceği görevleri özetini gözden geçirmek için İleri'ye tıklayın. Her şeyin doğru görünüyorsa, Son'a tıklayın.


[![Ad yöntemleri FillByCategoryID ve GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Şekil 6**: yöntemleri ad `FillByCategoryID` ve `GetProductsByCategoryID` ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Yeni oluşturduğumuz, TableAdapter yöntemleri `FillByCategoryID` ve `GetProductsByCategoryID`, türünde bir giriş parametresi beklediğiniz `int`. Bu giriş parametresi değeri saklı yordama geçirilir, `@CategoryID` parametresi. Değiştirirseniz `Products_SelectByCategory` saklı yordam s parametreleri, ayrıca bu TableAdapter yöntemleri için parametre güncellemeniz gerekecektir. Bölümünde açıklandığı gibi [önceki öğreticide](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), bu iki yoldan biriyle yapılabilir: el ile ekleyerek veya parametreler parametre koleksiyonunu veya kaldırarak TableAdapter Sihirbazı'nı yeniden çalıştırma tarafından.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>3. adım: Ekleme bir`GetProductsByCategoryID(categoryID)`BLL yöntemi

İle `GetProductsByCategoryID` tam DAL yöntemi, sonraki adım ise erişebilmesi için bu yöntemi, iş mantığı katmanı. Açık `ProductsBLLWithSprocs` sınıf dosyası ve aşağıdaki yöntemi ekleyin:


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Yalnızca bu BLL yöntemi döndürür `ProductsDataTable` döndürüldüğü `ProductsTableAdapter` s `GetProductsByCategoryID` yöntemi. `DataObjectMethodAttribute` Özniteliği ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı tarafından kullanılan meta veri sağlar. Özellikle, bu yöntem seçme sekmesinde s aşağı açılan listede görünür.

## <a name="step-4-displaying-products-by-category"></a>4. adım: Kategoriye göre ürünler görüntüleme

Yeni eklenen test etmek için `Products_SelectByCategoryID` saklı yordam ve bir DropDownList ve GridView içeren bir ASP.NET sayfası oluşturma s izin karşılık gelen DAL ve BLL yöntemleri. GridView Seçili kategoriye ait olan ürünleri görüntülenir ancak DropDownList veritabanında kategorilerin tümünü listeler.

> [!NOTE]
> Biz oluşturulan ve ana/ayrıntı arabirimleri önceki öğreticilerdeki DropDownList kullanarak. Böyle bir ana/ayrıntı raporu yürüten bir daha derinlemesine bakış için bkz [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) öğretici.


Açık `ExistingSprocs.aspx` sayfasını `AdvancedDAL` klasör ve bir DropDownList tasarımcı araç kutusundan sürükleyin. DropDownList s ayarlamak `ID` özelliğini `Categories` ve kendi `AutoPostBack` özelliğini `true`. Ardından, kendi akıllı etiketten DropDownList adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource`. ObjectDataSource, verileri alır, böylece yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi. Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN.


[![Veri s CategoriesBLL sınıfı GetCategories yöntemi](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Şekil 7**: verileri alma `CategoriesBLL` s sınıfı `GetCategories` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Şekil 8**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


ObjectDataSource sihirbazını tamamladıktan sonra görüntülenecek DropDownList yapılandırma `CategoryName` veri alan ve kullanmak için `CategoryID` olarak alan `Value` her `ListItem`.

Bu noktada, DropDownList ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer olmalıdır:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

Ardından, GridView DropDownList yerleştirmek tasarımcıya sürükleyin. GridView s ayarlamak `ID` için `ProductsByCategory` ve isteğe bağlı olarak, akıllı etiketten adlı yeni bir ObjectDataSource bağlama `ProductsByCategoryDataSource`. Yapılandırma `ProductsByCategoryDataSource` kullanılacak ObjectDataSource `ProductsBLLWithSprocs` sınıfı, bunu olması almak, veri kullanarak `GetProductsByCategoryID(categoryID)` yöntemi. Bu GridView yalnızca verileri görüntülemek için kullanılacağından, güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN ve İleri'ye tıklayın.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Veri GetProductsByCategoryID(categoryID) yöntemi](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Şekil 10**: verileri alma `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


Bize parametresi s kaynağı Sihirbazı'nın son adım ister şekilde seçme sekmesinde seçilen yöntemin bir parametre bekliyor. Parametre kaynak aşağı açılan liste denetimine ayarlayın ve seçin `Categories` ControlId aşağı açılan listeden denetimi. Sihirbazı tamamlamak için Son'u tıklatın.


[![Kategorileri DropDownList CategoryID parametresi kaynağı olarak kullanın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Şekil 11**: kullanım `Categories` kaynağı olarak DropDownList `categoryID` parametre ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


ObjectDataSource Sihirbazı tamamladığınızda, Visual Studio BoundFields ve bir CheckBoxField ürün veri alanların her biri için ekler. Gördüğünüz gibi bu alanları Özelleştir çekinmeyin.

Bir tarayıcı aracılığıyla sayfasını ziyaret edin. İçecekler kategorisindeki seçili sayfa ve ilgili ürünler kılavuzunda listelenen ziyaret edildiğinde. Şekil 12 olarak alternatif bir kategori için açılır listede değiştirme gösterir, geri göndermeye neden olur ve yeni seçilen kategorinin ürünlerle kılavuz yeniden yükler.


[![Üretmek kategorisinde ürünleri görüntülenir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Şekil 12**: üretmek kategorisinde ürünleri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>5. adım: bir saklı yordam s deyimleri bir işlem kapsamında sarmalama

İçinde [veritabanı değişikliklerini bir işlemin içinde sarmalama](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) ele aldığımız bir dizi bir işlem kapsam içinde veritabanı değişiklik deyim gerçekleştirmeye yönelik teknikleri öğretici. Bir işlemin genel altında ya da gerçekleştirilen değişikliklerin tümü başarılı bir geri çağırma veya tüm başarısız, kararlılık güvence altına alır. İşlem kullanmak için şu teknikler kullanılabilir:

- Sınıfları kullanarak `System.Transactions` ad alanı
- Veri erişim katmanı kullanmalarına ADO.NET sınıflar gibi `SqlTransaction`, ve
- Saklı yordam içinde doğrudan T-SQL işlem komut ekleme

*Veritabanı değişikliklerini bir işlemin içinde sarmalama* Öğreticisi kullanılan ADO.NET sınıflarını DAL. Bu öğreticinin geri kalanında bir saklı yordam T-SQL komutlarını kullanarak bir işlem yönetme inceler.

El ile başlatma, yapılıyor ve bir işlemin geri alınması için üç anahtar SQL komutları `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, ve `ROLLBACK TRANSACTION`sırasıyla. ADO.NET yaklaşımıyla işlemlerden ihtiyacımız aşağıdaki deseni uygulamak için bir saklı yordam içinde kullanılırken ister:

1. Bir işlem başlangıcını gösterir.
2. İşlemi oluşturan SQL deyimlerini yürütün.
3. 2. adım, geri alma işlemi deyimlerinden herhangi birinde bir hata varsa
4. Tüm 2. adım deyimlerinin hatasız tamamlanırsa, işlem kaydedilemiyor.

Bu düzen aşağıdaki şablonu kullanarak T-SQL söz dizimini uygulanabilir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

Tanımlayarak, şablon başlatan bir `TRY...CATCH` SQL Server 2005'e yeni bir yapı bloğu. İle gibi `try...catch` engeller C# ' ta SQL `TRY...CATCH` bloğun deyimlerinde `TRY` blok. Herhangi bir deyimle bir hata oluşturuyorsa, denetim için hemen aktarılır `CATCH` blok.

Bu işlem düzenini SQL deyimlerini yürütme hata yoksa `COMMIT TRANSACTION` deyimi değişiklikleri kaydeder ve hareketi tamamlar. Ancak, deyimlerden biri bir hatayla sonuçlanırsa, `ROLLBACK TRANSACTION` içinde `CATCH` blok veritabanı işlemi başlamadan önce durumuna döndürür. Saklı yordam de kullanarak bir hata oluşturur [RAISERROR komut](https://msdn.microsoft.com/library/ms178592.aspx), hangi neden bir `SqlException` uygulama oluşturulur.

> [!NOTE]
> Bu yana `TRY...CATCH` blok SQL Server 2005'e yeni, Microsoft SQL Server'ın eski sürümlerini kullanıyorsanız, yukarıdaki şablonu çalışmaz. SQL Server 2005 kullanmıyorsanız başvurun [yönetme işlemleri SQL Server saklı yordamları](http://www.4guysfromrolla.com/webtech/080305-1.shtml) SQL Server'ın diğer sürümlerinde çalışır bir şablon.


Somut bir örneğe bakmaktır s olanak tanır. Arasında bir yabancı anahtar kısıtlaması var. `Categories` ve `Products` tablolar, yani, her `CategoryID` alanındaki `Products` tablo eşlenmelidir bir `CategoryID` değerini `Categories` tablo. Ürünler, ilişkili bir kategoriyi silinmeye çalışılıyor gibi bu kısıtlamayı ihlal ediyor herhangi bir işlem bir yabancı anahtar kısıtlaması ihlali ile sonuçlanır. Bunu doğrulamak için ikili veri bölümü ile çalışma güncelleştirme ve silme mevcut ikili verileri örnekte yeniden ziyaret (`~/BinaryData/UpdatingAndDeleting.aspx`). (Bkz. Şekil 13) Düzenle ve Sil düğmeleri birlikte sistemdeki her kategori bu sayfada listelenir, ancak silme,-İçecekler gibi-ürünler ilişkili bir kategoriyi silmek çalışırsanız, bir yabancı anahtar kısıtlaması ihlali nedeniyle başarısız olur (bkz. Şekil 14).


[![Her kategori Düzenle ve Sil düğmeleri GridView görüntülenir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Şekil 13**: her kategori Düzenle ve Sil düğmeleri GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![Var olan ürünler olan bir kategorisi silinemiyor](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Şekil 14**: var olan ürünler olan bir kategorisi silinemiyor ([tam boyutlu görüntüyü görmek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


Ancak, biz olup bunların ürünleri ilişkilendirdiğiniz bakılmaksızın silinecek kategoriler istediğinizi varsayalım. Bir kategori ürünleri ile silinmelidir, ayrıca mevcut ürünlerinden silmek istediğimizi varsayalım (başka bir seçenek ürünlerinden ayarlanacak olacak olsa da `CategoryID` değerler `NULL`). Bu işlev, yabancı anahtar kısıtlaması cascade kurallarla uygulanabilir. Alternatif olarak, kabul eden bir saklı yordam oluşturabilir bir `@CategoryID` giriş parametresi ve çağrıldığında tüm ilişkili ürünleri ve belirtilen kategori açıkça siler.

Bizim ilk denemesini saklı bir yordam aşağıdakine benzeyebilir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Bu kesinlikle kategorisi ve ilgili ürünleri siler olsa da, bunu bir işlemin genel altında yapmaz. Yoktur, diğer bir yabancı anahtar kısıtlaması Imagine `Categories` belirli bir silinmesini engelliyor `@CategoryID` değeri. Sorun kategorisini silmek denemeden önce böyle bir durumda tüm ürünleri silineceğini olmasıdır. Hala başka bir tablodaki kayıtları ilişkili olduğundan, kategori kaldığını ancak böyle bir kategori için bu saklı yordamı tüm ürünleri kaldırmanız net sonucudur.

Saklı yordamı bir işlem kapsamında ancak silmeleri sarmalanmış, `Products` tablo gerçekleştirilen adımların geri alınması silinemedi karşılaşıldığında `Categories`. İkisi arasındaki kararlılık güvence altına almak için bir işlem aşağıdaki saklı yordam betiği kullanan `DELETE` ifadeleri:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Eklemek için birkaç dakikanızı `Categories_Delete` saklı yordamını kullanarak Northwind veritabanına. Saklı yordamlar veritabanına ekleme hakkında yönergeler için geri adım 1'e bakın.

## <a name="step-6-updating-thecategoriestableadapter"></a>6. adım: Güncelleştirme`CategoriesTableAdapter`

Biz sırasında eklediyseniz `Categories_Delete` saklı yordamı veritabanına, DAL şu anda silme gerçekleştirmek için geçici SQL deyimleri kullanacak şekilde yapılandırıldı. Güncelleştirilecek ihtiyacımız `CategoriesTableAdapter` ve kullanacak şekilde istemeniz `Categories_Delete` yerine saklı yordamı.

> [!NOTE]
> Bu öğreticide daha önce biz birlikte çalıştığınız `NorthwindWithSprocs` veri kümesi. Veri kümesi yalnızca tek bir varlık vardır ancak bu `ProductsDataTable`, ve kategorileri ile çalışmak gerekiyor. Bu nedenle, söz konusu veri erişim katmanı ı m hakkında konuşurken, bu öğreticinin geri kalanında için `Northwind` veri kümesi, oluşturduğumuz önce bir [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici.


Açık Northwind veri kümesi seçin `CategoriesTableAdapter`ve Özellikler penceresine gidin. Özellikler penceresi listeleri `InsertCommand`, `UpdateCommand`, `DeleteCommand`, ve `SelectCommand` TableAdapter yanı tarafından adı ve bağlantı bilgilerini kullanılır. Genişletin `DeleteCommand` ayrıntılarını görmek için özellik. Şekil 15 gösterildiği gibi `DeleteCommand` s `ComamndType` metin gönderilecek bildirir metin özelliği ayarlandığında `CommandText` özelliği olarak geçici SQL sorgusu.


![Özellikler penceresinde özelliklerini görüntülemek için tasarımcıda CategoriesTableAdapter seçin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Şekil 15**: seçin `CategoriesTableAdapter` tasarımcısında özelliklerini Özellikler penceresinde görüntülemek için


Bu ayarları değiştirmek için Özellikler penceresinde (DeleteCommand) metnini seçin ve aşağı açılan listeden (yeni) seçin. Bu genişletme ayarları için temizleyecek `CommandText`, `CommandType`, ve `Parameters` özellikleri. Ardından, ayarlama `CommandType` özelliğini `StoredProcedure` ve saklı yordam için adını yazarak `CommandText` (`dbo.Categories_Delete`). Özelliklerin şu sırayla - ilk girdiğinizden emin olun `CommandType` ardından `CommandText` -Visual Studio otomatik olarak parametre koleksiyonunu doldurur. Bu özelliklerin şu sırayla girmezseniz parametreler parametre koleksiyon Düzenleyicisi aracılığıyla el ile eklemeniz gerekir. Her iki durumda da, s akıllıca parametre koleksiyon Düzenleyicisi'kurmak doğru parametre ayarları değişiklikleri (bkz. Şekil 16) yapılmadığını doğrulamak için parametreleri özelliği içindeki üç noktaya tıklayın. İletişim kutusunda herhangi bir parametre görmüyorsanız ekleme `@CategoryID` parametresi el ile (eklemek gerekmez `@RETURN_VALUE` parametresi).


![Parametreleri ayarların doğru olduğundan emin olun](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Şekil 16**: parametreleri ayarların doğru olduğundan emin olun


DAL güncelleştirildi sonra bir kategori silindiğinde otomatik olarak tüm ilişkili ürünlerinden silin ve bir işlemin genel altında bunu. Bunu doğrulamak için güncelleştirme ve silme mevcut ikili verileri sayfasına dönün ve kategorilerden birini için Sil düğmesine tıklayın. Tek bir tıklamayla fare, kategori ve tüm ilişkili ürünlerinden silinir.

> [!NOTE]
> Test başlamadan önce `Categories_Delete` ürünlerin yanında, seçilen kategori sayısı siler, saklı yordam olabilir akıllıca veritabanınızı yedek bir kopyasını oluşturun. Kullanıyorsanız `NORTHWND.MDF` veritabanını `App_Data`, yalnızca Visual Studio'yu kapatın ve MDF ve LDF dosyaları kopyalama `App_Data` için başka bir klasör. İşlevi test edildikten sonra veritabanı Visual Studio kapatarak geri yükleyebilirsiniz ve geçerli MDF ve LDF değiştirerek dosyaları `App_Data` yedek kopyalar ile.


## <a name="summary"></a>Özet

TableAdapter s Sihirbazı otomatik olarak saklı yordamlar için bize oluştururken, ne zaman biz zaten oluşturulmuş tür saklı yordamları veya bunları el ile veya diğer araçlarla yerine oluşturmak istediğiniz zamanlar vardır. Bu senaryolara uyum sağlamak için TableAdapter varolan bir saklı yordam işaret edecek şekilde de yapılandırılabilir. Bu öğreticide Visual Studio ortamı ile bir veritabanını el ile saklı yordamlar eklemek ve bu saklı yordamlar için TableAdapter s yöntemleri wire inceledik. Biz de başlatılıyor, yapılıyor ve bir saklı yordam içinde işlemlerden geri için kullanılan komut düzen ve T-SQL komutlarını incelenir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Hilton Geisenow ve S ren Jacob Lauritsen Teresa Murphy yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [İleri](updating-the-tableadapter-to-use-joins-cs.md)
