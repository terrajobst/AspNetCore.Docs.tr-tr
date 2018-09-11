---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: tdykstra
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 2f3458f27fd7f22339751e0734dd8c5da709a3c0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340127"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="3fb2f-103">ASP.NET SignalR ile ASP.NET Core SignalR arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="3fb2f-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="3fb2f-104">ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="3fb2f-105">Bu makalede, kaldırılmış veya ASP.NET Core SignalR öğesinde değiştirilen özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="3fb2f-106">SignalR sürüm belirleme</span><span class="sxs-lookup"><span data-stu-id="3fb2f-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="3fb2f-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="3fb2f-107">ASP.NET SignalR</span></span> | <span data-ttu-id="3fb2f-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3fb2f-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="3fb2f-109">Sunucu NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="3fb2f-109">Server NuGet Package</span></span> | [<span data-ttu-id="3fb2f-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="3fb2f-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="3fb2f-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="3fb2f-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="3fb2f-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="3fb2f-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="3fb2f-113">İstemci NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="3fb2f-113">Client NuGet Packages</span></span> | [<span data-ttu-id="3fb2f-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="3fb2f-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="3fb2f-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="3fb2f-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="3fb2f-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="3fb2f-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="3fb2f-117">İstemci npm paket</span><span class="sxs-lookup"><span data-stu-id="3fb2f-117">Client npm Package</span></span> | [<span data-ttu-id="3fb2f-118">signalr</span><span class="sxs-lookup"><span data-stu-id="3fb2f-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="3fb2f-119">Sunucu uygulama türü</span><span class="sxs-lookup"><span data-stu-id="3fb2f-119">Server App Type</span></span> | <span data-ttu-id="3fb2f-120">ASP.NET (System.Web) veya OWIN barındırma</span><span class="sxs-lookup"><span data-stu-id="3fb2f-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="3fb2f-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3fb2f-121">ASP.NET Core</span></span> |
| <span data-ttu-id="3fb2f-122">Desteklenen sunucu platformları</span><span class="sxs-lookup"><span data-stu-id="3fb2f-122">Supported Server Platforms</span></span> | <span data-ttu-id="3fb2f-123">.NET framework 4.5 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3fb2f-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="3fb2f-124">.NET framework 4.6.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3fb2f-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="3fb2f-125">.NET core 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3fb2f-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="3fb2f-126">Özellik farkları</span><span class="sxs-lookup"><span data-stu-id="3fb2f-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="3fb2f-127">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="3fb2f-127">Automatic reconnects</span></span>

<span data-ttu-id="3fb2f-128">Otomatik yeniden bağlantılar artık desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-128">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="3fb2f-129">Daha önce SignalR bağlantı kesildi, sunucuya yeniden denedi.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-129">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="3fb2f-130">İstemci bağlantısı kesilmişse şimdi yeniden bağlamak isterseniz kullanıcı yeni bir bağlantı açıkça başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-130">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="3fb2f-131">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="3fb2f-131">Protocol support</span></span>

<span data-ttu-id="3fb2f-132">ASP.NET Core Signalr'yi destekleyen dayalı yeni bir ikili Protokolü yanı sıra JSON [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="3fb2f-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="3fb2f-133">Ayrıca, özel protokoller oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="3fb2f-134">Sunucuda farkları</span><span class="sxs-lookup"><span data-stu-id="3fb2f-134">Differences on the server</span></span>

<span data-ttu-id="3fb2f-135">ASP.NET Core SignalR sunucu tarafı kitaplıklar dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) parçası olan paket **ASP.NET Core Web uygulaması** Razor hem MVC şablonu projeleri.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="3fb2f-136">ASP.NET Core SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="3fb2f-137">Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) yöntem çağrısı `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="3fb2f-138">Artık gerekli Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="3fb2f-138">Sticky sessions now required</span></span>

<span data-ttu-id="3fb2f-139">Ölçek genişletme ASP.NET SignalR öğesinde nasıl çalışılan nedeniyle, istemciler yeniden ve gruptaki herhangi bir sunucuya ileti göndermek.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="3fb2f-140">Ölçek genişletme modeli, hem de yeniden bağlantılar desteklemediğinden değişiklikler nedeniyle, bu artık desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="3fb2f-141">İstemci, sunucuya bağlanır. sonra bağlantının süresi boyunca ile aynı sunucuya etkileşim kurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="3fb2f-142">Bağlantı başına tek bir hub</span><span class="sxs-lookup"><span data-stu-id="3fb2f-142">Single hub per connection</span></span>

<span data-ttu-id="3fb2f-143">ASP.NET Core SignalR öğesinde bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="3fb2f-144">Bağlantıları, doğrudan erişim birden çok hub'a paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="3fb2f-145">Akış</span><span class="sxs-lookup"><span data-stu-id="3fb2f-145">Streaming</span></span>

<span data-ttu-id="3fb2f-146">ASP.NET Core SignalR destekler [akış verileri](xref:signalr/streaming) istemciye hub'ından.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="3fb2f-147">Durum</span><span class="sxs-lookup"><span data-stu-id="3fb2f-147">State</span></span>

<span data-ttu-id="3fb2f-148">İlerleme durumu iletileri için destek yanı sıra hub'ı (HubState olarak da adlandırılır) ve istemciler arasında rastgele bir durum geçirme özelliği kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="3fb2f-149">Şu anda hiçbir hub proxy karşılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="3fb2f-150">İstemci üzerinde farkları</span><span class="sxs-lookup"><span data-stu-id="3fb2f-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="3fb2f-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="3fb2f-151">TypeScript</span></span>

<span data-ttu-id="3fb2f-152">ASP.NET Core SignalR istemci yazıldığı [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="3fb2f-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="3fb2f-153">JavaScript veya TypeScript kullanırken yazabileceğiniz [JavaScript istemci](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="3fb2f-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="3fb2f-154">JavaScript istemcisi, barındırılan [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="3fb2f-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="3fb2f-155">Önceki sürümlerde, Visual Studio'da bir NuGet paketi aracılığıyla JavaScript istemci alındı.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="3fb2f-156">Çekirdek sürümleri için [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) npm paket JavaScript kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="3fb2f-157">Bu paket bulunup bulunmadığına **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="3fb2f-158">Elde etmek ve yüklemek için npm kullanın `@aspnet/signalr` npm paketi.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="3fb2f-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="3fb2f-159">jQuery</span></span>

<span data-ttu-id="3fb2f-160">JQuery bağımlı projeler jQuery kullanmaya devam edebilirsiniz ancak kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="3fb2f-161">JavaScript istemci yöntem sözdizimi</span><span class="sxs-lookup"><span data-stu-id="3fb2f-161">JavaScript client method syntax</span></span>

<span data-ttu-id="3fb2f-162">JavaScript sözdizimi SignalR önceki sürümünden değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="3fb2f-163">Yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="3fb2f-164">Kullanım [üzerinde](/javascript/api/@aspnet/signalr/HubConnection#on) hub çağırabilirsiniz istemci yöntemlerini belirtmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="3fb2f-165">İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="3fb2f-166">Zincir bir [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) oturum veya hataları işlemek için bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="3fb2f-167">Hub proxy</span><span class="sxs-lookup"><span data-stu-id="3fb2f-167">Hub proxies</span></span>

<span data-ttu-id="3fb2f-168">Hub proxy artık otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="3fb2f-169">Bunun yerine, yöntem adı yöntemlere geçirilen [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) dize olarak API.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="3fb2f-170">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="3fb2f-170">.NET and other clients</span></span>

<span data-ttu-id="3fb2f-171">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="3fb2f-172">Kullanım [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) ve hub'ına bağlantı örneğini oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="3fb2f-173">Genişletme farkları</span><span class="sxs-lookup"><span data-stu-id="3fb2f-173">Scaleout differences</span></span>

<span data-ttu-id="3fb2f-174">ASP.NET SignalR, hem SQL Server hem de Redis destekler.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-174">ASP.NET SignalR supports both SQL Server and Redis.</span></span> <span data-ttu-id="3fb2f-175">ASP.NET Core SignalR, hem Azure SignalR hizmeti hem de Redis destekler.</span><span class="sxs-lookup"><span data-stu-id="3fb2f-175">ASP.NET Core SignalR supports both Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="3fb2f-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3fb2f-176">ASP.NET</span></span>

* [<span data-ttu-id="3fb2f-177">Azure Service Bus ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="3fb2f-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="3fb2f-178">Redis ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="3fb2f-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="3fb2f-179">SQL Server ile SignalR ölçeğini genişletme</span><span class="sxs-lookup"><span data-stu-id="3fb2f-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="3fb2f-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3fb2f-180">ASP.NET Core</span></span>

* [<span data-ttu-id="3fb2f-181">Azure SignalR hizmeti</span><span class="sxs-lookup"><span data-stu-id="3fb2f-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="3fb2f-182">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3fb2f-182">Additional resources</span></span>

* [<span data-ttu-id="3fb2f-183">Merkezler</span><span class="sxs-lookup"><span data-stu-id="3fb2f-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3fb2f-184">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="3fb2f-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3fb2f-185">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="3fb2f-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3fb2f-186">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="3fb2f-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
