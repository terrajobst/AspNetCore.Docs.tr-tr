---
title: SignalR ve ASP.NET Core arasındaki farklar SignalR
author: bradygaster
description: SignalR ve ASP.NET Core arasındaki farklar SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963716"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="a5d50-103">ASP.NET SignalR ve ASP.NET Core arasındaki farklar SignalR</span><span class="sxs-lookup"><span data-stu-id="a5d50-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="a5d50-104">ASP.NET Core SignalR, ASP.NET SignalRiçin istemcilerle veya sunucularla uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="a5d50-105">Bu makalede, ASP.NET Core SignalRkaldırılan veya değiştirilen özellikler ayrıntılı olarak anlatılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="a5d50-106">SignalR sürümünü belirleme</span><span class="sxs-lookup"><span data-stu-id="a5d50-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="a5d50-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="a5d50-107">ASP.NET SignalR</span></span> | <span data-ttu-id="a5d50-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a5d50-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="a5d50-109">Sunucu NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="a5d50-109">Server NuGet Package</span></span> | <span data-ttu-id="a5d50-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="a5d50-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="a5d50-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="a5d50-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="a5d50-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="a5d50-113">(.NET Framework)</span></span> |
| <span data-ttu-id="a5d50-114">İstemci NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="a5d50-114">Client NuGet Packages</span></span> | <span data-ttu-id="a5d50-115">[Microsoft. AspNet.SignalR. İstemcilerinin](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="a5d50-116">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="a5d50-117">[Microsoft. AspNetCore.SignalR. İstemcilerinin](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="a5d50-118">İstemci NPM paketi</span><span class="sxs-lookup"><span data-stu-id="a5d50-118">Client npm Package</span></span> | [<span data-ttu-id="a5d50-119">SignalR</span><span class="sxs-lookup"><span data-stu-id="a5d50-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="a5d50-120">Java Istemcisi</span><span class="sxs-lookup"><span data-stu-id="a5d50-120">Java Client</span></span> | <span data-ttu-id="a5d50-121">[GitHub deposu](https://github.com/SignalR/java-client) (kullanım dışı)</span><span class="sxs-lookup"><span data-stu-id="a5d50-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="a5d50-122">Maven paketi [com. Microsoft. SignalR](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="a5d50-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="a5d50-123">Sunucu uygulaması türü</span><span class="sxs-lookup"><span data-stu-id="a5d50-123">Server App Type</span></span> | <span data-ttu-id="a5d50-124">ASP.NET (System. Web) veya OWıN Self-Host</span><span class="sxs-lookup"><span data-stu-id="a5d50-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="a5d50-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5d50-125">ASP.NET Core</span></span> |
| <span data-ttu-id="a5d50-126">Desteklenen sunucu platformları</span><span class="sxs-lookup"><span data-stu-id="a5d50-126">Supported Server Platforms</span></span> | <span data-ttu-id="a5d50-127">.NET Framework 4,5 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a5d50-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="a5d50-128">.NET Framework 4.6.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a5d50-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="a5d50-129">.NET Core 2,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a5d50-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="a5d50-130">Özellik farklılıkları</span><span class="sxs-lookup"><span data-stu-id="a5d50-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="a5d50-131">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="a5d50-131">Automatic reconnects</span></span>

<span data-ttu-id="a5d50-132">Otomatik yeniden bağlantılar ASP.NET Core SignalRdesteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a5d50-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="a5d50-133">İstemcinin bağlantısı kesilirse, yeniden bağlanmak istiyorlarsa kullanıcının açıkça yeni bir bağlantı başlatması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="a5d50-134">ASP.NET SignalR' de, bağlantı kesildiğinde SignalR sunucuya yeniden bağlanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="a5d50-135">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="a5d50-135">Protocol support</span></span>

<span data-ttu-id="a5d50-136">ASP.NET Core SignalR, JSON 'ı ve [MessagePack](xref:signalr/messagepackhubprotocol)tabanlı yeni bir ikili protokolü destekler.</span><span class="sxs-lookup"><span data-stu-id="a5d50-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="a5d50-137">Ek olarak özel protokoller de oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="a5d50-138">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="a5d50-138">Transports</span></span>

<span data-ttu-id="a5d50-139">ASP.NET Core SignalRiçin süresiz çerçeve taşıması desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a5d50-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="a5d50-140">Sunucu farkları</span><span class="sxs-lookup"><span data-stu-id="a5d50-140">Differences on the server</span></span>

<span data-ttu-id="a5d50-141">ASP.NET Core SignalR sunucu tarafı kitaplıkları, hem Razor hem de MVC projeleri için **ASP.NET Core Web uygulaması** şablonunun parçası olan [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) paketine dahildir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="a5d50-142">ASP.NET Core SignalR ASP.NET Core bir ara yazılım olduğundan, `Startup.ConfigureServices`içinde [Addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) çağırarak yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a5d50-143">Yönlendirmeyi yapılandırmak için, yolları `Startup.Configure` yönteminde [Useendpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) yöntemi çağrısının içindeki hub 'lara eşleyin.</span><span class="sxs-lookup"><span data-stu-id="a5d50-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="a5d50-144">Yönlendirmeyi yapılandırmak için, yolları `Startup.Configure` yöntemi içindeki [Usesignalr](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntemi çağrısının içindeki hublara eşleyin.</span><span class="sxs-lookup"><span data-stu-id="a5d50-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="a5d50-145">Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="a5d50-145">Sticky sessions</span></span>

<span data-ttu-id="a5d50-146">ASP.NET SignalR için genişleme modeli, istemcilerin gruptaki herhangi bir sunucuya yeniden bağlanmasını ve iletileri göndermesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="a5d50-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="a5d50-147">ASP.NET Core SignalR, istemci bağlantı süresince aynı sunucu ile etkileşime sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="a5d50-148">Redsıs kullanarak genişleme için, bu, yapışkan oturumların gerekli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="a5d50-149">[Azure SignalR hizmetini](/azure/azure-signalr/)kullanarak genişleme için, hizmet istemcilerle bağlantıları işlediği için yapışkan oturumlar gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="a5d50-150">Bağlantı başına tek hub</span><span class="sxs-lookup"><span data-stu-id="a5d50-150">Single hub per connection</span></span>

<span data-ttu-id="a5d50-151">ASP.NET Core SignalR, bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="a5d50-152">Bağlantılar, birden çok hub 'a erişimi paylaşmak için kullanılan tek bir bağlantı yerine doğrudan tek bir hub 'a yapılır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="a5d50-153">Akış</span><span class="sxs-lookup"><span data-stu-id="a5d50-153">Streaming</span></span>

<span data-ttu-id="a5d50-154">ASP.NET Core SignalR artık hub 'dan istemciye [veri akışını](xref:signalr/streaming) desteklemektedir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="a5d50-155">Durum</span><span class="sxs-lookup"><span data-stu-id="a5d50-155">State</span></span>

<span data-ttu-id="a5d50-156">İstemcilerle Hub (genellikle HubState olarak adlandırılır) arasında rastgele durum geçirme özelliği kaldırılmıştır ve ilerleme durumu iletileri için destek de sağlar.</span><span class="sxs-lookup"><span data-stu-id="a5d50-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="a5d50-157">Şu anda hub proxy 'lerinin karşılığı yok.</span><span class="sxs-lookup"><span data-stu-id="a5d50-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="a5d50-158">PersistentConnection kaldırma</span><span class="sxs-lookup"><span data-stu-id="a5d50-158">PersistentConnection removal</span></span>

<span data-ttu-id="a5d50-159">ASP.NET Core SignalR, [Persistentconnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="a5d50-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="a5d50-160">GlobalHost</span></span>

<span data-ttu-id="a5d50-161">ASP.NET Core çerçeveye yerleşik bağımlılık ekleme (dı) vardır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="a5d50-162">Hizmetler, [Hubcontext](xref:signalr/hubcontext)'e erışmek için dı kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="a5d50-163">`HubContext` almak için ASP.NET SignalR içinde kullanılan `GlobalHost` nesnesi ASP.NET Core SignalRyok.</span><span class="sxs-lookup"><span data-stu-id="a5d50-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="a5d50-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="a5d50-164">HubPipeline</span></span>

<span data-ttu-id="a5d50-165">ASP.NET Core SignalR `HubPipeline` modül desteğine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="a5d50-166">İstemci farkları</span><span class="sxs-lookup"><span data-stu-id="a5d50-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="a5d50-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="a5d50-167">TypeScript</span></span>

<span data-ttu-id="a5d50-168">ASP.NET Core SignalR istemcisi [TypeScript](https://www.typescriptlang.org/)'te yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a5d50-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="a5d50-169">[JavaScript istemcisini](xref:signalr/javascript-client)kullanırken JavaScript veya TypeScript ile yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a5d50-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="a5d50-170">JavaScript istemcisi [NPM](https://www.npmjs.com/) 'de barındırılıyor</span><span class="sxs-lookup"><span data-stu-id="a5d50-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="a5d50-171">Önceki sürümlerde JavaScript istemcisi, Visual Studio 'da bir NuGet paketi aracılığıyla elde edildi.</span><span class="sxs-lookup"><span data-stu-id="a5d50-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="a5d50-172">Temel sürümler için [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) NPM paketi Javascript kitaplıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="a5d50-173">Bu paket, **ASP.NET Core Web uygulaması** şablonuna dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a5d50-174">`@aspnet/signalr` NPM paketini edinmek ve yüklemek için NPM kullanın.</span><span class="sxs-lookup"><span data-stu-id="a5d50-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="a5d50-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="a5d50-175">jQuery</span></span>

<span data-ttu-id="a5d50-176">JQuery üzerindeki bağımlılık kaldırılmıştır, ancak projeler jQuery kullanmaya devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="a5d50-177">Internet Explorer desteği</span><span class="sxs-lookup"><span data-stu-id="a5d50-177">Internet Explorer support</span></span>

<span data-ttu-id="a5d50-178">ASP.NET Core SignalR Microsoft Internet Explorer 11 veya üstünü gerektirir (ASP.NET SignalR desteklenen Microsoft Internet Explorer 8 ve üzeri).</span><span class="sxs-lookup"><span data-stu-id="a5d50-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="a5d50-179">JavaScript istemci yöntemi sözdizimi</span><span class="sxs-lookup"><span data-stu-id="a5d50-179">JavaScript client method syntax</span></span>

<span data-ttu-id="a5d50-180">JavaScript sözdizimi, SignalRönceki sürümünden değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="a5d50-181">`$connection` nesnesini kullanmak yerine, [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API 'sini kullanarak bir bağlantı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a5d50-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="a5d50-182">Hub 'ın çağırakullanabileceği istemci yöntemlerini belirtmek için [on](/javascript/api/@aspnet/signalr/HubConnection#on) metodunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a5d50-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="a5d50-183">İstemci yöntemini oluşturduktan sonra hub bağlantısını başlatın.</span><span class="sxs-lookup"><span data-stu-id="a5d50-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="a5d50-184">Günlüğe kaydetmek veya hataları işlemek için bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodunu zincirleyin.</span><span class="sxs-lookup"><span data-stu-id="a5d50-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="a5d50-185">Hub proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="a5d50-185">Hub proxies</span></span>

<span data-ttu-id="a5d50-186">Hub proxy 'leri artık otomatik olarak oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="a5d50-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="a5d50-187">Bunun yerine, yöntem adı [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API 'sine bir dize olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="a5d50-188">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="a5d50-188">.NET and other clients</span></span>

<span data-ttu-id="a5d50-189">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR.NET istemci kitaplıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="a5d50-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="a5d50-190">Hub 'a bir bağlantı örneği oluşturmak ve derlemek için [Hubconnectionbuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a5d50-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="a5d50-191">Ölçek Genişletme farklılıkları</span><span class="sxs-lookup"><span data-stu-id="a5d50-191">Scaleout differences</span></span>

<span data-ttu-id="a5d50-192">ASP.NET SignalR SQL Server ve Redsıs 'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="a5d50-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="a5d50-193">ASP.NET Core SignalR Azure SignalR hizmeti 'ni ve Redsıs 'yi destekler.</span><span class="sxs-lookup"><span data-stu-id="a5d50-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="a5d50-194">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a5d50-194">ASP.NET</span></span>

* <span data-ttu-id="a5d50-195">[Azure Service Bus ile ölçeği SignalR](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="a5d50-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="a5d50-196">[Redsıs ile SignalR ölçeği](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="a5d50-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="a5d50-197">[SQL Server ile ölçeği SignalR](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="a5d50-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="a5d50-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5d50-198">ASP.NET Core</span></span>

* <span data-ttu-id="a5d50-199">[Azure SignalR hizmeti](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="a5d50-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="a5d50-200">Redsıs geri düzlemi</span><span class="sxs-lookup"><span data-stu-id="a5d50-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="a5d50-201">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a5d50-201">Additional resources</span></span>

* [<span data-ttu-id="a5d50-202">Merkezler</span><span class="sxs-lookup"><span data-stu-id="a5d50-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a5d50-203">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="a5d50-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a5d50-204">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="a5d50-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a5d50-205">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="a5d50-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
