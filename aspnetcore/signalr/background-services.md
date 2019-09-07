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
# <a name="host-aspnet-core-signalr-in-background-services"></a>Arka plan hizmetlerinde konak ASP.NET Core SignalR

, [Brady Gaster](https://twitter.com/bradygaster) tarafından

Bu makalede şu yönergelere kılavuzluk sunulmaktadır:

* ASP.NET Core ile barındırılan bir arka plan çalışan işlemini kullanarak SignalR hub 'Ları barındırma.
* .NET Core [Backgroundservice](xref:Microsoft.Extensions.Hosting.BackgroundService)içinden bağlı istemcilere ileti gönderme.

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(indirme)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Başlatma sırasında SignalR 'yi tel

::: moniker range=">= aspnetcore-3.0"

Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Larını barındırmak, bir hub 'ı ASP.NET Core Web uygulamasında barındırmakla aynıdır. Yönteminde, çağrısı `services.AddSignalR` , SignalR 'yi desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına gerekli hizmetleri ekler. `Startup.ConfigureServices` ' De `Startup.Configure`, `UseEndpoints` yöntemi, ASP.NET Core isteği ardışık düzeninde Merkez uç noktaları arasında bağlantı kurmak için geri çağırmada `MapHub` çağrılır.

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

Arka plan çalışan işleminin bağlamında ASP.NET Core SignalR hub 'Larını barındırmak, bir hub 'ı ASP.NET Core Web uygulamasında barındırmakla aynıdır. Yönteminde, çağrısı `services.AddSignalR` , SignalR 'yi desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanına gerekli hizmetleri ekler. `Startup.ConfigureServices` ' De `Startup.Configure`, yöntemi,ASP.NETCoreisteğiardışıkdüzenindeMerkezuçnoktalarıiçinçağrılır.`UseSignalR`

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

Önceki örnekte, `ClockHub` sınıfı türü kesin belirlenmiş bir hub `Hub<T>` oluşturmak için sınıfı uygular. , `ClockHub` `Startup` Sınıfında, uç noktasındaki `/hubs/clock`isteklere yanıt vermek için yapılandırılmıştır.

Türü kesin belirlenmiş hub 'Lar hakkında daha fazla bilgi için bkz. [SignalR 'de hub 'Ları kullanma ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Bu işlevsellik, [\<hub t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfıyla sınırlı değildir. [Dynamichub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)gibi [hub](xref:Microsoft.AspNetCore.SignalR.Hub)'dan devralan tüm sınıflar da çalışır.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Kesin olarak yazılan `ClockHub` `IClock` tarafından kullanılan arabirim arabirimidir.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Arka plan hizmetinden bir SignalR hub 'ı çağırma

Başlangıç `Worker` sırasında, a `BackgroundService`sınıfı kullanılarak `AddHostedService`bağlanır.

```csharp
services.AddHostedService<Worker>();
```

SignalR, `Startup` aşama sırasında da kablolu olduğundan, her hub 'ın ASP.NET Core http isteği ardışık düzeninde tek bir uç noktaya eklendiği için, her hub sunucu üzerinde bir `IHubContext<T>` ile temsil edilir. ASP.NET Core dı özelliklerini kullanarak, sınıflar, MVC denetleyici sınıfları veya Razor sayfa modelleri gibi `BackgroundService` barındırma katmanı tarafından oluşturulan diğer sınıflar, oluşturma `IHubContext<ClockHub, IClock>` sırasında örneklerini kabul ederek sunucu tarafı hub 'larına başvurular alabilir.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Yöntemi arka plan hizmetinde yinelemeli olarak çağrıldığı için sunucunun geçerli tarih ve saati `ClockHub`kullanılarak bağlı istemcilere gönderilir. `ExecuteAsync`

## <a name="react-to-signalr-events-with-background-services"></a>Arka plan hizmetleriyle SignalR olaylarına tepki verme

SignalR için JavaScript istemcisini veya bir .net masaüstü uygulamasını <xref:signalr/dotnet-client>kullanan tek sayfalı bir uygulama gibi kullanarak, SignalR hub 'larına bağlanmak ve olaylara yanıt vermek için bir `BackgroundService` veya `IHostedService` uygulaması da kullanılabilir.

Sınıfı hem arabirimini hem de arabirimini uygular. `IHostedService` `ClockHubClient` `IClock` Bu sayede, sürekli olarak çalıştırılmak ve sunucudan `Startup` hub olaylarına yanıt vermek için bu şekilde kablolu olarak erişilebilir.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Başlatma sırasında,, `ClockHubClient` bir öğesinin örneğini oluşturur ve `HubConnection` hub 'ın `ShowTime` olayının işleyicisi `IClock.ShowTime` olarak yöntemi bir kablo olarak oluşturur.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

Uygulamada, zaman uyumsuz `HubConnection` olarak başlatılır. `IHostedService.StartAsync`

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

`IHostedService.StopAsync` Yöntemi`HubConnection` sırasında, zaman uyumsuz olarak bırakıldı.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Türü kesin belirlenmiş hub 'Lar](xref:signalr/hubs#strongly-typed-hubs)
