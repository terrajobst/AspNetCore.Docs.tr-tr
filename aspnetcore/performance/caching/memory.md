---
title: Belleğe yüklenmiş önbellek ASP.NET core'da
author: rick-anderson
description: ASP.NET Core bellekte önbelleğe öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 6433df36023b79bc679186bee8b0a92371661dbe
ms.sourcegitcommit: 258a97159da206f9009f23fdf6f8fa32f178e50b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425055"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="c63f1-103">Belleğe yüklenmiş önbellek ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="c63f1-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="c63f1-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c63f1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c63f1-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c63f1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="c63f1-106">Temel bilgileri önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="c63f1-106">Caching basics</span></span>

<span data-ttu-id="c63f1-107">Önbelleğe alma işlemi önemli ölçüde performans ve ölçeklenebilirlik, bir uygulamanın içeriği oluşturmak için gereken iş azaltarak artırabilir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="c63f1-108">Seyrek değişen verilerle iyi önbelleğe alma çalışır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="c63f1-109">Önbelleğe alma, çok döndürülen verilerin bir kopyasını yapar özgün kaynaktan daha hızlı.</span><span class="sxs-lookup"><span data-stu-id="c63f1-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="c63f1-110">Uygulamaları yazılan verilecek ve için test **hiçbir zaman** önbelleğe alınmış veri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-110">Apps should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="c63f1-111">ASP.NET Core, birkaç farklı önbellek destekler.</span><span class="sxs-lookup"><span data-stu-id="c63f1-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="c63f1-112">En basit önbellek dayanır [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), web sunucusu bellekte bir önbellek temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c63f1-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="c63f1-113">Bir sunucu grubunda birden çok sunucu üzerinde çalışan uygulamalar oturumları bellek içi önbelleği kullanırken Yapışkan emin olmalısınız.</span><span class="sxs-lookup"><span data-stu-id="c63f1-113">Apps that run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="c63f1-114">Yapışkan oturumlar tüm istemciden gelen sonraki istekleri aynı sunucuya gittiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="c63f1-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="c63f1-115">Örneğin, Azure Web apps kullanımını [uygulama isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) sonraki isteklere aynı sunucuya yönlendirmek için (ARR).</span><span class="sxs-lookup"><span data-stu-id="c63f1-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="c63f1-116">Bir web grubunda olmayan Yapışkan oturumlar gerektiren bir [dağıtılmış önbellek](distributed.md) önbellek tutarlılığı sorunları önlemek için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="c63f1-117">Bazı uygulamalar için bir bellek içi önbelleğe göre daha yüksek ölçeği genişletilmiş dağıtılmış önbellek destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="c63f1-118">Bir paylaşılan önbellek kullanarak, bir dış işlem önbelleği üzerinizden alır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c63f1-119">`IMemoryCache` Sürece, önbelleği önbellek girişlerinin bellek baskısı altında Tahliye [önbelleğe öncelik](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ayarlanır `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="c63f1-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="c63f1-120">Ayarlayabileceğiniz `CacheItemPriority` önbellek çıkarır bellek baskısı altında öğeleri önceliğini ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="c63f1-121">Bellek içi önbellek, herhangi bir nesne kaydedebilir; Dağıtılmış önbellek arabirimi sınırlı olan `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c63f1-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="c63f1-122">Anahtar-değer çiftleri olarak bellek içinde ve dağıtılmış önbellek deposu önbellek öğeleri.</span><span class="sxs-lookup"><span data-stu-id="c63f1-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="c63f1-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="c63f1-123">System.Runtime.Caching/MemoryCache</span></span>

<xref:System.Runtime.Caching><span data-ttu-id="c63f1-124">/<xref:System.Runtime.Caching.MemoryCache> ([NuGet paketini](https://www.nuget.org/packages/System.Runtime.Caching/)) ile birlikte kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="c63f1-124">/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="c63f1-125">.NET standard 2.0 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="c63f1-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="c63f1-126">Tüm [.NET uygulaması](/dotnet/standard/net-standard#net-implementation-support) hedefleyen .NET Standard 2.0 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="c63f1-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="c63f1-127">Örneğin, ASP.NET Core 2.0 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="c63f1-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="c63f1-128">.NET framework 4.5 veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="c63f1-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="c63f1-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (Bu makalede açıklanan) üzerinden önerilen `System.Runtime.Caching` / `MemoryCache` daha iyi ASP.NET Core ile tümleşik olduğundan.</span><span class="sxs-lookup"><span data-stu-id="c63f1-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="c63f1-130">Örneğin, `IMemoryCache` çalıştığı yerel olarak ASP.NET Core ile [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c63f1-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="c63f1-131">Kullanım `System.Runtime.Caching` / `MemoryCache` ASP.NET koddan taşırken uyumluluk köprü olarak 4.x ASP.NET Core için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="c63f1-132">Önbellek yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c63f1-132">Cache guidelines</span></span>

* <span data-ttu-id="c63f1-133">Kod, verileri getirmek için bir geri dönüş seçeneği her zaman olmalıdır ve **değil** kullanılabilir olan bir önbelleğe alınan değeri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="c63f1-134">Önbellek bellek nadir kaynak kullanır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="c63f1-135">Önbellek büyüme sınırla:</span><span class="sxs-lookup"><span data-stu-id="c63f1-135">Limit cache growth:</span></span>
  * <span data-ttu-id="c63f1-136">Yapmak **değil** dış girdi önbellek anahtarları kullanın.</span><span class="sxs-lookup"><span data-stu-id="c63f1-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="c63f1-137">Süre sonu önbellek büyümesini sınırlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c63f1-137">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="c63f1-138">[Önbellek boyutunu sınırlamak için SetSize, boyutu ve SizeLimit kullanın](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span><span class="sxs-lookup"><span data-stu-id="c63f1-138">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="c63f1-139">ASP.NET Core çalışma zamanı bellek Basıncı önbellek boyutunu sınırlamaz.</span><span class="sxs-lookup"><span data-stu-id="c63f1-139">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="c63f1-140">Bu önbellek boyutunu sınırlamak için geliştirici aittir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-140">It's up to the developer to limit cache size.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="c63f1-141">IMemoryCache kullanma</span><span class="sxs-lookup"><span data-stu-id="c63f1-141">Using IMemoryCache</span></span>

<span data-ttu-id="c63f1-142">Bellek içi önbelleğe alma bir *hizmet* , uygulama kullanımından başvurduğu [bağımlılık ekleme](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c63f1-142">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="c63f1-143">Çağrı `AddMemoryCache` içinde `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c63f1-143">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="c63f1-144">İstek `IMemoryCache` oluşturucu örneği:</span><span class="sxs-lookup"><span data-stu-id="c63f1-144">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c63f1-145">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="c63f1-145">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c63f1-146">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c63f1-146">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="c63f1-147">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c63f1-147">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="c63f1-148">Aşağıdaki kod [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) bir süre önbellekte olup olmadığını denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-148">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="c63f1-149">Bir süre önbelleğe, yeni bir girdi oluşturulur ve önbelleğe eklenen [ayarlamak](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="c63f1-149">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="c63f1-150">Geçerli saat ve önbelleğe alınan zaman görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="c63f1-150">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="c63f1-151">Önbelleğe alınan `DateTime` değeri, zaman aşımı süresi içinde istekler varken önbellekte kalır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-151">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span> <span data-ttu-id="c63f1-152">Aşağıdaki görüntüde, geçerli saat ve önbellekten daha eski bir zaman gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c63f1-152">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Dizin görünümünün görüntülenen iki farklı saatler](memory/_static/time.png)

<span data-ttu-id="c63f1-154">Aşağıdaki kod [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) verileri önbelleğe almak için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-154">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="c63f1-155">Aşağıdaki kod çağrıları [alma](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) önbelleğe alınmış zaman getirilemedi:</span><span class="sxs-lookup"><span data-stu-id="c63f1-155">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> <span data-ttu-id="c63f1-156">, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, ve [alma](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) uzantı yöntemleri parçası olan [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) kapasitesini genişleten sınıfı <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="c63f1-156">, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="c63f1-157">Bkz: [IMemoryCache yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [CacheExtensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) diğer önbellek yöntemleri açıklaması.</span><span class="sxs-lookup"><span data-stu-id="c63f1-157">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="c63f1-158">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="c63f1-158">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="c63f1-159">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="c63f1-159">The following sample:</span></span>

* <span data-ttu-id="c63f1-160">Mutlak zaman aşımı süresi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c63f1-160">Sets the absolute expiration time.</span></span> <span data-ttu-id="c63f1-161">Bu giriş önbelleğe alınacak en uzun süreyi ve öğe olmaadığını sürekli olarak yenilendiğinde çok eski duruma gelmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="c63f1-161">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
* <span data-ttu-id="c63f1-162">Kayan bir sona erme saati ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c63f1-162">Sets a sliding expiration time.</span></span> <span data-ttu-id="c63f1-163">Önbelleğe alınan bu öğenin erişim istekleri kayan sona erme saati sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="c63f1-163">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="c63f1-164">Önbellek önceliği ayarlar `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="c63f1-164">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
* <span data-ttu-id="c63f1-165">Ayarlar bir [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , çağrılacağı sonra giriş önbellekten çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-165">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="c63f1-166">Geri çağırma öğeyi önbellekten kaldırır kodundan farklı bir iş parçacığı üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c63f1-166">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="c63f1-167">SetSize, boyutu ve SizeLimit önbellek boyutunu sınırlamak için kullanın</span><span class="sxs-lookup"><span data-stu-id="c63f1-167">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="c63f1-168">A `MemoryCache` örnek isteğe bağlı olarak belirtin ve boyut sınırını uygular.</span><span class="sxs-lookup"><span data-stu-id="c63f1-168">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="c63f1-169">Önbellek giriş boyutu ölçmek için bir mekanizma bulunmadığından olduğundan bellek boyutu sınırı tanımlı ölçü yok.</span><span class="sxs-lookup"><span data-stu-id="c63f1-169">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="c63f1-170">Önbellek boyutu sınırı ayarlarsanız, tüm girişleri boyutu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-170">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="c63f1-171">ASP.NET Core çalışma zamanı bellek Basıncı önbellek boyutunu sınırlamaz.</span><span class="sxs-lookup"><span data-stu-id="c63f1-171">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="c63f1-172">Bu önbellek boyutunu sınırlamak için geliştirici aittir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-172">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="c63f1-173">Belirtilen birim Geliştirici seçer boyutudur.</span><span class="sxs-lookup"><span data-stu-id="c63f1-173">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="c63f1-174">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="c63f1-174">For example:</span></span>

* <span data-ttu-id="c63f1-175">Web uygulaması dizeleri öncelikle önbelleğe almayı her önbellek giriş boyutu dize uzunluğu olabilir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-175">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="c63f1-176">Uygulamanın tüm girişleri boyutu 1 belirtebilirsiniz ve boyut sınırını girişleri sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-176">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="c63f1-177">Aşağıdaki kod boyutu sabit bir unitless oluşturur [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) tarafından erişilebilen [bağımlılık ekleme](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="c63f1-177">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="c63f1-178">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) birim yok.</span><span class="sxs-lookup"><span data-stu-id="c63f1-178">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="c63f1-179">Önbelleğe alınan girişlerin boyutu ne olursa olsun birimleri, önbellek boyutunu ayarlarsanız en uygun bulduğunuz de belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-179">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="c63f1-180">Bir önbellek örneği, tüm kullanıcılar, aynı birim sistemi kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-180">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="c63f1-181">Önbelleğe alınan giriş boyutlarının toplamı tarafından belirtilen değeri aşıyorsa bir giriş önbelleğe alınmamış `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="c63f1-181">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="c63f1-182">Önbellek boyut sınırı olarak ayarlanırsa, önbellek boyutu girişinde ayarını yoksayılacak.</span><span class="sxs-lookup"><span data-stu-id="c63f1-182">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="c63f1-183">Aşağıdaki kod kayıtları `MyMemoryCache` ile [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="c63f1-183">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="c63f1-184">`MyMemoryCache` Bu boyutu sınırlı önbelleğini farkında olduğundan ve nasıl önbellek giriş boyutu uygun şekilde ayarlanacağını bilmek bileşenleri için bağımsız bir önbellek olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c63f1-184">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="c63f1-185">Aşağıdaki kod `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="c63f1-185">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="c63f1-186">Önbellek giriş boyutu ayarlanabilir [boyutu](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) veya [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c63f1-186">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="c63f1-187">Önbellek bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="c63f1-187">Cache dependencies</span></span>

<span data-ttu-id="c63f1-188">Aşağıdaki örnek, bir önbellek girdisi bağımlı giriş süresi dolarsa sona gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-188">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="c63f1-189">A `CancellationChangeToken` önbelleğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-189">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="c63f1-190">Zaman `Cancel` üzerinde çağrılır `CancellationTokenSource`, her iki önbellek girişlerinin çıkarılacak.</span><span class="sxs-lookup"><span data-stu-id="c63f1-190">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="c63f1-191">Kullanarak bir `CancellationTokenSource` grup olarak çıkarılacak birden fazla önbellek girişi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c63f1-191">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="c63f1-192">İle `using` önbellek girdisi Yukarıdaki kod modelinde oluşturulan içinde `using` blok, tetikleyiciler ve sona erme ayarları devralacaktır.</span><span class="sxs-lookup"><span data-stu-id="c63f1-192">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="c63f1-193">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="c63f1-193">Additional notes</span></span>

* <span data-ttu-id="c63f1-194">Bir önbellek öğesi yeniden doldurmak için bir geri çağırma kullanırken:</span><span class="sxs-lookup"><span data-stu-id="c63f1-194">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="c63f1-195">Birden çok istek geri çağırma henüz tamamlanmadığından önbelleğe alınan anahtar değeri boş bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c63f1-195">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="c63f1-196">Bu, önbelleğe alınan öğeyi yeniden birkaç iş parçacığı neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c63f1-196">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="c63f1-197">Bir önbellek girdisi başka oluşturmak için kullanıldığında, alt üst girişin sona erme belirteçleri ve zamana bağlı süre sonu ayarları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="c63f1-197">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="c63f1-198">Alt tarafından el ile temizleme süresi dolmuş veya üst girişinin güncelleştirme yok.</span><span class="sxs-lookup"><span data-stu-id="c63f1-198">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="c63f1-199">Kullanım [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) önbellek girdisi önbellekten çıkarıldığı sonra harekete geçirilmez geri çağırmaları ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="c63f1-199">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c63f1-200">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c63f1-200">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
