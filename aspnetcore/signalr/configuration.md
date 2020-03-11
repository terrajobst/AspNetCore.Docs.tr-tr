---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658636"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ef76d-103">ASP.NET Core SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ef76d-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ef76d-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-105">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ef76d-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ef76d-106">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ef76d-107">JSON serileştirme, [Addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="ef76d-108">`AddJsonProtocol`, `Startup.ConfigureServices`[Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ef76d-109">`AddJsonProtocol` yöntemi, `options` nesnesini alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ef76d-110">Bu nesnedeki [Payloadserializeroptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ef76d-111">Daha fazla bilgi için bkz. [System. Text. JSON belgeleri](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="ef76d-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="ef76d-112">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine özellik adlarının büyük küçük harflerini değiştirmemelidir şekilde yapılandırmak için, `Startup.ConfigureServices`içinde aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="ef76d-113">.NET istemcisinde aynı `AddJsonProtocol` uzantısı yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ef76d-114">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="ef76d-115">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="ef76d-116">Newtonsoft. JSON öğesine geç</span><span class="sxs-lookup"><span data-stu-id="ef76d-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="ef76d-117">`System.Text.Json`desteklenmeyen `Newtonsoft.Json` özelliklerine ihtiyacınız varsa, bkz. [Newtonsoft. JSON öğesine geçme](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="ef76d-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ef76d-118">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-118">MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-119">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ef76d-120">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ef76d-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-121">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ef76d-122">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-122">Configure server options</span></span>

<span data-ttu-id="ef76d-123">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ef76d-124">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-124">Option</span></span> | <span data-ttu-id="ef76d-125">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-125">Default Value</span></span> | <span data-ttu-id="ef76d-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ef76d-127">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-127">30 seconds</span></span> | <span data-ttu-id="ef76d-128">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ef76d-129">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ef76d-130">Önerilen değer `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ef76d-131">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-131">15 seconds</span></span> | <span data-ttu-id="ef76d-132">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ef76d-133">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-134">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ef76d-135">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-135">15 seconds</span></span> | <span data-ttu-id="ef76d-136">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ef76d-137">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ef76d-138">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ef76d-139">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ef76d-139">All installed protocols</span></span> | <span data-ttu-id="ef76d-140">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ef76d-140">Protocols supported by this hub.</span></span> <span data-ttu-id="ef76d-141">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ef76d-142">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ef76d-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ef76d-143">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="ef76d-144">İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="ef76d-145">Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="ef76d-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-146">32 KB</span></span> | <span data-ttu-id="ef76d-147">Tek bir gelen hub iletisinin en büyük boyutu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="ef76d-148">Seçenekler, `Startup.ConfigureServices``AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ef76d-149">Tek bir hub için seçenekler, `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ef76d-150">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ef76d-151">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ef76d-152">Bu seçenekler, `Startup.Configure`[> Maphub\<t](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 'ye bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ef76d-153">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ef76d-154">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-154">Option</span></span> | <span data-ttu-id="ef76d-155">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-155">Default Value</span></span> | <span data-ttu-id="ef76d-156">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ef76d-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-157">32 KB</span></span> | <span data-ttu-id="ef76d-158">İstemci tarafından, geri basıncı uygulamadan önce sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="ef76d-159">Bu değeri artırmak, sunucunun geri basınç uygulamadan daha büyük iletileri daha hızlı almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ef76d-160">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ef76d-161">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ef76d-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ef76d-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-162">32 KB</span></span> | <span data-ttu-id="ef76d-163">Uygulama tarafından, geri basıncını gözlemlenmadan önce sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="ef76d-164">Bu değeri artırmak, sunucunun geri basınç beklemeden daha büyük iletileri daha hızlı arabelleğe almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ef76d-165">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-165">All Transports are enabled.</span></span> | <span data-ttu-id="ef76d-166">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="ef76d-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ef76d-167">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-167">See below.</span></span> | <span data-ttu-id="ef76d-168">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ef76d-169">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-169">See below.</span></span> | <span data-ttu-id="ef76d-170">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ef76d-171">Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ef76d-172">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-172">Option</span></span> | <span data-ttu-id="ef76d-173">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-173">Default Value</span></span> | <span data-ttu-id="ef76d-174">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ef76d-175">90 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-175">90 seconds</span></span> | <span data-ttu-id="ef76d-176">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ef76d-177">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ef76d-178">WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ef76d-179">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-179">Option</span></span> | <span data-ttu-id="ef76d-180">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-180">Default Value</span></span> | <span data-ttu-id="ef76d-181">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ef76d-182">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-182">5 seconds</span></span> | <span data-ttu-id="ef76d-183">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ef76d-184">`Sec-WebSocket-Protocol` üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ef76d-185">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ef76d-186">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-186">Configure client options</span></span>

<span data-ttu-id="ef76d-187">İstemci seçenekleri `HubConnectionBuilder` türünde yapılandırılabilir (.NET ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="ef76d-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ef76d-188">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ef76d-189">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-189">Configure logging</span></span>

<span data-ttu-id="ef76d-190">Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ef76d-191">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ef76d-192">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-193">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ef76d-194">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ef76d-195">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ef76d-196">`AddConsole` uzantısı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ef76d-197">JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ef76d-198">Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri girin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ef76d-199">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="ef76d-200">`LogLevel` bir değer yerine, bir günlük düzeyi adını temsil eden bir `string` değeri de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="ef76d-201">Bu, `LogLevel` sabitlerine erişiminizin olmadığı ortamlarda SignalR günlüğünü yapılandırırken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="ef76d-202">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-202">The following table lists the available log levels.</span></span> <span data-ttu-id="ef76d-203">`configureLogging` için sağladığınız değer, günlüğe kaydedilecek **Minimum** günlük düzeyini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ef76d-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="ef76d-204">Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="ef76d-205">String</span><span class="sxs-lookup"><span data-stu-id="ef76d-205">String</span></span>                      | <span data-ttu-id="ef76d-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="ef76d-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="ef76d-207">`info` **veya** `information`</span><span class="sxs-lookup"><span data-stu-id="ef76d-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="ef76d-208">`warn` **veya** `warning`</span><span class="sxs-lookup"><span data-stu-id="ef76d-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="ef76d-209">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ef76d-210">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ef76d-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ef76d-211">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ef76d-212">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ef76d-213">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ef76d-214">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="ef76d-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ef76d-215">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ef76d-216">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-216">Configure allowed transports</span></span>

<span data-ttu-id="ef76d-217">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında yapılandırılabilir (JavaScript 'te`withUrl`).</span><span class="sxs-lookup"><span data-stu-id="ef76d-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ef76d-218">`HttpTransportType` değerlerinin bit düzeyinde veya değerleri yalnızca belirtilen aktarımları kullanacak şekilde sınırlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ef76d-219">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-219">All transports are enabled by default.</span></span>

<span data-ttu-id="ef76d-220">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="ef76d-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ef76d-221">JavaScript istemcisinde aktarımlar, `withUrl`için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="ef76d-222">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="ef76d-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="ef76d-223">Java istemcisinde, taşıma, `HttpHubConnectionBuilder``withTransport` yöntemiyle seçilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ef76d-224">Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ef76d-225">SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ef76d-226">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-226">Configure bearer authentication</span></span>

<span data-ttu-id="ef76d-227">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te`accessTokenFactory`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ef76d-228">.NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer`türü ile `Authorization` üst bilgisi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="ef76d-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ef76d-229">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ef76d-230">Bu durumlarda, erişim belirteci `access_token`bir sorgu dizesi değeri olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ef76d-231">.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl`içindeki seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ef76d-232">JavaScript istemcisinde, erişim belirteci `withUrl`içindeki seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ef76d-233">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ef76d-234">[Rxjava](https://github.com/ReactiveX/RxJava) [tek bir\<dize >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ef76d-235">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ef76d-236">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ef76d-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ef76d-237">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-238">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-239">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-239">Option</span></span> | <span data-ttu-id="ef76d-240">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-240">Default value</span></span> | <span data-ttu-id="ef76d-241">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ef76d-242">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-243">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-243">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-244">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısının kesileceğini kabul eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-245">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-246">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ef76d-247">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-247">15 seconds</span></span> | <span data-ttu-id="ef76d-248">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-249">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-250">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-251">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ef76d-252">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-252">15 seconds</span></span> | <span data-ttu-id="ef76d-253">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-254">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-255">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ef76d-256">.NET Istemcisinde zaman aşımı değerleri `TimeSpan` değerler olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="ef76d-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-258">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-258">Option</span></span> | <span data-ttu-id="ef76d-259">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-259">Default value</span></span> | <span data-ttu-id="ef76d-260">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ef76d-261">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-262">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-262">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-263">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ef76d-264">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-265">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ef76d-266">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-267">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-268">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-269">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-270">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-270">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-271">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-271">Option</span></span> | <span data-ttu-id="ef76d-272">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-272">Default value</span></span> | <span data-ttu-id="ef76d-273">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ef76d-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ef76d-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ef76d-275">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-276">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-276">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-277">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-278">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-279">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ef76d-280">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-280">15 seconds</span></span> | <span data-ttu-id="ef76d-281">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-282">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-283">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-284">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ef76d-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ef76d-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ef76d-286">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-287">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-288">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-289">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="ef76d-290">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-290">Configure additional options</span></span>

<span data-ttu-id="ef76d-291">Ek seçenekler, `HubConnectionBuilder` veya Java istemcisindeki `HttpHubConnectionBuilder` çeşitli yapılandırma API 'Lerinde `WithUrl` (JavaScript 'te`withUrl`) yönteminde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-292">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-293">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-293">.NET Option</span></span> |  <span data-ttu-id="ef76d-294">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-294">Default value</span></span> | <span data-ttu-id="ef76d-295">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ef76d-296">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ef76d-297">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-298">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-299">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ef76d-300">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-300">Empty</span></span> | <span data-ttu-id="ef76d-301">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ef76d-302">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-302">Empty</span></span> | <span data-ttu-id="ef76d-303">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ef76d-304">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-304">Empty</span></span> | <span data-ttu-id="ef76d-305">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="ef76d-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ef76d-306">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-306">5 seconds</span></span> | <span data-ttu-id="ef76d-307">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ef76d-307">WebSockets only.</span></span> <span data-ttu-id="ef76d-308">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ef76d-309">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="ef76d-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ef76d-310">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-310">Empty</span></span> | <span data-ttu-id="ef76d-311">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ef76d-312">HTTP istekleri göndermek için kullanılan `HttpMessageHandler` yapılandırmak veya değiştirmek için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ef76d-313">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="ef76d-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="ef76d-314">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ef76d-315">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ef76d-316">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="ef76d-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ef76d-317">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="ef76d-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ef76d-318">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ef76d-319">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ef76d-320">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ef76d-321">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="ef76d-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-323">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-323">JavaScript Option</span></span> | <span data-ttu-id="ef76d-324">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-324">Default Value</span></span> | <span data-ttu-id="ef76d-325">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ef76d-326">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ef76d-327">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-328">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-329">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-330">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-330">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-331">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-331">Java Option</span></span> | <span data-ttu-id="ef76d-332">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-332">Default Value</span></span> | <span data-ttu-id="ef76d-333">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ef76d-334">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ef76d-335">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-336">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-337">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ef76d-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ef76d-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ef76d-339">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-339">Empty</span></span> | <span data-ttu-id="ef76d-340">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ef76d-341">.NET Istemcisinde, bu seçenekler `WithUrl`için belirtilen seçenekler temsilcisi tarafından değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ef76d-342">JavaScript Istemcisinde, bu seçenekler `withUrl`için sunulan bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ef76d-343">Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` döndürülen `HttpHubConnectionBuilder` yöntemleriyle yapılandırılabilir</span><span class="sxs-lookup"><span data-stu-id="ef76d-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ef76d-344">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ef76d-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ef76d-345">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-346">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ef76d-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ef76d-347">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ef76d-348">JSON serileştirme, `Startup.ConfigureServices` yönteminizin [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilecek [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ef76d-349">`AddJsonProtocol` yöntemi, `options` nesnesini alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ef76d-350">Bu nesnedeki [Payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir JSON.net `JsonSerializerSettings` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ef76d-351">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="ef76d-352">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için, `Startup.ConfigureServices`' de aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="ef76d-353">.NET istemcisinde aynı `AddJsonProtocol` uzantısı yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ef76d-354">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ef76d-355">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ef76d-356">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-356">MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-357">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ef76d-358">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ef76d-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-359">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ef76d-360">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-360">Configure server options</span></span>

<span data-ttu-id="ef76d-361">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ef76d-362">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-362">Option</span></span> | <span data-ttu-id="ef76d-363">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-363">Default Value</span></span> | <span data-ttu-id="ef76d-364">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ef76d-365">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-365">30 seconds</span></span> | <span data-ttu-id="ef76d-366">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ef76d-367">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ef76d-368">Önerilen değer `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ef76d-369">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-369">15 seconds</span></span> | <span data-ttu-id="ef76d-370">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ef76d-371">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-372">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ef76d-373">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-373">15 seconds</span></span> | <span data-ttu-id="ef76d-374">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ef76d-375">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ef76d-376">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ef76d-377">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ef76d-377">All installed protocols</span></span> | <span data-ttu-id="ef76d-378">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ef76d-378">Protocols supported by this hub.</span></span> <span data-ttu-id="ef76d-379">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ef76d-380">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ef76d-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ef76d-381">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="ef76d-382">Seçenekler, `Startup.ConfigureServices``AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ef76d-383">Tek bir hub için seçenekler, `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ef76d-384">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ef76d-385">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ef76d-386">Bu seçenekler, `Startup.Configure`[> Maphub\<t](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 'ye bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ef76d-387">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ef76d-388">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-388">Option</span></span> | <span data-ttu-id="ef76d-389">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-389">Default Value</span></span> | <span data-ttu-id="ef76d-390">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ef76d-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-391">32 KB</span></span> | <span data-ttu-id="ef76d-392">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ef76d-393">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ef76d-394">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ef76d-395">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ef76d-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ef76d-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-396">32 KB</span></span> | <span data-ttu-id="ef76d-397">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ef76d-398">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ef76d-399">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-399">All Transports are enabled.</span></span> | <span data-ttu-id="ef76d-400">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="ef76d-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ef76d-401">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-401">See below.</span></span> | <span data-ttu-id="ef76d-402">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ef76d-403">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-403">See below.</span></span> | <span data-ttu-id="ef76d-404">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ef76d-405">Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ef76d-406">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-406">Option</span></span> | <span data-ttu-id="ef76d-407">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-407">Default Value</span></span> | <span data-ttu-id="ef76d-408">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ef76d-409">90 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-409">90 seconds</span></span> | <span data-ttu-id="ef76d-410">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ef76d-411">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ef76d-412">WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ef76d-413">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-413">Option</span></span> | <span data-ttu-id="ef76d-414">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-414">Default Value</span></span> | <span data-ttu-id="ef76d-415">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ef76d-416">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-416">5 seconds</span></span> | <span data-ttu-id="ef76d-417">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ef76d-418">`Sec-WebSocket-Protocol` üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ef76d-419">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ef76d-420">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-420">Configure client options</span></span>

<span data-ttu-id="ef76d-421">İstemci seçenekleri `HubConnectionBuilder` türünde yapılandırılabilir (.NET ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="ef76d-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ef76d-422">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ef76d-423">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-423">Configure logging</span></span>

<span data-ttu-id="ef76d-424">Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ef76d-425">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ef76d-426">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-427">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ef76d-428">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ef76d-429">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ef76d-430">`AddConsole` uzantısı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ef76d-431">JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ef76d-432">Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri girin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ef76d-433">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ef76d-434">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ef76d-435">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ef76d-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ef76d-436">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ef76d-437">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ef76d-438">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ef76d-439">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="ef76d-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ef76d-440">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ef76d-441">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-441">Configure allowed transports</span></span>

<span data-ttu-id="ef76d-442">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında yapılandırılabilir (JavaScript 'te`withUrl`).</span><span class="sxs-lookup"><span data-stu-id="ef76d-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ef76d-443">`HttpTransportType` değerlerinin bit düzeyinde veya değerleri yalnızca belirtilen aktarımları kullanacak şekilde sınırlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ef76d-444">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-444">All transports are enabled by default.</span></span>

<span data-ttu-id="ef76d-445">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="ef76d-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ef76d-446">JavaScript istemcisinde aktarımlar, `withUrl`için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="ef76d-447">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="ef76d-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ef76d-448">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-448">Configure bearer authentication</span></span>

<span data-ttu-id="ef76d-449">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te`accessTokenFactory`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ef76d-450">.NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer`türü ile `Authorization` üst bilgisi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="ef76d-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ef76d-451">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ef76d-452">Bu durumlarda, erişim belirteci `access_token`bir sorgu dizesi değeri olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ef76d-453">.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl`içindeki seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ef76d-454">JavaScript istemcisinde, erişim belirteci `withUrl`içindeki seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ef76d-455">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ef76d-456">[Rxjava](https://github.com/ReactiveX/RxJava) [tek bir\<dize >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ef76d-457">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ef76d-458">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ef76d-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ef76d-459">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-460">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-461">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-461">Option</span></span> | <span data-ttu-id="ef76d-462">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-462">Default value</span></span> | <span data-ttu-id="ef76d-463">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ef76d-464">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-465">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-465">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-466">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısının kesileceğini kabul eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-467">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-468">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ef76d-469">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-469">15 seconds</span></span> | <span data-ttu-id="ef76d-470">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-471">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-472">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-473">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ef76d-474">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-474">15 seconds</span></span> | <span data-ttu-id="ef76d-475">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-476">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-477">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ef76d-478">.NET Istemcisinde zaman aşımı değerleri `TimeSpan` değerler olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="ef76d-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-480">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-480">Option</span></span> | <span data-ttu-id="ef76d-481">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-481">Default value</span></span> | <span data-ttu-id="ef76d-482">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ef76d-483">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-484">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-484">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-485">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ef76d-486">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-487">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ef76d-488">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-489">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-490">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-491">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-492">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-492">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-493">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-493">Option</span></span> | <span data-ttu-id="ef76d-494">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-494">Default value</span></span> | <span data-ttu-id="ef76d-495">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ef76d-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ef76d-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ef76d-497">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-498">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-498">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-499">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-500">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-501">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ef76d-502">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-502">15 seconds</span></span> | <span data-ttu-id="ef76d-503">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-504">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-505">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-506">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ef76d-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ef76d-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ef76d-508">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-509">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ef76d-510">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ef76d-511">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ef76d-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="ef76d-512">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-512">Configure additional options</span></span>

<span data-ttu-id="ef76d-513">Ek seçenekler, `HubConnectionBuilder` veya Java istemcisindeki `HttpHubConnectionBuilder` çeşitli yapılandırma API 'Lerinde `WithUrl` (JavaScript 'te`withUrl`) yönteminde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-514">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-515">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-515">.NET Option</span></span> |  <span data-ttu-id="ef76d-516">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-516">Default value</span></span> | <span data-ttu-id="ef76d-517">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ef76d-518">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ef76d-519">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-520">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-521">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ef76d-522">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-522">Empty</span></span> | <span data-ttu-id="ef76d-523">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ef76d-524">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-524">Empty</span></span> | <span data-ttu-id="ef76d-525">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ef76d-526">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-526">Empty</span></span> | <span data-ttu-id="ef76d-527">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="ef76d-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ef76d-528">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-528">5 seconds</span></span> | <span data-ttu-id="ef76d-529">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ef76d-529">WebSockets only.</span></span> <span data-ttu-id="ef76d-530">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ef76d-531">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="ef76d-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ef76d-532">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-532">Empty</span></span> | <span data-ttu-id="ef76d-533">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ef76d-534">HTTP istekleri göndermek için kullanılan `HttpMessageHandler` yapılandırmak veya değiştirmek için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ef76d-535">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="ef76d-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="ef76d-536">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ef76d-537">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ef76d-538">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="ef76d-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ef76d-539">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="ef76d-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ef76d-540">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ef76d-541">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ef76d-542">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ef76d-543">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="ef76d-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-545">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-545">JavaScript Option</span></span> | <span data-ttu-id="ef76d-546">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-546">Default Value</span></span> | <span data-ttu-id="ef76d-547">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ef76d-548">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ef76d-549">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-550">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-551">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-552">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-552">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-553">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-553">Java Option</span></span> | <span data-ttu-id="ef76d-554">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-554">Default Value</span></span> | <span data-ttu-id="ef76d-555">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ef76d-556">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ef76d-557">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-558">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-559">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ef76d-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ef76d-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ef76d-561">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-561">Empty</span></span> | <span data-ttu-id="ef76d-562">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ef76d-563">.NET Istemcisinde, bu seçenekler `WithUrl`için belirtilen seçenekler temsilcisi tarafından değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ef76d-564">JavaScript Istemcisinde, bu seçenekler `withUrl`için sunulan bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ef76d-565">Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` döndürülen `HttpHubConnectionBuilder` yöntemleriyle yapılandırılabilir</span><span class="sxs-lookup"><span data-stu-id="ef76d-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ef76d-566">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ef76d-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ef76d-567">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-568">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ef76d-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ef76d-569">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="ef76d-570">JSON serileştirme, `Startup.ConfigureServices` yönteminizin [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilecek [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ef76d-571">`AddJsonProtocol` yöntemi, `options` nesnesini alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ef76d-572">Bu nesnedeki [Payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir JSON.net `JsonSerializerSettings` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ef76d-573">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="ef76d-574">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için, `Startup.ConfigureServices`' de aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="ef76d-575">.NET istemcisinde aynı `AddJsonProtocol` uzantısı yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ef76d-576">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ef76d-577">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ef76d-578">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-578">MessagePack serialization options</span></span>

<span data-ttu-id="ef76d-579">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ef76d-580">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ef76d-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-581">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ef76d-582">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-582">Configure server options</span></span>

<span data-ttu-id="ef76d-583">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="ef76d-584">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-584">Option</span></span> | <span data-ttu-id="ef76d-585">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-585">Default Value</span></span> | <span data-ttu-id="ef76d-586">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ef76d-587">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-587">15 seconds</span></span> | <span data-ttu-id="ef76d-588">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ef76d-589">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-590">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ef76d-591">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-591">15 seconds</span></span> | <span data-ttu-id="ef76d-592">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ef76d-593">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ef76d-594">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ef76d-595">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ef76d-595">All installed protocols</span></span> | <span data-ttu-id="ef76d-596">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ef76d-596">Protocols supported by this hub.</span></span> <span data-ttu-id="ef76d-597">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ef76d-598">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ef76d-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ef76d-599">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="ef76d-600">Seçenekler, `Startup.ConfigureServices``AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ef76d-601">Tek bir hub için seçenekler, `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ef76d-602">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ef76d-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="ef76d-603">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ef76d-604">Bu seçenekler, `Startup.Configure`[> Maphub\<t](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 'ye bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ef76d-605">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="ef76d-606">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-606">Option</span></span> | <span data-ttu-id="ef76d-607">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-607">Default Value</span></span> | <span data-ttu-id="ef76d-608">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ef76d-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-609">32 KB</span></span> | <span data-ttu-id="ef76d-610">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ef76d-611">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ef76d-612">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ef76d-613">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ef76d-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ef76d-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="ef76d-614">32 KB</span></span> | <span data-ttu-id="ef76d-615">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ef76d-616">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ef76d-617">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-617">All Transports are enabled.</span></span> | <span data-ttu-id="ef76d-618">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="ef76d-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ef76d-619">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-619">See below.</span></span> | <span data-ttu-id="ef76d-620">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ef76d-621">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-621">See below.</span></span> | <span data-ttu-id="ef76d-622">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="ef76d-623">Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ef76d-624">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-624">Option</span></span> | <span data-ttu-id="ef76d-625">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-625">Default Value</span></span> | <span data-ttu-id="ef76d-626">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ef76d-627">90 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-627">90 seconds</span></span> | <span data-ttu-id="ef76d-628">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ef76d-629">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="ef76d-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ef76d-630">WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ef76d-631">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-631">Option</span></span> | <span data-ttu-id="ef76d-632">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-632">Default Value</span></span> | <span data-ttu-id="ef76d-633">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ef76d-634">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-634">5 seconds</span></span> | <span data-ttu-id="ef76d-635">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ef76d-636">`Sec-WebSocket-Protocol` üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ef76d-637">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ef76d-638">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-638">Configure client options</span></span>

<span data-ttu-id="ef76d-639">İstemci seçenekleri `HubConnectionBuilder` türünde yapılandırılabilir (.NET ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="ef76d-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ef76d-640">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ef76d-641">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-641">Configure logging</span></span>

<span data-ttu-id="ef76d-642">Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ef76d-643">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ef76d-644">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ef76d-645">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ef76d-646">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ef76d-647">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ef76d-648">`AddConsole` uzantısı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ef76d-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ef76d-649">JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ef76d-650">Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri girin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ef76d-651">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ef76d-652">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="ef76d-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ef76d-653">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ef76d-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ef76d-654">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ef76d-655">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ef76d-656">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ef76d-657">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="ef76d-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ef76d-658">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ef76d-659">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-659">Configure allowed transports</span></span>

<span data-ttu-id="ef76d-660">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında yapılandırılabilir (JavaScript 'te`withUrl`).</span><span class="sxs-lookup"><span data-stu-id="ef76d-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ef76d-661">`HttpTransportType` değerlerinin bit düzeyinde veya değerleri yalnızca belirtilen aktarımları kullanacak şekilde sınırlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ef76d-662">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-662">All transports are enabled by default.</span></span>

<span data-ttu-id="ef76d-663">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="ef76d-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ef76d-664">JavaScript istemcisinde aktarımlar, `withUrl`için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ef76d-665">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-665">Configure bearer authentication</span></span>

<span data-ttu-id="ef76d-666">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te`accessTokenFactory`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ef76d-667">.NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer`türü ile `Authorization` üst bilgisi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="ef76d-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ef76d-668">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ef76d-669">Bu durumlarda, erişim belirteci `access_token`bir sorgu dizesi değeri olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ef76d-670">.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl`içindeki seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ef76d-671">JavaScript istemcisinde, erişim belirteci `withUrl`içindeki seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ef76d-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ef76d-672">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ef76d-673">[Rxjava](https://github.com/ReactiveX/RxJava) [tek bir\<dize >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ef76d-674">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ef76d-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ef76d-675">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ef76d-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ef76d-676">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-677">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-678">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-678">Option</span></span> | <span data-ttu-id="ef76d-679">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-679">Default value</span></span> | <span data-ttu-id="ef76d-680">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ef76d-681">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-682">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-682">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-683">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısının kesileceğini kabul eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-684">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-685">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ef76d-686">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-686">15 seconds</span></span> | <span data-ttu-id="ef76d-687">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-688">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ef76d-689">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-690">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ef76d-691">.NET Istemcisinde zaman aşımı değerleri `TimeSpan` değerler olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ef76d-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="ef76d-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-693">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-693">Option</span></span> | <span data-ttu-id="ef76d-694">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-694">Default value</span></span> | <span data-ttu-id="ef76d-695">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ef76d-696">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-697">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-697">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-698">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ef76d-699">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-700">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-701">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-701">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-702">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ef76d-702">Option</span></span> | <span data-ttu-id="ef76d-703">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-703">Default value</span></span> | <span data-ttu-id="ef76d-704">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ef76d-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ef76d-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ef76d-706">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ef76d-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ef76d-707">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-707">Timeout for server activity.</span></span> <span data-ttu-id="ef76d-708">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-709">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ef76d-710">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ef76d-711">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-711">15 seconds</span></span> | <span data-ttu-id="ef76d-712">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ef76d-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="ef76d-713">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ef76d-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ef76d-714">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ef76d-715">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ef76d-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="ef76d-716">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ef76d-716">Configure additional options</span></span>

<span data-ttu-id="ef76d-717">Ek seçenekler, `HubConnectionBuilder` veya Java istemcisindeki `HttpHubConnectionBuilder` çeşitli yapılandırma API 'Lerinde `WithUrl` (JavaScript 'te`withUrl`) yönteminde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="ef76d-718">.NET</span><span class="sxs-lookup"><span data-stu-id="ef76d-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ef76d-719">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-719">.NET Option</span></span> |  <span data-ttu-id="ef76d-720">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-720">Default value</span></span> | <span data-ttu-id="ef76d-721">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ef76d-722">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ef76d-723">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-724">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-725">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ef76d-726">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-726">Empty</span></span> | <span data-ttu-id="ef76d-727">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ef76d-728">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-728">Empty</span></span> | <span data-ttu-id="ef76d-729">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ef76d-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ef76d-730">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-730">Empty</span></span> | <span data-ttu-id="ef76d-731">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="ef76d-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ef76d-732">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ef76d-732">5 seconds</span></span> | <span data-ttu-id="ef76d-733">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ef76d-733">WebSockets only.</span></span> <span data-ttu-id="ef76d-734">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ef76d-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ef76d-735">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="ef76d-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ef76d-736">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-736">Empty</span></span> | <span data-ttu-id="ef76d-737">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ef76d-738">HTTP istekleri göndermek için kullanılan `HttpMessageHandler` yapılandırmak veya değiştirmek için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ef76d-739">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="ef76d-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="ef76d-740">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ef76d-741">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ef76d-742">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="ef76d-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ef76d-743">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="ef76d-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ef76d-744">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ef76d-745">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="ef76d-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ef76d-746">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ef76d-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ef76d-747">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="ef76d-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="ef76d-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ef76d-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ef76d-749">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-749">JavaScript Option</span></span> | <span data-ttu-id="ef76d-750">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-750">Default Value</span></span> | <span data-ttu-id="ef76d-751">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ef76d-752">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ef76d-753">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-754">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-755">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="ef76d-756">Java</span><span class="sxs-lookup"><span data-stu-id="ef76d-756">Java</span></span>](#tab/java)

| <span data-ttu-id="ef76d-757">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="ef76d-757">Java Option</span></span> | <span data-ttu-id="ef76d-758">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ef76d-758">Default Value</span></span> | <span data-ttu-id="ef76d-759">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ef76d-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ef76d-760">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ef76d-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ef76d-761">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ef76d-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ef76d-762">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ef76d-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ef76d-763">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ef76d-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ef76d-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ef76d-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ef76d-765">Boş</span><span class="sxs-lookup"><span data-stu-id="ef76d-765">Empty</span></span> | <span data-ttu-id="ef76d-766">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ef76d-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ef76d-767">.NET Istemcisinde, bu seçenekler `WithUrl`için belirtilen seçenekler temsilcisi tarafından değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ef76d-768">JavaScript Istemcisinde, bu seçenekler `withUrl`için sunulan bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ef76d-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ef76d-769">Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` döndürülen `HttpHubConnectionBuilder` yöntemleriyle yapılandırılabilir</span><span class="sxs-lookup"><span data-stu-id="ef76d-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ef76d-770">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ef76d-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end