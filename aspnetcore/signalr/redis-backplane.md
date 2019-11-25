---
title: ASP.NET Core SignalR genişletme için redsıs geri düzlemi
author: bradygaster
description: Bir ASP.NET Core SignalR uygulaması için ölçeklendirmeyi etkinleştirmek üzere Redsıs arka düzlemi ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 0461fc6a212ba78111bc2054cca74951721c5820
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289032"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a>ASP.NET Core SignalR genişleme için Redsıs arka düzlemi ayarlama

, [Andrew Stanton-nuri](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)ve [Tom Dykstra](https://github.com/tdykstra),

Bu makalede, bir ASP.NET Core SignalR uygulamasını ölçeklendirmek için kullanılacak bir [redo](https://redis.io/) sunucusu ayarlamanın SignalRözgü yönleri açıklanmaktadır.

## <a name="set-up-a-redis-backplane"></a>Redsıs geri düzlemi ayarlama

* Redsıs sunucusunu dağıtın.

  > [!IMPORTANT] 
  > Üretim kullanımı için, yalnızca SignalR uygulamasıyla aynı veri merkezinde çalıştığında Redsıs geri düzlemi önerilir. Aksi takdirde, ağ gecikmesi performansı düşürür. SignalR uygulamanız Azure bulutu 'nda çalışıyorsa, redin geri düzlemi yerine Azure SignalR hizmeti önerilir. Geliştirme ve test ortamları için Azure Redis Cache hizmetini kullanabilirsiniz.

  Daha fazla bilgi için aşağıdaki kaynaklara bakın:

  * <xref:signalr/scale>
  * [Redsıs belgeleri](https://redis.io/)
  * [Azure Redis Cache belgeleri](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* SignalR uygulamasında `Microsoft.AspNetCore.SignalR.Redis` NuGet paketini () yüklemelisiniz.
* `Startup.ConfigureServices` yönteminde, `AddSignalR`sonra `AddRedis` çağırın:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Seçenekleri gerektiği şekilde yapılandırın:
 
  Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir. `ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.

  Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir. Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* SignalR uygulamasında, aşağıdaki NuGet paketlerinden birini yüklemelisiniz:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`-StackExchange 'e bağlıdır. Redsıs 2. X.X. Bu, ASP.NET Core 2,2 ve üzeri için önerilen pakettir.
  * `Microsoft.AspNetCore.SignalR.Redis`-StackExchange. Redsıs 1. X.X. 'e bağımlıdır Bu paket ASP.NET Core 3,0 ve üzeri bir sürüme dahil değildir.

* `Startup.ConfigureServices` yönteminde, <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>çağırın:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 `Microsoft.AspNetCore.SignalR.Redis`kullanırken <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>çağırın.

* Seçenekleri gerektiği şekilde yapılandırın:
 
  Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir. `ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.

  Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir. Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 `Microsoft.AspNetCore.SignalR.Redis`kullanırken <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>çağırın.

  Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.

  Redu seçenekleri hakkında daha fazla bilgi için bkz. [StackExchange redin belgeleri](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* SignalR uygulamasında, aşağıdaki NuGet paketini yüklerken:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* `Startup.ConfigureServices` yönteminde, <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>çağırın:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* Seçenekleri gerektiği şekilde yapılandırın:
 
  Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir. `ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.

  Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir. Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.

  Redu seçenekleri hakkında daha fazla bilgi için bkz. [StackExchange redin belgeleri](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Birden çok SignalR uygulama için bir Redsıs sunucusu kullanıyorsanız, her bir SignalR uygulaması için farklı bir kanal öneki kullanın.

  Kanal öneki ayarlamak, farklı kanal öneklerini kullanan diğerlerinden bir SignalR uygulamasını yalıtır. Farklı ön ekler atamadıysanız, bir uygulamadan tüm istemcilerine gönderilen bir ileti, Redo sunucusunu bir geri düzlemi olarak kullanan tüm uygulamaların tüm istemcilerine gider.

* Sunucu grubu yük dengeleme yazılımınızı yapışkan oturumlar için yapılandırın. Bunun nasıl yapılacağını gösteren bazı örnekler aşağıda verilmiştir:

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [NGINX](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Redsıs sunucu hataları

Bir redin sunucusu aşağı gittiğinde SignalR, iletilerin teslim edilmediğini belirten özel durumlar oluşturur. Bazı tipik özel durum iletileri:

* *İleti yazılamadı*
* *' MethodName ' hub yöntemi çağrılamadı*
* *Redsıs bağlantısı başarısız oldu*

SignalR, sunucu geri geldiğinde iletileri göndermek için arabelleğe almaz. Redsıs sunucusu kapatıldığında gönderilen iletiler kaybedilir.

Redi sunucusu yeniden kullanılabilir olduğunda SignalR otomatik olarak yeniden bağlanır.

### <a name="custom-behavior-for-connection-failures"></a>Bağlantı hatalarıyla ilgili özel davranış

Redsıs bağlantı hatası olaylarının nasıl işleneceğini gösteren bir örnek aşağıda verilmiştir.

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="redis-clustering"></a>Redsıs Kümelemesi

[Redsıs Kümelemesi](https://redis.io/topics/cluster-spec) , birden çok redo sunucusu kullanarak yüksek kullanılabilirlik elde etmek için kullanılan bir yöntemdir. Kümeleme resmi olarak desteklenmez, ancak çalışmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* <xref:signalr/scale>
* [Redsıs belgeleri](https://redis.io/documentation)
* [StackExchange redin belgeleri](https://stackexchange.github.io/StackExchange.Redis/)
* [Azure Redis Cache belgeleri](https://docs.microsoft.com/azure/redis-cache/)
