---
title: ASP.NET SignalR Core yapılandırma
author: rachelappel
description: ASP.NET Core SignalR uygulamalarını yapılandırma öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36962464"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="fb111-103">ASP.NET SignalR Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb111-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="fb111-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fb111-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="fb111-105">ASP.NET Core SignalR iletileri kodlama için iki protokollerini destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="fb111-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="fb111-106">Her protokolün serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="fb111-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="fb111-107">JSON seri hale getirme kullanarak sunucu üzerinde yapılandırılabilir [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) sonra eklenmiş genişletme yöntemi [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb111-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fb111-108">`AddJsonProtocol` Yöntemi alır alan temsilci bir `options` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fb111-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="fb111-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Özelliktir bu nesne üzerinde bir JSON.NET `JsonSerializerSettings` bağımsız değişkenleri serileştirmek yapılandırmak ve dönüş değerleri için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="fb111-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="fb111-110">Bkz: [JSON.NET belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="fb111-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="fb111-111">Bir örnek olarak "PascalCase" özellik adları, varsayılan "camelCase" adları yerine kullanılacak serileştirici yapılandırmak için aşağıdaki kodu kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fb111-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="fb111-112">Aynı .NET İstemcisi'nde `AddJsonHubProtocol` genişletme yöntemi var. [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="fb111-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="fb111-113">`Microsoft.Extensions.DependencyInjection` Ad alanı içe, genişletme yöntemi gidermek için:</span><span class="sxs-lookup"><span data-stu-id="fb111-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="fb111-114">JSON seri hale getirme JavaScript istemcisinde şu anda yapılandırmak mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="fb111-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="fb111-115">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fb111-115">MessagePack serialization options</span></span>

<span data-ttu-id="fb111-116">Bir temsilciye sağlayarak MessagePack serileştirme yapılandırılabilir [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağırın.</span><span class="sxs-lookup"><span data-stu-id="fb111-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="fb111-117">Bkz: [SignalR MessagePack](xref:signalr/messagepackhubprotocol) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="fb111-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-118">Şu anda JavaScript istemci MessagePack serileştirme yapılandırmak mümkün değil.</span><span class="sxs-lookup"><span data-stu-id="fb111-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="fb111-119">Sunucu seçeneklerini yapılandır</span><span class="sxs-lookup"><span data-stu-id="fb111-119">Configure server options</span></span>

<span data-ttu-id="fb111-120">Aşağıdaki tabloda SignalR hub'ları yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="fb111-121">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fb111-121">Option</span></span> | <span data-ttu-id="fb111-122">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="fb111-123">İstemci bu zaman aralığında bir ilk el sıkışma iletisi göndermez, bağlantı kapalı.</span><span class="sxs-lookup"><span data-stu-id="fb111-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="fb111-124">Sunucu bu aralıkta bir ileti gönderdi taşınmadığından, ping iletisini bağlantıyı açık tutmak için otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="fb111-125">Bu hub tarafından desteklenen protokolleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-125">Protocols supported by this hub.</span></span> <span data-ttu-id="fb111-126">Varsayılan olarak, sunucuda kayıtlı tüm protokollerine izin verilir, ancak tek tek hub'ları için belirli protokolleri devre dışı bırakmak için bu listeden protokolleri kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="fb111-127">Varsa `true`, ayrıntılı bir Hub yöntemi bir özel durum oluştuğunda özel durum iletilerini istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="fb111-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="fb111-128">Varsayılan değer `false`, bu özel durum iletilerini gizli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="fb111-129">Seçenekler yapılandırılabilir tüm hub'ları için seçenekleri temsilciye sağlayarak `AddSignalR` Çağır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb111-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="fb111-130">Tek bir hub seçeneklerini geçersiz kıl sağlanan genel seçenekleri `AddSignalR` ve kullanılarak yapılandırılabilir [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="fb111-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="fb111-131">Kullanım `HttpConnectionDispatcherOptions` aktarımları ve bellek arabellek yönetimi ile ilgili gelişmiş ayarları yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="fb111-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="fb111-132">Bu seçenekler için temsilci geçirerek yapılandırılmış [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="fb111-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="fb111-133">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fb111-133">Option</span></span> | <span data-ttu-id="fb111-134">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="fb111-135">İstemciden alınan bayt sayısını, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="fb111-136">Bu değer artırıldığında sunucusunun büyük iletileri almasına izin verir, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="fb111-137">Varsayılan değer 32 KB'tır.</span><span class="sxs-lookup"><span data-stu-id="fb111-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="fb111-138">Listesini [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) bir istemciye hub'a bağlanmak için yetki verilip verilmediğini belirlemek için kullanılan nesne.</span><span class="sxs-lookup"><span data-stu-id="fb111-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="fb111-139">Varsayılan olarak, bu değerlerle doldurulur `Authorize` Hub sınıfına uygulanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="fb111-140">Uygulama tarafından gönderilen bayt sayısı, sunucu arabellekleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="fb111-141">Bu değer artırıldığında büyük iletileri göndermesini sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="fb111-142">Varsayılan değer 32 KB'tır.</span><span class="sxs-lookup"><span data-stu-id="fb111-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="fb111-143">Bir bit maskesi olan `HttpTransportType` taşımalar kısıtlayabilirsiniz değerleri bir istemci bağlamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="fb111-144">Tüm aktarımları varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="fb111-145">Uzun yoklama taşıma için özel ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="fb111-146">WebSockets taşıma için özel ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="fb111-147">Uzun yoklama taşıma kullanılarak yapılandırılabilir ek seçenekler bulunur `LongPolling` özelliği:</span><span class="sxs-lookup"><span data-stu-id="fb111-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="fb111-148">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fb111-148">Option</span></span> | <span data-ttu-id="fb111-149">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="fb111-150">En uzun süreyi sunucunun tek yoklama isteği sonlandırmadan önce istemciye gönderilecek bir ileti bekler.</span><span class="sxs-lookup"><span data-stu-id="fb111-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="fb111-151">Bu değeri azaltmak yeni yoklama istekleri daha sık vermek istemci neden olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="fb111-152">Varsayılan değeri 90 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="fb111-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="fb111-153">WebSocket aktarımı kullanılarak yapılandırılabilir ek seçenekler bulunur `WebSockets` özelliği:</span><span class="sxs-lookup"><span data-stu-id="fb111-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="fb111-154">Seçenek</span><span class="sxs-lookup"><span data-stu-id="fb111-154">Option</span></span> | <span data-ttu-id="fb111-155">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="fb111-156">Bu zaman aralığında kapatmak istemci başarısız olursa sunucunun kapandıktan sonra bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="fb111-157">Ayarlamak için kullanılan bir temsilci `Sec-WebSocket-Protocol` başlığına özel bir değer.</span><span class="sxs-lookup"><span data-stu-id="fb111-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="fb111-158">Temsilci giriş olarak istemci tarafından istenen değerleri alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="fb111-159">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb111-159">Configure client options</span></span>

<span data-ttu-id="fb111-160">İstemci seçenekleri yapılandırılabilir `HubConnectionBuilder` türü (kullanılabilir), hem .NET hem de JavaScript istemciler üzerinde de olarak `HubConnection` kendisi.</span><span class="sxs-lookup"><span data-stu-id="fb111-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="fb111-161">Günlük tutmayı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb111-161">Configure logging</span></span>

<span data-ttu-id="fb111-162">Günlük kaydı kullanarak .NET istemcisine yapılandırılmış `ConfigureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb111-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="fb111-163">Sunucuda oldukları gibi aynı şekilde sağlayıcıları ve filtreleri günlüğü kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="fb111-164">Bkz: [ASP.NET çekirdek günlüğü](xref:fundamentals/logging/index#how-to-add-providers) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="fb111-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-165">Günlüğe kaydetme sağlayıcıları kaydetmek için gerekli paketleri yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="fb111-166">Bkz: [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) tam listesi için belgeleri bölümü.</span><span class="sxs-lookup"><span data-stu-id="fb111-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="fb111-167">Örneğin, konsol günlük kaydını etkinleştirmek için yükleyin `Microsoft.Extensions.Logging.Console` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="fb111-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="fb111-168">Çağrı `AddConsole` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fb111-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="fb111-169">Benzer bir JavaScript istemci `configureLogging` yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="fb111-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="fb111-170">Sağlayan bir `LogLevel` günlük iletilerini oluşturmak için en düşük düzeyde belirten değer.</span><span class="sxs-lookup"><span data-stu-id="fb111-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="fb111-171">Günlükleri tarayıcı konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="fb111-172">Tamamen günlüğü devre dışı bırakmak için belirtmek `signalR.LogLevel.None` içinde `configureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fb111-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="fb111-173">Günlük düzeyleri JavaScript istemci için kullanılabilen aşağıda listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="fb111-174">Günlük düzeyini şu değerlerden birine ayarlamak, iletilere göz günlüğe kaydedilmesini sağlar **veya yukarıdaki** o düzeyi.</span><span class="sxs-lookup"><span data-stu-id="fb111-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="fb111-175">Düzey</span><span class="sxs-lookup"><span data-stu-id="fb111-175">Level</span></span> | <span data-ttu-id="fb111-176">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="fb111-177">İleti günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="fb111-178">Tüm uygulama hatası olduğunu gösteren iletileri.</span><span class="sxs-lookup"><span data-stu-id="fb111-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="fb111-179">Geçerli işlemde hatası olduğunu gösteren iletileri.</span><span class="sxs-lookup"><span data-stu-id="fb111-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="fb111-180">Önemli olmayan bir sorunu işaret eder iletileri.</span><span class="sxs-lookup"><span data-stu-id="fb111-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="fb111-181">Bilgilendirme iletileri.</span><span class="sxs-lookup"><span data-stu-id="fb111-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="fb111-182">Hata ayıklama için yararlı tanılama iletileri.</span><span class="sxs-lookup"><span data-stu-id="fb111-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="fb111-183">Çok ayrıntılı tanılama iletileri belirli sorunları tanılamak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fb111-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="fb111-184">İzin verilen taşımaları yapılandırın</span><span class="sxs-lookup"><span data-stu-id="fb111-184">Configure allowed transports</span></span>

<span data-ttu-id="fb111-185">SignalR tarafından kullanılan taşımalar yapılandırılabilir `WithUrl` çağrı (`withUrl` JavaScript'te).</span><span class="sxs-lookup"><span data-stu-id="fb111-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="fb111-186">Bir bit düzeyinde-OR değerlerinin `HttpTransportType` istemciye yalnızca belirtilen taşımalar kullanmasını kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="fb111-187">Tüm aktarımları varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-187">All transports are enabled by default.</span></span>

<span data-ttu-id="fb111-188">Örneğin, Server-Sent olayları aktarım devre dışı bırakmak için ancak WebSockets ve uzun yoklama bağlantılarına izin ver:</span><span class="sxs-lookup"><span data-stu-id="fb111-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="fb111-189">JavaScript istemci taşımaları ayarlayarak yapılandırılmış `transport` için sağlanan seçenekleri nesne üzerinde alan `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fb111-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="fb111-190">Taşıyıcı kimlik doğrulaması yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb111-190">Configure bearer authentication</span></span>

<span data-ttu-id="fb111-191">Kimlik doğrulama verilerinin SignalR istekleri yanı sıra sağlamak için kullanın `AccessTokenProvider` seçeneği (`accessTokenFactory` JavaScript'te) istenen erişim belirteci döndüren bir işlev belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="fb111-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="fb111-192">.NET istemcisi, bu erişim belirteci bir HTTP "Taşıyıcı kimlik doğrulaması" geçirilen belirteç (kullanma `Authorization` türü üstbilgiyle `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="fb111-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="fb111-193">JavaScript istemci erişim belirteci taşıyıcı belirteci olarak kullanılan **dışında** birkaç durumlarda burada tarayıcı API'leri kısıtlamak üstbilgilerinde (özellikle Server-Sent olayları ve WebSockets istekleri) uygulama özelliği.</span><span class="sxs-lookup"><span data-stu-id="fb111-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="fb111-194">Bu durumlarda, erişim belirteci bir sorgu dizesi değeri olarak sağlanan `access_token`.</span><span class="sxs-lookup"><span data-stu-id="fb111-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="fb111-195">.NET istemci `AccessTokenProvider` seçeneği seçenekleri temsilci kullanılarak belirtilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fb111-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="fb111-196">JavaScript istemci erişim belirteci ayarlayarak yapılandırılmış `accessTokenFactory` alanında seçenekleri nesnesinde bulunan `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fb111-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="fb111-197">Zaman aşımı ve tutma seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="fb111-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="fb111-198">Zaman aşımı ve tutma davranışını yapılandırmak için ek seçenekleri bulunur `HubConnection` nesnenin kendisini:</span><span class="sxs-lookup"><span data-stu-id="fb111-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="fb111-199">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="fb111-199">.NET Option</span></span> | <span data-ttu-id="fb111-200">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="fb111-200">JavaScript Option</span></span> | <span data-ttu-id="fb111-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="fb111-202">Sunucu etkinliğini için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="fb111-202">Timeout for server activity.</span></span> <span data-ttu-id="fb111-203">Sunucusu herhangi bir iletisi bu aralığa kurmadı gönderirse, istemci sunucu bağlantısı kesilmiş ve tetikleyici göz önünde bulundurur `Closed` olayı (`onclose` JavaScript'te).</span><span class="sxs-lookup"><span data-stu-id="fb111-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="fb111-204">Yapılandırılamaz</span><span class="sxs-lookup"><span data-stu-id="fb111-204">Not configurable</span></span> | <span data-ttu-id="fb111-205">İlk sunucu el sıkışma için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="fb111-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="fb111-206">Sunucu bu aralığa anlaşma yanıtı göndermez, istemci el sıkışma ve tetikleyici iptal `Closed` olayı (`onclose` JavaScript'te).</span><span class="sxs-lookup"><span data-stu-id="fb111-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="fb111-207">Olarak belirtilen .NET istemcisi, zaman aşımı değerleri `TimeSpan` değerleri.</span><span class="sxs-lookup"><span data-stu-id="fb111-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="fb111-208">JavaScript istemci zaman aşımı değerlerini sayı olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="fb111-209">Sayıları süre değerleri milisaniye cinsinden temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb111-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="fb111-210">Ek seçenekler yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fb111-210">Configure additional options</span></span>

<span data-ttu-id="fb111-211">Ek seçenekler yapılandırılabilir `WithUrl` (`withUrl` JavaScript'te) yöntemini `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fb111-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="fb111-212">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="fb111-212">.NET Option</span></span> | <span data-ttu-id="fb111-213">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="fb111-213">JavaScript Option</span></span> | <span data-ttu-id="fb111-214">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb111-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="fb111-215">HTTP isteklerinde taşıyıcı kimlik doğrulaması belirteci olarak sağlanan bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="fb111-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="fb111-216">Bu ayar `true` anlaşma adımı atlamak için.</span><span class="sxs-lookup"><span data-stu-id="fb111-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="fb111-217">**WebSockets aktarım yalnızca etkin aktarım olduğunda yalnızca desteklenen**.</span><span class="sxs-lookup"><span data-stu-id="fb111-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="fb111-218">Bu ayar, Azure SignalR hizmetini kullanırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="fb111-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="fb111-219">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-219">Not configurable \*</span></span> | <span data-ttu-id="fb111-220">Kimlik doğrulaması istekleri göndermek için TLS sertifikalar koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="fb111-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="fb111-221">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-221">Not configurable \*</span></span> | <span data-ttu-id="fb111-222">İçeren tüm HTTP istekleri göndermek için HTTP tanımlama bilgileri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="fb111-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="fb111-223">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-223">Not configurable \*</span></span> | <span data-ttu-id="fb111-224">Kimlik bilgilerini içeren tüm HTTP istekleri göndermek için.</span><span class="sxs-lookup"><span data-stu-id="fb111-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="fb111-225">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-225">Not configurable \*</span></span> | <span data-ttu-id="fb111-226">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="fb111-226">WebSockets only.</span></span> <span data-ttu-id="fb111-227">En uzun süreyi sunucusunun Kapat isteği onaylamak kapanış sonra istemcinin bekler.</span><span class="sxs-lookup"><span data-stu-id="fb111-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="fb111-228">Sunucusu close bu süre içinde kabul değil, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="fb111-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="fb111-229">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-229">Not configurable \*</span></span> | <span data-ttu-id="fb111-230">Daha fazla HTTP üstbilgileri içeren tüm HTTP istekleri göndermek için sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="fb111-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="fb111-231">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-231">Not configurable \*</span></span> | <span data-ttu-id="fb111-232">Yapılandırma veya değiştirmek için kullanılan bir temsilci `HttpMessageHandler` HTTP istekleri göndermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="fb111-233">WebSocket bağlantıları için kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="fb111-234">Bu temsilci null olmayan bir değer döndürmesi gerekir ve varsayılan değer bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="fb111-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="fb111-235">Bu varsayılan değeri ayarlarını değiştirin ve onu döndürür ya da tamamen yeni bir dönüş `HttpMessageHandler` örneği.</span><span class="sxs-lookup"><span data-stu-id="fb111-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="fb111-236">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-236">Not configurable \*</span></span> | <span data-ttu-id="fb111-237">HTTP istekleri gönderirken kullanmak için bir HTTP proxy.</span><span class="sxs-lookup"><span data-stu-id="fb111-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="fb111-238">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-238">Not configurable \*</span></span> | <span data-ttu-id="fb111-239">HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini göndermek için bu boolean ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="fb111-240">Bu, Windows kimlik doğrulaması kullanımını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="fb111-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="fb111-241">Yapılandırılamaz \*</span><span class="sxs-lookup"><span data-stu-id="fb111-241">Not configurable \*</span></span> | <span data-ttu-id="fb111-242">Ek WebSocket seçeneklerini yapılandırmak için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="fb111-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="fb111-243">Örneğini alır [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) seçeneklerini yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="fb111-244">Yıldız işareti (\*) seçenekleri API'leri tarayıcıda sınırlamaları nedeniyle JavaScript istemci yapılandırılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="fb111-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="fb111-245">.NET istemci, bu seçenekler için sağlanan seçenekleri temsilci tarafından değiştirilebilir `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="fb111-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="fb111-246">JavaScript istemci, bu seçenekler için sağlanan bir JavaScript nesnesinde sağlanabilir `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="fb111-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="fb111-247">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb111-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
