---
title: ASP.NET Core SignalR öğesinde akışı
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 0001eed830249ac46ba35331759187bb4e7e8fd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095268"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR öğesinde akışı

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler. Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır. Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Hub'ı ayarladınız

Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`. Akış verileri istemciye temelleri gösteren bir örnek aşağıdadır. Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilir. Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.

> [!NOTE]
> Yazma `ChannelReader` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk. Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET istemci

`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır. Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`. Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir. A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder. Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir. Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.

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

## <a name="javascript-client"></a>JavaScript istemcisi

JavaScript istemciler çağrı akış yöntemleri hub'ları kullanarak `connection.stream`. `stream` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub'yöntemini addır `Counter`.
* Hub yönteminin içinde tanımlanan bir bağımsız değişken. Aşağıdaki örnekte, bağımsız değişkenleri şunlardır: akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısı için bir sayı.

`connection.stream` döndürür bir `IStreamResult` içeren bir `subscribe` yöntemi. Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

İstemci çağrısı akıştan sonuna `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.

## <a name="related-resources"></a>İlgili kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
