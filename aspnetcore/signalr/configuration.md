---
title: ASP.NET Core SignalR yapılandırması
author: bradygaster
description: ASP.NET Core SignalR uygulamalarını nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963979"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="ebc9f-103">ASP.NET Core SignalR yapılandırması</span><span class="sxs-lookup"><span data-stu-id="ebc9f-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ebc9f-104">JSON/MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ebc9f-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ebc9f-105">ASP.NET Core SignalR iletileri kodlamak için iki protokolü destekler: [JSON](https://www.json.org/) ve [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ebc9f-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ebc9f-106">Her protokol serileştirme yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ebc9f-107">JSON serileştirme, [Addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="ebc9f-108">`AddJsonProtocol`, `Startup.ConfigureServices`[Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ebc9f-109">`AddJsonProtocol` yöntemi, `options` nesnesini alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ebc9f-110">Bu nesnedeki [Payloadserializeroptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ebc9f-111">Daha fazla bilgi için bkz. [System. Text. JSON belgeleri](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="ebc9f-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="ebc9f-112">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine özellik adlarının büyük küçük harflerini değiştirmemelidir şekilde yapılandırmak için, `Startup.ConfigureServices`içinde aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="ebc9f-113">.NET istemcisinde aynı `AddJsonProtocol` uzantısı yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ebc9f-114">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ebc9f-115">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="ebc9f-116">Newtonsoft. JSON öğesine geç</span><span class="sxs-lookup"><span data-stu-id="ebc9f-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="ebc9f-117">`System.Text.Json`desteklenmeyen `Newtonsoft.Json` özelliklerine ihtiyacınız varsa, bkz. [Newtonsoft. JSON öğesine geçme](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="ebc9f-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ebc9f-118">JSON serileştirme, `Startup.ConfigureServices` yönteminizin [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) öğesinden sonra eklenebilecek [addjsonprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) genişletme yöntemi kullanılarak sunucuda yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ebc9f-119">`AddJsonProtocol` yöntemi, `options` nesnesini alan bir temsilciyi alır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ebc9f-120">Bu nesnedeki [Payloadserializersettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) özelliği, bağımsız değişkenlerin serileştirmesini ve dönüş değerlerini yapılandırmak için kullanılabilen bir JSON.net `JsonSerializerSettings` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ebc9f-121">Daha fazla bilgi için [JSON.net belgelerine](https://www.newtonsoft.com/json/help/html/Introduction.htm)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="ebc9f-122">Örnek olarak, serileştiriciyi varsayılan "camelCase" adları yerine "PascalCase" özellik adlarını kullanacak şekilde yapılandırmak için, `Startup.ConfigureServices`' de aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="ebc9f-123">.NET istemcisinde aynı `AddJsonProtocol` uzantısı yöntemi [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder)üzerinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ebc9f-124">Uzantı metodunu çözümlemek için `Microsoft.Extensions.DependencyInjection` ad alanı içeri aktarılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="ebc9f-125">JavaScript istemcisinde Şu anda JSON serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ebc9f-126">MessagePack serileştirme seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ebc9f-126">MessagePack serialization options</span></span>

<span data-ttu-id="ebc9f-127">MessagePack serileştirme, [Addmessagepackprotocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) çağrısına bir temsilci sağlanarak yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ebc9f-128">Daha fazla bilgi için bkz. [SignalRMessagePack](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ebc9f-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ebc9f-129">Şu anda JavaScript istemcisinde MessagePack serileştirme yapılandırmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ebc9f-130">Sunucu seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-130">Configure server options</span></span>

<span data-ttu-id="ebc9f-131">Aşağıdaki tabloda SignalR hub 'ları yapılandırmaya yönelik seçenekler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ebc9f-132">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-132">Option</span></span> | <span data-ttu-id="ebc9f-133">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-133">Default Value</span></span> | <span data-ttu-id="ebc9f-134">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ebc9f-135">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-135">30 seconds</span></span> | <span data-ttu-id="ebc9f-136">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ebc9f-137">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ebc9f-138">Önerilen değer `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ebc9f-139">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-139">15 seconds</span></span> | <span data-ttu-id="ebc9f-140">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ebc9f-141">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-142">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ebc9f-143">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-143">15 seconds</span></span> | <span data-ttu-id="ebc9f-144">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ebc9f-145">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ebc9f-146">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ebc9f-147">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ebc9f-147">All installed protocols</span></span> | <span data-ttu-id="ebc9f-148">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-148">Protocols supported by this hub.</span></span> <span data-ttu-id="ebc9f-149">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ebc9f-150">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ebc9f-151">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="ebc9f-152">İstemci yükleme akışları için ara belleğe oluşturulabilecek en fazla öğe sayısı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="ebc9f-153">Bu sınıra ulaşıldığında, sunucu akış öğelerini işlemeden, etkinleştirmeleri işleme engellenir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="ebc9f-154">32 KB</span><span class="sxs-lookup"><span data-stu-id="ebc9f-154">32 KB</span></span> | <span data-ttu-id="ebc9f-155">Tek bir gelen hub iletisinin en büyük boyutu.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="ebc9f-156">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-156">Option</span></span> | <span data-ttu-id="ebc9f-157">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-157">Default Value</span></span> | <span data-ttu-id="ebc9f-158">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ebc9f-159">30 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-159">30 seconds</span></span> | <span data-ttu-id="ebc9f-160">Sunucu, bu aralıkta (canlı tut dahil) bir ileti almamışsa istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ebc9f-161">Bu işlem, uygulanması nedeniyle istemcinin bağlantısının gerçekten kesilmesinin ardından bu zaman aşımı aralığından daha uzun sürebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ebc9f-162">Önerilen değer `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ebc9f-163">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-163">15 seconds</span></span> | <span data-ttu-id="ebc9f-164">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ebc9f-165">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-166">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ebc9f-167">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-167">15 seconds</span></span> | <span data-ttu-id="ebc9f-168">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ebc9f-169">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ebc9f-170">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ebc9f-171">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ebc9f-171">All installed protocols</span></span> | <span data-ttu-id="ebc9f-172">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-172">Protocols supported by this hub.</span></span> <span data-ttu-id="ebc9f-173">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ebc9f-174">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ebc9f-175">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="ebc9f-176">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-176">Option</span></span> | <span data-ttu-id="ebc9f-177">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-177">Default Value</span></span> | <span data-ttu-id="ebc9f-178">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ebc9f-179">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-179">15 seconds</span></span> | <span data-ttu-id="ebc9f-180">İstemci bu zaman aralığı içinde bir ilk el sıkışma iletisi göndermezse bağlantı kapatılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ebc9f-181">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-182">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ebc9f-183">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-183">15 seconds</span></span> | <span data-ttu-id="ebc9f-184">Sunucu bu Aralık dahilinde bir ileti göndermediyse bağlantının açık tutulması için bir ping iletisi otomatik olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ebc9f-185">`KeepAliveInterval`değiştirilirken, istemcide `ServerTimeout`/`serverTimeoutInMilliseconds` ayarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ebc9f-186">Önerilen `ServerTimeout`/`serverTimeoutInMilliseconds` değeri `KeepAliveInterval` değeri iki katına kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ebc9f-187">Tüm yüklü protokoller</span><span class="sxs-lookup"><span data-stu-id="ebc9f-187">All installed protocols</span></span> | <span data-ttu-id="ebc9f-188">Bu hub tarafından desteklenen protokoller.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-188">Protocols supported by this hub.</span></span> <span data-ttu-id="ebc9f-189">Varsayılan olarak, sunucuda kayıtlı tüm protokollere izin verilir, ancak tek tek hub 'lara yönelik belirli protokolleri devre dışı bırakmak için protokollerin bu listeden kaldırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ebc9f-190">`true`, bir hub yönteminde özel durum oluştuğunda istemcilere ayrıntılı özel durum iletileri döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ebc9f-191">Varsayılan değer `false`, bu özel durum iletilerinde gizli bilgiler bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="ebc9f-192">Seçenekler, `Startup.ConfigureServices``AddSignalR` çağrısına bir seçenek temsilcisi sağlayarak tüm Hub 'lar için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ebc9f-193">Tek bir hub için seçenekler, `AddSignalR` belirtilen genel seçenekleri geçersiz kılar ve <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ebc9f-194">Gelişmiş HTTP yapılandırma seçenekleri</span><span class="sxs-lookup"><span data-stu-id="ebc9f-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ebc9f-195">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ebc9f-196">Bu seçenekler, `Startup.Configure`[> Maphub\<t](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) 'ye bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ebc9f-197">Aktarımlara ve bellek arabelleği yönetimine ilişkin gelişmiş ayarları yapılandırmak için `HttpConnectionDispatcherOptions` kullanın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ebc9f-198">Bu seçenekler, `Startup.Configure`[> Maphub\<t](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) 'ye bir temsilci geçirilerek yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ebc9f-199">Aşağıdaki tabloda ASP.NET Core SignalRgelişmiş HTTP seçeneklerini yapılandırma seçenekleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ebc9f-200">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-200">Option</span></span> | <span data-ttu-id="ebc9f-201">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-201">Default Value</span></span> | <span data-ttu-id="ebc9f-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ebc9f-203">32 KB</span><span class="sxs-lookup"><span data-stu-id="ebc9f-203">32 KB</span></span> | <span data-ttu-id="ebc9f-204">İstemci tarafından, geri basıncı uygulamadan önce sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="ebc9f-205">Bu değeri artırmak, sunucunun geri basınç uygulamadan daha büyük iletileri daha hızlı almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ebc9f-206">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ebc9f-207">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ebc9f-208">32 KB</span><span class="sxs-lookup"><span data-stu-id="ebc9f-208">32 KB</span></span> | <span data-ttu-id="ebc9f-209">Uygulama tarafından, geri basıncını gözlemlenmadan önce sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="ebc9f-210">Bu değeri artırmak, sunucunun geri basınç beklemeden daha büyük iletileri daha hızlı arabelleğe almasına izin verir, ancak bellek tüketimini artırabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ebc9f-211">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-211">All Transports are enabled.</span></span> | <span data-ttu-id="ebc9f-212">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ebc9f-213">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-213">See below.</span></span> | <span data-ttu-id="ebc9f-214">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ebc9f-215">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-215">See below.</span></span> | <span data-ttu-id="ebc9f-216">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="ebc9f-217">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-217">Option</span></span> | <span data-ttu-id="ebc9f-218">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-218">Default Value</span></span> | <span data-ttu-id="ebc9f-219">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ebc9f-220">32 KB</span><span class="sxs-lookup"><span data-stu-id="ebc9f-220">32 KB</span></span> | <span data-ttu-id="ebc9f-221">İstemciden sunucunun arabelleğe aldığı en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ebc9f-222">Bu değeri artırmak, sunucunun daha büyük iletiler almasına izin verir, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ebc9f-223">Veriler, hub sınıfına uygulanan `Authorize` özniteliklerinden otomatik olarak toplanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ebc9f-224">Bir istemcinin hub 'a bağlanmasına yetkili olup olmadığını belirlemede kullanılan [ıauthorizedata](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) nesnelerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ebc9f-225">32 KB</span><span class="sxs-lookup"><span data-stu-id="ebc9f-225">32 KB</span></span> | <span data-ttu-id="ebc9f-226">Uygulama tarafından sunucunun arabelleklerinin gönderdiği en fazla bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ebc9f-227">Bu değeri artırmak, sunucunun daha büyük iletiler göndermesini sağlar, ancak bellek tüketimini olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ebc9f-228">Tüm aktarımlar etkin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-228">All Transports are enabled.</span></span> | <span data-ttu-id="ebc9f-229">Bir istemcinin bağlanmak için kullanabileceği taşımaları kısıtlayabilecek `HttpTransportType` değerlerinin bir bit bayrakları numaralandırması.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ebc9f-230">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-230">See below.</span></span> | <span data-ttu-id="ebc9f-231">Uzun yoklama taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ebc9f-232">Aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-232">See below.</span></span> | <span data-ttu-id="ebc9f-233">WebSockets taşımasına özgü ek seçenekler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="ebc9f-234">Uzun yoklama taşıması `LongPolling` özelliği kullanılarak yapılandırılabilecek ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ebc9f-235">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-235">Option</span></span> | <span data-ttu-id="ebc9f-236">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-236">Default Value</span></span> | <span data-ttu-id="ebc9f-237">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ebc9f-238">90 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-238">90 seconds</span></span> | <span data-ttu-id="ebc9f-239">Tek bir yoklama isteğini sonlandırmadan önce sunucunun istemciye göndermek için bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ebc9f-240">Bu değeri azaltmak istemcinin yeni yoklama istekleri daha sık vermesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ebc9f-241">WebSocket taşıması `WebSockets` özelliği kullanılarak yapılandırılabilen ek seçeneklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ebc9f-242">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-242">Option</span></span> | <span data-ttu-id="ebc9f-243">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-243">Default Value</span></span> | <span data-ttu-id="ebc9f-244">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ebc9f-245">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-245">5 seconds</span></span> | <span data-ttu-id="ebc9f-246">Sunucu kapandıktan sonra, istemci bu zaman aralığında kapanamazsa bağlantı sonlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ebc9f-247">`Sec-WebSocket-Protocol` üst bilgisini özel bir değere ayarlamak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ebc9f-248">Temsilci, istemci tarafından istenen değerleri girdi olarak alır ve istenen değeri döndürmesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ebc9f-249">İstemci seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-249">Configure client options</span></span>

<span data-ttu-id="ebc9f-250">İstemci seçenekleri `HubConnectionBuilder` türünde yapılandırılabilir (.NET ve JavaScript istemcilerinde kullanılabilir).</span><span class="sxs-lookup"><span data-stu-id="ebc9f-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ebc9f-251">Java istemcisinde de bulunur, ancak `HttpHubConnectionBuilder` alt sınıfı, Oluşturucu yapılandırma seçeneklerinin yanı sıra `HubConnection` kendisidir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ebc9f-252">Günlüğe kaydetmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-252">Configure logging</span></span>

<span data-ttu-id="ebc9f-253">Günlüğe kaydetme, .NET Istemcisinde `ConfigureLogging` yöntemi kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ebc9f-254">Günlüğe kaydetme sağlayıcıları ve filtreler, sunucuda oldukları gibi aynı şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ebc9f-255">Daha fazla bilgi için [oturum açma ASP.NET Core](xref:fundamentals/logging/index) belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ebc9f-256">Günlüğe kaydetme sağlayıcılarını kaydetmek için gerekli paketleri yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ebc9f-257">Tam liste için docs 'ın [yerleşik günlük sağlayıcıları](xref:fundamentals/logging/index#built-in-logging-providers) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ebc9f-258">Örneğin, konsol günlüğünü etkinleştirmek için `Microsoft.Extensions.Logging.Console` NuGet paketini yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ebc9f-259">`AddConsole` uzantısı yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ebc9f-260">JavaScript istemcisinde benzer bir `configureLogging` yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ebc9f-261">Üretilecek günlük iletilerinin en düşük düzeyini belirten bir `LogLevel` değeri girin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ebc9f-262">Günlükler tarayıcı konsolu penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ebc9f-263">`LogLevel` bir değer yerine, bir günlük düzeyi adını temsil eden bir `string` değeri de sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="ebc9f-264">Bu, `LogLevel` sabitlerine erişiminizin olmadığı ortamlarda SignalR günlüğe kaydetme yapılandırılırken yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="ebc9f-265">Aşağıdaki tabloda kullanılabilir günlük düzeyleri listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-265">The following table lists the available log levels.</span></span> <span data-ttu-id="ebc9f-266">`configureLogging` için sağladığınız değer, günlüğe kaydedilecek **Minimum** günlük düzeyini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="ebc9f-267">Bu düzeyde günlüğe kaydedilen iletiler **veya tabloda bundan sonra listelenen düzeyler**günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="ebc9f-268">Dize</span><span class="sxs-lookup"><span data-stu-id="ebc9f-268">String</span></span>                      | <span data-ttu-id="ebc9f-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="ebc9f-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="ebc9f-270">`info` **veya** `information`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="ebc9f-271">`warn` **veya** `warning`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ebc9f-272">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ebc9f-273">Günlüğe kaydetme hakkında daha fazla bilgi için [SignalR tanılama belgelerine](xref:signalr/diagnostics)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ebc9f-274">SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ebc9f-275">Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ebc9f-276">Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ebc9f-277">Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ebc9f-278">Bu, güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ebc9f-279">İzin verilen taşımaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-279">Configure allowed transports</span></span>

<span data-ttu-id="ebc9f-280">SignalR tarafından kullanılan aktarımlar `WithUrl` çağrısında (`withUrl` JavaScript 'te) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ebc9f-281">`HttpTransportType` değerlerinin bit düzeyinde veya değerleri yalnızca belirtilen aktarımları kullanacak şekilde sınırlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ebc9f-282">Tüm aktarımlar varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-282">All transports are enabled by default.</span></span>

<span data-ttu-id="ebc9f-283">Örneğin, sunucu tarafından gönderilen olay taşımasını devre dışı bırakmak, ancak WebSockets ve uzun yoklama bağlantılarına izin vermek için:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ebc9f-284">JavaScript istemcisinde aktarımlar, `withUrl`için sunulan Options nesnesindeki `transport` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ebc9f-285">Java Client WebSockets 'in bu sürümünde, kullanılabilir tek aktarım bir sürümdür.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="ebc9f-286">Java istemcisinde, taşıma, `HttpHubConnectionBuilder``withTransport` yöntemiyle seçilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ebc9f-287">Java istemcisi WebSockets taşımasını varsayılan olarak kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ebc9f-288">SignalR Java istemcisi henüz taşıma geri dönüşü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ebc9f-289">Taşıyıcı kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-289">Configure bearer authentication</span></span>

<span data-ttu-id="ebc9f-290">SignalR isteklerle birlikte kimlik doğrulama verileri sağlamak için, istenen erişim belirtecini döndüren bir işlevi belirtmek üzere `AccessTokenProvider` seçeneğini (JavaScript 'te`accessTokenFactory`) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ebc9f-291">.NET Istemcisinde, bu erişim belirteci bir HTTP "taşıyıcı kimlik doğrulaması" belirteci olarak geçirilir (`Bearer`türü ile `Authorization` üst bilgisi kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="ebc9f-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ebc9f-292">JavaScript istemcisinde, tarayıcı API 'Lerinin üstbilgileri uygulama özelliğini (özellikle de sunucu tarafından gönderilen olaylar ve WebSockets istekleri) kısıtlayacağı birkaç durum **dışında** , erişim belirteci bir taşıyıcı belirteci olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ebc9f-293">Bu durumlarda, erişim belirteci `access_token`bir sorgu dizesi değeri olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ebc9f-294">.NET istemcisinde `AccessTokenProvider` seçeneği, `WithUrl`içindeki seçenekler temsilcisi kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ebc9f-295">JavaScript istemcisinde, erişim belirteci `withUrl`içindeki seçenekler nesnesindeki `accessTokenFactory` alanı ayarlanarak yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ebc9f-296">Java istemcisinde SignalR, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a bir erişim belirteci fabrikası sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ebc9f-297">[Rxjava](https://github.com/ReactiveX/RxJava) [tek bir\<dize >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ebc9f-298">[Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ebc9f-299">Zaman aşımını yapılandırın ve canlı tut seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ebc9f-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ebc9f-300">Zaman aşımını ve canlı tutma davranışını yapılandırmaya yönelik ek seçenekler `HubConnection` nesnenin kendisinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ebc9f-301">.NET</span><span class="sxs-lookup"><span data-stu-id="ebc9f-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ebc9f-302">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-302">Option</span></span> | <span data-ttu-id="ebc9f-303">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-303">Default value</span></span> | <span data-ttu-id="ebc9f-304">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ebc9f-305">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-306">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-306">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-307">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısının kesileceğini kabul eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ebc9f-308">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-309">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ebc9f-310">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-310">15 seconds</span></span> | <span data-ttu-id="ebc9f-311">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="ebc9f-312">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ebc9f-313">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-314">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ebc9f-315">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-315">15 seconds</span></span> | <span data-ttu-id="ebc9f-316">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ebc9f-317">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ebc9f-318">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ebc9f-319">.NET Istemcisinde zaman aşımı değerleri `TimeSpan` değerler olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ebc9f-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ebc9f-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ebc9f-321">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-321">Option</span></span> | <span data-ttu-id="ebc9f-322">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-322">Default value</span></span> | <span data-ttu-id="ebc9f-323">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ebc9f-324">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-325">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-325">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-326">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ebc9f-327">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-328">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ebc9f-329">15 saniye (15.000 'ları harcanan)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="ebc9f-330">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ebc9f-331">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ebc9f-332">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ebc9f-333">Java</span><span class="sxs-lookup"><span data-stu-id="ebc9f-333">Java</span></span>](#tab/java)

| <span data-ttu-id="ebc9f-334">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-334">Option</span></span> | <span data-ttu-id="ebc9f-335">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-335">Default value</span></span> | <span data-ttu-id="ebc9f-336">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ebc9f-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ebc9f-338">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-339">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-339">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-340">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ebc9f-341">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-342">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ebc9f-343">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-343">15 seconds</span></span> | <span data-ttu-id="ebc9f-344">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="ebc9f-345">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ebc9f-346">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-347">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ebc9f-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ebc9f-349">15 saniye (15.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-350">İstemcinin ping iletileri gönderdiği aralığı belirler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ebc9f-351">İstemciden herhangi bir ileti gönderildiğinde, süreölçeri aralığın başına sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ebc9f-352">İstemci sunucuda `ClientTimeoutInterval` ayarlanmış bir ileti göndermediyse sunucu, istemcinin bağlantısının kesileceğini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ebc9f-353">.NET</span><span class="sxs-lookup"><span data-stu-id="ebc9f-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ebc9f-354">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-354">Option</span></span> | <span data-ttu-id="ebc9f-355">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-355">Default value</span></span> | <span data-ttu-id="ebc9f-356">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ebc9f-357">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-358">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-358">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-359">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısının kesileceğini kabul eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ebc9f-360">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-361">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ebc9f-362">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-362">15 seconds</span></span> | <span data-ttu-id="ebc9f-363">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="ebc9f-364">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `Closed` olayını (JavaScript 'te`onclose`) tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ebc9f-365">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-366">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ebc9f-367">.NET Istemcisinde zaman aşımı değerleri `TimeSpan` değerler olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ebc9f-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ebc9f-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ebc9f-369">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-369">Option</span></span> | <span data-ttu-id="ebc9f-370">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-370">Default value</span></span> | <span data-ttu-id="ebc9f-371">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ebc9f-372">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-373">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-373">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-374">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onclose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ebc9f-375">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-376">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ebc9f-377">Java</span><span class="sxs-lookup"><span data-stu-id="ebc9f-377">Java</span></span>](#tab/java)

| <span data-ttu-id="ebc9f-378">Seçenek</span><span class="sxs-lookup"><span data-stu-id="ebc9f-378">Option</span></span> | <span data-ttu-id="ebc9f-379">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-379">Default value</span></span> | <span data-ttu-id="ebc9f-380">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ebc9f-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ebc9f-382">30 saniye (30.000 milisaniye)</span><span class="sxs-lookup"><span data-stu-id="ebc9f-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ebc9f-383">Sunucu etkinliği için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-383">Timeout for server activity.</span></span> <span data-ttu-id="ebc9f-384">Sunucu bu aralıkta bir ileti göndermediyse istemci, sunucunun bağlantısını kesmediği kabul eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ebc9f-385">Bu değer, ping iletisinin sunucudan gönderilmesi **ve** istemci tarafından zaman aşımı aralığı içinde alınması için yeterince büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ebc9f-386">Önerilen değer, ping için en az iki sunucunun `KeepAliveInterval` değeri olan bir sayıdır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ebc9f-387">15 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-387">15 seconds</span></span> | <span data-ttu-id="ebc9f-388">İlk sunucu el sıkışması için zaman aşımı.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="ebc9f-389">Sunucu bu aralıkta bir el sıkışma yanıtı göndermezse, istemci el sıkışmasını iptal eder ve `onClose` olayını tetikler.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ebc9f-390">Bu, yalnızca önemli ağ gecikmesi nedeniyle el sıkışma zaman aşımı hataları gerçekleşirken değiştirilmesi gereken gelişmiş bir ayardır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ebc9f-391">El sıkışma işlemi hakkında daha fazla ayrıntı için [SignalR hub protokol belirtimine](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md)bakın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="ebc9f-392">Ek seçenekleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ebc9f-392">Configure additional options</span></span>

<span data-ttu-id="ebc9f-393">Ek seçenekler, `HubConnectionBuilder` veya Java istemcisindeki `HttpHubConnectionBuilder` çeşitli yapılandırma API 'Lerinde `WithUrl` (JavaScript 'te`withUrl`) yönteminde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ebc9f-394">.NET</span><span class="sxs-lookup"><span data-stu-id="ebc9f-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ebc9f-395">.NET seçeneği</span><span class="sxs-lookup"><span data-stu-id="ebc9f-395">.NET Option</span></span> |  <span data-ttu-id="ebc9f-396">Varsayılan değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-396">Default value</span></span> | <span data-ttu-id="ebc9f-397">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ebc9f-398">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ebc9f-399">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ebc9f-400">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ebc9f-401">Azure SignalR hizmeti kullanılırken bu ayar etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ebc9f-402">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ebc9f-402">Empty</span></span> | <span data-ttu-id="ebc9f-403">Kimlik doğrulaması isteklerine gönderilmek üzere TLS sertifikaları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ebc9f-404">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ebc9f-404">Empty</span></span> | <span data-ttu-id="ebc9f-405">Her HTTP isteğiyle gönderilmek üzere HTTP tanımlama bilgilerinin bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ebc9f-406">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ebc9f-406">Empty</span></span> | <span data-ttu-id="ebc9f-407">Her HTTP isteğiyle gönderilen kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ebc9f-408">5 saniye</span><span class="sxs-lookup"><span data-stu-id="ebc9f-408">5 seconds</span></span> | <span data-ttu-id="ebc9f-409">Yalnızca WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-409">WebSockets only.</span></span> <span data-ttu-id="ebc9f-410">Sunucunun kapatma isteğini onaylaması için kapatıldıktan sonra bekleyeceği en uzun süre.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ebc9f-411">Sunucu bu süre içinde kapatmayı kabul etmezse, istemci bağlantısını keser.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ebc9f-412">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ebc9f-412">Empty</span></span> | <span data-ttu-id="ebc9f-413">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ebc9f-414">HTTP istekleri göndermek için kullanılan `HttpMessageHandler` yapılandırmak veya değiştirmek için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ebc9f-415">WebSocket bağlantıları için kullanılmıyor.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="ebc9f-416">Bu temsilci null olmayan bir değer döndürmelidir ve varsayılan değeri bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ebc9f-417">Bu varsayılan değerde ayarları değiştirin ve döndürün ya da yeni bir `HttpMessageHandler` örneği döndürün.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ebc9f-418">**İşleyiciyi değiştirirken, belirtilen işleyiciden tutmak istediğiniz ayarları kopyalamadığınızdan emin olun, aksi takdirde, yapılandırılan seçenekler (tanımlama bilgileri ve üstbilgiler gibi) yeni işleyiciye uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="ebc9f-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ebc9f-419">HTTP istekleri gönderilirken kullanılacak bir HTTP proxy 'si.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ebc9f-420">Bu Boole değeri HTTP ve WebSockets istekleri için varsayılan kimlik bilgilerini gönderecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ebc9f-421">Bu, Windows kimlik doğrulamasının kullanılmasını mümkün.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ebc9f-422">Ek WebSocket seçeneklerini yapılandırmak için kullanılabilen bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ebc9f-423">Seçenekleri yapılandırmak için kullanılabilecek [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ebc9f-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ebc9f-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ebc9f-425">JavaScript seçeneği</span><span class="sxs-lookup"><span data-stu-id="ebc9f-425">JavaScript Option</span></span> | <span data-ttu-id="ebc9f-426">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-426">Default Value</span></span> | <span data-ttu-id="ebc9f-427">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ebc9f-428">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ebc9f-429">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ebc9f-430">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ebc9f-431">Azure SignalR hizmeti kullanılırken bu ayar etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ebc9f-432">Java</span><span class="sxs-lookup"><span data-stu-id="ebc9f-432">Java</span></span>](#tab/java)

| <span data-ttu-id="ebc9f-433">Java seçeneği</span><span class="sxs-lookup"><span data-stu-id="ebc9f-433">Java Option</span></span> | <span data-ttu-id="ebc9f-434">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="ebc9f-434">Default Value</span></span> | <span data-ttu-id="ebc9f-435">Açıklama</span><span class="sxs-lookup"><span data-stu-id="ebc9f-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ebc9f-436">HTTP isteklerinde taşıyıcı kimlik doğrulama belirteci olarak belirtilen bir dize döndüren bir işlev.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ebc9f-437">Anlaşma adımını atlamak için bunu `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ebc9f-438">**Yalnızca WebSockets taşıması etkin olan tek taşıma olduğunda desteklenir**.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ebc9f-439">Azure SignalR hizmeti kullanılırken bu ayar etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ebc9f-440">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ebc9f-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ebc9f-441">Olmamalıdır</span><span class="sxs-lookup"><span data-stu-id="ebc9f-441">Empty</span></span> | <span data-ttu-id="ebc9f-442">Her HTTP isteğiyle birlikte gönderilmek üzere ek HTTP üstbilgileri haritası.</span><span class="sxs-lookup"><span data-stu-id="ebc9f-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ebc9f-443">.NET Istemcisinde, bu seçenekler `WithUrl`için belirtilen seçenekler temsilcisi tarafından değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ebc9f-444">JavaScript Istemcisinde, bu seçenekler `withUrl`için sunulan bir JavaScript nesnesi içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="ebc9f-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ebc9f-445">Java istemcisinde, bu seçenekler `HubConnectionBuilder.create("HUB URL")` döndürülen `HttpHubConnectionBuilder` yöntemleriyle yapılandırılabilir</span><span class="sxs-lookup"><span data-stu-id="ebc9f-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ebc9f-446">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ebc9f-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
