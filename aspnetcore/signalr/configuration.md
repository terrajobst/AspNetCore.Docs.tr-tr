---
title: ASP.NET Core SignalR yapılandırma
author: tdykstra
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: b7c9c3713faa952c2b5bd142ab4887ccbc120ea2
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373377"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="f5f98-103">ASP.NET Core SignalR yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5f98-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="f5f98-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="f5f98-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="f5f98-105">ASP.NET Core SignalR iletileri kodlama için iki protokollerini destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="f5f98-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="f5f98-106">Her protokolü, serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="f5f98-107">JSON seri hale getirme kullanılarak sunucuda yapılandırılabilir [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenen genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f5f98-108">`AddJsonProtocol` Aldığında bir temsilci yöntemi alır bir `options` nesne.</span><span class="sxs-lookup"><span data-stu-id="f5f98-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="f5f98-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirilmesi yapılandırmak ve dönüş değerleri için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="f5f98-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="f5f98-110">Bkz: [JSON.NET belgeleri](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="f5f98-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="f5f98-111">Örneğin, "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5f98-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="f5f98-112">.NET istemci, aynı `AddJsonProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="f5f98-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="f5f98-113">`Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yönteminin çözmek için:</span><span class="sxs-lookup"><span data-stu-id="f5f98-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
            new DefaultContractResolver();
    });
```

> [!NOTE]
> <span data-ttu-id="f5f98-114">JSON seri hale getirme, şu anda JavaScript istemci yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="f5f98-115">MessagePack seri hale getirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="f5f98-115">MessagePack serialization options</span></span>

<span data-ttu-id="f5f98-116">MessagePack serileştirme için temsilci sağlayarak yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın.</span><span class="sxs-lookup"><span data-stu-id="f5f98-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="f5f98-117">Bkz: [signalr'da MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="f5f98-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="f5f98-118">MessagePack serileştirme JavaScript istemci şu anda yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="f5f98-119">Sunucu seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5f98-119">Configure server options</span></span>

<span data-ttu-id="f5f98-120">Aşağıdaki tabloda, SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="f5f98-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="f5f98-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="f5f98-121">Option</span></span> | <span data-ttu-id="f5f98-122">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-122">Default Value</span></span> | <span data-ttu-id="f5f98-123">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="f5f98-124">15 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-124">15 seconds</span></span> | <span data-ttu-id="f5f98-125">İstemci, bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermediği durumlarda bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="f5f98-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="f5f98-126">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f5f98-127">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f5f98-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="f5f98-128">15 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-128">15 seconds</span></span> | <span data-ttu-id="f5f98-129">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, PING iletisi bağlantıyı açık tutma için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="f5f98-130">Değiştirilirken `KeepAliveInterval`, değiştirme `ServerTimeout` / `serverTimeoutInMilliseconds` istemcide ayarlama.</span><span class="sxs-lookup"><span data-stu-id="f5f98-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="f5f98-131">Önerilen `ServerTimeout` / `serverTimeoutInMilliseconds` değeri çift `KeepAliveInterval` değeri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="f5f98-132">Yüklü tüm protokolleri</span><span class="sxs-lookup"><span data-stu-id="f5f98-132">All installed protocols</span></span> | <span data-ttu-id="f5f98-133">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="f5f98-133">Protocols supported by this hub.</span></span> <span data-ttu-id="f5f98-134">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak belirli protokoller için tek tek hub'ları devre dışı bırakmak için bu listedeki protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="f5f98-135">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f5f98-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="f5f98-136">Varsayılan `false`gibi bu özel durum iletileri, hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="f5f98-137">Seçenekler yapılandırılabilir tüm hub'ları için bir seçenekler temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f5f98-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="f5f98-138">Tek bir hub seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="f5f98-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="f5f98-139">Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="f5f98-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="f5f98-140">Bu seçenekler bir temsilciye geçirerek yapılandırılan [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="f5f98-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="f5f98-141">Seçenek</span><span class="sxs-lookup"><span data-stu-id="f5f98-141">Option</span></span> | <span data-ttu-id="f5f98-142">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-142">Default Value</span></span> | <span data-ttu-id="f5f98-143">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="f5f98-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="f5f98-144">32 KB</span></span> | <span data-ttu-id="f5f98-145">İstemciden alınan bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="f5f98-146">Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="f5f98-147">Verileri otomatik olarak toplanan `Authorize` Hub sınıfına uygulanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="f5f98-148">Listesini [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="f5f98-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="f5f98-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="f5f98-149">32 KB</span></span> | <span data-ttu-id="f5f98-150">Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="f5f98-151">Bu değeri artırmak daha büyük iletiler göndermek sunucuyı sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="f5f98-152">Tüm aktarımları etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-152">All Transports are enabled.</span></span> | <span data-ttu-id="f5f98-153">Bir bit maskesi, `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlanmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5f98-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="f5f98-154">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="f5f98-154">See below.</span></span> | <span data-ttu-id="f5f98-155">Uzun yoklama taşıma imkanı kullanılarak belirli ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="f5f98-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="f5f98-156">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="f5f98-156">See below.</span></span> | <span data-ttu-id="f5f98-157">Ek seçenekler belirli WebSockets taşıma imkanı.</span><span class="sxs-lookup"><span data-stu-id="f5f98-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="f5f98-158">Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `LongPolling` özelliği:</span><span class="sxs-lookup"><span data-stu-id="f5f98-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="f5f98-159">Seçenek</span><span class="sxs-lookup"><span data-stu-id="f5f98-159">Option</span></span> | <span data-ttu-id="f5f98-160">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-160">Default Value</span></span> | <span data-ttu-id="f5f98-161">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="f5f98-162">90 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-162">90 seconds</span></span> | <span data-ttu-id="f5f98-163">En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler.</span><span class="sxs-lookup"><span data-stu-id="f5f98-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="f5f98-164">Bu değeri azaltmak daha sık verme yeni bir yoklama isteği göndermek istemci neden olur.</span><span class="sxs-lookup"><span data-stu-id="f5f98-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="f5f98-165">WebSocket taşıma kullanılarak yapılandırılabilir ek seçenekler sahip `WebSockets` özelliği:</span><span class="sxs-lookup"><span data-stu-id="f5f98-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="f5f98-166">Seçenek</span><span class="sxs-lookup"><span data-stu-id="f5f98-166">Option</span></span> | <span data-ttu-id="f5f98-167">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-167">Default Value</span></span> | <span data-ttu-id="f5f98-168">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="f5f98-169">5 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-169">5 seconds</span></span> | <span data-ttu-id="f5f98-170">Bu zaman aralığı içinde kapatmak istemci başarısız olursa sunucunun kapatıldıktan sonra bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="f5f98-171">Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` üst bilgisi için özel bir değer.</span><span class="sxs-lookup"><span data-stu-id="f5f98-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="f5f98-172">Temsilci, giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="f5f98-173">İstemci seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f5f98-173">Configure client options</span></span>

<span data-ttu-id="f5f98-174">İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (kullanılabilir), hem .NET hem de JavaScript istemcileri de itibariyle `HubConnection` kendisi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="f5f98-175">Günlük tutmayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5f98-175">Configure logging</span></span>

<span data-ttu-id="f5f98-176">Günlük kaydı kullanarak .NET istemci yapılandırılmış `ConfigureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="f5f98-177">Günlüğe kaydetme sağlayıcıları ve filtreleri sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="f5f98-178">Bkz: [ASP.NET Core günlüğü](xref:fundamentals/logging/index#how-to-add-providers) daha fazla bilgi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="f5f98-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="f5f98-179">Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="f5f98-180">Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgelere bölümü.</span><span class="sxs-lookup"><span data-stu-id="f5f98-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="f5f98-181">Örneğin, konsol günlüğe kaydetmeyi etkinleştirmek için yükleme `Microsoft.Extensions.Logging.Console` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="f5f98-182">Çağrı `AddConsole` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5f98-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="f5f98-183">Benzer bir JavaScript istemci `configureLogging` yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="f5f98-184">Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyini belirten değer.</span><span class="sxs-lookup"><span data-stu-id="f5f98-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="f5f98-185">Tarayıcı konsol penceresine günlüklerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="f5f98-186">Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="f5f98-187">JavaScript istemci için kullanılabilen günlük düzeyleri aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="f5f98-188">Günlük düzeyi şu değerlerden birini ayarlamak, iletileri günlüğe kaydedilmesini sağlar **veya yukarıdaki** düzeyi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="f5f98-189">Düzey</span><span class="sxs-lookup"><span data-stu-id="f5f98-189">Level</span></span> | <span data-ttu-id="f5f98-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="f5f98-191">Günlüğe ileti kaydedilmedi.</span><span class="sxs-lookup"><span data-stu-id="f5f98-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="f5f98-192">Bir uygulamanın tamamında hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="f5f98-193">Geçerli işlem bir hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="f5f98-194">Önemli olmayan bir sorunu işaret eden iletileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="f5f98-195">Bilgilendirme iletileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="f5f98-196">Tanılama iletileri hata ayıklama için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="f5f98-197">Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="f5f98-198">İzin verilen taşımalar yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f5f98-198">Configure allowed transports</span></span>

<span data-ttu-id="f5f98-199">SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağırın (`withUrl` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="f5f98-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="f5f98-200">Bir bit düzeyinde OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="f5f98-201">Tüm aktarımları varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-201">All transports are enabled by default.</span></span>

<span data-ttu-id="f5f98-202">Örneğin, Server-Sent olayları taşıma devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:</span><span class="sxs-lookup"><span data-stu-id="f5f98-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="f5f98-203">JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="f5f98-204">Taşıyıcı kimlik doğrulaması yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5f98-204">Configure bearer authentication</span></span>

<span data-ttu-id="f5f98-205">SignalR istekleri ile birlikte kimlik doğrulama verilerini sağlamak için kullanmayı `AccessTokenProvider` seçeneği (`accessTokenFactory` javascript'teki) istenen erişim belirteci döndüren bir işlev belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="f5f98-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="f5f98-206">.NET istemci, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` başlığı türünde `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="f5f98-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="f5f98-207">JavaScript istemci erişim belirtecini bir taşıyıcı belirteç kullanılan **dışında** tarayıcı API'leri kısıtlama (özellikle de Server-Sent olayları ve WebSockets istekleri) üst bilgileri uygulama özelliği burada birkaç durumda.</span><span class="sxs-lookup"><span data-stu-id="f5f98-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="f5f98-208">Bu durumlarda, erişim belirtecini bir sorgu dizesi değeri sağlanan `access_token`.</span><span class="sxs-lookup"><span data-stu-id="f5f98-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="f5f98-209">.NET istemci `AccessTokenProvider` seçenekleri Temsilcide kullanılarak seçeneği belirtilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="f5f98-210">JavaScript istemcisinde ayarlayarak erişim belirteci yapılandırılmıştır `accessTokenFactory` seçenekleri nesnesinde ile sekmesindeki `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="f5f98-211">Zaman aşımı ve tutma seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f5f98-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="f5f98-212">Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekler bulunur `HubConnection` nesnenin kendisini:</span><span class="sxs-lookup"><span data-stu-id="f5f98-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="f5f98-213">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="f5f98-213">.NET Option</span></span> | <span data-ttu-id="f5f98-214">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="f5f98-214">JavaScript Option</span></span> | <span data-ttu-id="f5f98-215">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-215">Default Value</span></span> | <span data-ttu-id="f5f98-216">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="f5f98-217">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="f5f98-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="f5f98-218">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="f5f98-218">Timeout for server activity.</span></span> <span data-ttu-id="f5f98-219">Sunucu bir ileti bu aralık içinde gönderilebilen taşınmadığından, istemci sunucusu bağlantısı kesildi ve Tetikleyicileri dikkate `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="f5f98-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f5f98-220">Bu değer sunucudan gönderilecek PING iletisi için yeterince büyük **ve** zaman aşımı aralığı içinde istemci tarafından alındı.</span><span class="sxs-lookup"><span data-stu-id="f5f98-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="f5f98-221">Önerilen değer: bir sayının en az iki sunucu `KeepAliveInterval` ping gelmesi için zaman tanınması için değer.</span><span class="sxs-lookup"><span data-stu-id="f5f98-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="f5f98-222">Yapılandırılamaz</span><span class="sxs-lookup"><span data-stu-id="f5f98-222">Not configurable</span></span> | <span data-ttu-id="f5f98-223">15 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-223">15 seconds</span></span> | <span data-ttu-id="f5f98-224">İlk sunucu el sıkışma için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="f5f98-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="f5f98-225">Sunucu bu aralığı el sıkışması yanıt göndermediği durumlarda, istemciyi el sıkışması ve tetikleyiciler iptal `Closed` olay (`onclose` JavaScript dilinde).</span><span class="sxs-lookup"><span data-stu-id="f5f98-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="f5f98-226">Bu zaman aşımı hataları el sıkışması ciddi ağ gecikmesi nedeniyle gerçekleşiyor, yalnızca değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="f5f98-227">Anlaşma işlemi hakkında daha fazla ayrıntı için [SignalR hub'ı protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="f5f98-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="f5f98-228">.NET istemci zaman aşımı değerlerini olarak belirtilen `TimeSpan` değerleri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="f5f98-229">JavaScript istemci zaman aşımı değerlerini, milisaniye cinsinden süre belirten bir sayı olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="f5f98-230">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f5f98-230">Configure additional options</span></span>

<span data-ttu-id="f5f98-231">Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) metodunda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="f5f98-232">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="f5f98-232">.NET Option</span></span> | <span data-ttu-id="f5f98-233">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="f5f98-233">JavaScript Option</span></span> | <span data-ttu-id="f5f98-234">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="f5f98-234">Default Value</span></span> | <span data-ttu-id="f5f98-235">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f5f98-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="f5f98-236">HTTP isteklerinde bir taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="f5f98-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="f5f98-237">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="f5f98-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="f5f98-238">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="f5f98-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="f5f98-239">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="f5f98-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="f5f98-240">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-240">Not configurable \*</span></span> | <span data-ttu-id="f5f98-241">boş</span><span class="sxs-lookup"><span data-stu-id="f5f98-241">Empty</span></span> | <span data-ttu-id="f5f98-242">TLS sertifikalarını isteklerinde kimlik doğrulamak üzere göndermek için bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="f5f98-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="f5f98-243">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-243">Not configurable \*</span></span> | <span data-ttu-id="f5f98-244">boş</span><span class="sxs-lookup"><span data-stu-id="f5f98-244">Empty</span></span> | <span data-ttu-id="f5f98-245">Her HTTP isteği göndermek için HTTP tanımlama bilgileri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="f5f98-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="f5f98-246">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-246">Not configurable \*</span></span> | <span data-ttu-id="f5f98-247">boş</span><span class="sxs-lookup"><span data-stu-id="f5f98-247">Empty</span></span> | <span data-ttu-id="f5f98-248">Her HTTP isteği göndermek için kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="f5f98-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="f5f98-249">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-249">Not configurable \*</span></span> | <span data-ttu-id="f5f98-250">5 saniye</span><span class="sxs-lookup"><span data-stu-id="f5f98-250">5 seconds</span></span> | <span data-ttu-id="f5f98-251">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="f5f98-251">WebSockets only.</span></span> <span data-ttu-id="f5f98-252">En uzun süreyi sunucusunun Kapat isteği onaylamak Kapanıştan sonra istemci bekler.</span><span class="sxs-lookup"><span data-stu-id="f5f98-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="f5f98-253">Bu süre içinde sunucu kapatma bildirimi değil, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="f5f98-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="f5f98-254">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-254">Not configurable \*</span></span> | <span data-ttu-id="f5f98-255">boş</span><span class="sxs-lookup"><span data-stu-id="f5f98-255">Empty</span></span> | <span data-ttu-id="f5f98-256">Her HTTP isteği göndermek için ek HTTP üstbilgileri sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="f5f98-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="f5f98-257">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="f5f98-258">Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="f5f98-259">WebSocket bağlantılarını için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="f5f98-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="f5f98-260">Bu temsilci, bir null olmayan değer döndürmelidir ve varsayılan değer bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="f5f98-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="f5f98-261">Bu varsayılan ayarları değiştirmek ve onu döndürür ya da yeni bir dönüş `HttpMessageHandler` örneği.</span><span class="sxs-lookup"><span data-stu-id="f5f98-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="f5f98-262">**Aksi takdirde işleyici değiştirerek yaptığınızda, sağlanan işleyicisinden tutmak istediğiniz ayarları kopyaladığınızdan emin (örneğin, tanımlama bilgileri ve üst) yapılandırılmış seçenekler için yeni işleyici uygulanmayacak.**</span><span class="sxs-lookup"><span data-stu-id="f5f98-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="f5f98-263">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="f5f98-264">HTTP istekleri gönderirken HTTP proxy.</span><span class="sxs-lookup"><span data-stu-id="f5f98-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="f5f98-265">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="f5f98-266">HTTP ve Websocket'istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="f5f98-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="f5f98-267">Bu, Windows kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="f5f98-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="f5f98-268">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="f5f98-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="f5f98-269">Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="f5f98-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="f5f98-270">Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="f5f98-271">Bir yıldız işareti (\*) ile seçenekleri JavaScript istemci tarayıcısında API sınırlamaları nedeniyle yapılandırılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="f5f98-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="f5f98-272">.NET istemci, bu seçenekler için sağlanan seçenekleri temsilci tarafından değiştirilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="f5f98-273">JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesi içinde sağlanabilir `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="f5f98-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="f5f98-274">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f5f98-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
