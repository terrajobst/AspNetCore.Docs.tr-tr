---
title: ASP.NET Core 'de önbellek belleği
author: rick-anderson
description: ASP.NET Core bellekteki verileri önbelleğe alma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 8/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 474f71225371ea89b023ee077d4ecc03e9751681
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030319"
---
# <a name="cache-in-memory-in-aspnet-core"></a>ASP.NET Core 'de önbellek belleği

By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)ve [Steve Smith](https://ardalis.com/)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Önbelleğe alma temelleri

Önbelleğe alma, içerik oluşturmak için gereken işi azaltarak bir uygulamanın performansını ve ölçeklenebilirliğini önemli ölçüde iyileştirebilir. Önbelleğe alma, seyrek olarak değişen verilerle en iyi şekilde sonuç verir. Önbelleğe alma, özgün kaynaktan çok daha hızlı döndürülebilecek verilerin bir kopyasını oluşturur. Uygulamalar, önbelleğe alınmış verilere **hiçbir** şekilde bağlı olmayacak şekilde yazılmalıdır ve test edilmelidir.

ASP.NET Core birçok farklı önbelleği destekler. En basit önbellek, Web sunucusunun belleğinde depolanan bir önbelleği temsil eden [ımemorycache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)' i temel alır. Birden çok sunucu grubu üzerinde çalışan uygulamalar, bellek içi önbellek kullanılırken oturumların yapışkan olmasını sağlamalıdır. Yapışkan oturumlar, bir istemciden gelen sonraki isteklerin aynı sunucuya gitmesini sağlar. Örneğin, Azure Web Apps, sonraki tüm istekleri aynı sunucuya yönlendirmek için [uygulama Isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) kullanır.

Bir Web grubundaki yapışkan olmayan oturumlar, önbellek tutarlılığı sorunlarından kaçınmak için [Dağıtılmış bir önbellek](distributed.md) gerektirir. Bazı uygulamalarda, dağıtılmış bir önbellek, bellek içi bir önbellekten daha yüksek genişleme desteği sağlayabilir. Dağıtılmış bir önbellek kullanmak önbellek belleğini bir dış işleme devreder.

::: moniker range="< aspnetcore-2.0"

Önbellek `IMemoryCache` [önceliği](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) olarak`CacheItemPriority.NeverRemove`ayarlanmadığı takdirde önbellek, bellek baskısı altında önbellek girdilerini çıkarır. Önbellek, bellek baskısı `CacheItemPriority` altındaki öğeleri çıkardıkları önceliği ayarlamak için ayarlayabilirsiniz.

::: moniker-end

Bellek içi önbellek herhangi bir nesneyi depolayabilirler; Dağıtılmış önbellek arabirimi ile `byte[]`sınırlıdır. Bellek içi ve dağıtılmış önbellek deposu öğeleri anahtar-değer çiftleri olarak önbelleğe alma.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([NuGet paketi](https://www.nuget.org/packages/System.Runtime.Caching/)) ile birlikte kullanılabilir:

* .NET Standard 2,0 veya üzeri.
* .NET Standard 2,0 veya sonraki bir sürümü hedefleyen tüm [.NET uygulamaları](/dotnet/standard/net-standard#net-implementation-support) . Örneğin, 2,0 veya üzeri ASP.NET Core.
* .NET Framework 4,5 veya üzeri.

[](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/ ASP.NET Core ' ye daha iyi tümleştirildiği için Microsoft. Extensions. Caching. Memory`IMemoryCache` ( `System.Runtime.Caching` / `MemoryCache` Bu makalede açıklanan) önerilir. Örneğin, ASP.NET Core `IMemoryCache` [bağımlılığı ekleme](xref:fundamentals/dependency-injection)ile yerel olarak işe yarar.

Kodu `System.Runtime.Caching` ASP.NET 4. x konumundan ASP.NET Core taşıma sırasında uyumluluk Köprüsü olarak kullanın / `MemoryCache` .

## <a name="cache-guidelines"></a>Önbellek yönergeleri

* Kodun, verileri getirmek için her zaman bir geri dönüş seçeneği olmalıdır ve kullanılabilir önbelleğe alınmış bir değere bağlı **değildir** .
* Önbellek bir nadir kaynağı, bellek kullanır. Önbellek büyümesini sınırla:
  * Dış girişi önbellek anahtarları olarak kullanmayın.
  * Önbellek büyümesini sınırlamak için süre sonlarını kullanın.
  * [Önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanın](#use-setsize-size-and-sizelimit-to-limit-cache-size). ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek boyutunu sınırlamaz. En fazla geliştirici, önbellek boyutunu sınırlayacak.

## <a name="using-imemorycache"></a>Imemorycache kullanma

> [!WARNING]
> [Bağımlılık ekleme](xref:fundamentals/dependency-injection) ve arama `SetSize`, `Size`ya `SizeLimit` da önbellek boyutunu sınırlamak için *paylaşılan* bellek önbelleğinin kullanılması uygulamanın başarısız olmasına neden olabilir. Önbellekte bir boyut sınırı ayarlandığında, tüm girişlerin eklenmekte olan bir boyut belirtmesi gerekir. Bu, geliştiricilerin paylaşılan önbelleğin kullanıldığı ilgili tam denetime sahip olmaması nedeniyle sorunlara yol açabilir. Örneğin, Entity Framework Core paylaşılan önbelleği kullanır ve bir boyut belirtmez. Bir uygulama önbellek boyutu sınırı ayarlarsa ve EF Core kullanıyorsa, uygulama bir `InvalidOperationException`oluşturur.
> `SetSize`, ,`Size`Veya önbelleğinisınırlandırmakiçin,`SizeLimit` önbelleğe alma için tek bir önbellek oluşturun. Daha fazla bilgi ve bir örnek için bkz. [önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanma](#use-setsize-size-and-sizelimit-to-limit-cache-size).

Bellek içi önbelleğe alma, [bağımlılık ekleme](xref:fundamentals/dependency-injection)kullanılarak uygulamanız tarafından başvurulan bir *hizmettir* . Çağırma `AddMemoryCache` :`ConfigureServices`

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

`IMemoryCache` Örneği oluşturucuda iste:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache`NuGet paketi [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)gerektirir.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache`Microsoft [. AspNetCore. All metapackage](xref:fundamentals/metapackage)' de bulunan NuGet paketi [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)gerektirir.

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache`Microsoft [. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)' de bulunan NuGet paketi [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)gerektirir.

::: moniker-end

Aşağıdaki kod, bir saatin önbellekte olup olmadığını denetlemek için [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) kullanır. Bir zaman önbelleğe alınmadıysa, yeni bir giriş oluşturulur ve [Ayarla](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)birlikte önbelleğe eklenir.

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Geçerli saat ve önbelleğe alınmış saat görüntülenir:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Zaman aşımı `DateTime` süresi içinde istekler varken önbelleğe alınan değer önbellekte kalır. Aşağıdaki görüntüde geçerli saat ve önbellekten alınan eski bir zaman gösterilmektedir:

![İki farklı saat görüntülenirken dizin görünümü](memory/_static/time.png)

Aşağıdaki kod, verileri önbelleğe almak için [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [Getorcreateasync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) kullanır.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Aşağıdaki kod, önbelleğe alınmış zamanı getirmek için [Al](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) yöntemini çağırır:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>ve [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , ve kapasitesini genişleten [cacheextensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>sınıfının bir parçası olan genişletme metodlarıdır. Diğer önbellek yöntemlerinin açıklaması için bkz. [ımemorycache metotları](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [cacheextensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) .

## <a name="memorycacheentryoptions"></a>Memorycachebir Yoptions

Aşağıdaki örnek:

* Mutlak süre sonu saatini ayarlar. Bu, girişin önbelleğe alınamayacağı en uzun süredir ve kayan süre sürekli yenilendiğinde öğenin çok eski hale gelmesini önler.
* Kayan süre sonu zamanı ayarlar. Bu önbelleğe alınmış öğeye erişen istekler, Kayan süre sonu saatini sıfırlayacaktır.
* Önbellek önceliğini olarak `CacheItemPriority.NeverRemove`ayarlar.
* Giriş önbellekten çıkarıldıktan sonra çağrılacak [Postevictiondelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ayarlar. Geri çağırma, öğeyi önbellekten kaldıran koddan farklı bir iş parçacığında çalıştırılır.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Önbellek boyutunu sınırlamak için SetSize, size ve SizeLimit kullanın

`MemoryCache` Örnek, isteğe bağlı olarak bir boyut sınırı belirtebilir ve uygulayabilir. Önbelleğin, girdilerin boyutunu ölçmeye yönelik bir mekanizması olmadığından, bellek boyutu sınırının tanımlı bir ölçü birimi yok. Önbellek bellek boyutu sınırı ayarlandıysa, tüm girişlerin boyut belirtmesi gerekir. ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek boyutunu sınırlamaz. En fazla geliştirici, önbellek boyutunu sınırlayacak. Belirtilen boyut, geliştiricinin seçtiği birimlerde bulunur.

Örneğin:

* Web uygulaması öncelikle dizeleri önbelleğe alıyorsa, her önbellek girdisi boyutu dize uzunluğu olabilir.
* Uygulama tüm girdilerin boyutunu 1 olarak belirtebilir ve boyut sınırı girdi sayısıdır.

Aşağıdaki kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection)tarafından erişilebilen bir unitless sabit Boyut [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) oluşturur:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) birimi yok. Önbelleğe alınan girişlerin, önbellek belleği boyutu ayarlandıysa en çok ne kadar uygun olduğunu belirleyen birimleri belirtmesi gerekir. Bir önbellek örneğinin tüm kullanıcıları aynı birim sistemini kullanmalıdır. Önbelleğe alınmış giriş boyutlarının toplamı tarafından `SizeLimit`belirtilen değeri aşarsa, bir giriş önbelleğe alınmaz. Önbellek boyutu sınırı ayarlanmamışsa, girişte ayarlanan önbellek boyutu yok sayılır.

Aşağıdaki kod, `MyMemoryCache` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydolur.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`, bu boyut sınırlı önbelleğin farkında olan bileşenler için bağımsız bir bellek önbelleği olarak oluşturulur ve önbellek girişi boyutunu uygun şekilde ayarlamayı öğrenin.

Aşağıdaki kod şunu kullanır `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Önbellek girişinin boyutu [boyuta](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) veya [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) uzantı yöntemine göre ayarlanabilir:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Önbellek bağımlılıkları

Aşağıdaki örnek, bağımlı bir girdinin süresi dolduğunda önbellek girişinin süresinin dolacağını gösterir. `CancellationChangeToken` Önbelleğe alınmış öğeye eklenir. `Cancel` Üzerindeçağrıldığında,heriki`CancellationTokenSource`önbellek girişi de çıkarıldı.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

Kullanmak, birden çok önbellek girişinin bir grup olarak çıkarıyapılmasınaizinverir.`CancellationTokenSource` `using` Yukarıdaki koddaki `using` örüntüle, blok içinde oluşturulan önbellek girdileri Tetikleyiciler ve süre sonu ayarlarını devralacak.

## <a name="additional-notes"></a>Ek notlar

* Bir önbellek öğesini yeniden doldurmak için geri çağırma kullanırken:

  * Geri arama tamamlanmadığından, birden çok istek önbelleğe alınan anahtar değeri boş olabilir.
  * Bu, birden çok iş parçacığının önbelleğe alınan öğeyi yeniden doldurma ile sonuçlanabilir.

* Diğeri oluşturmak için bir önbellek girdisi kullanıldığında, alt öğe üst girdinin süre sonu belirteçlerini ve zaman tabanlı süre sonu ayarlarını kopyalar. Üst girdinin el ile kaldırılması veya güncelleştirilmesi için alt öğenin kullanım dışı olmaması.

* Önbellek girdisi önbellekten çıkarıldıktan sonra uygulanacak geri çağırmaları ayarlamak için [Postevictioncallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) kullanın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
