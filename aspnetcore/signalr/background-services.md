---
title: Arka plan hizmetlerinde konak ASP.NET Core SignalR
author: bradygaster
description: .NET Core BackgroundService sınıflarından SignalR istemcilerine ileti gönderme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773946"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="bf0da-103">Arka plan hizmetlerinde konak ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bf0da-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="bf0da-104">, [Brady Gaster](https://twitter.com/bradygaster) tarafından</span><span class="sxs-lookup"><span data-stu-id="bf0da-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="bf0da-105">Bu makalede şu yönergelere kılavuzluk sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="bf0da-105">This article provides guidance for:</span></span>

* <span data-ttu-id="bf0da-106">ASP.NET Core ile barındırılan bir arka plan çalışan işlemini kullanarak SignalR hub 'Ları barındırma.</span><span class="sxs-lookup"><span data-stu-id="bf0da-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="bf0da-107">.NET Core [Backgroundservice](xref:Microsoft.Extensions.Hosting.BackgroundService)içinden bağlı istemcilere ileti gönderme.</span><span class="sxs-lookup"><span data-stu-id="bf0da-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="bf0da-108">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(indirme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="bf0da-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="bf0da-109">Başlatma sırasında SignalR 'yi tel</span><span class="sxs-lookup"><span data-stu-id="bf0da-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bf0da-110">Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Larını barındırmak, bir hub 'ı ASP.NET Core Web uygulamasında barındırmakla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="bf0da-111">Yönteminde, çağrısı `services.AddSignalR` , SignalR 'yi desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına gerekli hizmetleri ekler. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="bf0da-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="bf0da-112">' De `Startup.Configure`, `UseEndpoints` yöntemi, ASP.NET Core isteği ardışık düzeninde Merkez uç noktaları arasında bağlantı kurmak için geri çağırmada `MapHub` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

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

<span data-ttu-id="bf0da-113">Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Larını barındırmak, bir hub 'ı ASP.NET Core Web uygulamasında barındırmakla aynıdır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="bf0da-114">Yönteminde, çağrısı `services.AddSignalR` , SignalR 'yi desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına gerekli hizmetleri ekler. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="bf0da-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="bf0da-115">' De `Startup.Configure`, yöntemi,ASP.NETCoreisteğiardışıkdüzenindeMerkezuçnoktalarıiçinçağrılır.`UseSignalR`</span><span class="sxs-lookup"><span data-stu-id="bf0da-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="bf0da-116">Önceki örnekte, `ClockHub` sınıfı türü kesin belirlenmiş bir hub `Hub<T>` oluşturmak için sınıfı uygular.</span><span class="sxs-lookup"><span data-stu-id="bf0da-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="bf0da-117">, `ClockHub` `Startup` Sınıfında, uç noktasındaki `/hubs/clock`isteklere yanıt vermek için yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="bf0da-118">Türü kesin belirlenmiş hub 'Lar hakkında daha fazla bilgi için bkz. [SignalR 'de hub 'Ları kullanma ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="bf0da-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="bf0da-119">Bu işlevsellik, [\<hub t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfıyla sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="bf0da-120">[Dynamichub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)gibi [hub](xref:Microsoft.AspNetCore.SignalR.Hub)'dan devralan tüm sınıflar da çalışır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="bf0da-121">Kesin olarak yazılan `ClockHub` `IClock` tarafından kullanılan arabirim arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="bf0da-122">Arka plan hizmetinden bir SignalR hub 'ı çağırma</span><span class="sxs-lookup"><span data-stu-id="bf0da-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="bf0da-123">Başlangıç `Worker` sırasında, a `BackgroundService`sınıfı kullanılarak `AddHostedService`bağlanır.</span><span class="sxs-lookup"><span data-stu-id="bf0da-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="bf0da-124">SignalR, `Startup` aşama sırasında da kablolu olduğundan, her hub 'ın ASP.NET Core http isteği ardışık düzeninde tek bir uç noktaya eklendiği için, her hub sunucu üzerinde bir `IHubContext<T>` ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="bf0da-125">ASP.NET Core dı özelliklerini kullanarak, sınıflar, MVC denetleyici sınıfları veya Razor sayfa modelleri gibi `BackgroundService` barındırma katmanı tarafından oluşturulan diğer sınıflar, oluşturma `IHubContext<ClockHub, IClock>` sırasında örneklerini kabul ederek sunucu tarafı hub 'larına başvurular alabilir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="bf0da-126">Yöntemi arka plan hizmetinde yinelemeli olarak çağrıldığı için sunucunun geçerli tarih ve saati `ClockHub`kullanılarak bağlı istemcilere gönderilir. `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="bf0da-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="bf0da-127">Arka plan hizmetleriyle SignalR olaylarına tepki verme</span><span class="sxs-lookup"><span data-stu-id="bf0da-127">React to SignalR events with background services</span></span>

<span data-ttu-id="bf0da-128">SignalR için JavaScript istemcisini veya bir .net masaüstü uygulamasını <xref:signalr/dotnet-client>kullanan tek sayfalı bir uygulama gibi kullanarak, SignalR hub 'larına bağlanmak ve olaylara yanıt vermek için bir `BackgroundService` veya `IHostedService` uygulaması da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="bf0da-129">Sınıfı hem arabirimini hem de arabirimini uygular. `IHostedService` `ClockHubClient` `IClock`</span><span class="sxs-lookup"><span data-stu-id="bf0da-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="bf0da-130">Bu sayede, sürekli olarak çalıştırılmak ve sunucudan `Startup` hub olaylarına yanıt vermek için bu şekilde kablolu olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bf0da-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="bf0da-131">Başlatma sırasında,, `ClockHubClient` bir öğesinin örneğini oluşturur ve `HubConnection` hub 'ın `ShowTime` olayının işleyicisi `IClock.ShowTime` olarak yöntemi bir kablo olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bf0da-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="bf0da-132">Uygulamada, zaman uyumsuz `HubConnection` olarak başlatılır. `IHostedService.StartAsync`</span><span class="sxs-lookup"><span data-stu-id="bf0da-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="bf0da-133">`IHostedService.StopAsync` Yöntemi`HubConnection` sırasında, zaman uyumsuz olarak bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="bf0da-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="bf0da-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bf0da-134">Additional resources</span></span>

* [<span data-ttu-id="bf0da-135">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="bf0da-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="bf0da-136">Merkezler</span><span class="sxs-lookup"><span data-stu-id="bf0da-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="bf0da-137">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="bf0da-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="bf0da-138">Türü kesin belirlenmiş hub 'Lar</span><span class="sxs-lookup"><span data-stu-id="bf0da-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
