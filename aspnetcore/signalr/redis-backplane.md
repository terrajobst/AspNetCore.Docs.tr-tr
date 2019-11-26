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
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="70bf4-103">ASP.NET Core SignalR genişleme için Redsıs arka düzlemi ayarlama</span><span class="sxs-lookup"><span data-stu-id="70bf4-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="70bf4-104">, [Andrew Stanton-nuri](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster)ve [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="70bf4-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="70bf4-105">Bu makalede, bir ASP.NET Core SignalR uygulamasını ölçeklendirmek için kullanılacak bir [redo](https://redis.io/) sunucusu ayarlamanın SignalRözgü yönleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="70bf4-106">Redsıs geri düzlemi ayarlama</span><span class="sxs-lookup"><span data-stu-id="70bf4-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="70bf4-107">Redsıs sunucusunu dağıtın.</span><span class="sxs-lookup"><span data-stu-id="70bf4-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="70bf4-108">Üretim kullanımı için, yalnızca SignalR uygulamasıyla aynı veri merkezinde çalıştığında Redsıs geri düzlemi önerilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="70bf4-109">Aksi takdirde, ağ gecikmesi performansı düşürür.</span><span class="sxs-lookup"><span data-stu-id="70bf4-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="70bf4-110">SignalR uygulamanız Azure bulutu 'nda çalışıyorsa, redin geri düzlemi yerine Azure SignalR hizmeti önerilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="70bf4-111">Geliştirme ve test ortamları için Azure Redis Cache hizmetini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="70bf4-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="70bf4-112">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="70bf4-113">Redsıs belgeleri</span><span class="sxs-lookup"><span data-stu-id="70bf4-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="70bf4-114">Azure Redis Cache belgeleri</span><span class="sxs-lookup"><span data-stu-id="70bf4-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="70bf4-115">SignalR uygulamasında `Microsoft.AspNetCore.SignalR.Redis` NuGet paketini () yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="70bf4-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>
* <span data-ttu-id="70bf4-116">`Startup.ConfigureServices` yönteminde, `AddSignalR`sonra `AddRedis` çağırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="70bf4-117">Seçenekleri gerektiği şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="70bf4-118">Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="70bf4-119">`ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="70bf4-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="70bf4-120">Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="70bf4-121">Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.</span><span class="sxs-lookup"><span data-stu-id="70bf4-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="70bf4-122">Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="70bf4-123">SignalR uygulamasında, aşağıdaki NuGet paketlerinden birini yüklemelisiniz:</span><span class="sxs-lookup"><span data-stu-id="70bf4-123">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="70bf4-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis`-StackExchange 'e bağlıdır. Redsıs 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="70bf4-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="70bf4-125">Bu, ASP.NET Core 2,2 ve üzeri için önerilen pakettir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-125">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="70bf4-126">`Microsoft.AspNetCore.SignalR.Redis`-StackExchange. Redsıs 1. X.X. 'e bağımlıdır</span><span class="sxs-lookup"><span data-stu-id="70bf4-126">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="70bf4-127">Bu paket ASP.NET Core 3,0 ve üzeri bir sürüme dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-127">This package isn't included in ASP.NET Core 3.0 and later.</span></span>

* <span data-ttu-id="70bf4-128">`Startup.ConfigureServices` yönteminde, <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-128">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 <span data-ttu-id="70bf4-129">`Microsoft.AspNetCore.SignalR.Redis`kullanırken <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="70bf4-129">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

* <span data-ttu-id="70bf4-130">Seçenekleri gerektiği şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="70bf4-131">Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="70bf4-132">`ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="70bf4-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="70bf4-133">Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="70bf4-134">Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.</span><span class="sxs-lookup"><span data-stu-id="70bf4-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 <span data-ttu-id="70bf4-135">`Microsoft.AspNetCore.SignalR.Redis`kullanırken <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>çağırın.</span><span class="sxs-lookup"><span data-stu-id="70bf4-135">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

  <span data-ttu-id="70bf4-136">Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-136">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="70bf4-137">Redu seçenekleri hakkında daha fazla bilgi için bkz. [StackExchange redin belgeleri](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="70bf4-137">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="70bf4-138">SignalR uygulamasında, aşağıdaki NuGet paketini yüklerken:</span><span class="sxs-lookup"><span data-stu-id="70bf4-138">In the SignalR app, install the following NuGet package:</span></span>

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* <span data-ttu-id="70bf4-139">`Startup.ConfigureServices` yönteminde, <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-139">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* <span data-ttu-id="70bf4-140">Seçenekleri gerektiği şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-140">Configure options as needed:</span></span>
 
  <span data-ttu-id="70bf4-141">Çoğu seçenek bağlantı dizesinde veya [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesnesinde ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-141">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="70bf4-142">`ConfigurationOptions` belirtilen seçenekler bağlantı dizesinde ayarlanmış olanları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="70bf4-142">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="70bf4-143">Aşağıdaki örnek, `ConfigurationOptions` nesnesindeki seçeneklerin nasıl ayarlanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-143">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="70bf4-144">Bu örnek, aşağıdaki adımda anlatıldığı gibi birden çok uygulamanın aynı redo örneğini paylaşabilmesi için bir kanal öneki ekler.</span><span class="sxs-lookup"><span data-stu-id="70bf4-144">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="70bf4-145">Yukarıdaki kodda `options.Configuration`, bağlantı dizesinde belirtilen şeyle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-145">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="70bf4-146">Redu seçenekleri hakkında daha fazla bilgi için bkz. [StackExchange redin belgeleri](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="70bf4-146">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="70bf4-147">Birden çok SignalR uygulama için bir Redsıs sunucusu kullanıyorsanız, her bir SignalR uygulaması için farklı bir kanal öneki kullanın.</span><span class="sxs-lookup"><span data-stu-id="70bf4-147">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="70bf4-148">Kanal öneki ayarlamak, farklı kanal öneklerini kullanan diğerlerinden bir SignalR uygulamasını yalıtır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-148">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="70bf4-149">Farklı ön ekler atamadıysanız, bir uygulamadan tüm istemcilerine gönderilen bir ileti, Redo sunucusunu bir geri düzlemi olarak kullanan tüm uygulamaların tüm istemcilerine gider.</span><span class="sxs-lookup"><span data-stu-id="70bf4-149">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="70bf4-150">Sunucu grubu yük dengeleme yazılımınızı yapışkan oturumlar için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="70bf4-150">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="70bf4-151">Bunun nasıl yapılacağını gösteren bazı örnekler aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="70bf4-151">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="70bf4-152">ISS</span><span class="sxs-lookup"><span data-stu-id="70bf4-152">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="70bf4-153">HAProxy</span><span class="sxs-lookup"><span data-stu-id="70bf4-153">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="70bf4-154">NGINX</span><span class="sxs-lookup"><span data-stu-id="70bf4-154">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="70bf4-155">pfSense</span><span class="sxs-lookup"><span data-stu-id="70bf4-155">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="70bf4-156">Redsıs sunucu hataları</span><span class="sxs-lookup"><span data-stu-id="70bf4-156">Redis server errors</span></span>

<span data-ttu-id="70bf4-157">Bir redin sunucusu aşağı gittiğinde SignalR, iletilerin teslim edilmediğini belirten özel durumlar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="70bf4-157">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="70bf4-158">Bazı tipik özel durum iletileri:</span><span class="sxs-lookup"><span data-stu-id="70bf4-158">Some typical exception messages:</span></span>

* <span data-ttu-id="70bf4-159">*İleti yazılamadı*</span><span class="sxs-lookup"><span data-stu-id="70bf4-159">*Failed writing message*</span></span>
* <span data-ttu-id="70bf4-160">*' MethodName ' hub yöntemi çağrılamadı*</span><span class="sxs-lookup"><span data-stu-id="70bf4-160">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="70bf4-161">*Redsıs bağlantısı başarısız oldu*</span><span class="sxs-lookup"><span data-stu-id="70bf4-161">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="70bf4-162">, sunucu geri geldiğinde iletileri göndermek için arabelleğe almaz.</span><span class="sxs-lookup"><span data-stu-id="70bf4-162"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="70bf4-163">Redsıs sunucusu kapatıldığında gönderilen iletiler kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-163">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="70bf4-164">Redi sunucusu yeniden kullanılabilir olduğunda SignalR otomatik olarak yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="70bf4-164">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="70bf4-165">Bağlantı hatalarıyla ilgili özel davranış</span><span class="sxs-lookup"><span data-stu-id="70bf4-165">Custom behavior for connection failures</span></span>

<span data-ttu-id="70bf4-166">Redsıs bağlantı hatası olaylarının nasıl işleneceğini gösteren bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-166">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="redis-clustering"></a><span data-ttu-id="70bf4-167">Redsıs Kümelemesi</span><span class="sxs-lookup"><span data-stu-id="70bf4-167">Redis Clustering</span></span>

<span data-ttu-id="70bf4-168">[Redsıs Kümelemesi](https://redis.io/topics/cluster-spec) , birden çok redo sunucusu kullanarak yüksek kullanılabilirlik elde etmek için kullanılan bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-168">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="70bf4-169">Kümeleme resmi olarak desteklenmez, ancak çalışmayabilir.</span><span class="sxs-lookup"><span data-stu-id="70bf4-169">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70bf4-170">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="70bf4-170">Next steps</span></span>

<span data-ttu-id="70bf4-171">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="70bf4-171">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="70bf4-172">Redsıs belgeleri</span><span class="sxs-lookup"><span data-stu-id="70bf4-172">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="70bf4-173">StackExchange redin belgeleri</span><span class="sxs-lookup"><span data-stu-id="70bf4-173">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="70bf4-174">Azure Redis Cache belgeleri</span><span class="sxs-lookup"><span data-stu-id="70bf4-174">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
