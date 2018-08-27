---
title: Belleğe yüklenmiş önbellek ASP.NET core'da
author: rick-anderson
description: ASP.NET Core bellekte önbelleğe öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2018
uid: performance/caching/memory
ms.openlocfilehash: 091d00ca7a30b61bdd83618e055bf23e0f2753c4
ms.sourcegitcommit: 67a0a04ebb3b21c826e5b9600bacfc897abd6a46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42899850"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="61738-103">Belleğe yüklenmiş önbellek ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="61738-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="61738-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="61738-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="61738-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61738-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="61738-106">Temel bilgileri önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="61738-106">Caching basics</span></span>

<span data-ttu-id="61738-107">Önbelleğe alma işlemi önemli ölçüde performans ve ölçeklenebilirlik, bir uygulamanın içeriği oluşturmak için gereken iş azaltarak artırabilir.</span><span class="sxs-lookup"><span data-stu-id="61738-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="61738-108">Seyrek değişen verilerle iyi önbelleğe alma çalışır.</span><span class="sxs-lookup"><span data-stu-id="61738-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="61738-109">Önbelleğe alma, çok döndürülen verilerin bir kopyasını yapar özgün kaynaktan daha hızlı.</span><span class="sxs-lookup"><span data-stu-id="61738-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="61738-110">Yazın ve hiçbir zaman önbelleğe alınmış veri bağımlı uygulamanızı test etmek.</span><span class="sxs-lookup"><span data-stu-id="61738-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="61738-111">ASP.NET Core, birkaç farklı önbellek destekler.</span><span class="sxs-lookup"><span data-stu-id="61738-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="61738-112">En basit önbellek dayanır [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), web sunucusu bellekte bir önbellek temsil eder.</span><span class="sxs-lookup"><span data-stu-id="61738-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="61738-113">Bir sunucu grubu birden çok sunucu üzerinde çalışan uygulamalar oturumları bellek içi önbelleği kullanırken Yapışkan emin olmalısınız.</span><span class="sxs-lookup"><span data-stu-id="61738-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="61738-114">Yapışkan oturumlar tüm istemciden gelen sonraki istekleri aynı sunucuya gittiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="61738-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="61738-115">Örneğin, Azure Web apps kullanımını [uygulama isteği yönlendirme](https://www.iis.net/learn/extensions/planning-for-arr) sonraki isteklere aynı sunucuya yönlendirmek için (ARR).</span><span class="sxs-lookup"><span data-stu-id="61738-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="61738-116">Bir web grubunda olmayan Yapışkan oturumlar gerektiren bir [dağıtılmış önbellek](distributed.md) önbellek tutarlılığı sorunları önlemek için.</span><span class="sxs-lookup"><span data-stu-id="61738-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="61738-117">Bazı uygulamalar için bir bellek içi önbelleğe göre daha yüksek ölçeği genişletilmiş dağıtılmış önbellek destekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="61738-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="61738-118">Bir paylaşılan önbellek kullanarak, bir dış işlem önbelleği üzerinizden alır.</span><span class="sxs-lookup"><span data-stu-id="61738-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="61738-119">`IMemoryCache` Sürece, önbelleği önbellek girişlerinin bellek baskısı altında Tahliye [önbelleğe öncelik](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) ayarlanır `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="61738-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="61738-120">Ayarlayabileceğiniz `CacheItemPriority` önbellek çıkarır bellek baskısı altında öğeleri önceliğini ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="61738-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="61738-121">Bellek içi önbellek, herhangi bir nesne kaydedebilir; Dağıtılmış önbellek arabirimi sınırlı olan `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="61738-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

### <a name="cache-guidelines"></a><span data-ttu-id="61738-122">Önbellek yönergeleri</span><span class="sxs-lookup"><span data-stu-id="61738-122">Cache guidelines</span></span>

* <span data-ttu-id="61738-123">Kod, verileri getirmek için bir geri dönüş seçeneği her zaman olmalıdır ve **değil** kullanılabilir olan bir önbelleğe alınan değeri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="61738-123">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="61738-124">Önbellek bellek nadir kaynak kullanır.</span><span class="sxs-lookup"><span data-stu-id="61738-124">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="61738-125">Önbellek büyüme sınırla:</span><span class="sxs-lookup"><span data-stu-id="61738-125">Limit cache growth:</span></span>
  * <span data-ttu-id="61738-126">Yapmak **değil** dış girdi önbellek anahtarları kullanın.</span><span class="sxs-lookup"><span data-stu-id="61738-126">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="61738-127">Süre sonu önbellek büyümesini sınırlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="61738-127">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="61738-128">SetSize, boyutu ve SizeLimit önbellek boyutunu sınırlamak için kullanın</span><span class="sxs-lookup"><span data-stu-id="61738-128">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="61738-129">IMemoryCache kullanma</span><span class="sxs-lookup"><span data-stu-id="61738-129">Using IMemoryCache</span></span>

<span data-ttu-id="61738-130">Bellek içi önbelleğe alma bir *hizmet* , uygulama kullanımından başvurduğu [bağımlılık ekleme](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="61738-130">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="61738-131">Çağrı `AddMemoryCache` içinde `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="61738-131">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="61738-132">İstek `IMemoryCache` oluşturucu örneği:</span><span class="sxs-lookup"><span data-stu-id="61738-132">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="61738-133">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="61738-133">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="61738-134">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="61738-134">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="61738-135">`IMemoryCache` NuGet paketi için gerekli [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), kullanılabilir olduğu [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="61738-135">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="61738-136">Aşağıdaki kod [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) bir süre önbellekte olup olmadığını denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="61738-136">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="61738-137">Bir süre önbelleğe, yeni bir girdi oluşturulur ve önbelleğe eklenen [ayarlamak](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="61738-137">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="61738-138">Geçerli saat ve önbelleğe alınan zaman görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="61738-138">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="61738-139">Önbelleğe alınan `DateTime` değeri, zaman aşımı süresi (ve bellek baskısı nedeniyle hiçbir çıkarma) içinde istekler varken önbellekte kalır.</span><span class="sxs-lookup"><span data-stu-id="61738-139">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="61738-140">Aşağıdaki görüntüde, geçerli saat ve önbellekten daha eski bir zaman gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="61738-140">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Dizin görünümünün görüntülenen iki farklı saatler](memory/_static/time.png)

<span data-ttu-id="61738-142">Aşağıdaki kod [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) ve [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) verileri önbelleğe almak için.</span><span class="sxs-lookup"><span data-stu-id="61738-142">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="61738-143">Aşağıdaki kod çağrıları [alma](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) önbelleğe alınmış zaman getirilemedi:</span><span class="sxs-lookup"><span data-stu-id="61738-143">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="61738-144">Bkz: [IMemoryCache yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) ve [CacheExtensions yöntemleri](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) önbellek yöntemleri açıklaması.</span><span class="sxs-lookup"><span data-stu-id="61738-144">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="61738-145">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="61738-145">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="61738-146">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="61738-146">The following sample:</span></span>

- <span data-ttu-id="61738-147">Mutlak zaman aşımı süresi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="61738-147">Sets the absolute expiration time.</span></span> <span data-ttu-id="61738-148">Bu giriş önbelleğe alınacak en uzun süreyi ve öğe olmaadığını sürekli olarak yenilendiğinde çok eski duruma gelmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="61738-148">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="61738-149">Kayan bir sona erme saati ayarlar.</span><span class="sxs-lookup"><span data-stu-id="61738-149">Sets a sliding expiration time.</span></span> <span data-ttu-id="61738-150">Önbelleğe alınan bu öğenin erişim istekleri kayan sona erme saati sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="61738-150">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="61738-151">Önbellek önceliği ayarlar `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="61738-151">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="61738-152">Ayarlar bir [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , çağrılacağı sonra giriş önbellekten çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="61738-152">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="61738-153">Geri çağırma öğeyi önbellekten kaldırır kodundan farklı bir iş parçacığı üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="61738-153">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="61738-154">SetSize, boyutu ve SizeLimit önbellek boyutunu sınırlamak için kullanın</span><span class="sxs-lookup"><span data-stu-id="61738-154">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="61738-155">A `MemoryCache` örnek isteğe bağlı olarak belirtin ve boyut sınırını uygular.</span><span class="sxs-lookup"><span data-stu-id="61738-155">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="61738-156">Önbellek giriş boyutu ölçmek için bir mekanizma bulunmadığından olduğundan bellek boyutu sınırı tanımlı ölçü yok.</span><span class="sxs-lookup"><span data-stu-id="61738-156">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="61738-157">Önbellek boyutu sınırı ayarlarsanız, tüm girişleri boyutu belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61738-157">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="61738-158">Belirtilen birim Geliştirici seçer boyutudur.</span><span class="sxs-lookup"><span data-stu-id="61738-158">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="61738-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="61738-159">For example:</span></span>

* <span data-ttu-id="61738-160">Web uygulaması dizeleri öncelikle önbelleğe almayı her önbellek giriş boyutu dize uzunluğu olabilir.</span><span class="sxs-lookup"><span data-stu-id="61738-160">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="61738-161">Uygulamanın tüm girişleri boyutu 1 belirtebilirsiniz ve boyut sınırını girişleri sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="61738-161">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="61738-162">Aşağıdaki kod boyutu sabit bir unitless oluşturur [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) tarafından erişilebilen [bağımlılık ekleme](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="61738-162">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="61738-163">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) birim yok.</span><span class="sxs-lookup"><span data-stu-id="61738-163">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="61738-164">Önbelleğe alınan girişlerin boyutu ne olursa olsun birimleri, önbellek boyutunu ayarlarsanız en uygun bulduğunuz de belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61738-164">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="61738-165">Bir önbellek örneği, tüm kullanıcılar, aynı birim sistemi kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="61738-165">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="61738-166">Önbelleğe alınan giriş boyutlarının toplamı tarafından belirtilen değeri aşıyorsa bir giriş önbelleğe alınmamış `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="61738-166">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="61738-167">Önbellek boyut sınırı olarak ayarlanırsa, önbellek boyutu girişinde ayarını yoksayılacak.</span><span class="sxs-lookup"><span data-stu-id="61738-167">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="61738-168">Aşağıdaki kod kayıtları `MyMemoryCache` ile [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="61738-168">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="61738-169">`MyMemoryCache` Bu boyutu sınırlı önbelleğini farkında olduğundan ve nasıl önbellek giriş boyutu uygun şekilde ayarlanacağını bilmek bileşenleri için bağımsız bir önbellek olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="61738-169">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="61738-170">Aşağıdaki kod `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="61738-170">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="61738-171">Önbellek giriş boyutu ayarlanabilir [boyutu](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) veya [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="61738-171">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="61738-172">Önbellek bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="61738-172">Cache dependencies</span></span>

<span data-ttu-id="61738-173">Aşağıdaki örnek, bir önbellek girdisi bağımlı giriş süresi dolarsa sona gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="61738-173">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="61738-174">A `CancellationChangeToken` önbelleğe eklenir.</span><span class="sxs-lookup"><span data-stu-id="61738-174">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="61738-175">Zaman `Cancel` üzerinde çağrılır `CancellationTokenSource`, her iki önbellek girişlerinin çıkarılacak.</span><span class="sxs-lookup"><span data-stu-id="61738-175">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="61738-176">Kullanarak bir `CancellationTokenSource` grup olarak çıkarılacak birden fazla önbellek girişi sağlar.</span><span class="sxs-lookup"><span data-stu-id="61738-176">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="61738-177">İle `using` önbellek girdisi Yukarıdaki kod modelinde oluşturulan içinde `using` blok, tetikleyiciler ve sona erme ayarları devralacaktır.</span><span class="sxs-lookup"><span data-stu-id="61738-177">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="61738-178">Ek Notlar</span><span class="sxs-lookup"><span data-stu-id="61738-178">Additional notes</span></span>

- <span data-ttu-id="61738-179">Bir önbellek öğesi yeniden doldurmak için bir geri çağırma kullanırken:</span><span class="sxs-lookup"><span data-stu-id="61738-179">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="61738-180">Birden çok istek geri çağırma henüz tamamlanmadığından önbelleğe alınan anahtar değeri boş bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61738-180">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="61738-181">Bu, önbelleğe alınan öğeyi yeniden birkaç iş parçacığı neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="61738-181">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="61738-182">Bir önbellek girdisi başka oluşturmak için kullanıldığında, alt üst girişin sona erme belirteçleri ve zamana bağlı süre sonu ayarları kopyalar.</span><span class="sxs-lookup"><span data-stu-id="61738-182">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="61738-183">Alt tarafından el ile temizleme süresi dolmuş veya üst girişinin güncelleştirme yok.</span><span class="sxs-lookup"><span data-stu-id="61738-183">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="61738-184">Kullanım [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) önbellek girdisi önbellekten çıkarıldığı sonra harekete geçirilmez geri çağırmaları ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="61738-184">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61738-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="61738-185">Additional resources</span></span>

* [<span data-ttu-id="61738-186">Dağıtılmış önbellekle çalışma</span><span class="sxs-lookup"><span data-stu-id="61738-186">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="61738-187">Değişiklik belirteçleri ile değişiklikleri algılama</span><span class="sxs-lookup"><span data-stu-id="61738-187">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="61738-188">Yanıtları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="61738-188">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="61738-189">Yanıtları Önbelleğe Alma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="61738-189">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="61738-190">Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="61738-190">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="61738-191">Dağıtılmış Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="61738-191">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
