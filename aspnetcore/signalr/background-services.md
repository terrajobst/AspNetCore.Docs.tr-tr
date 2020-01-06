---
title: Arka plan hizmetlerinde ana bilgisayar ASP.NET Core SignalR
author: bradygaster
description: .NET Core BackgroundService sınıflarından SignalR istemcilere ileti gönderme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 324592759af79d1229eb147fb4551e97c678ef64
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358684"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="4db65-103">Arka plan hizmetlerinde ana bilgisayar ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4db65-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="4db65-104">, [Brady Gaster](https://twitter.com/bradygaster) tarafından</span><span class="sxs-lookup"><span data-stu-id="4db65-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="4db65-105">Bu makalede şu yönergelere kılavuzluk sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="4db65-105">This article provides guidance for:</span></span>

* <span data-ttu-id="4db65-106">ASP.NET Core ile barındırılan arka plan çalışan işlemini kullanarak SignalR hub 'Ları barındırma.</span><span class="sxs-lookup"><span data-stu-id="4db65-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="4db65-107">.NET Core [Backgroundservice](xref:Microsoft.Extensions.Hosting.BackgroundService)içinden bağlı istemcilere ileti gönderme.</span><span class="sxs-lookup"><span data-stu-id="4db65-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="4db65-108">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="4db65-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-opno-locsignalr-in-startup"></a><span data-ttu-id="4db65-109">Başlangıçta SignalR etkinleştir</span><span class="sxs-lookup"><span data-stu-id="4db65-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4db65-110">Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Ları barındırmak, bir ASP.NET Core Web uygulamasındaki hub barındırmakla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4db65-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="4db65-111">`Startup.ConfigureServices` yönteminde, `services.AddSignalR` çağırmak gereken hizmetleri SignalRdesteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına ekler.</span><span class="sxs-lookup"><span data-stu-id="4db65-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="4db65-112">`Startup.Configure`, `MapHub` yöntemi, ASP.NET Core isteği ardışık düzeninde hub uç noktalarına bağlanmak için `UseEndpoints` geri çağırmada çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db65-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="4db65-113">Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Ları barındırmak, bir ASP.NET Core Web uygulamasındaki hub barındırmakla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4db65-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="4db65-114">`Startup.ConfigureServices` yönteminde, `services.AddSignalR` çağırmak gereken hizmetleri SignalRdesteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına ekler.</span><span class="sxs-lookup"><span data-stu-id="4db65-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="4db65-115">`Startup.Configure`, `UseSignalR` yöntemi, ASP.NET Core isteği ardışık düzeninde hub uç noktaları bağlamak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db65-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="4db65-116">Yukarıdaki örnekte `ClockHub` sınıfı, türü kesin belirlenmiş bir hub oluşturmak için `Hub<T>` sınıfını uygular.</span><span class="sxs-lookup"><span data-stu-id="4db65-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="4db65-117">`ClockHub`, uç nokta `/hubs/clock`isteklere yanıt vermek için `Startup` sınıfında yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="4db65-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="4db65-118">Türü kesin belirlenmiş hub 'Lar hakkında daha fazla bilgi için bkz. [SignalR hub 'ları ASP.NET Core Için kullanma](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="4db65-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="4db65-119">Bu işlevsellik, [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfıyla sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="4db65-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="4db65-120">[Dynamichub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)gibi [hub](xref:Microsoft.AspNetCore.SignalR.Hub)'dan devralan tüm sınıflar da çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db65-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="4db65-121">Türü kesin belirlenmiş `ClockHub` tarafından kullanılan arabirim `IClock` arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="4db65-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="4db65-122">Arka plan hizmetinden bir SignalR hub 'ı çağırma</span><span class="sxs-lookup"><span data-stu-id="4db65-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="4db65-123">Başlangıç sırasında, `BackgroundService``Worker` sınıfı `AddHostedService`kullanılarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="4db65-124">SignalR, her hub 'ın ASP.NET Core HTTP isteği ardışık düzeninde tek bir uç noktaya eklendiği `Startup` aşamasında de etkinleştirildiğinden, her hub sunucu üzerinde bir `IHubContext<T>` temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="4db65-125">ASP.NET Core dı özelliklerini kullanarak, barındırma katmanı tarafından oluşturulan `BackgroundService` sınıflar, MVC denetleyici sınıfları veya Razor sayfa modelleri gibi diğer sınıflar, oluşturma sırasında `IHubContext<ClockHub, IClock>` örneklerini kabul ederek sunucu tarafı hub 'Larına başvurular alabilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="4db65-126">`ExecuteAsync` yöntemi arka plan hizmetinde yinelemeli olarak çağrıldığı için, sunucunun geçerli tarih ve saati `ClockHub`kullanılarak bağlı istemcilere gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="4db65-127">Arka plan hizmetleriyle SignalR olaylara tepki verme</span><span class="sxs-lookup"><span data-stu-id="4db65-127">React to SignalR events with background services</span></span>

<span data-ttu-id="4db65-128">SignalR veya bir .NET masaüstü uygulamasının JavaScript istemcisini kullanan tek sayfalı bir uygulama gibi <xref:signalr/dotnet-client>kullanarak kullanarak `BackgroundService` veya `IHostedService` bir uygulama da SignalR hub 'Larına bağlanıp olaylara yanıt vermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="4db65-129">`ClockHubClient` sınıfı hem `IClock` arabirimini hem de `IHostedService` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="4db65-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="4db65-130">Bu sayede `Startup`, sürekli olarak çalışacak ve sunucudan hub olaylarına yanıt veren bir şekilde etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4db65-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="4db65-131">Başlatma sırasında `ClockHubClient`, bir `HubConnection` örneğini oluşturur ve hub 'ın `ShowTime` olayı için işleyici olarak `IClock.ShowTime` yöntemini sunar.</span><span class="sxs-lookup"><span data-stu-id="4db65-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="4db65-132">`IHostedService.StartAsync` uygulamasında `HubConnection` zaman uyumsuz olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="4db65-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="4db65-133">`IHostedService.StopAsync` yöntemi sırasında, `HubConnection` zaman uyumsuz olarak bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="4db65-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="4db65-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4db65-134">Additional resources</span></span>

* [<span data-ttu-id="4db65-135">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="4db65-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4db65-136">Merkezler</span><span class="sxs-lookup"><span data-stu-id="4db65-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4db65-137">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="4db65-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="4db65-138">Türü kesin belirlenmiş hub 'Lar</span><span class="sxs-lookup"><span data-stu-id="4db65-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
