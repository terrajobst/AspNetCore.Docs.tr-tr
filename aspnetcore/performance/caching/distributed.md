---
title: Dağıtılmış önbelleğe alma ASP.NET Core
author: guardrex
description: Uygulama performansı ve ölçeklenebilirlik, özellikle bir Bulutu vea sunucusu grubu ortamında artırmak için bir ASP.NET Core dağıtılmış önbellek kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: performance/caching/distributed
ms.openlocfilehash: 7337ee3b823064c942832d8a44e4d4289bc4fd0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899430"
---
# <a name="distributed-caching-in-aspnet-core"></a>Dağıtılmış önbelleğe alma ASP.NET Core

Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)

Dağıtılmış bir önbellek genellikle bir dış hizmete erişebilmesi uygulama sunucularına olarak tutulur, birden çok uygulama sunucular tarafından paylaşılan bir önbellektir. Özellikle uygulama bir bulut hizmeti ya da bir sunucu grubu tarafından barındırıldığında dağıtılmış önbellek ASP.NET Core uygulaması, ölçeklenebilirliğini ve performansı artırabilir.

Dağıtılmış önbellek tek tek uygulama sunucularında önbelleğe alınan verilerin depolandığı önbelleğe alma diğer senaryolar üzerinden çeşitli avantajları vardır.

Önbelleğe alınan verilerin ne zaman dağıtılır, veri:

* Olan *tutarlı* istekleri birden çok sunucu arasında (tutarlı).
* Sunucu yeniden başlatıldıktan ve uygulama dağıtımlarını devam eder.
* Yerel bellek kullanmaz.

Dağıtılmış önbellek, uygulama belirli yapılandırmadır. Bu makalede, SQL Server yapılandırma ve dağıtılmış önbellek Redis açıklar. Üçüncü taraf uygulamalarında kullanılabilir olduğu gibi aynı zamanda [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github'da NCache](https://github.com/Alachisoft/NCache)). Hangi uygulama bağımsız olarak işaretlenirse, uygulama önbellek kullanarak ile etkileşim <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirimi.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

::: moniker range=">= aspnetcore-2.2"

Bir SQL Server kullanmak için Dağıtılmış önbellek, başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paket.

Dağıtılmış önbellek, başvuru bir Redis kullanılacak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve için bir paket başvurusu ekleme [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) paket. Redis paket bulunup bulunmadığına `Microsoft.AspNetCore.App` paketlemek için Redis paket proje dosyanızda ayrı olarak başvurmalıdır.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Bir SQL Server kullanmak için Dağıtılmış önbellek, başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paket.

Dağıtılmış önbellek, başvuru bir Redis kullanılacak [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve için bir paket başvurusu ekleme [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paket. Redis paket bulunup bulunmadığına `Microsoft.AspNetCore.App` paketlemek için Redis paket proje dosyanızda ayrı olarak başvurmalıdır.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Bir SQL Server kullanmak için Dağıtılmış önbellek, başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paket.

Dağıtılmış önbellek, başvuru bir Redis kullanılacak [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paket. Redis paketinde `Microsoft.AspNetCore.All` paketini ayrı olarak proje dosyanızda Redis paket başvurusu yapmak zorunda kalmazsınız.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Dağıtılmış önbellek bir SQL Server kullanmak için paket başvurusu ekleme [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) paket.

Dağıtılmış önbellek bir Redis kullanmak için paket başvurusu ekleme [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) paket.

::: moniker-end

## <a name="idistributedcache-interface"></a>IDistributedCache arabirimi

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Arabirimi dağıtılmış önbellek uygulamasında öğelerini işlemek için aşağıdaki yöntemleri sağlar:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Dize anahtarı kabul eder ve önbelleğe alınan öğe olarak alır. bir `byte[]` , dizi önbellekte bulundu.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Bir öğe ekler (olarak `byte[]` array) için bir dize anahtarı kullanarak önbelleğe.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Önbelleğinde (varsa), kayan zaman aşımı sıfırlama kendi anahtarını temel alan bir öğeyi yeniler.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Önbellek öğesinin temel dize anahtarıyla kaldırır.

## <a name="establish-distributed-caching-services"></a>Dağıtılmış önbelleğe alma hizmetleri oluşturma

Uygulaması kaydetme <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> içinde `Startup.ConfigureServices`. Bu konuda açıklanan framework tarafından sağlanan uygulamaları şunlardır:

* [Dağıtılmış önbellek](#distributed-memory-cache)
* [SQL Server dağıtılmış önbellek](#distributed-sql-server-cache)
* [Dağıtılmış bir Redis önbelleği](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Dağıtılmış önbellek

Dağıtılmış önbellek (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) bir framework tarafından sağlanan uygulamasıdır <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> , öğeleri bellekte depolar. Dağıtılmış önbellek gerçek bir dağıtılmış önbellek değildir. Önbelleğe alınmış öğeleri, uygulamanın çalıştığı sunucu üzerinde uygulama örneği tarafından depolanır.

Dağıtılmış önbellek faydalı bir uygulamadır:

* Geliştirme ve test senaryoları.
* Tek bir sunucu üretim ve bellek tüketimi kullanıldığında, bir sorun değildir. Dağıtılmış önbellek özetleri uygulama, veri depolama önbelleğe alınır. Birden çok düğüm veya hataya dayanıklılık gerekli hale ileride önbelleğe alma çözüme gerçek bir dağıtılmış uygulama için sağlar.

Örnek uygulama, uygulama geliştirme ortamında çalıştırıldığında dağıtılmış bellek önbelleğini kullanma yapar:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>Dağıtılmış SQL Sunucusu Önbelleği

Dağıtılmış SQL Server önbellek uygulaması (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>), yedekleme deposu bir SQL Server veritabanını kullanmak dağıtılmış bir önbellek sağlar. Bir SQL Server örneğinde SQL Server önbelleğe alınan öğe tablo oluşturmak için kullanabileceğiniz `sql-cache` aracı. Aracı adı ve belirttiğiniz şema ile bir tablo oluşturur.

::: moniker range="< aspnetcore-2.1"

Ekleme `SqlConfig.Tools` için `<ItemGroup>` proje dosyası ve çalışma öğesi `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Çalıştırarak SQL Server'da bir tablo oluşturma `sql-cache create` komutu. SQL Server örneği sağlayın (`Data Source`), veritabanı (`Initial Catalog`), şema (örneğin, `dbo`) ve tablo adı (örneğin, `TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Aracı başarılı olduğunu belirten bir ileti kaydedilir:

```console
Table and index were created successfully.
```

Tarafından oluşturulan tabloyu `sql-cache` aracı aşağıdaki şemayı sahiptir:

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Bir uygulamanın bir örneğini kullanarak önbellek değerleri değiştirmelisiniz <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>değil bir <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

Örnek uygulama uygular <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> olmayan geliştirme ortamında:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (ve isteğe bağlı olarak <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> ve <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) dışında kaynak denetimine genellikle depolanır (örneğin, tarafından depolanan [gizli dizi Yöneticisi](xref:security/app-secrets) veya *appsettings.json* / *appsettings. {Ortamı} .json* dosyaları). Bağlantı dizesi, kaynak denetimi sistemlerini dışında tutulması gereken kimlik bilgileri içeriyor olabilir.

### <a name="distributed-redis-cache"></a>Dağıtılmış bir Redis önbelleği

[Redis](https://redis.io/) dağıtılmış bir önbellek sık kullanılan bir açık kaynak bellek içi veri deposu. Yerel olarak Redis kullanın ve yapılandırabileceğiniz bir [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure'da barındırılan ASP.NET Core uygulaması için. Önbellek uygulamasını kullanarak bir uygulamayı yapılandırır bir <xref:Microsoft.Extensions.Caching.Redis.RedisCache> örneği (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

Yerel makinenizde Redis yüklemek için:

* Yükleme [Chocolatey Redis paket](https://chocolatey.org/packages/redis-64/).
* Çalıştırma `redis-server` bir komut isteminden.

## <a name="use-the-distributed-cache"></a>Dağıtılmış önbellek kullanma

Kullanılacak <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> arabirim, örneği istek <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulamadaki herhangi bir oluşturucudan. Örneği tarafından sağlanan [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).

Uygulama başlatıldığında <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> içine eklenen `Startup.Configure`. Geçerli zamanı kullanarak önbelleğe alınmış <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (daha fazla bilgi için [Web ana bilgisayarı: IApplicationLifetime arabirimi](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

Örnek uygulamayı eklediği <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> içine `IndexModel` dizin sayfası tarafından kullanılacak.

Dizin Sayfası yüklendiği her durumda, önbelleğe alınan kez önbellek denetlenir `OnGetAsync`. Önbelleğe alınan süresi dolmadığından zaman görüntülenir. Önbelleğe alınan saati (Bu sayfayı yüklenen son saat) erişildi son daraltılmasından 20 saniye geçtikten sayfası görüntüler *önbelleğe alınmış süre doldu*.

Önbelleğe alınan zaman seçerek geçerli saate hemen güncelleştirmek **sıfırlama önbelleğe alınmış süresi** düğmesi. Düğme tetikleyicisi `OnPostResetCachedTime` işleyicisi yöntemi.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> Bir Tekliyi veya kapsamındaki ömrü boyunca kullanılacak gerek yoktur <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> örnekleri (en az yerleşik uygulamalar için).
>
> Ayrıca oluşturabilirsiniz bir <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> DI kullanmak yerine gerekebilir, ancak kodda bir örnek oluşturma yapabilir, kodunuzu test etmek daha zor bağlanmayacaktır örnek ve ihlal [açık bağımlılıkları ilkesine](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Öneriler

Hangi uygulamasının verirken <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> uygulamanız için en iyisidir aşağıdakileri dikkate alın:

* Var olan altyapı
* Performans gereksinimleri
* Maliyet
* Takım deneyimi

Önbelleğe alma çözümleri genellikle önbelleğe alınmış verileri hızlı alınmasını sağlamak için bellek içi depolama alanı kullanır, ancak bellektir sınırlı bir kaynağa ve masraflı genişletin. Bir ön bellekte yaygın olarak kullanılan veri yalnızca depolama.

Genellikle, Redis önbelleği, yüksek aktarım hızı ve SQL Server önbelleğe göre daha düşük gecikme süresi sağlar. Ancak, değerlendirmesi önbelleğe alma stratejilerine performans özelliklerini belirlemek için genellikle gerekli değildir.

SQL Server dağıtılmış önbellek yedekleme deposu kullanıldığında, önbellek ve uygulamanın normal veri depolama için aynı veritabanını kullanın ve alma, her ikisi de performansı olumsuz yönde etkileyebilir. Dağıtılmış önbellek yedekleme deposu için adanmış bir SQL Server örneği kullanmanızı öneririz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Redis Cache azure'da](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure'da SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/)
* [ASP.NET Core Web gruplarındaki NCache IDistributedCache sağlayıcısı](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([github'da NCache](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
