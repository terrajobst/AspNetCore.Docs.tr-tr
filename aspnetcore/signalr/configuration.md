---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773926"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="b6b50-103">ASP.NET Core SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b6b50-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="b6b50-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b6b50-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="b6b50-105">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="b6b50-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="b6b50-106">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="b6b50-107">JSON serileştirme, yöntekinizdeki `Startup.ConfigureServices` [addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilen [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b6b50-108">Yöntemi `AddJsonProtocol` , bir `options` nesneyi alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b6b50-109">Bu nesnedeki [payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin ve dönüş `JsonSerializerSettings` değerlerinin serileştirilmesi yapılandırmak için kullanılabilen bir JSON.net nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b6b50-110">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm) bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="b6b50-111">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b6b50-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="b6b50-112">.Net istemcisinde, [hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)'da `AddJsonProtocol` aynı genişletme yöntemi bulunur.</span><span class="sxs-lookup"><span data-stu-id="b6b50-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b6b50-113">Uzantı metodunu çözümlemek için adalanıiçeriaktarılmalıdır:`Microsoft.Extensions.DependencyInjection`</span><span class="sxs-lookup"><span data-stu-id="b6b50-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="b6b50-114">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="b6b50-115">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b6b50-115">MessagePack serialization options</span></span>

<span data-ttu-id="b6b50-116">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="b6b50-117">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="b6b50-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="b6b50-118">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="b6b50-119">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-119">Configure server options</span></span>

<span data-ttu-id="b6b50-120">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b6b50-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b6b50-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-121">Option</span></span> | <span data-ttu-id="b6b50-122">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-122">Default Value</span></span> | <span data-ttu-id="b6b50-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b6b50-124">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-124">30 seconds</span></span> | <span data-ttu-id="b6b50-125">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b6b50-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b6b50-126">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b6b50-127">Önerilen değer Double `KeepAliveInterval` değeridir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b6b50-128">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-128">15 seconds</span></span> | <span data-ttu-id="b6b50-129">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b6b50-130">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-131">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b6b50-132">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-132">15 seconds</span></span> | <span data-ttu-id="b6b50-133">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b6b50-134">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b6b50-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b6b50-135">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b6b50-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b6b50-136">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b6b50-136">All installed protocols</span></span> | <span data-ttu-id="b6b50-137">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b6b50-137">Protocols supported by this hub.</span></span> <span data-ttu-id="b6b50-138">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b6b50-139">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b6b50-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b6b50-140">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="b6b50-141">İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="b6b50-142">Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="b6b50-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="b6b50-143">32 KB</span></span> | <span data-ttu-id="b6b50-144">Tek bir gelen hub iletisinin en büyük boyutu.</span><span class="sxs-lookup"><span data-stu-id="b6b50-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="b6b50-145">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-145">Option</span></span> | <span data-ttu-id="b6b50-146">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-146">Default Value</span></span> | <span data-ttu-id="b6b50-147">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b6b50-148">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-148">30 seconds</span></span> | <span data-ttu-id="b6b50-149">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b6b50-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b6b50-150">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b6b50-151">Önerilen değer Double `KeepAliveInterval` değeridir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b6b50-152">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-152">15 seconds</span></span> | <span data-ttu-id="b6b50-153">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b6b50-154">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-155">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b6b50-156">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-156">15 seconds</span></span> | <span data-ttu-id="b6b50-157">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b6b50-158">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b6b50-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b6b50-159">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b6b50-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b6b50-160">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b6b50-160">All installed protocols</span></span> | <span data-ttu-id="b6b50-161">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b6b50-161">Protocols supported by this hub.</span></span> <span data-ttu-id="b6b50-162">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b6b50-163">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b6b50-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b6b50-164">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b6b50-165">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-165">Option</span></span> | <span data-ttu-id="b6b50-166">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-166">Default Value</span></span> | <span data-ttu-id="b6b50-167">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="b6b50-168">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-168">15 seconds</span></span> | <span data-ttu-id="b6b50-169">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b6b50-170">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-171">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b6b50-172">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-172">15 seconds</span></span> | <span data-ttu-id="b6b50-173">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b6b50-174">Değişiklik yaparken `ServerTimeout` / , istemcideki ayarı`serverTimeoutInMilliseconds` değiştirin. `KeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b6b50-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b6b50-175">Önerilen `ServerTimeout` / değerDouble`KeepAliveInterval` değeridir. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="b6b50-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b6b50-176">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b6b50-176">All installed protocols</span></span> | <span data-ttu-id="b6b50-177">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b6b50-177">Protocols supported by this hub.</span></span> <span data-ttu-id="b6b50-178">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b6b50-179">İse `true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b6b50-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b6b50-180">Varsayılan `false`olarak, bu özel durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="b6b50-181">Seçenekler, `AddSignalR` içindeki `Startup.ConfigureServices`çağrıya bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="b6b50-182">Tek bir hub için Seçenekler ' de `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve kullanılarak <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="b6b50-183">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b6b50-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6b50-184">Aktarımlar `HttpConnectionDispatcherOptions` ve bellek arabellek yönetimiyle ilgili gelişmiş ayarları yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b6b50-185">Bu seçenekler, ' de `Startup.Configure` [maphub\<T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="b6b50-186">Aktarımlar `HttpConnectionDispatcherOptions` ve bellek arabellek yönetimiyle ilgili gelişmiş ayarları yapılandırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b6b50-187">Bu seçenekler, ' de `Startup.Configure` [maphub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="b6b50-188">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b6b50-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b6b50-189">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-189">Option</span></span> | <span data-ttu-id="b6b50-190">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-190">Default Value</span></span> | <span data-ttu-id="b6b50-191">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b6b50-192">32 KB</span><span class="sxs-lookup"><span data-stu-id="b6b50-192">32 KB</span></span> | <span data-ttu-id="b6b50-193">İstemci tarafından, geri basıncı uygulamadan önce sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="b6b50-194">Bu değeri artırmak, sunucunun geri basınç uygulamadan daha büyük iletileri daha hızlı almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b6b50-195">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b6b50-196">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="b6b50-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b6b50-197">32 KB</span><span class="sxs-lookup"><span data-stu-id="b6b50-197">32 KB</span></span> | <span data-ttu-id="b6b50-198">Uygulama tarafından, geri basıncını gözlemlenmadan önce sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="b6b50-199">Bu değeri artırmak, sunucunun geri basınç beklemeden daha büyük iletileri daha hızlı arabelleğe almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b6b50-200">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="b6b50-200">All Transports are enabled.</span></span> | <span data-ttu-id="b6b50-201">Bir istemcinin bağlanmak için kullanabileceği `HttpTransportType` taşımaları kısıtlayabilecek bir bit bayrakları değeri numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="b6b50-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b6b50-202">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-202">See below.</span></span> | <span data-ttu-id="b6b50-203">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b6b50-204">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-204">See below.</span></span> | <span data-ttu-id="b6b50-205">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="b6b50-206">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-206">Option</span></span> | <span data-ttu-id="b6b50-207">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-207">Default Value</span></span> | <span data-ttu-id="b6b50-208">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b6b50-209">32 KB</span><span class="sxs-lookup"><span data-stu-id="b6b50-209">32 KB</span></span> | <span data-ttu-id="b6b50-210">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="b6b50-211">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b6b50-212">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b6b50-213">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="b6b50-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b6b50-214">32 KB</span><span class="sxs-lookup"><span data-stu-id="b6b50-214">32 KB</span></span> | <span data-ttu-id="b6b50-215">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="b6b50-216">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b6b50-217">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="b6b50-217">All Transports are enabled.</span></span> | <span data-ttu-id="b6b50-218">Bir istemcinin bağlanmak için kullanabileceği `HttpTransportType` taşımaları kısıtlayabilecek bir bit bayrakları değeri numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="b6b50-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b6b50-219">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-219">See below.</span></span> | <span data-ttu-id="b6b50-220">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b6b50-221">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-221">See below.</span></span> | <span data-ttu-id="b6b50-222">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="b6b50-223">Uzun yoklama taşıması, `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="b6b50-224">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-224">Option</span></span> | <span data-ttu-id="b6b50-225">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-225">Default Value</span></span> | <span data-ttu-id="b6b50-226">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="b6b50-227">90 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-227">90 seconds</span></span> | <span data-ttu-id="b6b50-228">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="b6b50-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="b6b50-229">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="b6b50-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="b6b50-230">WebSocket taşıması, `WebSockets` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="b6b50-231">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-231">Option</span></span> | <span data-ttu-id="b6b50-232">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-232">Default Value</span></span> | <span data-ttu-id="b6b50-233">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="b6b50-234">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-234">5 seconds</span></span> | <span data-ttu-id="b6b50-235">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="b6b50-236">`Sec-WebSocket-Protocol` Üstbilgiyi özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b6b50-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="b6b50-237">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="b6b50-238">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-238">Configure client options</span></span>

<span data-ttu-id="b6b50-239">İstemci seçenekleri `HubConnectionBuilder` tür üzerinde yapılandırılabilir (.net ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="b6b50-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="b6b50-240">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıf, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisi de içerir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="b6b50-241">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-241">Configure logging</span></span>

<span data-ttu-id="b6b50-242">Günlüğe kaydetme, .net istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="b6b50-243">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="b6b50-244">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b6b50-245">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="b6b50-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="b6b50-246">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="b6b50-247">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="b6b50-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="b6b50-248">`AddConsole` Uzantı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="b6b50-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="b6b50-249">JavaScript istemcisinde, benzer `configureLogging` bir yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="b6b50-250">Üretilecek günlük `LogLevel` iletilerinin en düşük düzeyini belirten bir değer girin.</span><span class="sxs-lookup"><span data-stu-id="b6b50-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="b6b50-251">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6b50-252">Bir değer yerine, bir günlük düzeyi adını temsil eden `string` bir değer de sağlayabilirsiniz. `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="b6b50-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="b6b50-253">Bu, `LogLevel` sabitlere erişiminizin olmadığı ortamlarda SignalR günlüğünü yapılandırırken kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="b6b50-254">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-254">The following table lists the available log levels.</span></span> <span data-ttu-id="b6b50-255">Sağladığınız `configureLogging` değer, günlüğe kaydedilecek **Minimum** günlük düzeyini belirler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="b6b50-256">Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="b6b50-257">Dize</span><span class="sxs-lookup"><span data-stu-id="b6b50-257">String</span></span>                      | <span data-ttu-id="b6b50-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="b6b50-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="b6b50-259">`info`**ya da**`information`</span><span class="sxs-lookup"><span data-stu-id="b6b50-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="b6b50-260">`warn`**ya da**`warning`</span><span class="sxs-lookup"><span data-stu-id="b6b50-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b6b50-261">Günlüğe kaydetmeyi tamamen devre dışı bırakmak `signalR.LogLevel.None` için `configureLogging` yönteminde belirtin.</span><span class="sxs-lookup"><span data-stu-id="b6b50-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b6b50-262">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="b6b50-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="b6b50-263">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="b6b50-264">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="b6b50-265">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="b6b50-266">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="b6b50-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="b6b50-267">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="b6b50-268">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-268">Configure allowed transports</span></span>

<span data-ttu-id="b6b50-269">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrıda (`withUrl` JavaScript 'te) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="b6b50-270">Bir bit düzeyinde OR değeri `HttpTransportType` , istemciyi yalnızca belirtilen aktarımları kullanacak şekilde kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="b6b50-271">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-271">All transports are enabled by default.</span></span>

<span data-ttu-id="b6b50-272">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="b6b50-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="b6b50-273">JavaScript istemcisinde aktarımlar, için `transport` `withUrl`belirtilen seçenekler nesnesindeki alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b6b50-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b6b50-274">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="b6b50-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="b6b50-275">Java istemcisinde, taşıma `withTransport` yöntemi `HttpHubConnectionBuilder`ile birlikte seçilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="b6b50-276">Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="b6b50-277">SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b6b50-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="b6b50-278">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-278">Configure bearer authentication</span></span>

<span data-ttu-id="b6b50-279">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim `AccessTokenProvider` belirtecini döndüren`accessTokenFactory` bir işlevi belirtmek için (JavaScript 'te) seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="b6b50-280">.Net istemcisinde, bu erişim belirteci http "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (bir `Authorization` `Bearer`türü olan üst bilgi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="b6b50-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="b6b50-281">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="b6b50-282">Bu durumlarda, erişim belirteci bir sorgu dizesi değeri `access_token`olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="b6b50-283">.Net istemcisinde `AccessTokenProvider` seçeneği, içindeki `WithUrl`seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="b6b50-284">JavaScript istemcisinde erişim belirteci, içindeki `accessTokenFactory` `withUrl`seçenekler nesnesindeki alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b6b50-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="b6b50-285">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b6b50-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="b6b50-286">[Rxjava](https://github.com/ReactiveX/RxJava) [tek\<dize >](https://reactivex.io/documentation/single.html)sağlamak için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="b6b50-287">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b6b50-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="b6b50-288">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b6b50-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="b6b50-289">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b6b50-290">.NET</span><span class="sxs-lookup"><span data-stu-id="b6b50-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b6b50-291">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-291">Option</span></span> | <span data-ttu-id="b6b50-292">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-292">Default value</span></span> | <span data-ttu-id="b6b50-293">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b6b50-294">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-295">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-295">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-296">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="b6b50-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b6b50-297">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-298">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b6b50-299">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-299">15 seconds</span></span> | <span data-ttu-id="b6b50-300">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="b6b50-301">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="b6b50-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b6b50-302">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-303">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b6b50-304">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-304">15 seconds</span></span> | <span data-ttu-id="b6b50-305">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b6b50-306">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b6b50-307">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b6b50-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="b6b50-308">.Net istemcisinde zaman aşımı değerleri değer olarak `TimeSpan` belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b6b50-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6b50-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b6b50-310">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-310">Option</span></span> | <span data-ttu-id="b6b50-311">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-311">Default value</span></span> | <span data-ttu-id="b6b50-312">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b6b50-313">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-314">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-314">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-315">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onclose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b6b50-316">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-317">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="b6b50-318">15 saniye (15.000 'ları harcanan)</span><span class="sxs-lookup"><span data-stu-id="b6b50-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="b6b50-319">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b6b50-320">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b6b50-321">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b6b50-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b6b50-322">Java</span><span class="sxs-lookup"><span data-stu-id="b6b50-322">Java</span></span>](#tab/java)

| <span data-ttu-id="b6b50-323">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-323">Option</span></span> | <span data-ttu-id="b6b50-324">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-324">Default value</span></span> | <span data-ttu-id="b6b50-325">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b6b50-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b6b50-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b6b50-327">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-328">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-328">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-329">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b6b50-330">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-331">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b6b50-332">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-332">15 seconds</span></span> | <span data-ttu-id="b6b50-333">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="b6b50-334">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b6b50-335">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-336">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="b6b50-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b6b50-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="b6b50-338">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-339">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b6b50-340">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b6b50-341">İstemci, sunucuda `ClientTimeoutInterval` küme içinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b6b50-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b6b50-342">.NET</span><span class="sxs-lookup"><span data-stu-id="b6b50-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b6b50-343">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-343">Option</span></span> | <span data-ttu-id="b6b50-344">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-344">Default value</span></span> | <span data-ttu-id="b6b50-345">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b6b50-346">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-347">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-347">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-348">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="b6b50-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b6b50-349">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-350">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b6b50-351">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-351">15 seconds</span></span> | <span data-ttu-id="b6b50-352">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="b6b50-353">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayı tetikler (`onclose` JavaScript 'te).</span><span class="sxs-lookup"><span data-stu-id="b6b50-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b6b50-354">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-355">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="b6b50-356">.Net istemcisinde zaman aşımı değerleri değer olarak `TimeSpan` belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b6b50-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b6b50-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6b50-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b6b50-358">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-358">Option</span></span> | <span data-ttu-id="b6b50-359">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-359">Default value</span></span> | <span data-ttu-id="b6b50-360">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b6b50-361">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-362">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-362">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-363">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onclose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b6b50-364">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-365">Önerilen değer, ping için en az iki sunucu `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b6b50-366">Java</span><span class="sxs-lookup"><span data-stu-id="b6b50-366">Java</span></span>](#tab/java)

| <span data-ttu-id="b6b50-367">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b6b50-367">Option</span></span> | <span data-ttu-id="b6b50-368">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-368">Default value</span></span> | <span data-ttu-id="b6b50-369">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b6b50-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b6b50-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b6b50-371">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b6b50-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b6b50-372">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-372">Timeout for server activity.</span></span> <span data-ttu-id="b6b50-373">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesti ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b6b50-374">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b6b50-375">Önerilen değer, ping bir sürenin gelmesi için en az iki `KeepAliveInterval` değerin değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b6b50-376">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-376">15 seconds</span></span> | <span data-ttu-id="b6b50-377">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b6b50-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="b6b50-378">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b6b50-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b6b50-379">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b6b50-380">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b6b50-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="b6b50-381">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b6b50-381">Configure additional options</span></span>

<span data-ttu-id="b6b50-382">Java istemcisinde `WithUrl` `HttpHubConnectionBuilder` içindeki çeşitli yapılandırma API 'lerinde `HubConnectionBuilder` veya`withUrl` üzerinde bulunan (JavaScript 'te) yönteminde ek seçenekler yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b6b50-383">.NET</span><span class="sxs-lookup"><span data-stu-id="b6b50-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b6b50-384">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="b6b50-384">.NET Option</span></span> |  <span data-ttu-id="b6b50-385">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-385">Default value</span></span> | <span data-ttu-id="b6b50-386">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="b6b50-387">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b6b50-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="b6b50-388">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b6b50-389">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b6b50-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b6b50-390">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b6b50-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="b6b50-391">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b6b50-391">Empty</span></span> | <span data-ttu-id="b6b50-392">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b6b50-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="b6b50-393">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b6b50-393">Empty</span></span> | <span data-ttu-id="b6b50-394">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b6b50-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="b6b50-395">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b6b50-395">Empty</span></span> | <span data-ttu-id="b6b50-396">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="b6b50-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="b6b50-397">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b6b50-397">5 seconds</span></span> | <span data-ttu-id="b6b50-398">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b6b50-398">WebSockets only.</span></span> <span data-ttu-id="b6b50-399">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="b6b50-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="b6b50-400">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="b6b50-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="b6b50-401">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b6b50-401">Empty</span></span> | <span data-ttu-id="b6b50-402">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b6b50-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="b6b50-403">HTTP istekleri göndermek için `HttpMessageHandler` kullanılan yapılandırmak veya değiştirmek için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b6b50-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="b6b50-404">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="b6b50-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="b6b50-405">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="b6b50-406">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni `HttpMessageHandler` bir örnek döndürün.</span><span class="sxs-lookup"><span data-stu-id="b6b50-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="b6b50-407">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="b6b50-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="b6b50-408">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="b6b50-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="b6b50-409">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="b6b50-410">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="b6b50-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="b6b50-411">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b6b50-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="b6b50-412">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="b6b50-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b6b50-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6b50-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b6b50-414">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="b6b50-414">JavaScript Option</span></span> | <span data-ttu-id="b6b50-415">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-415">Default Value</span></span> | <span data-ttu-id="b6b50-416">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="b6b50-417">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b6b50-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="b6b50-418">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b6b50-419">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b6b50-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b6b50-420">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b6b50-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b6b50-421">Java</span><span class="sxs-lookup"><span data-stu-id="b6b50-421">Java</span></span>](#tab/java)

| <span data-ttu-id="b6b50-422">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="b6b50-422">Java Option</span></span> | <span data-ttu-id="b6b50-423">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b6b50-423">Default Value</span></span> | <span data-ttu-id="b6b50-424">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b6b50-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="b6b50-425">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b6b50-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="b6b50-426">Anlaşma adımını atlamak `true` için bunu olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b6b50-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b6b50-427">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b6b50-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b6b50-428">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b6b50-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="b6b50-429">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="b6b50-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="b6b50-430">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b6b50-430">Empty</span></span> | <span data-ttu-id="b6b50-431">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b6b50-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="b6b50-432">.NET Istemcisinde bu seçenekler, aşağıdakileri yapmak için `WithUrl`belirtilen seçenekler temsilcisinden değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="b6b50-433">JavaScript Istemcisinde, bu seçenekler şu şekilde `withUrl`sağlanmış bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="b6b50-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="b6b50-434">Java istemcisinde, bu seçenekler, içindeki `HttpHubConnectionBuilder` döndürülen yöntemlerle yapılandırılabilir`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="b6b50-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="b6b50-435">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b6b50-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
