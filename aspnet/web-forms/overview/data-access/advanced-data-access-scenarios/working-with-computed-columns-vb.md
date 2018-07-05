---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Hesaplanan sütunlar (VB) ile çalışma | Microsoft Docs
author: rick-anderson
description: Bir veritabanı tablosu oluştururken, Microsoft SQL Server, değeri bir ifade tarafından hesaplanan bir hesaplanan sütun tanımlamanıza olanak tanır, genellikle referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 5837baccb9e378546d26703c99dcddd144ba80de
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386779"
---
<a name="working-with-computed-columns-vb"></a>Hesaplanan sütunlar (VB) ile çalışma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) veya [PDF olarak indirin](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Bir veritabanı tablosu oluştururken, Microsoft SQL Server genellikle diğer değerleri de aynı veritabanı kaydına başvuran bir ifade değeri hesaplandığı hesaplanmış bir sütun tanımlamanızı sağlar. Bu tür, salt okunur, TableAdapter'ları ile çalışırken özel konular gerektiren veritabanı değerlerdir. Bu öğreticide, biz tarafından hesaplanan sütunlar teşkil zorlayan konusunda bilgi edinin.


## <a name="introduction"></a>Giriş

Microsoft SQL Server sağlar  *[hesaplanan sütunlar](https://msdn.microsoft.com/library/ms191250.aspx)*, olan sütun değerleri, genellikle aynı tablonun diğer sütunlardan değerleri başvuran bir ifadeden hesaplanır. Örneğin, bir süre veri modeli izleme adlı bir tablo olabilir `ServiceLog` dahil olmak üzere sütunlarla `ServicePerformed`, `EmployeeID`, `Rate`, ve `Duration`, diğerlerinin yanı sıra. While tutarın hizmet başına öğe (süresine çarpılan hızı anda) hesaplanacağını bir web sayfası veya diğer programlama arabirimi, bir sütun eklemek için kullanışlı olabilir `ServiceLog` adlı tablo `AmountDue` Bu rapor bilgiler. Bu sütun, normal bir sütun olarak oluşturulabilir, ancak dilediğiniz zaman güncelleştirilmesi gerekir `Rate` veya `Duration` sütun değerleri değiştirildi. Daha iyi bir yaklaşım sağlamak olacaktır `AmountDue` sütun ifadesi kullanarak hesaplanmış bir sütun `Rate * Duration`. Bunun yapılması SQL Server'ın otomatik olarak hesaplamak neden `AmountDue` sorguda başvurulan her sütun değeri.

Bir hesaplanan sütun s değeri bir ifade tarafından belirlenir olduğundan, bu tür sütunları salt okunur ve bu nedenle değerleri onlara içinde atanmış olamaz `INSERT` veya `UPDATE` deyimleri. Hesaplanmış sütunlar geçici SQL deyimleri kullanan bir TableAdapter için ana sorguda bir parçasıdır, ancak bunlar otomatik olarak otomatik olarak oluşturulan içinde eklenir `INSERT` ve `UPDATE` deyimleri. Sonuç olarak, TableAdapter s `INSERT` ve `UPDATE` sorgular ve `InsertCommand` ve `UpdateCommand` özellikleri, hesaplanan sütunlar başvuruları kaldırmak için güncelleştirilmesi gerekir.

Kullanmanın bir güçlük hesaplanmış sütunlar geçici SQL deyimleri kullanan bir TableAdapter ile TableAdapter s olan `INSERT` ve `UPDATE` sorgular otomatik olarak TableAdapter Yapılandırma Sihirbazı tamamlandıktan dilediğiniz zaman yeniden oluşturuldu. Bu nedenle, hesaplanan sütunlar el ile kaldırılmış `INSERT` ve `UPDATE` sorguları Sihirbazı yeniden çalıştırma ise görünecektir. Saklı yordamlar kullanma TableAdapters t ki rağmen bu brittleness olumsuz, biz 3. adımda ele alınacaktır, kendi quirks sahiptirler.

Bu öğreticide hesaplanmış bir sütun ekleyeceğiz `Suppliers` Northwind veritabanındaki tablo ve bu tablo, hesaplanan sütun ile çalışmak için karşılık gelen bir TableAdapter'ı oluşturun. Biz, saklı yordamlar, geçici SQL deyimleri yerine kullanabilir, böylece TableAdapter Yapılandırma Sihirbazı kullanıldığında bizim özelleştirmeleri dahilse t kayıp bizim TableAdapter sahip olur.

Let s başlayın!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>1. adım: hesaplanan bir sütun ekleme`Suppliers`tablo

Bir kendimize eklemeniz gerekmez, hesaplanan sütunlar Northwind veritabanı yok. Bu öğretici için hesaplanan bir sütun ekleyin s denetlemesine izin vermek için `Suppliers` adlı bir tablo `FullContactName` döndüren s ilgili kişi adı, unvanı ve çalıştıkları için şirket şu biçimde: `ContactName` (`ContactTitle`, `CompanyName`). Başka bir hesaplanan sütun raporlarda sağlayıcıları hakkında bilgi görüntülerken kullanılabilir.

Başlangıç açarak `Suppliers` tablo tanımı sağ tıklayarak `Suppliers` sunucu Gezgini'nde tablo ve bağlam menüsünden açın tablo tanımı seçme. Sağlarlar olup bu tabloyu ve kendi veri türü gibi özelliklerini sütunlarını görüntüler `NULL` s ve benzeri. Hesaplanmış bir sütun eklemek için tablo tanımında sütun adını yazarak başlatın. Ardından, hesaplanan sütun belirtimi bölümünde sütun Özellikler penceresinde (Formül) metin kutusu içine ifadesini girin (bkz. Şekil 1). Hesaplanmış bir sütun adı `FullContactName` ve şu ifadeyi kullanın:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

SQL dizeleri birleştirilebilir Not kullanarak `+` işleci. `CASE` Deyimi, koşullu geleneksel bir programlama dili gibi kullanılabilir. Yukarıdaki ifadede `CASE` deyimi olarak okunabilir: varsa `ContactTitle` değil `NULL` sonra çıktı `ContactTitle` virgül, aksi takdirde birleştirilmiş değer Yayma hiçbir şey. Kullanışlılığını hakkında daha fazla bilgi `CASE` deyimi bkz [SQL güç `CASE` deyimleri](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Kullanmak yerine bir `CASE` ifadesini Buraya alternatif olarak kullandığımız `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) döndürür *checkExpression* NULL olmayan ise, aksi halde döndürür *replacementValue*. While ya da `ISNULL` veya `CASE` çalışır Bu örnekte, daha karmaşık senaryolar da vardır burada esnekliğini `CASE` deyimi kullanılamaz eşleşen tarafından `ISNULL`.


Bu hesaplanan sütunu ekledikten sonra ekranınız, Şekil 1'de ekran gibi görünmelidir.


[![Üreticiler tablosuna FullContactName adlı hesaplanan sütun ekleme](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Şekil 1**: bir hesaplanmış sütunun adı Ekle `FullContactName` için `Suppliers` tablo ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image3.png))


Hesaplanan sütunu adlandırma ve ifadesini girdikten sonra değişiklikleri tabloya araç Kaydet simgesine tıklayarak, Ctrl + S ulaşmaktan tarafından veya dosya menüsüne gidip Kaydet'i seçme kaydetmek `Suppliers`.

Tablo kaydediliyor yeni eklenen sütun dahil olmak üzere Sunucu Gezgini yenilemelisiniz `Suppliers` s tablosundaki sütun listesi. Ayrıca, (Formül) metin kutusuna girdiğiniz ifade otomatik olarak gereksiz boşluk kaldırır, çevreleyen köşeli ayraçlar içeren sütun adları eşdeğer bir ifade için ayarlar (`[]`), daha açık olarak göstermek için parantez içerir işlem sırası:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Microsoft SQL Server'da hesaplanan sütunlar üzerinde daha fazla bilgi için [teknik belgeler](https://msdn.microsoft.com/library/ms191250.aspx). Ayrıca kullanıma [nasıl yapılır: hesaplanan sütunları belirtme](https://msdn.microsoft.com/library/ms188300.aspx) hesaplanan sütunlar oluşturma adım adım kılavuz.

> [!NOTE]
> Varsayılan olarak, hesaplanan sütunlar tabloya fiziksel olarak depolanmaz, ancak bunun yerine bir sorguda başvurulan her zaman yeniden hesaplanır. Kalıcıdır onay kutusunu işaretleyerek, ancak SQL Server'ın fiziksel olarak hesaplanan sütun tabloda depolamak için bildirebilirsiniz. Bunun yapılması, hesaplanan sütun değeri kullandığınızdan sorguların performansını geliştirebilir hesaplanan sütunu oluşturulacak bir dizin sağlar, `WHERE` yan tümceleri. Bkz: [hesaplanan sütunlar oluşturma dizinlerde](https://msdn.microsoft.com/library/ms189292.aspx) daha fazla bilgi için.


## <a name="step-2-viewing-the-computed-column-s-values"></a>2. adım: hesaplanan sütun s değerlerini görüntüleme

Let s'ı biz iş veri erişim katmanı başlamadan önce görüntülemek için bir dakikanızı ayırın `FullContactName` değerleri. Sunucu Gezgini'nden sağ `Suppliers` tablo adı ve bağlam menüsünden Yeni bir sorgu seçin. Bu sorguya dahil hangi tabloları seçmek için bize ister bir sorgu penceresi çıkarır. Ekleme `Suppliers` tablo ve Kapat'a tıklayın. Ardından, kontrol `CompanyName`, `ContactName`, `ContactTitle`, ve `FullContactName` Suppliers tablosundaki sütun. Son olarak, sorguyu çalıştırıp sonuçlarını görüntülemek için araç çubuğunda kırmızı ünlem simgesine tıklayın.

Şekil 2 gösterildiği gibi sonuçlarında `FullContactName`, hangi listeleri `CompanyName`, `ContactName`, ve `ContactTitle` biçimini kullanarak sütunları `ContactName` (`ContactTitle`, `CompanyName`).


[![FullContactName biçimi ContactName (ContactTitle, CompanyName) kullanır.](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Şekil 2**: `FullContactName` biçimini kullanan `ContactName` (`ContactTitle`, `CompanyName`) ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>3. adım: Ekleme`SuppliersTableAdapter`için veri erişim katmanı

Uygulamamızı tedarikçi bilgiler ile çalışmak için önce bir TableAdapter ve DataTable içinde bizim DAL oluşturmak ihtiyacımız var. İdeal olarak, bu önceki öğreticilerde incelenirken basit adımları kullanarak gerçekleştirilmesi. Bununla birlikte, hesaplanan sütunlar ile çalışma tartışma kadar geniştir birkaç kırışıklıkların tanıtır.

Geçici SQL deyimleri kullanan bir TableAdapter'ı kullanıyorsanız, TableAdapter Yapılandırma Sihirbazı aracılığıyla TableAdapter s ana sorguda yalnızca hesaplanmış bir sütun ekleyebilirsiniz. Bu, ancak otomatik-oluşturur `INSERT` ve `UPDATE` hesaplanan sütunu içeren ifadeler. Aşağıdaki yöntemlerden birini çalıştırmak denerseniz bir `SqlException` sütunu iletisiyle *ColumnName* hesaplanan bir sütun veya birleşim işlecinin sonucunu oluşturulur olduğu için değiştirilemiyor. Sırada `INSERT` ve `UPDATE` deyimi TableAdapter s el ile da ayarlanabilir `InsertCommand` ve `UpdateCommand` özellikleri, bu özelleştirmeler kaybolacak her TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın.

Kırılganlığının da artacağını geçici SQL deyimlerini kullanarak TableAdapter'ları nedeniyle, saklı yordamlar hesaplanan sütunlar ile çalışırken kullandığımızı önerilir. Mevcut saklı yordamlara kullanıyorsanız, yalnızca TableAdapter anlatıldığı gibi yapılandırın [kullanarak mevcut saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) öğretici. Saklı yordamları oluşturduğunuz TableAdapter Sihirbazı varsa, ancak başlangıçta ana sorgudan hiçbir hesaplanmış sütun atlamak önemlidir. Ana sorguda bir hesaplanan sütun eklerseniz, TableAdapter Yapılandırma Sihirbazı'nı, tamamlandıktan sonra karşılık gelen saklı yordamları oluşturulamıyor bilgilendirecektir. Kısacası, hesaplanan bir sütun ücretsiz ana sorgu kullanarak TableAdapter'ı ilk kez yapılandırdıktan ve ardından ilgili saklı yordam ve TableAdapter s el ile güncelleştirmek ihtiyacımız `SelectCommand` hesaplanmış bir sütun eklemek için. Bu yaklaşım kullanılanla benzer [TableAdapter kullanacak biçimde güncelleştirme](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* öğretici.

Bu öğretici için yeni bir TableAdapter ekleyin ve bu bizim için otomatik olarak saklı yordamları oluştur s olanak tanır. Sonuç olarak, başlangıçta atlamak ihtiyacımız `FullContactName` ana sorgudan hesaplanmış bir sütun.

Başlangıç açarak `NorthwindWithSprocs` kümesinde `~/App_Code/DAL` klasör. Tasarımcıda sağ tıklayın ve bağlam menüsünden Yeni bir TableAdapter eklemek seçin. Bu, TableAdapter Yapılandırma Sihirbazı başlatılır. Verileri sorgulamak için bir veritabanı belirtin (`NORTHWNDConnectionString` gelen `Web.config`) ve İleri'ye tıklayın. Biz sorgulama veya değiştirmek için saklı yordamlarda henüz oluşturmadıysanız bu yana `Suppliers` tablo, yeni saklı yordamlar seçeneği sihirbaz bizim için oluşturma ve İleri'ye tıklayın, Oluştur'u seçin.


[![Oluştur Yeni saklı yordamlar seçeneği seçin](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Şekil 3**: oluştur Yeni saklı yordamlar seçeneği seçin ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image9.png))


Sonraki adım, ABD için ana sorguda ister. Döndüren aşağıdaki sorguyu girin `SupplierID`, `CompanyName`, `ContactName`, ve `ContactTitle` sütunları her üretici için. Bu sorgu kullanılamıyor.%n%nÇözüm hesaplanan sütunu atlar Not (`FullContactName`); Adım 4'te bu sütun içerecek şekilde karşılık gelen saklı yordamı güncelleştireceğiz.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Ana sorguda girme ve İleri'yi tıklatmadan sonra sihirbaz bunu oluşturacak dört saklı yordamları ad olanak sağlıyor. Bu saklı yordamlar ad `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, ve `Suppliers_Delete`Şekil 4'te gösterildiği gibi.


[![Otomatik olarak oluşturulan saklı yordamları adlarını özelleştirin](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Şekil 4**: Auto-Generated saklı yordamlar adlarını özelleştirin ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image12.png))


Sonraki sihirbaz adımı TableAdapter s yöntemleri adını ve erişim ve güncelleştirme verileri için kullanılan desenleri belirtmek olanak sağlıyor. Tüm üç işaretli bırakın, ancak yeniden adlandırmak `GetData` yönteme `GetSuppliers`. Sihirbazı tamamlamak için Son'u tıklatın.


[![GetData metodu GetSuppliers için yeniden adlandırın.](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Şekil 5**: yeniden adlandırma `GetData` yönteme `GetSuppliers` ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image15.png))


Bitiş tıklandıktan sonra sihirbaz dört saklı yordamları oluştur ve karşılık gelen DataTable ve TableAdapter türü belirtilmiş veri kümesi ekleyin.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>4. adım: hesaplanan sütun TableAdapter s ana sorguda dahil.

Artık TableAdapter güncelleştirmek ihtiyacımız ve DataTable dahil etmek için adım 3'te oluşturulan `FullContactName` hesaplanan sütun. Bu iki adımdan oluşur:

1. Güncelleştirme `Suppliers_Select` saklı yordamı döndürülecek `FullContactName` hesaplanan sütun ve
2. DataTable karşılık gelen içerecek şekilde güncelleştirilmesi `FullContactName` sütun.

Sunucu Gezginine giderek ve saklı yordamlar klasörüne detaya başlatın. Açık `Suppliers_Select` saklı yordam ve güncelleştirme `SELECT` eklemek için sorgu `FullContactName` hesaplanan sütun:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Saklı yordam için araç çubuğunda Kaydet simgesine tıklayarak, Ctrl + S ulaşmaktan veya kaydetme seçerek değişiklikleri kaydetmek `Suppliers_Select` Dosya menüsünden seçeneği.

Sağ tıklayın, ardından, veri kümesini tasarımcıya dönün `SuppliersTableAdapter`ve bağlam menüsünden Yapılandır'ı seçin. Unutmayın `Suppliers_Select` artık sütununu içeren `FullContactName` sütunu, veri sütunları koleksiyonu.


[![DataTable s sütunlarını güncelleştirebilmek için TableAdapter s Yapılandırma Sihirbazı'nı çalıştırın](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Şekil 6**: TableAdapter s DataTable s sütunları güncelleştirmek için Yapılandırma Sihirbazı'nı çalıştırın ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image18.png))


Sihirbazı tamamlamak için Son'u tıklatın. Bu otomatik olarak karşılık gelen bir sütun ekler `SuppliersDataTable`. TableAdapter Sihirbazı'nı algılandığı akıllı `FullContactName` sütunu hesaplanan sütun olduğundan ve bu nedenle salt okunur. Sonuç olarak, bu sütunu s ayarlar `ReadOnly` özelliğini `true`. Bunu doğrulamak için sütunu seçin `SuppliersDataTable` özellikleri penceresine gidin (bkz. Şekil 7). Unutmayın `FullContactName` sütunu s `DataType` ve `MaxLength` özellikleri de uygun şekilde ayarlanır.


[![FullContactName sütun salt okunur olarak işaretlenmiş.](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Şekil 7**: `FullContactName` sütun salt okunur olarak işaretlenmiş ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>5. adım: Ekleme bir`GetSupplierBySupplierID`TableAdapter yöntemi

Bu öğretici için tedarikçileri güncelleştirilebilir bir kılavuzda görüntüleyen bir ASP.NET sayfasına oluşturacağız. İçinde öğreticiler tek bir iş mantığı katmanı kaydından DAL olarak özelliklerini güncelleştirmek ve ardından güncelleştirilmiş DataTable gönderme kesin türü belirtilmiş DataTable, belirli kaydından değişiklikleri yaymak için DAL için geri alarak güncelleştirdik Veritabanı. -DAL güncelleştiriliyor kayıt alınıyor - ilk bu adımı tamamlamak için ilk olarak eklemek ihtiyacımız bir `GetSupplierBySupplierID(supplierID)` DAL için yöntemi.

Sağ `SuppliersTableAdapter` veri kümesi tasarımında ve bağlam menüsünden Sorgu Ekle seçeneğini seçin. Adım 3'te yaptığımız gibi yeni bir saklı yordamı bizim için yeni saklı yordam seçeneğini seçerek Oluştur Sihirbazı'nı sağlar (geri ekran görüntüsü bu sihirbazı adımı için Şekil 3'e bakın). Bu yöntem, birden çok sütun içeren bir kayıt döndürür olduğundan, bir SQL sorgusuna satır döndüren SELECT kullanın ve İleri'ye istiyoruz gösterir.


[![Satır seçeneği döndüren Seç](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Şekil 8**: satır seçeneği döndüren Seç ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image24.png))


Sonraki adım, bizim için bu yöntemi kullanmak bir sorgu için ister. Ana sorguda ancak belirli bir üretici için aynı veri alanları döndüren aşağıdaki girin.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Sonraki ekranda bize otomatik olarak oluşturulan bir saklı yordam adı ister. Bu saklı yordam adı `Suppliers_SelectBySupplierID` ve İleri'ye tıklayın.


[![Saklı yordam Suppliers_SelectBySupplierID adı](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Şekil 9**: saklı yordam adı `Suppliers_SelectBySupplierID` ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image27.png))


Son olarak, sihirbaz yönergelerini bizim için veri erişim desenleri ve TableAdapter için kullanılacak yöntem adları. Hem işaretli bırakın, ancak yeniden adlandırmak `FillBy` ve `GetDataBy` yöntemlere `FillBySupplierID` ve `GetSupplierBySupplierID`sırasıyla.


[![TableAdapter yöntemleri FillBySupplierID adı ve GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Şekil 10**: TableAdapter metotları ad `FillBySupplierID` ve `GetSupplierBySupplierID` ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image30.png))


Sihirbazı tamamlamak için Son'u tıklatın.

## <a name="step-6-creating-the-business-logic-layer"></a>6. adım: iş mantığı katmanı oluşturma

Biz 1. adımda oluşturduğunuz hesaplanan sütunu kullanan bir ASP.NET sayfasını oluşturmadan önce ilk BLL karşılık gelen yöntemlere eklemek ihtiyacımız var. Adım 7'de oluşturacağız, ASP.NET sayfamızı görüntülemek ve düzenlemek Üreticiler kullanıcılara izin verir. Bu nedenle, en azından tüm üreticiler ve başka bir belirli bir üretici güncelleştirilecek almak için bir yöntem sağlamak için sunduğumuz BLL ihtiyacımız var.

Adlı yeni bir sınıf dosyası oluşturma `SuppliersBLLWithSprocs` içinde `~/App_Code/BLL` klasörü ve aşağıdaki kodu ekleyin:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Gibi diğer BLL sınıfları `SuppliersBLLWithSprocs` sahip bir `Protected` `Adapter` örneğini döndüren özellik `SuppliersTableAdapter` sınıfı iki birlikte `Public` yöntemleri: `GetSuppliers` ve `UpdateSupplier`. `GetSuppliers` Yöntemi çağırır ve döndürür `SuppliersDataTable` karşılık gelen tarafından döndürülen `GetSupplier` veri erişim katmanında yöntemi. `UpdateSupplier` Yöntemi DAL s çağrısı aracılığıyla güncelleştirilmesini belirli tedarikçi ilgili bilgileri alır `GetSupplierBySupplierID(supplierID)` yöntemi. Ardından güncelleştirmeleri `CategoryName`, `ContactName`, ve `ContactTitle` özellikleri ve veri erişim katmanı s çağırarak bu değişiklikleri veritabanına kaydeder `Update` değiştirilmiş geçirerek yöntemini `SuppliersRow` nesne.

> [!NOTE]
> Dışında `SupplierID` ve `CompanyName`, üreticiler tablodaki tüm sütunların izin `NULL` değerleri. Bu nedenle, geçilen `contactName` veya `contactTitle` parametreler `Nothing` karşılık gelen ayarlamak ihtiyacımız `ContactName` ve `ContactTitle` özelliklerine bir `NULL` veritabanı değerini kullanarak `SetContactNameNull` ve `SetContactTitleNull`yöntemleri, sırasıyla.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>7. adım: hesaplanan sütun sunu katmanı ile çalışma

Eklenen hesaplanan sütunu ile `Suppliers` güncelleştirilir ve DAL ve BLL buna göre çalışan bir ASP.NET sayfasına oluşturmaya hazırız `FullContactName` hesaplanan sütun. Başlangıç açarak `ComputedColumns.aspx` sayfasını `AdvancedDAL` klasörü ve GridView tasarımcı araç kutusundan sürükleyin. GridView s ayarlamak `ID` özelliğini `Suppliers` ve isteğe bağlı olarak, akıllı etiketten adlı yeni bir ObjectDataSource bağlama `SuppliersDataSource`. ObjectDataSource kullanmak için yapılandırma `SuppliersBLLWithSprocs` ekledik sınıfı adım 6'da geri ve İleri'ye tıklayın.


[![ObjectDataSource SuppliersBLLWithSprocs sınıfını kullanmak için yapılandırma](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image33.png))


Tanımlanan yalnızca iki farklı yöntemle `SuppliersBLLWithSprocs` sınıfı: `GetSuppliers` ve `UpdateSupplier`. Bu iki yöntem seçme içinde belirtilir ve sırasıyla sekmeler, güncelleştirme ve ObjectDataSource yapılandırmasını tamamlamak için Son'u tıklatın emin olun.

Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio döndürülen veri alanların her biri için bir BoundField ekleyeceksiniz. Kaldırma `SupplierID` BoundField değiştirip `HeaderText` özelliklerini `CompanyName`, `ContactName`, `ContactTitle`, ve `FullContactName` BoundFields şirket, ilgili kişi adı, başlık ve tam kişi adı, sırasıyla. Akıllı etiket GridView s yerleşik düzenleme özellikleri etkinleştirmek için düzenlemeyi etkinleştir onay kutusunu işaretleyin.

GridView'a BoundFields eklemenin yanı sıra veri kaynağı Sihirbazı'nın tamamlanma ayrıca ObjectDataSource s ayarlamak Visual Studio neden `OldValuesParameterFormatString` özgün özelliğini\_{0}. Bu geri, varsayılan değere geri {0} .

GridView ve ObjectDataSource bu düzenlemeler yaptıktan sonra bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Ardından, bir tarayıcı aracılığıyla bu sayfayı ziyaret edin. Şekil 12 gösterildiği gibi her tedarikçi içeren kılavuzda listelenen `FullContactName` sütun değeri yalnızca diğer üç sütun birleşimi olan, biçimlendirilmiş olarak `ContactName` (`ContactTitle`, `CompanyName`).


[![Her tedarikçi kılavuz listelenir.](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Şekil 12**: her tedarikçi kılavuzunda listelenen ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image36.png))


Belirli bir üretici geri göndermeye neden olur ve içinde satır tarafından işlenen Düzenle düğmesine tıklayarak düzenleme, arabirim (bkz. Şekil 13). İlk üç sütun arabirimini düzenleme, varsayılan olarak işleme - TextBox denetimi `Text` özelliğini veri alanına değerine ayarlayın. `FullContactName` Sütun, ancak metin olarak kalır. BoundFields veri kaynağı Yapılandırma Sihirbazı'nın tamamlanma GridView eklendiğinde `FullContactName` BoundField s `ReadOnly` özelliğinin ayarlandığı `True` çünkü ilgili `FullContactName` sütununda `SuppliersDataTable` sahip, `ReadOnly` özelliğini `True`. Adım 4'te belirtildiği gibi `FullContactName` s `ReadOnly` özelliğinin ayarlandığı `True` TableAdapter sütunu hesaplanan bir sütun olduğunu algıladığından.


[![FullContactName sütundur düzenlenemez](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Şekil 13**: `FullContactName` sütundur düzenlenemez ([tam boyutlu görüntüyü görmek için tıklatın](working-with-computed-columns-vb/_static/image39.png))


Devam edin ve bir veya daha fazla düzenlenebilir sütunların değerini güncelleştirin ve Güncelleştir'e tıklayın. Not nasıl `FullContactName` s değer değişikliği yansıtacak şekilde otomatik olarak güncelleştirilir.

> [!NOTE]
> Düzenlenebilir alanları için şu anda kullandığı BoundFields arabirimini düzenleme varsayılan olarak sonuçlanan GridView. Bu yana `CompanyName` alan gereklidir, bir RequiredFieldValidator içeren bir TemplateField dönüştürülmesi. Ben bunu bir alıştırma olarak için ilginizi okuyucu bırakın. Başvurun [arabirimleri ekleme ve düzenleme için doğrulama denetimleri ekleme](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) öğretici ilişkin adım adım yönergeler için bir TemplateField bir BoundField dönüştürme ve doğrulama denetimleri ekleme.


## <a name="summary"></a>Özet

Bir tablo için şema tanımlarken, Microsoft SQL Server eklenmesi, hesaplanan sütunlar sağlar. Değerleri genellikle aynı kaydın diğer sütunlardan değerleri başvuran bir ifade hesaplanan sütunlar şunlardır. Değerleri bu yana bir ifade üzerinde hesaplanan sütunlar bağlıdır bunlar salt okunur ve bir değer atanamaz bir `INSERT` veya `UPDATE` deyimi. Bu hesaplanan bir sütuna karşılık gelen otomatik olarak oluşturmak için çalışan TableAdapter bağdaştırıcısının ana sorgusunda kullanırken güçlükler `INSERT`, `UPDATE`, ve `DELETE` deyimleri.

Bu öğreticide tarafından hesaplanan sütunlar teşkil zorlukları atlanarak tekniklerini ele almıştık. Özellikle, saklı yordamlar bizim TableAdapter geçici SQL deyimlerini kullanarak TableAdapter bağdaştırıcıları devralınan brittleness çözmek için kullandık. Yeni Oluştur TableAdapter Sihirbazı olan saklı yordamları, biz, hesaplanan sütunlar varlıklarını veri değişikliği depolanan yordamları oluşturulmasını önlediği için başlangıçta atlamak ana sorguda olması önemlidir. TableAdapter başlangıçta yapılandırıldıktan sonra kendi `SelectCommand` saklı yordam retooled, hesaplanan sütunlar eklenecek.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Hilton Geisenow ve Teresa Murphy yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](adding-additional-datatable-columns-vb.md)
> [İleri](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
