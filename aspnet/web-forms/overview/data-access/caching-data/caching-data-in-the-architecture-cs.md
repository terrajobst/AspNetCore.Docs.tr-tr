---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: (C#) mimaride verileri önbelleğe alma | Microsoft Docs
author: rick-anderson
description: Önceki öğreticide biz önbelleğe alma sunu katmanı uygulama öğrendiniz. Bu öğreticide, biz müşterilerimize katmanlı architectu yararlanmak konusunda bilgi edinin...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 3971140aa7a6c829287e74df804694c19e34adcf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755364"
---
<a name="caching-data-in-the-architecture-c"></a>(C#) mimaride verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) veya [PDF olarak indirin](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> Önceki öğreticide biz önbelleğe alma sunu katmanı uygulama öğrendiniz. Bu öğreticide, biz müşterilerimize katmanlı mimarinin iş mantığı katmanı önbellek verilere nasıl yararlanabileceğinizi öğrenin. Bu mimari bir önbellek katmanı içerecek şekilde genişleterek desteklemiyoruz.


## <a name="introduction"></a>Giriş

Önceki öğreticide gördüğümüz gibi ObjectDataSource s verileri önbelleğe alma özellikleri birkaç ayarı olarak kadar kolaydır. Ne yazık ki, önbelleğe alma ilkelerini ASP.NET sayfası ile sıkı bir şekilde couples sunu katmanında önbelleğe alma ObjectDataSource geçerlidir. Katmanlı bir mimari oluşturmak için nedenlerden biri dolayısıyla bozuk tür couplings izin vermektir. Veri erişim katmanı veri erişim ayrıntılarını ayırır ancak iş mantığı katmanı örneği için ASP.NET sayfaları'ndan iş mantığı ayırır. Daha okunabilir, daha sürdürülebilir ve değiştirmek için daha esnek sistem yapar bu iş mantığı ve verileri erişim ayrıntılarını ayırma parçası olarak, tercih edilen, olmasıdır. Ayrıca etki alanı bilgisini ve sunu katmanı eklenmemişse t üzerinde çalışan bir geliştirici kendi işi yapmak için veritabanı s ayrıntıları ile ilgili bilgi sahibi olması gerekir işçilik bölümü olanak sağlar. Sunu katmanı önbellek ilkeden ayırma benzer avantajlar sunar.

Bu öğreticide size içerecek şekilde mimarimiz genişletecektir bir *önbelleğe alma katman* (veya kısaca CL), önbelleğe alma ilkemiz kullanır. Önbellek katmanı içerecektir bir `ProductsCL` gibi yöntemleri ile ürün bilgilerine erişim sağlar sınıfını `GetProducts()`, `GetProductsByCategoryID(categoryID)`, vb., çağrıldığında, önbellekten veri almak için ilk girişim olacak. Önbellek boşsa, bu yöntemler uygun çağırma `ProductsBLL` veri karşılık DAL elde edebileceğiniz BLL yöntemi. `ProductsCL` Yöntemleri döndürmeden önce BLL alınan verileri önbelleğe alın.

Şekil 1 gösterildiği gibi CL sunu ve iş mantığı katmanları arasında yer alıyor.


![Önbelleğe alma katman (CI) başka bir katman Mız mimarisinde ise](caching-data-in-the-architecture-cs/_static/image1.png)

**Şekil 1**: önbelleğe alma katman (CI) olan başka bir katmanda Mız mimarisi


## <a name="step-1-creating-the-caching-layer-classes"></a>1. adım: önbelleğe alma katman sınıfları oluşturma

Bu öğreticide basit bir CI ile tek bir sınıf oluşturacağız `ProductsCL` yöntemleri olması sahip. Tüm uygulama oluşturmayı gerektirecek için tam bir önbellek katmanı oluşturma `CategoriesCL`, `EmployeesCL`, ve `SuppliersCL` sınıflar ve BLL her veri erişim veya değiştirilmesi yöntemi için bu önbelleğe alma katman sınıflardaki bir yöntem sağlar. BLL ve DAL gibi önbelleğe alma katman ideal olarak ayrı bir sınıf kitaplığı projesi olarak uygulanmalıdır; ancak biz bunu bir sınıfta olarak uygulayacak `App_Code` klasör.

DAL ve BLL sınıflardan daha fazla indrebilirsiniz ayrı CL sınıfları için let s, yeni bir alt klasör oluşturmak `App_Code` klasör. Sağ `App_Code` Çözüm Gezgini'nde klasörü yeni bir klasör seçin ve yeni klasör adı `CL`. Bu klasör oluşturduktan sonra ona adlı yeni bir sınıf ekleyin `ProductsCL.cs`.


![CL adlı yeni bir klasör ve ProductsCL.cs adlı bir sınıf ekleyin](caching-data-in-the-architecture-cs/_static/image2.png)

**Şekil 2**: adlı yeni bir klasör eklemek `CL` ve adlı bir sınıf `ProductsCL.cs`


`ProductsCL` Sınıfı, karşılık gelen iş mantığı katmanı sınıfıyla bulunan veri erişim ve değişiklik yöntemleri aynı dizi içermelidir (`ProductsBLL`). Bu yöntemlerin tümü, let s yalnızca derleme desenleri için bir genel görünüm almak için birkaç burada kullanılan CL tarafından oluşturmak yerine. Özellikle, ekleyeceğiz `GetProducts()` ve `GetProductsByCategoryID(categoryID)` adım 3'te yöntemleri ve `UpdateProduct` aşırı 4. adım. Kalan ekleyebilirsiniz `ProductsCL` yöntemleri ve `CategoriesCL`, `EmployeesCL`, ve `SuppliersCL` zamanınızda sınıfları.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>2. adım: Okuma ve yazma veri önbelleğine

Önbelleğe alma özelliği önceki öğreticide dahili olarak araştırılan ObjectDataSource BLL alınan verileri depolamak için ASP.NET veri önbelleğini kullanır. Veri önbelleğini de programlı olarak ASP.NET sayfaları arka plan kod sınıflardan veya web uygulaması s mimarisi sınıflardan erişilebilir. Veri önbelleği için ASP.NET sayfası s arka plan kod sınıfı okuyup için şu biçimi kullanın:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache` Sınıfı](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` yöntemi](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) aşırı yüklemeleri vardır. `Cache["key"] = value` ve `Cache.Insert(key, value)` eşanlamlıdır ve her ikisi de tanımlanan bir süre sonu olmadan belirtilen anahtarı kullanarak önbelleğe bir öğe ekleyin. Genellikle, öğenin önbelleğe almak için bir bağımlılık, zamana bağlı süre sonu veya her ikisi de olarak eklerken bir süre sonu belirtmek istiyoruz. Diğer birini kullanın `Insert` s yöntemi aşırı yüklemeleri, bağımlılık veya zamana bağlı süre sonu bilgi sağlamak için.

İstenen veriler önbellekte değilse ve bu durumda, ilk denetlenecek s yöntemlere ihtiyaç önbelleğe alma katman buradan döndürür. İstenen veriler önbellekte değilse, uygun BLL yöntemin çağrılması gerekir. Dönüş değerinin önbelleğe alınır ve döndürülen, aşağıdaki sıralı diyagramda gösterildiği gibi.


![Önbellek katmanı s yöntemler, bu verileri önbellekten döndürür, s kullanılabilir](caching-data-in-the-architecture-cs/_static/image3.png)

**Şekil 3**: önbellek katmanı s yöntemler, bu verileri önbellekten döndürür, s kullanılabilir


Şekil 3'te gösterilen dizisi CL sınıflarda aşağıdaki deseni kullanılarak gerçekleştirilir:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

Burada, *türü* önbellekte depolanmakta olan veri türü `Northwind.ProductsDataTable`, örneğin *anahtar* önbellek öğesinin benzersiz olarak tanımlayan anahtar. Varsa belirtilen öğeyle *anahtarı* önbellekte sonra değil *örneği* olacaktır `null` ve verileri uygun BLL yöntemden alınır ve önbelleğe eklenir. Zamana göre `return instance` ulaşıldığında *örneği* BLL oluşan bir derleme veya verileri önbellekten ya da bir başvuru içeriyor.

Yukarıdaki desen verileri önbellekten erişirken kullanılacak emin olun. İlk bakışta, eşdeğer görünüyor, deseni, bir yarış durumu tanıtan bir fark içerir. Yarış durumları çünkü bunlar, kendilerini tutularak açığa çıkar ve yeniden oluşturulması zor olan hata ayıklamak zordur.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

Bu ikinci fark, yanlış kod parçacığı, yerine önbelleğe alınan öğeyi başvuru yerel bir değişkende depolanması, veri önbelleğini koşullu deyimde doğrudan erişilir *ve* içinde `return`. Bu kod ulaşıldığında, Imagine `Cache["key"]` olan olmayan`null`, ancak önce `return` deyimine ulaşıldığında, sistem çıkarır *anahtar* önbellekten. Nadir bu durumda, kod döndüreceği bir `null` değer beklenen türde bir nesne yerine.

> [!NOTE]
> Veri önbelleğini t basit okuma ve yazma için iş parçacığı erişimi eşitlemeniz gerekir ki iş parçacığı açısından güvenli olduğundan. Ancak önbellekte atomik olması gereken veriler üzerinde birden fazla işlem yapmanız gereken, bir kilitleme veya iş parçacığı güvenliği sağlamak için diğer bazı mekanizması uygulamak için sorumlu olursunuz. Bkz: [ASP.NET önbelleğe erişimi eşitleme](http://www.ddj.com/184406369) daha fazla bilgi için.


Bir öğe program aracılığıyla kullanarak veri önbelleği çıkartılabilir [ `Remove` yöntemi](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) şu şekilde:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>3. adım: Ürün bilgileri döndüren`ProductsCL`sınıfı

Bu öğreticide, ürün bilgilerini döndürmek için iki yöntem uygulamak s denetlemesine izin vermek için `ProductsCL` sınıfı: `GetProducts()` ve `GetProductsByCategoryID(categoryID)`. İle gibi `ProductsBL` iş mantığı katmanı sınıfında `GetProducts()` CL yönteminde, tüm ürünleri ilgili bilgileri döndürür bir `Northwind.ProductsDataTable` nesnesi sırada `GetProductsByCategoryID(categoryID)` belirtilen bir kategorideki tüm ürünleri döndürür.

Aşağıdaki kod yöntemleri bir bölümü gösterilmektedir `ProductsCL` sınıfı:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

İlk olarak, Not `DataObject` ve `DataObjectMethodAttribute` sınıf ve yöntemler için uygulanan öznitelikleri. Bu öznitelikler, hangi sınıflar ve yöntemler Sihirbazı s adımlarda görünmelidir belirten ObjectDataSource s Sihirbazı bilgi sağlar. Sunu katmanındaki bir ObjectDataSource gelen CL sınıflar ve yöntemler erişilecek olduğundan, tasarım zamanı deneyimi geliştirmek için bu öznitelikler ekledim. Kiracıurl [iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-cs.md) bu öznitelikler ve bunların etkileri hakkında daha kapsamlı bir açıklama için öğretici.

İçinde `GetProducts()` ve `GetProductsByCategoryID(categoryID)` yöntemleri, döndürülen veriler `GetCacheItem(key)` yöntemi, yerel bir değişkene atanır. `GetCacheItem(key)` Kısa süre içinde inceleyeceğiz, yöntem, belirli bir öğe belirtilen temel önbellekten döndürür *anahtar*. Böyle bir veri önbellekte bulunamazsa, ilgili alınır `ProductsBLL` sınıfı yöntemi ve ardından önbelleği kullanmaya eklenen `AddCacheItem(key, value)` yöntemi.

`GetCacheItem(key)` Ve `AddCacheItem(key, value)` arabirim yöntemleri veri önbelleği, okuma ve yazma değerler, sırasıyla. `GetCacheItem(key)` Yöntemidir iki basit. Yalnızca geçirilen bileşenini kullanarak önbellek sınıfından bir değer döndürür *anahtar*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` kullanmaz *anahtarı* değer sağladığı gibi ancak bunun yerine çağrı `GetCacheKey(key)` döndüren yöntemi *anahtarı* ProductsCache - ile etkileşimlidir. `MasterCacheKeyArray`, ProductsCache, dize tutan ayrıca tarafından kullanılıyor `AddCacheItem(key, value)` yöntemi, kısa bir süre içinde anlatıldığı gibi.

Bir ASP.NET sayfasında s arka plan kod sınıfı kullanılarak veri önbelleğini erişilebilir `Page` s sınıfı [ `Cache` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)ve gibi bir söz dizimi sağlar `Cache["key"] = value`, 2. adımda açıklandığı gibi. Öğesinden bir sınıf içinde mimari kullanarak veri önbelleğini erişilebilir `HttpRuntime.Cache` veya `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)ın blog girişine [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) kullanarak küçük bir performans avantajı notları `HttpRuntime` yerine `HttpContext.Current`; sonuç olarak, `ProductsCL` kullanan `HttpRuntime`.

> [!NOTE]
> Sınıf kitaplığı projeleri kullanarak Mimarinizi uygulanan sonra bir başvuru eklemeniz gerekecektir `System.Web` kullanmak için derleme [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) ve [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) sınıfları.


Öğeyi önbellekte bulunamazsa `ProductsCL` s sınıfı yöntemleri veri BLL Al ve önbelleği kullanmaya ekleme `AddCacheItem(key, value)` yöntemi. Eklenecek *değer* önbelleğe 60 saniye süre sonu kullanan aşağıdaki kodu kullanabiliriz:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` zamana bağlı süre sonu gelecekteki while 60 saniye belirtir [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) hiçbir olmaadığını olmadığını s gösterir. Bu `Insert` yöntemi aşırı yüklemesi, hem bir mutlak parametrelerini giriş vardır ve süre sonu kayan, yalnızca iki birini sağlayabilirsiniz. Mutlak bir zaman ve bir zaman aralığı belirtmek çalışırsanız `Insert` yöntemi oluşturur bir `ArgumentException` özel durum.

> [!NOTE]
> Bu uygulaması `AddCacheItem(key, value)` yöntemi şu anda bazı eksiklikleri vardır. Biz adres ve adım 4'te bu sorunlarının üstesinden.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>4. adım: önbellek, verileri geçersiz kılmalarını değiştiren aracılığıyla mimaridir

Veri alma yöntemi ile birlikte, önbelleğe alma katman ekleme, güncelleştirme ve verileri silme olarak BLL aynı yöntemleri sağlaması gerekir. CL s veri değişikliği yöntemi önbelleğe alınan verilerin değiştirmeyin ancak yerine BLL s karşılık gelen veri değişikliği yöntemini çağırın ve ardından önbelleği geçersiz kılar. Önceki öğreticide gördüğümüz gibi önbelleğe alma özellikleri etkinleştirildiğinde ObjectDataSource uygulayan aynı davranış budur ve kendi `Insert`, `Update`, veya `Delete` yöntemi çağrılır.

Aşağıdaki `UpdateProduct` aşırı yükleme CL veri değişikliği yöntemleri uygulamak nasıl gösterilmektedir:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Uygun veri değişikliği iş mantığı katmanı yöntemi çağrılır ancak yanıtına döndürülmeden önce önbellek geçersiz kılmak üzere oluşturmamız gerekir. Ne yazık ki, önbelleğe geçersiz kılmalarını açık olduğundan değil `ProductsCL` s sınıfı `GetProducts()` ve `GetProductsByCategoryID(categoryID)` her yöntemler öğeleri önbelleğe farklı anahtarlarla ve `GetProductsByCategoryID(categoryID)` yöntemi her biri için farklı bir önbellek öğesi ekler benzersiz *CategoryID*.

Önbellek geçersiz kılmalarını, kaldırmak ihtiyacımız *tüm* tarafından eklenmiş olabilir öğeleri `ProductsCL` sınıfı. Bu ilişkilendirerek gerçekleştirilebilir bir *önbelleğe bağımlılık* önbelleğe eklenen her bir öğesi ile `AddCacheItem(key, value)` yöntemi. Genel olarak, bir önbellek bağımlılık önbellek bir dosya dosya sisteminde veya Microsoft SQL Server veritabanından veri çubuğunda, başka bir öğe olabilir. Olduğunda bağımlılık değiştirir veya önbellekten kaldırıldı, ilişkili olduğu önbellek öğeleri otomatik olarak önbellekten çıkarılan. Bu öğreticide, bir şeye eklenen tüm öğeler için bir önbellek bağımlılık olarak hizmet veren önbelleğinde oluşturmak istiyoruz `ProductsCL` sınıfı. Böylece, tüm bu öğeleri önbellekten önbellek bağımlılığını kaldırarak kaldırılabilir.

Let s güncelleştirme `AddCacheItem(key, value)` yöntemi her öğesi, bu yöntem kullanılarak önbelleğe eklenir, böylelikle bir tek önbellek bağımlılıkla ilişkili:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` tek bir değer olarak ProductsCache tutan bir dize dizisidir. İlk olarak, bir önbellek öğesi önbelleğe eklenir ve geçerli tarih ve saat atanmış. Önbellek öğesi zaten varsa güncelleştirilir. Ardından, bir önbellek bağımlılık oluşturulur. [ `CacheDependency` Sınıfı](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s oluşturucu aşırı yüklemeleri sayısı var, ancak burada kullanılan bir iki bekliyor `string` giriş dizisi. İlk bağımlılıkları kullanılacak dosya kümesini belirtir. Biz ki bu yana t değeri tüm dosya tabanlı bağımlılıkları, kullanmak istediğiniz `null` ilk giriş parametresi için kullanılır. İkinci giriş parametresi bağımlılıklarını kullanacak şekilde önbellek anahtar kümesini belirtir. Burada tek bizim bağımlılık belirttiğimiz `MasterCacheKeyArray`. `CacheDependency` Ardından yöntemlere geçirilen `Insert` yöntemi.

Bu değişiklik ile `AddCacheItem(key, value)`, invaliding bağımlılık kaldırma olarak basit bir önbellektir.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>5. adım: önbellek katmanı, sunu katmanı çağırma

Çalışmak için teknikleri kullanarak verilerle biz Bu öğretici incelenir ve önbelleğe alma katman s sınıflar ve yöntemler kullanılabilir. Önbelleğe alınmış veri ile çalışma göstermek için yaptığınız değişiklikleri kaydetmek `ProductsCL` sınıfı ve ardından açın `FromTheArchitecture.aspx` sayfasını `Caching` klasörü ve GridView ekleyin. GridView s akıllı etiketten yeni ObjectDataSource oluşturun. Sihirbaz s ilk adımda görmelisiniz `ProductsCL` açılır liste seçeneklerinden biri olarak sınıf.


[![İş nesnesi aşağı açılan listesinde bulunan ProductsCL sınıfı](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Şekil 4**: `ProductsCL` sınıf iş nesnesi aşağı açılan listesinde yer almaktadır ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-in-the-architecture-cs/_static/image6.png))


Seçtikten sonra `ProductsCL`, İleri'ye tıklayın. İki öğe - seçme sekmesinde açılır listede olan `GetProducts()` ve `GetProductsByCategoryID(categoryID)` ve yalnızca güncelleştirme sekmesi vardır `UpdateProduct` aşırı yükleme. Seçin `GetProducts()` yöntemi seçme sekmesinden ve `UpdateProducts` yöntemi UPDATE sekmesi ve son.


[![S ProductsCL sınıfı yöntemleri aşağı açılan listesi içinde listelenir](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Şekil 5**: `ProductsCL` s sınıfı yöntemleri, aşağı açılan listesi içinde listelenir ([tam boyutlu görüntüyü görmek için tıklatın](caching-data-in-the-architecture-cs/_static/image9.png))


Sihirbazı tamamladıktan sonra Visual Studio ObjectDataSource s ayarlayacak `OldValuesParameterFormatString` özelliğini `original_{0}` GridView'a uygun alanları ekleyin. Değişiklik `OldValuesParameterFormatString` özelliği varsayılan değerine geri dön `{0}`ve GridView düzenlemeyi sayfalama ve sıralama destekleyecek şekilde yapılandırın. Bu yana `UploadProducts` CL tarafından kullanılan aşırı yükleme, yalnızca s düzenlenen ürün adı ve fiyat, böylece bu alanlar yalnızca düzenlenebilir GridView sınırlamak kabul eder.

Önceki öğreticide tanımladığımız için alanları içerecek şekilde GridView `ProductName`, `CategoryName`, ve `UnitPrice` alanları. Bu biçimlendirme ve yapı çoğaltma çekinmeyin, bu, GridView ve ObjectDataSource s bildirim temelli durumda biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

Bu noktada önbelleğe alma katmanı kullanan bir sayfa sahibiz. Eylem önbellekte görmek için kesme noktaları ayarlayın `ProductsCL` s sınıfı `GetProducts()` ve `UpdateProduct` yöntemleri. Sıralama yaparken bir tarayıcı ve kodu adımlayın sayfasını ziyaret edin ve önbellekten verileri görmek için disk belleği çekilir. Ardından bir kaydı güncelleştirme ve önbellekte geçersiz kılınan ve sonuç olarak, veri, GridView'a DataSet'e yüklediğinizde, BLL alınıp dikkat edin.

> [!NOTE]
> Bu makalede eşlik eden indirmesinde sağlanan önbellek katmanı tamamlanmadı. Yalnızca bir sınıf içeren `ProductsCL`, hangi yalnızca Spor birkaç yöntemleri. Üstelik, yalnızca tek bir ASP.NET sayfası CL kullanır (`~/Caching/FromTheArchitecture.aspx`) diğer tüm hala BLL doğrudan başvuru. Uygulamanızda bir CL kullanmayı planlıyorsanız, sunu katmanındaki tüm çağrıların CL s sınıfları, gerektirecek CL gitmesi gereken ve bu sınıflar ve yöntemler, sunu katmanı tarafından şu anda kullanılan BLL yöntemleri ele.


## <a name="summary"></a>Özet

Önbelleğe alma ASP.NET 2.0 s SqlDataSource içeren sunu katmanı ve ObjectDataSource denetimleri uygulanırken, ideal olarak sorumlulukları önbelleğe alma için ayrı bir katman mimarisinde temsilci. Bu öğreticide bir önbelleğe alma iş mantığı katmanı ve bir sunu katmanı arasında yer alan katman oluşturduk. Önbellek katmanı, sınıflar ve BLL içinde mevcut ve bir sunu katmanı olarak adlandırılır yöntemleri aynı kümesi sağlamak gerekir.

Bu ve önceki öğreticiler size incelediniz önbelleğe alma katman örnekleri sergilenen *reaktif yükleme*. Reaktif yükleyerek, veriler yalnızca veriler için bir istek yapıldığında ve bu verileri önbellekten eksik önbelleğine yüklenir. Veri de olabilir *proaktif olarak yüklenen* önbelleğine bir teknik, yükler verileri önbelleğe gerçekten gerekli önce. Sonraki öğreticide uygulama başlangıcında önbelleğe statik değerleri depolamak nasıl baktığınızda proaktif yükleme örneği göreceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murph oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](caching-data-with-the-objectdatasource-cs.md)
> [İleri](caching-data-at-application-startup-cs.md)
