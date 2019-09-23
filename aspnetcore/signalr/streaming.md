---
title: ASP.NET Core SignalR 'de akış kullanma
author: bradygaster
description: İstemci ve sunucu arasında veri akışını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: d520c8eec3e777acb9604bdcb5969268deabf8da
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186936"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 'de akış kullanma

[Brennan Conroy](https://github.com/BrennanConroy) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR, istemciden sunucuya ve sunucudan istemciye akışı destekler. Bu, veri parçalarının zaman içinde nereden ulaştığını senaryolar için yararlıdır. Akış sırasında her parça, tüm verilerin kullanılabilir hale gelmesini beklemek yerine istemciye veya sunucuya gönderilir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR, sunucu yöntemlerinin akış dönüş değerlerini destekler. Bu, veri parçalarının zaman içinde nereden ulaştığını senaryolar için yararlıdır. Bir dönüş değeri istemciye akışa eklendiğinde, her parça, tüm verilerin kullanılabilir hale gelmesini beklemek yerine istemciye gönderilir.

::: moniker-end

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Akış için bir hub ayarlama

::: moniker range=">= aspnetcore-3.0"

<xref:System.Collections.Generic.IAsyncEnumerable`1>Hub yöntemi <xref:System.Threading.Channels.ChannelReader%601> ,`Task<IAsyncEnumerable<T>>`,, veya`Task<ChannelReader<T>>`döndürdüğünde otomatik olarak akış hub yöntemi haline gelir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bir hub yöntemi, bir <xref:System.Threading.Channels.ChannelReader%601> `Task<ChannelReader<T>>`veya döndürdüğünde otomatik olarak akış hub yöntemi haline gelir.

::: moniker-end

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış

::: moniker range=">= aspnetcore-3.0"

Akış hub 'ı yöntemleri, `IAsyncEnumerable<T>` öğesine `ChannelReader<T>`ek olarak dönebilir. ' `IAsyncEnumerable<T>` In en basit yolu, aşağıdaki örnekte gösterildiği gibi, hub yöntemini zaman uyumsuz bir yineleyici yöntemi yaparak kullanmaktır. Hub zaman uyumsuz Yineleyici yöntemleri, istemci `CancellationToken` akıştan aboneliği kaldırdığında tetiklenen bir parametreyi kabul edebilir. Zaman uyumsuz Yineleyici yöntemleri, kanallarla ortak olan sorunları önlemek için `ChannelReader` <xref:System.Threading.Channels.ChannelWriter`1>, yeterince erken dönmemek veya yönteminden çıkmak gibi.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

Aşağıdaki örnekte, kanalları kullanarak istemciye veri akışı hakkında temel bilgiler gösterilmektedir. Nesnesine bir nesne yazıldığında <xref:System.Threading.Channels.ChannelWriter%601>, nesne hemen istemciye gönderilir. Sonunda `ChannelWriter` , istemciye akışın kapalı olduğunu bildirmek için tamamlanır.

> [!NOTE]
> Arka plan iş `ChannelWriter<T>` parçacığında öğesine yazın ve mümkün olan en `ChannelReader` kısa sürede geri döndürün. Diğer Merkez çağırmaları `ChannelReader` , döndürülünceye kadar engellenir.
>
> İçindeki mantığı sarın `try ... catch`. Hub yöntemi çağrısının doğru `catch` şekilde tamamlandığından emin `catch` olmak için içindekivedışındakiiçindekiöğesinidoldurun.`Channel`

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

Sunucudan istemciye akış hub 'ı yöntemleri, istemci akıştan aboneliği kaldırdığında tetiklenen bir `CancellationToken` parametreyi kabul edebilir. Sunucu işlemini durdurmak ve istemci akışın sonundan önce bağlantıyı kestiğinde tüm kaynakları serbest bırakmak için bu belirteci kullanın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>İstemciden sunucuya akış

Hub yöntemi, veya <xref:System.Threading.Channels.ChannelReader%601> <xref:System.Collections.Generic.IAsyncEnumerable%601>türünde bir veya daha fazla nesne kabul ettiğinde, istemci-sunucu akış hub yöntemi otomatik olarak olur. Aşağıdaki örnek, istemciden gönderilen akış verilerini okumayla ilgili temel bilgileri gösterir. İstemci öğesine <xref:System.Threading.Channels.ChannelWriter%601>her yazdığında, veriler hub yönteminin okuduğu sunucuda `ChannelReader` üzerine yazılır.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Yönteminin <xref:System.Collections.Generic.IAsyncEnumerable%601> bir sürümü aşağıda verilmiştir.

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

Ve `StreamAsync` üzerindekiyöntemleri`HubConnection` , sunucudan istemciye akış yöntemlerini çağırmak için kullanılır. `StreamAsChannelAsync` Hub yöntemi `StreamAsync` veya hub metodunda tanımlanan bağımsız değişkenleri ya `StreamAsChannelAsync`da ' a geçirin. `StreamAsync<T>` Ve`StreamAsChannelAsync<T>` üzerindeki genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. `IAsyncEnumerable<T>` Veya`ChannelReader<T>` türündeki bir nesne, akış çağrısından döndürülür ve istemcideki akışı temsil eder.

Şunu `StreamAsync` döndüren`IAsyncEnumerable<int>`bir örnek:

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

Şunu döndüren `StreamAsChannelAsync` `ChannelReader<int>`karşılık gelen bir örnek:

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

`StreamAsChannelAsync` Üzerinde`HubConnection` yöntemi, sunucudan istemciye akış yöntemini çağırmak için kullanılır. Hub yöntemi içinde `StreamAsChannelAsync`tanımlanan bağımsız değişkenleri ve hub metodunu geçirin. Üzerinde `StreamAsChannelAsync<T>` genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. Bir `ChannelReader<T>` akış çağrısından döndürülür ve istemcideki akışı temsil eder.

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

`StreamAsChannelAsync` Üzerinde`HubConnection` yöntemi, sunucudan istemciye akış yöntemini çağırmak için kullanılır. Hub yöntemi içinde `StreamAsChannelAsync`tanımlanan bağımsız değişkenleri ve hub metodunu geçirin. Üzerinde `StreamAsChannelAsync<T>` genel parametresi, akış yöntemi tarafından döndürülen nesne türünü belirtir. Bir `ChannelReader<T>` akış çağrısından döndürülür ve istemcideki akışı temsil eder.

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

.NET istemcisinden istemciden sunucuya akış hub 'ı yöntemini çağırmak için iki yol vardır. Çağrılan hub yöntemine bağlı olarak, `IAsyncEnumerable<T>` veya bir `ChannelReader` `SendAsync` `InvokeAsync`bağımsız değişken olarak bir ya da bir bağımsız değişken olarakgeçirebilirsiniz.`StreamAsChannelAsync`

Veriler `IAsyncEnumerable` veya`ChannelWriter` nesnesine her yazıldığında, sunucudaki hub yöntemi istemciden alınan verilerle yeni bir öğe alır.

Bir `IAsyncEnumerable` nesne kullanılıyorsa, akış öğeleri döndüren yöntem çıktıktan sonra akış sonlanır.

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

Ya da kullanıyorsanız `ChannelWriter`, şu ile `channel.Writer.Complete()`kanalı tamamlıyoruz:

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

JavaScript istemcileri, ile `connection.stream`hub 'larda sunucudan istemciye akış yöntemlerini çağırır. `stream` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub yöntemi adı `Counter`.
* Hub yönteminde tanımlanan bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkenler alacak akış öğesi sayısı ve akış öğeleri arasındaki gecikme için bir sayıdır.

`connection.stream``IStreamResult` bir`subscribe` yöntemi içeren bir döndürür. `IStreamSubscriber` 'Ageçirin`stream` ve çağrıdan bildirim almak için, `complete` ve geri çağırmaları ayarlayın. `subscribe` `error` `next`

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Akışı istemciden sonlandırmak için, `dispose` `subscribe` yönteminden döndürülen `ISubscription` üzerinde yöntemi çağırın. Bu yöntemi çağırmak, bir tane sağladıysanız hub yönteminin `CancellationToken` parametresinin iptal edilmesine neden olur.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Akışı istemciden sonlandırmak için, `dispose` `subscribe` yönteminden döndürülen `ISubscription` üzerinde yöntemi çağırın.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>İstemciden sunucuya akış

`Subject` JavaScript istemcileri `send`, `stream`çağrılan hub yöntemine bağlı olarak, veya olarak bir bağımsız değişken olarak geçirerek hub 'larda istemciden sunucuya akış yöntemlerini çağırır. `invoke` `Subject` ,`Subject`Gibi görünen bir sınıftır. Örneğin, RxJS ' de, bu kitaplıktaki [Konu](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) sınıfını kullanabilirsiniz.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Bir `subject.next(item)` öğeyle çağırmak öğeyi akışa yazar ve hub yöntemi sunucudaki öğeyi alır.

Akışı sonlandırmak için çağırın `subject.complete()`.

## <a name="java-client"></a>Java istemcisi

### <a name="server-to-client-streaming"></a>Sunucudan istemciye akış

SignalR Java istemcisi, akış yöntemlerini `stream` çağırmak için yöntemini kullanır. `stream`üç veya daha fazla bağımsız değişken kabul eder:

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

`stream` Üzerindeki`HubConnection` yöntemi, akış öğesi türü için bir observable döndürür. Observable türünün `subscribe` yöntemi nerede `onNext`, `onError` ve `onCompleted` işleyiciler tanımlanmıştır.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
