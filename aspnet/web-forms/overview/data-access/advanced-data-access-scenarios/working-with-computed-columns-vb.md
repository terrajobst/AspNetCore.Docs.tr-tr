---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Hesaplanan sütunlar (VB) ile çalışma | Microsoft Docs
author: rick-anderson
description: Bir veritabanı tablosu oluştururken, Microsoft SQL Server, hesaplanan bir sütun değeri ifadeden hesaplanan tanımlamanıza olanak sağlar, genellikle referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 04b39902aae05d815eb11ec7b7163988d017f78c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877209"
---
<a name="working-with-computed-columns-vb"></a>Hesaplanan sütunlar (VB) ile çalışma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) veya [PDF indirin](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Bir veritabanı tablosu oluştururken, Microsoft SQL Server genellikle diğer değerleri de aynı veritabanı kaydını başvuran bir ifade değeri hesaplandığı hesaplanmış bir sütun tanımlamanızı sağlar. Bu tür, salt okunur TableAdapters ile çalışırken, özel durumların dikkate alınmasını gerektirir veritabanı adresindeki değerlerdir. Bu öğreticide biz tarafından hesaplanan sütunlar teşkil zorlayan öğrenin.


## <a name="introduction"></a>Giriş

Microsoft SQL Server sağlar  *[hesaplanan sütunlar](https://msdn.microsoft.com/library/ms191250.aspx)*, değerleri genellikle aynı tablodaki başka sütunlardan değerleri başvuran bir ifadeden hesaplanan sütun olduğu. Örnek olarak, bir süre veri modeli izleme adlı bir tablo olabilir `ServiceLog` dahil olmak üzere sütunlarla `ServicePerformed`, `EmployeeID`, `Rate`, ve `Duration`, diğerlerinin yanı sıra. While miktarı nedeniyle hizmeti başına öğesi (süresine çarpılan oranı olan) hesaplanacağını bir web sayfası veya diğer program arabirimi, bir sütunda dahil etmek için kullanışlı olabilecek `ServiceLog` adlı tablo `AmountDue` Bu rapor bilgi. Bu sütun için normal bir sütun olarak oluşturulamadı, ancak dilediğiniz zaman güncelleştirilmesi gerekir `Rate` veya `Duration` sütun değerlerini değiştirildi. Daha iyi bir yaklaşım sağlamak olacaktır `AmountDue` sütun ifade kullanılarak hesaplanan bir sütun `Rate * Duration`. Bunun yapılması SQL Server'ın otomatik olarak hesaplamak neden `AmountDue` sorguda başvuruldu her sütun değeri.

Bir hesaplanan sütun s değeri bir ifade tarafından belirlenir olduğundan, bu sütunları salt okunur ve bu nedenle değerleri bunları atadığınız olamaz `INSERT` veya `UPDATE` deyimleri. Hesaplanan sütunlar geçici SQL deyimlerini kullanan bir TableAdapter için ana sorgu parçası olduğunda, ancak bunlar otomatik olarak otomatik olarak oluşturulan dahil `INSERT` ve `UPDATE` deyimleri. Sonuç olarak, TableAdapter s `INSERT` ve `UPDATE` sorgular ve `InsertCommand` ve `UpdateCommand` özellikleri tüm hesaplanan sütunlar başvuruları kaldırmak için güncelleştirilmesi gerekir.

Kullanarak bir sınama hesaplanan sütunlar geçici SQL deyimlerini kullanan bir TableAdapter ile TableAdapter s olan `INSERT` ve `UPDATE` sorguları otomatik olarak TableAdapter Yapılandırma Sihirbazı'nı tamamlandığında dilediğiniz zaman yeniden oluşturuldu. Bu nedenle, hesaplanan sütunlar el ile kaldırılması `INSERT` ve `UPDATE` sorguları Sihirbazı'nı yeniden çalışma ise görünecektir. Saklı yordamları kullanma TableAdapters güncelleştireceğinizi t rağmen bu brittleness yaşar, adım 3'te ele alınacaktır kendi quirks gerekir.

Bu öğreticide bir hesaplanan sütun ekleyeceğiz `Suppliers` tablo Northwind veritabanına ve bu tablo ve hesaplanan sütunda çalışmak için karşılık gelen bir TableAdapter oluşturun. Saklı yordamlar, geçici SQL deyimleri yerine kullanabilir, böylece TableAdapter Yapılandırma Sihirbazı kullanıldığında bizim özelleştirmeleri geçekleştirilen t kayıp bizim TableAdapter sahibiz.

Let s başlayın!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>1. adım: bir hesaplanan sütun ekleme`Suppliers`tablosu

Kendisini bir eklemek ihtiyacımız şekilde Northwind veritabanı hesaplanan sütun yok. Bu öğretici için hesaplanan bir sütun ekleyin s denetlemesine izin vermek için `Suppliers` adlı bir tablo `FullContactName` döndüren sorumlu s adı, başlık ve çalışmak için şirket şu biçimde: `ContactName` (`ContactTitle`, `CompanyName`). Başka bir hesaplanan sütun raporlarda Üreticiler hakkında bilgi görüntülerken kullanılabilir.

Başlangıç açarak `Suppliers` tablo tanımı üzerinde sağ tıklayarak `Suppliers` sunucu Gezgini'nde tablo ve bağlam menüsünden açık tablo tanımı seçme. Bunlar izin olup olmadığını bu tabloyu ve kendi veri türü gibi özelliklerinin sütunlarını görüntüler `NULL` s ve benzeri. Hesaplanmış bir sütun eklemek için sütun adına tablo tanımı yazarak başlatın. Ardından, kendi ifade sütun özellikleri penceresinde hesaplanan sütun belirtimi bölümünde (Formül) metin girerek (bkz: Şekil 1). Hesaplanan sütun adı `FullContactName` ve aşağıdaki deyimi kullanın:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

SQL dizeleri birleştirilmiş olduğunu unutmayın kullanarak `+` işleci. `CASE` Deyimi, geleneksel bir programlama dili koşullu gibi kullanılabilir. Yukarıdaki ifadede `CASE` deyimi olarak okunabilir: varsa `ContactTitle` değil `NULL` ardından çıktı `ContactTitle` virgül ile aksi takdirde birleştirilmiş değer nothing yayma. Yararlılığı hakkında daha fazla bilgi için `CASE` deyimi, bkz: [SQL güç `CASE` deyimleri](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Kullanmak yerine bir `CASE` burada deyimi, alternatif olarak kullandık `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) döndürür *checkExpression* NULL değilse, aksi takdirde döndürür *replacementValue*. While ya da `ISNULL` veya `CASE` çalışır Bu örnekte, daha karmaşık senaryolar vardır burada esnekliğini `CASE` deyimi tarafından eşleşebilecek olamaz `ISNULL`.


Bu hesaplanan sütunu ekledikten sonra ekranınızın Şekil 1'de ekran gibi görünmelidir.


[![Üreticiler tablosuna FullContactName adlı hesaplanan bir sütun ekleyin](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Şekil 1**: hesaplanan sütun adlandırılmış eklemek `FullContactName` için `Suppliers` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image3.png))


Hesaplanan sütunu adlandırma ve ifadesini girdikten sonra değişiklikleri tabloya araç çubuğunda Kaydet simgesini tıklatarak Ctrl + S basarsa veya dosya menüsüne giderek ve kaydetme seçerek kaydetmek `Suppliers`.

Yeni eklenen sütununda dahil olmak üzere Sunucu Gezgini tablosu kaydediliyor yenileme `Suppliers` s tablosu sütun listesi. Ayrıca, (Formül) metin kutusuna girdiğiniz ifade otomatik olarak gereksiz boşluk kaldırır, sütun adları köşeli ile çevreleyen eşdeğer bir ifadeye ayarlar (`[]`) ve daha açıkça göstermek için parantez içerir işlem sırası:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Microsoft SQL Server'da hesaplanan sütunlar üzerinde daha fazla bilgi için bkz [teknik belgeler](https://msdn.microsoft.com/library/ms191250.aspx). Ayrıca kullanıma [nasıl yapılır: hesaplanan sütunlar belirtin](https://msdn.microsoft.com/library/ms188300.aspx) hesaplanan sütunlar oluşturma adım adım kılavuz.

> [!NOTE]
> Varsayılan olarak, hesaplanan sütunlar tabloda fiziksel olarak depolanan değil ancak bunun yerine bir sorguda başvurulan her zaman yeniden hesaplanır. Kalıcıdır onay kutusunu işaretleyerek, ancak, fiziksel olarak hesaplanan sütunu tabloda depolamak için SQL Server söyleyebilirsiniz. Bunun yapılması, hesaplanan sütun değeri kullanan sorguları performansını artırabilir hesaplanan sütunda oluşturulması için bir dizin verir kendi `WHERE` yan tümceleri. Bkz: [hesaplanan sütun dizinleri oluşturma](https://msdn.microsoft.com/library/ms189292.aspx) daha fazla bilgi için.


## <a name="step-2-viewing-the-computed-column-s-values"></a>2. adım: hesaplanan sütun s değerlerini görüntüleme

Let s'ı biz iş veri erişim katmanı başlamadan önce görüntülemek için bir dakikanızı ayırın `FullContactName` değerleri. Server Explorer'dan sağ `Suppliers` tablo adı ve bağlam menüsünden Yeni sorguyu seçin. Bu bize sorguya eklenecek hangi tabloları seçmenizi ister bir sorgu penceresi getirir. Ekleme `Suppliers` tablo ve Kapat'ı tıklatın. Ardından, denetleme `CompanyName`, `ContactName`, `ContactTitle`, ve `FullContactName` değişecekse sütunlarından. Son olarak, sorguyu çalıştırmak ve sonuçları görüntülemek için araç çubuğunda kırmızı bir ünlem işareti simgesine tıklayın.

Şekil 2'de görüldüğü gibi sonuçlarında `FullContactName`, hangi listeleri `CompanyName`, `ContactName`, ve `ContactTitle` biçimini kullanarak sütunları `ContactName` (`ContactTitle`, `CompanyName`).


[![FullContactName biçimi ContactName (ContactTitle, ŞirketAdı) kullanır](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Şekil 2**: `FullContactName` biçimini kullanan `ContactName` (`ContactTitle`, `CompanyName`) ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>3. adım: Ekleme`SuppliersTableAdapter`veri erişim katmanı için

Uygulamamız tedarikçi bilgileri ile çalışması için size bir TableAdapter ve DataTable bizim DAL oluşturmanız gerekir. İdeal olarak, bu önceki eğitimlerine incelenmesi aynı basit adımları kullanarak gerçekleştirilmesi. Ancak, hesaplanan sütunlar çalışmak tartışma belirlenmiştir birkaç kırışıklıkların tanıtır.

Geçici SQL deyimlerini kullanan bir TableAdapter kullanıyorsanız, TableAdapter Yapılandırma Sihirbazı'nı aracılığıyla TableAdapter s ana sorgu yalnızca hesaplanan sütunu ekleyebilirsiniz. Bu, ancak otomatik-oluşturur `INSERT` ve `UPDATE` hesaplanan sütunu içeren deyimleri. Aşağıdaki yöntemlerden birini yürütmeyi denerseniz bir `SqlException` sütun iletiyle *ColumnName* hesaplanan bir sütun ya da bir UNION işlecinin sonucu durum olduğu için değiştirilemiyor. Sırada `INSERT` ve `UPDATE` deyimi TableAdapter s el ile da ayarlanabilir `InsertCommand` ve `UpdateCommand` özelliklerini bu özelleştirmeler kaybolacak gerektiğinde TableAdapter Yapılandırma Sihirbazı'nı yeniden çalıştırın.

Kırılganlığının da artacağını geçici SQL deyimleri kullanmak TableAdapters nedeniyle, saklı yordamlar hesaplanan sütunlar ile çalışırken kullandığımızı önerilir. Var olan saklı yordamları kullanıyorsanız, yalnızca TableAdapter içinde açıklandığı gibi yapılandırın [kullanarak var olan saklı yordamlar için yazılan veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Öğreticisi. Ancak, saklı yordamları oluşturduğunuz TableAdapter Sihirbazı varsa, başlangıçta tüm hesaplanan sütunlar ana sorgudan atlayın önemlidir. Hesaplanmış bir sütun ana sorguya eklerseniz, TableAdapter Yapılandırma Sihirbazı'nı, tamamlandıktan sonra karşılık gelen saklı yordamlar oluşturulamıyor size bildirir. Kısacası, başlangıçta bir hesaplanan sütun serbest ana sorgu kullanarak TableAdapter yapılandırma ve daha sonra el ile ilgili saklı yordam ve TableAdapter s güncelleştirmek ihtiyacımız `SelectCommand` hesaplanan sütun eklemek için. Bu yaklaşım kullanılanla benzer [TableAdapter kullanımı için güncelleştirme](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* Öğreticisi.

Bu öğretici için yeni bir TableAdapter ekleyin ve bu saklı yordamları bize için otomatik olarak oluştur sahip s olanak tanır. Sonuç olarak, başlangıçta atlamak ihtiyacımız `FullContactName` ana sorgudan hesaplanan sütun.

Başlangıç açarak `NorthwindWithSprocs` kümesinde `~/App_Code/DAL` klasör. Tasarımcıda sağ tıklatın ve bağlam menüsünden Yeni bir TableAdapter eklemek için seçin. Bu TableAdapter Yapılandırma Sihirbazı'nı başlatır. Veritabanı sorgusu verilerden belirtin (`NORTHWNDConnectionString` gelen `Web.config`) ve İleri'yi tıklatın. Biz henüz sorgulama veya değiştirmek için saklı yordamlarda oluşturmadıysanız bu yana `Suppliers` tablo, yeni saklı yordamlar seçeneği sihirbaz bize oluşturur ve İleri'yi tıklatın, Oluştur'u seçin.


[![Create Yeni saklı yordamlar seçeneği seçin](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Şekil 3**: oluştur Yeni saklı yordamlar seçeneği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image9.png))


Sonraki adım bize için ana sorgu ister. Döndürür aşağıdaki sorguyu girin `SupplierID`, `CompanyName`, `ContactName`, ve `ContactTitle` her sağlayıcı için sütun. Bu sorgu var hesaplanan sütunu atlar Not (`FullContactName`); 4. adımda bu sütun eklemek için karşılık gelen saklı yordam güncelleştireceğiz.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Ana sorgu girme ve İleri'yi tıklatmadan sonra sihirbaz bunu oluşturacak dört saklı yordamlar ad olanak tanır. Bu saklı yordamları ad `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, ve `Suppliers_Delete`Şekil 4'te gösterildiği gibi.


[![Otomatik olarak oluşturulan saklı yordamları adlarını özelleştirin](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Şekil 4**: Auto-Generated saklı yordamlar adlarını özelleştirin ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image12.png))


Sonraki sihirbaz adımı TableAdapter s yöntemleri adı ve erişim ve güncelleştirme verileri için kullanılan desenlerini belirtmek olanak tanır. Seçili tüm üç onay kutularını bırakır, ancak yeniden adlandırma `GetData` yöntemine `GetSuppliers`. Sihirbazı tamamlamak için Son'u tıklatın.


[![GetData yöntemi için GetSuppliers yeniden adlandır](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Şekil 5**: yeniden adlandırma `GetData` yönteme `GetSuppliers` ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image15.png))


Bitiş tıklatıldığında, sihirbaz dört saklı yordamlar oluşturun ve karşılık gelen DataTable ve TableAdapter yazılan veri kümesi ekleyin.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>4. adım: hesaplanan sütun TableAdapter s ana sorguya

Şimdi TableAdapter güncelleştirmek ihtiyacımız ve DataTable eklemek için adım 3'te oluşturulan `FullContactName` hesaplanan sütun. Bu iki adımdan oluşur:

1. Güncelleştirme `Suppliers_Select` saklı yordamı döndürülecek `FullContactName` hesaplanmış bir sütun ve
2. Karşılık gelen içerecek şekilde DataTable güncelleştirme `FullContactName` sütun.

Sunucu Gezgini gittikten saklı yordamlar klasörüne araştırıp başlatın. Açık `Suppliers_Select` saklı yordam ve güncelleştirme `SELECT` dahil etmek için sorgu `FullContactName` hesaplanan sütun:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Araç çubuğundaki Kaydet simgesine tıklayarak, Ctrl + S basarsa veya kaydetme seçme saklı yordam değişiklikleri kaydetmek `Suppliers_Select` Dosya menüsünden seçeneği.

Sağ tıklayın, ardından, veri kümesi tasarımcısına geri dönmek `SuppliersTableAdapter`ve bağlam menüsünden Yapılandır'ı seçin. Unutmayın `Suppliers_Select` sütun artık içerir `FullContactName` sütunu, veri sütunlarını koleksiyonu.


[![DataTable s sütunları güncelleştirmek için TableAdapter s Yapılandırma Sihirbazı'nı çalıştırın](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Şekil 6**: TableAdapter s DataTable s sütunları güncelleştirmek için Yapılandırma Sihirbazı'nı çalıştırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image18.png))


Sihirbazı tamamlamak için Son'u tıklatın. Bu otomatik olarak karşılık gelen bir sütun ekler `SuppliersDataTable`. TableAdapter Sihirbazı'nı algılayan akıllı `FullContactName` sütundur hesaplanmış bir sütun ve dolayısıyla salt okunur. Sonuç olarak, s sütunu ayarlar `ReadOnly` özelliğine `true`. Bunu doğrulamak için sütunundan seçin `SuppliersDataTable` ve Özellikler penceresine gidin (bkz. Şekil 7). Unutmayın `FullContactName` s sütunu `DataType` ve `MaxLength` özellikleri da ayarlanır buna göre.


[![FullContactName sütun salt okunur olarak işaretlenmiş](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Şekil 7**: `FullContactName` sütun salt okunur olarak işaretlenmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>5. adım: Ekleme bir`GetSupplierBySupplierID`TableAdapter yöntemi

Bu öğretici için güncelleştirilebilir kılavuzunda tedarikçileri görüntüleyen bir ASP.NET sayfası oluşturacağız. İçinde öğreticileri biz tek bir iş mantığı katmanı kaydından özelliklerini güncelleştirmek ve güncelleştirilmiş DataTable gönderme kesin türü belirtilmiş DataTable, olarak DAL belirli kaydından değişiklikleri yaymak için DAL için geri alarak güncelleştirdiniz Veritabanı. -DAL güncelleştirilmekte kayıt alma - bu ilk adımı gerçekleştirmek için ilk olarak eklemek ihtiyacımız bir `GetSupplierBySupplierID(supplierID)` DAL yöntemi.

Sağ `SuppliersTableAdapter` DataSet tasarım ve bağlam menüsünden Sorgu Ekle seçeneğini belirleyin. Adım 3'te yaptığımız gibi yeni bir saklı yordam bize oluştur Yeni saklı yordam seçeneğini seçerek Oluştur Sihirbazı sağlar (geri Bu sihirbaz adımı için bir ekran görüntüsü Şekil 3'e bakın). Bu yöntem birden çok sütun içeren bir kayıt döndürecektir beri satırlar döndüren bir SELECT olan bir SQL sorgusu kullanın ve İleri'ye istiyoruz gösterir.


[![Satır seçeneği döndüren Seç](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Şekil 8**: satır seçeneği döndüren Seç ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image24.png))


Sonraki adım için bu yöntemi kullanmak bir sorgu için bize ister. Ana sorgu olarak ancak belirli bir üretici için aynı veri alanları döndüren aşağıdaki girin.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Sonraki ekranda bize otomatik olarak oluşturulan saklı yordam adı ister. Bu saklı yordam adı `Suppliers_SelectBySupplierID` ve İleri'yi tıklatın.


[![Saklı yordam Suppliers_SelectBySupplierID adı](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Şekil 9**: saklı yordam adı `Suppliers_SelectBySupplierID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image27.png))


Son olarak, Sihirbazı ister bize veriler için erişim desenlerini ve TableAdapter için kullanılacak yöntemi adları. İki onay kutusunun işaretli bırakın, ancak yeniden adlandırma `FillBy` ve `GetDataBy` yöntemlere `FillBySupplierID` ve `GetSupplierBySupplierID`sırasıyla.


[![Ad TableAdapter yöntemleri FillBySupplierID ve GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Şekil 10**: TableAdapter yöntemleri ad `FillBySupplierID` ve `GetSupplierBySupplierID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image30.png))


Sihirbazı tamamlamak için Son'u tıklatın.

## <a name="step-6-creating-the-business-logic-layer"></a>6. adım: iş mantığı katmanı oluşturma

Biz, 1. adımda oluşturduğunuz hesaplanan sütun kullanan bir ASP.NET sayfası oluşturmadan önce biz öncelikle karşılık gelen yöntemleri BLL eklemeniz gerekir. Adım 7'de oluşturacağız, ASP.NET sayfamızı kullanıcıların görüntülemek ve Üreticiler düzenlemesine izin verir. Bu nedenle, en azından tüm tedarikçileri ve başka bir belirli tedarikçi güncelleştirmek için almak için bir yöntem sağlamak amacıyla bizim BLL gerekir.

Adlı yeni bir sınıf dosyası oluşturma `SuppliersBLLWithSprocs` içinde `~/App_Code/BLL` klasörü ve aşağıdaki kodu ekleyin:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Gibi diğer BLL sınıfları `SuppliersBLLWithSprocs` sahip bir `Protected` `Adapter` örneğini döndüren özelliği `SuppliersTableAdapter` sınıfı iki birlikte `Public` yöntemleri: `GetSuppliers` ve `UpdateSupplier`. `GetSuppliers` Yöntemi çağırır ve döndürür `SuppliersDataTable` karşılık gelen tarafından döndürülen `GetSupplier` veri erişim katmanı yöntemi. `UpdateSupplier` Yöntemi alır DAL s çağrısıyla güncelleştirilmekte belirli tedarikçi hakkında bilgi `GetSupplierBySupplierID(supplierID)` yöntemi. Daha sonra güncelleştirmeleri `CategoryName`, `ContactName`, ve `ContactTitle` özellikleri ve veri erişim katmanı s çağırarak bu değişiklikleri veritabanına kaydeder `Update` değiştirilmiş geçirerek yöntemini `SuppliersRow` nesnesi.

> [!NOTE]
> Dışında `SupplierID` ve `CompanyName`, üreticiler tablodaki tüm sütunların izin `NULL` değerleri. Bu nedenle, varsa geçilen `contactName` veya `contactTitle` parametreleri `Nothing` karşılık gelen ayarlamak ihtiyacımız `ContactName` ve `ContactTitle` özelliklerine bir `NULL` veritabanı değerini kullanarak `SetContactNameNull` ve `SetContactTitleNull`yöntemleri, sırasıyla.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>7. adım: sunu katmanı hesaplanan sütunu ile çalışma

Eklenen hesaplanan sütunu ile `Suppliers` ve DAL ve BLL güncelleştirilmiş buna göre biz çalışır bir ASP.NET sayfası oluşturmak hazır duruma gelirsiniz `FullContactName` hesaplanan sütun. Başlangıç açarak `ComputedColumns.aspx` sayfasındaki `AdvancedDAL` klasörü ve araç tasarımcıya GridView sürükleyin. GridView s ayarlamak `ID` özelliğine `Suppliers` ve akıllı etiketten adlı yeni bir ObjectDataSource bağlayın `SuppliersDataSource`. ObjectDataSource kullanmak için yapılandırma `SuppliersBLLWithSprocs` eklediğimiz sınıf adım 6'geri ve İleri'yi tıklatın.


[![ObjectDataSource SuppliersBLLWithSprocs sınıfını kullanmak için yapılandırma](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Şekil 11**: ObjectDataSource kullanılacak yapılandırma `SuppliersBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image33.png))


Tanımlanan yalnızca iki yöntem vardır `SuppliersBLLWithSprocs` sınıfı: `GetSuppliers` ve `UpdateSupplier`. Bu iki yöntem Seç belirtilir ve sekmeler, sırasıyla GÜNCELLEŞTİRMEK ve ObjectDataSource yapılandırmasını tamamlamak için Son'u tıklatın emin olun.

Veri Kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra Visual Studio döndürülen veri alanların her biri için bir BoundField ekleyeceksiniz. Kaldırma `SupplierID` BoundField değiştirip `HeaderText` özelliklerini `CompanyName`, `ContactName`, `ContactTitle`, ve `FullContactName` BoundFields şirket, ilgili kişi adı, başlık ve tam ilgili kişi adı, sırasıyla. Akıllı etiketten GridView s yerleşik düzenleme yetenekleri açın için düzenlemeyi etkinleştir onay kutusunu işaretleyin.

GridView BoundFields ekleme ek olarak, veri kaynağı Sihirbazı'nın tamamlanma ayrıca ObjectDataSource s ayarlamak Visual Studio neden `OldValuesParameterFormatString` özgün özelliğine\_{0}. Bu geri {0} varsayılan değerine ayarına geri alın.

Bu düzenlemeler GridView ve ObjectDataSource yaptıktan sonra bunların bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Ardından, bir tarayıcı aracılığıyla bu sayfasını ziyaret edin. Şekil 12 gösterildiği gibi her üretici içeren kılavuzunda listelenen `FullContactName` değeri yalnızca diğer üç sütun birleştirme sütunu, biçimlendirilmiş olarak `ContactName` (`ContactTitle`, `CompanyName`).


[![Her üretici kılavuzunda listelenen](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Şekil 12**: her tedarikçi kılavuzunda listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image36.png))


Belirli bir üretici geri gönderimin neden olur ve bu satırın işlenmiş olan için Düzenle düğmesini tıklatarak kendi düzenleme arabirim (bkz. Şekil 13). İlk üç sütun arabirimini düzenleme kendi varsayılan işleme - metin kutusu denetim `Text` özelliği, verileri alan değerine ayarlanır. `FullContactName` Sütun, ancak metin olarak devam eder. Veri Kaynağı Yapılandırma Sihirbazı'nın tamamlanma GridView BoundFields eklendiğinde `FullContactName` BoundField s `ReadOnly` özelliği ayarlandığı `True` çünkü karşılık gelen `FullContactName` sütununda `SuppliersDataTable` sahip kendi `ReadOnly` özelliğini `True`. Adım 4'te belirtildiği gibi `FullContactName` s `ReadOnly` özelliği ayarlandığı `True` çünkü TableAdapter sütunu hesaplanmış bir sütun olduğunu algıladı.


[![FullContactName sütundur düzenlenemez](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Şekil 13**: `FullContactName` sütundur düzenlenemez ([tam boyutlu görüntüyü görüntülemek için tıklatın](working-with-computed-columns-vb/_static/image39.png))


Şimdi, bir veya daha fazla düzenlenebilir sütunları değerini güncelleştirin ve Güncelleştir'i tıklatın. Not nasıl `FullContactName` s değeri değişikliği yansıtacak şekilde otomatik olarak güncelleştirilir.

> [!NOTE]
> Düzenlenebilir alanları için şu anda kullandığı BoundFields arabirimini düzenleme varsayılan olarak kaynaklanan GridView. Bu yana `CompanyName` alan gereklidir, bir RequiredFieldValidator içeren TemplateField'a dönüştürülmesi. I bunu bir alıştırma olarak ilgi Okuyucu için bırakın. Başvurun [doğrulama denetimleri ekleme düzenleme ve ekleme arabirimleri için](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) ilişkin adım adım yönergeler için TemplateField bir BoundField dönüştürme ve doğrulama denetimleri ekleme Öğreticisi.


## <a name="summary"></a>Özet

Bir tablo için şema tanımlarken, Microsoft SQL Server hesaplanan sütunlar dahil edilmesi sağlar. Bu değerleri genellikle diğer sütunlardan aynı kayıtta değerleri başvuran bir ifade hesaplanan sütunlar'dır. ' Den itibaren değerler hesaplanan sütunlar ifadeye bağlı tabanlı için bunlar salt okunurdur ve bir değer atanamaz bir `INSERT` veya `UPDATE` deyimi. Bu hesaplanan bir sütuna karşılık gelen otomatik olarak oluşturmak için çalışan bir TableAdapter ana sorguda kullanırken zorluklar getirir `INSERT`, `UPDATE`, ve `DELETE` deyimleri.

Bu öğreticide tarafından hesaplanan sütunlar teşkil zorluklar atlamak için teknikleri açıklanmıştır. Özellikle, saklı yordamlar bizim TableAdapter geçici SQL deyimlerini kullanan TableAdapters içinde devralınan brittleness üstesinden gelmek için kullandık. Saklı yordamlar yeni oluşturma TableAdapter Sihirbazı'nı sahip olduğunda, biz varlıklarını veri değişikliği saklı yordamları oluşturulmasını önler engellediğinden Başlangıçta tüm hesaplanan sütunlar atlayın ana sorgu olması önemlidir. TableAdapter başlangıçta yapılandırıldıktan sonra kendi `SelectCommand` saklı yordam retooled tüm hesaplanan sütunlar eklenecek.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Hilton Geisenow ve Teresa Murphy yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](adding-additional-datatable-columns-vb.md)
> [sonraki](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
