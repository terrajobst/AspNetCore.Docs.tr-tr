---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246487"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="b0031-103">ASP.NET Core SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b0031-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="b0031-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0031-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="b0031-105">ASP.NET Core SignalR, kodlama iletileri için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="b0031-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="b0031-106">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b0031-107">JSON serileştirme, [Addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="b0031-108">`AddJsonProtocol`, `Startup.ConfigureServices` ' de [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b0031-109">@No__t-0 yöntemi, bir `options` nesnesi alan bir temsilci alır.</span><span class="sxs-lookup"><span data-stu-id="b0031-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b0031-110">Bu nesnedeki [Payloadserializeroptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) özelliği, bağımsız değişkenlerin ve dönüş değerlerinin serileştirilmesi yapılandırmak için kullanılabilen bir `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="b0031-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b0031-111">Daha fazla bilgi için bkz. [System. Text. JSON belgeleri](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="b0031-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="b0031-112">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine özellik adlarının büyük küçük harflerini değiştirmemelidir şekilde yapılandırmak için `Startup.ConfigureServices` ' da aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b0031-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="b0031-113">.NET istemcisinde aynı `AddJsonProtocol` genişletme yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="b0031-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b0031-114">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="b0031-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="b0031-115">Newtonsoft. JSON öğesine geç</span><span class="sxs-lookup"><span data-stu-id="b0031-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="b0031-116">@No__t-1 ' de desteklenmeyen `Newtonsoft.Json` özelliklerine ihtiyacınız varsa, bkz. [Newtonsoft. JSON öğesine geçme](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="b0031-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="b0031-117">JSON serileştirme, `Startup.ConfigureServices` yönteminde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilecek [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="b0031-118">@No__t-0 yöntemi, bir `options` nesnesi alan bir temsilci alır.</span><span class="sxs-lookup"><span data-stu-id="b0031-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="b0031-119">Bu nesnedeki [Payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir JSON.net `JsonSerializerSettings` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="b0031-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="b0031-120">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="b0031-121">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için `Startup.ConfigureServices` ' da aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="b0031-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="b0031-122">.NET istemcisinde aynı `AddJsonProtocol` genişletme yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="b0031-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="b0031-123">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="b0031-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b0031-124">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b0031-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="b0031-125">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0031-125">MessagePack serialization options</span></span>

<span data-ttu-id="b0031-126">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="b0031-127">Daha fazla ayrıntı için bkz. [SignalR 'de MessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="b0031-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="b0031-128">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="b0031-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="b0031-129">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-129">Configure server options</span></span>

<span data-ttu-id="b0031-130">Aşağıdaki tabloda, SignalR hub 'larını yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b0031-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b0031-131">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-131">Option</span></span> | <span data-ttu-id="b0031-132">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-132">Default Value</span></span> | <span data-ttu-id="b0031-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b0031-134">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-134">30 seconds</span></span> | <span data-ttu-id="b0031-135">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b0031-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b0031-136">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b0031-137">Önerilen değer `KeepAliveInterval` değerinin çift değeridir.</span><span class="sxs-lookup"><span data-stu-id="b0031-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b0031-138">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-138">15 seconds</span></span> | <span data-ttu-id="b0031-139">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0031-140">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-141">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0031-142">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-142">15 seconds</span></span> | <span data-ttu-id="b0031-143">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0031-144">@No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0031-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0031-145">Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.</span><span class="sxs-lookup"><span data-stu-id="b0031-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0031-146">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b0031-146">All installed protocols</span></span> | <span data-ttu-id="b0031-147">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0031-147">Protocols supported by this hub.</span></span> <span data-ttu-id="b0031-148">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0031-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0031-149">@No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0031-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0031-150">Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="b0031-151">İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı.</span><span class="sxs-lookup"><span data-stu-id="b0031-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="b0031-152">Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.</span><span class="sxs-lookup"><span data-stu-id="b0031-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="b0031-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0031-153">32 KB</span></span> | <span data-ttu-id="b0031-154">Tek bir gelen hub iletisinin en büyük boyutu.</span><span class="sxs-lookup"><span data-stu-id="b0031-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="b0031-155">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-155">Option</span></span> | <span data-ttu-id="b0031-156">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-156">Default Value</span></span> | <span data-ttu-id="b0031-157">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="b0031-158">30 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-158">30 seconds</span></span> | <span data-ttu-id="b0031-159">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b0031-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="b0031-160">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="b0031-161">Önerilen değer `KeepAliveInterval` değerinin çift değeridir.</span><span class="sxs-lookup"><span data-stu-id="b0031-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="b0031-162">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-162">15 seconds</span></span> | <span data-ttu-id="b0031-163">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0031-164">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-165">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0031-166">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-166">15 seconds</span></span> | <span data-ttu-id="b0031-167">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0031-168">@No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0031-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0031-169">Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.</span><span class="sxs-lookup"><span data-stu-id="b0031-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0031-170">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b0031-170">All installed protocols</span></span> | <span data-ttu-id="b0031-171">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0031-171">Protocols supported by this hub.</span></span> <span data-ttu-id="b0031-172">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0031-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0031-173">@No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0031-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0031-174">Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="b0031-175">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-175">Option</span></span> | <span data-ttu-id="b0031-176">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-176">Default Value</span></span> | <span data-ttu-id="b0031-177">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="b0031-178">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-178">15 seconds</span></span> | <span data-ttu-id="b0031-179">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="b0031-180">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-181">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0031-182">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-182">15 seconds</span></span> | <span data-ttu-id="b0031-183">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="b0031-184">@No__t-0 ' ı değiştirirken istemcide `ServerTimeout` @ no__t-2 @ no__t-3 ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b0031-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="b0031-185">Önerilen `ServerTimeout` @ no__t-1 @ no__t-2 değeri `KeepAliveInterval` değeri çift.</span><span class="sxs-lookup"><span data-stu-id="b0031-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="b0031-186">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="b0031-186">All installed protocols</span></span> | <span data-ttu-id="b0031-187">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="b0031-187">Protocols supported by this hub.</span></span> <span data-ttu-id="b0031-188">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b0031-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="b0031-189">@No__t-0 ise, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0031-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="b0031-190">Varsayılan değer `false` ' dır, bu durum iletileri hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="b0031-191">Seçenekler, `Startup.ConfigureServices` ' deki `AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="b0031-192">Tek bir hub için seçenekler `AddSignalR` ' da belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*> kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="b0031-193">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b0031-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b0031-194">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0031-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b0031-195">Bu seçenekler, `Startup.Configure` ' de [Maphub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="b0031-196">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0031-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="b0031-197">Bu seçenekler, `Startup.Configure` ' de [Maphub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="b0031-198">Aşağıdaki tabloda ASP.NET Core SignalR 'nin gelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b0031-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="b0031-199">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-199">Option</span></span> | <span data-ttu-id="b0031-200">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-200">Default Value</span></span> | <span data-ttu-id="b0031-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b0031-202">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0031-202">32 KB</span></span> | <span data-ttu-id="b0031-203">İstemci tarafından, geri basıncı uygulamadan önce sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b0031-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="b0031-204">Bu değeri artırmak, sunucunun geri basınç uygulamadan daha büyük iletileri daha hızlı almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b0031-205">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b0031-206">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="b0031-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b0031-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0031-207">32 KB</span></span> | <span data-ttu-id="b0031-208">Uygulama tarafından, geri basıncını gözlemlenmadan önce sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b0031-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="b0031-209">Bu değeri artırmak, sunucunun geri basınç beklemeden daha büyük iletileri daha hızlı arabelleğe almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b0031-210">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="b0031-210">All Transports are enabled.</span></span> | <span data-ttu-id="b0031-211">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="b0031-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b0031-212">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-212">See below.</span></span> | <span data-ttu-id="b0031-213">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b0031-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b0031-214">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-214">See below.</span></span> | <span data-ttu-id="b0031-215">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b0031-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="b0031-216">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-216">Option</span></span> | <span data-ttu-id="b0031-217">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-217">Default Value</span></span> | <span data-ttu-id="b0031-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="b0031-219">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0031-219">32 KB</span></span> | <span data-ttu-id="b0031-220">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b0031-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="b0031-221">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="b0031-222">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="b0031-223">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="b0031-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="b0031-224">32 KB</span><span class="sxs-lookup"><span data-stu-id="b0031-224">32 KB</span></span> | <span data-ttu-id="b0031-225">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b0031-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="b0031-226">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="b0031-227">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="b0031-227">All Transports are enabled.</span></span> | <span data-ttu-id="b0031-228">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="b0031-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="b0031-229">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-229">See below.</span></span> | <span data-ttu-id="b0031-230">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b0031-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="b0031-231">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-231">See below.</span></span> | <span data-ttu-id="b0031-232">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="b0031-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="b0031-233">Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b0031-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="b0031-234">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-234">Option</span></span> | <span data-ttu-id="b0031-235">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-235">Default Value</span></span> | <span data-ttu-id="b0031-236">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="b0031-237">90 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-237">90 seconds</span></span> | <span data-ttu-id="b0031-238">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="b0031-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="b0031-239">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="b0031-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="b0031-240">WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="b0031-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="b0031-241">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-241">Option</span></span> | <span data-ttu-id="b0031-242">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-242">Default Value</span></span> | <span data-ttu-id="b0031-243">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="b0031-244">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-244">5 seconds</span></span> | <span data-ttu-id="b0031-245">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="b0031-246">@No__t-0 üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b0031-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="b0031-247">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="b0031-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="b0031-248">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-248">Configure client options</span></span>

<span data-ttu-id="b0031-249">İstemci seçenekleri `HubConnectionBuilder` türünde (.NET ve JavaScript istemcilerinde bulunur) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="b0031-250">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` ' i içerir.</span><span class="sxs-lookup"><span data-stu-id="b0031-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="b0031-251">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-251">Configure logging</span></span>

<span data-ttu-id="b0031-252">Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="b0031-253">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="b0031-254">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b0031-255">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="b0031-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="b0031-256">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b0031-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="b0031-257">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="b0031-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="b0031-258">@No__t-0 genişletme yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="b0031-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="b0031-259">JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="b0031-260">Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri belirtin.</span><span class="sxs-lookup"><span data-stu-id="b0031-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="b0031-261">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b0031-262">@No__t-0 değeri yerine, bir günlük düzeyi adını temsil eden bir `string` değeri de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0031-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="b0031-263">Bu, `LogLevel` sabitlerine erişiminizin olmadığı ortamlarda SignalR günlüğünü yapılandırırken yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="b0031-264">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="b0031-264">The following table lists the available log levels.</span></span> <span data-ttu-id="b0031-265">@No__t-0 ' a sağladığınız değer, günlüğe kaydedilecek **Minimum** günlük düzeyini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b0031-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="b0031-266">Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="b0031-267">Dize</span><span class="sxs-lookup"><span data-stu-id="b0031-267">String</span></span>                      | <span data-ttu-id="b0031-268">logLevel</span><span class="sxs-lookup"><span data-stu-id="b0031-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="b0031-269">`info` **veya** `information`</span><span class="sxs-lookup"><span data-stu-id="b0031-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="b0031-270">`warn` **veya** `warning`</span><span class="sxs-lookup"><span data-stu-id="b0031-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b0031-271">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="b0031-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="b0031-272">Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [SignalR Diagnostics belgeleri](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="b0031-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="b0031-273">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="b0031-274">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="b0031-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="b0031-275">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` ' nın nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b0031-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="b0031-276">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="b0031-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="b0031-277">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="b0031-278">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-278">Configure allowed transports</span></span>

<span data-ttu-id="b0031-279">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında yapılandırılabilir (JavaScript 'te `withUrl`).</span><span class="sxs-lookup"><span data-stu-id="b0031-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="b0031-280">@No__t-0 değerleri, istemciyi yalnızca belirtilen aktarımları kullanacak şekilde kısıtlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="b0031-281">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="b0031-281">All transports are enabled by default.</span></span>

<span data-ttu-id="b0031-282">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="b0031-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="b0031-283">JavaScript istemcisinde aktarımlar, `withUrl` için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b0031-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b0031-284">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="b0031-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="b0031-285">Java istemcisinde, taşıma, `HttpHubConnectionBuilder` üzerinde `withTransport` yöntemiyle seçilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="b0031-286">Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="b0031-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="b0031-287">SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b0031-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="b0031-288">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-288">Configure bearer authentication</span></span>

<span data-ttu-id="b0031-289">Kimlik doğrulama verilerini SignalR istekleriyle birlikte sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te `accessTokenFactory`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0031-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="b0031-290">.NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer` türü ile `Authorization` üst bilgisi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="b0031-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="b0031-291">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0031-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="b0031-292">Bu gibi durumlarda, erişim belirteci bir sorgu dizesi değeri olarak sağlanır `access_token`.</span><span class="sxs-lookup"><span data-stu-id="b0031-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="b0031-293">.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl` ' deki Seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="b0031-294">JavaScript istemcisinde erişim belirteci, `withUrl` ' deki Seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="b0031-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="b0031-295">SignalR Java istemcisinde, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0031-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="b0031-296">[Rxjava](https://github.com/ReactiveX/RxJava) [tek @ No__t-3string >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="b0031-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="b0031-297">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0031-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="b0031-298">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="b0031-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="b0031-299">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnesinin kendisi üzerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b0031-300">.NET</span><span class="sxs-lookup"><span data-stu-id="b0031-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b0031-301">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-301">Option</span></span> | <span data-ttu-id="b0031-302">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-302">Default value</span></span> | <span data-ttu-id="b0031-303">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b0031-304">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-305">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-305">Timeout for server activity.</span></span> <span data-ttu-id="b0031-306">Sunucu bu aralıkta bir ileti göndermediyse, istemci, sunucunun bağlantısını kesmiş olarak değerlendirir ve `Closed` olayını (JavaScript 'te `onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0031-307">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-308">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b0031-309">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-309">15 seconds</span></span> | <span data-ttu-id="b0031-310">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0031-311">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını tetikler (JavaScript 'te `onclose`).</span><span class="sxs-lookup"><span data-stu-id="b0031-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0031-312">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-313">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="b0031-314">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-314">15 seconds</span></span> | <span data-ttu-id="b0031-315">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b0031-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b0031-316">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b0031-317">İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b0031-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="b0031-318">.NET Istemcisinde, zaman aşımı değerleri `TimeSpan` değerleri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b0031-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0031-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b0031-320">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-320">Option</span></span> | <span data-ttu-id="b0031-321">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-321">Default value</span></span> | <span data-ttu-id="b0031-322">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b0031-323">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-324">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-324">Timeout for server activity.</span></span> <span data-ttu-id="b0031-325">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b0031-326">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-327">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="b0031-328">15 saniye (15.000 'ları harcanan)</span><span class="sxs-lookup"><span data-stu-id="b0031-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="b0031-329">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b0031-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b0031-330">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b0031-331">İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b0031-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b0031-332">Java</span><span class="sxs-lookup"><span data-stu-id="b0031-332">Java</span></span>](#tab/java)

| <span data-ttu-id="b0031-333">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-333">Option</span></span> | <span data-ttu-id="b0031-334">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-334">Default value</span></span> | <span data-ttu-id="b0031-335">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b0031-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b0031-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b0031-337">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-338">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-338">Timeout for server activity.</span></span> <span data-ttu-id="b0031-339">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b0031-340">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-341">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b0031-342">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-342">15 seconds</span></span> | <span data-ttu-id="b0031-343">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0031-344">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b0031-345">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-346">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="b0031-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="b0031-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="b0031-348">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="b0031-349">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="b0031-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="b0031-350">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="b0031-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="b0031-351">İstemci sunucuda `ClientTimeoutInterval` kümesinde bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b0031-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="b0031-352">.NET</span><span class="sxs-lookup"><span data-stu-id="b0031-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b0031-353">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-353">Option</span></span> | <span data-ttu-id="b0031-354">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-354">Default value</span></span> | <span data-ttu-id="b0031-355">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="b0031-356">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-357">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-357">Timeout for server activity.</span></span> <span data-ttu-id="b0031-358">Sunucu bu aralıkta bir ileti göndermediyse, istemci, sunucunun bağlantısını kesmiş olarak değerlendirir ve `Closed` olayını (JavaScript 'te `onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0031-359">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-360">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="b0031-361">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-361">15 seconds</span></span> | <span data-ttu-id="b0031-362">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0031-363">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını tetikler (JavaScript 'te `onclose`).</span><span class="sxs-lookup"><span data-stu-id="b0031-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="b0031-364">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-365">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="b0031-366">.NET Istemcisinde, zaman aşımı değerleri `TimeSpan` değerleri olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b0031-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b0031-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0031-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b0031-368">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-368">Option</span></span> | <span data-ttu-id="b0031-369">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-369">Default value</span></span> | <span data-ttu-id="b0031-370">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="b0031-371">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-372">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-372">Timeout for server activity.</span></span> <span data-ttu-id="b0031-373">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="b0031-374">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-375">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olması için gereken bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b0031-376">Java</span><span class="sxs-lookup"><span data-stu-id="b0031-376">Java</span></span>](#tab/java)

| <span data-ttu-id="b0031-377">Seçenek</span><span class="sxs-lookup"><span data-stu-id="b0031-377">Option</span></span> | <span data-ttu-id="b0031-378">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-378">Default value</span></span> | <span data-ttu-id="b0031-379">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="b0031-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="b0031-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="b0031-381">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="b0031-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="b0031-382">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-382">Timeout for server activity.</span></span> <span data-ttu-id="b0031-383">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="b0031-384">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="b0031-385">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="b0031-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="b0031-386">15 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-386">15 seconds</span></span> | <span data-ttu-id="b0031-387">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="b0031-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="b0031-388">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0031-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="b0031-389">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="b0031-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="b0031-390">El sıkışma işlemi hakkında daha fazla bilgi için bkz. [SignalR hub protokol belirtimi](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="b0031-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="b0031-391">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b0031-391">Configure additional options</span></span>

<span data-ttu-id="b0031-392">Ek seçenekler `HubConnectionBuilder` ' deki `WithUrl` (JavaScript 'te `withUrl`) yönteminde veya Java istemcisinde `HttpHubConnectionBuilder` ' teki çeşitli yapılandırma API 'Lerinde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="b0031-393">.NET</span><span class="sxs-lookup"><span data-stu-id="b0031-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="b0031-394">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0031-394">.NET Option</span></span> |  <span data-ttu-id="b0031-395">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-395">Default value</span></span> | <span data-ttu-id="b0031-396">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="b0031-397">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0031-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="b0031-398">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b0031-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0031-399">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b0031-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0031-400">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0031-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="b0031-401">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b0031-401">Empty</span></span> | <span data-ttu-id="b0031-402">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b0031-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="b0031-403">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b0031-403">Empty</span></span> | <span data-ttu-id="b0031-404">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="b0031-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="b0031-405">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b0031-405">Empty</span></span> | <span data-ttu-id="b0031-406">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="b0031-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="b0031-407">5 saniye</span><span class="sxs-lookup"><span data-stu-id="b0031-407">5 seconds</span></span> | <span data-ttu-id="b0031-408">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="b0031-408">WebSockets only.</span></span> <span data-ttu-id="b0031-409">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="b0031-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="b0031-410">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="b0031-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="b0031-411">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b0031-411">Empty</span></span> | <span data-ttu-id="b0031-412">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b0031-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="b0031-413">HTTP istekleri göndermek için kullanılan @no__t (0) yapılandırmak veya değiştirmek için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b0031-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="b0031-414">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="b0031-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="b0031-415">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="b0031-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="b0031-416">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün.</span><span class="sxs-lookup"><span data-stu-id="b0031-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="b0031-417">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="b0031-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="b0031-418">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="b0031-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="b0031-419">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b0031-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="b0031-420">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="b0031-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="b0031-421">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="b0031-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="b0031-422">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="b0031-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="b0031-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="b0031-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="b0031-424">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0031-424">JavaScript Option</span></span> | <span data-ttu-id="b0031-425">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-425">Default Value</span></span> | <span data-ttu-id="b0031-426">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="b0031-427">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0031-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="b0031-428">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b0031-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0031-429">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b0031-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0031-430">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0031-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="b0031-431">Java</span><span class="sxs-lookup"><span data-stu-id="b0031-431">Java</span></span>](#tab/java)

| <span data-ttu-id="b0031-432">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="b0031-432">Java Option</span></span> | <span data-ttu-id="b0031-433">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="b0031-433">Default Value</span></span> | <span data-ttu-id="b0031-434">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b0031-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="b0031-435">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="b0031-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="b0031-436">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b0031-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="b0031-437">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="b0031-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="b0031-438">Bu ayar, Azure SignalR hizmeti kullanılırken etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="b0031-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="b0031-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="b0031-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="b0031-440">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="b0031-440">Empty</span></span> | <span data-ttu-id="b0031-441">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="b0031-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="b0031-442">.NET Istemcisinde, bu seçenekler `WithUrl` ' a sunulan seçenekler temsilcisi tarafından değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="b0031-443">JavaScript Istemcisinde, bu seçenekler `withUrl` ' a sunulan bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="b0031-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="b0031-444">Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` ' den döndürülen `HttpHubConnectionBuilder` ' daki yöntemlerle yapılandırılabilir</span><span class="sxs-lookup"><span data-stu-id="b0031-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="b0031-445">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b0031-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
