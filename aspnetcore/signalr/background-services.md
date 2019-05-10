---
title: Konak ASP.NET Core SignalR içinde arka plan Hizmetleri
author: bradygaster
description: .NET Core BackgroundService sınıflardan SignalR istemcilere göndermek nasıl öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087748"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="d037c-103">Konak ASP.NET Core SignalR içinde arka plan Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="d037c-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="d037c-104">Tarafından [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="d037c-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="d037c-105">Bu makale için yönergeler sağlar:</span><span class="sxs-lookup"><span data-stu-id="d037c-105">This article provides guidance for:</span></span>

* <span data-ttu-id="d037c-106">SignalR hub'larını kullanarak ASP.NET Core ile barındırılan bir arka plan çalışan işlemi barındırma.</span><span class="sxs-lookup"><span data-stu-id="d037c-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="d037c-107">İleti gönderilmesi bağlı .NET Core içinde istemcilerden [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="d037c-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="d037c-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d037c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="d037c-109">SignalR başlatma sırasında bağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d037c-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="d037c-110">ASP.NET Core SignalR hub'ları barındıran bir arka plan çalışan işleminin bağlamında, bir ASP.NET Core web uygulamasında bir hub'ı barındırmak için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="d037c-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="d037c-111">İçinde `Startup.ConfigureServices` yöntemi, arama `services.AddSignalR` gerekli hizmetler SignalR desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanı ekler.</span><span class="sxs-lookup"><span data-stu-id="d037c-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="d037c-112">İçinde `Startup.Configure`, `UseSignalR` yöntemi çağrıldığında kablo Hub uç noktalarına ASP.NET Core istek ardışık düzeninde'kurmak için.</span><span class="sxs-lookup"><span data-stu-id="d037c-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="d037c-113">Önceki örnekte `ClockHub` sınıfının Implements `Hub<T>` kesin türü belirtilmiş bir Hub oluşturmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="d037c-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="d037c-114">`ClockHub` Yapılandırılmış `Startup` uç noktasında isteklerini yanıtlamak için sınıf `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="d037c-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="d037c-115">Kesin türü belirtilmiş hub'ları hakkında daha fazla bilgi için bkz. [ASP.NET Core için içinde SignalR hub'larını kullanma](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="d037c-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="d037c-116">Bu işlev için sınırlı değildir [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d037c-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="d037c-117">Devralınan herhangi bir sınıf [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), gibi [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), aynı zamanda çalışır.</span><span class="sxs-lookup"><span data-stu-id="d037c-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="d037c-118">Kesin türü belirtilmiş tarafından kullanılan arabirimi `ClockHub` olduğu `IClock` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="d037c-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="d037c-119">Arka plan hizmetinden bir SignalR hub'ı çağırın</span><span class="sxs-lookup"><span data-stu-id="d037c-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="d037c-120">Başlatma sırasında `Worker` sınıfı, bir `BackgroundService`, kullanılarak kablolu `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="d037c-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="d037c-121">SignalR ayrıca yedekleme sırasında bağlı olduğundan `Startup` aşama, hangi her Hub ASP.NET Core'nın HTTP istek işlem hattında tek bir uç nokta bağlı olarak, her Hub tarafından temsil edilen bir `IHubContext<T>` sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="d037c-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="d037c-122">ASP.NET Core'nın kullanarak DI özellikleri, barındırma katmanı tarafından gibi örneği diğer sınıflar `BackgroundService` sınıf, MVC denetleyici sınıflarına ya da Razor sayfası modelleri başvurular alma sunucu taraflı hub'lara örneklerini kabul ederek `IHubContext<ClockHub, IClock>` oluşturma sırasında.</span><span class="sxs-lookup"><span data-stu-id="d037c-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="d037c-123">Olarak `ExecuteAsync` yöntemi arka plan hizmetinde tekrarlayarak çağrıldığında, sunucunun geçerli tarih ve saat kullanarak bağlı istemcilere gönderilen `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="d037c-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="d037c-124">Arka plan Hizmetleri ile SignalR olaylara tepki verin</span><span class="sxs-lookup"><span data-stu-id="d037c-124">React to SignalR events with background services</span></span>

<span data-ttu-id="d037c-125">SignalR veya .NET masaüstü uygulaması için JavaScript istemcisi kullanarak tek sayfa uygulaması gibi kullanarak <xref:signalr/dotnet-client>, `BackgroundService` veya `IHostedService` uygulama ayrıca SignalR hub'larını bağlamak ve olaylarına yanıt vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d037c-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="d037c-126">`ClockHubClient` Sınıfı her ikisini birden uygular `IClock` arabirimi ve `IHostedService` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="d037c-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="d037c-127">Bu yolla bunu kablolu yedekleme sırasında `Startup` sürekli olarak çalıştırın ve sunucudan Hub olaylarına yanıt vermek için.</span><span class="sxs-lookup"><span data-stu-id="d037c-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="d037c-128">Başlatma sırasında `ClockHubClient` örneği oluşturur bir `HubConnection` ve yukarı bağlayan `IClock.ShowTime` Hub'ın işleyici olarak yöntemi `ShowTime` olay.</span><span class="sxs-lookup"><span data-stu-id="d037c-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="d037c-129">İçinde `IHostedService.StartAsync` uygulaması `HubConnection` zaman uyumsuz olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="d037c-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="d037c-130">Sırasında `IHostedService.StopAsync` yöntemi `HubConnection` zaman uyumsuz olarak kapatılır.</span><span class="sxs-lookup"><span data-stu-id="d037c-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="d037c-131">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d037c-131">Additional resources</span></span>

* [<span data-ttu-id="d037c-132">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d037c-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="d037c-133">Merkezler</span><span class="sxs-lookup"><span data-stu-id="d037c-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d037c-134">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="d037c-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="d037c-135">Kesin türü belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="d037c-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
