---
title: ASP.NET Core SignalR akışı kullanma
author: bradygaster
description: İstemci ve sunucu arasında veri akışını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 7825beba55cefb6236fd8d8e332d030a7e4fc6df
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963893"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>ASP.NET Core SignalR akışı kullanma

[Brennan Conroy](https://github.com/BrennanConroy) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR istemciden sunucuya ve sunucudan istemciye akışı destekler. Bu, veri parçalarının zaman içinde nereden ulaştığını senaryolar için yararlıdır. Akış sırasında her parça, tüm verilerin kullanılabilir hale gelmesini beklemek yerine istemciye veya sunucuya gönderilir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR, sunucu yöntemlerinin akış dönüş değerlerini destekler. Bu, veri parçalarının zaman içinde nereden ulaştığını senaryolar için yararlıdır. Bir dönüş değeri istemciye akışa eklendiğinde, her parça, tüm verilerin kullanılabilir hale gelmesini beklemek yerine istemciye gönderilir.

::: moniker-end

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Akış için bir hub ayarlama

::: moniker range=">= aspnetcore-3.0"

Hub yöntemi, <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`veya `Task<ChannelReader<T>>`döndürdüğünde otomatik olarak bir akış hub yöntemi haline gelir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir hub yöntemi, bir <xref:System.Threading.Channels.ChannelReader%601> veya `Task<ChannelReader<T>>`döndürdüğünde otomatik olarak akış hub yöntemi haline gelir.

::: moniker-end

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış

::: moniker range=">= aspnetcore-3.0"

Akış hub 'ı yöntemleri, `ChannelReader<T>`ek olarak `IAsyncEnumerable<T>` döndürebilir. `IAsyncEnumerable<T>` döndürmenin en kolay yolu, aşağıdaki örnekte gösterildiği gibi hub yöntemini zaman uyumsuz bir yineleyici yöntemi yaparak kullanmaktır. Merkez zaman uyumsuz Yineleyici yöntemleri, istemci akıştan aboneliği kaldırdığında tetiklenen bir `CancellationToken` parametresi kabul edebilir. Zaman uyumsuz Yineleyici yöntemleri, `ChannelReader` erken olarak döndürülmemek veya <xref:System.Threading.Channels.ChannelWriter`1>tamamlamadan yöntemden çıkmak gibi kanallarla ortak olan sorunlardan kaçının.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Aşağıdaki örnekte, kanalları kullanarak istemciye veri akışı hakkında temel bilgiler gösterilmektedir. <xref:System.Threading.Channels.ChannelWriter%601>bir nesne her yazıldığında, nesne hemen istemciye gönderilir. Sonunda `ChannelWriter`, istemciye akışın kapatıldığını bildirmek için tamamlanır.

> [!NOTE]
> Arka plan iş parçacığında `ChannelWriter<T>` yazın ve `ChannelReader` mümkün olan en kısa sürede geri döndürün. Diğer Merkez çağırmaları `ChannelReader` döndürülünceye kadar engellenir.
>
> `try ... catch`mantığı çevrele. Hub yöntemi çağrısının düzgün şekilde tamamlandığından emin olmak için `catch` ve `catch` dışında `Channel` doldurun.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Sunucudan istemciye akış hub 'ı yöntemleri, istemci akıştan aboneliği kaldırdığında tetiklenen bir `CancellationToken` parametresini kabul edebilir. Sunucu işlemini durdurmak ve istemci akışın sonundan önce bağlantıyı kestiğinde tüm kaynakları serbest bırakmak için bu belirteci kullanın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>İstemciden sunucuya akış

Hub yöntemi, <xref:System.Threading.Channels.ChannelReader%601> veya <xref:System.Collections.Generic.IAsyncEnumerable%601>türünde bir veya daha fazla nesne kabul ettiğinde, otomatik olarak istemciden sunucuya akış hub yöntemi haline gelir. Aşağıdaki örnek, istemciden gönderilen akış verilerini okumayla ilgili temel bilgileri gösterir. İstemci <xref:System.Threading.Channels.ChannelWriter%601>her yazdığında, veriler hub yönteminin okuduğu sunucuda `ChannelReader` yazılır.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Yöntemin <xref:System.Collections.Generic.IAsyncEnumerable%601> bir sürümü aşağıda verilmiştir.

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a>.NET istemcisi

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış


::: moniker range=">= aspnetcore-3.0"

`HubConnection` `StreamAsync` ve `StreamAsChannelAsync` yöntemleri sunucudan istemciye akış yöntemlerini çağırmak için kullanılır. Hub yöntemi adı ve `StreamAsync` veya `StreamAsChannelAsync`, hub metodunda tanımlanan bağımsız değişkenleri geçirin. `StreamAsync<T>` ve `StreamAsChannelAsync<T>` genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. `IAsyncEnumerable<T>` veya `ChannelReader<T>` türünde bir nesne, akış çağrısından döndürülür ve istemcideki akışı temsil eder.

`IAsyncEnumerable<int>`döndüren `StreamAsync` bir örnek:

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

`ChannelReader<int>`döndüren karşılık gelen `StreamAsChannelAsync` örneği:

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

::: moniker range=">= aspnetcore-2.2"

`HubConnection` `StreamAsChannelAsync` yöntemi, sunucudan istemciye akış yöntemini çağırmak için kullanılır. `StreamAsChannelAsync`için hub yöntemi adını ve hub yöntemi içinde tanımlanan bağımsız değişkenleri geçirin. `StreamAsChannelAsync<T>` genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. Akış çağrısından bir `ChannelReader<T>` döndürülür ve istemcideki akışı temsil eder.

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

`HubConnection` `StreamAsChannelAsync` yöntemi, sunucudan istemciye akış yöntemini çağırmak için kullanılır. `StreamAsChannelAsync`için hub yöntemi adını ve hub yöntemi içinde tanımlanan bağımsız değişkenleri geçirin. `StreamAsChannelAsync<T>` genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. Akış çağrısından bir `ChannelReader<T>` döndürülür ve istemcideki akışı temsil eder.

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

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>İstemciden sunucuya akış

.NET istemcisinden istemciden sunucuya akış hub 'ı yöntemini çağırmak için iki yol vardır. Çağrılan hub yöntemine bağlı olarak, bir `IAsyncEnumerable<T>` veya `ChannelReader` bir bağımsız değişken olarak `SendAsync`, `InvokeAsync`veya `StreamAsChannelAsync`olarak geçirebilirsiniz.

Veriler `IAsyncEnumerable` veya `ChannelWriter` nesnesine yazıldığında, sunucudaki hub yöntemi istemciden alınan verilerle yeni bir öğe alır.

Bir `IAsyncEnumerable` nesnesi kullanıyorsanız, akış öğeleri döndüren yöntem çıktıktan sonra akış sonlanır.

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

`ChannelWriter`kullanıyorsanız, kanalı `channel.Writer.Complete()`ile tamamlıyoruz:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>JavaScript istemcisi

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış

JavaScript istemcileri, `connection.stream`hub 'larda sunucudan istemciye akış yöntemlerini çağırır. `stream` yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub yöntemi adı `Counter`.
* Hub yönteminde tanımlanan bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkenler alacak akış öğesi sayısı ve akış öğeleri arasındaki gecikme için bir sayıdır.

`connection.stream`, `subscribe` yöntemi içeren bir `IStreamResult`döndürür. Bir `IStreamSubscriber` `subscribe` geçirin ve `complete` çağrısından bildirimleri almak için `next`, `error`ve `stream` geri çağırmaları ayarlayın.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Akışı istemciden sonlandırmak için `subscribe` yönteminden döndürülen `ISubscription` `dispose` yöntemi çağırın. Bu yöntemi çağırmak, bir tane sağladıysanız hub yönteminin `CancellationToken` parametresinin iptal edilmesine neden olur.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Akışı istemciden sonlandırmak için `subscribe` yönteminden döndürülen `ISubscription` `dispose` yöntemi çağırın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>İstemciden sunucuya akış

JavaScript istemcileri, çağrılan hub yöntemine bağlı olarak bir `Subject` `send`, `invoke`veya `stream`bir bağımsız değişken olarak geçirerek hub 'larda istemciden sunucuya akış yöntemlerini çağırır. `Subject`, `Subject`gibi görünen bir sınıftır. Örneğin, RxJS ' de, bu kitaplıktaki [Konu](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) sınıfını kullanabilirsiniz.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Bir öğeyle `subject.next(item)` çağırmak öğeyi akışa yazar ve hub yöntemi sunucudaki öğeyi alır.

Akışı sonlandırmak için `subject.complete()`çağırın.

## <a name="java-client"></a>Java istemcisi

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış

SignalR Java istemcisi, akış yöntemlerini çağırmak için `stream` yöntemini kullanır. `stream` üç veya daha fazla bağımsız değişken kabul eder:

* Akış öğelerinin beklenen türü.
* Hub yönteminin adı.
* Hub yönteminde tanımlanan bağımsız değişkenler.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

`HubConnection` `stream` yöntemi akış öğesi türü için bir observable döndürür. Observable türünün `subscribe` yöntemi `onNext`, `onError` ve `onCompleted` işleyicilerinin tanımlanmıştır.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
