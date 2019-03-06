---
title: ASP.NET Core SignalR öğesinde akışı
author: bradygaster
description: Sunucu hub yöntemleri akışları değerleri döndürür ve .NET ve JavaScript istemcileri kullanarak akışları kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345593"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR öğesinde akışı

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler. Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır. Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Hub'ı ayarladınız

::: moniker range=">= aspnetcore-3.0"

Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, veya `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 3.0 veya sonraki sürümlerde, hub yöntemlerini akış döndürebilir `IAsyncEnumerable<T>` ek olarak `ChannelReader<T>`. Döndürülecek en basit yolu `IAsyncEnumerable<T>` aşağıdaki örnekte gösterildiği gibi hub yönteminin zaman uyumsuz bir yineleyici yöntem tarafından sunuluyor. Hub zaman uyumsuz yineleyicinin yöntemler kabul edebileceği bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre. Zaman uyumsuz yineleyicinin yöntemler kolayca döndürmez gibi kanallar ile ortak sorunları önlemek `ChannelReader` kadar erken veya tamamlamadan yönteminden çıkılıyor `ChannelWriter`.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Aşağıdaki örnek, akış kanalları kullanarak bir istemciye ilişkin temel bilgileri gösterir. Her bir nesne yazılır `ChannelWriter` söz konusu nesne hemen istemciye gönderilir. Sonunda, `ChannelWriter` Akış kapalı istemci bildirmek için tamamlandı.

> [!NOTE]
> * Yazma `ChannelWriter` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk. Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.
> * Mantığınızda kaydırma bir `try ... catch` ve tamamlayın `Channel` yakalama ve hub emin olmak için yakalama dışında yöntemi çağırma düzgün bir şekilde tamamlandı.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

ASP.NET Core 2.2 veya sonraki sürümlerde, hub yöntemlerini akış kabul edebilen bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre. Sunucu işlemi durdurmak ve akışın sonuna önce istemcinin bağlantısı kesilirse tüm kaynakları serbest bırakmak için bu belirteci kullanın.

::: moniker-end

## <a name="net-client"></a>.NET istemcisi

`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır. Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`. Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir. A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder. Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir. Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
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

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a>Java istemcisi
SignalR Java istemcinin kullandığı `stream` akış yöntemlerini çağırmak için yöntem. Bu, üç veya daha fazla bağımsız değişken kabul eder:

* Akış öğeleri beklenen tür 
* Hub yönteminin adı.
* Hub yönteminin içinde tanımlanan bir bağımsız değişken. 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
`stream` Metodunda `HubConnection` Observable akış öğesi türünü döndürür. Gözlemlenebilir türün `subscribe` yöntemdir tanımladığınız yerlerde, `onNext`, `onError` ve `onCompleted` işleyicileri.

::: moniker-end

## <a name="related-resources"></a>İlgili kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
