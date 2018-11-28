---
title: Devre kartına ASP.NET Core SignalR ölçek genişletme için redis
author: tdykstra
description: Bir ASP.NET Core SignalR uygulama için ölçek genişletme etkinleştirmek için bir Redis devre kartı kurmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52453033"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="ee73c-103">Bir Redis devre kartı ASP.NET Core SignalR genişleme için ayarlama</span><span class="sxs-lookup"><span data-stu-id="ee73c-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="ee73c-104">Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), ve [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="ee73c-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="ee73c-105">Bu makalede, SignalR özgü yönlerini ayarlama açıklanmaktadır bir [Redis](https://redis.io/) bir ASP.NET Core SignalR uygulamayı kullanıma ölçeklendirme için kullanılacak sunucusu.</span><span class="sxs-lookup"><span data-stu-id="ee73c-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="ee73c-106">Bir Redis devre kartı ayarlayın</span><span class="sxs-lookup"><span data-stu-id="ee73c-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="ee73c-107">Bir Redis sunucusuna dağıtın.</span><span class="sxs-lookup"><span data-stu-id="ee73c-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="ee73c-108">Üretim kullanımı için yalnızca şirket içi altyapı için bir Redis devre kartı önerilir.</span><span class="sxs-lookup"><span data-stu-id="ee73c-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="ee73c-109">Gecikme süresini en aza indirmek için aynı veri merkezinde SignalR uygulama olarak Redis sunucusu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ee73c-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="ee73c-110">SignalR uygulamanızı Azure bulutunda çalışır durumdaysa bir Redis devre kartı yerine Azure SignalR hizmeti öneririz.</span><span class="sxs-lookup"><span data-stu-id="ee73c-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="ee73c-111">Azure Redis Cache hizmeti kullanmak için geliştirme ve test ortamları.</span><span class="sxs-lookup"><span data-stu-id="ee73c-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="ee73c-112">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="ee73c-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="ee73c-113">Redis belgeleri</span><span class="sxs-lookup"><span data-stu-id="ee73c-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="ee73c-114">Azure Redis Cache belgeleri</span><span class="sxs-lookup"><span data-stu-id="ee73c-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="ee73c-115">SignalR uygulamada, yükleme `Microsoft.AspNetCore.SignalR.Redis` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="ee73c-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="ee73c-116">İçinde `Startup.ConfigureServices` yöntemi, çağrı `AddRedis` sonra `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="ee73c-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="ee73c-117">Seçenekleri gerektiği gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ee73c-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="ee73c-118">Bağlantı dizesi veya çoğu seçenekler ayarlanabilir [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesne.</span><span class="sxs-lookup"><span data-stu-id="ee73c-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="ee73c-119">Belirtilen seçenekler `ConfigurationOptions` bağlantı dizesinde olanları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ee73c-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="ee73c-120">Aşağıdaki örnekte de seçeneklerini ayarlama işlemi gösterilmektedir `ConfigurationOptions` nesne.</span><span class="sxs-lookup"><span data-stu-id="ee73c-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="ee73c-121">Birden çok uygulama aynı Redis örneği paylaşabileceği Bu örnek aşağıdaki adımda açıklandığı gibi bir kanal önek ekler.</span><span class="sxs-lookup"><span data-stu-id="ee73c-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="ee73c-122">Önceki kodda, `options.Configuration` ne olursa olsun bağlantı dizesinde belirtilmedi ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="ee73c-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="ee73c-123">SignalR uygulamada, yükleme `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="ee73c-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="ee73c-124">İçinde `Startup.ConfigureServices` yöntemi, çağrı `AddStackExchangeRedis` sonra `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="ee73c-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="ee73c-125">Seçenekleri gerektiği gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ee73c-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="ee73c-126">Bağlantı dizesi veya çoğu seçenekler ayarlanabilir [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) nesne.</span><span class="sxs-lookup"><span data-stu-id="ee73c-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="ee73c-127">Belirtilen seçenekler `ConfigurationOptions` bağlantı dizesinde olanları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ee73c-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="ee73c-128">Aşağıdaki örnekte de seçeneklerini ayarlama işlemi gösterilmektedir `ConfigurationOptions` nesne.</span><span class="sxs-lookup"><span data-stu-id="ee73c-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="ee73c-129">Birden çok uygulama aynı Redis örneği paylaşabileceği Bu örnek aşağıdaki adımda açıklandığı gibi bir kanal önek ekler.</span><span class="sxs-lookup"><span data-stu-id="ee73c-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="ee73c-130">Önceki kodda, `options.Configuration` ne olursa olsun bağlantı dizesinde belirtilmedi ile başlatılır.</span><span class="sxs-lookup"><span data-stu-id="ee73c-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="ee73c-131">Redis seçenekleri hakkında daha fazla bilgi için bkz. [StackExchange Redis belgeleri](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="ee73c-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="ee73c-132">Birden fazla SignalR uygulama için bir Redis sunucusu kullanıyorsanız, her bir SignalR uygulama için farklı bir kanal önek kullanın.</span><span class="sxs-lookup"><span data-stu-id="ee73c-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="ee73c-133">Bir kanal önek ayarı bir SignalR uygulamadan farklı bir kanal önekleri kullanan diğer yalıtır.</span><span class="sxs-lookup"><span data-stu-id="ee73c-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="ee73c-134">Farklı önekler atamazsanız, tümü kendi istemcileri bir uygulamadan gönderilen ileti devre kartı olarak Redis sunucusunu kullanan tüm uygulamalar, tüm istemcilere geçer.</span><span class="sxs-lookup"><span data-stu-id="ee73c-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="ee73c-135">Yazılım Yapışkan oturumlar için sunucu grubunda yük dengelemeyi yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ee73c-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="ee73c-136">Bunu yapmak ilgili belgeler bazı örnekleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="ee73c-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="ee73c-137">IIS</span><span class="sxs-lookup"><span data-stu-id="ee73c-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="ee73c-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="ee73c-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="ee73c-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="ee73c-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="ee73c-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="ee73c-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="ee73c-141">Redis sunucu hataları</span><span class="sxs-lookup"><span data-stu-id="ee73c-141">Redis server errors</span></span>

<span data-ttu-id="ee73c-142">Bir Redis sunucusuna çıktığında SignalR iletileri teslim olmaz gösteren özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ee73c-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="ee73c-143">Bazı tipik bir özel durum iletileri:</span><span class="sxs-lookup"><span data-stu-id="ee73c-143">Some typical exception messages:</span></span>

* <span data-ttu-id="ee73c-144">*Başarısız yazma iletisi*</span><span class="sxs-lookup"><span data-stu-id="ee73c-144">*Failed writing message*</span></span>
* <span data-ttu-id="ee73c-145">*'MethodName' hub'yöntemini çağırmak başarısız oldu*</span><span class="sxs-lookup"><span data-stu-id="ee73c-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="ee73c-146">*Redis bağlantısı başarısız oldu*</span><span class="sxs-lookup"><span data-stu-id="ee73c-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="ee73c-147">SignalR sunucunun geri geldiğinde bunları gönderilecek iletileri arabelleğe almayan.</span><span class="sxs-lookup"><span data-stu-id="ee73c-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="ee73c-148">Redis sunucusu çalışmadığında gönderilen tüm iletiler kaybolur.</span><span class="sxs-lookup"><span data-stu-id="ee73c-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="ee73c-149">SignalR Redis sunucusuna yeniden kullanılabilir olduğunda otomatik olarak yeniden bağlanır.</span><span class="sxs-lookup"><span data-stu-id="ee73c-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="ee73c-150">Bağlantı hataları için özel davranış</span><span class="sxs-lookup"><span data-stu-id="ee73c-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="ee73c-151">Redis bağlantısı hatası olaylarının nasıl ele alınacağını gösteren bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ee73c-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="ee73c-152">Kümeleme</span><span class="sxs-lookup"><span data-stu-id="ee73c-152">Clustering</span></span>

<span data-ttu-id="ee73c-153">Kümeleme, birden çok Redis sunucuları kullanarak yüksek kullanılabilirlik elde etmek için kullanabileceğiniz bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="ee73c-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="ee73c-154">Kümeleme resmi olarak desteklenmez ancak işe yarayabilir.</span><span class="sxs-lookup"><span data-stu-id="ee73c-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee73c-155">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ee73c-155">Next steps</span></span>

<span data-ttu-id="ee73c-156">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="ee73c-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="ee73c-157">Redis belgeleri</span><span class="sxs-lookup"><span data-stu-id="ee73c-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="ee73c-158">StackExchange Redis belgeleri</span><span class="sxs-lookup"><span data-stu-id="ee73c-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="ee73c-159">Azure Redis Cache belgeleri</span><span class="sxs-lookup"><span data-stu-id="ee73c-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
