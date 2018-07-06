---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: JOIN. TableAdapter kullanacak biçimde güncelleştirme (C#) | Microsoft Docs
author: rick-anderson
description: Bir veritabanı ile çalışırken, birden çok tabloda dağıldığında istek verileri için yaygındır. İki farklı tablodan veri almak için'de ya da kullanabiliriz...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: c59ab13eb5e5e548055c6c8c9343a8d5c4cabe7d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841127"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>TableAdapter'ı JOIN kullanacak biçimde güncelleştirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) veya [PDF olarak indirin](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Bir veritabanı ile çalışırken, birden çok tabloda dağıldığında istek verileri için yaygındır. İki farklı tablodan veri almak için ilişkili bir alt sorgu ya da bir birleştirme işlemi kullanabiliriz. Bu öğreticide size bağıntılı alt sorgular ve JOIN söz dizimini kendi ana Sorguda birleştirme içeren bir TableAdapter oluşturma bakarak önce karşılaştırın.


## <a name="introduction"></a>Giriş

İlişkisel veritabanları ile biz ile çalışma ilgilendiğiniz veriler genellikle birden çok tabloda yayılır. Örneğin, ürün bilgileri görüntülerken, büyük olasılıkla her bir ürün s ilgili kategoriye ve tedarikçi s adlarını listelemek istiyoruz. `Products` Tablolu `CategoryID` ve `SupplierID` değerler, ancak gerçek kategorisi ve tedarikçi adlarını bulunduğunuz `Categories` ve `Suppliers` tabloları, sırasıyla.

Başka bir, ilgili tablosundan bilgileri almak için biz kullanabilir *alt sorgular bağıntılı* veya `JOIN` *s*. İç içe bir ilişkili alt sorgu olduğu `SELECT` dış sorgudaki sütunları başvuran sorgu. Örneğin, [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) iki bağıntılı alt sorgularda kullandık öğretici `ProductsTableAdapter` s ana sorguda her ürün için kategori ve tedarikçi adlarını döndürmek için. A `JOIN` iki farklı tablolardaki ilişkili satırları birleştirir SQL yapıdır. Kullandık bir `JOIN` içinde [SqlDataSource denetimi ile veri sorgulama](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) her ürün yanı sıra kategori bilgileri görüntülemek için öğretici.

Biz abstained kullanımından nedeni `JOIN` TableAdapter'ları ile s'dir karşılık gelen otomatik oluşturmak için TableAdapter s sihirbazdaki sınırlamaları nedeniyle `INSERT`, `UPDATE`, ve `DELETE` deyimleri. Daha açık belirtmek gerekirse TableAdapter s ana sorgu herhangi içeriyorsa `JOIN` s, TableAdapter otomatik-oluşturamıyor geçici SQL deyimleri ya da saklı yordamlar için kendi `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri.

Bu öğretici ki kısaca karşılaştırın ve karşıtlık alt sorgular bağıntılı ve `JOIN` içeren bir TableAdapter oluşturmak nasıl araştırma önce s `JOIN` kendi ana sorguda s.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Karşılaştırma ve çakışan bağıntılı alt sorgular ve`JOIN` s

Bu geri çağırma `ProductsTableAdapter` ilk öğreticide oluşturulan `Northwind` veri kümesi her ürün s karşılık gelen kategorisi ve üretici adı geri getirmek için bağıntılı alt sorgular kullanır. `ProductsTableAdapter` s ana sorgu aşağıda gösterilmektedir.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

İki alt sorgular - bağıntılı `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` ve `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -olan `SELECT` dış içinde ek bir sütun olarak ürün başına tek bir değer döndüren sorgular `SELECT` deyimi s sütun listesi.

Alternatif olarak, bir `JOIN` her ürün s Tedarikçi ve kategori adını döndürmek için kullanılabilir. Aşağıdaki sorgu, yukarıdaki bir aynı çıkışı döndürür ancak kullanan `JOIN` alt sorgular yerine s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A `JOIN` bazı ölçütleri temel alarak başka bir tablodaki kayıtları içeren bir tablodaki kayıtları birleştirir. Yukarıdaki sorguda, örneğin, `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` bildirir her birleştirmek için SQL Server ürün kategorisi kayıtla kaydetmek `CategoryID` değerle s ürün `CategoryID` değeri. Birleştirilmiş sonucu her ürün için karşılık gelen kategori alanlarını ile çalışacak şekilde sağlıyor (gibi `CategoryName`).

> [!NOTE]
> `JOIN` İlişkisel veritabanlarından alınan veriler sorgulanırken s yaygın olarak kullanılan. Yeni kullanıyorsanız `JOIN` söz dizimi veya kendi kullanımı biraz bilgilerinizi tazelemek mi ihtiyacı, d önerim [SQL birleştirme öğretici](http://www.w3schools.com/sql/sql_join.asp) adresindeki [W3 okullar](http://www.w3schools.com/). Aynı zamanda okuma değer olan [ `JOIN` Temelleri](https://msdn.microsoft.com/library/ms191517.aspx) ve [alt sorgu Temelleri](https://msdn.microsoft.com/library/ms189575.aspx) bölümlerini [SQL Books Online](https://msdn.microsoft.com/library/ms130214.aspx).


Bu yana `JOIN` s ve ilişkili alt sorgular hem de diğer tablolardan ilgili verileri almak için kullanılabilir, birçok geliştirici, kendi heads scratching ve kullanmak için hangi yaklaşımın merak ediyor bırakılır. Tüm SQL uzmanları miyim yaklaşık aynı şeyden söz konusu konuştum ve BT'nin eklenmemişse t açısından gerçekten önemli performance-wise olarak SQL Server yaklaşık aynı yürütme planlarını oluşturur. Ardından, öneriler, sizin ve ekibinizin en rahat kullanabileceğiniz teknik kullanmaktır. Bu, bu önerileri imparting sonra bu uzmanlar hemen kendi tercih ifade ettiğini gösteren değeri `JOIN` s üzerinden bağıntılı alt sorgular.

Yazılan veri kümeleri kullanarak veri erişim katmanını oluşturma sırasında araçları alt sorgular kullanırken daha iyi çalışır. Özellikle, TableAdapter s Sihirbazı'nı otomatik-karşılık gelen oluşturmaz `INSERT`, `UPDATE`, ve `DELETE` ana sorguda herhangi içeriyorsa deyimleri `JOIN` s, ancak otomatik oluşturur-bağıntılı olduğunda bu deyimler alt sorgular kullanılır.

Bu eksiklikleri keşfetmek için geçici bir türü belirtilmiş veri kümesi içinde oluşturma `~/App_Code/DAL` klasör. TableAdapter Yapılandırma Sihirbazı sırasında geçici SQL deyimlerini ve aşağıdakileri girin seçin `SELECT` sorgu (bkz. Şekil 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Birleşimler içeren bir ana sorgu girin](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Şekil 1**: bir ana sorgusu bu içerir girin `JOIN` s ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Varsayılan olarak, TableAdapter otomatik olarak oluşturacak `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorgusuna dayalı. Gelişmiş düğmesine tıklarsanız, bu özelliğin etkin olup olmadığını görebilirsiniz. Bu ayarı olmasına rağmen TableAdapter oluşturmak mümkün olmayacaktır `INSERT`, `UPDATE`, ve `DELETE` deyimleri ana sorgu içerdiğinden bir `JOIN`.


![Birleşimler içeren bir ana sorgu girin](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Şekil 2**: içeren temel bir sorgu girin `JOIN` s


Sihirbazı tamamlamak için Son'u tıklatın. Bu noktada, veri kümesi s Tasarımcısı sütunları ile bir DataTable ile tek bir TableAdapter döndürülen alanların her biri için dahil edilir `SELECT` sorgu s sütun listesi. Bu içerir `CategoryName` ve `SupplierName`Şekil 3'te gösterildiği gibi.


![DataTable döndürülen sütun listesinde her bir alan için bir sütun içerir.](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Şekil 3**: sütun listesinde her bir alan döndürdü DataTable bir sütun içerir.


DataTable uygun sütunları olsa da değerler için TableAdapter eksik kendi `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri. Bunu doğrulamak için Tasarımcısı'nda TableAdapter'ı tıklatın ve ardından Özellikler penceresine gidin. Orada göreceksiniz `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri (hiçbiri) ayarlanır.


[![(Hiçbiri) InsertCommand UpdateCommand ve DeleteCommand özellikleri ayarlanır](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Şekil 4**: `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri (hiçbiri) ayarlanır ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Bu eksiklikleri geçici olarak çözmek için size el ile SQL deyimlerini ve parametreler için sağlayabilir `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri Özellikler penceresinde aracılığıyla. Alternatif olarak, biz TableAdapter s ana sorguda yapılandırarak başlatabilir *değil* herhangi bir dosyayı eklemek `JOIN` s. Bu izin `INSERT`, `UPDATE`, ve `DELETE` bizim için otomatik olarak oluşturulmuş olmasını deyimleri. Sihirbazı tamamladıktan sonra daha sonra el ile TableAdapter s güncelleştiriyoruz `SelectCommand` içerecek şekilde Özellikler penceresinden `JOIN` söz dizimi.

Bu yaklaşım çalışırken, TableAdapter s ana sorgu için istediğiniz zaman geçici SQL sorguları kullanarak Sihirbazı otomatik olarak oluşturulan yeniden yapılandırılmış olduğunda bu çok kırılır `INSERT`, `UPDATE`, ve `DELETE` deyimleri yeniden. Biz Tableadapter'a sağ, bağlam menüsünden yapılandırma seçtiyseniz ve sihirbazı tekrar tamamlandı tüm daha sonra yapılan özelleştirmeler kaybolacak anlamına gelir.

Kırılganlığının otomatik üretilmiş TableAdapter s da bir artacağını `INSERT`, `UPDATE`, ve `DELETE` deyimleri olan Neyse ki, geçici SQL deyimlerini sınırlı. Saklı yordamlar, TableAdapter kullanıyorsa, özelleştirebileceğiniz `SelectCommand`, `InsertCommand`, `UpdateCommand`, veya `DeleteCommand` saklı yordamlar ve saklı yordamları olacağını endişe gerek kalmadan TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın değiştirilmiş.

Sonraki biz oluşturur bir TableAdapter, başlangıçta birkaç adım atlar herhangi bir ana sorgusu kullanan `JOIN` s karşılık gelen eklemek, güncelleştirmek ve delete saklı yordamlar, otomatik olarak oluşturulan olacaktır. Ardından güncelleştireceğiz `SelectCommand` şekilde kullanan bir `JOIN` ek sütunlar ilişkili tabloları döndürür. Son olarak, karşılık gelen bir iş mantığı katmanı sınıf oluşturun ve TableAdapter kullanarak bir ASP.NET web sayfasında gösterme.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>1. adım: TableAdapter kullanarak basitleştirilmiş bir ana sorgu oluşturma

Bu öğretici için bir TableAdapter ve kesin türü belirtilmiş DataTable için ekleyeceğiz `Employees` tablosundaki `NorthwindWithSprocs` veri kümesi. `Employees` Tablo içeren bir `ReportsTo` belirtilen alan `EmployeeID` çalışan s Yöneticisi. Örneğin, anne Ilgaz sahip çalışan bir `ReportTo` olan 5 değerini `EmployeeID` Steven Koç. Sonuç olarak, Anne Steven, olduğunu yöneticisine bildirir. Her bir çalışanın s Raporlama ile birlikte `ReportsTo` değeri, biz isteyebilir yöneticisine adını almak. Bu kullanarak gerçekleştirilebilir bir `JOIN`. Ancak kullanarak bir `JOIN` başlangıçta TableAdapter Oluşturma Sihirbazı'nı otomatik olarak ilgili ekleme oluşturma ışığının, güncelleştirme ve yetenekleri silme. Bu nedenle, ana sorguda herhangi içermediğinden bir TableAdapter oluşturarak başlayacağız `JOIN` s. Ardından, adım 2'de aracılığıyla yönetici s adını almak için ana sorgu depolanan yordamı güncelleştireceğiz bir `JOIN`.

Başlangıç açarak `NorthwindWithSprocs` kümesinde `~/App_Code/DAL` klasör. Tasarımcıda sağ tıklayın, bağlam menüsünden Ekle seçeneğini belirleyin ve TableAdapter menü öğesini seçin. Bu, TableAdapter Yapılandırma Sihirbazı başlatılır. Şekil 5 gösterilmektedir, sihirbazın yeni saklı yordamlar oluşturma ve İleri'ye sahip. Saklı yordamları TableAdapter s sihirbazından yeni oluşturma Yenileyici için başvurun [oluşturma yeni saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) öğretici.


[![Oluştur Yeni saklı yordamlar seçeneği seçin](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Şekil 5**: Select oluştur Yeni saklı yordamlar seçeneği ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Aşağıdaki `SELECT` TableAdapter s ana sorgu deyimi:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Bu sorgu tüm içermediğinden `JOIN` s, TableAdapter Sihirbazı otomatik olarak oluşturur saklı yordamların uygun `INSERT`, `UPDATE`, ve `DELETE` ana yürütme için bir saklı yordam yanı sıra deyimleri Sorgu.

Aşağıdaki adım TableAdapter s depolanan yordamları ad olanak sağlıyor. Adları `Employees_Select`, `Employees_Insert`, `Employees_Update`, ve `Employees_Delete`Şekil 6'da gösterildiği gibi.


[![TableAdapter s depolanan yordamları adı](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Şekil 6**: TableAdapter s saklı yordamlar adlandırın ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Son adım bize TableAdapter s yöntemleri adı ister. Kullanım `Fill` ve `GetEmployees` yöntem adları olarak. Ayrıca, doğrudan veritabanı (GenerateDBDirectMethods) onay kutusunu işaretli için güncelleştirmeleri göndermek için Create yöntemlerini bırakmayı unutmayın.


[![TableAdapter s yöntemleri dolgu adı ve GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Şekil 7**: TableAdapter s yöntemleri adında `Fill` ve `GetEmployees` ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Sihirbazı tamamladıktan sonra veritabanındaki saklı yordamları incelemek için bir dakikanızı ayırın. Dört yenilerini görmeniz gerekir: `Employees_Select`, `Employees_Insert`, `Employees_Update`, ve `Employees_Delete`. Ardından, inceleme `EmployeesDataTable` ve `EmployeesTableAdapter` oluşturduğunuz. DataTable ana sorgu tarafından döndürülen her alan için bir sütun içerir. TableAdapter öğesinde tıklayın ve sonra Özellikler penceresine gidin. Orada göreceksiniz `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri karşılık gelen saklı yordamları çağırmak için doğru şekilde yapılandırılır.


[![TableAdapter INSERT, Update, içerir ve özelliklerini silme](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Şekil 8**: TableAdapter içerir ekleme, güncelleştirme ve silme özelliklerini ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Ekleme, güncelleştirme ve delete saklı yordamlarını otomatik olarak oluşturulan ve `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` düzgün yapılandırılmış özellikleri, biz özelleştirmek hazır `SelectCommand` s saklı yordamı ek döndürmek için Her çalışan s Yöneticisi hakkında bilgi sağlar. Özellikle, güncelleştirilecek ihtiyacımız `Employees_Select` saklı yordamı kullanmak için bir `JOIN` ve s manager dönüş `FirstName` ve `LastName` değerleri. Saklı yordam güncelleştirildikten sonra Biz bu ek sütunlar içeren DataTable güncelleştirmeniz gerekir. Bu iki görevleri adım 2 ve 3 üstesinden.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>2. adım: Dahil etmek için saklı yordam özelleştirme bir`JOIN`

Sunucu Gezginine giderek, Northwind veritabanı s saklı yordamlar klasörüne detaya gitme ve açma başlangıç `Employees_Select` saklı yordamı. Bu saklı yordamı görmüyorsanız, saklı yordamlar klasörü sağ tıklatın ve Yenile'yi seçin. Saklı yordam güncelleştirin, böylece kullandığı bir `LEFT JOIN` s Yöneticisi ilk dönün ve Soyadı:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Güncelleştirdikten sonra `SELECT` deyimi, dosya menüsüne gidip Kaydet'i seçme değişiklikleri kaydetme `Employees_Select`. Alternatif olarak, araç çubuğunda Kaydet simgesine tıklayın ya da Ctrl + S basın. Yaptığınız değişiklikleri kaydettikten sonra sağ `Employees_Select` saklı yordamı sunucu Gezgini'nde ve Çalıştır'ı seçin. Bu saklı yordamı çalıştırın ve sonuçları çıkış penceresinde Göster (bkz. Şekil 9).


[![Saklı yordamları sonuçları çıkış penceresinde görüntülenir.](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Şekil 9**: saklı yordamları sonuçları çıkış penceresinde görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>3. adım: bir DataTable s sütunları güncelleştiriliyor

Bu noktada, `Employees_Select` depolanan yordam döndürür `ManagerFirstName` ve `ManagerLastName` değerleri, ancak `EmployeesDataTable` bu sütunlar eksik. Bu eksik sütunlar iki yoldan biriyle DataTable eklenebilir:

- **El ile** - veri kümesi tasarımcısında DataTable sağ tıklayın ve sütunu Ekle menüsünden'ı seçin. Sütun adı ve özelliklerini uygun şekilde ayarlayın.
- **Otomatik olarak** -TableAdapter Yapılandırma Sihirbazı'nı DataTable s sütunlar tarafından döndürülen alanlarla yansıtacak şekilde güncelleştirilir `SelectCommand` saklı yordamı. Geçici SQL deyimlerini kullanarak, sihirbaz da kaldırır `InsertCommand`, `UpdateCommand`, ve `DeleteCommand` özellikleri beri `SelectCommand` artık içeren bir `JOIN`. Ancak, saklı yordamlar kullanılırken, bu komut özellikleri değişmeden kalır.

DataTable sütunları el ile ekleme dahil olmak üzere önceki öğreticilerde, denedikten [ana/ayrıntı bir Ayrıntılar DataList'i ile bir madde işaretli liste, ana kayıtları kullanarak](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) ve [yüklenen dosyalar](../working-with-binary-files/uploading-files-cs.md), ve yapacağız Bu işlem yeniden daha ayrıntılı sonraki müşterilerimize öğreticide bakın. Bu öğreticide, Bununla birlikte, TableAdapter Yapılandırma Sihirbazı aracılığıyla otomatik yaklaşımı kullanmak s olanak tanır.

Başlangıç sağ tıklayarak `EmployeesTableAdapter` ve bağlam menüsünden Yapılandır'ı seçerek. Bu seçme, ekleme, güncelleştirme ve silme, kendi dönüş değerleri ve parametreler (varsa) ile birlikte kullanılan saklı yordamları listeler TableAdapter Yapılandırma Sihirbazı getirir. Şekil 10, bu sihirbaz gösterilir. Burada görebiliriz `Employees_Select` saklı yordamı artık döndürür `ManagerFirstName` ve `ManagerLastName` alanları.


[![Saklı yordamı Employees_Select için güncelleştirilmiş sütun listesi Sihirbazı gösterir](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Şekil 10**: Sihirbaz için güncelleştirilmiş sütun listesi gösterir `Employees_Select` saklı yordam ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Bitiş tıklayarak Sihirbazı tamamlayın. Veri kümesi Tasarımcısı için döndüren bağlı `EmployeesDataTable` iki ek sütunları içerir: `ManagerFirstName` ve `ManagerLastName`.


[![İki yeni sütun EmployeesDataTable içerir](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Şekil 11**: `EmployeesDataTable` içeren iki yeni sütun ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Bu göstermek için güncelleştirilmiş `Employees_Select` saklı yordam etkindir ve ekleme, güncelleştirme ve TableAdapter yeteneklerini Sil yine de işlevsel, s ve çalışanlar silme olanağı tanıyan bir web sayfası oluşturmak istiyorum. Böyle bir sayfa oluşturacağız önce ancak ilk çalışanlardan ile çalışmak için iş mantığı katmanı yeni bir sınıf oluşturmak ihtiyacımız `NorthwindWithSprocs` veri kümesi. Adım 4'te oluşturacağız bir `EmployeesBLLWithSprocs` sınıfı. Adım 5'te, bu sınıftan bir ASP.NET sayfasına kullanacağız.

## <a name="step-4-implementing-the-business-logic-layer"></a>4. adım: iş mantığı katmanı uygulama

Yeni bir sınıf dosyası oluşturma `~/App_Code/BLL` adlı klasöre `EmployeesBLLWithSprocs.cs`. Bu sınıf varolan semantiği taklit eden `EmployeesBLL` kullanır ve daha az yöntemler sağlar sınıfını, yalnızca bu yeni `NorthwindWithSprocs` veri kümesi (yerine `Northwind` veri kümesi). Aşağıdaki kodu ekleyin `EmployeesBLLWithSprocs` sınıfı.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` s sınıfı `Adapter` özelliği bir örneğini döndürür `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Bu s sınıfı tarafından kullanılan `GetEmployees` ve `DeleteEmployee` yöntemleri. `GetEmployees` Yöntem çağrılarını `EmployeesTableAdapter` s karşılık gelen `GetEmployees` çağıran, yöntemi `Employees_Select` saklı yordam ve kendi sonuçlarında dolduran bir `EmployeeDataTable`. `DeleteEmployee` Benzer şekilde yöntemini çağırır `EmployeesTableAdapter` s `Delete` çağıran yöntem `Employees_Delete` saklı yordamı.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>5. adım: sunu katmanı verileri ile çalışma

İle `EmployeesBLLWithSprocs` sınıfı tam, biz re ASP.NET sayfası aracılığıyla çalışan verilerle çalışmaya hazır. Açık `JOINs.aspx` sayfasını `AdvancedDAL` klasörü ve ayar Tasarımcısı araç kutusundan sürükleyip GridView kendi `ID` özelliğini `Employees`. Ardından, kılavuz GridView s akıllı etiketten adlı yeni bir ObjectDataSource denetimine bağlama `EmployeesDataSource`.

ObjectDataSource kullanmak için yapılandırma `EmployeesBLLWithSprocs` sınıfı ve seçin ve DELETE sekmelerinden emin `GetEmployees` ve `DeleteEmployee` yöntemleri, aşağı açılan listelerden seçilir. ObjectDataSource s yapılandırmasını tamamlamak için Son'u tıklatın.


[![ObjectDataSource EmployeesBLLWithSprocs sınıfını kullanmak için yapılandırma](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `EmployeesBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![ObjectDataSource kullanma GetEmployees ve DeleteEmployee yöntemleri vardır.](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Şekil 13**: ObjectDataSource kullanması `GetEmployees` ve `DeleteEmployee` yöntemleri ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio ekleyecek bir BoundField GridView'a her biri için `EmployeesDataTable` s sütunları. Tüm bu BoundFields dışında kaldırmak `Title`, `LastName`, `FirstName`, `ManagerFirstName`, ve `ManagerLastName` ve yeniden adlandırma `HeaderText` Soyadı, ad, ad Yöneticisi s, son dört BoundFields özelliklerini ve Yöneticisi s Soyadı, sırasıyla.

Çalışanların silmek iki işlem yapmanız gereken bu sayfadan izin vermek için. İlk olarak, akıllı etiketinde silmeyi etkinleştir seçeneğini işaretleyerek silme yetenekleri sağlamak üzere GridView isteyin. İkinci olarak, ObjectDataSource s değiştirme `OldValuesParameterFormatString` özelliği değerinden ObjectDataSource Sihirbazı'nı ayarlayın (`original_{0}`) varsayılan değerine (`{0}`). Bu değişiklikleri yaptıktan sonra GridView ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Bir tarayıcıdan ziyaret ederek sayfayı test edin. Şekil 14 gösterildiği gibi sayfa her çalışan ve (bir sahip oldukları varsayılarak) manager s adını listeler.


[![Birleştirme Employees_Select depolanan yordamı döndürür Yöneticisi adı](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Şekil 14**: `JOIN` içinde `Employees_Select` saklı yordam adı s Manager döndürür ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


İçinde yürütülmesini culminates silme workflow başlar Sil düğmesine tıklanarak `Employees_Delete` saklı yordamı. Ancak, denenen `DELETE` saklı yordamı deyiminde bir yabancı anahtar kısıtlaması ihlali nedeniyle başarısız olur (bkz. Şekil 15). Özellikle, her çalışana sahip bir veya daha fazla kayıt `Orders` silme başarısız olmasına neden olan tablo.


[![Bir yabancı anahtar kısıtlaması ihlali ile ilgili Siparişler sonuçları olan bir çalışanın siliniyor](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Şekil 15**: yabancı anahtar kısıtlaması ihlali ile ilgili Siparişler sonuçları olan bir çalışanın siliniyor ([tam boyutlu görüntüyü görmek için tıklatın](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Bir çalışan olmasını izin vermek için silinmiş olabilir:

- Güncelleştirmeyi siler basamaklı yabancı anahtar kısıtlaması
- Kayıtlardan el ile silmeniz `Orders` tablosunu silmek istediğinizden employee(s) veya
- Güncelleştirme `Employees_Delete` saklı yordamı ilgili kayıtların silmelisiniz `Orders` silmeden önce tablo `Employees` kaydı. Bu teknikte ele almıştık [kullanarak mevcut saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) öğretici.

Ben bunu bir alıştırma olarak için okuyucu bırakın.

## <a name="summary"></a>Özet

İlişkisel veritabanları ile çalışırken, tabloları ilgili birden çok, verilerini çekmesini sorgular için yaygındır. Bağıntılı alt sorgular ve `JOIN` s sorguda ilgili tablolardan veri erişimi için iki farklı teknikleri sağlar. İçinde en sık yapılan önceki bu öğreticileri kullanın bağıntılı alt sorgularda, TableAdapter otomatik olarak oluşturmak için kullanılamaz `INSERT`, `UPDATE`, ve `DELETE` deyimleri içeren sorgular için `JOIN` s. Bu değerler, el ile geçici SQL deyimleri kullanılırken sağlanabilir ancak TableAdapter Yapılandırma Sihirbazı tamamlandığında, tüm özelleştirmeler üzerine yazılır.

Neyse ki, saklı yordamlar kullanılarak oluşturulan TableAdapter'ları ile aynı brittleness geçici SQL deyimlerini kullanarak oluşturulanlar olarak gelen karşılaşmaz. Bu nedenle, ana sorgu kullanan bir TableAdapter oluşturmak için uygun bir `JOIN` saklı yordamlar kullanılırken. Bu öğreticide bu tür bir TableAdapter oluşturma gördük. Kullanılarak başlatılan bir `JOIN`-daha az `SELECT` karşılık gelen bir INSERT, update ve delete saklı yordamlarını otomatik olarak oluşturulmuş olur böylece TableAdapter s ana sorgu için sorgu. TableAdapter s ile ilk yapılandırmayı tam, biz Genişletilmiş `SelectCommand` saklı yordamı kullanmak için bir `JOIN` ve TableAdapter Yapılandırma Sihirbazı'nı güncelleştirmek için yeniden çalıştırıldı `EmployeesDataTable` s sütunları.

Otomatik olarak güncelleştirilen TableAdapter yapılandırma sihirbazını yeniden çalıştırarak `EmployeesDataTable` tarafından döndürülen veri alanlarını yansıtacak şekilde sütunları `Employees_Select` saklı yordamı. Alternatif olarak, bu sütunların el ile DataTable tablosuna ekledik. Sonraki öğreticide DataTable tablosuna biz el ile ekleme sütunları inceleyeceksiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Hilton Geisenow, David Suru ve Teresa Murphy yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [İleri](adding-additional-datatable-columns-cs.md)
