---
title: ASP.NET Core SignalR yapılandırma
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 662565e537fa0eb13ed80e558949740739a63558
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500388"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="b0121-103">ASP.NET Core SignalR yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0121-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="b0121-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0121-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="b0121-105">ASP.NET Core SignalR iletileri kodlama için iki protokol destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="b0121-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="b0121-106">Her protokolü, serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="b0121-107">JSON seri hale getirme kullanılarak sunucuda yapılandırılabilir [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenen genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0121-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b0121-108">`AddJsonProtocol` Aldığında bir temsilci yöntemi alır bir `options` nesne.</span><span class="sxs-lookup"><span data-stu-id="b0121-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b0121-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirilmesi yapılandırmak ve dönüş değerleri için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="b0121-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b0121-110">Bkz: [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="b0121-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="b0121-111">Örneğin, "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b0121-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="b0121-112">.NET istemci, aynı `AddJsonProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b0121-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b0121-113">`Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yönteminin çözmek için:</span><span class="sxs-lookup"><span data-stu-id="b0121-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="b0121-114">JSON seri hale getirme, şu anda JavaScript istemci yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b0121-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="b0121-115">MessagePack seri hale getirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0121-115">MessagePack serialization options</span></span>

<span data-ttu-id="b0121-116">MessagePack serileştirme için temsilci sağlayarak yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın.</span><span class="sxs-lookup"><span data-stu-id="b0121-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="b0121-117">Bkz: [signalr'da MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="b0121-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="b0121-118">MessagePack serileştirme JavaScript istemci şu anda yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b0121-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="b0121-119">Sunucu seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0121-119">Configure server options</span></span>

<span data-ttu-id="b0121-120">Aşağıdaki tabloda, SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b0121-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b0121-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-121">Option</span></span> | <span data-ttu-id="b0121-122">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-122">Default Value</span></span> | <span data-ttu-id="b0121-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b0121-124">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-124">30 seconds</span></span> | <span data-ttu-id="b0121-125">Sunucunun istemci dikkate alacaktır (tutma dahil) bir ileti bu aralıkta yer aldığı edilmemiş olursa bağlantısı kesildi.</span><span class="sxs-lookup"><span data-stu-id="b0121-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b0121-126">Aslında, nasıl bu uygulandığını nedeniyle, bağlantısız işaretlenmesini istemcisi için bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b0121-127">Önerilen değer çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="b0121-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b0121-128">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-128">15 seconds</span></span> | <span data-ttu-id="b0121-129">İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="b0121-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0121-130">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0121-131">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0121-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0121-132">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-132">15 seconds</span></span> | <span data-ttu-id="b0121-133">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0121-134">Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama.</span><span class="sxs-lookup"><span data-stu-id="b0121-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0121-135">Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="b0121-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0121-136">Yüklü tüm protokolleri</span><span class="sxs-lookup"><span data-stu-id="b0121-136">All installed protocols</span></span> | <span data-ttu-id="b0121-137">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0121-137">Protocols supported by this hub.</span></span> <span data-ttu-id="b0121-138">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0121-139">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0121-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0121-140">Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="b0121-141">İstemci için arabelleğe alınan öğelerin sayısı akışları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b0121-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="b0121-142">Bu sınıra ulaşıldığında, çağrıları işlenmesini akışı öğeleri sunucu işler kadar engellenir.</span><span class="sxs-lookup"><span data-stu-id="b0121-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="b0121-143">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-143">Option</span></span> | <span data-ttu-id="b0121-144">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-144">Default Value</span></span> | <span data-ttu-id="b0121-145">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b0121-146">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-146">30 seconds</span></span> | <span data-ttu-id="b0121-147">Sunucunun istemci dikkate alacaktır (tutma dahil) bir ileti bu aralıkta yer aldığı edilmemiş olursa bağlantısı kesildi.</span><span class="sxs-lookup"><span data-stu-id="b0121-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b0121-148">Aslında, nasıl bu uygulandığını nedeniyle, bağlantısız işaretlenmesini istemcisi için bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b0121-149">Önerilen değer çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="b0121-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b0121-150">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-150">15 seconds</span></span> | <span data-ttu-id="b0121-151">İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="b0121-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0121-152">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0121-153">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0121-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0121-154">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-154">15 seconds</span></span> | <span data-ttu-id="b0121-155">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0121-156">Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama.</span><span class="sxs-lookup"><span data-stu-id="b0121-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0121-157">Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="b0121-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0121-158">Yüklü tüm protokolleri</span><span class="sxs-lookup"><span data-stu-id="b0121-158">All installed protocols</span></span> | <span data-ttu-id="b0121-159">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0121-159">Protocols supported by this hub.</span></span> <span data-ttu-id="b0121-160">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0121-161">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0121-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0121-162">Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b0121-163">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-163">Option</span></span> | <span data-ttu-id="b0121-164">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-164">Default Value</span></span> | <span data-ttu-id="b0121-165">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="b0121-166">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-166">15 seconds</span></span> | <span data-ttu-id="b0121-167">İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="b0121-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0121-168">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0121-169">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0121-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0121-170">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-170">15 seconds</span></span> | <span data-ttu-id="b0121-171">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0121-172">Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama.</span><span class="sxs-lookup"><span data-stu-id="b0121-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0121-173">Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="b0121-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0121-174">Yüklü tüm protokolleri</span><span class="sxs-lookup"><span data-stu-id="b0121-174">All installed protocols</span></span> | <span data-ttu-id="b0121-175">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0121-175">Protocols supported by this hub.</span></span> <span data-ttu-id="b0121-176">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0121-177">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0121-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0121-178">Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="b0121-179">Seçenekler yapılandırılabilir tüm hub'ları için bir seçenekler temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b0121-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="b0121-180">Tek bir hub seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="b0121-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="b0121-181">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0121-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="b0121-182">Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="b0121-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b0121-183">Bu seçenekler bir temsilciye geçirerek yapılandırılan [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b0121-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="b0121-184">Aşağıdaki tabloda, ASP.NET Core SignalR Gelişmiş HTTP OPTIONS yapılandırmak için seçenekleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="b0121-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="b0121-185">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-185">Option</span></span> | <span data-ttu-id="b0121-186">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-186">Default Value</span></span> | <span data-ttu-id="b0121-187">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b0121-188">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0121-188">32 KB</span></span> | <span data-ttu-id="b0121-189">İstemciden alınan bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="b0121-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="b0121-190">Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b0121-191">Verileri otomatik olarak toplanan `Authorize` Hub sınıfına uygulanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="b0121-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b0121-192">Listesini [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="b0121-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b0121-193">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0121-193">32 KB</span></span> | <span data-ttu-id="b0121-194">Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="b0121-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="b0121-195">Bu değeri artırmak daha büyük iletiler göndermek sunucuyı sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b0121-196">Tüm aktarımları etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-196">All Transports are enabled.</span></span> | <span data-ttu-id="b0121-197">Bir bit maskesi, `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlanmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0121-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b0121-198">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0121-198">See below.</span></span> | <span data-ttu-id="b0121-199">Uzun yoklama taşıma imkanı kullanılarak belirli ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b0121-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b0121-200">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0121-200">See below.</span></span> | <span data-ttu-id="b0121-201">Ek seçenekler belirli WebSockets taşıma imkanı.</span><span class="sxs-lookup"><span data-stu-id="b0121-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="b0121-202">Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `LongPolling` özelliği:</span><span class="sxs-lookup"><span data-stu-id="b0121-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="b0121-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-203">Option</span></span> | <span data-ttu-id="b0121-204">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-204">Default Value</span></span> | <span data-ttu-id="b0121-205">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="b0121-206">90 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-206">90 seconds</span></span> | <span data-ttu-id="b0121-207">En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler.</span><span class="sxs-lookup"><span data-stu-id="b0121-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="b0121-208">Bu değeri azaltmak daha sık verme yeni bir yoklama isteği göndermek istemci neden olur.</span><span class="sxs-lookup"><span data-stu-id="b0121-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="b0121-209">WebSocket taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `WebSockets` özelliği:</span><span class="sxs-lookup"><span data-stu-id="b0121-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="b0121-210">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-210">Option</span></span> | <span data-ttu-id="b0121-211">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-211">Default Value</span></span> | <span data-ttu-id="b0121-212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="b0121-213">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-213">5 seconds</span></span> | <span data-ttu-id="b0121-214">Bu zaman aralığı içinde kapatmak istemci başarısız olursa sunucunun kapatıldıktan sonra bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0121-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="b0121-215">Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` üst bilgisi için özel bir değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="b0121-216">Temsilci, giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="b0121-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="b0121-217">İstemci seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b0121-217">Configure client options</span></span>

<span data-ttu-id="b0121-218">İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (.NET ve JavaScript istemcilerden kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="b0121-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="b0121-219">Ayrıca Java istemcisinde kullanılabilir ancak `HttpHubConnectionBuilder` Oluşturucusu yapılandırma seçeneklerini de itibariyle ne içerdiğini sınıfıdır `HubConnection` kendisi.</span><span class="sxs-lookup"><span data-stu-id="b0121-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="b0121-220">Günlük tutmayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0121-220">Configure logging</span></span>

<span data-ttu-id="b0121-221">Günlük kaydı kullanarak .NET istemci yapılandırılmış `ConfigureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0121-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="b0121-222">Günlüğe kaydetme sağlayıcıları ve filtreleri sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="b0121-223">Bkz: [ASP.NET Core günlüğü](xref:fundamentals/logging/index) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="b0121-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b0121-224">Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0121-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="b0121-225">Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgelere bölümü.</span><span class="sxs-lookup"><span data-stu-id="b0121-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="b0121-226">Örneğin, konsol günlüğe kaydetmeyi etkinleştirmek için yükleme `Microsoft.Extensions.Logging.Console` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b0121-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="b0121-227">Çağrı `AddConsole` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b0121-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="b0121-228">Benzer bir JavaScript istemci `configureLogging` yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="b0121-229">Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyini belirten değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="b0121-230">Tarayıcı konsol penceresine günlüklerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="b0121-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b0121-231">Yerine bir `LogLevel` değerini de sağlayabilirsiniz bir `string` günlük düzeyi adı temsil eden değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="b0121-232">SignalR ortamlarda günlüğü burada yoksa erişiminiz yapılandırırken bu kullanışlıdır `LogLevel` sabitler.</span><span class="sxs-lookup"><span data-stu-id="b0121-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="b0121-233">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listeler.</span><span class="sxs-lookup"><span data-stu-id="b0121-233">The following table lists the available log levels.</span></span> <span data-ttu-id="b0121-234">Sunduğunuz değeri `configureLogging` ayarlar **minimum** günlüğe kaydedilir düzeyi.</span><span class="sxs-lookup"><span data-stu-id="b0121-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="b0121-235">Bu düzeyde günlüğe ileti **veya düzeyleri bundan sonra tabloda listelenen**, günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="b0121-236">dize</span><span class="sxs-lookup"><span data-stu-id="b0121-236">string</span></span> | <span data-ttu-id="b0121-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="b0121-237">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="b0121-238">`"info"` **veya** `"information"`</span><span class="sxs-lookup"><span data-stu-id="b0121-238">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="b0121-239">`"warn"` **veya** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="b0121-239">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b0121-240">Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b0121-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b0121-241">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR tanılama belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="b0121-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="b0121-242">SignalR Java istemcinin kullandığı [SLF4J](https://www.slf4j.org/) günlüğe kaydetme için kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="b0121-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="b0121-243">Seçtiğiniz kendi özel günlük uygulama özel günlük bağımlılık olarak getirerek kullanıcıların Kitaplığı'nın izin veren bir üst düzey günlüğe kaydetme API'si var.</span><span class="sxs-lookup"><span data-stu-id="b0121-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="b0121-244">Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `java.util.logging` SignalR Java istemcisi ile.</span><span class="sxs-lookup"><span data-stu-id="b0121-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="b0121-245">Bağımlılıklarınızı içinde günlüğü yapılandırmazsanız, aşağıdaki uyarı iletisi ile bir varsayılan yok-işlem günlükçü SLF4J yükler:</span><span class="sxs-lookup"><span data-stu-id="b0121-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="b0121-246">Bu güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="b0121-247">İzin verilen taşımalar yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b0121-247">Configure allowed transports</span></span>

<span data-ttu-id="b0121-248">SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağırın (`withUrl` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="b0121-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="b0121-249">Bir bit düzeyinde OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="b0121-250">Tüm aktarımları varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="b0121-250">All transports are enabled by default.</span></span>

<span data-ttu-id="b0121-251">Örneğin, Server-Sent olayları taşıma devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:</span><span class="sxs-lookup"><span data-stu-id="b0121-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="b0121-252">JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b0121-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b0121-253">Bu Java sürümünde istemci websockets'i yalnızca Aktarım ' dir.</span><span class="sxs-lookup"><span data-stu-id="b0121-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="b0121-254">Java istemci ile taşıma seçili `withTransport` metodunda `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b0121-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="b0121-255">Java istemci WebSockets aktarım varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="b0121-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="b0121-256">SignalR Java istemci taşıma geri dönüş henüz desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="b0121-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="b0121-257">Taşıyıcı kimlik doğrulaması yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0121-257">Configure bearer authentication</span></span>

<span data-ttu-id="b0121-258">SignalR istekleri ile birlikte kimlik doğrulama verilerini sağlamak için kullanmayı `AccessTokenProvider` seçeneği (`accessTokenFactory` javascript'teki) istenen erişim belirteci döndüren bir işlev belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="b0121-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="b0121-259">.NET istemci, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` başlığı türünde `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="b0121-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="b0121-260">JavaScript istemci erişim belirtecini bir taşıyıcı belirteç kullanılan **dışında** tarayıcı API'leri kısıtlama (özellikle de Server-Sent olayları ve WebSockets istekleri) üst bilgileri uygulama özelliği burada birkaç durumda.</span><span class="sxs-lookup"><span data-stu-id="b0121-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="b0121-261">Bu durumlarda, erişim belirtecini bir sorgu dizesi değeri sağlanan `access_token`.</span><span class="sxs-lookup"><span data-stu-id="b0121-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="b0121-262">.NET istemci `AccessTokenProvider` seçenekleri Temsilcide kullanılarak seçeneği belirtilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="b0121-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="b0121-263">JavaScript istemcisinde ayarlayarak erişim belirteci yapılandırılmıştır `accessTokenFactory` seçenekleri nesnesinde ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b0121-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="b0121-264">SignalR Java istemci, bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabileceğiniz [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="b0121-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="b0121-265">Kullanım [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) sağlamak için bir [RxJava](https://github.com/ReactiveX/RxJava) [tek\<dizesi >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="b0121-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="b0121-266">Çağrısıyla [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), istemciniz için erişim belirteci üretmek için mantığı yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0121-266">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="b0121-267">Zaman aşımı ve tutma seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b0121-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="b0121-268">Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekler bulunur `HubConnection` nesnenin kendisini:</span><span class="sxs-lookup"><span data-stu-id="b0121-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b0121-269">.NET</span><span class="sxs-lookup"><span data-stu-id="b0121-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b0121-270">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-270">Option</span></span> | <span data-ttu-id="b0121-271">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0121-271">Default value</span></span> | <span data-ttu-id="b0121-272">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b0121-273">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0121-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0121-274">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0121-274">Timeout for server activity.</span></span> <span data-ttu-id="b0121-275">Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="b0121-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0121-276">Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı.</span><span class="sxs-lookup"><span data-stu-id="b0121-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0121-277">Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-277">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b0121-278">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-278">15 seconds</span></span> | <span data-ttu-id="b0121-279">İlk sunucu el sıkışma için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0121-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0121-280">Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="b0121-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0121-281">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0121-282">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0121-282">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="b0121-283">.NET istemci zaman aşımı değerlerini olarak belirtilen `TimeSpan` değerleri.</span><span class="sxs-lookup"><span data-stu-id="b0121-283">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b0121-284">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0121-284">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b0121-285">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-285">Option</span></span> | <span data-ttu-id="b0121-286">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0121-286">Default value</span></span> | <span data-ttu-id="b0121-287">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-287">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b0121-288">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0121-288">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0121-289">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0121-289">Timeout for server activity.</span></span> <span data-ttu-id="b0121-290">Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `onclose` olay.</span><span class="sxs-lookup"><span data-stu-id="b0121-290">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b0121-291">Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı.</span><span class="sxs-lookup"><span data-stu-id="b0121-291">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0121-292">Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-292">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b0121-293">Java</span><span class="sxs-lookup"><span data-stu-id="b0121-293">Java</span></span>](#tab/java)

| <span data-ttu-id="b0121-294">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0121-294">Option</span></span> | <span data-ttu-id="b0121-295">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0121-295">Default value</span></span> | <span data-ttu-id="b0121-296">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-296">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="b0121-297">`getServerTimeout``setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b0121-297">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="b0121-298">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0121-298">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0121-299">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0121-299">Timeout for server activity.</span></span> <span data-ttu-id="b0121-300">Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `onClose` olay.</span><span class="sxs-lookup"><span data-stu-id="b0121-300">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b0121-301">Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı.</span><span class="sxs-lookup"><span data-stu-id="b0121-301">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0121-302">Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer.</span><span class="sxs-lookup"><span data-stu-id="b0121-302">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b0121-303">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-303">15 seconds</span></span> | <span data-ttu-id="b0121-304">İlk sunucu el sıkışma için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0121-304">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0121-305">Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `onClose` olay.</span><span class="sxs-lookup"><span data-stu-id="b0121-305">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b0121-306">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0121-306">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0121-307">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0121-307">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="b0121-308">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0121-308">Configure additional options</span></span>

<span data-ttu-id="b0121-309">Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) metodunda `HubConnectionBuilder` veya çeşitli yapılandırma API'leri üzerinde `HttpHubConnectionBuilder` Java istemci:</span><span class="sxs-lookup"><span data-stu-id="b0121-309">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b0121-310">.NET</span><span class="sxs-lookup"><span data-stu-id="b0121-310">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b0121-311">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0121-311">.NET Option</span></span> |  <span data-ttu-id="b0121-312">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0121-312">Default value</span></span> | <span data-ttu-id="b0121-313">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-313">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="b0121-314">HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0121-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="b0121-315">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="b0121-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0121-316">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="b0121-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0121-317">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0121-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="b0121-318">boş</span><span class="sxs-lookup"><span data-stu-id="b0121-318">Empty</span></span> | <span data-ttu-id="b0121-319">TLS sertifikalarını isteklerinde kimlik doğrulamak üzere göndermek için bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b0121-319">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="b0121-320">boş</span><span class="sxs-lookup"><span data-stu-id="b0121-320">Empty</span></span> | <span data-ttu-id="b0121-321">Her HTTP isteği göndermek için HTTP tanımlama bilgileri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b0121-321">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="b0121-322">boş</span><span class="sxs-lookup"><span data-stu-id="b0121-322">Empty</span></span> | <span data-ttu-id="b0121-323">Her HTTP isteği göndermek için kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="b0121-323">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="b0121-324">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b0121-324">5 seconds</span></span> | <span data-ttu-id="b0121-325">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b0121-325">WebSockets only.</span></span> <span data-ttu-id="b0121-326">En uzun süreyi sunucusunun Kapat isteği onaylamak Kapanıştan sonra istemci bekler.</span><span class="sxs-lookup"><span data-stu-id="b0121-326">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="b0121-327">Bu süre içinde sunucu kapatma bildirimi değil, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="b0121-327">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="b0121-328">boş</span><span class="sxs-lookup"><span data-stu-id="b0121-328">Empty</span></span> | <span data-ttu-id="b0121-329">Her HTTP isteği göndermek için ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b0121-329">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="b0121-330">Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0121-330">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="b0121-331">WebSocket bağlantılarını için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="b0121-331">Not used for WebSocket connections.</span></span> <span data-ttu-id="b0121-332">Bu temsilci, bir null olmayan değer döndürmelidir ve varsayılan değer bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="b0121-332">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="b0121-333">Bu varsayılan ayarları değiştirmek ve onu döndürür ya da yeni bir dönüş `HttpMessageHandler` örneği.</span><span class="sxs-lookup"><span data-stu-id="b0121-333">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="b0121-334">**Aksi takdirde işleyici değiştirerek yaptığınızda, sağlanan işleyicisinden tutmak istediğiniz ayarları kopyaladığınızdan emin (örneğin, tanımlama bilgileri ve üst) yapılandırılmış seçenekler için yeni işleyici uygulanmayacak.**</span><span class="sxs-lookup"><span data-stu-id="b0121-334">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="b0121-335">HTTP istekleri gönderirken HTTP proxy.</span><span class="sxs-lookup"><span data-stu-id="b0121-335">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="b0121-336">HTTP ve Websocket'istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b0121-336">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="b0121-337">Bu, Windows kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0121-337">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="b0121-338">Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b0121-338">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="b0121-339">Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0121-339">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b0121-340">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0121-340">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b0121-341">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0121-341">JavaScript Option</span></span> | <span data-ttu-id="b0121-342">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-342">Default Value</span></span> | <span data-ttu-id="b0121-343">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-343">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="b0121-344">HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0121-344">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="b0121-345">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="b0121-345">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0121-346">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="b0121-346">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0121-347">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0121-347">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b0121-348">Java</span><span class="sxs-lookup"><span data-stu-id="b0121-348">Java</span></span>](#tab/java)

| <span data-ttu-id="b0121-349">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0121-349">Java Option</span></span> | <span data-ttu-id="b0121-350">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="b0121-350">Default Value</span></span> | <span data-ttu-id="b0121-351">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0121-351">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="b0121-352">HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0121-352">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="b0121-353">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="b0121-353">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0121-354">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="b0121-354">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0121-355">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0121-355">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="b0121-356">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="b0121-356">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="b0121-357">boş</span><span class="sxs-lookup"><span data-stu-id="b0121-357">Empty</span></span> | <span data-ttu-id="b0121-358">Her HTTP isteği göndermek için ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b0121-358">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="b0121-359">.NET istemci, bu seçenekler için sağlanan seçenekleri temsilci tarafından değiştirilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="b0121-359">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="b0121-360">JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesi içinde sağlanabilir `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="b0121-360">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="b0121-361">Java istemci, bu seçenekler yöntemleriyle yapılandırılabilir `HttpHubConnectionBuilder` döndürüldüğü `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="b0121-361">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="b0121-362">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0121-362">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
