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
# <a name="distributed-caching-in-aspnet-core"></a>ASP.NET Core 'de dağıtılmış önbelleğe alma

[Luke Latham](https://github.com/guardrex), [mohsin Nasir](https://github.com/mohsinnasir)ve [Steve Smith](https://ardalis.com/) tarafından

Dağıtılmış önbellek, genellikle ona erişen uygulama sunucuları için bir dış hizmet olarak tutulan birden çok uygulama sunucusu tarafından paylaşılan bir önbellektir. Dağıtılmış önbellek, özellikle uygulama bir bulut hizmeti veya sunucu grubu tarafından barındırıldığı zaman bir ASP.NET Core uygulamasının performansını ve ölçeklenebilirliğini iyileştirebilir.

Dağıtılmış bir önbellek, önbelleğe alınan verilerin ayrı uygulama sunucularında depolandığı diğer önbelleğe alma senaryolarından daha fazla avantaj sunar.

Önbelleğe alınan veriler dağıtıldığında, veriler:

* Birden çok sunucuya yönelik istekler arasında *tutarlı (tutarlı* ).
* Sunucu yeniden başlatmaları ve uygulama dağıtımları.
* Yerel bellek kullanmaz.

Dağıtılmış önbellek yapılandırması uygulamaya özgüdür. Bu makalede SQL Server ve Redsıs dağıtılmış önbelleklerinin nasıl yapılandırılacağı açıklanır. [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([GitHub üzerindeki nCache](https://github.com/Alachisoft/NCache)) gibi üçüncü taraf uygulamalar da mevcuttur. Uygulama hangi uygulamanın seçildiğine bakılmaksızın, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimini kullanarak önbellek ile etkileşime girer.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

SQL Server dağıtılmış önbellek kullanmak için [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.

Redsıs dağıtılmış önbelleği kullanmak için [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paketine bir paket başvurusu ekleyin.

NCache dağıtılmış önbelleğini kullanmak için [nCache. Microsoft. Extensions. Caching. OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

SQL Server dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.

Redsıs dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [Microsoft. Extensions. Caching. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paketine bir paket başvurusu ekleyin. Redsıs paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle Redu paketine proje dosyanızda ayrı olarak başvurmanız gerekir.

NCache dağıtılmış önbelleğini kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [nCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin. NCache paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle, proje dosyanızda ayrı olarak NCache paketine başvurmanız gerekir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

SQL Server dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paketine bir paket başvurusu ekleyin.

Redsıs dağıtılmış önbellek kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [Microsoft. Extensions. Caching. redsıs](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paketine bir paket başvurusu ekleyin. Redsıs paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle Redu paketine proje dosyanızda ayrı olarak başvurmanız gerekir.

NCache dağıtılmış önbelleğini kullanmak için [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun ve [nCache. Microsoft. Extensions. Caching. opensource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) paketine bir paket başvurusu ekleyin. NCache paketi `Microsoft.AspNetCore.App` paketine dahil değildir, bu nedenle, proje dosyanızda ayrı olarak NCache paketine başvurmanız gerekir.

::: moniker-end

## <a name="idistributedcache-interface"></a>Idistributedcache arabirimi

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimi, dağıtılmış önbellek uygulamasındaki öğeleri işlemek için aşağıdaki yöntemleri sağlar:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; bir dize anahtarını kabul eder ve önbellekte bulunursa önbelleğe alınmış bir öğeyi bir `byte[]` dizisi olarak alır.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; bir dize anahtarı kullanarak önbelleğe bir öğe (`byte[]` dizisi olarak) ekler.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;, kendi anahtarını temel alarak önbellekteki bir öğeyi yeniler, Kayan süre sonu zaman aşımını (varsa) sıfırlarken.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; bir önbellek öğesini dize anahtarına göre kaldırır.

## <a name="establish-distributed-caching-services"></a>Dağıtılmış önbelleğe alma hizmetleri oluşturma

`Startup.ConfigureServices`bir <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulanmasını kaydedin. Bu konuda açıklanan Framework tarafından sunulan uygulamalar şunlardır:

* [Dağıtılmış bellek önbelleği](#distributed-memory-cache)
* [Dağıtılmış SQL Server önbelleği](#distributed-sql-server-cache)
* [Dağıtılmış Redsıs önbelleği](#distributed-redis-cache)
* [Dağıtılmış NCache önbelleği](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a>Dağıtılmış bellek önbelleği

Dağıtılmış bellek önbelleği (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>), öğeleri bellekte depolayan <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> çerçeve tarafından sağlanmış bir uygulamasıdır. Dağıtılmış bellek önbelleği gerçek bir dağıtılmış önbellek değildir. Önbelleğe alınan öğeler, uygulamanın çalıştığı sunucuda uygulama örneği tarafından depolanır.

Dağıtılmış bellek önbelleği, yararlı bir uygulama:

* Geliştirme ve test senaryolarında.
* Üretimde tek bir sunucu kullanıldığında ve bellek tüketimi bir sorun değildir. Dağıtılmış bellek önbelleğinin uygulanması, önbelleğe alınmış veri depolamayı soyutlar. Birden çok düğüm veya hata toleransı gerekliyse, gelecekte doğru bir dağıtılmış önbelleğe alma çözümü uygulamaya olanak sağlar.

Örnek uygulama, uygulama `Startup.ConfigureServices`' deki geliştirme ortamında çalıştırıldığında dağıtılmış bellek önbelleğinin kullanımını sağlar:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a>Dağıtılmış SQL Server önbelleği

Dağıtılmış SQL Server önbellek uygulama (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) dağıtılmış önbelleğin, kendi yedekleme deposu olarak bir SQL Server veritabanı kullanmasına izin verir. SQL Server örneğinde SQL Server önbelleğe alınmış bir öğe tablosu oluşturmak için `sql-cache` aracını kullanabilirsiniz. Araç, belirttiğiniz ad ve şemaya sahip bir tablo oluşturur.

`sql-cache create` komutunu çalıştırarak SQL Server tablo oluşturun. SQL Server örneği (`Data Source`), veritabanı (`Initial Catalog`), şema (örneğin, `dbo`) ve tablo adı (örneğin, `TestCache`) sağlayın:

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Aracın başarılı olduğunu göstermek için bir ileti günlüğe kaydedilir:

```console
Table and index were created successfully.
```

`sql-cache` aracı tarafından oluşturulan tablo aşağıdaki şemaya sahiptir:

![SqlServer önbellek tablosu](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Bir uygulama, bir <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>değil <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>örneğini kullanarak önbellek değerlerini işlemelidir.

Örnek uygulama, `Startup.ConfigureServices`bir geliştirme dışı ortamda <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> uygular:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (ve isteğe bağlı olarak, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> ve <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) genellikle kaynak denetimi dışında (örneğin, [gizli yönetici](xref:security/app-secrets) veya *appsettings. JSON*/appSettings içinde depolanır *. { ENVIRONMENT}. JSON* dosyaları). Bağlantı dizesinde kaynak denetim sistemlerinden tutulması gereken kimlik bilgileri bulunabilir.

### <a name="distributed-redis-cache"></a>Dağıtılmış Redis Cache

[Redsıs](https://redis.io/) , genellikle dağıtılmış önbellek olarak kullanılan açık kaynaklı bir bellek içi veri deposudur. Redsıs 'yi yerel olarak kullanabilir ve Azure 'da barındırılan ASP.NET Core uygulaması için bir [Azure Redis Cache](https://azure.microsoft.com/services/cache/) yapılandırabilirsiniz.

::: moniker range=">= aspnetcore-3.0"

Bir uygulama, `Startup.ConfigureServices`geliştirme dışı bir ortamda bir <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Bir uygulama, `Startup.ConfigureServices`geliştirme dışı bir ortamda bir <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Bir uygulama, bir <xref:Microsoft.Extensions.Caching.Redis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>) kullanarak önbellek uygulamasını yapılandırır:

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

Redsıs 'i yerel makinenize yüklemek için:

1. [Chocolatey redsıs paketini](https://chocolatey.org/packages/redis-64/)yükler.
1. Komut isteminden `redis-server` çalıştırın.

### <a name="distributed-ncache-cache"></a>Dağıtılmış NCache önbelleği

[NCache](https://github.com/Alachisoft/NCache) , .NET ve .NET Core 'da yerel olarak geliştirilen açık kaynaklı bellek içi dağıtılmış bir önbellektir. NCache hem yerel olarak hem de Azure 'da veya diğer barındırma platformlarında çalışan bir ASP.NET Core uygulama için dağıtılmış önbellek kümesi olarak yapılandırılmış şekilde çalışır.

Yerel makinenizde NCache 'i yüklemek ve yapılandırmak için bkz. [Windows Için nCache Başlangıç Kılavuzu](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).

NCache 'yi yapılandırmak için:

1. [NCache açık kaynak NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)'i yükler.
1. Cache kümesini [Client. ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)' de yapılandırın.
1. Aşağıdaki kodu `Startup.ConfigureServices`ekleyin:

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a>Dağıtılmış önbelleği kullan

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimini kullanmak için, uygulamadaki herhangi bir oluşturucudan <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> bir örnek isteyin. Örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)tarafından sağlanır.

::: moniker range=">= aspnetcore-3.0"

Örnek uygulama başlatıldığında <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `Startup.Configure`eklenir. Geçerli zaman <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> kullanılarak önbelleğe alındı (daha fazla bilgi için bkz. [genel konak: ıhostapplicationlifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Örnek uygulama başlatıldığında <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `Startup.Configure`eklenir. Geçerli zaman <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> kullanılarak önbelleğe alındı (daha fazla bilgi için bkz [. Web Host: ıapplicationlifetime arabirimi](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

Örnek uygulama, Dizin sayfası tarafından kullanılmak üzere `IndexModel` <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> çıkartır.

Dizin sayfası her yüklendiğinde, önbellek `OnGetAsync`önbelleğe alınmış zamana göre denetlenir. Önbelleğe alınmış sürenin süresi dolmamışsa, zaman görüntülenir. Önbelleğe alınan saate son erişildiği zamandan bu yana 20 saniye geçtikten sonra (bu sayfanın son yüklenilişinde), sayfanın *önbelleğe alınma süresi doldu*.

Önbelleğe alınmış süreyi **Sıfırla** ' yı seçerek önbelleğe alınmış zamanı geçerli saate hemen güncelleştirin. Düğme `OnPostResetCachedTime` işleyicisi metodunu tetikler.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> örnekleri için tek veya kapsamlı bir yaşam süresi kullanmanız gerekmez (en azından yerleşik uygulamalar için).
>
> Ayrıca, DI kullanmak yerine bir <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> örneği oluşturabilirsiniz, ancak kodda örnek oluşturmak kodunuzu test etmek ve [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)ihlal etmek için daha zor hale gelebilir.

## <a name="recommendations"></a>Öneriler

Uygulamanız için hangi <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulamasının en iyisi olduğuna karar verirken aşağıdakileri göz önünde bulundurun:

* Mevcut altyapı
* Performans gereksinimleri
* Maliyet
* Ekip deneyimi

Önbelleğe alma çözümleri genellikle önbelleğe alınmış verilerin hızlı bir şekilde alınmasını sağlamak için bellek içi depolamaya dayanır, ancak bellek sınırlı bir kaynaktır ve genişleyebilir. Yalnızca yaygın olarak kullanılan verileri bir önbellekte depolayın.

Genellikle, Redsıs önbelleği daha yüksek aktarım hızı ve bir SQL Server önbelleğinden daha düşük gecikme süresi sağlar. Bununla birlikte, genellikle önbelleğe alma stratejilerinin performans özelliklerini belirlemede bir değerlendirme gerekir.

SQL Server dağıtılmış önbellek yedekleme deposu olarak kullanıldığında, önbellek için aynı veritabanını kullanın ve uygulamanın sıradan veri depolama ve alma, her ikisinin performansını olumsuz yönde etkileyebilir. Dağıtılmış önbellek yedekleme deposu için adanmış bir SQL Server örneği kullanmanızı öneririz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure üzerinde Redis Cache](/azure/azure-cache-for-redis/)
* [Azure 'da SQL veritabanı](/azure/sql-database/)
* [Web çiftlerine nCache Için ıdistributedcache sağlayıcısını ASP.NET Core](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([GitHub 'da nCache](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
