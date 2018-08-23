---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Bir iş mantığı katmanı (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, iş kuralları içinde bir iş mantığı katmanı (t arasında veri değişimi için bir aracı olarak görev yapan BLL) tek bir merkezden yönetin. nasıl görüyoruz...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 345f4981ebdd5384068bd42bce0581f94866ad1d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756126"
---
<a name="creating-a-business-logic-layer-vb"></a>Bir iş mantığı katmanı (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) veya [PDF olarak indirin](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> Bu öğreticide, iş kuralları içine bir iş mantığı katmanı (DAL sunu katmanı arasındaki veri değişimi için bir aracı görevi gören BLL) tek bir merkezden yönetin. nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Veri erişim katmanı (DAL) oluşturulan [ilk öğreticide](creating-a-data-access-layer-vb.md) sunu mantığından indrebilirsiniz ayırır veri erişim mantığı. Ancak, DAL, düzgün bir şekilde veri erişim ayrıntılarını sunu katmanı ayıran olsa da uygulanabilir iş kuralları uygulamaz. Örneğin, uygulamamız için vermemek istiyoruz `CategoryID` veya `SupplierID` alanlarının `Products` ne zaman değiştirilecek tablo `Discontinued` alan 1 olarak ayarlayın ya da biz Kıdem kuralları zorunlu olduğu durumlarda yasaklanması isteyebilirsiniz bir çalışan, sonra bunları işe alındım birisi tarafından yönetilir. Başka bir yaygın Yetkilendirme belki de yalnızca kullanıcıların belirli bir roldeki ürünleri silebilir veya değiştirebilirsiniz senaryodur `UnitPrice` değeri.

Bu öğreticide bu iş kuralları içine bir iş mantığı katmanı (DAL sunu katmanı arasındaki veri değişimi için bir aracı görevi gören BLL) tek bir merkezden yönetin. nasıl göreceğiz. Gerçek bir uygulamada, BLL ayrı bir sınıf kitaplığı projesi olarak uygulanması gereken; Ancak, bu öğreticiler için biz BLL sınıflarda oluşan bir dizi olarak uygulayacaksınız bizim `App_Code` proje yapısını kolaylaştırmak için klasör. Şekil 1, sunu katmanı, BLL ve DAL mimari ilişkileri göstermektedir.


![BLL sunu katmanını veri erişim katmanından ayırır ve iş kuralları uygular](creating-a-business-logic-layer-vb/_static/image1.png)

**Şekil 1**: BLL sunu katmanını veri erişim katmanından ayırır ve iş kuralları uygular


Uygulamak için ayrı sınıfları oluşturma yerine bizim [iş mantığı](http://en.wikipedia.org/wiki/Business_logic), biz alternatif olarak bu mantık türü belirtilmiş veri kümesi kısmi sınıfları ile doğrudan içinde yerleştirebilirsiniz. Örneği oluşturma ve bir türü belirtilmiş veri kümesi genişletme geri ilk Öğreticisine bakın.

## <a name="step-1-creating-the-bll-classes"></a>1. adım: BLL sınıfları oluşturma

DAL, her bir TableAdapter için dört sınıfların bizim BLL oluşacaktır; Bu BLL sınıfların her birini, alma, ekleme, güncelleştirme ve uygun iş kuralları uygulanıyor, DAL, ilgili TableAdapter silmeye yöntemleri sahip olur.

DAL ve BLL ilgili sınıflar indrebilirsiniz daha ayırın, iki alt klasör oluşturalım `App_Code` klasöründe `DAL` ve `BLL`. Yalnızca sağ `App_Code` Çözüm Gezgini'nde klasörü yeni bir klasör seçin. Bu iki klasör oluşturduktan sonra içinde ilk öğreticide oluşturulan türü belirtilmiş DataSet taşıma `DAL` alt.

Ardından, dört BLL sınıf dosyaları oluşturmak `BLL` alt. Bunu yapmak için sağ `BLL` alt, Ekle, yeni bir öğe seçin ve sınıf şablonu seçin. Dört sınıf adı `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, ve `EmployeesBLL`.


![Dört yeni sınıflar için App_Code klasörünü Ekle](creating-a-business-logic-layer-vb/_static/image2.png)

**Şekil 2**: dört yeni sınıfa eklemek `App_Code` klasörü


Ardından, her TableAdapter bağdaştırıcılarından ilk öğreticide için tanımlanan yöntemler kaydırılmasına sınıflarının yöntemleri ekleyelim. Şu an için bu yöntemleri yalnızca doğrudan DAL çağıracak; gerekli iş mantığı eklemek için sonraki getireceğiz.

> [!NOTE]
> Visual Studio Standard Edition kullanıyorsanız veya üzeri (diğer bir deyişle, işiniz *değil* Visual Web Developer kullanarak), isteğe bağlı olarak kullanarak görsel olarak sınıflarınızı tasarlayabilirsiniz [Sınıf Tasarımcısı](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Başvurmak [Sınıf Tasarımcısı Blog](https://blogs.msdn.com/classdesigner/default.aspx) Visual Studio'daki bu yeni özellik hakkında daha fazla bilgi için.


İçin `ProductsBLL` sınıfı ihtiyacımız yedi yöntemin toplam eklemek için:

- `GetProducts()` Tüm ürünler döndürür
- `GetProductByProductID(productID)` Belirtilen ürün Kimliğine sahip ürün döndürür
- `GetProductsByCategoryID(categoryID)` Belirtilen kategorideki tüm ürünleri döndürür
- `GetProductsBySupplier(supplierID)` Tüm ürünler belirtilen tedarikçiden döndürür
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Yeni ürün değerleri kullanarak veritabanına ekler. geçilen; döndürür `ProductID` yeni eklenen kaydın değeri
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` geçirilen değerleri kullanarak veritabanında varolan bir ürün güncelleştirmeleri; döndürür `True` tam olarak bir satır güncelleştirildiyse, `False` Aksi takdirde
- `DeleteProduct(productID)` Belirtilen ürün veritabanından siler.

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Yalnızca veri döndüren yöntemler `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, ve `GetProductBySuppliersID` bunlar yalnızca DAL olarak oldukça basittir. Bazı senaryolarda olsa da uygulanması gereken iş kurallarını bu düzeyde (örneğin, şu anda oturum açmış kullanıcı veya kullanıcının ait olduğu rol tabanlı yetkilendirme kuralları), yalnızca bu yöntemleri olarak bırakacağız-olduğu. Bu yöntemler için daha sonra BLL yalnızca ve sunu katmanını temel alınan verileri veri erişim katmanından erişen bir proxy görevi görür.

`AddProduct` Ve `UpdateProduct` yöntemleri hem çeşitli ürün alanları için değerler parametre olarak olur ve yeni bir ürün ekleyin veya mevcut bir sırasıyla güncelleştirin. Bu yana birçok `Product` tablonun sütunlarını kabul edebilir `NULL` değerleri (`CategoryID`, `SupplierID`, ve `UnitPrice`dizileri için), bu giriş parametreleri için `AddProduct` ve `UpdateProduct` böyle bir sütun kullanımı eşleştiren[boş değer atanabilir türler](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Boş değer atanabilir türler .NET 2.0 için yenidir ve bir değer türü, bunun yerine, gerekip gerekmediğini belirten olması için bir teknik sağlayan `Nothing`. Başvurmak [Paul Vick](http://www.panopticoncentral.net/)ın blog girişine [gerçekte hakkında boş değer atanabilir türler ve VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) ve için teknik belgeler [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) yapısı daha fazla bilgi için.

Her üç yöntemi bir satır eklenen, güncelleştirilen veya işlemi etkilenen bir satırda oluşturmayabilir beri silinmiş olup olmadığını gösteren bir Boole değeri döndürür. Örneğin, sayfa Geliştirici çağırırsa `DeleteProduct` öğesinde geçen bir `ProductID` mevcut olmayan ürününün `DELETE` veritabanına verilen deyimi herhangi bir etkisi olacaktır ve bu nedenle `DeleteProduct` yöntemi döndürür `False`.

Yeni ürün eklerken dikkat edin veya var olan bir güncelleştirme Biz yeni veya değiştirilmiş ürünün alan değerlerini kabul aksine skalerler listesi olarak ele bir `ProductsRow` örneği. Bu yaklaşım, çünkü seçildi `ProductsRow` sınıfı türetilen ADO.NET'ten `DataRow` sınıfını varsayılan parametresiz bir oluşturucu yok. Yeni bir oluşturmak için `ProductsRow` örneğinin, sorundan oluşturmalısınız bir `ProductsDataTable` örneği ve ardından çağırmak, `NewProductRow()` yöntemi (biz bunu `AddProduct`). Biz ekleme ve ObjectDataSource kullanarak ürünleri güncelleştirmek için etkinleştirdiğinizde, bu eksiklikleri tamamen baş rears. Kısacası, ObjectDataSource giriş parametrelerini örneği oluşturmayı dener. BLL yöntemi bekliyorsa bir `ProductsRow` örneği ObjectDataSource oluşturabilirsiniz ancak varsayılan bir parametresiz oluşturucusu olmaması nedeniyle başarısız çalışır. Bu sorunla ilgili daha fazla bilgi için aşağıdaki iki ASP.NET Forum postalarına bakın: [güncelleştirme ObjectDataSources Strongly-Typed veri kümeleriyle](https://forums.asp.net/1098630/ShowPost.aspx), ve [sorunu ile ObjectDataSource ve Strongly-Typed veri kümesi](https://forums.asp.net/1048212/ShowPost.aspx).

Hem de sonraki `AddProduct` ve `UpdateProduct`, kod oluşturur bir `ProductsRow` örneği ve yalnızca geçirilen değerlerle doldurur. Çeşitli alan düzeyindeki doğrulama denetimleri bir DataRow DataColumn nesneleri için değerler atama meydana gelebilir. Bu nedenle, el ile geçirilen değerlerin bir DataRow koyarak BLL yönteme geçirilen verilerin geçerliliğini sağlamaya yardımcı olur. Ne yazık ki Visual Studio tarafından oluşturulan türü kesin belirlenmiş DataRow sınıfları boş değer atanabilir türler kullanmayın. Bunun yerine, belirli bir DataRow DataColumn karşılık gelmelidir belirtmek için bir `NULL` veritabanı kullanmamız gerekir değeri `SetColumnNameNull()` yöntemi.

İçinde `UpdateProduct` ürün kullanarak güncelleştirmek için önce yüklediğimiz `GetProductByProductID(productID)`. Bu veritabanı için gerekli olmayan bir seyahat gibi görünebilir, ancak bu ek seyahat iyimser eşzamanlılık keşfedin sonraki öğreticilerde faydalı inanıyor. İyimser eşzamanlılığı, aynı veriler üzerinde aynı anda çalışan iki kullanıcı, başka birinin değişiklikleri yanlışlıkla üzerine yazmayın emin olmak için kullanılan bir tekniktir. Kaydın tamamı kapmasını da güncelleştirme yöntemleri yalnızca DataRow nesnesinin sütunların bir alt kümesini Değiştir BLL içinde oluşturmak kolaylaştırır. Ne zaman inceleyeceğiz `SuppliersBLL` sınıfı biz buna bir örnek göreceksiniz.

Son olarak, dikkat `ProductsBLL` sınıfında [DataObject özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) uygulanmış ( `[System.ComponentModel.DataObject]` hemen önce dosyanın en üstüne yakın class deyimi sözdizimi) ve yöntemleri [ DataObjectMethodAttribute öznitelikleri](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Öznitelik sınıfı bağlama için uygun bir nesne olarak işaretler bir [ObjectDataSource Denetimi](https://msdn.microsoft.com/library/9a4kyhcx.aspx)bilgileriyse `DataObjectMethodAttribute` yöntemi amacı gösterir. Öğreticiler gelecekte anlatıldığı gibi ASP.NET 2.0'ın ObjectDataSource bildirimli olarak bir sınıftan verilere erişmek kolaylaştırır. ObjectDataSource Sihirbazı'nda bağlamak için olası sınıflar listesine filtre uygulamak amacıyla, yalnızca olarak işaretlenmiş sınıflar varsayılan olarak `DataObjects` sihirbazın aşağı açılan listesinde görüntülenir. `ProductsBLL` Sınıfı aynı zamanda bu öznitelikler çalışır, ancak bunları eklemeyi kolaylaştırır ObjectDataSource Sihirbazı'nda çalışmak.

## <a name="adding-the-other-classes"></a>Diğer sınıfları ekleme

İle `ProductsBLL` tam sınıf, biz yine de kategorileri, tedarikçileri ve çalışanlar ile çalışmak için sınıflar eklemeniz gerekir. Aşağıdaki sınıflar ve yöntemler Yukarıdaki örnekteki kavramları kullanarak oluşturmak için bir dakikanızı ayırın:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Önemli bir metottur `SuppliersBLL` sınıfın `UpdateSupplierAddress` yöntemi. Bu yöntem, yalnızca tedarikçi adres bilgilerini güncelleştirmek için bir arabirim sağlar. Dahili olarak, bu yöntem okur `SupplierDataRow` nesne için belirtilen `supplierID` (kullanarak `GetSupplierBySupplierID`) adresi ile ilgili özellikleri ayarlar ve sonra içine yapılan çağrılar `SupplierDataTable`'s `Update` yöntemi. `UpdateSupplierAddress` Yöntemi aşağıdadır:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Bu makaledeki karşıdan my tam bir uygulamaya BLL sınıfların bakın.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>2. adım: türü belirtilmiş DataSets BLL sınıflar üzerinden erişme

İlk öğreticide gördüğümüz doğrudan türü belirtilmiş veri kümesi ile program aracılığıyla çalışma örnekleri, ancak bizim BLL sınıfları'nın eklenmesiyle, sunu katmanı karşı BLL yerine çalışması gerekir. İçinde `AllProducts.aspx` ilk öğreticide, bir örnekten `ProductsTableAdapter` GridView'a, ürünlerin listesini bağlamak için aşağıdaki kodda gösterildiği gibi kullanılan:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Yeni BLL tüm değiştirilmesi gereken sınıflar, kullanmaktır kodun ilk satırını yalnızca değiştirmek `ProductsTableAdapter` nesnesi ile bir `ProductBLL` nesnesi:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

BLL sınıfları ayrıca bildirimli olarak (yazılan veri kümesi gibi) ObjectDataSource kullanılarak erişilebilir. Size daha ayrıntılı ObjectDataSource aşağıdaki öğreticilerde görüştükten.


[![Ürünleri listeler GridView görüntülenir](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Şekil 3**: ürünleri listeler GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>3. adım: Alan düzeyindeki doğrulama DataRow sınıfları ekleme

Alan düzeyindeki doğrulama olan ekleme veya güncelleştirme sırasında iş nesnelerin özellik değerlerini ilgilidir denetler. Bazı ürünler için alan düzeyindeki doğrulama kuralları şunlardır:

- `ProductName` Alan 40 karakter veya daha az olmalıdır
- `QuantityPerUnit` Alan 20 karakter veya daha az olmalıdır
- `ProductID`, `ProductName`, Ve `Discontinued` alanları gereklidir, ancak diğer tüm alanlar isteğe bağlıdır
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, Ve `ReorderLevel` alanları değerinden büyük veya sıfıra eşit olmalıdır

Bu kurallar, olabilir ve veritabanı düzeyinde ifade edilmelidir. Karakter sınırı `ProductName` ve `QuantityPerUnit` alanları bu sütunların veri türleri tarafından yakalanır `Products` tablo (`nvarchar(40)` ve `nvarchar(20)`sırasıyla). Gerekli ve isteğe bağlı alanları olup ifade edilir tarafından veritabanı tablo sütununa izin veriyorsa `NULL` s. Dört [denetim kısıtlamaları](https://msdn.microsoft.com/library/ms188258.aspx) mevcut değerler yalnızca büyük veya sıfıra eşit, içine zorlaştırabilir emin `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, veya `ReorderLevel` sütunları.

Bu kurallar, veritabanı zorlamayı yanı sıra, ayrıca veri kümesi düzeyinde zorunlu tutulmalıdır. Aslında, alan uzunluğu ve bir değer gerekli veya isteğe bağlı olup DataColumn nesneleri her DataTable'nın kümesi için zaten yakalanır. Otomatik olarak sağlanan mevcut alan düzeyindeki doğrulama görmek için veri kümesi Tasarımcısı'na gidin, DataTables birinden bir alan seçin ve sonra Özellikler penceresine gidin. Şekil 4'te gösterildiği gibi `QuantityPerUnit` DataColumn `ProductsDataTable` en fazla 20 karakterden oluşabilir ve izin vermiyor `NULL` değerleri. Ayarlamaya çalışırsanız `ProductsDataRow`'s `QuantityPerUnit` 20 karakterden uzun bir dize özelliğini bir `ArgumentException` oluşturulur.


[![DataColumn temel alan düzeyindeki doğrulama sağlar.](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Şekil 4**: DataColumn ile sağlayan temel alan düzeyindeki doğrulama ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-business-logic-layer-vb/_static/image8.png))


Ne yazık ki, size sınırları denetimleri gibi belirtemezsiniz `UnitPrice` değeri büyüktür ya da Özellikler penceresinde aracılığıyla sıfıra eşit olmalıdır. Bu tür bir alan düzeyindeki doğrulama sağlamak için bir olay işleyicisi için olan DataTable öğesiyle 's oluşturmak ihtiyacımız [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) olay. Belirtildiği gibi [önceki öğretici](creating-a-data-access-layer-vb.md), kısmi sınıflar kullanarak türü belirtilmiş veri kümesi tarafından oluşturulan veri kümesi, DataTables ve DataRow nesneleri genişletilebilir. Biz oluşturabilir, bu tekniği kullanarak bir `ColumnChanging` için olay işleyicisi `ProductsDataTable` sınıfı. Bir sınıfta oluşturarak başlayın `App_Code` adlı klasöre `ProductsDataTable.ColumnChanging.vb`.


[![Yeni bir sınıf, App_Code klasörü Ekle](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Şekil 5**: yeni bir sınıfa eklemek `App_Code` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-business-logic-layer-vb/_static/image11.png))


Ardından, bir olay işleyicisi oluşturun `ColumnChanging` sağlar olay `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` sütun değerleri (Aksi halde `NULL`) daha büyük veya sıfıra eşit. Herhangi bir sütun je mimo rozsah bağlanamazsa bir `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>4. adım: Özel iş kurallarını BLL'ın sınıflara ekleme

Alan düzeyindeki doğrulama yanı sıra gibi farklı varlıkları ya da tek bir sütun düzeyinde değil ifade kavramları içeren üst düzey özel iş kurallarını olabilir:

- Bir ürün kullanımdan kaldırılmıştır, kendi `UnitPrice` güncelleştirilemiyor
- Bir çalışanın ülkeye göre kendi manager'ın ülkeye göre aynı olması gerekir
- Varsa sağlayıcı tarafından sağlanan tek ürün ürün kullanımdan olamaz

BLL sınıfları, uygulamanın işletme kurallarına çoğaltılmadığını denetler içermelidir. Bu denetimler, uygulandıkları yöntemlerini doğrudan eklenebilir.

Belirli bir sağlayıcı yalnızca ürün olduysa bir ürün artık sağlanmayan işaretlenemez, iş kurallarımızın dikte düşünün. Diğer bir deyişle, varsa ürün *X* tedarikçiden satın biz yalnızca ürün *Y*, biz değil işaretleyebilir *X* kullanımdan kaldırıldı; gibi ancak tedarikçi *Y*bize üç ürünleri ile sağlanan *A*, *B*, ve *C*, sonra size her işaretleyebilir ve bu dosyaların tümünün kullanımdan kaldırıldı. Bir tek bir iş kuralı, ancak iş kuralları ve sağduyunuzu her zaman hizalı değil!

İçinde bu iş kuralını uygulamak için `UpdateProducts` biz start olmadığının kontrol edilmesiyle yöntemi `Discontinued` ayarlandı `True` ve bu nedenle, biz çağırırsınız, `GetProductsBySupplierID` kaç ürünleri belirlemek için Biz bu ürün uygulamasının tedarikçiden satın. Yalnızca bu tedarikçiden bir ürün satın alınır, biz throw bir `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Sunu katmanındaki doğrulama hataları ele alma

Sunu katmanı BLL çağırırken yükseltilmiş ya da ASP.NET kadar Kabarcık bildirmek özel durumları işlemek deneme edilip edilmeyeceğine karar verebilirsiniz (hangi yükseltmek `HttpApplication`'s `Error` olay). BLL ile programlı olarak çalışırken, özel bir durumu işlemek için kullanabiliriz bir [deneyin... Catch](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) blok, aşağıdaki örnekte gösterildiği gibi:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Gelecekte öğreticiler göreceğiz, yedekleme veri kullanırken BLL Kabarcık özel durumları işleme denetim ekleme, güncelleştirme, Web veya veri silme işlenebilir kod içinde kaydırmak zorunda yerine doğrudan bir olay işleyicisi olarak `Try...Catch` engeller.

## <a name="summary"></a>Özet

Her biri belirli bir rol yalıtan ayrı katmanlara hazırlanmış bir iyi düzenlenmiş bir uygulama. Bu makale serisi ilk öğreticide yazılan veri kümeleri kullanarak veri erişim katmanını oluşturduğumuz; Bu öğreticide bir iş mantığı katmanı sınıfları bir dizi olarak bizim uygulamanın oluşturduk `App_Code` bizim DAL çağırmak klasör. BLL uygulamamız için alan ve iş düzeyi mantığını uygular. Bu öğreticide, yaptığımız ayrı BLL oluşturmaya ek olarak, başka bir seçenek kısmi sınıflar kullanarak TableAdapter bağdaştırıcıları yöntemleri genişletmek için aynıdır. Ancak, bu tekniği kullanarak mevcut yöntemleri geçersiz kılmak bize izin vermez veya bu bizim DAL ve bizim BLL Biz bu makalede zamandaki yaklaşım olarak düzgün bir şekilde ayırmak yapar.

DAL ve BLL tam size sunduğumuz sunu katmanına başlamak hazırsınız. İçinde [sonraki öğreticiye](master-pages-and-site-navigation-vb.md) biz kısa bir sapma veri erişim konulardan yararlanın ve öğreticileri boyunca kullanmanız için bir tutarlı sayfa düzeni tanımlamak.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Liz Shulok, Dennis Patterson ve Konuğu, Carlos Santos ve Hilton Giesenow yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-a-data-access-layer-vb.md)
> [İleri](master-pages-and-site-navigation-vb.md)
