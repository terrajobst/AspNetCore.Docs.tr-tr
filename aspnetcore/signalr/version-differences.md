---
title: SignalR ve ASP.NET Core SignalR arasındaki farklar
author: tdykstra
description: SignalR ve ASP.NET Core SignalR arasındaki farklar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095014"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="aa11f-103">SignalR ve ASP.NET Core SignalR arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="aa11f-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="aa11f-104">ASP.NET Core SignalR istemciler veya sunucular için ASP.NET SignalR ile uyumlu değil.</span><span class="sxs-lookup"><span data-stu-id="aa11f-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="aa11f-105">Bu makalede kaldırılmış veya ASP.NET Core SignalR içinde değiştirilen özellikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="aa11f-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="aa11f-106">Özellik farkları</span><span class="sxs-lookup"><span data-stu-id="aa11f-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="aa11f-107">Otomatik yeniden bağlantılar</span><span class="sxs-lookup"><span data-stu-id="aa11f-107">Automatic reconnects</span></span>

<span data-ttu-id="aa11f-108">Otomatik yeniden bağlantılar artık desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="aa11f-109">Daha önce SignalR bağlantı kesildi, sunucuya yeniden denedi.</span><span class="sxs-lookup"><span data-stu-id="aa11f-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="aa11f-110">İstemci bağlantısı kesilmişse şimdi yeniden bağlamak isterseniz kullanıcı yeni bir bağlantı açıkça başlatmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="aa11f-111">Protokol desteği</span><span class="sxs-lookup"><span data-stu-id="aa11f-111">Protocol support</span></span>

<span data-ttu-id="aa11f-112">ASP.NET Core Signalr'yi destekleyen dayalı yeni bir ikili Protokolü yanı sıra JSON [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="aa11f-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="aa11f-113">Ayrıca, özel protokoller oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="aa11f-114">Sunucuda farkları</span><span class="sxs-lookup"><span data-stu-id="aa11f-114">Differences on the server</span></span>

<span data-ttu-id="aa11f-115">SignalR sunucu tarafı kitaplıklar dahil `Microsoft.AspNetCore.App` parçası olan paket **ASP.NET Core Web uygulaması** Razor hem MVC projeleri için şablon.</span><span class="sxs-lookup"><span data-stu-id="aa11f-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="aa11f-116">SignalR çağırarak yapılandırılmalıdır bir ASP.NET Core ara yazılımını olduğundan `AddSignalR` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="aa11f-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="aa11f-117">Yönlendirmeyi yapılandırmak için eşleme rotaları hub içinde `UseSignalR` yöntem çağrısı `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="aa11f-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="aa11f-118">Artık gerekli Yapışkan oturumlar</span><span class="sxs-lookup"><span data-stu-id="aa11f-118">Sticky sessions now required</span></span>

<span data-ttu-id="aa11f-119">Ölçek genişletme SignalR önceki sürümlerinde nasıl çalışılan nedeniyle, istemciler yeniden ve gruptaki herhangi bir sunucuya ileti göndermek.</span><span class="sxs-lookup"><span data-stu-id="aa11f-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="aa11f-120">Ölçek genişletme modeli, hem de yeniden bağlantılar desteklemediğinden değişiklikler nedeniyle, bu artık desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="aa11f-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="aa11f-121">Şimdi, istemcinin sunucuya bağlanan sonra ile aynı sunucuya bağlantı süresi boyunca etkileşim kurmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="aa11f-122">Bağlantı başına tek bir hub</span><span class="sxs-lookup"><span data-stu-id="aa11f-122">Single hub per connection</span></span>

<span data-ttu-id="aa11f-123">ASP.NET Core SignalR öğesinde bağlantı modeli basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="aa11f-124">Bağlantıları, doğrudan erişim birden çok hub'a paylaşmak için kullanılan tek bir bağlantı yerine tek bir hub için gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="aa11f-125">Akış</span><span class="sxs-lookup"><span data-stu-id="aa11f-125">Streaming</span></span>

<span data-ttu-id="aa11f-126">SignalR destekler [akış verileri](xref:signalr/streaming) istemciye hub'ından.</span><span class="sxs-lookup"><span data-stu-id="aa11f-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="aa11f-127">Durum</span><span class="sxs-lookup"><span data-stu-id="aa11f-127">State</span></span>

<span data-ttu-id="aa11f-128">İlerleme durumu iletileri için destek yanı sıra hub'ı (HubState olarak da adlandırılır) ve istemciler arasında rastgele bir durum geçirme özelliği kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="aa11f-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="aa11f-129">Şu anda hiçbir hub proxy karşılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="aa11f-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="aa11f-130">İstemci üzerinde farkları</span><span class="sxs-lookup"><span data-stu-id="aa11f-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="aa11f-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="aa11f-131">TypeScript</span></span>

<span data-ttu-id="aa11f-132">SignalR ASP.NET Core sürümü yazılır [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="aa11f-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="aa11f-133">JavaScript veya TypeScript kullanırken yazabileceğiniz [JavaScript istemci](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="aa11f-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="aa11f-134">JavaScript istemcisi, barındırılan [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="aa11f-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="aa11f-135">Önceki sürümlerde, Visual Studio'da bir NuGet paketi aracılığıyla JavaScript istemci alındı.</span><span class="sxs-lookup"><span data-stu-id="aa11f-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="aa11f-136">Çekirdek sürümleri için [ @aspnet/signalr npm paketini](https://www.npmjs.com/package/@aspnet/signalr) JavaScript kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="aa11f-137">Bu paket bulunup bulunmadığına **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="aa11f-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="aa11f-138">Elde etmek ve yüklemek için npm kullanın `@aspnet/signalr` npm paketi.</span><span class="sxs-lookup"><span data-stu-id="aa11f-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="aa11f-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="aa11f-139">jQuery</span></span>

<span data-ttu-id="aa11f-140">JQuery bağımlı projeler jQuery kullanmaya devam edebilirsiniz ancak kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="aa11f-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="aa11f-141">JavaScript istemci yöntem sözdizimi</span><span class="sxs-lookup"><span data-stu-id="aa11f-141">JavaScript client method syntax</span></span>

<span data-ttu-id="aa11f-142">JavaScript sözdizimi SignalR önceki sürümünden değişmiştir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="aa11f-143">Yerine `$connection` nesne, bir bağlantı kullanarak oluşturduğunuz `HubConnectionBuilder` API.</span><span class="sxs-lookup"><span data-stu-id="aa11f-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="aa11f-144">Kullanım `connection.on` hub çağırabilirsiniz istemci yöntemlerini belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="aa11f-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="aa11f-145">İstemci yöntemi oluşturduktan sonra hub Bağlantısı'nı başlatın.</span><span class="sxs-lookup"><span data-stu-id="aa11f-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="aa11f-146">Zincir bir `catch` oturum veya hataları işlemek için bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="aa11f-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="aa11f-147">Hub proxy</span><span class="sxs-lookup"><span data-stu-id="aa11f-147">Hub proxies</span></span>

<span data-ttu-id="aa11f-148">Hub proxy artık otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aa11f-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="aa11f-149">Bunun yerine, yöntem adı yöntemlere geçirilen `invoke` dize olarak API.</span><span class="sxs-lookup"><span data-stu-id="aa11f-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="aa11f-150">.NET ve diğer istemciler</span><span class="sxs-lookup"><span data-stu-id="aa11f-150">.NET and other clients</span></span>

<span data-ttu-id="aa11f-151">`Microsoft.AspNetCore.SignalR.Client` NuGet paketi, ASP.NET Core SignalR için .NET istemci kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="aa11f-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="aa11f-152">Kullanım `HubConnectionBuilder` ve hub'ına bağlantı örneğini oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="aa11f-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="aa11f-153">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="aa11f-153">Additional resources</span></span>

* [<span data-ttu-id="aa11f-154">Merkezler</span><span class="sxs-lookup"><span data-stu-id="aa11f-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="aa11f-155">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="aa11f-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="aa11f-156">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="aa11f-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="aa11f-157">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="aa11f-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
