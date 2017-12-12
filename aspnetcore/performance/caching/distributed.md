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
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a>ASP.NET Core dağıtılmış önbelleğinde ile çalışma

Tarafından [Steve Smith](https://ardalis.com/)

Dağıtılmış önbellek özellikle bir bulut veya sunucu grubu ortamında barındırıldığında ASP.NET Core uygulamaları ölçeklenebilirliğini ve performansı artırabilir. Bu makalede, ASP.NET Core'nın yerleşik dağıtılmış önbellek soyutlamalar ve uygulamaları ile nasıl çalışılacağı açıklanmaktadır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>Dağıtılmış önbellek nedir

Dağıtılmış önbellek birden çok uygulama sunucuları tarafından paylaşılan (bkz [önbelleğe alma Temelleri](memory.md#caching-basics)). Önbellek bilgileri tek tek web sunucuları bellekte depolanmaz ve önbelleğe alınan veriler tüm uygulamanın sunucuları için kullanılabilir. Bu, çeşitli avantajları sağlar:

1. Önbelleğe alınan tüm web sunucularında tutarlı verilerdir. Kullanıcıların hangi web bağlı olarak sunucu, isteği işler farklı sonuçlar görmüyorum

2. Önbelleğe alınan veriler web sunucu yeniden başlatıldıktan ve dağıtımları devam eder. Tek tek web sunucuları, kaldırıldı veya önbellek etkilemeden eklendi.

3. Kaynak veri deposu kendisine (birden çok bellek içi önbellekte veya Hayır hiç önbellek daha) yapılan daha az istekleri sahiptir.

> [!NOTE]
> Yalnızca bir SQL Server dağıtılmış önbellek kullanıyorsanız, bu avantajlar bazıları önbelleği uygulamanın kaynak verileri için ayrı veritabanı örneği kullanılıyorsa true.

Herhangi bir önbellek gibi dağıtılmış önbellek genellikle veri bir ilişkisel veritabanı (veya web hizmeti) çok daha hızlı önbellekten alınabilir bu yana bir uygulamanın yanıtlama hızı, önemli ölçüde artırabilir.

Önbellek yapılandırma belirli uygulamasıdır. Bu makalede nasıl yapılandırılacağı her ikisi de Redis ve SQL Server dağıtılmış önbelleğe açıklanmaktadır. Hangi uygulama seçili bağımsız olarak, uygulama bir ortak kullanarak önbelleği ile etkileşim `IDistributedCache` arabirimi.

## <a name="the-idistributedcache-interface"></a>IDistributedCache arabirimi

`IDistributedCache` Arabirim, zaman uyumlu ve zaman uyumsuz yöntemleri içerir. Arabirimi eklenebilir, alınan ve dağıtılmış önbellek uygulamasından kaldırılabilir öğelerine izin verir. `IDistributedCache` Arabirimi aşağıdaki yöntemleri içerir:

**GET, GetAsync**

Bir dize anahtarı alır ve önbelleğe alınan bir öğe olarak alır bir `byte[]` , önbellekte bulunamadı.

**Ayarlama, SetAsync**

Bir öğe ekler (olarak `byte[]`) bir dize anahtarı kullanarak önbelleği için.

**Yenileme, RefreshAsync**

Bir öğe (varsa), kayan zaman aşımı süresi sıfırlama kendi anahtarı, temel önbelleğinde yeniler.

**Kaldır, RemoveAsync**

Alt anahtarına göre bir önbellek girişi kaldırır.

Kullanılacak `IDistributedCache` arabirimi:

   1. Gerekli NuGet paketlerini proje dosyanıza ekleyin.

   2. Belirli uygulanması yapılandırma `IDistributedCache` içinde `Startup` sınıfının `ConfigureServices` yöntemi ve orada kapsayıcıya ekleyin.

   3. Uygulamanın gelen [ara yazılımı](../../fundamentals/middleware.md) veya MVC denetleyicisi sınıfları, istek örneği `IDistributedCache` oluşturucusundan. Örneği tarafından sağlanan [bağımlılık ekleme](../../fundamentals/dependency-injection.md) (dı).

> [!NOTE]
> Singleton veya kapsamındaki yaşam süresi kullanmaya gerek yoktur `IDistributedCache` örnekleri (en az yerleşik uygulamaları için). Bir gereksinim duyabileceğiniz her yerde bir örneği de oluşturabilirsiniz (kullanmak yerine [bağımlılık ekleme](../../fundamentals/dependency-injection.md)), ancak bu kodunuzu test etmek daha zor hale getirebilir ve ihlal [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).

Aşağıdaki örnek, bir örneğini kullanması gösterilmiştir `IDistributedCache` basit ara yazılım bileşeni içinde:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

Yukarıdaki kod önbelleğe alınan değer okuma, ancak hiçbir zaman yazılır. Bu örnekte, bir sunucu başlatıldığında ve değişmez değer yalnızca ayarlanır. Çok sunuculu bir senaryoda, diğer sunucular tarafından ayarlanan herhangi bir önceki değeri başlatmak için en son sunucu üzerine yazar. `Get` Ve `Set` yöntemleri kullanın `byte[]` türü. Bu nedenle, dize değeri kullanılarak dönüştürülmelidir `Encoding.UTF8.GetString` (için `Get`) ve `Encoding.UTF8.GetBytes` (için `Set`).

Aşağıdaki kod *haline* ayarlanan değer gösterir:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> Bu yana `IDistributedCache` yapılandırılan `ConfigureServices` yöntemi, onu kullanılabilir `Configure` yöntemi bir parametre olarak. Parametre olarak eklenmesi dı sağlanacak yapılandırılmış örneği olanak tanır.

## <a name="using-a-redis-distributed-cache"></a>Dağıtılmış Redis önbelleği kullanma

[Redis](https://redis.io/) dağıtılmış önbellek sık kullanılan bir açık kaynak bellek içi veri deposu. Yerel olarak kullanın ve yapılandırabileceğiniz bir [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/) Azure barındırılan ASP.NET Core uygulamalarınız için. Önbellek uygulamasını kullanarak ASP.NET Core uygulamanızı yapılandırır bir `RedisDistributedCache` örneği.

Redis uygulamasında yapılandırma `ConfigureServices` ve örneği isteyerek uygulama kodunuzda erişim `IDistributedCache` (Yukarıdaki kod bakın).

Örnek kodda bir `RedisCache` uygulama sunucusu için yapılandırıldığında kullanılır bir `Staging` ortamı. Bu nedenle `ConfigureStagingServices` yöntemi yapılandırır `RedisCache`:

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> Yerel makinenizde Redis yüklemek için chocolatey paketini Yükle [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) çalıştırıp `redis-server` bir komut isteminden.

## <a name="using-a-sql-server-distributed-cache"></a>SQL Server'ı kullanarak dağıtılmış önbellek

SqlServerCache uygulama, yedekleme deposu olarak bir SQL Server veritabanını kullanmak dağıtılmış önbellek sağlar. SQL sunucusu oluşturmak için belirttiğiniz adı ve şema ile sql önbellek aracı Aracı'nı kullanabilirsiniz Tablosu bir tablo oluşturur.

Sql önbellek aracını kullanmak için add `SqlConfig.Tools` için `<ItemGroup>` öğesinin *.csproj* dosya ve dotnet geri yükleme çalıştırın.

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

Aşağıdaki komutu çalıştırarak SqlConfig.Tools test etme

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

"sql önbellek oluşturma" komutunu çalıştırarak sql Server'a tablolar oluşturabilirsiniz artık sql önbelleği aracı kullanımı, seçenekleri ve komut Yardım görüntülenir:

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

Oluşturulan tablonun aşağıdaki şema sahiptir:

![SqlServer önbelleği tablosu](distributed/_static/SqlServerCacheTable.png)

Tüm önbellek uygulamaları gibi uygulamanız alma ve ayarlama örneği kullanarak önbellek değerleri `IDistributedCache`değil bir `SqlServerCache`. Örnek uygulayan `SqlServerCache` içinde `Production` ortam (olarak yapılandırıldığı şekilde `ConfigureProductionServices`).

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> `ConnectionString` (Ve isteğe bağlı olarak `SchemaName` ve `TableName`) kimlik bilgileri içerebilir gibi tipik olarak (örneğin, UserSecrets), kaynak denetimi dışında depolanması gerekir.

## <a name="recommendations"></a>Önerileri

Hangi uyarlamasını karar verirken `IDistributedCache` sağ uygulamanız için Redis arasında seçin ve SQL Server tabanlı, mevcut altyapınızı ve ortam, performans gereksinimlerinizi ve ekibinizin deneyimi. Ekibinizin Redis ile çalışmayı daha rahat ise, mükemmel bir seçimdir. SQL Server, takım tercih ediyorsa, bu uygulamada emin olabilir. Geleneksel bir önbelleğe alma çözümünü verileri hızlı veri alımını sağlayan bellek içi içerdiğini unutmayın. Yaygın olarak kullanılan verilerini bir önbellekte depolamak ve SQL Server ya da Azure depolama gibi bir arka uç kalıcı deposundaki tüm verileri depolamak gerekir. Redis önbelleği karşılaştırıldığında SQL önbellek yüksek verimlilik ve düşük gecikme süresi sunan bir önbelleğe alma çözümüdür.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure önbelleği redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Azure üzerinde SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/)
* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıt önbelleğe alma](xref:performance/caching/response)
* [Yanıt önbelleğe alma Ara](xref:performance/caching/middleware)
* [Önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
