---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 4706a1e2774fa9f6fb40085da944e8a82476ef05
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820038"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ea68c-103">ASP.NET Core SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ea68c-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ea68c-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ea68c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ea68c-105">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ea68c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ea68c-106">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ea68c-107">JSON serileştirme, yöntekinizdeki `Startup.ConfigureServices` [addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilen [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ea68c-108">Yöntemi `AddJsonProtocol` , bir `options` nesneyi alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ea68c-109">Bu nesnedeki [payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin ve dönüş `JsonSerializerSettings` değerlerinin serileştirilmesi yapılandırmak için kullanılabilen bir JSON.net nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ea68c-110">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm) bakın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="ea68c-111">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ea68c-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="ea68c-112">.Net istemcisinde, [hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)'da `AddJsonProtocol` aynı genişletme yöntemi bulunur.</span><span class="sxs-lookup"><span data-stu-id="ea68c-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ea68c-113">Uzantı metodunu çözümlemek için adalanıiçeriaktarılmalıdır:`Microsoft.Extensions.DependencyInjection`</span><span class="sxs-lookup"><span data-stu-id="ea68c-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="ea68c-114">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ea68c-115">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ea68c-115">MessagePack serialization options</span></span>

<span data-ttu-id="ea68c-116">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ea68c-117">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ea68c-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ea68c-118">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ea68c-119">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-119">Configure server options</span></span>

<span data-ttu-id="ea68c-120">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ea68c-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ea68c-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-121">Option</span></span> | <span data-ttu-id="ea68c-122">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-122">Default Value</span></span> | <span data-ttu-id="ea68c-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ea68c-124">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-124">30 seconds</span></span> | <span data-ttu-id="ea68c-125">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ea68c-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ea68c-126">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ea68c-127">Önerilen değer Double `KeepAliveInterval` değeridir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ea68c-128">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-128">15 seconds</span></span> | <span data-ttu-id="ea68c-129">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ea68c-130">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-131">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ea68c-132">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-132">15 seconds</span></span> | <span data-ttu-id="ea68c-133">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ea68c-134">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ea68c-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ea68c-135">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="ea68c-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ea68c-136">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ea68c-136">All installed protocols</span></span> | <span data-ttu-id="ea68c-137">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ea68c-137">Protocols supported by this hub.</span></span> <span data-ttu-id="ea68c-138">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ea68c-139">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ea68c-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ea68c-140">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="ea68c-141">İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="ea68c-142">Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="ea68c-143">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-143">Option</span></span> | <span data-ttu-id="ea68c-144">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-144">Default Value</span></span> | <span data-ttu-id="ea68c-145">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ea68c-146">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-146">30 seconds</span></span> | <span data-ttu-id="ea68c-147">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ea68c-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ea68c-148">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ea68c-149">Önerilen değer Double `KeepAliveInterval` değeridir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ea68c-150">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-150">15 seconds</span></span> | <span data-ttu-id="ea68c-151">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ea68c-152">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-153">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ea68c-154">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-154">15 seconds</span></span> | <span data-ttu-id="ea68c-155">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ea68c-156">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ea68c-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ea68c-157">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="ea68c-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ea68c-158">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ea68c-158">All installed protocols</span></span> | <span data-ttu-id="ea68c-159">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ea68c-159">Protocols supported by this hub.</span></span> <span data-ttu-id="ea68c-160">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ea68c-161">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ea68c-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ea68c-162">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="ea68c-163">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-163">Option</span></span> | <span data-ttu-id="ea68c-164">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-164">Default Value</span></span> | <span data-ttu-id="ea68c-165">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ea68c-166">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-166">15 seconds</span></span> | <span data-ttu-id="ea68c-167">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ea68c-168">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-169">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ea68c-170">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-170">15 seconds</span></span> | <span data-ttu-id="ea68c-171">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ea68c-172">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ea68c-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ea68c-173">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="ea68c-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ea68c-174">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ea68c-174">All installed protocols</span></span> | <span data-ttu-id="ea68c-175">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ea68c-175">Protocols supported by this hub.</span></span> <span data-ttu-id="ea68c-176">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ea68c-177">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ea68c-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ea68c-178">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="ea68c-179">Seçenekler, `AddSignalR` içindeki `Startup.ConfigureServices`çağrıya bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="ea68c-180">Tek bir hub için Seçenekler ' de `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve kullanılarak <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ea68c-181">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ea68c-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ea68c-182">Aktarımlar `HttpConnectionDispatcherOptions` ve bellek arabellek yönetimiyle ilgili gelişmiş ayarları yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ea68c-183">Bu seçenekler, ' de `Startup.Configure` [maphub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="ea68c-184">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ea68c-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ea68c-185">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-185">Option</span></span> | <span data-ttu-id="ea68c-186">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-186">Default Value</span></span> | <span data-ttu-id="ea68c-187">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ea68c-188">32 KB</span><span class="sxs-lookup"><span data-stu-id="ea68c-188">32 KB</span></span> | <span data-ttu-id="ea68c-189">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ea68c-190">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ea68c-191">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ea68c-192">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ea68c-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ea68c-193">32 KB</span><span class="sxs-lookup"><span data-stu-id="ea68c-193">32 KB</span></span> | <span data-ttu-id="ea68c-194">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ea68c-195">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ea68c-196">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ea68c-196">All Transports are enabled.</span></span> | <span data-ttu-id="ea68c-197">Bir istemcinin bağlanmak `HttpTransportType` için kullanabileceği taşımaları kısıtlayabilecek değerlerin bir bit maskesi.</span><span class="sxs-lookup"><span data-stu-id="ea68c-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ea68c-198">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-198">See below.</span></span> | <span data-ttu-id="ea68c-199">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ea68c-200">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-200">See below.</span></span> | <span data-ttu-id="ea68c-201">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ea68c-202">Uzun yoklama taşıması, `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ea68c-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-203">Option</span></span> | <span data-ttu-id="ea68c-204">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-204">Default Value</span></span> | <span data-ttu-id="ea68c-205">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ea68c-206">90 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-206">90 seconds</span></span> | <span data-ttu-id="ea68c-207">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ea68c-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ea68c-208">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="ea68c-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ea68c-209">WebSocket taşıması, `WebSockets` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ea68c-210">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-210">Option</span></span> | <span data-ttu-id="ea68c-211">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-211">Default Value</span></span> | <span data-ttu-id="ea68c-212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ea68c-213">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-213">5 seconds</span></span> | <span data-ttu-id="ea68c-214">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ea68c-215">`Sec-WebSocket-Protocol` Üstbilgiyi özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ea68c-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ea68c-216">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ea68c-217">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-217">Configure client options</span></span>

<span data-ttu-id="ea68c-218">İstemci seçenekleri `HubConnectionBuilder` tür üzerinde yapılandırılabilir (.net ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="ea68c-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ea68c-219">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıf, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisi de içerir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ea68c-220">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-220">Configure logging</span></span>

<span data-ttu-id="ea68c-221">Günlüğe kaydetme, .net istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ea68c-222">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ea68c-223">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ea68c-224">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ea68c-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ea68c-225">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ea68c-226">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="ea68c-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ea68c-227">`AddConsole` Uzantı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ea68c-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ea68c-228">JavaScript istemcisinde, benzer `configureLogging` bir yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ea68c-229">Üretilecek günlük `LogLevel` iletilerinin en düşük düzeyini belirten bir değer girin.</span><span class="sxs-lookup"><span data-stu-id="ea68c-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ea68c-230">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ea68c-231">Bir değer yerine, bir günlük düzeyi adını temsil eden `string` bir değer de sağlayabilirsiniz. `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="ea68c-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="ea68c-232">Bu, `LogLevel` sabitlere erişiminizin olmadığı ortamlarda SignalR günlüğünü yapılandırırken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="ea68c-233">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-233">The following table lists the available log levels.</span></span> <span data-ttu-id="ea68c-234">Sağladığınız `configureLogging` değer, günlüğe kaydedilecek **Minimum** günlük düzeyini belirler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="ea68c-235">Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="ea68c-236">Dize</span><span class="sxs-lookup"><span data-stu-id="ea68c-236">String</span></span>                      | <span data-ttu-id="ea68c-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="ea68c-237">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="ea68c-238">`info`**ya da**`information`</span><span class="sxs-lookup"><span data-stu-id="ea68c-238">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="ea68c-239">`warn`**ya da**`warning`</span><span class="sxs-lookup"><span data-stu-id="ea68c-239">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ea68c-240">Günlüğe kaydetmeyi tamamen devre dışı bırakmak `signalR.LogLevel.None` için `configureLogging` yönteminde belirtin.</span><span class="sxs-lookup"><span data-stu-id="ea68c-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ea68c-241">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ea68c-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ea68c-242">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ea68c-243">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ea68c-244">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ea68c-245">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="ea68c-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ea68c-246">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ea68c-247">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-247">Configure allowed transports</span></span>

<span data-ttu-id="ea68c-248">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrıda (`withUrl` JavaScript 'te) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ea68c-249">Bir bit düzeyinde OR değeri `HttpTransportType` , istemciyi yalnızca belirtilen aktarımları kullanacak şekilde kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ea68c-250">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-250">All transports are enabled by default.</span></span>

<span data-ttu-id="ea68c-251">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="ea68c-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ea68c-252">JavaScript istemcisinde aktarımlar, için `transport` `withUrl`belirtilen seçenekler nesnesindeki alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ea68c-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ea68c-253">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="ea68c-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="ea68c-254">Java istemcisinde, taşıma `withTransport` yöntemi `HttpHubConnectionBuilder`ile birlikte seçilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ea68c-255">Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ea68c-256">SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ea68c-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ea68c-257">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-257">Configure bearer authentication</span></span>

<span data-ttu-id="ea68c-258">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim `AccessTokenProvider` belirtecini döndüren`accessTokenFactory` bir işlevi belirtmek için (JavaScript 'te) seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ea68c-259">.Net istemcisinde, bu erişim belirteci http "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (bir `Authorization` `Bearer`türü olan üst bilgi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="ea68c-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ea68c-260">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ea68c-261">Bu durumlarda, erişim belirteci bir sorgu dizesi değeri `access_token`olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ea68c-262">.Net istemcisinde `AccessTokenProvider` seçeneği, içindeki `WithUrl`seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ea68c-263">JavaScript istemcisinde erişim belirteci, içindeki `accessTokenFactory` `withUrl`seçenekler nesnesindeki alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ea68c-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="ea68c-264">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ea68c-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ea68c-265">[Rxjava](https://github.com/ReactiveX/RxJava) [tek\<dize >](https://reactivex.io/documentation/single.html)sağlamak için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ea68c-266">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ea68c-266">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ea68c-267">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ea68c-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ea68c-268">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ea68c-269">.NET</span><span class="sxs-lookup"><span data-stu-id="ea68c-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ea68c-270">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-270">Option</span></span> | <span data-ttu-id="ea68c-271">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-271">Default value</span></span> | <span data-ttu-id="ea68c-272">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ea68c-273">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-274">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-274">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-275">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="ea68c-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ea68c-276">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-277">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-277">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ea68c-278">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-278">15 seconds</span></span> | <span data-ttu-id="ea68c-279">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="ea68c-280">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="ea68c-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ea68c-281">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-282">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-282">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ea68c-283">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-283">15 seconds</span></span> | <span data-ttu-id="ea68c-284">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-284">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ea68c-285">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-285">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ea68c-286">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ea68c-286">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ea68c-287">.Net istemcisinde zaman aşımı değerleri değer olarak `TimeSpan` belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-287">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ea68c-288">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea68c-288">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ea68c-289">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-289">Option</span></span> | <span data-ttu-id="ea68c-290">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-290">Default value</span></span> | <span data-ttu-id="ea68c-291">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-291">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ea68c-292">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-292">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-293">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-293">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-294">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onclose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-294">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ea68c-295">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-295">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-296">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-296">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ea68c-297">15 saniye (15.000 'ları harcanan)</span><span class="sxs-lookup"><span data-stu-id="ea68c-297">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="ea68c-298">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-298">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ea68c-299">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-299">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ea68c-300">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ea68c-300">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ea68c-301">Java</span><span class="sxs-lookup"><span data-stu-id="ea68c-301">Java</span></span>](#tab/java)

| <span data-ttu-id="ea68c-302">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-302">Option</span></span> | <span data-ttu-id="ea68c-303">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-303">Default value</span></span> | <span data-ttu-id="ea68c-304">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-304">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ea68c-305">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ea68c-305">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ea68c-306">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-306">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-307">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-307">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-308">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-308">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ea68c-309">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-309">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-310">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-310">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ea68c-311">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-311">15 seconds</span></span> | <span data-ttu-id="ea68c-312">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-312">Timeout for initial server handshake.</span></span> <span data-ttu-id="ea68c-313">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-313">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ea68c-314">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-314">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-315">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-315">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ea68c-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ea68c-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ea68c-317">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-317">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-318">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-318">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ea68c-319">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-319">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ea68c-320">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ea68c-320">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ea68c-321">.NET</span><span class="sxs-lookup"><span data-stu-id="ea68c-321">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ea68c-322">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-322">Option</span></span> | <span data-ttu-id="ea68c-323">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-323">Default value</span></span> | <span data-ttu-id="ea68c-324">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-324">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ea68c-325">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-325">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-326">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-326">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-327">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="ea68c-327">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ea68c-328">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-328">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-329">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-329">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ea68c-330">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-330">15 seconds</span></span> | <span data-ttu-id="ea68c-331">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-331">Timeout for initial server handshake.</span></span> <span data-ttu-id="ea68c-332">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="ea68c-332">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ea68c-333">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-333">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-334">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-334">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ea68c-335">.Net istemcisinde zaman aşımı değerleri değer olarak `TimeSpan` belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ea68c-335">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ea68c-336">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea68c-336">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ea68c-337">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-337">Option</span></span> | <span data-ttu-id="ea68c-338">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-338">Default value</span></span> | <span data-ttu-id="ea68c-339">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-339">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ea68c-340">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-340">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-341">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-341">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-342">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onclose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-342">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ea68c-343">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-343">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-344">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-344">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ea68c-345">Java</span><span class="sxs-lookup"><span data-stu-id="ea68c-345">Java</span></span>](#tab/java)

| <span data-ttu-id="ea68c-346">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ea68c-346">Option</span></span> | <span data-ttu-id="ea68c-347">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-347">Default value</span></span> | <span data-ttu-id="ea68c-348">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-348">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ea68c-349">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ea68c-349">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ea68c-350">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ea68c-350">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ea68c-351">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-351">Timeout for server activity.</span></span> <span data-ttu-id="ea68c-352">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-352">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ea68c-353">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-353">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ea68c-354">Önerilen değer, ping bir sürenin gelmesi için en az iki `KeepAliveInterval` değerin değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-354">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ea68c-355">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-355">15 seconds</span></span> | <span data-ttu-id="ea68c-356">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ea68c-356">Timeout for initial server handshake.</span></span> <span data-ttu-id="ea68c-357">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="ea68c-357">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ea68c-358">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-358">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ea68c-359">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ea68c-359">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="ea68c-360">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ea68c-360">Configure additional options</span></span>

<span data-ttu-id="ea68c-361">Java istemcisinde `WithUrl` `HttpHubConnectionBuilder` içindeki çeşitli yapılandırma API 'lerinde `HubConnectionBuilder` veya`withUrl` üzerinde bulunan (JavaScript 'te) yönteminde ek seçenekler yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-361">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ea68c-362">.NET</span><span class="sxs-lookup"><span data-stu-id="ea68c-362">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ea68c-363">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="ea68c-363">.NET Option</span></span> |  <span data-ttu-id="ea68c-364">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-364">Default value</span></span> | <span data-ttu-id="ea68c-365">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-365">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ea68c-366">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ea68c-366">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ea68c-367">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-367">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ea68c-368">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ea68c-368">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ea68c-369">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ea68c-369">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ea68c-370">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ea68c-370">Empty</span></span> | <span data-ttu-id="ea68c-371">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ea68c-371">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ea68c-372">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ea68c-372">Empty</span></span> | <span data-ttu-id="ea68c-373">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ea68c-373">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ea68c-374">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ea68c-374">Empty</span></span> | <span data-ttu-id="ea68c-375">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="ea68c-375">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ea68c-376">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ea68c-376">5 seconds</span></span> | <span data-ttu-id="ea68c-377">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ea68c-377">WebSockets only.</span></span> <span data-ttu-id="ea68c-378">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ea68c-378">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ea68c-379">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="ea68c-379">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ea68c-380">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ea68c-380">Empty</span></span> | <span data-ttu-id="ea68c-381">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ea68c-381">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ea68c-382">HTTP istekleri göndermek için `HttpMessageHandler` kullanılan yapılandırmak veya değiştirmek için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ea68c-382">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ea68c-383">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="ea68c-383">Not used for WebSocket connections.</span></span> <span data-ttu-id="ea68c-384">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-384">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ea68c-385">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni `HttpMessageHandler` bir örnek döndürün.</span><span class="sxs-lookup"><span data-stu-id="ea68c-385">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ea68c-386">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="ea68c-386">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ea68c-387">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="ea68c-387">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ea68c-388">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-388">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ea68c-389">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="ea68c-389">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ea68c-390">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ea68c-390">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ea68c-391">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="ea68c-391">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ea68c-392">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea68c-392">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ea68c-393">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="ea68c-393">JavaScript Option</span></span> | <span data-ttu-id="ea68c-394">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-394">Default Value</span></span> | <span data-ttu-id="ea68c-395">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-395">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ea68c-396">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ea68c-396">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ea68c-397">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-397">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ea68c-398">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ea68c-398">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ea68c-399">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ea68c-399">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ea68c-400">Java</span><span class="sxs-lookup"><span data-stu-id="ea68c-400">Java</span></span>](#tab/java)

| <span data-ttu-id="ea68c-401">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="ea68c-401">Java Option</span></span> | <span data-ttu-id="ea68c-402">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ea68c-402">Default Value</span></span> | <span data-ttu-id="ea68c-403">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ea68c-403">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ea68c-404">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ea68c-404">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ea68c-405">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ea68c-405">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ea68c-406">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ea68c-406">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ea68c-407">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ea68c-407">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ea68c-408">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ea68c-408">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ea68c-409">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ea68c-409">Empty</span></span> | <span data-ttu-id="ea68c-410">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ea68c-410">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ea68c-411">.NET Istemcisinde bu seçenekler, aşağıdakileri yapmak için `WithUrl`belirtilen seçenekler temsilcisinden değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-411">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ea68c-412">JavaScript Istemcisinde, bu seçenekler şu şekilde `withUrl`sağlanmış bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ea68c-412">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ea68c-413">Java istemcisinde, bu seçenekler, içindeki `HttpHubConnectionBuilder` döndürülen yöntemlerle yapılandırılabilir`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="ea68c-413">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ea68c-414">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ea68c-414">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
