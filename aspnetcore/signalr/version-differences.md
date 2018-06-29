---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: rachelappel
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090182"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="3b14b-103">SignalR ve ASP.NET Core SignalR arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="3b14b-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="3b14b-104">ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="3b14b-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="3b14b-105">Bu makalede kaldırılmış veya ASP.NET Core SignalR değiştirilmiş özellikleri ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3b14b-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="3b14b-106">Özellik farklılıkları</span><span class="sxs-lookup"><span data-stu-id="3b14b-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="3b14b-107">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="3b14b-107">Automatic reconnects</span></span>

<span data-ttu-id="3b14b-108">Otomatik yeniden bağlantılar artık desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="3b14b-109">Daha önce SignalR bağlantı kesildi durumunda sunucuya yeniden denedi.</span><span class="sxs-lookup"><span data-stu-id="3b14b-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="3b14b-110">İstemci bağlantısı kesilmişse şimdi yeniden bağlamak isterseniz kullanıcı yeni bir bağlantı açıkça başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="3b14b-111">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="3b14b-111">Protocol support</span></span>

<span data-ttu-id="3b14b-112">ASP.NET Core SignalR destekleyen dayalı yeni bir ikili Protokolü yanı sıra, JSON [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="3b14b-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="3b14b-113">Ayrıca, özel protokoller oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="3b14b-114">Sunucu üzerindeki farklar</span><span class="sxs-lookup"><span data-stu-id="3b14b-114">Differences on the server</span></span>

<span data-ttu-id="3b14b-115">SignalR sunucu tarafı kitaplıkları içinde yer alan `Microsoft.AspNetCore.App` parçası olan paket **ASP.NET çekirdek Web uygulaması** Razor ve MVC projeler için şablon.</span><span class="sxs-lookup"><span data-stu-id="3b14b-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="3b14b-116">SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan `AddSignalR` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b14b-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="3b14b-117">Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde `UseSignalR` yöntem çağrısı `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3b14b-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="3b14b-118">Artık gerekli Yapışkan oturumları</span><span class="sxs-lookup"><span data-stu-id="3b14b-118">Sticky sessions now required</span></span>

<span data-ttu-id="3b14b-119">Genişleme SignalR önceki sürümlerinde nasıl çalışılan nedeniyle, istemciler yeniden bağlanın ve iletileri gruptaki herhangi bir sunucuya gönderir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="3b14b-120">Genişleme modeli yanı sıra yeniden bağlantılar desteklemediğinden değişiklikler nedeniyle, bu artık desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="3b14b-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="3b14b-121">Şimdi, istemci, sunucuya bağlanır sonra bağlantı boyunca aynı sunucusu ile etkileşim gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3b14b-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="3b14b-122">Bağlantı başına tek hub</span><span class="sxs-lookup"><span data-stu-id="3b14b-122">Single hub per connection</span></span>

<span data-ttu-id="3b14b-123">ASP.NET Core SignalR bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="3b14b-124">Bağlantılar, çok sayıda hub erişimi paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için doğrudan yapılır.</span><span class="sxs-lookup"><span data-stu-id="3b14b-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="3b14b-125">Akış</span><span class="sxs-lookup"><span data-stu-id="3b14b-125">Streaming</span></span>

<span data-ttu-id="3b14b-126">SignalR destekler [veri akış](xref:signalr/streaming) istemciye bu hub'dan.</span><span class="sxs-lookup"><span data-stu-id="3b14b-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="3b14b-127">Durum</span><span class="sxs-lookup"><span data-stu-id="3b14b-127">State</span></span>

<span data-ttu-id="3b14b-128">İlerleme durumu iletileri için destek yanı sıra rasgele durumu (HubState olarak da adlandırılır) hub ve istemciler arasında geçirmek olanağı kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3b14b-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="3b14b-129">Şu anda hiçbir karşılık gelen hub proxy yoktur.</span><span class="sxs-lookup"><span data-stu-id="3b14b-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="3b14b-130">İstemci üzerinde farklar</span><span class="sxs-lookup"><span data-stu-id="3b14b-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="3b14b-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="3b14b-131">TypeScript</span></span>

<span data-ttu-id="3b14b-132">SignalR ASP.NET Core sürümü yazılmış [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="3b14b-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="3b14b-133">Kullanırken, JavaScript veya TypeScript yazabilirsiniz [JavaScript istemci](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="3b14b-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="3b14b-134">JavaScript istemci konumunda barındırılan [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="3b14b-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="3b14b-135">Önceki sürümlerde, NuGet paketini Visual Studio'da JavaScript istemci edindiğiniz.</span><span class="sxs-lookup"><span data-stu-id="3b14b-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="3b14b-136">Çekirdek sürümleri için [ @aspnet/signalr npm paket](https://www.npmjs.com/package/@aspnet/signalr) JavaScript kitaplıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="3b14b-137">Bu paket bulunup **ASP.NET çekirdek Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="3b14b-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="3b14b-138">Edinme ve yükleme npm kullanmak `@aspnet/signalr` npm paket.</span><span class="sxs-lookup"><span data-stu-id="3b14b-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="3b14b-139">JQuery</span><span class="sxs-lookup"><span data-stu-id="3b14b-139">jQuery</span></span>

<span data-ttu-id="3b14b-140">Projeleri jQuery kullanmaya devam edebilirsiniz ancak jQuery bağımlılığını kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3b14b-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="3b14b-141">JavaScript istemci yöntem sözdizimi</span><span class="sxs-lookup"><span data-stu-id="3b14b-141">JavaScript client method syntax</span></span>

<span data-ttu-id="3b14b-142">JavaScript sözdizimi SignalR önceki sürümünden değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="3b14b-143">Kullanarak yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="3b14b-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="3b14b-144">Kullanım `connection.on` hub çağırabilirsiniz istemci yöntemlerini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="3b14b-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="3b14b-145">İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="3b14b-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="3b14b-146">Zincir bir `catch` oturum veya hataları işlemek için bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="3b14b-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="3b14b-147">Hub proxy</span><span class="sxs-lookup"><span data-stu-id="3b14b-147">Hub proxies</span></span>

<span data-ttu-id="3b14b-148">Hub proxy artık otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3b14b-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="3b14b-149">Yöntem adı içine bunun yerine, geçirilen `invoke` dize olarak API.</span><span class="sxs-lookup"><span data-stu-id="3b14b-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="3b14b-150">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="3b14b-150">.NET and other clients</span></span>

<span data-ttu-id="3b14b-151">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3b14b-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="3b14b-152">Kullanım `HubConnectionBuilder` ve hub'ına bağlantısı örneği oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="3b14b-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="3b14b-153">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3b14b-153">Additional resources</span></span>

* [<span data-ttu-id="3b14b-154">Merkezler</span><span class="sxs-lookup"><span data-stu-id="3b14b-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3b14b-155">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="3b14b-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3b14b-156">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="3b14b-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3b14b-157">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="3b14b-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
