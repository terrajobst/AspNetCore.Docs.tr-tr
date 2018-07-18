---
title: Dağıtılmış bir önbellekte ASP.NET Core ile çalışma
author: ardalis
description: Dağıtılmış ASP.NET Core uygulaması performans ve ölçeklenebilirlik, özellikle bir Bulutu vea sunucusu grubu ortamında artırmak için önbelleğe alma kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 861664fcad576c11abe052837b72367eb2b9479a
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095687"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Dağıtılmış bir önbellekte ASP.NET Core ile çalışma

Tarafından [Steve Smith](https://ardalis.com/)

Dağıtılmış önbellek, özellikle bulutta veya sunucu grubu içinde barındırıldığında ASP.NET Core uygulamaları, ölçeklenebilirliğini ve performansı artırabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Dağıtılmış önbellek nedir

Dağıtılmış önbellek birden çok uygulama sunucuları tarafından paylaşılan (bkz [önbellek Temelleri](memory.md#caching-basics)). Tek tek web sunucuları bellekte önbellekte bilgi saklanmaz ve önbelleğe alınan verilerin tüm uygulama sunucuları için kullanılabilir. Bu, çeşitli avantajlar sağlar:

1. Önbelleğe alınan tüm web sunucularında tutarlı verilerdir. Kullanıcıların bağlı olarak hangi web sunucusu isteği işleme farklı sonuçlar göremiyorum

2. Önbelleğe alınmış veriler, web sunucu yeniden başlatılır ve dağıtımları devam eder. Tek tek web sunucuları, kaldırıldı veya önbellek etkilemeden eklendi.

3. Kaynak veri deposu, kendisine (birden çok bellek içi önbellekler veya no ile tüm önbelleğe alma çok) daha az istekleri sahiptir.

> [!NOTE]
> Bu avantajlar bazıları yalnızca bir SQL Server dağıtılmış önbellek kullanıyorsanız, önbellek için uygulamanın kaynak verileri için ayrı bir veritabanı kullanılıyorsa true.

Herhangi bir önbellek gibi dağıtılmış bir önbellek genellikle veri bir ilişkisel veritabanı (veya web hizmeti) çok daha hızlı bir şekilde önbelleğe alınabilir olduğundan bir uygulamanın yanıt verme hızını, önemli ölçüde artırabilir.

Önbellek, uygulama belirli yapılandırmadır. Bu makalede, SQL Server dağıtılmış önbelleğe alır ve nasıl yapılandırmak için her ikisi de Redis açıklanmaktadır. Hangi uygulamayı bağımsız olarak işaretlenirse, uygulama yaygın bir önbellek ile etkileşime giren `IDistributedCache` arabirimi.

## <a name="the-idistributedcache-interface"></a>IDistributedCache arabirimi

`IDistributedCache` Arabirimi zaman uyumlu ve zaman uyumsuz yöntemler içerir. Arabirim öğeleri eklenen, alınan ve dağıtılmış önbellek uygulamasından kaldırıldı sağlar. `IDistributedCache` Arabirimi aşağıdaki yöntemleri içerir:

**GET, GetAsync**

Bir dize anahtarı alır ve önbelleğe alınan öğe olarak alır. bir `byte[]` , önbellekte bulundu.

**SetAsync kümesi**

Bir öğe ekler (olarak `byte[]`) bir dize anahtarı kullanarak önbelleğe.

**Yenileme, RefreshAsync**

Kendi kayan zaman aşımı (varsa) sıfırlama kendi anahtarını temel alan önbellekteki bir öğeyi yeniler.

**RemoveAsync Kaldır**

Kendi anahtarını temel alan bir önbellek girdisi kaldırır.

Kullanılacak `IDistributedCache` arabirimi:

   1. Gerekli NuGet paketlerini proje dosyanıza ekleyin.

   2. Özel uygulanışı yapılandırma `IDistributedCache` içinde `Startup` sınıfın `ConfigureServices` yöntemi ve kapsayıcıya var. ekleyin.

   3. Uygulamanın gelen [ara yazılım](xref:fundamentals/middleware/index) veya MVC denetleyici sınıflarına istek örneği `IDistributedCache` oluşturucudan. Örneği tarafından sağlanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı).

> [!NOTE]
> Bir Tekliyi veya kapsamındaki ömrü boyunca kullanılacak gerek yoktur `IDistributedCache` örnekleri (en az yerleşik uygulamalar için). Bir gereksinim duyabileceğiniz her yerde örneği de oluşturabilirsiniz (kullanmak yerine [bağımlılık ekleme](../../fundamentals/dependency-injection.md)), ancak bu kodunuzu test etmek daha zor hale getirebilir ve ihlal [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).

Aşağıdaki örnek, bir örneğini kullanması gösterilmektedir `IDistributedCache` basit bir ara yazılım bileşeni içinde:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

Yukarıdaki kodda, önbelleğe alınan değeri okuyun, ancak hiçbir zaman yazılır. Bu örnekte, bir sunucu başlatıldığında ve değişmez değer yalnızca ayarlanır. Çok sunuculu bir senaryoda, diğer sunucular tarafından ayarlanan herhangi bir önceki değeri başlatmak için en son sunucu üzerine yazar. `Get` Ve `Set` yöntemleri `byte[]` türü. Bu nedenle, dize değeri kullanarak dönüştürülmelidir `Encoding.UTF8.GetString` (için `Get`) ve `Encoding.UTF8.GetBytes` (için `Set`).

Aşağıdaki kodu *Startup.cs* ayarlanan değer gösterir:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Bu yana `IDistributedCache` yapılandırılan `ConfigureServices` yöntemi, kullanılabilir `Configure` yönteme bir parametre olarak. Bir parametre olarak ekleme DI sağlanacak yapılandırılmış örneği izin verir.

## <a name="using-a-redis-distributed-cache"></a>Dağıtılmış bir Redis önbelleği kullanma

[Redis](https://redis.io/) dağıtılmış bir önbellek sık kullanılan bir açık kaynak bellek içi veri deposu. Yerel olarak kullanabilirsiniz ve yapılandırabileceğiniz bir [Azure Redis Cache](https://azure.microsoft.com/services/cache/) Azure'da barındırılan ASP.NET Core uygulamalarınız için. Önbellek uygulaması kullanarak ASP.NET Core uygulamanızı yapılandırır bir `RedisDistributedCache` örneği.

Redis uygulamasında yapılandırma `ConfigureServices` ve uygulama kodunuzda bir örneğini isteyerek erişim `IDistributedCache` (Yukarıdaki kod bakın).

Örnek kodda bir `RedisCache` uygulama kullanılan sunucu için yapılandırıldığında bir `Staging` ortam. Bu nedenle `ConfigureStagingServices` yöntemi yapılandırır `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Yerel makinenizde Redis yüklemek için chocolatey paket yükleme [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) çalıştırıp `redis-server` bir komut isteminden.

## <a name="using-a-sql-server-distributed-cache"></a>SQL Server'ı kullanarak dağıtılmış önbellek

SqlServerCache uygulama, yedekleme deposu bir SQL Server veritabanını kullanmak dağıtılmış bir önbellek sağlar. SQL Server'ı oluşturmak için belirttiğiniz ad ve şema ile sql önbelleği aracı, Aracı'nı kullanabilirsiniz. Tablo bir tablo oluşturur.

::: moniker range="< aspnetcore-2.1"

Ekleme `SqlConfig.Tools` için `<ItemGroup>` proje dosyası ve çalışma öğesi `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Aşağıdaki komutu çalıştırarak SqlConfig.Tools test edin:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools kullanım, Seçenekler ve komut Yardımı görüntüler.

Çalıştırarak SQL Server'da bir tablo oluşturma `sql-cache create` komutu:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

Aşağıdaki şemayı oluşturduğunuz bir tabloya sahiptir:

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

Tüm önbellek uygulaması gibi uygulamanız get ve set örneğini kullanarak önbellek değerleri `IDistributedCache`değil bir `SqlServerCache`. Örnek uygulayan `SqlServerCache` üretim ortamında (olarak yapılandırıldığı şekilde `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> `ConnectionString` (Ve isteğe bağlı olarak `SchemaName` ve `TableName`) kimlik bilgilerini olabileceğinden genellikle (örneğin, UserSecrets), kaynak denetimi dışında depolanması gerekir.

## <a name="recommendations"></a>Önerileri

Hangi uygulamasının verirken `IDistributedCache` SQL Server tabanlı mevcut altyapınıza ve ortam, performans gereksinimlerinizi ve takımınızın deneyimi ve Redis arasında doğrudan uygulamanız için seçin. Takımınız, Redis ile çalışma konusunda daha yetkin ise, mükemmel bir seçimdir. SQL Server takımınızın tercih ettiği, bu uygulamada emin olabilirsiniz. Geleneksel bir önbellek çözümü veri sağlayan hızlı verilerinin alınması için bellek içi içerdiğini unutmayın. Bir ön bellekte yaygın olarak kullanılan veri depolama ve SQL Server veya Azure depolama gibi bir arka uç kalıcı depoya tüm verileri depolar gerekir. Redis önbelleği SQL önbellek karşılaştırıldığında yüksek çıkış ve düşük gecikme sunan bir önbelleğe alma çözümüdür.

## <a name="additional-resources"></a>Ek kaynaklar

* [Redis Cache azure'da](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure'da SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/primitives/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
