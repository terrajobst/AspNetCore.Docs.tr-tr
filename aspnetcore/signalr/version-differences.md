---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: bradygaster
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746546"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="d0f26-103">ASP.NET SignalR ile ASP.NET Core SignalR arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="d0f26-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="d0f26-104">ASP.NET Core SignalR, ASP.NET SignalR için istemcilerle veya sunucularla uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="d0f26-105">Bu makalede, ASP.NET Core SignalR 'de kaldırılan veya değiştirilen özellikler ayrıntılı olarak anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="d0f26-106">SignalR sürümünü belirleme</span><span class="sxs-lookup"><span data-stu-id="d0f26-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="d0f26-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="d0f26-107">ASP.NET SignalR</span></span> | <span data-ttu-id="d0f26-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d0f26-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="d0f26-109">Sunucu NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="d0f26-109">Server NuGet Package</span></span> | [<span data-ttu-id="d0f26-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d0f26-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="d0f26-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="d0f26-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="d0f26-112">[Microsoft. AspNetCore. SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="d0f26-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="d0f26-113">İstemci NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="d0f26-113">Client NuGet Packages</span></span> | [<span data-ttu-id="d0f26-114">Microsoft. AspNet. SignalR. Client</span><span class="sxs-lookup"><span data-stu-id="d0f26-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="d0f26-115">Microsoft. AspNet. SignalR. JS</span><span class="sxs-lookup"><span data-stu-id="d0f26-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="d0f26-116">Microsoft. AspNetCore. SignalR. Client</span><span class="sxs-lookup"><span data-stu-id="d0f26-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="d0f26-117">İstemci NPM paketi</span><span class="sxs-lookup"><span data-stu-id="d0f26-117">Client npm Package</span></span> | [<span data-ttu-id="d0f26-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="d0f26-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="d0f26-119">Java Istemcisi</span><span class="sxs-lookup"><span data-stu-id="d0f26-119">Java Client</span></span> | <span data-ttu-id="d0f26-120">[GitHub deposu](https://github.com/SignalR/java-client) kullanım dışı</span><span class="sxs-lookup"><span data-stu-id="d0f26-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="d0f26-121">Maven paketi [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="d0f26-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="d0f26-122">Sunucu uygulaması türü</span><span class="sxs-lookup"><span data-stu-id="d0f26-122">Server App Type</span></span> | <span data-ttu-id="d0f26-123">ASP.NET (System. Web) veya OWıN Self-Host</span><span class="sxs-lookup"><span data-stu-id="d0f26-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="d0f26-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0f26-124">ASP.NET Core</span></span> |
| <span data-ttu-id="d0f26-125">Desteklenen sunucu platformları</span><span class="sxs-lookup"><span data-stu-id="d0f26-125">Supported Server Platforms</span></span> | <span data-ttu-id="d0f26-126">.NET Framework 4,5 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d0f26-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="d0f26-127">.NET Framework 4.6.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d0f26-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="d0f26-128">.NET Core 2,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="d0f26-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="d0f26-129">Özellik farklılıkları</span><span class="sxs-lookup"><span data-stu-id="d0f26-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="d0f26-130">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="d0f26-130">Automatic reconnects</span></span>

<span data-ttu-id="d0f26-131">ASP.NET Core SignalR 'de otomatik yeniden bağlantılar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="d0f26-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="d0f26-132">İstemcinin bağlantısı kesilirse, yeniden bağlanmak istiyorlarsa kullanıcının açıkça yeni bir bağlantı başlatması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="d0f26-133">ASP.NET SignalR sürümünde, bağlantı kesildiğinde SignalR sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="d0f26-134">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="d0f26-134">Protocol support</span></span>

<span data-ttu-id="d0f26-135">ASP.NET Core SignalR, JSON 'ı ve [MessagePack](xref:signalr/messagepackhubprotocol)tabanlı yeni bir ikili protokolü destekler.</span><span class="sxs-lookup"><span data-stu-id="d0f26-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="d0f26-136">Ek olarak özel protokoller de oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="d0f26-137">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="d0f26-137">Transports</span></span>

<span data-ttu-id="d0f26-138">ASP.NET Core SignalR içinde süresiz çerçeve taşıması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="d0f26-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="d0f26-139">Sunucu farkları</span><span class="sxs-lookup"><span data-stu-id="d0f26-139">Differences on the server</span></span>

<span data-ttu-id="d0f26-140">ASP.NET Core SignalR sunucu tarafı kitaplıkları, hem Razor hem de MVC projeleri için **ASP.NET Core Web uygulaması** şablonunun bir parçası olan [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) paketine dahildir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="d0f26-141">ASP.NET Core SignalR bir ASP.NET Core ara yazılım olduğundan, içinde `Startup.ConfigureServices` [addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) çağırarak yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d0f26-142">Yönlendirmeyi yapılandırmak için, yollar içindeki `Startup.Configure` [useendpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) yöntemi çağrısının içindeki bağlantıları eşleyin.</span><span class="sxs-lookup"><span data-stu-id="d0f26-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d0f26-143">Yönlendirmeyi yapılandırmak için, `Startup.Configure` yöntemleri yöntemindeki [usesignalr](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntemi çağrısının içindeki hublara eşleyin.</span><span class="sxs-lookup"><span data-stu-id="d0f26-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="d0f26-144">Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="d0f26-144">Sticky sessions</span></span>

<span data-ttu-id="d0f26-145">ASP.NET SignalR için genişleme modeli, istemcilerin gruptaki herhangi bir sunucuya yeniden bağlanmasını ve iletileri göndermesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0f26-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="d0f26-146">ASP.NET Core SignalR 'de, istemci bağlantı süresince aynı sunucu ile etkileşime sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="d0f26-147">Redsıs kullanarak genişleme için, bu, yapışkan oturumların gerekli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="d0f26-148">[Azure SignalR hizmetini](/azure/azure-signalr/)kullanarak genişleme için, hizmet istemcilerle bağlantıları işlediği için yapışkan oturumlar gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="d0f26-149">Bağlantı başına tek hub</span><span class="sxs-lookup"><span data-stu-id="d0f26-149">Single hub per connection</span></span>

<span data-ttu-id="d0f26-150">ASP.NET Core SignalR sürümünde bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="d0f26-151">Bağlantılar, birden çok hub 'a erişimi paylaşmak için kullanılan tek bir bağlantı yerine doğrudan tek bir hub 'a yapılır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="d0f26-152">Akış</span><span class="sxs-lookup"><span data-stu-id="d0f26-152">Streaming</span></span>

<span data-ttu-id="d0f26-153">ASP.NET Core SignalR artık hub 'dan istemciye [veri akışı](xref:signalr/streaming) desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="d0f26-154">Durum</span><span class="sxs-lookup"><span data-stu-id="d0f26-154">State</span></span>

<span data-ttu-id="d0f26-155">İstemcilerle Hub (genellikle HubState olarak adlandırılır) arasında rastgele durum geçirme özelliği kaldırılmıştır ve ilerleme durumu iletileri için destek de sağlar.</span><span class="sxs-lookup"><span data-stu-id="d0f26-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="d0f26-156">Şu anda hub proxy 'lerinin karşılığı yok.</span><span class="sxs-lookup"><span data-stu-id="d0f26-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="d0f26-157">PersistentConnection kaldırma</span><span class="sxs-lookup"><span data-stu-id="d0f26-157">PersistentConnection removal</span></span>

<span data-ttu-id="d0f26-158">ASP.NET Core SignalR 'de, [Persistentconnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="d0f26-159">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="d0f26-159">GlobalHost</span></span>

<span data-ttu-id="d0f26-160">ASP.NET Core çerçeveye yerleşik bağımlılık ekleme (dı) vardır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="d0f26-161">Hizmetler, [Hubcontext](xref:signalr/hubcontext)'e erışmek için dı kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="d0f26-162">Almak için ASP.NET SignalR içinde kullanılan nesneASP.NETCoreSignalRiçindeyok.`GlobalHost` `HubContext`</span><span class="sxs-lookup"><span data-stu-id="d0f26-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="d0f26-163">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="d0f26-163">HubPipeline</span></span>

<span data-ttu-id="d0f26-164">ASP.NET Core SignalR `HubPipeline` modül desteğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="d0f26-165">İstemci farkları</span><span class="sxs-lookup"><span data-stu-id="d0f26-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="d0f26-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="d0f26-166">TypeScript</span></span>

<span data-ttu-id="d0f26-167">ASP.NET Core SignalR istemcisi [TypeScript](https://www.typescriptlang.org/)'te yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d0f26-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="d0f26-168">[JavaScript istemcisini](xref:signalr/javascript-client)kullanırken JavaScript veya TypeScript ile yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0f26-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="d0f26-169">JavaScript istemcisi [NPM](https://www.npmjs.com/) 'de barındırılıyor</span><span class="sxs-lookup"><span data-stu-id="d0f26-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="d0f26-170">Önceki sürümlerde JavaScript istemcisi, Visual Studio 'da bir NuGet paketi aracılığıyla elde edildi.</span><span class="sxs-lookup"><span data-stu-id="d0f26-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="d0f26-171">Temel sürümler [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) için NPM paketi Javascript kitaplıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="d0f26-172">Bu paket, **ASP.NET Core Web uygulaması** şablonuna dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="d0f26-173">NPM paketini edinmek ve yüklemek `@aspnet/signalr` için NPM kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0f26-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="d0f26-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="d0f26-174">jQuery</span></span>

<span data-ttu-id="d0f26-175">JQuery üzerindeki bağımlılık kaldırılmıştır, ancak projeler jQuery kullanmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="d0f26-176">Internet Explorer desteği</span><span class="sxs-lookup"><span data-stu-id="d0f26-176">Internet Explorer support</span></span>

<span data-ttu-id="d0f26-177">ASP.NET Core SignalR, Microsoft Internet Explorer 11 veya üstünü gerektirir (ASP.NET SignalR desteklenen Microsoft Internet Explorer 8 ve üzeri).</span><span class="sxs-lookup"><span data-stu-id="d0f26-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="d0f26-178">JavaScript istemci yöntemi sözdizimi</span><span class="sxs-lookup"><span data-stu-id="d0f26-178">JavaScript client method syntax</span></span>

<span data-ttu-id="d0f26-179">JavaScript sözdizimi, SignalR 'nin önceki sürümünden değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="d0f26-180">`$connection` Nesnesini kullanmak yerine, [hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 'sini kullanarak bir bağlantı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d0f26-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="d0f26-181">Hub 'ın çağırakullanabileceği istemci yöntemlerini belirtmek için [on](/javascript/api/@aspnet/signalr/HubConnection#on) metodunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0f26-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="d0f26-182">İstemci yöntemini oluşturduktan sonra hub bağlantısını başlatın.</span><span class="sxs-lookup"><span data-stu-id="d0f26-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="d0f26-183">Günlüğe kaydetmek veya hataları işlemek için bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodunu zincirleyin.</span><span class="sxs-lookup"><span data-stu-id="d0f26-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="d0f26-184">Hub proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="d0f26-184">Hub proxies</span></span>

<span data-ttu-id="d0f26-185">Hub proxy 'leri artık otomatik olarak oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="d0f26-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="d0f26-186">Bunun yerine, yöntem adı [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API 'sine bir dize olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="d0f26-187">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="d0f26-187">.NET and other clients</span></span>

<span data-ttu-id="d0f26-188">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi ASP.NET Core SignalR için .NET istemci kitaplıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="d0f26-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="d0f26-189">Hub 'a bir bağlantı örneği oluşturmak ve derlemek için [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0f26-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="d0f26-190">Ölçek Genişletme farklılıkları</span><span class="sxs-lookup"><span data-stu-id="d0f26-190">Scaleout differences</span></span>

<span data-ttu-id="d0f26-191">ASP.NET SignalR SQL Server ve Redsıs 'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="d0f26-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="d0f26-192">ASP.NET Core SignalR, Azure SignalR hizmetini ve Redl 'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="d0f26-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="d0f26-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d0f26-193">ASP.NET</span></span>

* [<span data-ttu-id="d0f26-194">Azure Service Bus ile SignalR ölçeği</span><span class="sxs-lookup"><span data-stu-id="d0f26-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="d0f26-195">Redsıs ile SignalR ölçeği</span><span class="sxs-lookup"><span data-stu-id="d0f26-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="d0f26-196">SQL Server ile SignalR ölçeği</span><span class="sxs-lookup"><span data-stu-id="d0f26-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="d0f26-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0f26-197">ASP.NET Core</span></span>

* [<span data-ttu-id="d0f26-198">Azure SignalR Hizmeti</span><span class="sxs-lookup"><span data-stu-id="d0f26-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="d0f26-199">Redsıs geri düzlemi</span><span class="sxs-lookup"><span data-stu-id="d0f26-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="d0f26-200">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d0f26-200">Additional resources</span></span>

* [<span data-ttu-id="d0f26-201">Merkezler</span><span class="sxs-lookup"><span data-stu-id="d0f26-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d0f26-202">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="d0f26-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="d0f26-203">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="d0f26-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="d0f26-204">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="d0f26-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
