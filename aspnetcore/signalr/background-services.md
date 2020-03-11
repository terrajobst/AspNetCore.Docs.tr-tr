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
ms.openlocfilehash: 86319cc93febab18c29e2fb6366cef0d025943ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658146"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>Arka plan hizmetlerinde ana bilgisayar ASP.NET Core SignalR

, [Brady Gaster](https://twitter.com/bradygaster) tarafından

Bu makalede şu yönergelere kılavuzluk sunulmaktadır:

* ASP.NET Core ile barındırılan arka plan çalışan işlemini kullanarak SignalR hub 'Ları barındırma.
* .NET Core [Backgroundservice](xref:Microsoft.Extensions.Hosting.BackgroundService)içinden bağlı istemcilere ileti gönderme.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)

## <a name="enable-opno-locsignalr-in-startup"></a>Başlangıçta SignalR etkinleştir

::: moniker range=">= aspnetcore-3.0"

Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Ları barındırmak, bir ASP.NET Core Web uygulamasındaki hub barındırmakla aynıdır. `Startup.ConfigureServices` yönteminde, `services.AddSignalR` çağırmak gereken hizmetleri SignalRdesteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına ekler. `Startup.Configure`, `MapHub` yöntemi, ASP.NET Core isteği ardışık düzeninde hub uç noktalarına bağlanmak için `UseEndpoints` geri çağırmada çağrılır.

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

Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Ları barındırmak, bir ASP.NET Core Web uygulamasındaki hub barındırmakla aynıdır. `Startup.ConfigureServices` yönteminde, `services.AddSignalR` çağırmak gereken hizmetleri SignalRdesteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına ekler. `Startup.Configure`, `UseSignalR` yöntemi, ASP.NET Core isteği ardışık düzeninde hub uç noktaları bağlamak için çağrılır.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Yukarıdaki örnekte `ClockHub` sınıfı, türü kesin belirlenmiş bir hub oluşturmak için `Hub<T>` sınıfını uygular. `ClockHub`, uç nokta `/hubs/clock`isteklere yanıt vermek için `Startup` sınıfında yapılandırıldı.

Türü kesin belirlenmiş hub 'Lar hakkında daha fazla bilgi için bkz. [SignalR hub 'ları ASP.NET Core Için kullanma](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Bu işlevsellik, [Hub\<t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfıyla sınırlı değildir. [Dynamichub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)gibi [hub](xref:Microsoft.AspNetCore.SignalR.Hub)'dan devralan tüm sınıflar da çalışır.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Türü kesin belirlenmiş `ClockHub` tarafından kullanılan arabirim `IClock` arabirimidir.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Arka plan hizmetinden bir SignalR hub 'ı çağırma

Başlangıç sırasında, `BackgroundService``Worker` sınıfı `AddHostedService`kullanılarak etkinleştirilir.

```csharp
services.AddHostedService<Worker>();
```

SignalR, her hub 'ın ASP.NET Core HTTP isteği ardışık düzeninde tek bir uç noktaya eklendiği `Startup` aşamasında de etkinleştirildiğinden, her hub sunucu üzerinde bir `IHubContext<T>` temsil edilir. ASP.NET Core dı özelliklerini kullanarak, barındırma katmanı tarafından oluşturulan `BackgroundService` sınıflar, MVC denetleyici sınıfları veya Razor sayfa modelleri gibi diğer sınıflar, oluşturma sırasında `IHubContext<ClockHub, IClock>` örneklerini kabul ederek sunucu tarafı hub 'Larına başvurular alabilir.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

`ExecuteAsync` yöntemi arka plan hizmetinde yinelemeli olarak çağrıldığı için, sunucunun geçerli tarih ve saati `ClockHub`kullanılarak bağlı istemcilere gönderilir.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Arka plan hizmetleriyle SignalR olaylara tepki verme

SignalR veya bir .NET masaüstü uygulamasının JavaScript istemcisini kullanan tek sayfalı bir uygulama gibi <xref:signalr/dotnet-client>kullanarak kullanarak `BackgroundService` veya `IHostedService` bir uygulama da SignalR hub 'Larına bağlanıp olaylara yanıt vermek için kullanılabilir.

`ClockHubClient` sınıfı hem `IClock` arabirimini hem de `IHostedService` arabirimini uygular. Bu sayede `Startup`, sürekli olarak çalışacak ve sunucudan hub olaylarına yanıt veren bir şekilde etkinleştirilebilir.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Başlatma sırasında `ClockHubClient`, bir `HubConnection` örneğini oluşturur ve hub 'ın `ShowTime` olayı için işleyici olarak `IClock.ShowTime` yöntemini sunar.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync` uygulamasında `HubConnection` zaman uyumsuz olarak başlatılır.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

`IHostedService.StopAsync` yöntemi sırasında, `HubConnection` zaman uyumsuz olarak bırakıldı.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Başlarken](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Türü kesin belirlenmiş hub 'Lar](xref:signalr/hubs#strongly-typed-hubs)
