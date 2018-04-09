---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: TableAdapter kullanımı için güncelleştirme birleşimler (C#) | Microsoft Docs
author: rick-anderson
description: Bir veritabanı ile çalışırken, birden çok tablo arasında yayılır istek verileri için yaygındır. İki farklı tablolardan veri almak için şu ya da kullanabilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: be74be8865b021be1f2e2d8181d2eb42cb74eb75
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>TableAdapter kullanımı için güncelleştirme (C#) birleştirir
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) veya [PDF indirin](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Bir veritabanı ile çalışırken, birden çok tablo arasında yayılır istek verileri için yaygındır. İki farklı tablolardan veri almak için şu ilişkili bir alt sorgu veya bir birleştirme işlemi kullanabilirsiniz. Bu öğreticide biz bağıntılı alt sorgular ve JOIN söz dizimi ana sorguda bir birleştirme içeren bir TableAdapter oluşturma bakarak önce karşılaştırın.


## <a name="introduction"></a>Giriş

İlişkisel veritabanları ile biz ile çalışma ilgilendiğiniz veri genellikle birden çok tablo arasında yayılır. Örneğin, ürün bilgilerini görüntülerken, büyük olasılıkla her ürün s karşılık gelen kategorisi ve tedarikçi s adlarını listelemek istiyoruz. `Products` Tablolu `CategoryID` ve `SupplierID` değerleri, ancak gerçek kategori ve sağlayıcı adları olan `Categories` ve `Suppliers` tabloları, sırasıyla.

Başka bir, ilgili tablosundan bilgi almak için biz kullanabilir *alt sorgulara bağıntılı* veya `JOIN` *s*. İlişkili bir alt sorgu iç içe bir olduğu `SELECT` dış sorgu sütunlarında başvuran sorgu. Örneğin, [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md) iki bağıntılı sorgularda kullandık öğretici `ProductsTableAdapter` her ürün için kategori ve tedarikçi adlarını döndürmek için ana sorgu s. A `JOIN` iki farklı tablolardan ilgili satırları birleştirir SQL yapısıdır. Kullandık bir `JOIN` içinde [SqlDataSource denetimi sorgulama verilerle](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) her ürünün yanına kategori bilgileri görüntülemek için öğretici.

Biz abstained kullanımından neden `JOIN` TableAdapters ile s'dir karşılık gelen otomatik oluşturma için TableAdapter s Sihirbazı'nı sınırlamaları nedeniyle `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Daha açık belirtmek gerekirse TableAdapter s ana sorgu herhangi içeriyorsa, `JOIN` s, TableAdapter otomatik-oluşturamıyor geçici SQL deyimlerini veya için saklı yordamlar, `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri.

Bu öğretici biz kısaca karşılaştırın ve karşıtlık alt sorgulara bağıntılı ve `JOIN` içeren bir TableAdapter oluşturma keşfetme önce s `JOIN` s ana sorgu.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Karşılaştırın ve çakışan bağıntılı alt sorgular ve`JOIN` s

Sözcüğünün `ProductsTableAdapter` ilk öğreticide oluşturduğunuz `Northwind` DataSet her ürün s karşılık gelen kategori ve üretici adı geri getirmek için ilişkili alt sorgular kullanır. `ProductsTableAdapter` s ana sorgu aşağıda gösterilmektedir.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

İki alt sorgulara - bağıntılı `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` ve `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -olan `SELECT` dış içinde ek bir sütun olarak ürün başına tek bir değer döndüren sorgular `SELECT` deyimi s sütun listesi.

Alternatif olarak, bir `JOIN` her ürün s sağlayıcı ve kategori adını döndürmek için kullanılır. Aşağıdaki sorgu, yukarıdaki biri aynı çıktıyı döndürür ancak kullanan `JOIN` s alt sorgular yerine:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A `JOIN` bir tablodan kayıt bazı ölçütlere göre başka bir tablodan kayıt ile birleştirir. Yukarıdaki sorguda için `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` SQL Server'ın her birleştirme bildirir ürün kaydı kategorisiyle kayıt `CategoryID` değerle s ürün `CategoryID` değeri. Her ürün için karşılık gelen kategori alanlarını çalışmak için bize birleştirilmiş sonuç verir (gibi `CategoryName`).

> [!NOTE]
> `JOIN` İlişkisel veritabanlarından veri sorgulanırken s yaygın olarak kullanılan. Yeni olması durumunda `JOIN` sözdizimi veya kullanım üzerinde biraz tazelemek gerek, d ı öneririz [SQL JOIN öğretici](http://www.w3schools.com/sql/sql_join.asp) adresindeki [W3 okullar](http://www.w3schools.com/). Aynı zamanda okuma değer olan [ `JOIN` Temelleri](https://msdn.microsoft.com/library/ms191517.aspx) ve [alt sorgu Temelleri](https://msdn.microsoft.com/library/ms189575.aspx) bölümlerini [SQL Books Online](https://msdn.microsoft.com/library/ms130214.aspx).


Bu yana `JOIN` s ve ilişkili alt sorgulara hem de diğer tablolardan ilgili verileri almak için kullanılabilir, geliştiricilerin çoğu sol kendi uyarı scratching ve kullanmak için hangi yaklaşımın merak ediyor. Tüm SQL uzmanları ı aynısını kabaca denirse açıklandı ve onun içermiyor t gerçekten önemli performance-wise SQL Server kabaca aynı yürütme planları oluşturacak şekilde. Ardından, kendi öneriler sizin ve ekibinizin en uygun olanını teknik kullanmaktır. Bu, bu önerileri imparting sonra bu uzmanlar hemen kendi tercih ifade ettiğini belirtmeye değeri `JOIN` bağıntılı alt sorgulara üzerinden s.

Yazılan veri kümeleri kullanarak veri erişim katmanı oluştururken, Araçlar alt sorgulara kullanırken daha iyi çalışır. Özellikle, TableAdapter s Sihirbazı'nı otomatik-karşılık gelen oluşturmaz `INSERT`, `UPDATE`, ve `DELETE` ana sorgu herhangi içeriyorsa deyimleri `JOIN` s, ancak otomatik oluşturur-bağıntılı olduğunda bu deyimleri alt sorgular kullanılır.

Bu eksiklikleri keşfetmek için bir geçici yazılan veri kümesinde oluşturma `~/App_Code/DAL` klasör. TableAdapter Yapılandırma Sihirbazı sırasında geçici SQL deyimlerini aşağıdaki girin kullanmayı tercih ve `SELECT` sorgu (bkz: Şekil 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Birleştirmeler içeren ana bir sorgu girin](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Şekil 1**: ana sorgusu, CONTAINS girin `JOIN` s ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Varsayılan olarak, TableAdapter otomatik olarak oluşturacak `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorguya dayalı. Gelişmiş düğmesine tıklarsanız, bu özellik etkin olduğunu görebilirsiniz. Bu ayar rağmen TableAdapter oluşturmak mümkün olmaz `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorgu içerdiğinden bir `JOIN`.


![Birleştirmeler içeren ana bir sorgu girin](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Şekil 2**: içeren ana bir sorgu girin `JOIN` s


Sihirbazı tamamlamak için Son'u tıklatın. Bu noktada, veri kümesi s Tasarımcısı sütunlarla DataTable ile tek bir TableAdapter döndürülen alanların her biri için dahil edilir `SELECT` sorgu s sütun listesi. Bu içerir `CategoryName` ve `SupplierName`, Şekil 3'te gösterildiği gibi.


![DataTable sütun listesinde döndürülen her bir alan için bir sütun içerir.](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Şekil 3**: sütun listesinde her bir alan döndürülen DataTable bir sütun içerir.


DataTable uygun sütunları sahipken TableAdapter değerleri eksik kendi `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Bunu doğrulamak için Tasarımcısı'nda TableAdapter tıklayın ve Özellikler penceresine gidin. Olmadığını göreceksiniz `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri (hiçbiri) ayarlanır.


[![InsertCommand, UpdateCommand ve DeleteCommand özellikleri (hiçbiri) ayarlanır](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Şekil 4**: `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri (hiçbiri) ayarlanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Bu eksiklikleri olarak çözmek için şu el ile SQL deyimlerini ve parametrelerini sağlayabilir `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` Özellikler penceresini aracılığıyla özellikleri. Alternatif olarak, biz TableAdapter s ana sorguya yapılandırarak başlatamadığından *değil* herhangi dahil `JOIN` s. Bu, olanak sağlayacaktır `INSERT`, `UPDATE`, ve `DELETE` otomatik bize için olarak oluşturulup deyimleri. Sihirbazı tamamladıktan sonra daha sonra el ile TableAdapter s güncelleştiriyoruz `SelectCommand` onun içerecek şekilde Özellikleri penceresinde `JOIN` sözdizimi.

Bu yaklaşım çalışırken, herhangi bir TableAdapter s ana sorgu zaman çünkü geçici SQL sorgularını kullanarak Sihirbazı kullanarak, otomatik olarak oluşturulan yeniden yapılandırılmış, çok kırılır olduğunda `INSERT`, `UPDATE`, ve `DELETE` deyimleri yeniden oluşturuluyorsa. Biz TableAdapter üzerinde sağ bağlam menüsünden Yapılandır seçtiyseniz ve sihirbazı yeniden tamamlandı tüm daha sonra yapılan özelleştirmeler kaybolacak anlamına gelir.

Kırılganlığının otomatik olarak oluşturulan TableAdapter s da artacağını `INSERT`, `UPDATE`, ve `DELETE` deyimleri olan Neyse ki, geçici SQL deyimlerini sınırlıdır. Saklı yordamlar, TableAdapter kullanıyorsa, özelleştirebileceğiniz `SelectCommand`, `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` saklı yordamlar ve saklı yordamları olacağını endişe gerek kalmadan TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın değiştirdi.

Sonraki biz oluşturacak bir TableAdapter, başlangıçta, çeşitli adımları kullanan herhangi atlar ana bir sorgu `JOIN` s karşılık gelen eklemek için güncelleştirme ve silme saklı yordamları, otomatik olarak oluşturulan olacaktır. Ardından güncelleştireceğiz `SelectCommand` şekilde kullanan bir `JOIN` ilişkili tablolardan ek sütunlar getirir. Son olarak, karşılık gelen bir iş mantığı katmanı sınıf oluşturun ve bir ASP.NET web sayfası TableAdapter kullanarak gösterin.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>1. adım: basitleştirilmiş bir ana sorgu kullanarak TableAdapter oluşturma

Bu öğretici için bir TableAdapter ve kesin türü belirtilmiş DataTable için ekleyeceğiz `Employees` tablosundaki `NorthwindWithSprocs` veri kümesi. `Employees` Tablo içeren bir `ReportsTo` belirtilen alan `EmployeeID` çalışan s Yöneticisi. Örneğin, Ayla Ilgaz sahip çalışan bir `ReportTo` olan 5 değerini `EmployeeID` Steven Buchanan. Sonuç olarak, Anne Steven, kendi manager raporlar. Her çalışan s raporlama birlikte `ReportsTo` değeri, biz de isteyebilirsiniz kendi yöneticisinin adını almak. Bu kullanılarak gerçekleştirilebilir bir `JOIN`. Ancak kullanarak bir `JOIN` başlangıçta TableAdapter Oluşturma Sihirbazı'nı karşılık gelen Ekle otomatik olarak üretmek önleyen, güncelleştirme ve silme özellikleri. Bu nedenle, biz, ana sorgu herhangi içermediğinden bir TableAdapter oluşturarak başlar `JOIN` s. Ardından, 2. adımda Yöneticisi s ad yoluyla almak için ana sorgu saklı yordamı güncelleştireceğiz bir `JOIN`.

Başlangıç açarak `NorthwindWithSprocs` kümesinde `~/App_Code/DAL` klasör. Designer'ı sağ tıklatın, ardından bağlam menüsünden Ekle seçeneğini seçin ve TableAdapter menü öğesini seçin. Bu TableAdapter Yapılandırma Sihirbazı'nı başlatır. Şekil 5 gösterilmektedir gibi yeni saklı yordamlar oluşturmak ve İleri'yi tıklatın Sihirbazı sahiptir. Yeni oluşturma Yenileyici saklı yordamları TableAdapter s Sihirbazı'ndan için başvurun [oluşturma yeni saklı yordamlar için yazılan veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Öğreticisi.


[![Create Yeni saklı yordamlar seçeneği seçin](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Şekil 5**: Select oluştur Yeni saklı yordamlar seçeneği ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Aşağıdaki `SELECT` TableAdapter s ana sorgu bildirimi:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Bu sorgu içermez beri `JOIN` s, TableAdapter Sihirbazı otomatik olarak oluşturacak saklı yordamlar karşılık gelen ile `INSERT`, `UPDATE`, ve `DELETE` ana yürütülmesi için bir saklı yordam yanı sıra deyimleri Sorgu.

Aşağıdaki adımı TableAdapter s saklı yordamları ad olanak tanır. Adları kullanın `Employees_Select`, `Employees_Insert`, `Employees_Update`, ve `Employees_Delete`Şekil 6'da gösterildiği gibi.


[![Ad TableAdapter s saklı yordamlar](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Şekil 6**: TableAdapter s saklı yordamlar adı ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Son adım bize TableAdapter s yöntemleri adı ister. Kullanım `Fill` ve `GetEmployees` yöntemi adları olarak. Ayrıca işaretli doğrudan veritabanı (GenerateDBDirectMethods) onay kutusu güncelleştirmeleri göndermek için Create yöntemlerini bırakın emin olun.


[![Ad TableAdapter s yöntemleri dolgu ve GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Şekil 7**: TableAdapter s yöntemleri ad `Fill` ve `GetEmployees` ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Sihirbazı tamamladıktan sonra veritabanında depolanan yordamları incelemek için bir dakikanızı ayırın. Dört yenilerini görmeniz gerekir: `Employees_Select`, `Employees_Insert`, `Employees_Update`, ve `Employees_Delete`. Ardından, inceleme `EmployeesDataTable` ve `EmployeesTableAdapter` yeni oluşturduğunuz. DataTable ana sorgu tarafından döndürülen her bir alan için bir sütun içerir. TableAdapter üzerinde tıklayın ve ardından Properties penceresine gidin. Olmadığını göreceksiniz `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri karşılık gelen saklı yordamları çağırmak için doğru şekilde yapılandırılır.


[![TableAdapter INSERT, Update içerir ve yetenekleri silin](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Şekil 8**: TableAdapter Ekle içerir, güncelleştirme ve silme özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


INSERT ile güncelleştirin ve otomatik olarak oluşturulan saklı yordamlar silin ve `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` doğru yapılandırılmış özellikleri, biz özelleştirmek hazır `SelectCommand` s saklı yordamı ek döndürmek için Her çalışan s Yöneticisi hakkında bilgi. Özellikle, güncelleştirmek ihtiyacımız `Employees_Select` saklı yordamı kullanmak için bir `JOIN` ve s Yöneticisi dönmek `FirstName` ve `LastName` değerleri. Saklı yordam güncelleştirildikten sonra Biz bu ek sütunlar içeren DataTable güncelleştirmeniz gerekecektir. Bu iki görevleri adımları 2 ve 3 üstesinden.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>2. adım: Dahil etmek için saklı yordam özelleştirme bir`JOIN`

Sunucu Gezgini, Northwind veritabanı s saklı yordamlar klasörüne araştırıp, gidip açma başlangıç `Employees_Select` saklı yordamı. Bu saklı yordam görmüyorsanız, saklı yordamlar klasörü sağ tıklatın ve Yenile seçeneğini seçin. Saklı yordamı güncelleştirin, böylece kullandığı bir `LEFT JOIN` s Yöneticisi ilk dönün ve Soyadı:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Güncelleştirdikten sonra `SELECT` dosya menüsüne giderek ve kaydetme seçerek değişiklikleri kaydetme deyimi `Employees_Select`. Alternatif olarak, araç çubuğunda Kaydet simgesine tıklayın veya Ctrl + S ulaştı. Değişiklikleri kaydettikten sonra sağ tıklayın `Employees_Select` saklı yordamı Server Explorer'da ve Çalıştır'ı seçin. Bu saklı yordamını çalıştırma ve sonuçları çıktı penceresinde Göster (bkz. Şekil 9).


[![Saklı yordamlar sonuçlar çıktı penceresinde görüntülenir](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Şekil 9**: saklı yordamları sonuçları, çıktı penceresinde görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>3. adım: DataTable s sütunları güncelleştiriliyor

Bu noktada, `Employees_Select` depolanan yordamı döndürür `ManagerFirstName` ve `ManagerLastName` değerleri, ancak `EmployeesDataTable` bu sütunları eksik. Bu eksik sütunları iki yoldan biriyle DataTable eklenebilir:

- **El ile** - veri kümesi tasarımcısında DataTable sağ tıklayın ve Sütun Ekle menüsünden'i seçin. Sütun adı ve özelliklerini uygun şekilde ayarlayın.
- **Otomatik olarak** -TableAdapter Yapılandırma Sihirbazı'nı DataTable s sütunları tarafından döndürülen alanları yansıtacak şekilde güncelleştirilir `SelectCommand` saklı yordamı. Geçici SQL deyimlerini kullanırken, sihirbaz aynı zamanda kaldıracak `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikler bu yana `SelectCommand` şimdi içeren bir `JOIN`. Ancak saklı yordamları kullanırken, bu komut özellikleri değişmeden kalır.

El ile ekleme DataTable sütunları önceki eğitimlerine denedikten [ana/Ayrıntılar DataList ile bir madde işaretli listesi, ana kayıtları kullanarak ayrıntı](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) ve [yüklenen dosyalar](../working-with-binary-files/uploading-files-cs.md), ve şunları yapacağız Bu işlem yeniden daha ayrıntılı bizim sonraki öğreticide bakın. Bu öğreticide, ancak TableAdapter Yapılandırma Sihirbazı'nı aracılığıyla otomatik yaklaşımı kullanın s olanak tanır.

Başlangıç üzerinde sağ tıklayarak `EmployeesTableAdapter` ve bağlam menüsünden Yapılandır seçme. Bu seçme, ekleme, güncelleştirme ve silme, kendi dönüş değerleri ve parametreleri (varsa) ile birlikte kullanılan saklı yordamları listeler TableAdapter Yapılandırma Sihirbazı açar. Şekil 10 Bu sihirbaz gösterilir. Burada görebiliriz `Employees_Select` depolanan yordamı şimdi döndürür `ManagerFirstName` ve `ManagerLastName` alanları.


[![Saklı yordam Employees_Select için güncelleştirilmiş sütun listesi Sihirbazı'nı gösterir](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Şekil 10**: Sihirbazı için güncelleştirilmiş sütun listesi gösterir `Employees_Select` saklı yordam ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Son'u tıklatarak Sihirbazı tamamlayın. Veri kümesi Tasarımcısı için döndürme bağlı `EmployeesDataTable` iki ek sütunları içerir: `ManagerFirstName` ve `ManagerLastName`.


[![İki yeni sütun EmployeesDataTable içerir](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Şekil 11**: `EmployeesDataTable` içeren iki yeni sütun ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Bu göstermek için güncelleştirilmiş `Employees_Select` saklı yordam etkindir ve ekleme, güncelleştirme ve TableAdapter yeteneklerini silme hala işlevsel, görüntülemek ve çalışanlar silmek kullanıcılara bir web sayfası oluşturmak s olanak tanır. Biz bu tür bir sayfayı oluşturmadan önce ancak önce yeni bir sınıf çalışanlarıyla çalışmak için iş mantığı katmanı oluşturmak ihtiyacımız `NorthwindWithSprocs` veri kümesi. Adım 4'te oluşturacağız bir `EmployeesBLLWithSprocs` sınıfı. Adım 5'te bu sınıf bir ASP.NET sayfasından kullanacağız.

## <a name="step-4-implementing-the-business-logic-layer"></a>4. adım: iş mantığı katmanı uygulama

Yeni bir sınıf dosyasını içinde oluşturmak `~/App_Code/BLL` adlı klasörü `EmployeesBLLWithSprocs.cs`. Bu sınıf varolan semantiğini taklit eder `EmployeesBLL` biri daha az yöntemleri sağlar ve kullandığı sınıfı, yalnızca bu yeni `NorthwindWithSprocs` veri kümesi (yerine `Northwind` veri kümesi). Aşağıdaki kodu ekleyin `EmployeesBLLWithSprocs` sınıfı.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` s sınıfı `Adapter` özelliği döndürür örneği `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Bu s sınıfı tarafından kullanılan `GetEmployees` ve `DeleteEmployee` yöntemleri. `GetEmployees` Yöntem çağrılarını `EmployeesTableAdapter` s karşılık gelen `GetEmployees` çağırır yöntemi `Employees_Select` saklı yordamı ve onun sonuçlarında dolduran bir `EmployeeDataTable`. `DeleteEmployee` Yöntemi benzer şekilde çağırır `EmployeesTableAdapter` s `Delete` çağırır yöntemi `Employees_Delete` saklı yordamı.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>5. adım: sunu katmanı verilerle çalışma

İle `EmployeesBLLWithSprocs` sınıfı tam, biz re ASP.NET sayfası üzerinden çalışan verilerle çalışmak için hazır. Açık `JOINs.aspx` sayfasındaki `AdvancedDAL` klasörü ve sürükleme GridView ayarı tasarımcıya araç kendi `ID` özelliğine `Employees`. Ardından, GridView s akıllı etiketten kılavuz adlı yeni bir ObjectDataSource denetimine bağlama `EmployeesDataSource`.

ObjectDataSource kullanmak için yapılandırma `EmployeesBLLWithSprocs` sınıfı ve seçin ve DELETE sekmelerinden emin olun `GetEmployees` ve `DeleteEmployee` yöntemleri, aşağı açılan listelerden seçilir. ObjectDataSource s yapılandırmasını tamamlamak için Son'u tıklatın.


[![ObjectDataSource EmployeesBLLWithSprocs sınıfını kullanmak için yapılandırma](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `EmployeesBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![ObjectDataSource kullanımı GetEmployees ve DeleteEmployee yöntemleri vardır](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Şekil 13**: ObjectDataSource kullanması `GetEmployees` ve `DeleteEmployee` yöntemleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio ekleyecek bir BoundField GridView her biri için `EmployeesDataTable` s sütun. Tüm bu BoundFields dışında kaldırmak `Title`, `LastName`, `FirstName`, `ManagerFirstName`, ve `ManagerLastName` ve yeniden adlandırma `HeaderText` Soyadı, ad, Yöneticisi s adını, son dört BoundFields özelliklerini ve Yöneticisi s Soyadı, sırasıyla.

Bu sayfadan çalışanları silebilirsiniz yapmalarına izin vermek için size iki şey yapmanız gerekir. İlk olarak, kendi akıllı etiket Enable Deleting seçeneğinden denetleyerek silme özellikleri sağlamak üzere GridView isteyin. İkinci olarak, ObjectDataSource s değiştirme `OldValuesParameterFormatString` özelliği değerinden ObjectDataSource Sihirbazı'nı ayarlayın (`original_{0}`) varsayılan değerine (`{0}`). Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Sayfa bir tarayıcıdan ziyaret ederek test edin. Şekil 14 gösterildiği gibi sayfa her çalışan (bir sahip oldukları varsayılarak) bilgilendirilmesine manager s adını listeler.


[![Employees_Select BİRLEŞİMDE depolanan yordamı döndürür Manager s adı](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Şekil 14**: `JOIN` içinde `Employees_Select` saklı yordam adı s Yöneticisi döndürür ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Yürütme işlemi culminates silme akışı başlar Sil düğmesini tıklatarak `Employees_Delete` saklı yordamı. Ancak, denenen `DELETE` saklı yordamı deyiminde bir yabancı anahtar kısıtlaması ihlali nedeniyle başarısız olur (bkz. Şekil 15). Özellikle, her çalışana sahip bir veya daha fazla kayıt `Orders` silme başarısız olmasına neden olan tablo.


[![Bir yabancı anahtar kısıtlaması ihlali karşılık gelen siparişler sonuçları sahip bir çalışan silme](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Şekil 15**: karşılık gelen siparişler sonuçları bir yabancı anahtar kısıtlaması ihlali sahip bir çalışan silme ([tam boyutlu görüntüyü görüntülemek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Bir çalışan olmasını izin vermek için silinmiş olabilir:

- Güncelleştirmeyi siler basamaklı yabancı anahtar kısıtlaması
- Kayıtlardan el ile silmeniz `Orders` silmek istediğinizden employee(s) için tablo veya
- Güncelleştirme `Employees_Delete` saklı yordamı ilgili kayıtlarını silmeniz `Orders` silmeden önce tablo `Employees` kaydı. Biz bu teknik ele [kullanarak var olan saklı yordamlar için yazılan veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Öğreticisi.

I bunu bir alıştırma olarak okuyucuya bırakın.

## <a name="summary"></a>Özet

İlişkisel veritabanları ile çalışırken, tablolar ilgili birden çok, verilerini çekmesini sorgular için yaygındır. Bağıntılı alt sorgular ve `JOIN` s sorguda ilgili tablolardan veri erişimi için iki farklı teknikleri sağlar. İçindeki en yaygın olarak yapılan önceki öğreticileri kullanın bağıntılı alt sorgulara TableAdapter otomatik oluşturması nedeniyle olamaz `INSERT`, `UPDATE`, ve `DELETE` ilgili sorgular için deyimleri `JOIN` s. Bu değerleri el ile geçici SQL deyimlerini kullanılırken sağlanabilir sırada TableAdapter Yapılandırma Sihirbazı tamamlandığında, tüm özelleştirmeler üzerine yazılır.

Neyse ki, saklı yordamlar kullanılarak oluşturulan TableAdapters geçici SQL deyimlerini kullanarak oluşturulanlar olarak aynı brittleness gelen karşılaşmaz. Bu nedenle, ana sorgusu kullanan bir TableAdapter oluşturmak için uygun bir `JOIN` saklı yordamları kullanırken. Bu öğreticide böyle bir TableAdapter oluşturma gördük. Kullanılarak başlatılan bir `JOIN`-daha az `SELECT` karşılık gelen INSERT, update ve delete saklı yordamlar, otomatik oluşturulan böylece TableAdapter s ana sorgu için sorgu. TableAdapter s ilk yapılandırma ile tam, biz engagement'ta `SelectCommand` saklı yordamı kullanmak için bir `JOIN` ve TableAdapter Yapılandırma Sihirbazı'nı güncelleştirmek için yeniden çalıştırılmıştır `EmployeesDataTable` s sütun.

Otomatik olarak güncelleştirilen TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırmak `EmployeesDataTable` tarafından döndürülen veri alanları yansıtacak şekilde sütunları `Employees_Select` saklı yordamı. Alternatif olarak, bu sütunları el ile DataTable tablosuna ekledik. Sonraki öğreticide DataTable tablosuna biz el ile ekleme sütunları inceleyeceksiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Hilton Geisenow, David Suru ve Teresa Murphy yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [sonraki](adding-additional-datatable-columns-cs.md)
