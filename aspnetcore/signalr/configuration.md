---
title: ASP.NET Core SignalR yapılandırma
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/29/2019
uid: signalr/configuration
ms.openlocfilehash: ce970199984cdb8333ed1fd51f744dcda2df9c61
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667615"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="8ce22-103">ASP.NET Core SignalR yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ce22-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="8ce22-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="8ce22-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="8ce22-105">ASP.NET Core SignalR iletileri kodlama için iki protokol destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="8ce22-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="8ce22-106">Her protokolü, serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="8ce22-107">JSON seri hale getirme kullanılarak sunucuda yapılandırılabilir [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenen genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="8ce22-108">`AddJsonProtocol` Aldığında bir temsilci yöntemi alır bir `options` nesne.</span><span class="sxs-lookup"><span data-stu-id="8ce22-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="8ce22-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirilmesi yapılandırmak ve dönüş değerleri için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="8ce22-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="8ce22-110">Bkz: [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="8ce22-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="8ce22-111">Örneğin, "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="8ce22-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="8ce22-112">.NET istemci, aynı `AddJsonProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="8ce22-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="8ce22-113">`Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yönteminin çözmek için:</span><span class="sxs-lookup"><span data-stu-id="8ce22-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="8ce22-114">JSON seri hale getirme, şu anda JavaScript istemci yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="8ce22-115">MessagePack seri hale getirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="8ce22-115">MessagePack serialization options</span></span>

<span data-ttu-id="8ce22-116">MessagePack serileştirme için temsilci sağlayarak yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın.</span><span class="sxs-lookup"><span data-stu-id="8ce22-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="8ce22-117">Bkz: [signalr'da MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="8ce22-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="8ce22-118">MessagePack serileştirme JavaScript istemci şu anda yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="8ce22-119">Sunucu seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ce22-119">Configure server options</span></span>

<span data-ttu-id="8ce22-120">Aşağıdaki tabloda, SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="8ce22-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="8ce22-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="8ce22-121">Option</span></span> | <span data-ttu-id="8ce22-122">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-122">Default Value</span></span> | <span data-ttu-id="8ce22-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="8ce22-124">30 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-124">30 seconds</span></span> | <span data-ttu-id="8ce22-125">Sunucunun istemci dikkate alacaktır (tutma dahil) bir ileti bu aralıkta yer aldığı edilmemiş olursa bağlantısı kesildi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="8ce22-126">Aslında, nasıl bu uygulandığını nedeniyle, bağlantısız işaretlenmesini istemcisi için bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="8ce22-127">Önerilen değer çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="8ce22-128">15 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-128">15 seconds</span></span> | <span data-ttu-id="8ce22-129">İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="8ce22-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="8ce22-130">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8ce22-131">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="8ce22-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="8ce22-132">15 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-132">15 seconds</span></span> | <span data-ttu-id="8ce22-133">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="8ce22-134">Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama.</span><span class="sxs-lookup"><span data-stu-id="8ce22-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="8ce22-135">Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="8ce22-136">Yüklü tüm protokolleri</span><span class="sxs-lookup"><span data-stu-id="8ce22-136">All installed protocols</span></span> | <span data-ttu-id="8ce22-137">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="8ce22-137">Protocols supported by this hub.</span></span> <span data-ttu-id="8ce22-138">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="8ce22-139">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8ce22-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="8ce22-140">Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="8ce22-141">Seçenekler yapılandırılabilir tüm hub'ları için bir seçenekler temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8ce22-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="8ce22-142">Tek bir hub seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="8ce22-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="8ce22-143">Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="8ce22-143">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="8ce22-144">Bu seçenekler bir temsilciye geçirerek yapılandırılan [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="8ce22-144">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="8ce22-145">Seçenek</span><span class="sxs-lookup"><span data-stu-id="8ce22-145">Option</span></span> | <span data-ttu-id="8ce22-146">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-146">Default Value</span></span> | <span data-ttu-id="8ce22-147">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="8ce22-148">32 KB</span><span class="sxs-lookup"><span data-stu-id="8ce22-148">32 KB</span></span> | <span data-ttu-id="8ce22-149">İstemciden alınan bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-149">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="8ce22-150">Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-150">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="8ce22-151">Verileri otomatik olarak toplanan `Authorize` Hub sınıfına uygulanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-151">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="8ce22-152">Listesini [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="8ce22-152">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="8ce22-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="8ce22-153">32 KB</span></span> | <span data-ttu-id="8ce22-154">Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-154">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="8ce22-155">Bu değeri artırmak daha büyük iletiler göndermek sunucuyı sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-155">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="8ce22-156">Tüm aktarımları etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-156">All Transports are enabled.</span></span> | <span data-ttu-id="8ce22-157">Bir bit maskesi, `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlanmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ce22-157">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="8ce22-158">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="8ce22-158">See below.</span></span> | <span data-ttu-id="8ce22-159">Uzun yoklama taşıma imkanı kullanılarak belirli ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="8ce22-159">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="8ce22-160">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="8ce22-160">See below.</span></span> | <span data-ttu-id="8ce22-161">Ek seçenekler belirli WebSockets taşıma imkanı.</span><span class="sxs-lookup"><span data-stu-id="8ce22-161">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="8ce22-162">Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `LongPolling` özelliği:</span><span class="sxs-lookup"><span data-stu-id="8ce22-162">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="8ce22-163">Seçenek</span><span class="sxs-lookup"><span data-stu-id="8ce22-163">Option</span></span> | <span data-ttu-id="8ce22-164">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-164">Default Value</span></span> | <span data-ttu-id="8ce22-165">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="8ce22-166">90 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-166">90 seconds</span></span> | <span data-ttu-id="8ce22-167">En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler.</span><span class="sxs-lookup"><span data-stu-id="8ce22-167">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="8ce22-168">Bu değeri azaltmak daha sık verme yeni bir yoklama isteği göndermek istemci neden olur.</span><span class="sxs-lookup"><span data-stu-id="8ce22-168">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="8ce22-169">WebSocket taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `WebSockets` özelliği:</span><span class="sxs-lookup"><span data-stu-id="8ce22-169">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="8ce22-170">Seçenek</span><span class="sxs-lookup"><span data-stu-id="8ce22-170">Option</span></span> | <span data-ttu-id="8ce22-171">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-171">Default Value</span></span> | <span data-ttu-id="8ce22-172">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-172">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="8ce22-173">5 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-173">5 seconds</span></span> | <span data-ttu-id="8ce22-174">Bu zaman aralığı içinde kapatmak istemci başarısız olursa sunucunun kapatıldıktan sonra bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-174">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="8ce22-175">Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` üst bilgisi için özel bir değer.</span><span class="sxs-lookup"><span data-stu-id="8ce22-175">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="8ce22-176">Temsilci, giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-176">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="8ce22-177">İstemci seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8ce22-177">Configure client options</span></span>

<span data-ttu-id="8ce22-178">İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (kullanılabilir), hem .NET hem de JavaScript istemcileri de itibariyle `HubConnection` kendisi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-178">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="8ce22-179">Günlük tutmayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ce22-179">Configure logging</span></span>

<span data-ttu-id="8ce22-180">Günlük kaydı kullanarak .NET istemci yapılandırılmış `ConfigureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-180">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="8ce22-181">Günlüğe kaydetme sağlayıcıları ve filtreleri sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-181">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="8ce22-182">Bkz: [ASP.NET Core günlüğü](xref:fundamentals/logging/index) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="8ce22-182">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="8ce22-183">Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-183">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="8ce22-184">Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgelere bölümü.</span><span class="sxs-lookup"><span data-stu-id="8ce22-184">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="8ce22-185">Örneğin, konsol günlüğe kaydetmeyi etkinleştirmek için yükleme `Microsoft.Extensions.Logging.Console` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-185">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="8ce22-186">Çağrı `AddConsole` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8ce22-186">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="8ce22-187">Benzer bir JavaScript istemci `configureLogging` yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-187">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="8ce22-188">Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyini belirten değer.</span><span class="sxs-lookup"><span data-stu-id="8ce22-188">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="8ce22-189">Tarayıcı konsol penceresine günlüklerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-189">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="8ce22-190">Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-190">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="8ce22-191">JavaScript istemci için kullanılabilen günlük düzeyleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-191">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="8ce22-192">Günlük düzeyi şu değerlerden birini ayarlamak, iletileri günlüğe kaydedilmesini sağlar **veya yukarıdaki** düzeyi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-192">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="8ce22-193">Düzey</span><span class="sxs-lookup"><span data-stu-id="8ce22-193">Level</span></span> | <span data-ttu-id="8ce22-194">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-194">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="8ce22-195">Günlüğe ileti kaydedilmedi.</span><span class="sxs-lookup"><span data-stu-id="8ce22-195">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="8ce22-196">Bir uygulamanın tamamında hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-196">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="8ce22-197">Geçerli işlem bir hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-197">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="8ce22-198">Önemli olmayan bir sorunu işaret eden iletileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-198">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="8ce22-199">Bilgilendirme iletileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-199">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="8ce22-200">Tanılama iletileri hata ayıklama için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-200">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="8ce22-201">Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-201">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="8ce22-202">İzin verilen taşımalar yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8ce22-202">Configure allowed transports</span></span>

<span data-ttu-id="8ce22-203">SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağırın (`withUrl` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="8ce22-203">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="8ce22-204">Bir bit düzeyinde OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-204">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="8ce22-205">Tüm aktarımları varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-205">All transports are enabled by default.</span></span>

<span data-ttu-id="8ce22-206">Örneğin, Server-Sent olayları taşıma devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:</span><span class="sxs-lookup"><span data-stu-id="8ce22-206">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="8ce22-207">JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-207">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="8ce22-208">Taşıyıcı kimlik doğrulaması yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ce22-208">Configure bearer authentication</span></span>

<span data-ttu-id="8ce22-209">SignalR istekleri ile birlikte kimlik doğrulama verilerini sağlamak için kullanmayı `AccessTokenProvider` seçeneği (`accessTokenFactory` javascript'teki) istenen erişim belirteci döndüren bir işlev belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="8ce22-209">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="8ce22-210">.NET istemci, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` başlığı türünde `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="8ce22-210">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="8ce22-211">JavaScript istemci erişim belirtecini bir taşıyıcı belirteç kullanılan **dışında** tarayıcı API'leri kısıtlama (özellikle de Server-Sent olayları ve WebSockets istekleri) üst bilgileri uygulama özelliği burada birkaç durumda.</span><span class="sxs-lookup"><span data-stu-id="8ce22-211">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="8ce22-212">Bu durumlarda, erişim belirtecini bir sorgu dizesi değeri sağlanan `access_token`.</span><span class="sxs-lookup"><span data-stu-id="8ce22-212">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="8ce22-213">.NET istemci `AccessTokenProvider` seçenekleri Temsilcide kullanılarak seçeneği belirtilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-213">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="8ce22-214">JavaScript istemcisinde ayarlayarak erişim belirteci yapılandırılmıştır `accessTokenFactory` seçenekleri nesnesinde ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-214">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="8ce22-215">Zaman aşımı ve tutma seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="8ce22-215">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="8ce22-216">Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekler bulunur `HubConnection` nesnenin kendisini:</span><span class="sxs-lookup"><span data-stu-id="8ce22-216">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="8ce22-217">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="8ce22-217">.NET Option</span></span> | <span data-ttu-id="8ce22-218">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="8ce22-218">JavaScript Option</span></span> | <span data-ttu-id="8ce22-219">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-219">Default Value</span></span> | <span data-ttu-id="8ce22-220">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-220">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="8ce22-221">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="8ce22-221">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="8ce22-222">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="8ce22-222">Timeout for server activity.</span></span> <span data-ttu-id="8ce22-223">Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="8ce22-223">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8ce22-224">Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı.</span><span class="sxs-lookup"><span data-stu-id="8ce22-224">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="8ce22-225">Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer.</span><span class="sxs-lookup"><span data-stu-id="8ce22-225">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="8ce22-226">Yapılandırılamaz</span><span class="sxs-lookup"><span data-stu-id="8ce22-226">Not configurable</span></span> | <span data-ttu-id="8ce22-227">15 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-227">15 seconds</span></span> | <span data-ttu-id="8ce22-228">İlk sunucu el sıkışma için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="8ce22-228">Timeout for initial server handshake.</span></span> <span data-ttu-id="8ce22-229">Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="8ce22-229">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="8ce22-230">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-230">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="8ce22-231">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="8ce22-231">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="8ce22-232">.NET istemci zaman aşımı değerlerini olarak belirtilen `TimeSpan` değerleri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-232">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="8ce22-233">JavaScript istemci zaman aşımı değerlerini, milisaniye cinsinden süre belirten bir sayı olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-233">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="8ce22-234">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ce22-234">Configure additional options</span></span>

<span data-ttu-id="8ce22-235">Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) metodunda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-235">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="8ce22-236">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="8ce22-236">.NET Option</span></span> | <span data-ttu-id="8ce22-237">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="8ce22-237">JavaScript Option</span></span> | <span data-ttu-id="8ce22-238">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="8ce22-238">Default Value</span></span> | <span data-ttu-id="8ce22-239">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8ce22-239">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="8ce22-240">HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="8ce22-240">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="8ce22-241">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="8ce22-241">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="8ce22-242">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="8ce22-242">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="8ce22-243">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="8ce22-243">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="8ce22-244">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-244">Not configurable \*</span></span> | <span data-ttu-id="8ce22-245">boş</span><span class="sxs-lookup"><span data-stu-id="8ce22-245">Empty</span></span> | <span data-ttu-id="8ce22-246">TLS sertifikalarını isteklerinde kimlik doğrulamak üzere göndermek için bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="8ce22-246">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="8ce22-247">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-247">Not configurable \*</span></span> | <span data-ttu-id="8ce22-248">boş</span><span class="sxs-lookup"><span data-stu-id="8ce22-248">Empty</span></span> | <span data-ttu-id="8ce22-249">Her HTTP isteği göndermek için HTTP tanımlama bilgileri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="8ce22-249">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="8ce22-250">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-250">Not configurable \*</span></span> | <span data-ttu-id="8ce22-251">boş</span><span class="sxs-lookup"><span data-stu-id="8ce22-251">Empty</span></span> | <span data-ttu-id="8ce22-252">Her HTTP isteği göndermek için kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="8ce22-252">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="8ce22-253">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-253">Not configurable \*</span></span> | <span data-ttu-id="8ce22-254">5 saniye</span><span class="sxs-lookup"><span data-stu-id="8ce22-254">5 seconds</span></span> | <span data-ttu-id="8ce22-255">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="8ce22-255">WebSockets only.</span></span> <span data-ttu-id="8ce22-256">En uzun süreyi sunucusunun Kapat isteği onaylamak Kapanıştan sonra istemci bekler.</span><span class="sxs-lookup"><span data-stu-id="8ce22-256">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="8ce22-257">Bu süre içinde sunucu kapatma bildirimi değil, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="8ce22-257">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="8ce22-258">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-258">Not configurable \*</span></span> | <span data-ttu-id="8ce22-259">boş</span><span class="sxs-lookup"><span data-stu-id="8ce22-259">Empty</span></span> | <span data-ttu-id="8ce22-260">Her HTTP isteği göndermek için ek HTTP üstbilgileri sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8ce22-260">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="8ce22-261">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-261">Not configurable \*</span></span> | `null` | <span data-ttu-id="8ce22-262">Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-262">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="8ce22-263">WebSocket bağlantılarını için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="8ce22-263">Not used for WebSocket connections.</span></span> <span data-ttu-id="8ce22-264">Bu temsilci, bir null olmayan değer döndürmelidir ve varsayılan değer bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="8ce22-264">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="8ce22-265">Bu varsayılan ayarları değiştirmek ve onu döndürür ya da yeni bir dönüş `HttpMessageHandler` örneği.</span><span class="sxs-lookup"><span data-stu-id="8ce22-265">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="8ce22-266">**Aksi takdirde işleyici değiştirerek yaptığınızda, sağlanan işleyicisinden tutmak istediğiniz ayarları kopyaladığınızdan emin (örneğin, tanımlama bilgileri ve üst) yapılandırılmış seçenekler için yeni işleyici uygulanmayacak.**</span><span class="sxs-lookup"><span data-stu-id="8ce22-266">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="8ce22-267">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-267">Not configurable \*</span></span> | `null` | <span data-ttu-id="8ce22-268">HTTP istekleri gönderirken HTTP proxy.</span><span class="sxs-lookup"><span data-stu-id="8ce22-268">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="8ce22-269">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-269">Not configurable \*</span></span> | `false` | <span data-ttu-id="8ce22-270">HTTP ve Websocket'istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8ce22-270">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="8ce22-271">Bu, Windows kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ce22-271">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="8ce22-272">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="8ce22-272">Not configurable \*</span></span> | `null` | <span data-ttu-id="8ce22-273">Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="8ce22-273">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="8ce22-274">Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-274">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="8ce22-275">Bir yıldız işareti (\*) ile seçenekleri JavaScript istemci tarayıcısında API sınırlamaları nedeniyle yapılandırılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8ce22-275">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="8ce22-276">.NET istemci, bu seçenekler için sağlanan seçenekleri temsilci tarafından değiştirilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-276">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="8ce22-277">JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesi içinde sağlanabilir `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="8ce22-277">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="8ce22-278">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8ce22-278">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
