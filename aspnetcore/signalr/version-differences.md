---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: tdykstra
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: c9302f1c9e7cd4e62eaeaef871feb54ef26aa3ca
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708419"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="56794-103">ASP.NET SignalR ile ASP.NET Core SignalR arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="56794-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="56794-104">ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="56794-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="56794-105">Bu makalede, kaldırılmış veya ASP.NET Core SignalR öğesinde değiştirilen özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="56794-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="56794-106">SignalR sürüm belirleme</span><span class="sxs-lookup"><span data-stu-id="56794-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="56794-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="56794-107">ASP.NET SignalR</span></span> | <span data-ttu-id="56794-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="56794-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="56794-109">Sunucu NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="56794-109">Server NuGet Package</span></span> | [<span data-ttu-id="56794-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="56794-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="56794-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="56794-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="56794-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="56794-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="56794-113">İstemci NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="56794-113">Client NuGet Packages</span></span> | [<span data-ttu-id="56794-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="56794-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="56794-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="56794-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="56794-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="56794-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="56794-117">İstemci npm paket</span><span class="sxs-lookup"><span data-stu-id="56794-117">Client npm Package</span></span> | [<span data-ttu-id="56794-118">signalr</span><span class="sxs-lookup"><span data-stu-id="56794-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="56794-119">Sunucu uygulama türü</span><span class="sxs-lookup"><span data-stu-id="56794-119">Server App Type</span></span> | <span data-ttu-id="56794-120">ASP.NET (System.Web) veya OWIN barındırma</span><span class="sxs-lookup"><span data-stu-id="56794-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="56794-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56794-121">ASP.NET Core</span></span> |
| <span data-ttu-id="56794-122">Desteklenen sunucu platformları</span><span class="sxs-lookup"><span data-stu-id="56794-122">Supported Server Platforms</span></span> | <span data-ttu-id="56794-123">.NET framework 4.5 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="56794-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="56794-124">.NET framework 4.6.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="56794-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="56794-125">.NET core 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="56794-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="56794-126">Özellik farkları</span><span class="sxs-lookup"><span data-stu-id="56794-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="56794-127">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="56794-127">Automatic reconnects</span></span>

<span data-ttu-id="56794-128">ASP.NET Core SignalR öğesinde otomatik yeniden bağlantılar desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="56794-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="56794-129">İstemci bağlantısı kesilmişse, kullanıcı yeniden bağlamak isterseniz yeni bir bağlantı açıkça başlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="56794-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="56794-130">ASP.NET SignalR SignalR bağlantısı kesilirse, sunucuya yeniden dener.</span><span class="sxs-lookup"><span data-stu-id="56794-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="56794-131">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="56794-131">Protocol support</span></span>

<span data-ttu-id="56794-132">ASP.NET Core Signalr'yi destekleyen dayalı yeni bir ikili Protokolü yanı sıra JSON [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="56794-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="56794-133">Ayrıca, özel protokoller oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="56794-133">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="56794-134">Taşımalar</span><span class="sxs-lookup"><span data-stu-id="56794-134">Transports</span></span>

<span data-ttu-id="56794-135">ASP.NET Core SignalR öğesinde sonsuza kadar çerçeve taşıma desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="56794-135">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="56794-136">Sunucuda farkları</span><span class="sxs-lookup"><span data-stu-id="56794-136">Differences on the server</span></span>

<span data-ttu-id="56794-137">ASP.NET Core SignalR sunucu tarafı kitaplıklar dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) parçası olan paket **ASP.NET Core Web uygulaması** Razor hem MVC şablonu projeleri.</span><span class="sxs-lookup"><span data-stu-id="56794-137">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="56794-138">ASP.NET Core SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="56794-138">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="56794-139">Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntem çağrısı `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56794-139">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="56794-140">Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="56794-140">Sticky sessions</span></span>

<span data-ttu-id="56794-141">ASP.NET SignalR ölçeğini genişletme modeli yeniden ve gruptaki herhangi bir sunucuya ileti gönderme etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="56794-141">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="56794-142">ASP.NET Core SignalR öğesinde, istemci ile aynı sunucuya bağlantı süresi boyunca etkileşim kurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56794-142">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="56794-143">Redis kullanarak genişletme için Yapışkan oturumlar gerekli olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="56794-143">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="56794-144">Genişletme kullanma [Azure SignalR hizmeti](/azure/azure-signalr/), Yapışkan oturumlar bulunmamaktır hizmet bağlantıları istemcilere hallettiğinden.</span><span class="sxs-lookup"><span data-stu-id="56794-144">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="56794-145">Bağlantı başına tek bir hub</span><span class="sxs-lookup"><span data-stu-id="56794-145">Single hub per connection</span></span>

<span data-ttu-id="56794-146">ASP.NET Core SignalR öğesinde bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="56794-146">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="56794-147">Bağlantıları, doğrudan erişim birden çok hub'a paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="56794-147">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="56794-148">Akış</span><span class="sxs-lookup"><span data-stu-id="56794-148">Streaming</span></span>

<span data-ttu-id="56794-149">ASP.NET Core SignalR destekler [akış verileri](xref:signalr/streaming) istemciye hub'ından.</span><span class="sxs-lookup"><span data-stu-id="56794-149">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="56794-150">Durum</span><span class="sxs-lookup"><span data-stu-id="56794-150">State</span></span>

<span data-ttu-id="56794-151">İlerleme durumu iletileri için destek yanı sıra hub'ı (HubState olarak da adlandırılır) ve istemciler arasında rastgele bir durum geçirme özelliği kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="56794-151">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="56794-152">Şu anda hiçbir hub proxy karşılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="56794-152">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="56794-153">PersistentConnection kaldırma</span><span class="sxs-lookup"><span data-stu-id="56794-153">PersistentConnection removal</span></span>

<span data-ttu-id="56794-154">ASP.NET Core signalr'da [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) sınıfı kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="56794-154">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span> 

### <a name="globalhost"></a><span data-ttu-id="56794-155">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="56794-155">GlobalHost</span></span>

<span data-ttu-id="56794-156">ASP.NET Core Framework'e yerleşik bağımlılık ekleme (dı) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="56794-156">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="56794-157">Hizmetleri DI erişmek için kullanabileceğiniz [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="56794-157">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="56794-158">`GlobalHost` ASP.NET SignalR öğesinde almak için kullanılan nesne bir `HubContext` ASP.NET Core SignalR öğesinde yok.</span><span class="sxs-lookup"><span data-stu-id="56794-158">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="56794-159">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="56794-159">HubPipeline</span></span>

<span data-ttu-id="56794-160">ASP.NET Core SignalR için destek yok `HubPipeline` modüller.</span><span class="sxs-lookup"><span data-stu-id="56794-160">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="56794-161">İstemci üzerinde farkları</span><span class="sxs-lookup"><span data-stu-id="56794-161">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="56794-162">TypeScript</span><span class="sxs-lookup"><span data-stu-id="56794-162">TypeScript</span></span>

<span data-ttu-id="56794-163">ASP.NET Core SignalR istemci yazıldığı [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="56794-163">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="56794-164">JavaScript veya TypeScript kullanırken yazabileceğiniz [JavaScript istemci](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="56794-164">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="56794-165">JavaScript istemcisi, barındırılan [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="56794-165">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="56794-166">Önceki sürümlerde, Visual Studio'da bir NuGet paketi aracılığıyla JavaScript istemci alındı.</span><span class="sxs-lookup"><span data-stu-id="56794-166">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="56794-167">Çekirdek sürümleri için [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm paket JavaScript kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="56794-167">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="56794-168">Bu paket bulunup bulunmadığına **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="56794-168">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="56794-169">Elde etmek ve yüklemek için npm kullanın `@aspnet/signalr` npm paketi.</span><span class="sxs-lookup"><span data-stu-id="56794-169">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="56794-170">jQuery</span><span class="sxs-lookup"><span data-stu-id="56794-170">jQuery</span></span>

<span data-ttu-id="56794-171">JQuery bağımlı projeler jQuery kullanmaya devam edebilirsiniz ancak kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="56794-171">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="56794-172">Internet Explorer desteği</span><span class="sxs-lookup"><span data-stu-id="56794-172">Internet Explorer support</span></span>

<span data-ttu-id="56794-173">ASP.NET Core SignalR (ASP.NET SignalR, Microsoft Internet Explorer 8 ve üzeri desteklenir) Microsoft Internet Explorer 11 veya üstü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="56794-173">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="56794-174">JavaScript istemci yöntem sözdizimi</span><span class="sxs-lookup"><span data-stu-id="56794-174">JavaScript client method syntax</span></span>

<span data-ttu-id="56794-175">JavaScript sözdizimi SignalR önceki sürümünden değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="56794-175">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="56794-176">Yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="56794-176">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="56794-177">Kullanım [üzerinde](/javascript/api/@aspnet/signalr/HubConnection#on) hub çağırabilirsiniz istemci yöntemlerini belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="56794-177">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="56794-178">İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="56794-178">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="56794-179">Zincir bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) oturum veya hataları işlemek için bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="56794-179">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="56794-180">Hub proxy</span><span class="sxs-lookup"><span data-stu-id="56794-180">Hub proxies</span></span>

<span data-ttu-id="56794-181">Hub proxy artık otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="56794-181">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="56794-182">Bunun yerine, yöntem adı yöntemlere geçirilen [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) dize olarak API.</span><span class="sxs-lookup"><span data-stu-id="56794-182">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="56794-183">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="56794-183">.NET and other clients</span></span>

<span data-ttu-id="56794-184">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="56794-184">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="56794-185">Kullanım [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ve hub'ına bağlantı örneğini oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="56794-185">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="56794-186">Genişletme farkları</span><span class="sxs-lookup"><span data-stu-id="56794-186">Scaleout differences</span></span>

<span data-ttu-id="56794-187">ASP.NET SignalR, SQL Server ve Redis destekler.</span><span class="sxs-lookup"><span data-stu-id="56794-187">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="56794-188">Azure SignalR hizmeti ve Redis ASP.NET Core SignalR destekler.</span><span class="sxs-lookup"><span data-stu-id="56794-188">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="56794-189">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="56794-189">ASP.NET</span></span>

* [<span data-ttu-id="56794-190">Azure Service Bus ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="56794-190">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="56794-191">Redis ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="56794-191">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="56794-192">SQL Server ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="56794-192">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="56794-193">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56794-193">ASP.NET Core</span></span>

* [<span data-ttu-id="56794-194">Azure SignalR hizmeti</span><span class="sxs-lookup"><span data-stu-id="56794-194">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="56794-195">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="56794-195">Additional resources</span></span>

* [<span data-ttu-id="56794-196">Merkezler</span><span class="sxs-lookup"><span data-stu-id="56794-196">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="56794-197">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="56794-197">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="56794-198">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="56794-198">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="56794-199">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="56794-199">Supported platforms</span></span>](xref:signalr/supported-platforms)
