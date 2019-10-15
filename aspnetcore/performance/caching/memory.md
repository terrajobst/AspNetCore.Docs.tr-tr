---
title: ASP.NET Core 'de önbellek belleği
author: rick-anderson
description: ASP.NET Core bellekteki verileri önbelleğe alma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: aa39503f034cf46fa4317a1f3cbb8d130afd1b8c
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333746"
---
# <a name="cache-in-memory-in-aspnet-core"></a>ASP.NET Core 'de önbellek belleği

::: moniker range=">= aspnetcore-3.0"

By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)ve [Steve Smith](https://ardalis.com/)

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Önbelleğe alma temelleri

Önbelleğe alma, içerik oluşturmak için gereken işi azaltarak bir uygulamanın performansını ve ölçeklenebilirliğini önemli ölçüde iyileştirebilir. Önbelleğe alma işlemi, seyrek olarak değişen **ve** üretime masraflı verilerle en iyi şekilde çalışıyor. Önbelleğe alma, kaynaktan çok daha hızlı döndürülebilecek verilerin bir kopyasını oluşturur. Uygulamalar, önbelleğe alınmış verilere **hiçbir** şekilde bağlı olmayacak şekilde yazılmalıdır ve test edilmelidir.

ASP.NET Core birçok farklı önbelleği destekler. En basit önbellek [ımemorycache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)' i temel alır. `IMemoryCache`, Web sunucusunun belleğinde depolanan bir önbelleği temsil eder. Sunucu grubunda (birden çok sunucu) çalışan uygulamalar, bellek içi önbellek kullanılırken oturumların yapışkan olmasını sağlamalıdır. Yapışkan oturumlar, bir istemciden gelen sonraki isteklerin aynı sunucuya gitmesini sağlar. Örneğin, Azure Web Apps, sonraki tüm istekleri aynı sunucuya yönlendirmek için [uygulama Isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) kullanır.

Bir Web grubundaki yapışkan olmayan oturumlar, önbellek tutarlılığı sorunlarından kaçınmak için [Dağıtılmış bir önbellek](distributed.md) gerektirir. Bazı uygulamalarda, dağıtılmış bir önbellek, bellek içi bir önbellekten daha yüksek genişleme desteği sağlayabilir. Dağıtılmış bir önbellek kullanmak önbellek belleğini bir dış işleme devreder.

Bellek içi önbellek herhangi bir nesneyi depolayabilirler. Dağıtılmış önbellek arabirimi `byte[]` ile sınırlıdır. Bellek içi ve dağıtılmış önbellek deposu öğeleri anahtar-değer çiftleri olarak önbelleğe alma.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. Caching/MemoryCache

<xref:System.Runtime.Caching> @ no__t-1 @ no__t-2 ([NuGet paketi](https://www.nuget.org/packages/System.Runtime.Caching/)) ile birlikte kullanılabilir:

* .NET Standard 2,0 veya üzeri.
* .NET Standard 2,0 veya sonraki bir sürümü hedefleyen tüm [.NET uygulamaları](/dotnet/standard/net-standard#net-implementation-support) . Örneğin, 2,0 veya üzeri ASP.NET Core.
* .NET Framework 4,5 veya üzeri.

ASP.NET Core daha iyi tümleşik olduğundan, [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/ @ no__t-2 (Bu makalede açıklanan) `System.Runtime.Caching` @ no__t-4 @ no__t-5 üzerinden önerilir. Örneğin `IMemoryCache`, ASP.NET Core [bağımlılığı ekleme](xref:fundamentals/dependency-injection)ile yerel olarak çalışmaktadır.

ASP.NET 4. x öğesinden ASP.NET Core kod taşırken uyumluluk Köprüsü olarak `System.Runtime.Caching` @ no__t-1 @ no__t-2 kullanın.

## <a name="cache-guidelines"></a>Önbellek yönergeleri

* Kodun, verileri getirmek için her zaman bir geri dönüş seçeneği olmalıdır ve kullanılabilir önbelleğe alınmış bir değere bağlı **değildir** .
* Önbellek bir nadir kaynağı, bellek kullanır. Önbellek büyümesini sınırla:
  * Dış girişi önbellek anahtarları **olarak kullanmayın.**
  * Önbellek büyümesini sınırlamak için süre sonlarını kullanın.
  * [Önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanın](#use-setsize-size-and-sizelimit-to-limit-cache-size). ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek **boyutunu sınırlamaz.** En fazla geliştirici, önbellek boyutunu sınırlayacak.

## <a name="use-imemorycache"></a>Imemorycache kullan

> [!WARNING]
> Ön sınır [ekleme](xref:fundamentals/dependency-injection) ve `SetSize`, `Size` veya `SizeLimit` ' e çağrı için bir *paylaşılan* bellek önbelleğinin kullanılması uygulamanın başarısız olmasına neden olabilir. Önbellekte bir boyut sınırı ayarlandığında, tüm girişlerin eklenmekte olan bir boyut belirtmesi gerekir. Bu, geliştiricilerin paylaşılan önbelleğin kullanıldığı ilgili tam denetime sahip olmaması nedeniyle sorunlara yol açabilir. Örneğin, Entity Framework Core paylaşılan önbelleği kullanır ve bir boyut belirtmez. Bir uygulama önbellek boyutu sınırı ayarlarsa ve EF Core kullanıyorsa, uygulama bir `InvalidOperationException` oluşturur.
> Önbelleği sınırlandırmak için `SetSize`, `Size` veya `SizeLimit` kullanıldığında, önbelleğe alma için bir önbellek oluşturun. Daha fazla bilgi ve bir örnek için bkz. [önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanma](#use-setsize-size-and-sizelimit-to-limit-cache-size).

Bellek içi önbelleğe alma, [bağımlılık ekleme](xref:fundamentals/dependency-injection)kullanılarak bir uygulamadan başvurulan bir *hizmettir* . Oluşturucuda `IMemoryCache` örneği isteyin:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

Aşağıdaki kod, bir saatin önbellekte olup olmadığını denetlemek için [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) kullanır. Bir zaman önbelleğe alınmadıysa, yeni bir giriş oluşturulur ve [Ayarla](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)birlikte önbelleğe eklenir. @No__t-0 sınıfı, indirme örneğinin bir parçasıdır.

[!code-csharp[](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

Geçerli saat ve önbelleğe alınmış saat görüntülenir:

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

Önbelleğe alınan @no__t 0 değeri, zaman aşımı süresi içinde istekler varken önbellekte kalır.

Aşağıdaki kod, verileri önbelleğe almak için [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [Getorcreateasync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) kullanır.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Aşağıdaki kod, önbelleğe alınmış zamanı getirmek için [Al](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) yöntemini çağırır:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

Aşağıdaki kod, mutlak süre sonu ile önbelleğe alınmış bir öğe alır veya oluşturur:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

Yalnızca kayan bir süre sonu olan önbelleğe alınmış bir öğe kümesi, eski olma riski altında. Kayan süre sonu aralığından daha sık erişiliyorsa, öğe hiçbir zaman sona ermez. Mutlak süre sonu zamanı başarılı olduktan sonra öğenin süresinin dolacağını garantilemek için kayan bir süre sonu mutlak bir süre sonu ile birleştirin. Mutlak süre sonu, öğenin, Kayan süre sonu aralığı içinde istenmediğinde daha önce süresinin dolmasına izin verirken öğenin ne kadar süre önbellekte önbelleğe alınacağını belirleyen bir üst sınır ayarlar. Mutlak ve kayan süre sonu belirtildiğinde, süre sonları mantıksal ORed. Kayan süre sonu aralığı *veya* mutlak süre sonu zamanı başarılı olursa, öğe önbellekten çıkarıldı.

Aşağıdaki kod *hem kayan hem de mutlak* süre sonu ile önbelleğe alınmış bir öğe alır veya oluşturur:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

Yukarıdaki kod, verilerin mutlak süreden daha uzun süre önbelleğe alınmamasını garanti eder.

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> ve <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> sınıfında uzantı yöntemleridir. Bu yöntemler <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> özelliğini genişletir.

## <a name="memorycacheentryoptions"></a>Memorycachebir Yoptions

Aşağıdaki örnek:

* Kayan süre sonu zamanı ayarlar. Bu önbelleğe alınmış öğeye erişen istekler, Kayan süre sonu saatini sıfırlayacaktır.
* Önbellek önceliğini [Cacheitempriınıd. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove)olarak ayarlar.
* Giriş önbellekten çıkarıldıktan sonra çağrılacak [Postevictiondelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ayarlar. Geri çağırma, öğeyi önbellekten kaldıran koddan farklı bir iş parçacığında çalıştırılır.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Önbellek boyutunu sınırlamak için SetSize, size ve SizeLimit kullanın

@No__t-0 örneği, isteğe bağlı olarak bir boyut sınırı belirtebilir ve uygulayabilir. Önbelleğin, girdilerin boyutunu ölçmeye yönelik bir mekanizması olmadığından, önbellek boyutu sınırının tanımlı bir ölçü birimi yok. Önbellek boyutu sınırı ayarlandıysa, tüm girişlerin boyut belirtmesi gerekir. ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek boyutunu sınırlamaz. En fazla geliştirici, önbellek boyutunu sınırlayacak. Belirtilen boyut, geliştiricinin seçtiği birimlerde bulunur.

Örneğin:

* Web uygulaması öncelikle dizeleri önbelleğe alıyorsa, her önbellek girdisi boyutu dize uzunluğu olabilir.
* Uygulama tüm girdilerin boyutunu 1 olarak belirtebilir ve boyut sınırı girdi sayısıdır.

@No__t-0 ayarlanmamışsa, önbellek bağlantılı olmadan büyür. ASP.NET Core çalışma zamanı, sistem belleği azaldığında önbelleği kırpmaz. Uygulamalar şu şekilde tasarlanmıştır:

* Önbellek büyümesini sınırla.
* Kullanılabilir bellek sınırlıysa <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> veya <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> ' i çağırın:

Aşağıdaki kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection)tarafından erişilebilen @no__t bir unitless sabit boyutu oluşturur:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit` ' da birim yok. Önbellek boyutu sınırı ayarlandıysa, önbelleğe alınmış girişler, en çok ne kadar uygun olduğunu belirleyen birimlerde boyut belirtmelidir. Bir önbellek örneğinin tüm kullanıcıları aynı birim sistemini kullanmalıdır. Önbelleğe alınmış giriş boyutlarının toplamı, `SizeLimit` tarafından belirtilen değeri aşarsa bir giriş önbelleğe alınmaz. Önbellek boyutu sınırı ayarlanmamışsa, girişte ayarlanan önbellek boyutu yok sayılır.

Aşağıdaki kod `MyMemoryCache` ' i [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydeder.

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

`MyMemoryCache`, bu boyut sınırlı önbelleğin farkında olan bileşenler için bağımsız bir bellek önbelleği olarak oluşturulur ve önbellek girişi boyutunu uygun şekilde ayarlamayı öğrenin.

Aşağıdaki kod @no__t kullanır-0:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

Önbellek girişinin boyutu <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> veya <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> uzantı yöntemleriyle ayarlanabilir:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache. Compact

`MemoryCache.Compact` önbelleğin belirtilen yüzdesini aşağıdaki sırada kaldırmaya çalışır:

* Tüm süre dolmamış öğeler.
* Önceliğe göre öğeler. Önce en düşük öncelik öğeleri kaldırılır.
* En son kullanılan nesneler.
* En erken mutlak bitiş tarihi olan öğeler.
* En erken Kayan süre sonu olan öğeler.

@No__t-0 önceliğiyle sabitlenmiş öğeler hiçbir şekilde kaldırılmaz. Aşağıdaki kod bir önbellek öğesini ve çağrıları kaldırır `Compact`:

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Daha fazla bilgi için bkz. [GitHub 'Da Compact Source](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Önbellek bağımlılıkları

Aşağıdaki örnek, bağımlı bir girdinin süresi dolduğunda önbellek girişinin süresinin dolacağını gösterir. Önbelleğe alınmış öğeye bir `CancellationChangeToken` eklenir. @No__t-0 `CancellationTokenSource` ' de çağrıldığında, her iki önbellek girişi de kaldırılır.

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

@No__t-0 kullanmak, birden çok önbellek girişinin bir grup olarak çıkarıyapılmasına izin verir. Yukarıdaki kodda `using` düzeniyle, `using` bloğunun içinde oluşturulan önbellek girdileri Tetikleyiciler ve süre sonu ayarlarını devralacak.

## <a name="additional-notes"></a>Ek notlar

* Süre sonu arka planda gerçekleşmez. Süre dolmakta olan öğeler için önbelleği etkin bir şekilde tarayan bir Zamanlayıcı yok. Önbellekteki (`Get`, `Set`, `Remove`) herhangi bir etkinlik, süre sonu olmayan öğeler için bir arka plan taraması tetikleyebilir. @No__t-0 (`CancelAfter`) üzerindeki bir Zamanlayıcı Ayrıca girişi kaldırır ve vadesi geçmiş öğeler için bir tarama tetikler. Örneğin, `SetAbsoluteExpiration(TimeSpan.FromHours(1))` kullanmak yerine, kayıtlı belirteç için `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` kullanın. Bu belirteç tetiklendiğinde, girdiyi hemen kaldırır ve çıkarma geri çağırmaları tetikler. Daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/Caching/issues/248)bakın.
* Bir önbellek öğesini yeniden doldurmak için geri çağırma kullanırken:

  * Geri arama tamamlanmadığından, birden çok istek önbelleğe alınan anahtar değeri boş olabilir.
  * Bu, birden çok iş parçacığının önbelleğe alınan öğeyi yeniden doldurma ile sonuçlanabilir.

* Diğeri oluşturmak için bir önbellek girdisi kullanıldığında, alt öğe üst girdinin süre sonu belirteçlerini ve zaman tabanlı süre sonu ayarlarını kopyalar. Üst girdinin el ile kaldırılması veya güncelleştirilmesi için alt öğenin kullanım dışı olmaması.

* Önbellek girdisi önbellekten çıkarıldıktan sonra uygulanacak geri çağırmaları ayarlamak için <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> kullanın.
* Çoğu uygulama için, `IMemoryCache` etkindir. Örneğin, `ConfigureServices` ' te `AddMvc`, `AddControllersWithViews`, `AddRazorPages`, `AddMvcCore().AddRazorViewEngine` ve diğer birçok `Add{Service}` yöntemi çağrılırken `IMemoryCache` etkinleştirilir. Önceki `Add{Service}` yöntemlerinden birini çağırmayan uygulamalarda, `ConfigureServices` ' de <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> ' i çağırmak gerekebilir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo)ve [Steve Smith](https://ardalis.com/)

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Önbelleğe alma temelleri

Önbelleğe alma, içerik oluşturmak için gereken işi azaltarak bir uygulamanın performansını ve ölçeklenebilirliğini önemli ölçüde iyileştirebilir. Önbelleğe alma, seyrek olarak değişen verilerle en iyi şekilde sonuç verir. Önbelleğe alma, özgün kaynaktan çok daha hızlı döndürülebilecek verilerin bir kopyasını oluşturur. Kod, önbelleğe alınmış verilere **hiçbir** şekilde bağlı olmayacak şekilde yazılmalıdır ve test edilmelidir.

ASP.NET Core birçok farklı önbelleği destekler. En basit önbellek, Web sunucusunun belleğinde depolanan bir önbelleği temsil eden [ımemorycache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)' i temel alır. Sunucu grubunda çalışan uygulamalar (birden çok sunucu), bellek içi önbellek kullanılırken oturumların yapışkan olmasını sağlamalıdır. Yapışkan oturumlar, bir istemciden sonraki isteklerin aynı sunucuya gitmesini sağlar. Örneğin, Azure Web Apps, bir Kullanıcı aracısından tüm istekleri aynı sunucuya yönlendirmek için [uygulama Isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) kullanır.

Bir Web grubundaki yapışkan olmayan oturumlar, önbellek tutarlılığı sorunlarından kaçınmak için [Dağıtılmış bir önbellek](distributed.md) gerektirir. Bazı uygulamalarda, dağıtılmış bir önbellek, bellek içi bir önbellekten daha yüksek genişleme desteği sağlayabilir. Dağıtılmış bir önbellek kullanmak önbellek belleğini bir dış işleme devreder.

Bellek içi önbellek herhangi bir nesneyi depolayabilirler. Dağıtılmış önbellek arabirimi `byte[]` ile sınırlıdır. Bellek içi ve dağıtılmış önbellek deposu öğeleri anahtar-değer çiftleri olarak önbelleğe alma.

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. Caching/MemoryCache

<xref:System.Runtime.Caching> @ no__t-1 @ no__t-2 ([NuGet paketi](https://www.nuget.org/packages/System.Runtime.Caching/)) ile birlikte kullanılabilir:

* .NET Standard 2,0 veya üzeri.
* .NET Standard 2,0 veya sonraki bir sürümü hedefleyen tüm [.NET uygulamaları](/dotnet/standard/net-standard#net-implementation-support) . Örneğin, 2,0 veya üzeri ASP.NET Core.
* .NET Framework 4,5 veya üzeri.

ASP.NET Core daha iyi tümleşik olduğundan, [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/ @ no__t-2 (Bu makalede açıklanan) `System.Runtime.Caching` @ no__t-4 @ no__t-5 üzerinden önerilir. Örneğin `IMemoryCache`, ASP.NET Core [bağımlılığı ekleme](xref:fundamentals/dependency-injection)ile yerel olarak çalışmaktadır.

ASP.NET 4. x öğesinden ASP.NET Core kod taşırken uyumluluk Köprüsü olarak `System.Runtime.Caching` @ no__t-1 @ no__t-2 kullanın.

## <a name="cache-guidelines"></a>Önbellek yönergeleri

* Kodun, verileri getirmek için her zaman bir geri dönüş seçeneği olmalıdır ve kullanılabilir önbelleğe alınmış bir değere bağlı **değildir** .
* Önbellek bir nadir kaynağı, bellek kullanır. Önbellek büyümesini sınırla:
  * Dış girişi önbellek anahtarları **olarak kullanmayın.**
  * Önbellek büyümesini sınırlamak için süre sonlarını kullanın.
  * [Önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanın](#use-setsize-size-and-sizelimit-to-limit-cache-size). ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek boyutunu sınırlamaz. En fazla geliştirici, önbellek boyutunu sınırlayacak.

## <a name="using-imemorycache"></a>Imemorycache kullanma

> [!WARNING]
> Ön sınır [ekleme](xref:fundamentals/dependency-injection) ve `SetSize`, `Size` veya `SizeLimit` ' e çağrı için bir *paylaşılan* bellek önbelleğinin kullanılması uygulamanın başarısız olmasına neden olabilir. Önbellekte bir boyut sınırı ayarlandığında, tüm girişlerin eklenmekte olan bir boyut belirtmesi gerekir. Bu, geliştiricilerin paylaşılan önbelleğin kullanıldığı ilgili tam denetime sahip olmaması nedeniyle sorunlara yol açabilir. Örneğin, Entity Framework Core paylaşılan önbelleği kullanır ve bir boyut belirtmez. Bir uygulama önbellek boyutu sınırı ayarlarsa ve EF Core kullanıyorsa, uygulama bir `InvalidOperationException` oluşturur.
> Önbelleği sınırlandırmak için `SetSize`, `Size` veya `SizeLimit` kullanıldığında, önbelleğe alma için bir önbellek oluşturun. Daha fazla bilgi ve bir örnek için bkz. [önbellek boyutunu sınırlamak Için SetSize, size ve SizeLimit kullanma](#use-setsize-size-and-sizelimit-to-limit-cache-size).

Bellek içi önbelleğe alma, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)kullanılarak uygulamanız tarafından başvurulan bir *hizmettir* . @No__t-1 içinde `AddMemoryCache` çağrısı:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Oluşturucuda `IMemoryCache` örneği isteyin:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

`IMemoryCache`, [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)Içinde bulunan [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)için NuGet paketini gerektirir.

Aşağıdaki kod, bir saatin önbellekte olup olmadığını denetlemek için [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) kullanır. Bir zaman önbelleğe alınmadıysa, yeni bir giriş oluşturulur ve [Ayarla](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)birlikte önbelleğe eklenir.

[!code-csharp[](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Geçerli saat ve önbelleğe alınmış saat görüntülenir:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Önbelleğe alınan @no__t 0 değeri, zaman aşımı süresi içinde istekler varken önbellekte kalır. Aşağıdaki görüntüde geçerli saat ve önbellekten alınan eski bir zaman gösterilmektedir:

![İki farklı saat görüntülenirken dizin görünümü](memory/_static/time.png)

Aşağıdaki kod, verileri önbelleğe almak için [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [Getorcreateasync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) kullanır.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Aşağıdaki kod, önbelleğe alınmış zamanı getirmek için [Al](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) yöntemini çağırır:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> ve [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) , [cacheextensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) sınıfının <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> özelliğini genişleten bir parçası. Diğer önbellek yöntemlerinin açıklaması için bkz. [ımemorycache metotları](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [cacheextensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) .

## <a name="memorycacheentryoptions"></a>Memorycachebir Yoptions

Aşağıdaki örnek:

* Kayan süre sonu zamanı ayarlar. Bu önbelleğe alınmış öğeye erişen istekler, Kayan süre sonu saatini sıfırlayacaktır.
* Önbellek önceliğini `CacheItemPriority.NeverRemove` olarak ayarlar.
* Giriş önbellekten çıkarıldıktan sonra çağrılacak [Postevictiondelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ayarlar. Geri çağırma, öğeyi önbellekten kaldıran koddan farklı bir iş parçacığında çalıştırılır.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Önbellek boyutunu sınırlamak için SetSize, size ve SizeLimit kullanın

@No__t-0 örneği, isteğe bağlı olarak bir boyut sınırı belirtebilir ve uygulayabilir. Önbelleğin, girdilerin boyutunu ölçmeye yönelik bir mekanizması olmadığından, önbellek boyutu sınırının tanımlı bir ölçü birimi yok. Önbellek boyutu sınırı ayarlandıysa, tüm girişlerin boyut belirtmesi gerekir. ASP.NET Core çalışma zamanı, bellek baskısı temelinde önbellek boyutunu sınırlamaz. En fazla geliştirici, önbellek boyutunu sınırlayacak. Belirtilen boyut, geliştiricinin seçtiği birimlerde bulunur.

Örneğin:

* Web uygulaması öncelikle dizeleri önbelleğe alıyorsa, her önbellek girdisi boyutu dize uzunluğu olabilir.
* Uygulama tüm girdilerin boyutunu 1 olarak belirtebilir ve boyut sınırı girdi sayısıdır.

@No__t-0 ayarlanmamışsa, önbellek bağlantılı olmadan büyür. ASP.NET Core çalışma zamanı, sistem belleği azaldığında önbelleği kırpmaz. Uygulamalar şu şekilde tasarlanmıştır:

* Önbellek büyümesini sınırla.
* Kullanılabilir bellek sınırlıysa <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> veya <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> ' i çağırın:

Aşağıdaki kod, [bağımlılık ekleme](xref:fundamentals/dependency-injection)tarafından erişilebilen @no__t bir unitless sabit boyutu oluşturur:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit` ' da birim yok. Önbellek boyutu sınırı ayarlandıysa, önbelleğe alınmış girişler, en çok ne kadar uygun olduğunu belirleyen birimlerde boyut belirtmelidir. Bir önbellek örneğinin tüm kullanıcıları aynı birim sistemini kullanmalıdır. Önbelleğe alınmış giriş boyutlarının toplamı, `SizeLimit` tarafından belirtilen değeri aşarsa bir giriş önbelleğe alınmaz. Önbellek boyutu sınırı ayarlanmamışsa, girişte ayarlanan önbellek boyutu yok sayılır.

Aşağıdaki kod `MyMemoryCache` ' i [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına kaydeder.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`, bu boyut sınırlı önbelleğin farkında olan bileşenler için bağımsız bir bellek önbelleği olarak oluşturulur ve önbellek girişi boyutunu uygun şekilde ayarlamayı öğrenin.

Aşağıdaki kod @no__t kullanır-0:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Önbellek girişinin boyutu [boyuta](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) veya [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) uzantı yöntemine göre ayarlanabilir:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache. Compact

`MemoryCache.Compact` önbelleğin belirtilen yüzdesini aşağıdaki sırada kaldırmaya çalışır:

* Tüm süre dolmamış öğeler.
* Önceliğe göre öğeler. Önce en düşük öncelik öğeleri kaldırılır.
* En son kullanılan nesneler.
* En erken mutlak bitiş tarihi olan öğeler.
* En erken Kayan süre sonu olan öğeler.

@No__t-0 önceliğiyle sabitlenmiş öğeler hiçbir şekilde kaldırılmaz.

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

Daha fazla bilgi için bkz. [GitHub 'Da Compact Source](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) .

## <a name="cache-dependencies"></a>Önbellek bağımlılıkları

Aşağıdaki örnek, bağımlı bir girdinin süresi dolduğunda önbellek girişinin süresinin dolacağını gösterir. Önbelleğe alınmış öğeye bir `CancellationChangeToken` eklenir. @No__t-0 `CancellationTokenSource` ' de çağrıldığında, her iki önbellek girişi de kaldırılır.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

@No__t-0 kullanmak, birden çok önbellek girişinin bir grup olarak çıkarıyapılmasına izin verir. Yukarıdaki kodda `using` düzeniyle, `using` bloğunun içinde oluşturulan önbellek girdileri Tetikleyiciler ve süre sonu ayarlarını devralacak.

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

::: moniker-end
