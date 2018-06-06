---
title: ASP.NET Core dağıtılmış önbelleğinde ile çalışma
author: ardalis
description: Dağıtılmış ASP.NET Core uygulama performans ve ölçeklenebilirlik, özellikle bir bulut veya sunucu grubu ortamında artırmak için önbelleğe alma kullanmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 6c595572641604d241c0c8f702d4f392afe34f71
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734464"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="e5980-103">ASP.NET Core dağıtılmış önbelleğinde ile çalışma</span><span class="sxs-lookup"><span data-stu-id="e5980-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="e5980-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e5980-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e5980-105">Dağıtılmış önbellek özellikle bir bulut veya sunucu grubu ortamında barındırıldığında ASP.NET Core uygulamaları ölçeklenebilirliğini ve performansı artırabilir.</span><span class="sxs-lookup"><span data-stu-id="e5980-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="e5980-106">Bu makalede, ASP.NET Core'nın yerleşik dağıtılmış önbellek soyutlamalar ve uygulamaları ile nasıl çalışılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e5980-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="e5980-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e5980-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="e5980-108">Dağıtılmış önbellek nedir</span><span class="sxs-lookup"><span data-stu-id="e5980-108">What is a distributed cache</span></span>

<span data-ttu-id="e5980-109">Dağıtılmış önbellek birden çok uygulama sunucuları tarafından paylaşılan (bkz [önbellek Temelleri](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="e5980-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="e5980-110">Önbellek bilgileri tek tek web sunucuları bellekte depolanan değil ve önbelleğe alınan veriler tüm uygulamanın sunucuları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5980-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="e5980-111">Bu, çeşitli avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="e5980-111">This provides several advantages:</span></span>

1. <span data-ttu-id="e5980-112">Önbelleğe alınan tüm web sunucularında tutarlı verilerdir.</span><span class="sxs-lookup"><span data-stu-id="e5980-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="e5980-113">Kullanıcıların hangi web bağlı olarak sunucu, isteği işler farklı sonuçlar görmüyorum</span><span class="sxs-lookup"><span data-stu-id="e5980-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="e5980-114">Önbelleğe alınan veriler web sunucu yeniden başlatıldıktan ve dağıtımları devam eder.</span><span class="sxs-lookup"><span data-stu-id="e5980-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="e5980-115">Tek tek web sunucuları, kaldırıldı veya önbellek etkilemeden eklendi.</span><span class="sxs-lookup"><span data-stu-id="e5980-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="e5980-116">Kaynak veri deposu kendisine (birden çok bellek içi önbellekte veya Hayır hiç önbellek daha) yapılan daha az istekleri sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e5980-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="e5980-117">Yalnızca bir SQL Server dağıtılmış önbellek kullanıyorsanız, bu avantajlar bazıları önbelleği uygulamanın kaynak verileri için ayrı veritabanı örneği kullanılıyorsa true.</span><span class="sxs-lookup"><span data-stu-id="e5980-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="e5980-118">Herhangi bir önbellek gibi dağıtılmış önbellek genellikle veri bir ilişkisel veritabanı (veya web hizmeti) çok daha hızlı önbellekten alınabilir bu yana bir uygulamanın yanıtlama hızı, önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="e5980-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="e5980-119">Önbellek yapılandırma belirli uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e5980-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="e5980-120">Bu makalede nasıl yapılandırılacağı her ikisi de Redis ve SQL Server dağıtılmış önbelleğe açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e5980-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="e5980-121">Hangi uygulama seçili bağımsız olarak, uygulama bir ortak kullanarak önbelleği ile etkileşim `IDistributedCache` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="e5980-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="e5980-122">IDistributedCache arabirimi</span><span class="sxs-lookup"><span data-stu-id="e5980-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="e5980-123">`IDistributedCache` Arabirim, zaman uyumlu ve zaman uyumsuz yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e5980-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="e5980-124">Arabirimi eklenebilir, alınan ve dağıtılmış önbellek uygulamasından kaldırılabilir öğelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="e5980-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="e5980-125">`IDistributedCache` Arabirimi aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="e5980-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="e5980-126">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="e5980-126">**Get, GetAsync**</span></span>

<span data-ttu-id="e5980-127">Bir dize anahtarı alır ve önbelleğe alınan bir öğe olarak alır bir `byte[]` , önbellekte bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="e5980-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="e5980-128">**Ayarlama, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="e5980-128">**Set, SetAsync**</span></span>

<span data-ttu-id="e5980-129">Bir öğe ekler (olarak `byte[]`) bir dize anahtarı kullanarak önbelleği için.</span><span class="sxs-lookup"><span data-stu-id="e5980-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="e5980-130">**Yenileme, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="e5980-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="e5980-131">Bir öğe (varsa), kayan zaman aşımı süresi sıfırlama kendi anahtarı, temel önbelleğinde yeniler.</span><span class="sxs-lookup"><span data-stu-id="e5980-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="e5980-132">**Kaldır, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="e5980-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="e5980-133">Alt anahtarına göre bir önbellek girişi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="e5980-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="e5980-134">Kullanılacak `IDistributedCache` arabirimi:</span><span class="sxs-lookup"><span data-stu-id="e5980-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="e5980-135">Gerekli NuGet paketlerini proje dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5980-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="e5980-136">Belirli uygulanması yapılandırma `IDistributedCache` içinde `Startup` sınıfının `ConfigureServices` yöntemi ve orada kapsayıcıya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5980-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="e5980-137">Uygulamanın gelen [ara yazılımı](xref:fundamentals/middleware/index) veya MVC denetleyicisi sınıfları, istek örneği `IDistributedCache` oluşturucusundan.</span><span class="sxs-lookup"><span data-stu-id="e5980-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="e5980-138">Örneği tarafından sağlanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı).</span><span class="sxs-lookup"><span data-stu-id="e5980-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="e5980-139">Singleton veya kapsamındaki yaşam süresi kullanmaya gerek yoktur `IDistributedCache` örnekleri (en az yerleşik uygulamaları için).</span><span class="sxs-lookup"><span data-stu-id="e5980-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="e5980-140">Bir gereksinim duyabileceğiniz her yerde bir örneği de oluşturabilirsiniz (kullanmak yerine [bağımlılık ekleme](../../fundamentals/dependency-injection.md)), ancak bu kodunuzu test etmek daha zor hale getirebilir ve ihlal [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="e5980-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="e5980-141">Aşağıdaki örnek, bir örneğini kullanması gösterilmiştir `IDistributedCache` basit ara yazılım bileşeni içinde:</span><span class="sxs-lookup"><span data-stu-id="e5980-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="e5980-142">Yukarıdaki kod önbelleğe alınan değer okuma, ancak hiçbir zaman yazılır.</span><span class="sxs-lookup"><span data-stu-id="e5980-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="e5980-143">Bu örnekte, bir sunucu başlatıldığında ve değişmez değer yalnızca ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="e5980-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="e5980-144">Çok sunuculu bir senaryoda, diğer sunucular tarafından ayarlanan herhangi bir önceki değeri başlatmak için en son sunucu üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="e5980-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="e5980-145">`Get` Ve `Set` yöntemleri kullanın `byte[]` türü.</span><span class="sxs-lookup"><span data-stu-id="e5980-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="e5980-146">Bu nedenle, dize değeri kullanılarak dönüştürülmelidir `Encoding.UTF8.GetString` (için `Get`) ve `Encoding.UTF8.GetBytes` (için `Set`).</span><span class="sxs-lookup"><span data-stu-id="e5980-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="e5980-147">Aşağıdaki kod *haline* ayarlanan değer gösterir:</span><span class="sxs-lookup"><span data-stu-id="e5980-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="e5980-148">Bu yana `IDistributedCache` yapılandırılan `ConfigureServices` yöntemi, onu kullanılabilir `Configure` yöntemi bir parametre olarak.</span><span class="sxs-lookup"><span data-stu-id="e5980-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="e5980-149">Parametre olarak eklenmesi dı sağlanacak yapılandırılmış örneği olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e5980-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="e5980-150">Dağıtılmış Redis önbelleği kullanma</span><span class="sxs-lookup"><span data-stu-id="e5980-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="e5980-151">[Redis](https://redis.io/) dağıtılmış önbellek sık kullanılan bir açık kaynak bellek içi veri deposu.</span><span class="sxs-lookup"><span data-stu-id="e5980-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="e5980-152">Yerel olarak kullanın ve yapılandırabileceğiniz bir [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/) Azure barındırılan ASP.NET Core uygulamalarınız için.</span><span class="sxs-lookup"><span data-stu-id="e5980-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="e5980-153">Önbellek uygulamasını kullanarak ASP.NET Core uygulamanızı yapılandırır bir `RedisDistributedCache` örneği.</span><span class="sxs-lookup"><span data-stu-id="e5980-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="e5980-154">Redis uygulamasında yapılandırma `ConfigureServices` ve örneği isteyerek uygulama kodunuzda erişim `IDistributedCache` (Yukarıdaki kod bakın).</span><span class="sxs-lookup"><span data-stu-id="e5980-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="e5980-155">Örnek kodda bir `RedisCache` uygulama sunucusu için yapılandırıldığında kullanılır bir `Staging` ortamı.</span><span class="sxs-lookup"><span data-stu-id="e5980-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="e5980-156">Bu nedenle `ConfigureStagingServices` yöntemi yapılandırır `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="e5980-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="e5980-157">Yerel makinenizde Redis yüklemek için chocolatey paketini Yükle [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) çalıştırıp `redis-server` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="e5980-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="e5980-158">SQL Server'ı kullanarak dağıtılmış önbellek</span><span class="sxs-lookup"><span data-stu-id="e5980-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="e5980-159">SqlServerCache uygulama, yedekleme deposu olarak bir SQL Server veritabanını kullanmak dağıtılmış önbellek sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5980-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="e5980-160">SQL sunucusu oluşturmak için belirttiğiniz adı ve şema ile sql önbellek aracı Aracı'nı kullanabilirsiniz Tablosu bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e5980-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="e5980-161">Ekleme `SqlConfig.Tools` için `<ItemGroup>` proje dosyası ve çalışma öğesi `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="e5980-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="e5980-162">Aşağıdaki komutu çalıştırarak SqlConfig.Tools test edin:</span><span class="sxs-lookup"><span data-stu-id="e5980-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="e5980-163">SqlConfig.Tools kullanımı, seçenekleri ve komut Yardımı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e5980-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="e5980-164">Çalıştırarak SQL Server'da bir tablo oluşturma `sql-cache create` komutu:</span><span class="sxs-lookup"><span data-stu-id="e5980-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="e5980-165">Oluşturulan tablonun aşağıdaki şema sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e5980-165">The created table has the following schema:</span></span>

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="e5980-167">Tüm önbellek uygulamaları gibi uygulamanız alma ve ayarlama örneği kullanarak önbellek değerleri `IDistributedCache`değil bir `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="e5980-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="e5980-168">Örnek uygulayan `SqlServerCache` üretim ortamında (olarak yapılandırıldığı şekilde `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="e5980-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="e5980-169">`ConnectionString` (Ve isteğe bağlı olarak `SchemaName` ve `TableName`) kimlik bilgileri içerebilir gibi tipik olarak (örneğin, UserSecrets), kaynak denetimi dışında depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5980-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="e5980-170">Önerileri</span><span class="sxs-lookup"><span data-stu-id="e5980-170">Recommendations</span></span>

<span data-ttu-id="e5980-171">Hangi uyarlamasını karar verirken `IDistributedCache` sağ uygulamanız için Redis arasında seçin ve SQL Server tabanlı, mevcut altyapınızı ve ortam, performans gereksinimlerinizi ve ekibinizin deneyimi.</span><span class="sxs-lookup"><span data-stu-id="e5980-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="e5980-172">Ekibinizin Redis ile çalışmayı daha rahat ise, mükemmel bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="e5980-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="e5980-173">SQL Server, takım tercih ediyorsa, bu uygulamada emin olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5980-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="e5980-174">Geleneksel bir önbelleğe alma çözümünü verileri hızlı veri alımını sağlayan bellek içi içerdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e5980-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="e5980-175">Yaygın olarak kullanılan verilerini bir önbellekte depolamak ve SQL Server ya da Azure depolama gibi bir arka uç kalıcı deposundaki tüm verileri depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5980-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="e5980-176">Redis önbelleği karşılaştırıldığında SQL önbellek yüksek verimlilik ve düşük gecikme süresi sunan bir önbelleğe alma çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="e5980-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5980-177">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e5980-177">Additional resources</span></span>

* [<span data-ttu-id="e5980-178">Azure önbelleği redis</span><span class="sxs-lookup"><span data-stu-id="e5980-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="e5980-179">Azure üzerinde SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="e5980-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="e5980-180">Belleğe yüklenmiş önbellek</span><span class="sxs-lookup"><span data-stu-id="e5980-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="e5980-181">Değişiklik belirteçleri değişikliklerle Algıla</span><span class="sxs-lookup"><span data-stu-id="e5980-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="e5980-182">Yanıtları önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="e5980-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="e5980-183">Yanıtları Önbelleğe Alma Ara Yazılımı</span><span class="sxs-lookup"><span data-stu-id="e5980-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="e5980-184">Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="e5980-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="e5980-185">Dağıtılmış Önbellek Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="e5980-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
