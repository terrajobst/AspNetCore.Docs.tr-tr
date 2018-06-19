---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: Bir iş mantığı katmanı (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide, iş kurallarını içine bir iş mantığı katmanı (t arasında veri alışverişi için bir aracı görevi gören BLL) merkezileştirmek nasıl göreceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 150862decbbb69747f3e957a941b71b118b7231c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892770"
---
<a name="creating-a-business-logic-layer-vb"></a>Bir iş mantığı katmanı (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe) veya [PDF indirin](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> Bu öğreticide, iş kurallarını içine bir iş mantığı katmanı (sunu katmanı ve DAL arasında veri alışverişi için bir aracı görevi gören BLL) merkezileştirmek nasıl göreceğiz.


## <a name="introduction"></a>Giriş

Veri erişim düzeyi (DAL) oluşturulan [ilk öğreticide](creating-a-data-access-layer-vb.md) düzgün bir şekilde ayırır veri sunu mantığından mantığı erişim. Ancak, DAL, veri erişim ayrıntıları düzgün bir şekilde sunu katmanı ayırır. olsa da, uygulanabilir tüm iş kuralları uygulamaz. Örneğin, uygulamamız için size izin vermeyecek şekilde isteyebilir `CategoryID` veya `SupplierID` alanlarının `Products` ne zaman değiştirilecek tabloyu `Discontinued` alan 1 olarak ayarlayın veya Kıdem kurallarını zorunlu tutmak hangi durumlarda yasaklanması istiyoruz bir çalışan, bunlardan sonra işe biri tarafından yönetilir. Başka bir yaygın Yetkilendirme belki de yalnızca kullanıcıların belirli bir rol olarak ürünleri silebilir veya değiştirebilirsiniz senaryodur `UnitPrice` değeri.

Bu öğreticide içine bir iş mantığı katmanı (sunu katmanı ve DAL arasında veri alışverişi için bir aracı görevi gören BLL) bu iş kuralları merkezileştirmek nasıl göreceğiz. Gerçek dünya uygulamada BLL ayrı bir sınıf kitaplığı proje uygulanması gerekir; Ancak, bu öğreticileri için biz BLL sınıflarında serisi olarak uygulamak bizim `App_Code` proje yapısını basitleştirmek için klasör. Şekil 1 sunu katmanı, BLL ve DAL arasındaki mimari ilişkiler gösterilmektedir.


![BLL veri erişim katmanı sunu katmanı ayırır ve iş kuralları uygular](creating-a-business-logic-layer-vb/_static/image1.png)

**Şekil 1**: BLL sunu katmanı veri erişim katmanı ayırır ve iş kuralları uygular


Uygulamak için ayrı sınıfları oluşturmak yerine bizim [iş mantığı](http://en.wikipedia.org/wiki/Business_logic), biz alternatif olarak bu mantığı doğrudan kısmi sınıflar ile yazılan DataSet yerleştirin. Bir örnek oluşturma ve yazılmış bir veri kümesi genişletme geri ilk Öğreticisine bakın.

## <a name="step-1-creating-the-bll-classes"></a>1. adım: BLL sınıfları oluşturma

Bizim BLL oluşan her TableAdapter içinde DAL için dört sınıfları; Bu BLL sınıfların her biri, alma, ekleme, güncelleştirme ve ilgili TableAdapter uygun iş kuralları uygulama DAL içinde silmeye yöntemleri sahip olur.

Daha fazla düzgün bir şekilde için DAL ve BLL ilgili sınıflarını ayırmak, iki alt klasör oluşturalım `App_Code` klasörünü `DAL` ve `BLL`. Yalnızca sağ `App_Code` Çözüm Gezgininde klasör ve yeni bir klasör seçin. Bu iki klasör oluşturduktan sonra yazılan ilk öğreticide oluşturulan DataSet taşıma `DAL` alt klasörü.

Ardından, dört BLL sınıfı dosyalarında oluşturun `BLL` alt klasörü. Bunu başarmak için sağ `BLL` alt, yeni öğe Ekle'yi seçin ve sınıf şablonu seçin. Dört sınıf adı `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, ve `EmployeesBLL`.


![Dört yeni sınıf App_Code klasörüne eklemek](creating-a-business-logic-layer-vb/_static/image2.png)

**Şekil 2**: için dört yeni sınıflar ekleyebilirsiniz `App_Code` klasörü


Ardından, yalnızca ilk öğreticiden TableAdapters için tanımlanan yöntemler sarmalamak için sınıfların her biri için yöntemleri ekleyelim. Şimdilik, bu yöntemleri yalnızca doğrudan DAL çağıracak; gerekli iş mantığı eklemek için sonraki getireceğiz.

> [!NOTE]
> Visual Studio Standard Edition kullanıyorsanız veya üstü (diğer bir deyişle, olduğunuz *değil* Visual Web Developer kullanarak), isteğe bağlı olarak görsel olarak kullanarak sınıflarınızı tasarlayabilirsiniz [Sınıf Tasarımcısı](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Başvurmak [Sınıf Tasarımcısı Blog](https://blogs.msdn.com/classdesigner/default.aspx) Visual Studio'da bu yeni özellik hakkında daha fazla bilgi için.


İçin `ProductsBLL` sınıfı ihtiyacımız yedi yöntemleri toplam eklemek için:

- `GetProducts()` tüm ürünleri verir
- `GetProductByProductID(productID)` Belirtilen ürün kimlikli ürün döndürür
- `GetProductsByCategoryID(categoryID)` Belirtilen kategorideki tüm ürünleri verir
- `GetProductsBySupplier(supplierID)` tüm ürünleri listesinden belirtilen tedarikçi döndürür
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Yeni bir ürün değerleri kullanarak veritabanına ekler geçilen; döndürür `ProductID` yeni eklenen kaydın değeri
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` geçirilen değerlerini kullanarak veritabanında varolan bir ürün güncelleştirmeleri; döndürür `True` tam olarak bir satır güncelleştirildiyse, `False` Aksi takdirde
- `DeleteProduct(productID)` Belirtilen ürün veritabanından siler

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

Yalnızca veri döndüren yöntemler `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, ve `GetProductBySuppliersID` bunlar yalnızca DAL çağrı gibi oldukça basittir. Bazı senaryolarda olsa da uygulanması gereken iş kurallarını bu düzeyde (örneğin, yetkilendirme kuralları) şu anda oturum açmış kullanıcı veya kullanıcının ait olduğu rol göre yalnızca bu yöntemleri olarak bırakacağız-değil. Bu yöntemler, daha sonra BLL yalnızca ve sunu katmanı temel alınan veriler veri erişim katmanı erişen bir proxy görevi görür.

`AddProduct` Ve `UpdateProduct` yöntemleri hem parametre olarak çeşitli ürün alanlarına ait değerlerin olur ve yeni bir ürün ekleyin veya mevcut bir sırasıyla güncelleştirin. Birçok itibaren `Product` tablo sütunlarından kabul edebilir `NULL` değerleri (`CategoryID`, `SupplierID`, ve `UnitPrice`, birkaçıdır), bu giriş parametreleri için `AddProduct` ve `UpdateProduct` böyle bir sütun kullanımı eşlenen[boş değer atanabilir türler](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Boş değer atanabilir türler .NET 2.0 için yenidir ve bir değer türü, bunun yerine, gerekip gerekmediğini belirten olması için bir yöntem sağlayan `Nothing`. Başvurmak [Paul Vick](http://www.panopticoncentral.net/)ın blog girdisi [gerçekte hakkında boş değer atanabilir türler ve VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx) ve için teknik belgeler [null atanabilir](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx) yapısı daha fazla bilgi için.

Üçünü bir satır eklenir, güncelleştirilmiş veya işlemi etkilenen bir satırda sağlamayabilir bu yana silinmiş olup olmadığını gösteren bir Boole değeri döndürür. Örneğin, sayfa Geliştirici çağırırsa `DeleteProduct` tümleştirilmesidir bir `ProductID` mevcut olmayan ürününün `DELETE` veritabanına verilen deyimi herhangi bir etkisi olacaktır ve bu nedenle `DeleteProduct` yöntemi döndürür `False`.

Yeni bir ürün eklerken dikkat edin veya var olan bir güncelleştirme Biz yeni veya değiştirilmiş ürünün alan değerlerini kabul aksine skalerler listesi olarak ele bir `ProductsRow` örneği. Bu yaklaşım seçildi, çünkü `ProductsRow` sınıfı ADO.NET türer `DataRow` varsayılan parametresiz bir kurucusu yok sınıfı. Yeni bir oluşturmak için `ProductsRow` örneği, biz önce oluşturmanız gerekir bir `ProductsDataTable` örneği ve ardından çağırma kendi `NewProductRow()` yöntemi (hangi bunu yapmadan `AddProduct`). Biz ekleme ve ObjectDataSource kullanarak ürünleri güncelleştirmek için etkinleştirdiğinizde bu eksiklikleri kendi head rears. Kısacası, ObjectDataSource giriş parametreleri örneği oluşturmak çalışacaktır. BLL yöntemi görüyorsa bir `ProductsRow` örneği ObjectDataSource oluşturun, ancak varsayılan parametresiz bir kurucusu eksikliği nedeniyle başarısız çalışır. Bu sorunla ilgili daha fazla bilgi için aşağıdaki iki ASP.NET forumları gönderileri bakın: [güncelleştirme ObjectDataSources Strongly-Typed veri kümeleriyle](https://forums.asp.net/1098630/ShowPost.aspx), ve [sorunu ile ObjectDataSource ve Strongly-Typed DataSet](https://forums.asp.net/1048212/ShowPost.aspx).

Hem de sonraki `AddProduct` ve `UpdateProduct`, kod oluşturur bir `ProductsRow` örneği ve yalnızca geçirilen değerlerle doldurur. DataRow DataColumn nesneleri için değerler atama çeşitli alan düzeyindeki doğrulama denetimlerini ortaya çıkabilir. Bu nedenle, el ile geçirilen değerlerin bir DataRow satırının koyma BLL yönteme geçirilen verileri geçerliliğini sağlamaya yardımcı olur. Ne yazık ki kesin türü belirtilmiş DataRow Visual Studio tarafından oluşturulan sınıflar boş değer atanabilir türler kullanmayın. Bunun yerine, bir DataRow satırının belirli bir DataColumn karşılık belirtmek için bir `NULL` veritabanı gerekir kullanırız değeri `SetColumnNameNull()` yöntemi.

İçinde `UpdateProduct` biz öncelikle kullanarak güncelleştirmek için ürün yük `GetProductByProductID(productID)`. Bu veritabanı için gereksiz bir seyahat gibi görünebilir, ancak bu ek seyahat iyimser eşzamanlılık keşfedin gelecekteki eğitimlerine faydalı kanıtlamak. İyimser eşzamanlılık, aynı verileri aynı anda çalışan iki kullanıcılar, başka birinin değişiklikleri yanlışlıkla üzerine yazmayın sağlamak için kullanılan bir tekniktir. Kaydın tamamını kapmasını Ayrıca, güncelleştirme yöntemleri yalnızca bir sütun alt kümesini DataRow nesnesinin değişiklik BLL içinde oluşturmak kolaylaştırır. Ne zaman biz keşfedin `SuppliersBLL` sınıfı biz buna bir örnek göreceksiniz.

Son olarak, unutmayın `ProductsBLL` sınıfına sahip [DataObject özniteliği](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) uygulanan ( `[System.ComponentModel.DataObject]` hemen önce dosyanın en üstüne yakın class deyimi sözdizimi) ve yöntemleri [ DataObjectMethodAttribute öznitelikleri](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). `DataObject` Özniteliği işaretler bağlama için uygun bir nesne olarak sınıfı bir [ObjectDataSource Denetimi](https://msdn.microsoft.com/library/9a4kyhcx.aspx), ancak `DataObjectMethodAttribute` yöntemi amacını gösterir. Öğreticiler gelecekte anlatıldığı gibi ASP.NET 2.0'ın ObjectDataSource bildirimli olarak bir sınıftan verilere erişmek kolaylaştırır. ObjectDataSource'nın Sihirbazı'nda bağlamak için olası sınıfların listesi filtre yardımcı olmak için yalnızca olarak işaretlenmiş sınıfları varsayılan olarak `DataObjects` sihirbazın aşağı açılan listesinde gösterilir. `ProductsBLL` Sınıfı bu öznitelikler da çalışır, ancak eklemeden kolaylaştırır ObjectDataSource'nın Sihirbazı'nda çalışmak.

## <a name="adding-the-other-classes"></a>Diğer sınıf ekleme

İle `ProductsBLL` sınıfı tam, hala ihtiyacımız kategorileri, üreticiler ve çalışanlar ile çalışmaya yönelik sınıfları eklemek. Aşağıdaki sınıflar ve yöntemler Yukarıdaki örnekteki kavramları kullanarak oluşturmak için bir dakikanızı ayırın:

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

Eşitlenmeyeceği yöntemi `SuppliersBLL` sınıfının `UpdateSupplierAddress` yöntemi. Bu yöntem yalnızca tedarikçi adres bilgilerini güncelleştirmek için bir arabirim sağlar. Dahili olarak, bu yöntem okur `SupplierDataRow` için belirtilen nesne `supplierID` (kullanarak `GetSupplierBySupplierID`), adresi ilgili özelliklerini ayarlar ve içine çağırır `SupplierDataTable`'s `Update` yöntemi. `UpdateSupplierAddress` Yöntemi aşağıdadır:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

Bu makalenin indirme my tam uygulama BLL sınıfları için bakın.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>2. adım: yazılan veri kümeleri BLL sınıflar üzerinden erişme

İlk öğreticide gördüğümüz doğrudan yazılan kümesiyle program aracılığıyla çalışma örnekleri, ancak bizim BLL sınıfları eklenmesiyle, sunu katmanı karşı BLL yerine çalışması gerekir. İçinde `AllProducts.aspx` ilk öğreticide örnekten `ProductsTableAdapter` aşağıdaki kodda gösterildiği gibi bir GridView için ürünlerin listesini bağlamak için kullanılan:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

Yeni BLL kullanmaktır tüm değiştirilmesi gereken sınıflar, yalnızca ilk satır kod değiştirin `ProductsTableAdapter` nesnesi ile bir `ProductBLL` nesnesi:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

BLL sınıfları ayrıca bildirimli olarak (yazılan veri kümesi gibi) ObjectDataSource kullanılarak erişilebilir. Biz daha ayrıntılı ObjectDataSource aşağıdaki öğreticilerde ele.


[![Ürünleri listeler bir GridView görüntülenir](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**Şekil 3**: ürünleri listeler, bir GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-business-logic-layer-vb/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>3. adım: Alan düzeyindeki doğrulama DataRow sınıfları ekleme

Alan düzeyindeki doğrulama olan ekleme veya güncelleştirme sırasında iş nesnelerin özellik değerlerini ilgilidir denetler. Bazı alan düzeyindeki doğrulama kuralları ürünleri için şunları içerir:

- `ProductName` Alanı 40 karakter veya daha az olmalıdır
- `QuantityPerUnit` Alanı 20 karakter veya daha az olmalıdır
- `ProductID`, `ProductName`, Ve `Discontinued` alanları gereklidir, ancak diğer tüm alanlar isteğe bağlıdır
- `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, Ve `ReorderLevel` alanları değerinden büyük veya sıfıra eşit olmalıdır

Bu kurallar, olabilir ve veritabanı düzeyinde ifade edilmelidir. Karakter sınırını `ProductName` ve `QuantityPerUnit` alanları bu sütunların veri türleri tarafından yakalanan `Products` tablosu (`nvarchar(40)` ve `nvarchar(20)`sırasıyla). Alanları gerekli ve isteğe bağlı olup ifade edilir tarafından veritabanı tablo sütununa izin veriyorsa `NULL` s. Dört [denetim kısıtlamaları](https://msdn.microsoft.com/library/ms188258.aspx) mevcut emin yalnızca şu değerler sıfırdan büyük veya sıfıra eşit içine yapabilirsiniz `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, veya `ReorderLevel` sütun.

Bu kurallar veritabanı sırasında zorlama yanı sıra bunların ayrıca veri kümesi düzeyinde zorunlu tutulmalıdır. Aslında, alan uzunluğu ve bir değer gerekli veya isteğe bağlı olup zaten DataColumn nesneleri her DataTable nesnesinin kümesi için yakalanır. Otomatik olarak sağlanan mevcut alan düzeyindeki doğrulama görmek için veri kümesi Tasarımcısı gidin, DataTables birinden bir alan seçin ve Özellikler penceresine gidin. Şekil 4'te gösterildiği gibi `QuantityPerUnit` DataColumn `ProductsDataTable` en çok 20 karakter içeriyor ve izin vermiyor `NULL` değerleri. Biz ayarlamaya çalışırsanız `ProductsDataRow`'s `QuantityPerUnit` 20 karakterden uzun bir dize değeri özelliğine bir `ArgumentException` oluşturulur.


[![DataColumn temel alan düzeyindeki doğrulama sağlar](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**Şekil 4**: DataColumn ile sağlar temel alan düzeyindeki doğrulama ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-business-logic-layer-vb/_static/image8.png))


Ne yazık ki, biz sınırları denetimleri gibi belirtemezsiniz `UnitPrice` değeri Özellikler penceresini aracılığıyla sıfıra eşit veya daha büyük olmalıdır. Bu tür bir alan düzeyindeki doğrulama sağlamak için bir olay işleyicisi DataTable için nesnesinin oluşturmak ihtiyacımız [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) olay. Bölümünde belirtildiği gibi [önceki öğretici](creating-a-data-access-layer-vb.md), kısmi sınıflar kullanımı ile yazılan veri kümesi tarafından oluşturulan veri kümesi, DataTable ve DataRow nesneler genişletilebilir. Biz oluşturabilirsiniz Bu teknik kullanılarak bir `ColumnChanging` için olay işleyicisini `ProductsDataTable` sınıfı. Başlangıç bir sınıfta oluşturarak `App_Code` adlı klasörü `ProductsDataTable.ColumnChanging.vb`.


[![Yeni bir sınıf App_Code klasörüne ekleyin](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**Şekil 5**: yeni bir sınıf ekleyin `App_Code` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-business-logic-layer-vb/_static/image11.png))


Ardından, bir olay işleyicisi oluşturun `ColumnChanging` sağlar olay `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ve `ReorderLevel` sütun değerlerini (değilse `NULL`) değerinden büyük veya sıfıra eşit olan. Böyle bir sütunu aralık dışında ise, throw bir `ArgumentException`.

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>4. adım: Özel iş kurallarını BLL'ın sınıflara ekleme

Alan düzeyindeki doğrulama yanı sıra, farklı varlıklar veya tek bir sütun düzeyinde değil ifade kavramları gibi ilgili üst düzey özel iş kurallarını olabilir:

- Bir ürün kesilir, kendi `UnitPrice` güncelleştirilemiyor
- Çalışanın ülke ikamet ettiğiniz ikamet ettiğiniz ülke kendi manager'ın aynı olması gerekir
- Sağlayıcısı tarafından sağlanan tek ürün ise bir ürün dönüştürülmesine olamaz

Uygulamanın iş kuralları bağlılığı emin olmak için denetimleri BLL sınıfları içerir. Bu denetimler, uygulandıkları yöntemleri doğrudan eklenebilir.

Bizim iş kurallarını yalnızca ürün belirli bir sağlayıcının ise, bir ürün devam etmeyen işaretlenemez, dikte düşünün. Diğer bir deyişle, varsa ürün *X* biz satın listesinden Tedarikçi yalnızca ürün *Y*, biz değil işaretleyebilir *X* ; değilse, kullanımdan gibi ancak tedarikçi *Y*bize üç ürünleri ile sağlanan *A*, *B*, ve *C*, ardından biz herhangi işaretleyebilir ve tüm bunların devam etmez. Bir tek iş kuralı ancak iş kuralları ve sağduyunuzu her zaman hizalı değil!

Bu iş kuralı zorlamak için `UpdateProducts` biz start olmadığını denetleyerek yöntemi `Discontinued` ayarlandı `True` ve bu nedenle, biz çağırırdı varsa `GetProductsBySupplierID` kaç ürünleri belirlemek için şu listesinden bu ürünün tedarikçi satın alınmış. Yalnızca bir ürün listesinden bu tedarikçi satın biz throw bir `ApplicationException`.


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Sunu katmanındaki doğrulama hataları yanıtlama

Sunu Katmanı'ndan BLL çağrılırken, yükseltilmiş veya ASP.NET kadar Kabarcık bildirmek özel durumları işleme girişimi edilmeyeceğine karar verebilirsiniz (Yükselt `HttpApplication`'s `Error` olay). Program aracılığıyla BLL ile çalışırken, özel bir durumu işlemek için şu kullanabilirsiniz bir [deneyin... Catch](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx) aşağıdaki örnekte gösterildiği gibi engelle:


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

Gelecekte öğreticileri göreceğiz, Yukarı veri kullanırken BLL Kabarcık işleme özel durumları, güncelleştirme, ekleme, Denetim Web veya verileri silme işlenebilir kodda kaydırma gerek kalmadan aksine doğrudan bir olay işleyicisi `Try...Catch` engeller.

## <a name="summary"></a>Özet

Her biri belirli bir rol yalıtır farklı katmanlara hazırlanmış bir iyi tasarlanmış bir uygulama. Bu makale dizisinin ilk öğreticide yazılan veri kümeleri kullanarak veri erişim katmanı oluşturduğumuz; Bu öğreticide bir iş mantığı katmanı sınıfları bir dizi olarak bizim uygulamanın içinde oluşturduğumuz `App_Code` bizim DAL çağrı klasör. BLL uygulamamız için alan ve iş düzeyi mantığını uygular. Bu öğreticide, yaptığımız ayrı BLL oluşturmaya ek olarak, başka bir seçenek TableAdapters yöntemleri kısmi sınıflar kullanımı ile genişletmek için aynıdır. Ancak, bu teknik kullanılarak bize varolan yöntemleri geçersiz kılmak izin vermez veya onu bizim DAL ve bizim BLL Biz bu makalede ayırdıktan yaklaşımı olarak düzgün bir şekilde ayrı yapar.

DAL ve BLL tam biz bizim sunu katmanı başlamaya hazırsınız. İçinde [sonraki öğretici](master-pages-and-site-navigation-vb.md) şu veri erişimi konulardan kısa detour alın ve öğreticiler boyunca kullanmanız için bir tutarlı sayfa düzeni tanımlamak.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Liz Shulok, Dennis Patterson, Carlos Santos ve Hilton Giesenow yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](creating-a-data-access-layer-vb.md)
> [sonraki](master-pages-and-site-navigation-vb.md)
