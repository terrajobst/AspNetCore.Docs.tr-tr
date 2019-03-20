---
title: ASP.NET Core SignalR öğesinde akışı
author: bradygaster
description: Sunucu hub yöntemleri akışları değerleri döndürür ve .NET ve JavaScript istemcileri kullanarak akışları kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 7c176e3f21ffca7b97d9d3c2e8861032f22587b8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264298"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="42b64-103">ASP.NET Core SignalR öğesinde akışı</span><span class="sxs-lookup"><span data-stu-id="42b64-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="42b64-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="42b64-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="42b64-105">ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler.</span><span class="sxs-lookup"><span data-stu-id="42b64-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="42b64-106">Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="42b64-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="42b64-107">Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="42b64-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="42b64-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="42b64-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="42b64-109">Hub'ı ayarladınız</span><span class="sxs-lookup"><span data-stu-id="42b64-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="42b64-110">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, veya `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="42b64-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="42b64-111">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="42b64-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="42b64-112">ASP.NET Core 3.0 veya sonraki sürümlerde, hub yöntemlerini akış döndürebilir `IAsyncEnumerable<T>` ek olarak `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="42b64-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="42b64-113">Döndürülecek en basit yolu `IAsyncEnumerable<T>` aşağıdaki örnekte gösterildiği gibi hub yönteminin zaman uyumsuz bir yineleyici yöntem tarafından sunuluyor.</span><span class="sxs-lookup"><span data-stu-id="42b64-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="42b64-114">Hub zaman uyumsuz yineleyicinin yöntemler kabul edebileceği bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre.</span><span class="sxs-lookup"><span data-stu-id="42b64-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="42b64-115">Zaman uyumsuz yineleyicinin yöntemler kolayca döndürmez gibi kanallar ile ortak sorunları önlemek `ChannelReader` kadar erken veya tamamlamadan yönteminden çıkılıyor `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="42b64-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="42b64-116">Aşağıdaki örnek, akış kanalları kullanarak bir istemciye ilişkin temel bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="42b64-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="42b64-117">Her bir nesne yazılır `ChannelWriter` söz konusu nesne hemen istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="42b64-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="42b64-118">Sonunda, `ChannelWriter` Akış kapalı istemci bildirmek için tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="42b64-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="42b64-119">Yazma `ChannelWriter` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk.</span><span class="sxs-lookup"><span data-stu-id="42b64-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="42b64-120">Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="42b64-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="42b64-121">Mantığınızda kaydırma bir `try ... catch` ve tamamlayın `Channel` yakalama ve hub emin olmak için yakalama dışında yöntemi çağırma düzgün bir şekilde tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="42b64-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="42b64-122">ASP.NET Core 2.2 veya sonraki sürümlerde, hub yöntemlerini akış kabul edebilen bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre.</span><span class="sxs-lookup"><span data-stu-id="42b64-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="42b64-123">Sunucu işlemi durdurmak ve akışın sonuna önce istemcinin bağlantısı kesilirse tüm kaynakları serbest bırakmak için bu belirteci kullanın.</span><span class="sxs-lookup"><span data-stu-id="42b64-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="42b64-124">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="42b64-124">.NET client</span></span>

<span data-ttu-id="42b64-125">`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="42b64-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="42b64-126">Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="42b64-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="42b64-127">Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="42b64-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="42b64-128">A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder.</span><span class="sxs-lookup"><span data-stu-id="42b64-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="42b64-129">Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="42b64-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="42b64-130">Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="42b64-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="42b64-131">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="42b64-131">JavaScript client</span></span>

<span data-ttu-id="42b64-132">JavaScript istemciler çağrı akış yöntemleri hub'ları kullanarak `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="42b64-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="42b64-133">`stream` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="42b64-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="42b64-134">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="42b64-134">The name of the hub method.</span></span> <span data-ttu-id="42b64-135">Aşağıdaki örnekte, hub'yöntemini addır `Counter`.</span><span class="sxs-lookup"><span data-stu-id="42b64-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="42b64-136">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="42b64-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="42b64-137">Aşağıdaki örnekte, bağımsız değişkenleri şunlardır: akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısı için bir sayı.</span><span class="sxs-lookup"><span data-stu-id="42b64-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="42b64-138">`connection.stream` döndürür bir `IStreamResult` içeren bir `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="42b64-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="42b64-139">Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.</span><span class="sxs-lookup"><span data-stu-id="42b64-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="42b64-140">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="42b64-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="42b64-141">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="42b64-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="42b64-142">Bu yöntem neden arama `CancellationToken` (bir sağlandıysa) Hub yönteminin parametresi iptal edilecek.</span><span class="sxs-lookup"><span data-stu-id="42b64-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="java-client"></a><span data-ttu-id="42b64-143">Java istemcisi</span><span class="sxs-lookup"><span data-stu-id="42b64-143">Java client</span></span>

<span data-ttu-id="42b64-144">SignalR Java istemcinin kullandığı `stream` akış yöntemlerini çağırmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="42b64-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="42b64-145">Bu, üç veya daha fazla bağımsız değişken kabul eder:</span><span class="sxs-lookup"><span data-stu-id="42b64-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="42b64-146">Akış öğeleri beklenen tür</span><span class="sxs-lookup"><span data-stu-id="42b64-146">The expected type of the stream items</span></span>
* <span data-ttu-id="42b64-147">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="42b64-147">The name of the hub method.</span></span>
* <span data-ttu-id="42b64-148">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="42b64-148">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="42b64-149">`stream` Metodunda `HubConnection` Observable akış öğesi türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="42b64-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="42b64-150">Gözlemlenebilir türün `subscribe` yöntemdir tanımladığınız yerlerde, `onNext`, `onError` ve `onCompleted` işleyicileri.</span><span class="sxs-lookup"><span data-stu-id="42b64-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="42b64-151">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="42b64-151">Related resources</span></span>

* [<span data-ttu-id="42b64-152">Merkezler</span><span class="sxs-lookup"><span data-stu-id="42b64-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="42b64-153">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="42b64-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="42b64-154">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="42b64-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="42b64-155">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="42b64-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
