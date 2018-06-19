---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Türü belirtilmiş veri kümesi'nin TableAdapters (VB) için saklı yordamlar Varolanı kullanma | Microsoft Docs
author: rick-anderson
description: Önceki öğreticide TableAdapter Sihirbazı'nı kullanarak yeni saklı yordamlar oluşturmak nasıl öğrendiniz. Biz bu öğreticide öğrenin nasıl aynı TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: ac319b67c9215c5dde8e7507076ed45a1f7825c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877378"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Türü belirtilmiş veri kümesi'nin TableAdapters (VB) saklı yordamları Varolanı kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) veya [PDF indirin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Önceki öğreticide TableAdapter Sihirbazı'nı kullanarak yeni saklı yordamlar oluşturmak nasıl öğrendiniz. Bu öğreticide aynı TableAdapter Sihirbazı ile var olan saklı yordamları nasıl çalışabileceğini öğrenin. Biz de el ile yeni saklı yordamlar Veritabanımıza eklemeyi öğrenin.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) erişim geçici yerine veri SQL deyimleri saklı yordamları kullanmak için yazılan veri kümesi s TableAdapters nasıl yapılandırılabilir gördük. Özellikle, otomatik olarak bu saklı yordamları oluşturma TableAdapter Sihirbazı'nı nasıl incelendi. ASP.NET 2.0 eski bir uygulamaya bağlantı noktası oluşturma veya varolan bir veri modeli geçici bir ASP.NET 2.0 Web oluştururken, veritabanı zaten ihtiyacımız saklı yordamları içeriyor yüksektir. Alternatif olarak, el ile veya saklı yordamlar otomatik olarak oluşturur TableAdapter Sihirbazı'nı dışındaki bazı araçla saklı yordamlar oluşturmak isteyebilirsiniz.

Bu öğreticide biz TableAdapter var olan saklı yordamları kullanmak üzere yapılandırmak nasıl ele alacağız. Northwind veritabanı yalnızca yerleşik saklı yordamlar, küçük bir kümesini olduğundan, ayrıca veritabanını Visual Studio ortamı aracılığıyla el ile yeni saklı yordamlar eklemek için gerekli olan adımları ele alacağız. Let s başlayın!

> [!NOTE]
> İçinde [kaydırma veritabanı değişiklikleri bir işlem içinde](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) öğreticide eklediğimiz yöntemleri işlemleri desteklemek için TableAdapter (`BeginTransaction`, `CommitTransaction`, vb.). Alternatif olarak, hiçbir veri erişim katmanı kodda değişiklik gerektiren tamamen bir saklı yordam içinde işlemleri yönetilebilir. Bu öğreticide biz bir saklı yordam s bilgilerinin kapsamındaki bir işlem yürütmek için kullanılan T-SQL komutlarıyla ele alacağız.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>1. adım: Northwind veritabanına saklı yordamlar ekleme

Visual Studio, bir veritabanına yeni saklı yordamlar eklemek kolaylaştırır. Let s tüm sütunları döndürür Northwind veritabanına yeni bir saklı yordam eklemek `Products` tablosu için belirli bir olan `CategoryID` değeri. Böylece klasörlerinde - veritabanı diyagramları, tablolar, görünümler ve benzeri - görüntülenir Sunucu Gezgini penceresinde, Northwind veritabanı genişletin. Önceki öğreticide gördüğümüz gibi saklı yordamlar klasörü veritabanı s var olan saklı yordamları içerir. Yeni bir saklı yordam eklemek için saklı yordamlar klasörünü sağ tıklatın ve bağlam menüsünden Yeni saklı yordam ekleme seçeneğini seçin.


[![Saklı yordamlar klasörünü sağ tıklatın ve yeni bir saklı yordam ekleyin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Şekil 1**: saklı yordamları klasörü sağ tıklatın ve yeni bir saklı yordam ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Şekil 1'de görüldüğü gibi yeni bir saklı yordam Ekle seçeneğini belirleyerek bir komut penceresi Visual Studio'da saklı yordam oluşturmak için gereken SQL komut dosyası ana hattı ile açılır. Bu komut dosyası ayrıntılı hale getirmek ve bu noktada saklı yordamı veritabanına eklenir, yürütmek için bizim işi var.

Aşağıdaki komut dosyasını girin:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Çalıştırıldığında, bu komut, yeni bir saklı yordam adlı Northwind veritabanı ekleyecek `Products_SelectByCategoryID`. Bu saklı yordam tek bir giriş parametre kabul eder (`@CategoryID`, türü `int`) ve eşleşen bu ürünler için alanların tümünü döndürür `CategoryID` değeri.

Bu yürütmek için `CREATE PROCEDURE` komut dosyası ve saklı yordam veritabanına ekleyin, araç çubuğunda Kaydet simgesine tıklayın veya Ctrl + S isabet. Bunu, saklı yordamlar klasörü yenilemeler yaptıktan sonra yeni oluşturulan gösteren saklı yordamı. Komut penceresinde gelen subtlety da, değiştirir `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` için `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` veritabanına yeni bir saklı yordam ekler sırada `ALTER PROCEDURE` mevcut bir güncelleştirir. Komut dosyası başlangıcı için değiştikten `ALTER PROCEDURE`, saklı yordamları değiştirme giriş parametreleri veya SQL deyimlerini ve Kaydet simgesine tıklayarak güncelleştirilecek saklı yordamı bu değişikliklerle.

Şekil 2 gösterir sonra Visual Studio `Products_SelectByCategoryID` saklı yordam kaydedildi.


[![Saklı yordam Products_SelectByCategoryID veritabanına eklendi](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Şekil 2**: saklı yordam `Products_SelectByCategoryID` eklendi veritabanına ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>2. adım: var olan bir saklı yordamı kullanmak için TableAdapter yapılandırma

Şimdi `Products_SelectByCategoryID` saklı yordam eklendi veritabanına biz bizim yöntemlerinden birini çağrıldığında Bu saklı yordamı kullanmak için veri erişim katmanı yapılandırabilirsiniz. Özellikle, ekleyeceğiz bir `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` yönteme `ProductsTableAdapter` içinde `NorthwindWithSprocs` yazılan çağıran DataSet `Products_SelectByCategoryID` saklı yordamı yeni oluşturduğumuz.

Başlangıç açarak `NorthwindWithSprocs` veri kümesi. Sağ `ProductsTableAdapter` ve TableAdapter sorgu Yapılandırma Sihirbazı'nı başlatmak için Sorgu Ekle'i seçin. İçinde [önceki öğretici](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) biz bize için yeni bir saklı yordam oluşturma TableAdapter olmasını seçti. Bu öğreticide, ancak varolan yeni TableAdapter yöntemi wire istiyoruz `Products_SelectByCategoryID` saklı yordamı. Bu nedenle, kullanım varolan saklı yordam seçeneği sihirbaz s ilk adımdan ve İleri'yi tıklatın.


[![Saklı yordam seçeneği mevcut Kullan'ı seçin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Şekil 3**: kullanım varolan saklı yordamı seçeneği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Aşağıdaki ekran aşağı açılan liste s veritabanıyla saklı yordamlar doldurulmuş sağlar. Saklı yordam seçerek sol ve sağ taraftaki (varsa) döndürülen veri alanları kendi giriş parametreleri listeler. Seçin `Products_SelectByCategoryID` saklı yordamı listesinden ve İleri'yi tıklatın.


[![Products_SelectByCategoryID çekme saklı yordamı](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Şekil 4**: çekme `Products_SelectByCategoryID` saklı yordam ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Sonraki ekranda bize ne tür veriler saklı yordam tarafından döndürülen ister ve burada bizim yanıt TableAdapter s yöntemi tarafından döndürülen türü belirler. Örneğin, tablo veri döndürdüğünü belirtmek, yöntemi döndürür bir `ProductsDataTable` örneği saklı yordam tarafından döndürülen kayıt ile doldurulur. TableAdapter Bu saklı yordam tek bir değer döndüren gösteriyorsa, buna karşılık, döndürülecek bir `Object` saklı yordam tarafından döndürülen ilk kaydının ilk sütunundaki değeri atanır.

Bu yana `Products_SelectByCategoryID` saklı yordamı belirli bir kategoriye ait, ilk yanıt - tablo verisi-'ı seçin ve İleri'yi tıklatın, tüm ürünleri döndürür.


[![Saklı yordam tablo verisi döndüren belirtin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Şekil 5**: saklı yordam tablo verisi döndüren belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Kalan tek şey yöntemi desenleri kullanmak için bu yöntemlere ait adlarıyla ardından belirtmek için. Her iki dolgu bir DataTable ve Return DataTable seçenekleri işaretli, ancak yöntemlerini yeniden adlandırmak bırakın `FillByCategoryID` ve `GetProductsByCategoryID`. Sihirbazın gerçekleştireceği görevleri özetini gözden geçirmek için İleri'ye tıklayın. Her şey doğru görünüyorsa, Son'u tıklatın.


[![Ad yöntemleri FillByCategoryID ve GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Şekil 6**: yöntemler ad `FillByCategoryID` ve `GetProductsByCategoryID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Yeni oluşturduğumuz, TableAdapter yöntemleri `FillByCategoryID` ve `GetProductsByCategoryID`, türünde bir giriş parametresi beklediğiniz `Integer`. Bu giriş parametresi değeri saklı yordam içinde geçirilen kendi `@CategoryID` parametresi. Değiştirirseniz `Products_SelectByCategory` saklı yordam s parametreleri, aynı zamanda bu TableAdapter yöntemleri parametrelerini güncelleştirmeniz gerekir. ' Da anlatıldığı gibi [önceki öğretici](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), bu iki yoldan biriyle yapılabilir: el ile ekleyerek veya parametreleri parametreleri koleksiyonundan veya kaldırarak TableAdapter sihirbazını yeniden tarafından.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>3. adım: Ekleme bir`GetProductsByCategoryID(categoryID)`BLL yöntemi

İle `GetProductsByCategoryID` DAL tam, sonraki adıma yöntemdir iş mantığı katmanı Bu yöntemde erişim sağlamak için. Açık `ProductsBLLWithSprocs` sınıf dosya ve aşağıdaki yöntemi ekleyin:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Bu BLL yöntemi yalnızca döndürür `ProductsDataTable` döndürülen `ProductsTableAdapter` s `GetProductsByCategoryID` yöntemi. `DataObjectMethodAttribute` Özniteliği ObjectDataSource s veri kaynağı Yapılandırma Sihirbazı tarafından kullanılan meta veri sağlar. Özellikle, bu yöntem seçme sekmesinde s aşağı açılan listede görüntülenir.

## <a name="step-4-displaying-products-by-category"></a>4. adım: Ürün kategorisine göre görüntüleme

Yeni eklenen test etmek için `Products_SelectByCategoryID` saklı yordam ve bir DropDownList ve GridView içeren bir ASP.NET sayfası oluşturma s izin ilgili DAL ve BLL yöntemleri. GridView seçilen kategoriye ait ürünleri görüntüler sırada DropDownList tüm veritabanındaki kategorilerini listeler.

> [!NOTE]
> Biz oluşturulan hedefteki ana/ayrıntı arabirimleri DropDownLists önceki eğitimlerine kullanma. Böyle bir ana öğe/ayrıntı raporu uygulayan bir daha derinlemesine bakış için bkz [ana/ayrıntı filtreleme ile bir DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) Öğreticisi.


Açık `ExistingSprocs.aspx` sayfasındaki `AdvancedDAL` klasörü ve araç tasarımcıya bir DropDownList sürükleyin. DropDownList s ayarlamak `ID` özelliğine `Categories` ve kendi `AutoPostBack` özelliğine `True`. Ardından, akıllı etiketten DropDownList adlı yeni bir ObjectDataSource bağlama `CategoriesDataSource`. Böylece kendi veri alır ObjectDataSource yapılandırma `CategoriesBLL` s sınıfı `GetCategories` yöntemi. Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![S CategoriesBLL sınıfı GetCategories yönteminden verilerini alma](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Şekil 7**: verileri alma `CategoriesBLL` s sınıfı `GetCategories` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) silme](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Şekil 8**: aşağı açılan listeler güncelleştirme, ekleme ve silme sekmeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


ObjectDataSource Sihirbazı'nı tamamladıktan sonra görüntülenecek DropDownList yapılandırma `CategoryName` veri alan ve kullanmak için `CategoryID` olarak alan `Value` her `ListItem`.

Bu noktada, DropDownList ve ObjectDataSource s bildirim temelli biçimlendirmesi aşağıdakine benzer olmalıdır:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Ardından, GridView DropDownList yerleştirme Tasarımcısı sürükleyin. GridView s ayarlamak `ID` için `ProductsByCategory` ve akıllı etiketten adlı yeni bir ObjectDataSource bağlayın `ProductsByCategoryDataSource`. Yapılandırma `ProductsByCategoryDataSource` kullanılacak ObjectDataSource `ProductsBLLWithSprocs` sınıfı, onu sahip almak, verileri kullanarak `GetProductsByCategoryID(categoryID)` yöntemi. Bu GridView yalnızca verileri görüntülemek için kullanılacağından, güncelleştirme, ekleme, açılan listeleri ayarlamak ve sekmeleri (hiçbiri) SİLİN ve İleri'yi tıklatın.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Şekil 9**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Verileri elde edilen GetProductsByCategoryID(categoryID) yöntemi](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Şekil 10**: verileri alma `GetProductsByCategoryID(categoryID)` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Sihirbazın son adım bize parametre s kaynağı ister şekilde SELECT sekmede seçilen yöntemi bir parametre bekler. Parametre kaynak aşağı açılan listesi denetimine ayarlayın ve seçin `Categories` ControlId aşağı açılan listeden denetim. Sihirbazı tamamlamak için Son'u tıklatın.


[![Kategoriler DropDownList adlı kullanıcı, Categoryıd'si parametresi kaynağı olarak kullanın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Şekil 11**: kullanım `Categories` DropDownList kaynağı olarak `categoryID` parametre ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


ObjectDataSource Sihirbazı tamamladığınızda, Visual Studio BoundFields ve bir CheckBoxField ürün veri alanların her biri için ekleyeceksiniz. Bu alanların uygun gördüğünüz şekilde özelleştirmek çekinmeyin.

Bir tarayıcı aracılığıyla sayfasını ziyaret edin. İçecekler kategorisini seçili sayfa ve kılavuzunda listelenen ilgili ürünler ziyaret ederken. Şekil 12 olarak alternatif bir kategori için aşağı açılan liste değiştirme gösterir, geri gönderimin neden olur ve yeni seçili kategorinin ürünlerle kılavuz yeniden yükler.


[![Üretmek kategori ürünleri görüntülenir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Şekil 12**: üretmek kategorideki ürünleri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>5. adım: bir saklı yordam s deyimleri bir işlem kapsamı içinde sarmalama

İçinde [kaydırma veritabanı değişiklikleri bir işlem içinde](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) biz açıklanan teknikleri bir işlem kapsamı içinde veritabanı değişikliği bildirimlerini bir dizi gerçekleştirmek için öğretici. Bir işlem şemsiyesi ya da yapılan değişiklikler tüm başarılı geri çağırma veya tüm başarısız, kararlılık güvence altına alır. İşlemleri kullanma için teknikler şunlardır:

- Sınıflarda kullanarak `System.Transactions` ad alanı
- Veri erişim katmanı kullanmalarına ADO.NET sınıflarını gibi `SqlTransaction`, ve
- Saklı yordam içinde doğrudan T-SQL işlem komut ekleme

*Kaydırma veritabanı değişiklikleri bir işlem içinde* Öğreticisi kullanılan ADO.NET sınıflarını DAL. Bu öğreticinin geri kalanında bir saklı yordam içinde T-SQL komutları kullanarak bir işlem yönetme inceler.

El ile başlatma, yürütme ve bir işlemin geri alınması için üç anahtar SQL komutları şunlardır `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, ve `ROLLBACK TRANSACTION`sırasıyla. ADO.NET yaklaşımda ihtiyacımız aşağıdaki desen uygulamak için bir saklı yordam içinde hareketleri kullanırken ister:

1. Bir işlem başlangıcını gösterir.
2. İşlem oluşturan SQL deyimlerini yürütün.
3. Adım 2 ', geri alma işlemi deyimleri herhangi birinde bir hata varsa.
4. Adım 2'deyimleri tümünün hatasız tamamlarsanız, işlem uygulayın.

Bu desen T-SQL söz dizimi aşağıdaki şablonu kullanarak uygulanabilir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Şablon tanımlayarak başlatır bir `TRY...CATCH` SQL Server 2005'e yeni bir yapı bloğu. İle gibi `Try...Catch` Visual Basic'te, SQL engeller `TRY...CATCH` bloğu yürütür deyimlerinde `TRY` bloğu. Herhangi bir deyimle hata geçirirse denetimi için hemen aktarılır `CATCH` bloğu.

SQL deyimleri bu oluşma şekli işlem yürütülürken hata varsa `COMMIT TRANSACTION` deyimi değişiklikleri kaydeder ve işlem tamamlar. Ancak, deyimlerden bir hatayla sonuçlanırsa, `ROLLBACK TRANSACTION` içinde `CATCH` blok veritabanı işlem başlamadan önce durumuna döndürür. Saklı yordamı kullanarak bir hata da başlatır [RAISERROR komutu](https://msdn.microsoft.com/library/ms178592.aspx), hangi neden bir `SqlException` uygulamada oluşturulması için.

> [!NOTE]
> Bu yana `TRY...CATCH` blok SQL Server 2005'e yeni, Microsoft SQL Server'ın eski sürümlerini kullanıyorsanız, yukarıdaki şablonu çalışmaz. SQL Server 2005 kullanmıyorsanız başvurun [SQL Server saklı yordamlar yönetme işlemlerinde](http://www.4guysfromrolla.com/webtech/080305-1.shtml) SQL Server'ın diğer sürümlerinde çalışır bir şablon.


S somut bir örneğe bakın sağlar. Arasında bir yabancı anahtar kısıtlaması var. `Categories` ve `Products` tabloları, yani, her `CategoryID` alanındaki `Products` tablo eşlenmesi için bir `CategoryID` değeri `Categories` tablo. Ürünler, ilişkili bir kategori silme girişimi gibi bu kısıtlamayı ihlal ediyor herhangi bir eylem bir yabancı anahtar kısıtlaması ihlaline neden oluyor. Bunu doğrulamak için ikili veri bölümü ile çalışma güncelleştirme ve silme Varolan ikili verileri örnekte yeniden ziyaret (`~/BinaryData/UpdatingAndDeleting.aspx`). (Bkz. Şekil 13) düzenleme ve silme düğmeleri birlikte sistemindeki her kategori bu sayfada listelenir, ancak ürünler - Meşrubat gibi-ilişkili olan bir kategori silmeye çalışırsanız silme bir yabancı anahtar kısıtlaması ihlali nedeniyle başarısız (Şekil 14 bakın).


[![Her kategori düzenleme ve silme düğmeleri ile GridView görüntülenir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Şekil 13**: her kategori düzenleme ve silme düğmeleri ile GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Var olan ürünler sahip bir kategori silinemiyor](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Şekil 14**: var olan ürünler sahip bir kategori silemezsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Ancak, biz olup bunların ürünleri ilişkilendirdiğiniz bağımsız olarak silinmesi için kategoriler izin vermek istediğiniz düşünün. Bir kategori ürünlerle izin silinmesi, aynı zamanda mevcut ürünlerinden silmek istediğimizi düşünün (yalnızca ürünlerinden ayarlamak için başka bir seçenek olabilir ancak `CategoryID` değerler `NULL`). Bu işlevsellik, yabancı anahtar kısıtlaması cascade kurallarla uygulanabilir. Alternatif olarak, biz kabul eden bir saklı yordam oluşturabilirsiniz bir `@CategoryID` giriş parametresi ve çağrıldığında, tüm ilişkili ürünleri ve belirtilen kategori açıkça siler.

Bu tür bir saklı yordam bizim ilk teşebbüs aşağıdakine benzeyebilir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Bu kesinlikle ilişkili ürünleri ve kategori silinmesine neden olur, ancak, bunu bir işlem şemsiyesi yapmaz. Olduğundan başka bir yabancı anahtar kısıtlaması düşünün `Categories` belirli bir silinmesini engellemek `@CategoryID` değeri. Kategori Sil denemeden önce böyle bir durumda ürünlerin tamamı silineceğini sorunudur. Hala başka bir tablo kayıtlarında ilişkili olduğundan kategori kalan sırada bu tür bir kategori için bu saklı yordam tüm ürünlerinde kaldırmanız net sonucudur.

Saklı yordamı bir işlem kapsamı içinde ancak silmeleri Sarmalanan varsa `Products` tablo geri silinemedi karşısında `Categories`. İkisi arasındaki kararlılık güvence altına almak için aşağıdaki saklı yordamı komut dosyası bir işlem kullanır `DELETE` deyimleri:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Eklemek için bir dakikanızı ayırın `Categories_Delete` saklı yordamı Northwind veritabanına. Geri saklı yordamlar bir veritabanına ekleme hakkında yönergeler için adım 1'e bakın.

## <a name="step-6-updating-thecategoriestableadapter"></a>6. adım: Güncelleştirme`CategoriesTableAdapter`

Hedefteki sırasında eklediğimiz `Categories_Delete` DAL veritabanına saklı yordam şu anda silme gerçekleştirmek için geçici SQL deyimlerini kullanacak şekilde yapılandırıldı. Güncelleştirmek ihtiyacımız `CategoriesTableAdapter` ve kullanmak için talimatını `Categories_Delete` yerine saklı yordamı.

> [!NOTE]
> Bu öğreticide daha önce biz birlikte çalıştığınız `NorthwindWithSprocs` veri kümesi. Veri kümesi yalnızca tek bir varlık vardır ancak bu `ProductsDataTable`, ve kategorileri ile çalışması gerekir. Bu nedenle, Bu öğretici için başvuran veri erişim katmanı ı m hakkında konuşurken kalanı için `Northwind` veri kümesi, ilk olarak oluşturduğumuz bir [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) Öğreticisi.


Açık Northwind veri kümesini seçin `CategoriesTableAdapter`ve Özellikler penceresine gidin. Özellikler penceresi listeleri `InsertCommand`, `UpdateCommand`, `DeleteCommand`, ve `SelectCommand` TableAdapter yanı sıra tarafından adını ve bağlantı bilgilerini kullanılır. Genişletme `DeleteCommand` ayrıntılarını görmek için özellik. Şekil 15 gösterildiği gibi `DeleteCommand` s `ComamndType` özellik metin gönderilecek bildirir metin olarak ayarlanmış `CommandText` özelliği geçici SQL sorgusu olarak.


![Özellikler penceresinde özelliklerini görüntülemek için Tasarımcısı'nda CategoriesTableAdapter seçin](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Şekil 15**: seçin `CategoriesTableAdapter` Özellikler penceresinde özelliklerini görüntülemek için Tasarımcısı'nda


Bu ayarları değiştirmek için Özellikler penceresinde (DeleteCommand) metni seçin ve aşağı açılan listeden (yeni) seçin. Bu genişletme ayarları için temizleyecek `CommandText`, `CommandType`, ve `Parameters` özellikleri. Ardından, ayarlayın `CommandType` özelliğine `StoredProcedure` ve saklı yordam için adına yazın `CommandText` (`dbo.Categories_Delete`). Özelliklerin şu sırayla - ilk girdiğinizden emin olun, `CommandType` ve ardından `CommandText` -Visual Studio otomatik olarak parametreler koleksiyonu doldurur. Bu sırada bu özellikleri girmezseniz aracılığıyla parametreler koleksiyonu Düzenleyicisi parametreleri el ile eklemeniz gerekir. Her iki durumda da, parametreler koleksiyonu Düzenleyicisi yukarı doğru parametre ayarları değişiklikler (bkz. Şekil 16) yapılmıştır doğrulamak için duruma getirmek için parametreleri özelliğinde üç noktaya tıklayın akıllıca s. İletişim kutusunda herhangi bir parametre görmüyorsanız eklemek `@CategoryID` parametresi el ile (eklemek gerekmez `@RETURN_VALUE` parametresi).


![Parametreleri ayarların doğru olduğundan emin olun](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Şekil 16**: parametreleri ayarların doğru olduğundan emin olun


DAL güncelleştirilmiş sonra bir kategori silindiğinde otomatik olarak tüm ilişkili ürünlerinde silin ve bir işlem şemsiyesi bunu. Bunu doğrulamak için güncelleştirme ve silme Varolan ikili veri sayfasına dönün ve kategorilerden birini Sil düğmesini tıklatın. Tek tek bir tıklatmayla fare, kategori ve tüm ilişkili ürünlerinde silinir.

> [!NOTE]
> Sınama önce `Categories_Delete` ürünleri seçilen kategori yanı sıra sayısı silecek, saklı yordam, olabilir, veritabanının bir yedek kopyasının akıllıca. Kullanıyorsanız `NORTHWND.MDF` veritabanını `App_Data`, yalnızca Visual Studio'yu kapatın ve MDF ve LDF dosyalarını kopyalayın `App_Data` başka bir klasör için. İşlevselliğini test etme sonra veritabanını Visual Studio kapatarak geri yükleyebilirsiniz ve geçerli MDF ve LDF değiştirme dosyaları `App_Data` yedek kopyaları ile.


## <a name="summary"></a>Özet

TableAdapter s Sihirbazı otomatik olarak saklı yordamlar bize oluşturur, ancak ne zaman biz zaten oluşturulmuş böyle saklı yordamları veya bunları el ile veya diğer araçları ile bunun yerine oluşturmak istediğiniz zamanlar vardır. Bu tür senaryolara yer vermek için TableAdapter varolan bir saklı yordamı işaret edecek şekilde de yapılandırılabilir. Bu öğreticide saklı yordamlar Visual Studio ortamı ile bir veritabanını el ile ekleyin ve bu saklı yordamları TableAdapter s yöntemleri wire inceledik. Biz de T-SQL komutlarını ve komut dosyası düzeni başlatma, yürütme ve işlemlerin bir saklı yordam içinde geri kullanılan incelendi.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Hilton Geisenow, S ren Jacob Lauritsen ve Teresa Murphy yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [sonraki](updating-the-tableadapter-to-use-joins-vb.md)
