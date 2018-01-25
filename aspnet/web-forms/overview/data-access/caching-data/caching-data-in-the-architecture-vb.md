---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: "(VB) mimarisinde verileri önbelleğe alma | Microsoft Docs"
author: rick-anderson
description: "Önceki öğreticide sunu katmanında önbelleğe alma uygulamak nasıl öğrendiniz. Bu öğreticide bizim katmanlı architectu yararlanmak nasıl öğrenin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aca89b022bb3bb7e4154ab575b5bb5513144cd5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-the-architecture-vb"></a>(VB) mimarisinde verileri önbelleğe alma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) veya [PDF indirin](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> Önceki öğreticide sunu katmanında önbelleğe alma uygulamak nasıl öğrendiniz. Bu öğreticide iş mantığı katmanı önbellek verileri için katmanlı mimarimizin avantajlarından yararlanmak nasıl öğrenin. Biz önbelleğe alma katman içerecek şekilde mimarisi genişleterek yapın.


## <a name="introduction"></a>Giriş

Önceki öğreticide gördüğümüz gibi ObjectDataSource s verileri önbelleğe alma özelliklerini birkaç ayarı olarak kadar basittir. Ne yazık ki, sıkı bir şekilde önbelleğe alma ilkelerini ASP.NET sayfasıyla couples sunu katmanında önbelleğe alma ObjectDataSource geçerlidir. Katmanlı bir mimari oluşturmak için nedenlerden biri dolayısıyla kesilmesi böyle couplings izin vermektir. Veri erişim katmanı veri erişim ayrıntıları ayrıştırır sırada iş mantığı katmanı örneği için ASP.NET sayfaları iş mantığından ayrıştırır. Sistem daha okunabilir, daha rahat ve değiştirmek için daha esnek kolaylaştırır bu iş mantığı ve verileri erişim ayrıntılarını kesilmesi parçası olarak, tercih edilen, çünkü. Ayrıca etki alanı bilgi ve sunu katmanı içermiyor t üzerinde çalışan bir geliştirici kendi iş yapmak için veritabanı s ayrıntıları ile ilgili bilgi sahibi olmanız gereken işgücü bölümünün olanak sağlar. Sunu katmanı önbelleğe alma ilkesinden kesilmesi benzer avantajları sunar.

Bu öğreticide biz içerecek şekilde mimarimizin büyütmek bir *önbelleğe alma katman* (veya kısaca CL) önbelleğe alma ilkemizi kullanır. Önbelleğe alma katman içerecek bir `ProductsCL` gibi yöntemleriyle ürün bilgilerine erişim sağlayan sınıf `GetProducts()`, `GetProductsByCategoryID(categoryID)`, vb., çağrıldığında, önbellekten veri almak için ilk deneme olur. Önbellek boşsa, bu yöntemleri uygun çağırma `ProductsBLL` veri DAL sırayla elde edebileceğiniz BLL yöntemi. `ProductsCL` Yöntemleri önbelleğe döndürmeden önce BLL alınan veri.

Şekil 1'de görüldüğü gibi CL sunu ve iş mantığı katmanlar arasında yer alıyor.


![Önbelleğe alma katman (CL) Mız mimarisinde başka bir katmanıdır](caching-data-in-the-architecture-vb/_static/image1.png)

**Şekil 1**: önbelleğe alma katman (CL) olan başka bir katmanında Mız mimarisi


## <a name="step-1-creating-the-caching-layer-classes"></a>1. adım: önbelleğe alma katman sınıfları oluşturma

Bu öğreticide çok basit bir CL ile tek bir sınıf oluşturacağız `ProductsCL` yöntemleri sayıda sahip. Tüm uygulama oluşturma gerektirecek için tam bir önbelleğe alma katmanı oluşturma `CategoriesCL`, `EmployeesCL`, ve `SuppliersCL` sınıfları ve BLL her veri erişimi veya değiştirilmesi yöntem için bu önbelleğe alma katman sınıflardaki bir yöntem sağlar. BLL ve DAL gibi önbelleğe alma katman ideal ayrı bir sınıf kitaplığı proje uygulanmalıdır; Ancak, biz onu bir sınıf olarak uygulayacak `App_Code` klasör.

DAL ve BLL sınıflardan daha fazla düzgün bir şekilde ayrı CL sınıflarına olanak sağlayan s oluşturma yeni bir alt `App_Code` klasör. Sağ `App_Code` Çözüm Gezgininde klasör yeni bir klasör seçin ve yeni bir klasör adı `CL`. Bu klasör oluşturduktan sonra adlı yeni bir sınıf ekleyin `ProductsCL.vb`.


![CL adlı yeni bir klasör ve ProductsCL.vb adlı bir sınıf ekleyin](caching-data-in-the-architecture-vb/_static/image2.png)

**Şekil 2**: adlı yeni bir klasör ekleyin `CL` ve adlı bir sınıf`ProductsCL.vb`


`ProductsCL` Sınıfı içinde karşılık gelen iş mantığı katmanı sınıfındaki bulunan veri erişimi ve değiştirme yöntemleri aynı kümesini içermelidir (`ProductsBLL`). Bu yöntemlerin let s yalnızca derleme için desenleri bir fikir almak için birkaç burada kullanılan CL tarafından oluşturmak yerine. Özellikle, ekleyeceğiz `GetProducts()` ve `GetProductsByCategoryID(categoryID)` adım 3'te yöntemleri ve bir `UpdateProduct` adım 4'te aşırı yükleme. Kalan ekleyebilirsiniz `ProductsCL` yöntemleri ve `CategoriesCL`, `EmployeesCL`, ve `SuppliersCL` zamanınızda sınıfları.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>2. adım: Okuma ve veri önbelleğine yazma

Önceki öğreticide dahili olarak keşfedilen özellik önbelleğe alma ObjectDataSource ASP.NET veri önbelleği BLL alınan verileri depolamak için kullanır. Veri önbelleği de programlı olarak ASP.NET sayfaları arka plan kodu sınıflardan veya web uygulaması s mimarisinde sınıflardan erişilebilir. Okumak ve veri önbelleği için ASP.NET sayfası s arka plandaki kod sınıfı yazmak için şu biçimi kullanın:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

[ `Cache` Sınıfı](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` yöntemi](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) aşırı sayısı. `Cache("key") = value`ve `Cache.Insert(key, value)` eşanlamlıdır ve her ikisi de tanımlı bir süre sonu olmadan belirtilen anahtarı kullanarak önbelleğe bir öğe ekleyin. Genellikle, bir öğe için önbellek, bir bağımlılık, zamana bağlı süre sonu veya her ikisini de olarak eklerken bir süre sonu belirtmek istiyoruz. Diğer birini kullanın `Insert` s yöntemi aşırı bağımlılık veya zamana bağlı süre sonu bilgileri sağlayın.

Önbelleğe alma s yöntemleri istenen veri önbellekte ise ve bu durumda, ilk kontrol etmeniz katman buradan döndür. İstenen veri önbellekte değilse, uygun BLL yöntemin çağrılması gerekir. Dönüş değerini önbelleğe ve döndürülen, aşağıdaki dizisi diyagramda gösterildiği gibi.


![Önbelleğe alma katman s yöntemleri, bu veri önbellekten döndürür, s kullanılabilir](caching-data-in-the-architecture-vb/_static/image3.png)

**Şekil 3**: önbelleğe alma katman s yöntemleri, bu veri önbellekten döndürür, s kullanılabilir


Şekil 3'te gösterilen dizisi şu biçimi kullanarak CL sınıflarda gerçekleştirilir:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Burada, *türü* önbellekte saklanmasını veri türünde `Northwind.ProductsDataTable`, örneğin *anahtar* önbellek öğesinin benzersiz olarak tanıtan anahtarıdır. Varsa belirtilen bir öğesiyle *anahtar* önbelleğinde sonra değil *örneği* olacaktır `Nothing` ve verileri uygun BLL yönteminden alınır ve önbelleğe eklenir. Zamana göre `Return instance` ulaşıldığında *örneği* veri önbelleğinden ya da bir başvuru içeriyor veya BLL çekilen.

Yukarıdaki düzeni önbellekten verilere erişilirken kullandığınızdan emin olun. İlk bakışta, eşdeğer görünüyor, aşağıdaki deseni bir yarış durumu tanıtan bir fark içerir. Yarış durumları, çünkü bunlar kendilerini zaman zaman ortaya ve yeniden oluşturması zor olan hata ayıklama zordur.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Bu ikinci fark, hatalı kod parçacığında, yerine önbelleğe alınan öğe başvuru yerel bir değişkende depolama, veri önbelleği koşullu deyimde doğrudan erişilen *ve* içinde `Return`. Bu kod ulaşıldığında, düşünün `Cache("key")` değil `Nothing`, ancak önce `Return` deyimi ulaşıldığında, sistem çıkarır *anahtar* önbellekten. Bu nadir durumda kodu döndürülecek `Nothing` yerine beklenen türde bir nesne.

> [!NOTE]
> Veri önbelleği iş parçacığı açısından güvenli olduğundan, basit okuma veya yazma işlemleri için iş parçacığı erişimini eşitlemek gerek yoktur. Atomik olmasına gerek önbellekte verileri birden çok işlemleri ihtiyacınız varsa, ancak, siz kilit veya iş parçacığı güvenliği sağlamak için başka bir düzenek uygulamak için sorumlu olursunuz. Bkz: [ASP.NET önbelleğe erişim eşitleme](http://www.ddj.com/184406369) daha fazla bilgi için.


Bir öğe programlı olarak kullanarak veri önbelleği çıkarılmasına [ `Remove` yöntemi](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) sözlüğüdür:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>3. adım: Ürün bilgilerini döndürme`ProductsCL`sınıfı

Bu öğretici ürün bilgileri döndürmek için iki yöntem uygulamak s denetlemesine izin vermek için `ProductsCL` sınıfı: `GetProducts()` ve `GetProductsByCategoryID(categoryID)`. İle gibi `ProductsBL` iş mantığı katmanı sınıfında `GetProducts()` CL yöntemi ürünlerin tamamı hakkında bilgi döndürür bir `Northwind.ProductsDataTable` nesnesi sırada `GetProductsByCategoryID(categoryID)` belirtilen bir kategorideki tüm ürünleri döndürür.

Aşağıdaki kod yöntemleri bir kısmı gösterir `ProductsCL` sınıfı:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

İlk olarak, Not `DataObject` ve `DataObjectMethodAttribute` sınıfı ve yöntemleri uygulanan öznitelikleri. Bu öznitelikler ne sınıflar ve yöntemler Sihirbazı s adımlarda görünmelidir belirten ObjectDataSource s Sihirbazına bilgi sağlar. Sunu katmanındaki bir ObjectDataSource gelen CL sınıflar ve yöntemler erişileceği olduğundan, bu öznitelikler tasarım zamanı deneyimini geliştirmek için eklendi. Geri başvurmak [bir iş mantığı katmanı oluşturma](../introduction/creating-a-business-logic-layer-vb.md) bu öznitelikler ve etkilerini daha kapsamlı bir açıklama için Öğreticisi.

İçinde `GetProducts()` ve `GetProductsByCategoryID(categoryID)` yöntemleri, döndürülen veriler `GetCacheItem(key)` yöntemi, yerel bir değişkene atanır. `GetCacheItem(key)` Size kısa süre içinde inceleyeceğiz, yöntem, belirtilen temel önbelleğinden belirli bir öğe döndürür *anahtar*. Böyle bir veri önbelleği'nde bulunursa, ilgili alınır `ProductsBLL` sınıf yöntemi ve önbellek kullanmaya eklenen `AddCacheItem(key, value)` yöntemi.

`GetCacheItem(key)` Ve `AddCacheItem(key, value)` arabirim yöntemleri veri önbelleği, okuma ve yazma değerler, sırasıyla. `GetCacheItem(key)` Yöntem basittir iki. Yalnızca geçirilen bileşenini kullanarak önbellek sınıfından değeri döndürür *anahtar*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)`kullanmayan *anahtar* değer sağlanan gibi ancak bunun yerine çağrıları `GetCacheKey(key)` döndürür yöntemi *anahtar* $a ProductsCache - ile. `MasterCacheKeyArray`, ProductsCache, dize tutan de kullanıldığında tarafından `AddCacheItem(key, value)` yöntemi, kısa bir süre içinde anlatıldığı gibi.

Bir ASP.NET sayfası s arka plandaki kod sınıfı kullanılarak veri önbelleği erişilebilir `Page` s sınıfı [ `Cache` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)ve benzer bir sözdizimi sağlar `Cache("key") = value`, 2. adımda açıklandığı gibi. Öğesinden bir sınıf mimarisi içinde veri önbelleği ya da kullanılarak erişilebilir `HttpRuntime.Cache` veya `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)ın blog girdisi [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) kullanarak küçük bir performans avantajı Notlar `HttpRuntime` yerine `HttpContext.Current`; sonuç olarak, `ProductsCL` kullanan `HttpRuntime`.

> [!NOTE]
> Sınıf Kitaplığı projelerinde kullanılarak Mimarinizi uygulanan sonra bir başvuru eklemeniz gerekir `System.Web` kullanmak için derleme [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) ve [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) sınıflar.


Öğe önbellekte bulunmazsa `ProductsCL` s sınıfı yöntemleri BLL Veri Al ve önbelleği kullanmaya eklemek `AddCacheItem(key, value)` yöntemi. Eklemek için *değeri* önbelleğe biz 60 saniye süre süre sonu kullanan aşağıdaki kodu kullanabilirsiniz:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)`zamana bağlı süre sonu 60 saniye gelecekteki while belirtir [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) Kayan süre sonu olmadığını s gösterir. Bu `Insert` yöntemi aşırı yüklemesini hem bir mutlak parametrelerini giriş ve bitiş kayan, yalnızca iki birini sağlayabilirsiniz. Mutlak bir zaman ve bir zaman aralığı belirtmek çalışırsanız `Insert` yöntemi oluşturur bir `ArgumentException` özel durum.

> [!NOTE]
> Bu uygulaması, `AddCacheItem(key, value)` yöntemi şu anda bazı eksik yok. Biz adres ve adım 4'te bu sorunlarının üstesinden.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>4. adım: önbellek zaman verileri geçersiz kılmalarını değiştiren aracılığıyla mimarisidir

Veri alma yöntemleri yanı sıra, önbelleğe alma katmanı ekleme, güncelleştirme ve verileri silme için aynı yöntemleri BLL olarak sağlaması gerekir. CL s veri değişikliği yöntemleri önbelleğe alınmış verileri değiştiremez, ancak yerine BLL s karşılık gelen veri değişikliği yöntemini çağırın ve önbelleği geçersiz kılar. Önceki öğreticide gördüğümüz gibi ObjectDataSource önbelleğe alma özelliklerini etkinleştirildiğinde uygulayan aynı davranış budur ve kendi `Insert`, `Update`, veya `Delete` yöntemleri çağrılır.

Aşağıdaki `UpdateProduct` aşırı CL veri değişikliği yöntemleri uygulamak nasıl gösterilmektedir:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Uygun veri değişikliği iş mantığı katmanı yöntemi çağrılır, ancak yanıt döndürmeden önce önbellek geçersiz kılmak ihtiyacımız. Ne yazık ki, önbellek geçersiz kılmalarını açık olduğundan değil `ProductsCL` s sınıfı `GetProducts()` ve `GetProductsByCategoryID(categoryID)` yöntemleri her öğe Ekle önbelleğe farklı anahtarlara ve `GetProductsByCategoryID(categoryID)` yöntemi her biri için farklı önbellek öğesi ekler benzersiz *adlı kullanıcı, Categoryıd'si*.

Önbellek geçersiz kılınması, kaldırmak ihtiyacımız *tüm* tarafından eklenmemiş olabilir öğelerin `ProductsCL` sınıfı. Bu ilişkilendirerek gerçekleştirilebilir bir *önbelleğe bağımlılık* önbelleğe eklenen her öğeyle `AddCacheItem(key, value)` yöntemi. Genel olarak, bir önbellek bağımlılığı önbelleği, dosya sistemi veya bir Microsoft SQL Server veritabanındaki verileri bir dosyaya başka bir öğe olabilir. Olduğunda bağımlılık değiştirir veya önbellekten kaldırıldı, ilişkili olduğu önbellek öğeleri otomatik olarak önbellekten çıkarılmasına. Bu öğretici için tüm öğeleri için önbellek bağımlılık aracılığıyla eklenen, hizmet önbelleğinde ek öğe oluşturma istiyoruz `ProductsCL` sınıfı. Böylece, bu öğelerin tümünü önbellekten önbellek bağımlılığını kaldırarak kaldırılabilir.

Let s güncelleştirme `AddCacheItem(key, value)` yöntemi bu yöntemle önbelleğe eklenen her öğe, böylelikle tek önbellek bağımlılığı ile ilişkili:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray`ProductsCache tek bir değer içeren bir dize dizisidir. İlk olarak, bir önbellek öğesi önbelleğe eklenir ve geçerli tarih ve saat atandı. Önbellek öğesi zaten varsa, güncelleştirilir. Ardından, önbellek bağımlılığı oluşturulur. [ `CacheDependency` Sınıfı](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s kurucusunun aşırı sayısı, ancak burada kullanılan bir iki bekliyor `String` dizi girdi. Birinci bağımlılıklar olarak kullanılacak dosya kümesini belirtir. Biz güncelleştireceğinizi beri t değeri herhangi dosya tabanlı bağımlılıkları, kullanmak istediğiniz `Nothing` ilk giriş parametresi için kullanılır. İkinci giriş parametresi bağımlılıklar olarak kullanmak için önbellek anahtarlarını belirtir. Bizim tek bağımlılık burada belirttiğimiz `MasterCacheKeyArray`. `CacheDependency` Daha sonra içine geçirilir `Insert` yöntemi.

Bu değişikliği ile `AddCacheItem(key, value)`, invaliding bağımlılığın kaldırılması olarak basit bir önbelleğidir.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>5. adım: önbelleğe alma katman sunu katmanı çağırma

Önbelleğe alma katman s sınıflar ve yöntemler çalışmaya teknikler kullanılarak verilerle Biz bu öğreticileri incelenmesi ve kullanılabilir. Önbelleğe alınan verilerle çalışmak göstermek için yaptığınız değişiklikleri kaydetmek `ProductsCL` sınıfı ve ardından açın `FromTheArchitecture.aspx` sayfasındaki `Caching` klasörü ve GridView ekleyin. GridView s akıllı etiketten yeni ObjectDataSource oluşturun. Sihirbaz s ilk adımda görmelisiniz `ProductsCL` sınıf açılır liste seçeneklerinden biri olarak.


[![ProductsCL sınıfı iş nesnesi aşağı açılan listesinde yer](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Şekil 4**: `ProductsCL` sınıfı iş nesnesi aşağı açılan listesinde dahil ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-in-the-architecture-vb/_static/image6.png))


Seçtikten sonra `ProductsCL`, İleri'yi tıklatın. SELECT sekmesi açılır listede iki öğe - sahip `GetProducts()` ve `GetProductsByCategoryID(categoryID)` ve güncelleştirme sekmesi yalnızca `UpdateProduct` aşırı yükleme. Seçin `GetProducts()` yöntemi seçme sekmesinden ve `UpdateProducts` tıklatın ve güncelleştirme sekmesini yönteminden son.


[![S ProductsCL sınıfı yöntemleri aşağı açılan listeler içinde listelenir](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Şekil 5**: `ProductsCL` s sınıfı yöntemleri, aşağı açılan listeler içinde listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](caching-data-in-the-architecture-vb/_static/image9.png))


Sihirbazı tamamladıktan sonra Visual Studio ObjectDataSource s ayarlayacaktır `OldValuesParameterFormatString` özelliğine `original_{0}` ve GridView uygun alanları ekleyin. Değişiklik `OldValuesParameterFormatString` varsayılan değerini geri özelliğine `{0}`ve disk belleği, sıralama ve düzenleme desteklemek için GridView yapılandırın. Bu yana `UploadProducts` CL tarafından kullanılan aşırı kabul yalnızca s düzenlenen ürün adı ve fiyat, böylece yalnızca bu alanlar düzenlenebilir GridView sınırlandırın.

Önceki öğreticide için alanları içerecek şekilde GridView tanımladığımız `ProductName`, `CategoryName`, ve `UnitPrice` alanları. Bu biçimlendirme ve yapı çoğaltmak çekinmeyin, GridView ve ObjectDataSource s bildirim temelli durumda biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

Bu noktada önbelleğe alma katmanı kullanan bir sayfa vardır. Eylem önbelleğinde görmek için kümesinde kesme noktaları `ProductsCL` s sınıfı `GetProducts()` ve `UpdateProduct` yöntemleri. Sıralama ve verileri görmek için disk belleği önbellekten çekilen, tarayıcı ve kod aracılığıyla adım sayfasını ziyaret edin. Bir kaydı güncelleştirmeye sonra Not önbellekte geçersiz kılınan ve sonuç olarak, verileri GridView DataSet'e olduğunda, BLL alınır.

> [!NOTE]
> Bu makalede eşlik indirme sağlanan önbelleğe alma katmanı tam değil. Yalnızca bir sınıf içerir `ProductsCL`, hangi yalnızca Spor yöntemleri sayıda. Ayrıca, yalnızca bir ASP.NET sayfası CL kullanır (`~/Caching/FromTheArchitecture.aspx`) diğerlerini hala BLL doğrudan başvurun. Uygulamanızda bir CL kullanmayı planlıyorsanız, bu sınıfları ve sunu katmanı tarafından kullanılmakta BLL yöntemleri yöntemlerini ele ve sunu katmanı gelen tüm çağrıları CL s sınıfları, gerektirecek CL tamamlamalıdır.


## <a name="summary"></a>Özet

Önbelleğe alma ASP.NET 2.0 s SqlDataSource sunu katmanı ve ObjectDataSource denetimleri uygulanabilir olsa da, ideal olarak sorumlulukları önbelleğe alma mimarisi ayrı bir katmana temsilci. Bu öğreticide bir önbelleğe alma sunu katmanı ve iş mantığı katmanı arasında duran katman oluşturduk. Önbelleğe alma katman sınıfları ve BLL var ve sunu katmanı adlı yöntemleri aynı kümesi sağlaması gerekir.

Biz keşfedilen bu ve önceki öğreticileri önbelleğe alma katman örnekleri sergilenen *reaktif yükleme*. Geriye dönük yükleme ile veriler yalnızca veriler için bir istek yapıldığında ve bu verileri önbellekten eksik önbelleğine yüklenir. Verileri de olabilir *önceden yüklenen* önbelleğine bir teknik, yükler verileri önbelleğe gerçekten gerekli önce. Sonraki öğreticide bir örnek uygulama başlangıcında önbelleğine statik değerlerini depolamak nasıl ele zaman öngörülü yüklenirken göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](caching-data-with-the-objectdatasource-vb.md)
[sonraki](caching-data-at-application-startup-vb.md)
