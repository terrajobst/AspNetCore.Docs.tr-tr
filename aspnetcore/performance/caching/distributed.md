---
title: Dağıtılmış bir önbellekte ASP.NET Core ile çalışma
author: ardalis
description: Dağıtılmış ASP.NET Core uygulaması performans ve ölçeklenebilirlik, özellikle bir Bulutu vea sunucusu grubu ortamında artırmak için önbelleğe alma kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 9c41a6e008045231bd2e1c1f53a9161e11daafa9
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123846"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="d6567-103">Dağıtılmış bir önbellekte ASP.NET Core ile çalışma</span><span class="sxs-lookup"><span data-stu-id="d6567-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="d6567-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d6567-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d6567-105">Dağıtılmış önbellek, özellikle bulutta veya sunucu grubu içinde barındırıldığında ASP.NET Core uygulamaları, ölçeklenebilirliğini ve performansı artırabilir.</span><span class="sxs-lookup"><span data-stu-id="d6567-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="d6567-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6567-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="d6567-107">Dağıtılmış önbellek nedir</span><span class="sxs-lookup"><span data-stu-id="d6567-107">What is a distributed cache</span></span>

<span data-ttu-id="d6567-108">Dağıtılmış önbellek birden çok uygulama sunucuları tarafından paylaşılan (bkz [önbellek Temelleri](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="d6567-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="d6567-109">Tek tek web sunucuları bellekte önbellekte bilgi saklanmaz ve önbelleğe alınan verilerin tüm uygulama sunucuları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6567-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="d6567-110">Bu, çeşitli avantajlar sağlar:</span><span class="sxs-lookup"><span data-stu-id="d6567-110">This provides several advantages:</span></span>

1. <span data-ttu-id="d6567-111">Önbelleğe alınan tüm web sunucularında tutarlı verilerdir.</span><span class="sxs-lookup"><span data-stu-id="d6567-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="d6567-112">Kullanıcıların bağlı olarak hangi web sunucusu isteği işleme farklı sonuçlar göremiyorum</span><span class="sxs-lookup"><span data-stu-id="d6567-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="d6567-113">Önbelleğe alınmış veriler, web sunucu yeniden başlatılır ve dağıtımları devam eder.</span><span class="sxs-lookup"><span data-stu-id="d6567-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="d6567-114">Tek tek web sunucuları, kaldırıldı veya önbellek etkilemeden eklendi.</span><span class="sxs-lookup"><span data-stu-id="d6567-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="d6567-115">Kaynak veri deposu, kendisine (birden çok bellek içi önbellekler veya no ile tüm önbelleğe alma çok) daha az istekleri sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d6567-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-116">Bu avantajlar bazıları yalnızca bir SQL Server dağıtılmış önbellek kullanıyorsanız, önbellek için uygulamanın kaynak verileri için ayrı bir veritabanı kullanılıyorsa true.</span><span class="sxs-lookup"><span data-stu-id="d6567-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="d6567-117">Herhangi bir önbellek gibi dağıtılmış bir önbellek genellikle veri bir ilişkisel veritabanı (veya web hizmeti) çok daha hızlı bir şekilde önbelleğe alınabilir olduğundan bir uygulamanın yanıt verme hızını, önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="d6567-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="d6567-118">Önbellek, uygulama belirli yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="d6567-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="d6567-119">Bu makalede, SQL Server dağıtılmış önbelleğe alır ve nasıl yapılandırmak için her ikisi de Redis açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d6567-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="d6567-120">Hangi uygulamayı bağımsız olarak işaretlenirse, uygulama yaygın bir önbellek ile etkileşime giren `IDistributedCache` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="d6567-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="d6567-121">IDistributedCache arabirimi</span><span class="sxs-lookup"><span data-stu-id="d6567-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="d6567-122">`IDistributedCache` Arabirimi zaman uyumlu ve zaman uyumsuz yöntemler içerir.</span><span class="sxs-lookup"><span data-stu-id="d6567-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="d6567-123">Arabirim öğeleri eklenen, alınan ve dağıtılmış önbellek uygulamasından kaldırıldı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6567-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="d6567-124">`IDistributedCache` Arabirimi aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="d6567-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="d6567-125">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="d6567-125">**Get, GetAsync**</span></span>

<span data-ttu-id="d6567-126">Bir dize anahtarı alır ve önbelleğe alınan öğe olarak alır. bir `byte[]` , önbellekte bulundu.</span><span class="sxs-lookup"><span data-stu-id="d6567-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="d6567-127">**SetAsync kümesi**</span><span class="sxs-lookup"><span data-stu-id="d6567-127">**Set, SetAsync**</span></span>

<span data-ttu-id="d6567-128">Bir öğe ekler (olarak `byte[]`) bir dize anahtarı kullanarak önbelleğe.</span><span class="sxs-lookup"><span data-stu-id="d6567-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="d6567-129">**Yenileme, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="d6567-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="d6567-130">Kendi kayan zaman aşımı (varsa) sıfırlama kendi anahtarını temel alan önbellekteki bir öğeyi yeniler.</span><span class="sxs-lookup"><span data-stu-id="d6567-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="d6567-131">**RemoveAsync Kaldır**</span><span class="sxs-lookup"><span data-stu-id="d6567-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="d6567-132">Kendi anahtarını temel alan bir önbellek girdisi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="d6567-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="d6567-133">Kullanılacak `IDistributedCache` arabirimi:</span><span class="sxs-lookup"><span data-stu-id="d6567-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="d6567-134">Gerekli NuGet paketlerini proje dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6567-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="d6567-135">Özel uygulanışı yapılandırma `IDistributedCache` içinde `Startup` sınıfın `ConfigureServices` yöntemi ve kapsayıcıya var. ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6567-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="d6567-136">Uygulamanın gelen [ara yazılım](xref:fundamentals/middleware/index) veya MVC denetleyici sınıflarına istek örneği `IDistributedCache` oluşturucudan.</span><span class="sxs-lookup"><span data-stu-id="d6567-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="d6567-137">Örneği tarafından sağlanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı).</span><span class="sxs-lookup"><span data-stu-id="d6567-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="d6567-138">Bir Tekliyi veya kapsamındaki ömrü boyunca kullanılacak gerek yoktur `IDistributedCache` örnekleri (en az yerleşik uygulamalar için).</span><span class="sxs-lookup"><span data-stu-id="d6567-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="d6567-139">Bir gereksinim duyabileceğiniz her yerde örneği de oluşturabilirsiniz (kullanmak yerine [bağımlılık ekleme](../../fundamentals/dependency-injection.md)), ancak bu kodunuzu test etmek daha zor hale getirebilir ve ihlal [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="d6567-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="d6567-140">Aşağıdaki örnek, bir örneğini kullanması gösterilmektedir `IDistributedCache` basit bir ara yazılım bileşeni içinde:</span><span class="sxs-lookup"><span data-stu-id="d6567-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="d6567-141">Yukarıdaki kodda, önbelleğe alınan değeri okuyun, ancak hiçbir zaman yazılır.</span><span class="sxs-lookup"><span data-stu-id="d6567-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="d6567-142">Bu örnekte, bir sunucu başlatıldığında ve değişmez değer yalnızca ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d6567-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="d6567-143">Çok sunuculu bir senaryoda, diğer sunucular tarafından ayarlanan herhangi bir önceki değeri başlatmak için en son sunucu üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="d6567-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="d6567-144">`Get` Ve `Set` yöntemleri `byte[]` türü.</span><span class="sxs-lookup"><span data-stu-id="d6567-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="d6567-145">Bu nedenle, dize değeri kullanarak dönüştürülmelidir `Encoding.UTF8.GetString` (için `Get`) ve `Encoding.UTF8.GetBytes` (için `Set`).</span><span class="sxs-lookup"><span data-stu-id="d6567-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="d6567-146">Aşağıdaki kodu *Startup.cs* ayarlanan değer gösterir:</span><span class="sxs-lookup"><span data-stu-id="d6567-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="d6567-147">Bu yana `IDistributedCache` yapılandırılan `ConfigureServices` yöntemi, kullanılabilir `Configure` yönteme bir parametre olarak.</span><span class="sxs-lookup"><span data-stu-id="d6567-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="d6567-148">Bir parametre olarak ekleme DI sağlanacak yapılandırılmış örneği izin verir.</span><span class="sxs-lookup"><span data-stu-id="d6567-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="d6567-149">Dağıtılmış bir Redis önbelleği kullanma</span><span class="sxs-lookup"><span data-stu-id="d6567-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="d6567-150">[Redis](https://redis.io/) dağıtılmış bir önbellek sık kullanılan bir açık kaynak bellek içi veri deposu.</span><span class="sxs-lookup"><span data-stu-id="d6567-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="d6567-151">Yerel olarak kullanabilirsiniz ve yapılandırabileceğiniz bir [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure'da barındırılan ASP.NET Core uygulamalarınız için.</span><span class="sxs-lookup"><span data-stu-id="d6567-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="d6567-152">Önbellek uygulaması kullanarak ASP.NET Core uygulamanızı yapılandırır bir `RedisDistributedCache` örneği.</span><span class="sxs-lookup"><span data-stu-id="d6567-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="d6567-153">Redis cache gerektirir [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="d6567-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="d6567-154">Redis uygulamasında yapılandırma `ConfigureServices` ve uygulama kodunuzda bir örneğini isteyerek erişim `IDistributedCache` (Yukarıdaki kod bakın).</span><span class="sxs-lookup"><span data-stu-id="d6567-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="d6567-155">Örnek kodda bir `RedisCache` uygulama kullanılan sunucu için yapılandırıldığında bir `Staging` ortam.</span><span class="sxs-lookup"><span data-stu-id="d6567-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="d6567-156">Bu nedenle `ConfigureStagingServices` yöntemi yapılandırır `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="d6567-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="d6567-157">Yerel makinenizde Redis yüklemek için chocolatey paket yükleme [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) çalıştırıp `redis-server` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="d6567-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="d6567-158">SQL Server'ı kullanarak dağıtılmış önbellek</span><span class="sxs-lookup"><span data-stu-id="d6567-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="d6567-159">SqlServerCache uygulama, yedekleme deposu bir SQL Server veritabanını kullanmak dağıtılmış bir önbellek sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6567-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="d6567-160">SQL Server'ı oluşturmak için belirttiğiniz ad ve şema ile sql önbelleği aracı, Aracı'nı kullanabilirsiniz. Tablo bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d6567-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="d6567-161">Ekleme `SqlConfig.Tools` için `<ItemGroup>` proje dosyası ve çalışma öğesi `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="d6567-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="d6567-162">Aşağıdaki komutu çalıştırarak SqlConfig.Tools test edin:</span><span class="sxs-lookup"><span data-stu-id="d6567-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="d6567-163">SqlConfig.Tools kullanım, Seçenekler ve komut Yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="d6567-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="d6567-164">Çalıştırarak SQL Server'da bir tablo oluşturma `sql-cache create` komutu:</span><span class="sxs-lookup"><span data-stu-id="d6567-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="d6567-165">Aşağıdaki şemayı oluşturduğunuz bir tabloya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="d6567-165">The created table has the following schema:</span></span>

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="d6567-167">Tüm önbellek uygulaması gibi uygulamanız get ve set örneğini kullanarak önbellek değerleri `IDistributedCache`değil bir `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="d6567-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="d6567-168">Örnek uygulayan `SqlServerCache` üretim ortamında (olarak yapılandırıldığı şekilde `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="d6567-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="d6567-169">`ConnectionString` (Ve isteğe bağlı olarak `SchemaName` ve `TableName`) kimlik bilgilerini olabileceğinden genellikle (örneğin, UserSecrets), kaynak denetimi dışında depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6567-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="d6567-170">Önerileri</span><span class="sxs-lookup"><span data-stu-id="d6567-170">Recommendations</span></span>

<span data-ttu-id="d6567-171">Hangi uygulamasının verirken `IDistributedCache` SQL Server tabanlı mevcut altyapınıza ve ortam, performans gereksinimlerinizi ve takımınızın deneyimi ve Redis arasında doğrudan uygulamanız için seçin.</span><span class="sxs-lookup"><span data-stu-id="d6567-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="d6567-172">Takımınız, Redis ile çalışma konusunda daha yetkin ise, mükemmel bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="d6567-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="d6567-173">SQL Server takımınızın tercih ettiği, bu uygulamada emin olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d6567-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="d6567-174">Geleneksel bir önbellek çözümü veri sağlayan hızlı verilerinin alınması için bellek içi içerdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d6567-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="d6567-175">Bir ön bellekte yaygın olarak kullanılan veri depolama ve SQL Server veya Azure depolama gibi bir arka uç kalıcı depoya tüm verileri depolar gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6567-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="d6567-176">Redis önbelleği SQL önbellek karşılaştırıldığında yüksek çıkış ve düşük gecikme sunan bir önbelleğe alma çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="d6567-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6567-177">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6567-177">Additional resources</span></span>

* [<span data-ttu-id="d6567-178">Redis Cache azure'da</span><span class="sxs-lookup"><span data-stu-id="d6567-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="d6567-179">Azure'da SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="d6567-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
