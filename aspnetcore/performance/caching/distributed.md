---
title: ASP.NET Core 'de dağıtılmış önbelleğe alma
author: guardrex
description: Özellikle bir bulutta veya sunucu grubu ortamında uygulama performansını ve ölçeklenebilirliğini artırmak için ASP.NET Core dağıtılmış bir önbelleğin nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727235"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="c076e-103">ASP.NET Core 'de dağıtılmış önbelleğe alma</span><span class="sxs-lookup"><span data-stu-id="c076e-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="c076e-104">[Luke Latham](https://github.com/guardrex), [mohsin Nasir](https://github.com/mohsinnasir)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="c076e-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c076e-105">Dağıtılmış önbellek, genellikle ona erişen uygulama sunucuları için bir dış hizmet olarak tutulan birden çok uygulama sunucusu tarafından paylaşılan bir önbellektir.</span><span class="sxs-lookup"><span data-stu-id="c076e-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="c076e-106">Dağıtılmış önbellek, özellikle uygulama bir bulut hizmeti veya sunucu grubu tarafından barındırıldığı zaman bir ASP.NET Core uygulamasının performansını ve ölçeklenebilirliğini iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="c076e-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="c076e-107">Dağıtılmış bir önbellek, önbelleğe alınan verilerin ayrı uygulama sunucularında depolandığı diğer önbelleğe alma senaryolarından daha fazla avantaj sunar.</span><span class="sxs-lookup"><span data-stu-id="c076e-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="c076e-108">Önbelleğe alınan veriler dağıtıldığında, veriler:</span><span class="sxs-lookup"><span data-stu-id="c076e-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="c076e-109">Birden çok sunucuya yönelik istekler arasında *tutarlı (tutarlı* ).</span><span class="sxs-lookup"><span data-stu-id="c076e-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="c076e-110">Sunucu yeniden başlatmaları ve uygulama dağıtımları.</span><span class="sxs-lookup"><span data-stu-id="c076e-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="c076e-111">Yerel bellek kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="c076e-111">Doesn't use local memory.</span></span>

<span data-ttu-id="c076e-112">Dağıtılmış önbellek yapılandırması uygulamaya özgüdür.</span><span class="sxs-lookup"><span data-stu-id="c076e-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="c076e-113">Bu makalede SQL Server ve Redsıs dağıtılmış önbelleklerinin nasıl yapılandırılacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="c076e-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="c076e-114">[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([GitHub üzerindeki nCache](https://github.com/Alachisoft/NCache)) gibi üçüncü taraf uygulamalar da mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="c076e-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="c076e-115">Uygulama hangi uygulamanın seçildiğine bakılmaksızın, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimini kullanarak önbellek ile etkileşime girer.</span><span class="sxs-lookup"><span data-stu-id="c076e-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="c076e-116">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c076e-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c076e-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c076e-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c076e-118">SQL Server dağıtılmış önbellek kullanmak için [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="c076e-119">Redsıs dağıtılmış önbelleği kullanmak için [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="c076e-120">NCache dağıtılmış önbelleğini kullanmak için [nCache. Microsoft. Extensions. Caching. OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="c076e-121">SQL Server dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="c076e-122">Redsıs dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="c076e-123">Redsıs paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle Redu paketine proje dosyanızda ayrı olarak başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c076e-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="c076e-124">NCache dağıtılmış önbelleğini kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [nCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="c076e-125">NCache paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle, proje dosyanızda ayrı olarak NCache paketine başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c076e-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c076e-126">SQL Server dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="c076e-127">Redsıs dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [Microsoft. Extensions. Caching. redsıs](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="c076e-128">Redsıs paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle Redu paketine proje dosyanızda ayrı olarak başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c076e-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="c076e-129">NCache dağıtılmış önbelleğini kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [nCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="c076e-130">NCache paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle, proje dosyanızda ayrı olarak NCache paketine başvurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c076e-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="c076e-131">Idistributedcache arabirimi</span><span class="sxs-lookup"><span data-stu-id="c076e-131">IDistributedCache interface</span></span>

<span data-ttu-id="c076e-132"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimi, dağıtılmış önbellek uygulamasındaki öğeleri işlemek için aşağıdaki yöntemleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="c076e-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="c076e-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; bir dize anahtarını kabul eder ve önbellekte bulunursa önbelleğe alınmış bir öğeyi bir `byte[]` dizisi olarak alır.</span><span class="sxs-lookup"><span data-stu-id="c076e-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="c076e-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; bir dize anahtarı kullanarak önbelleğe bir öğe (`byte[]` dizisi olarak) ekler.</span><span class="sxs-lookup"><span data-stu-id="c076e-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="c076e-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;, kendi anahtarını temel alarak önbellekteki bir öğeyi yeniler, Kayan süre sonu zaman aşımını (varsa) sıfırlarken.</span><span class="sxs-lookup"><span data-stu-id="c076e-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="c076e-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; bir önbellek öğesini dize anahtarına göre kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c076e-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="c076e-137">Dağıtılmış önbelleğe alma hizmetleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="c076e-137">Establish distributed caching services</span></span>

<span data-ttu-id="c076e-138">`Startup.ConfigureServices`bir <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulanmasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="c076e-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c076e-139">Bu konuda açıklanan Framework tarafından sunulan uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c076e-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="c076e-140">Dağıtılmış bellek önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="c076e-141">Dağıtılmış SQL Server önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="c076e-142">Dağıtılmış Redsıs önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="c076e-143">Dağıtılmış NCache önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="c076e-144">Dağıtılmış bellek önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-144">Distributed Memory Cache</span></span>

<span data-ttu-id="c076e-145">Dağıtılmış bellek önbelleği (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>), öğeleri bellekte depolayan <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> çerçeve tarafından sağlanmış bir uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c076e-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="c076e-146">Dağıtılmış bellek önbelleği gerçek bir dağıtılmış önbellek değildir.</span><span class="sxs-lookup"><span data-stu-id="c076e-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="c076e-147">Önbelleğe alınan öğeler, uygulamanın çalıştığı sunucuda uygulama örneği tarafından depolanır.</span><span class="sxs-lookup"><span data-stu-id="c076e-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="c076e-148">Dağıtılmış bellek önbelleği, yararlı bir uygulama:</span><span class="sxs-lookup"><span data-stu-id="c076e-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="c076e-149">Geliştirme ve test senaryolarında.</span><span class="sxs-lookup"><span data-stu-id="c076e-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="c076e-150">Üretimde tek bir sunucu kullanıldığında ve bellek tüketimi bir sorun değildir.</span><span class="sxs-lookup"><span data-stu-id="c076e-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="c076e-151">Dağıtılmış bellek önbelleğinin uygulanması, önbelleğe alınmış veri depolamayı soyutlar.</span><span class="sxs-lookup"><span data-stu-id="c076e-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="c076e-152">Birden çok düğüm veya hata toleransı gerekliyse, gelecekte doğru bir dağıtılmış önbelleğe alma çözümü uygulamaya olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="c076e-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="c076e-153">Örnek uygulama, uygulama `Startup.ConfigureServices`' deki geliştirme ortamında çalıştırıldığında dağıtılmış bellek önbelleğinin kullanımını sağlar:</span><span class="sxs-lookup"><span data-stu-id="c076e-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="c076e-154">Dağıtılmış SQL Server önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="c076e-155">Dağıtılmış SQL Server önbellek uygulama (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) dağıtılmış önbelleğin, kendi yedekleme deposu olarak bir SQL Server veritabanı kullanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="c076e-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="c076e-156">SQL Server örneğinde SQL Server önbelleğe alınmış bir öğe tablosu oluşturmak için `sql-cache` aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c076e-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="c076e-157">Araç, belirttiğiniz ad ve şemaya sahip bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c076e-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="c076e-158">`sql-cache create` komutunu çalıştırarak SQL Server tablo oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c076e-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="c076e-159">SQL Server örneği (`Data Source`), veritabanı (`Initial Catalog`), şema (örneğin, `dbo`) ve tablo adı (örneğin, `TestCache`) sağlayın:</span><span class="sxs-lookup"><span data-stu-id="c076e-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="c076e-160">Aracın başarılı olduğunu göstermek için bir ileti günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="c076e-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="c076e-161">`sql-cache` aracı tarafından oluşturulan tablo aşağıdaki şemaya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="c076e-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer önbellek tablosu](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="c076e-163">Bir uygulama, bir <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>değil <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>örneğini kullanarak önbellek değerlerini işlemelidir.</span><span class="sxs-lookup"><span data-stu-id="c076e-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="c076e-164">Örnek uygulama, `Startup.ConfigureServices`bir geliştirme dışı ortamda <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> uygular:</span><span class="sxs-lookup"><span data-stu-id="c076e-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="c076e-165"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (ve isteğe bağlı olarak, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> ve <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) genellikle kaynak denetimi dışında (örneğin, [gizli yönetici](xref:security/app-secrets) veya *appsettings. JSON*/appSettings içinde depolanır *. { ENVIRONMENT}. JSON* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="c076e-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="c076e-166">Bağlantı dizesinde kaynak denetim sistemlerinden tutulması gereken kimlik bilgileri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="c076e-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="c076e-167">Dağıtılmış Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c076e-167">Distributed Redis Cache</span></span>

<span data-ttu-id="c076e-168">[Redsıs](https://redis.io/) , genellikle dağıtılmış önbellek olarak kullanılan açık kaynaklı bir bellek içi veri deposudur.</span><span class="sxs-lookup"><span data-stu-id="c076e-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="c076e-169">Redsıs 'yi yerel olarak kullanabilir ve Azure 'da barındırılan ASP.NET Core uygulaması için bir [Azure Redis Cache](https://azure.microsoft.com/services/cache/) yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c076e-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c076e-170">Bir uygulama, `Startup.ConfigureServices`geliştirme dışı bir ortamda bir <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c076e-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="c076e-171">Bir uygulama, `Startup.ConfigureServices`geliştirme dışı bir ortamda bir <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c076e-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c076e-172">Bir uygulama, bir <xref:Microsoft.Extensions.Caching.Redis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="c076e-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="c076e-173">Redsıs 'i yerel makinenize yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="c076e-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="c076e-174">[Chocolatey redsıs paketini](https://chocolatey.org/packages/redis-64/)yükler.</span><span class="sxs-lookup"><span data-stu-id="c076e-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="c076e-175">Komut isteminden `redis-server` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c076e-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="c076e-176">Dağıtılmış NCache önbelleği</span><span class="sxs-lookup"><span data-stu-id="c076e-176">Distributed NCache Cache</span></span>

<span data-ttu-id="c076e-177">[NCache](https://github.com/Alachisoft/NCache) , .NET ve .NET Core 'da yerel olarak geliştirilen açık kaynaklı bellek içi dağıtılmış bir önbellektir.</span><span class="sxs-lookup"><span data-stu-id="c076e-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="c076e-178">NCache hem yerel olarak hem de Azure 'da veya diğer barındırma platformlarında çalışan bir ASP.NET Core uygulama için dağıtılmış önbellek kümesi olarak yapılandırılmış şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="c076e-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="c076e-179">Yerel makinenizde NCache 'i yüklemek ve yapılandırmak için bkz. [Windows Için nCache Başlangıç Kılavuzu](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="c076e-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="c076e-180">NCache 'yi yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="c076e-180">To configure NCache:</span></span>

1. <span data-ttu-id="c076e-181">[NCache açık kaynak NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)'i yükler.</span><span class="sxs-lookup"><span data-stu-id="c076e-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="c076e-182">Cache kümesini [Client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)' de yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="c076e-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="c076e-183">Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c076e-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="c076e-184">Dağıtılmış önbelleği kullan</span><span class="sxs-lookup"><span data-stu-id="c076e-184">Use the distributed cache</span></span>

<span data-ttu-id="c076e-185"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimini kullanmak için, uygulamadaki herhangi bir oluşturucudan <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> bir örnek isteyin.</span><span class="sxs-lookup"><span data-stu-id="c076e-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="c076e-186">Örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c076e-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c076e-187">Örnek uygulama başlatıldığında <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `Startup.Configure`eklenir.</span><span class="sxs-lookup"><span data-stu-id="c076e-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="c076e-188">Geçerli zaman <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> kullanılarak önbelleğe alındı (daha fazla bilgi için bkz. [genel konak: ıhostapplicationlifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="c076e-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c076e-189">Örnek uygulama başlatıldığında <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `Startup.Configure`eklenir.</span><span class="sxs-lookup"><span data-stu-id="c076e-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="c076e-190">Geçerli zaman <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> kullanılarak önbelleğe alındı (daha fazla bilgi için bkz [. Web Host: ıapplicationlifetime arabirimi](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="c076e-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="c076e-191">Örnek uygulama, Dizin sayfası tarafından kullanılmak üzere `IndexModel` <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> çıkartır.</span><span class="sxs-lookup"><span data-stu-id="c076e-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="c076e-192">Dizin sayfası her yüklendiğinde, önbellek `OnGetAsync`önbelleğe alınmış zamana göre denetlenir.</span><span class="sxs-lookup"><span data-stu-id="c076e-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="c076e-193">Önbelleğe alınmış sürenin süresi dolmamışsa, zaman görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="c076e-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="c076e-194">Önbelleğe alınan saate son erişildiği zamandan bu yana 20 saniye geçtikten sonra (bu sayfanın son yüklenilişinde), sayfanın *önbelleğe alınma süresi doldu*.</span><span class="sxs-lookup"><span data-stu-id="c076e-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="c076e-195">Önbelleğe alınmış süreyi **Sıfırla** ' yı seçerek önbelleğe alınmış zamanı geçerli saate hemen güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="c076e-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="c076e-196">Düğme `OnPostResetCachedTime` işleyicisi metodunu tetikler.</span><span class="sxs-lookup"><span data-stu-id="c076e-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="c076e-197"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> örnekleri için tek veya kapsamlı bir yaşam süresi kullanmanız gerekmez (en azından yerleşik uygulamalar için).</span><span class="sxs-lookup"><span data-stu-id="c076e-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="c076e-198">Ayrıca, DI kullanmak yerine bir <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> örneği oluşturabilirsiniz, ancak kodda örnek oluşturmak kodunuzu test etmek ve [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)ihlal etmek için daha zor hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="c076e-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="c076e-199">Öneriler</span><span class="sxs-lookup"><span data-stu-id="c076e-199">Recommendations</span></span>

<span data-ttu-id="c076e-200">Uygulamanız için hangi <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulamasının en iyisi olduğuna karar verirken aşağıdakileri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="c076e-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="c076e-201">Mevcut altyapı</span><span class="sxs-lookup"><span data-stu-id="c076e-201">Existing infrastructure</span></span>
* <span data-ttu-id="c076e-202">Performans gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="c076e-202">Performance requirements</span></span>
* <span data-ttu-id="c076e-203">Maliyet</span><span class="sxs-lookup"><span data-stu-id="c076e-203">Cost</span></span>
* <span data-ttu-id="c076e-204">Ekip deneyimi</span><span class="sxs-lookup"><span data-stu-id="c076e-204">Team experience</span></span>

<span data-ttu-id="c076e-205">Önbelleğe alma çözümleri genellikle önbelleğe alınmış verilerin hızlı bir şekilde alınmasını sağlamak için bellek içi depolamaya dayanır, ancak bellek sınırlı bir kaynaktır ve genişleyebilir.</span><span class="sxs-lookup"><span data-stu-id="c076e-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="c076e-206">Yalnızca yaygın olarak kullanılan verileri bir önbellekte depolayın.</span><span class="sxs-lookup"><span data-stu-id="c076e-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="c076e-207">Genellikle, Redsıs önbelleği daha yüksek aktarım hızı ve bir SQL Server önbelleğinden daha düşük gecikme süresi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c076e-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="c076e-208">Bununla birlikte, genellikle önbelleğe alma stratejilerinin performans özelliklerini belirlemede bir değerlendirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="c076e-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="c076e-209">SQL Server dağıtılmış önbellek yedekleme deposu olarak kullanıldığında, önbellek için aynı veritabanını kullanın ve uygulamanın sıradan veri depolama ve alma, her ikisinin performansını olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="c076e-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="c076e-210">Dağıtılmış önbellek yedekleme deposu için adanmış bir SQL Server örneği kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="c076e-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c076e-211">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c076e-211">Additional resources</span></span>

* [<span data-ttu-id="c076e-212">Azure üzerinde Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c076e-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="c076e-213">Azure 'da SQL veritabanı</span><span class="sxs-lookup"><span data-stu-id="c076e-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="c076e-214">[Web çiftlerine nCache Için ıdistributedcache sağlayıcısını ASP.NET Core](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([GitHub 'da nCache](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="c076e-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
