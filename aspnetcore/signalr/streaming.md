---
title: ASP.NET Core SignalR akış kullanın
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 08ddea4fb83150bab27a9e2685c75ff34565606b
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327499"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR akış kullanın

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR sunucu yöntemlerin akış dönüş değerlerini destekler. Bu, zaman içinde veri parçalarını burada gelecektir senaryoları için kullanışlıdır. Dönüş değeri istemciye akışı, bu duruma hemen her parça istemciye kullanılabilir yerine tüm veriler kullanılabilir duruma gelmesi bekleniyor gönderilir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Hub'ı ayarlama

Bir hub yöntemini, geri döndüğünde otomatik olarak bir akış hub yöntemi olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`. Aşağıda istemciye veri akış temelleri gösteren bir örnektir. Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilebilir. Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.

> [!NOTE]
> Yazma `ChannelReader` arka plan iş parçacığı ve return `ChannelReader` mümkün olan en kısa sürede. Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET istemci

`StreamAsChannelAsync` Yöntemi `HubConnection` akış yöntemini çağırmak için kullanılır. Hub yönteminin adını ve hub yönteminin tanımlanan bağımsız değişkenler geçirmek `StreamAsChannelAsync`. Genel parametresini `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir. A `ChannelReader<T>` akış çağrısından döndürülen ve istemci akışta temsil eder. Verileri okumak için genel bir desen üzerinden döngü oluşturmaktır `WaitToReadAsync` ve arama `TryRead` veri olduğunda kullanılabilir. Döngü akış sunucusu tarafından kapatıldı veya iptal belirteci geçirilen sona erer `StreamAsChannelAsync` iptal edilir.

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

## <a name="javascript-client"></a>JavaScript istemci

JavaScript istemcileri aramak akış yöntemleri hub'larında kullanarak `connection.stream`. `stream` Yöntemi iki bağımsız değişken kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub yöntemi addır `Counter`.
* Hub yönteminin tanımlanmış bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkenler: almak için akış öğelerini ve akış öğeler arasındaki gecikme sayısı için bir sayı.

`connection.stream` döndüren bir `IStreamResult` içeren bir `subscribe` yöntemi. Geçirmek bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

İstemci çağrısı akıştan sonuna `dispose` yöntemi `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.

## <a name="related-resources"></a>İlgili kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
