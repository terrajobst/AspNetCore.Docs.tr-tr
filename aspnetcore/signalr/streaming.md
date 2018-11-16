---
title: ASP.NET Core SignalR öğesinde akışı
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708445"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR öğesinde akışı

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler. Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır. Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Hub'ı ayarladınız

Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`. Akış verileri istemciye temelleri gösteren bir örnek aşağıdadır. Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilir. Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.

> [!NOTE]
> Yazma `ChannelReader` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk. Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> ASP.NET Core 2.2 veya sonraki sürümlerde, Hub yöntemlerini akış kabul edebilen bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre. Sunucu işlemi durdurmak ve akışın sonuna önce istemcinin bağlantısı kesilirse tüm kaynakları serbest bırakmak için bu belirteci kullanın.

::: moniker-end

## <a name="net-client"></a>.NET istemci

`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır. Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`. Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir. A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder. Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir. Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

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

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

::: moniker-end

## <a name="javascript-client"></a>JavaScript istemcisi

JavaScript istemciler çağrı akış yöntemleri hub'ları kullanarak `connection.stream`. `stream` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub'yöntemini addır `Counter`.
* Hub yönteminin içinde tanımlanan bir bağımsız değişken. Aşağıdaki örnekte, bağımsız değişkenleri şunlardır: akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısı için bir sayı.

`connection.stream` döndürür bir `IStreamResult` içeren bir `subscribe` yöntemi. Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi. Bu yöntem neden arama `CancellationToken` (bir sağlandıysa) Hub yönteminin parametresi iptal edilecek.

::: moniker-end

## <a name="related-resources"></a>İlgili kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
