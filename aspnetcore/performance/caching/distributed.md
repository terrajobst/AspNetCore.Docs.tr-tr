---
title: "ASP.NET Core dağıtılmış önbelleğinde ile çalışma"
author: ardalis
description: "Özellikle bir bulut veya sunucu grubu ortamında barındırıldığında performans ve ASP.NET Core uygulamaları ölçeklenebilirliği artırmak için Dağıtılmış önbelleğe alma kullanmayı öğrenin."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: a00937e8c47e73fa8e29af883f44f6e1f4d4b1b4
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="7f3b6-103">ASP.NET Core dağıtılmış önbelleğinde ile çalışma</span><span class="sxs-lookup"><span data-stu-id="7f3b6-103">Working with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="7f3b6-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7f3b6-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7f3b6-105">Dağıtılmış önbellek özellikle bir bulut veya sunucu grubu ortamında barındırıldığında ASP.NET Core uygulamaları ölçeklenebilirliğini ve performansı artırabilir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="7f3b6-106">Bu makalede, ASP.NET Core'nın yerleşik dağıtılmış önbellek soyutlamalar ve uygulamaları ile nasıl çalışılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="7f3b6-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f3b6-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="7f3b6-108">Dağıtılmış önbellek nedir</span><span class="sxs-lookup"><span data-stu-id="7f3b6-108">What is a distributed cache</span></span>

<span data-ttu-id="7f3b6-109">Dağıtılmış önbellek birden çok uygulama sunucuları tarafından paylaşılan (bkz [önbelleğe alma Temelleri](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="7f3b6-110">Önbellek bilgileri tek tek web sunucuları bellekte depolanmaz ve önbelleğe alınan veriler tüm uygulamanın sunucuları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="7f3b6-111">Bu, çeşitli avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-111">This provides several advantages:</span></span>

1. <span data-ttu-id="7f3b6-112">Önbelleğe alınan tüm web sunucularında tutarlı verilerdir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="7f3b6-113">Kullanıcıların hangi web bağlı olarak sunucu, isteği işler farklı sonuçlar görmüyorum</span><span class="sxs-lookup"><span data-stu-id="7f3b6-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="7f3b6-114">Önbelleğe alınan veriler web sunucu yeniden başlatıldıktan ve dağıtımları devam eder.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="7f3b6-115">Tek tek web sunucuları, kaldırıldı veya önbellek etkilemeden eklendi.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="7f3b6-116">Kaynak veri deposu kendisine (birden çok bellek içi önbellekte veya Hayır hiç önbellek daha) yapılan daha az istekleri sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="7f3b6-117">Yalnızca bir SQL Server dağıtılmış önbellek kullanıyorsanız, bu avantajlar bazıları önbelleği uygulamanın kaynak verileri için ayrı veritabanı örneği kullanılıyorsa true.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="7f3b6-118">Herhangi bir önbellek gibi dağıtılmış önbellek genellikle veri bir ilişkisel veritabanı (veya web hizmeti) çok daha hızlı önbellekten alınabilir bu yana bir uygulamanın yanıtlama hızı, önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="7f3b6-119">Önbellek yapılandırma belirli uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="7f3b6-120">Bu makalede nasıl yapılandırılacağı her ikisi de Redis ve SQL Server dağıtılmış önbelleğe açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="7f3b6-121">Hangi uygulama seçili bağımsız olarak, uygulama bir ortak kullanarak önbelleği ile etkileşim `IDistributedCache` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="7f3b6-122">IDistributedCache arabirimi</span><span class="sxs-lookup"><span data-stu-id="7f3b6-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="7f3b6-123">`IDistributedCache` Arabirim, zaman uyumlu ve zaman uyumsuz yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="7f3b6-124">Arabirimi eklenebilir, alınan ve dağıtılmış önbellek uygulamasından kaldırılabilir öğelerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="7f3b6-125">`IDistributedCache` Arabirimi aşağıdaki yöntemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="7f3b6-126">**GET, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="7f3b6-126">**Get, GetAsync**</span></span>

<span data-ttu-id="7f3b6-127">Bir dize anahtarı alır ve önbelleğe alınan bir öğe olarak alır bir `byte[]` , önbellekte bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="7f3b6-128">**Ayarlama, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="7f3b6-128">**Set, SetAsync**</span></span>

<span data-ttu-id="7f3b6-129">Bir öğe ekler (olarak `byte[]`) bir dize anahtarı kullanarak önbelleği için.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="7f3b6-130">**Yenileme, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="7f3b6-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="7f3b6-131">Bir öğe (varsa), kayan zaman aşımı süresi sıfırlama kendi anahtarı, temel önbelleğinde yeniler.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="7f3b6-132">**Kaldır, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="7f3b6-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="7f3b6-133">Alt anahtarına göre bir önbellek girişi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="7f3b6-134">Kullanılacak `IDistributedCache` arabirimi:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="7f3b6-135">Gerekli NuGet paketlerini proje dosyanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="7f3b6-136">Belirli uygulanması yapılandırma `IDistributedCache` içinde `Startup` sınıfının `ConfigureServices` yöntemi ve orada kapsayıcıya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="7f3b6-137">Uygulamanın gelen [ara yazılımı](../../fundamentals/middleware.md) veya MVC denetleyicisi sınıfları, istek örneği `IDistributedCache` oluşturucusundan.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-137">From the app's [Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="7f3b6-138">Örneği tarafından sağlanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="7f3b6-139">Singleton veya kapsamındaki yaşam süresi kullanmaya gerek yoktur `IDistributedCache` örnekleri (en az yerleşik uygulamaları için).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="7f3b6-140">Bir gereksinim duyabileceğiniz her yerde bir örneği de oluşturabilirsiniz (kullanmak yerine [bağımlılık ekleme](../../fundamentals/dependency-injection.md)), ancak bu kodunuzu test etmek daha zor hale getirebilir ve ihlal [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="7f3b6-141">Aşağıdaki örnek, bir örneğini kullanması gösterilmiştir `IDistributedCache` basit ara yazılım bileşeni içinde:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

<span data-ttu-id="7f3b6-142">Yukarıdaki kod önbelleğe alınan değer okuma, ancak hiçbir zaman yazılır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="7f3b6-143">Bu örnekte, bir sunucu başlatıldığında ve değişmez değer yalnızca ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="7f3b6-144">Çok sunuculu bir senaryoda, diğer sunucular tarafından ayarlanan herhangi bir önceki değeri başlatmak için en son sunucu üzerine yazar.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="7f3b6-145">`Get` Ve `Set` yöntemleri kullanın `byte[]` türü.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="7f3b6-146">Bu nedenle, dize değeri kullanılarak dönüştürülmelidir `Encoding.UTF8.GetString` (için `Get`) ve `Encoding.UTF8.GetBytes` (için `Set`).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="7f3b6-147">Aşağıdaki kod *haline* ayarlanan değer gösterir:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> <span data-ttu-id="7f3b6-148">Bu yana `IDistributedCache` yapılandırılan `ConfigureServices` yöntemi, onu kullanılabilir `Configure` yöntemi bir parametre olarak.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="7f3b6-149">Parametre olarak eklenmesi dı sağlanacak yapılandırılmış örneği olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="7f3b6-150">Dağıtılmış Redis önbelleği kullanma</span><span class="sxs-lookup"><span data-stu-id="7f3b6-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="7f3b6-151">[Redis](https://redis.io/) dağıtılmış önbellek sık kullanılan bir açık kaynak bellek içi veri deposu.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="7f3b6-152">Yerel olarak kullanın ve yapılandırabileceğiniz bir [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/) Azure barındırılan ASP.NET Core uygulamalarınız için.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="7f3b6-153">Önbellek uygulamasını kullanarak ASP.NET Core uygulamanızı yapılandırır bir `RedisDistributedCache` örneği.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="7f3b6-154">Redis uygulamasında yapılandırma `ConfigureServices` ve örneği isteyerek uygulama kodunuzda erişim `IDistributedCache` (Yukarıdaki kod bakın).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="7f3b6-155">Örnek kodda bir `RedisCache` uygulama sunucusu için yapılandırıldığında kullanılır bir `Staging` ortamı.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="7f3b6-156">Bu nedenle `ConfigureStagingServices` yöntemi yapılandırır `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> <span data-ttu-id="7f3b6-157">Yerel makinenizde Redis yüklemek için chocolatey paketini Yükle [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) çalıştırıp `redis-server` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="7f3b6-158">SQL Server'ı kullanarak dağıtılmış önbellek</span><span class="sxs-lookup"><span data-stu-id="7f3b6-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="7f3b6-159">SqlServerCache uygulama, yedekleme deposu olarak bir SQL Server veritabanını kullanmak dağıtılmış önbellek sağlar.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="7f3b6-160">SQL sunucusu oluşturmak için belirttiğiniz adı ve şema ile sql önbellek aracı Aracı'nı kullanabilirsiniz Tablosu bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="7f3b6-161">Sql önbellek aracını kullanmak için add `SqlConfig.Tools` için `<ItemGroup>` öğesinin *.csproj* dosya ve dotnet geri yükleme çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-161">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

<span data-ttu-id="7f3b6-162">Aşağıdaki komutu çalıştırarak SqlConfig.Tools test etme</span><span class="sxs-lookup"><span data-stu-id="7f3b6-162">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="7f3b6-163">"sql önbellek oluşturma" komutunu çalıştırarak sql Server'a tablolar oluşturabilirsiniz artık sql önbelleği aracı kullanımı, seçenekleri ve komut Yardım görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-163">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="7f3b6-164">Oluşturulan tablonun aşağıdaki şema sahiptir:</span><span class="sxs-lookup"><span data-stu-id="7f3b6-164">The created table has the following schema:</span></span>

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="7f3b6-166">Tüm önbellek uygulamaları gibi uygulamanız alma ve ayarlama örneği kullanarak önbellek değerleri `IDistributedCache`değil bir `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="7f3b6-167">Örnek uygulayan `SqlServerCache` içinde `Production` ortam (olarak yapılandırıldığı şekilde `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="7f3b6-167">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> <span data-ttu-id="7f3b6-168">`ConnectionString` (Ve isteğe bağlı olarak `SchemaName` ve `TableName`) kimlik bilgileri içerebilir gibi tipik olarak (örneğin, UserSecrets), kaynak denetimi dışında depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="7f3b6-169">Önerileri</span><span class="sxs-lookup"><span data-stu-id="7f3b6-169">Recommendations</span></span>

<span data-ttu-id="7f3b6-170">Hangi uyarlamasını karar verirken `IDistributedCache` sağ uygulamanız için Redis arasında seçin ve SQL Server tabanlı, mevcut altyapınızı ve ortam, performans gereksinimlerinizi ve ekibinizin deneyimi.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="7f3b6-171">Ekibinizin Redis ile çalışmayı daha rahat ise, mükemmel bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="7f3b6-172">SQL Server, takım tercih ediyorsa, bu uygulamada emin olabilir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="7f3b6-173">Geleneksel bir önbelleğe alma çözümünü verileri hızlı veri alımını sağlayan bellek içi içerdiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-173">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="7f3b6-174">Yaygın olarak kullanılan verilerini bir önbellekte depolamak ve SQL Server ya da Azure depolama gibi bir arka uç kalıcı deposundaki tüm verileri depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="7f3b6-175">Redis önbelleği karşılaştırıldığında SQL önbellek yüksek verimlilik ve düşük gecikme süresi sunan bir önbelleğe alma çözümüdür.</span><span class="sxs-lookup"><span data-stu-id="7f3b6-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f3b6-176">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7f3b6-176">Additional resources</span></span>

* [<span data-ttu-id="7f3b6-177">Azure önbelleği redis</span><span class="sxs-lookup"><span data-stu-id="7f3b6-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="7f3b6-178">Azure üzerinde SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="7f3b6-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="7f3b6-179">Bellek içi önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="7f3b6-179">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="7f3b6-180">Değişiklik belirteçleri değişikliklerle Algıla</span><span class="sxs-lookup"><span data-stu-id="7f3b6-180">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="7f3b6-181">Yanıt önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="7f3b6-181">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="7f3b6-182">Yanıt önbelleğe alma Ara</span><span class="sxs-lookup"><span data-stu-id="7f3b6-182">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="7f3b6-183">Önbellek etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7f3b6-183">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="7f3b6-184">Dağıtılmış önbellek etiket Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7f3b6-184">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
