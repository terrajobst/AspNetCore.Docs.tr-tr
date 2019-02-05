---
title: Konak ASP.NET Core SignalR içinde arka plan Hizmetleri
author: bradygaster
description: .NET Core BackgroundService sınıflardan SignalR istemcilere göndermek nasıl öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739676"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Konak ASP.NET Core SignalR içinde arka plan Hizmetleri

Tarafından [Brady Gaster](https://twitter.com/bradygaster)

Bu makale için yönergeler sağlar:

* SignalR hub'larını kullanarak ASP.NET Core ile barındırılan bir arka plan çalışan işlemi barındırma.
* İleti gönderilmesi bağlı .NET Core içinde istemcilerden [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>SignalR başlatma sırasında bağlayabilirsiniz.

ASP.NET Core SignalR hub'ları barındıran bir arka plan çalışan işleminin bağlamında, bir ASP.NET Core web uygulamasında bir hub'ı barındırmak için aynıdır. İçinde `Startup.ConfigureServices` yöntemi, arama `services.AddSignalR` gerekli hizmetler SignalR desteklemek için ASP.NET Core bağımlılık ekleme (dı) katmanı ekler. İçinde `Startup.Configure`, `UseSignalR` yöntemi çağrıldığında kablo Hub uç noktalarına ASP.NET Core istek ardışık düzeninde'kurmak için.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

Önceki örnekte `ClockHub` sınıfının Implements `Hub<T>` kesin türü belirtilmiş bir Hub oluşturmak için sınıf. `ClockHub` Yapılandırılmış `Startup` uç noktasında isteklerini yanıtlamak için sınıf `/hubs/clock`.

Kesin türü belirtilmiş hub'ları hakkında daha fazla bilgi için bkz. [ASP.NET Core için içinde SignalR hub'larını kullanma](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Bu işlev için sınırlı değildir [Hub\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) sınıfı. Devralınan herhangi bir sınıf [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), gibi [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), aynı zamanda çalışır.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Kesin türü belirtilmiş tarafından kullanılan arabirimi `ClockHub` olduğu `IClock` arabirimi.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Arka plan hizmetinden bir SignalR hub'ı çağırın

Başlatma sırasında `Worker` sınıfı, bir `BackgroundService`, kullanılarak kablolu `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

SignalR ayrıca yedekleme sırasında bağlı olduğundan `Startup` aşama, hangi her Hub ASP.NET Core'nın HTTP istek işlem hattında tek bir uç nokta bağlı olarak, her Hub tarafından temsil edilen bir `IHubContext<T>` sunucusunda. ASP.NET Core'nın kullanarak DI özellikleri, barındırma katmanı tarafından gibi örneği diğer sınıflar `BackgroundService` sınıf, MVC denetleyici sınıflarına ya da Razor sayfası modelleri başvurular alma sunucu taraflı hub'lara örneklerini kabul ederek `IHubContext<ClockHub, IClock>` oluşturma sırasında.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Olarak `ExecuteAsync` yöntemi arka plan hizmetinde tekrarlayarak çağrıldığında, sunucunun geçerli tarih ve saat kullanarak bağlı istemcilere gönderilen `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Arka plan Hizmetleri ile SignalR olaylara tepki verin

SignalR veya .NET masaüstü uygulaması için JavaScript istemcisi kullanarak tek sayfa uygulaması gibi kullanarak <xref:signalr/dotnet-client>, `BackgroundService` veya `IHostedService` uygulama ayrıca SignalR hub'larını bağlamak ve olaylarına yanıt vermek için kullanılabilir.

`ClockHubClient` Sınıfı her ikisini birden uygular `IClock` arabirimi ve `IHostedService` arabirimi. Bu yolla bunu kablolu yedekleme sırasında `Startup` sürekli olarak çalıştırın ve sunucudan Hub olaylarına yanıt vermek için. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Başlatma sırasında `ClockHubClient` örneği oluşturur bir `HubConnection` ve yukarı bağlayan `IClock.ShowTime` Hub'ın işleyici olarak yöntemi `ShowTime` olay.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

İçinde `IHostedService.StartAsync` uygulaması `HubConnection` zaman uyumsuz olarak başlatılır.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Sırasında `IHostedService.StopAsync` yöntemi `HubConnection` zaman uyumsuz olarak kapatılır.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Kesin türü belirtilmiş hub'ları](xref:signalr/hubs#strongly-typed-hubs)
