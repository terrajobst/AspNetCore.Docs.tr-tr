---
title: ASP.NET Core SignalR öğesinde akışı
author: bradygaster
description: İstemci ve sunucu arasında veri akışı yapmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: 8f39fdfa45766b5bbec572970f009abefefdc419
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902951"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="1634a-103">ASP.NET Core SignalR öğesinde akışı</span><span class="sxs-lookup"><span data-stu-id="1634a-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="1634a-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="1634a-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1634a-105">ASP.NET Core SignalR, istemciden sunucuya ve sunucudan istemciye akışını destekler.</span><span class="sxs-lookup"><span data-stu-id="1634a-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="1634a-106">Bu, burada zaman içinde veri parçalarını geldiğinde senaryoları için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="1634a-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="1634a-107">Bu duruma hemen sonra akış, her parça istemci veya sunucu için kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1634a-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1634a-108">ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler.</span><span class="sxs-lookup"><span data-stu-id="1634a-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="1634a-109">Bu, burada zaman içinde veri parçalarını geldiğinde senaryoları için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="1634a-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="1634a-110">Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1634a-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="1634a-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1634a-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="1634a-112">Akış hub'ı ayarlama</span><span class="sxs-lookup"><span data-stu-id="1634a-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1634a-113">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, veya `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="1634a-113">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1634a-114">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir <xref:System.Threading.Channels.ChannelReader%601> veya `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="1634a-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="1634a-115">Server istemcisi akış</span><span class="sxs-lookup"><span data-stu-id="1634a-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1634a-116">Hub yöntemlerinde akış döndürebilir `IAsyncEnumerable<T>` ek olarak `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="1634a-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="1634a-117">Döndürülecek en basit yolu `IAsyncEnumerable<T>` aşağıdaki örnekte gösterildiği gibi hub yönteminin zaman uyumsuz bir yineleyici yöntem tarafından sunuluyor.</span><span class="sxs-lookup"><span data-stu-id="1634a-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="1634a-118">Hub zaman uyumsuz yineleyicinin yöntemler kabul edebileceği bir `CancellationToken` akıştan istemci aboneliği kaldırma tetikleyen parametresi.</span><span class="sxs-lookup"><span data-stu-id="1634a-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="1634a-119">Zaman uyumsuz yineleyicinin yöntemler döndürmez gibi Kanallar, ortak sorunları önlemek `ChannelReader` kadar erken veya tamamlamadan yönteminden çıkılıyor <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="1634a-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="1634a-120">Aşağıdaki örnek, akış kanalları kullanarak bir istemciye ilişkin temel bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="1634a-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="1634a-121">Her bir nesne yazılır <xref:System.Threading.Channels.ChannelWriter%601>, nesneyi hemen istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="1634a-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="1634a-122">Sonunda, `ChannelWriter` Akış kapalı istemci bildirmek için tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="1634a-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="1634a-123">Yazma `ChannelWriter<T>` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk.</span><span class="sxs-lookup"><span data-stu-id="1634a-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="1634a-124">Diğer hub çağrılarını kadar engellenen bir `ChannelReader` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1634a-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="1634a-125">Mantık kaydırma bir `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="1634a-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="1634a-126">Tamamlamak `Channel` içinde `catch` ve dış `catch` hub emin olmak için yöntem çağırma düzgün bir şekilde tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="1634a-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="1634a-127">Server istemcisi akış hub yöntemleri kabul edebileceği bir `CancellationToken` akıştan istemci aboneliği kaldırma tetikleyen parametresi.</span><span class="sxs-lookup"><span data-stu-id="1634a-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="1634a-128">Sunucu işlemi durdurmak ve akışın sonuna önce istemcinin bağlantısı kesilirse tüm kaynakları serbest bırakmak için bu belirteci kullanın.</span><span class="sxs-lookup"><span data-stu-id="1634a-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="1634a-129">İstemci sunucu akış</span><span class="sxs-lookup"><span data-stu-id="1634a-129">Client-to-server streaming</span></span>

<span data-ttu-id="1634a-130">Bir hub yöntemini bir veya daha fazla kabul ettiğinde, otomatik olarak bir istemci-sunucu akış hub yönteminin olur <xref:System.Threading.Channels.ChannelReader`1>s.</span><span class="sxs-lookup"><span data-stu-id="1634a-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more <xref:System.Threading.Channels.ChannelReader`1>s.</span></span> <span data-ttu-id="1634a-131">Aşağıdaki örnek istemci tarafından gönderilen veri akışı okuma temellerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1634a-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="1634a-132">Her istemci Yazar <xref:System.Threading.Channels.ChannelWriter`1>, veri yazılır `ChannelReader` hub yönteminin okuma sunucuda.</span><span class="sxs-lookup"><span data-stu-id="1634a-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter`1>, the data is written into the `ChannelReader` on the server that the hub method is reading from.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="1634a-133">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="1634a-133">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="1634a-134">Server istemcisi akış</span><span class="sxs-lookup"><span data-stu-id="1634a-134">Server-to-client streaming</span></span>

<span data-ttu-id="1634a-135">`StreamAsChannelAsync` Metodunda `HubConnection` sunucusu istemci akışı yöntemi çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1634a-135">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="1634a-136">Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="1634a-136">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="1634a-137">Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="1634a-137">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="1634a-138">A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder.</span><span class="sxs-lookup"><span data-stu-id="1634a-138">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="1634a-139">İstemci sunucu akış</span><span class="sxs-lookup"><span data-stu-id="1634a-139">Client-to-server streaming</span></span>

<span data-ttu-id="1634a-140">Bir istemci-sunucu akış hub'yöntemini .NET istemcisinden çağırmak için oluşturma bir `Channel` ve `ChannelReader` bağımsız değişkeni olarak `SendAsync`, `InvokeAsync`, veya `StreamAsChannelAsync`hub yönteminin çağrılması bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="1634a-140">To invoke a client-to-server streaming hub method from the .NET client, create a `Channel` and pass the `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="1634a-141">Her veri yazılır `ChannelWriter`, sunucudaki hub yönteminin istemciden veri içeren yeni bir öğe alır.</span><span class="sxs-lookup"><span data-stu-id="1634a-141">Whenever data is written to the `ChannelWriter`, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="1634a-142">Akış sonuna kanalıyla tamamlamak `channel.Writer.Complete()`.</span><span class="sxs-lookup"><span data-stu-id="1634a-142">To end the stream, complete the channel with `channel.Writer.Complete()`.</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="1634a-143">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1634a-143">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="1634a-144">Server istemcisi akış</span><span class="sxs-lookup"><span data-stu-id="1634a-144">Server-to-client streaming</span></span>

<span data-ttu-id="1634a-145">JavaScript istemcilerinin hub'larla üzerinde sunucu istemci akış yöntemleri çağırmak `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="1634a-145">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="1634a-146">`stream` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="1634a-146">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="1634a-147">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="1634a-147">The name of the hub method.</span></span> <span data-ttu-id="1634a-148">Aşağıdaki örnekte, hub'yöntemini addır `Counter`.</span><span class="sxs-lookup"><span data-stu-id="1634a-148">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="1634a-149">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="1634a-149">Arguments defined in the hub method.</span></span> <span data-ttu-id="1634a-150">Aşağıdaki örnekte, akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısını sayısını bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="1634a-150">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="1634a-151">`connection.stream` döndürür bir `IStreamResult`, içeren bir `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1634a-151">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="1634a-152">Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.</span><span class="sxs-lookup"><span data-stu-id="1634a-152">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="1634a-153">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1634a-153">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="1634a-154">Bu yöntemin çağrılması iptaline neden `CancellationToken` sağlandıysa bir Hub yöntemi parametresi.</span><span class="sxs-lookup"><span data-stu-id="1634a-154">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="1634a-155">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1634a-155">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="1634a-156">İstemci sunucu akış</span><span class="sxs-lookup"><span data-stu-id="1634a-156">Client-to-server streaming</span></span>

<span data-ttu-id="1634a-157">JavaScript istemcilerinin geçirerek hub'larında istemci-sunucu akış yöntemleri çağırmak bir `Subject` bağımsız değişkeni olarak `send`, `invoke`, veya `stream`hub yönteminin çağrılması bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="1634a-157">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="1634a-158">`Subject` Şuna benzer bir sınıf olan bir `Subject`.</span><span class="sxs-lookup"><span data-stu-id="1634a-158">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="1634a-159">Örneğin RxJS içinde kullanabilirsiniz [konu](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) bu kitaplıktan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1634a-159">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="1634a-160">Çağırma `subject.next(item)` ile bir öğe öğesi akışa yazar ve hub yönteminin sunucuda öğeyi alır.</span><span class="sxs-lookup"><span data-stu-id="1634a-160">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="1634a-161">Akış sonuna çağrı `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="1634a-161">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="1634a-162">Java istemcisi</span><span class="sxs-lookup"><span data-stu-id="1634a-162">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="1634a-163">Server istemcisi akış</span><span class="sxs-lookup"><span data-stu-id="1634a-163">Server-to-client streaming</span></span>

<span data-ttu-id="1634a-164">SignalR Java istemcinin kullandığı `stream` akış yöntemlerini çağırmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="1634a-164">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="1634a-165">`stream` üç veya daha fazla bağımsız değişken kabul eder:</span><span class="sxs-lookup"><span data-stu-id="1634a-165">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="1634a-166">Akış öğeleri beklenen türü.</span><span class="sxs-lookup"><span data-stu-id="1634a-166">The expected type of the stream items.</span></span>
* <span data-ttu-id="1634a-167">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="1634a-167">The name of the hub method.</span></span>
* <span data-ttu-id="1634a-168">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="1634a-168">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="1634a-169">`stream` Metodunda `HubConnection` Observable akış öğesi türünü döndürür.</span><span class="sxs-lookup"><span data-stu-id="1634a-169">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="1634a-170">Gözlemlenebilir türün `subscribe` yöntemdir nerede `onNext`, `onError` ve `onCompleted` işleyicileri tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="1634a-170">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="1634a-171">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1634a-171">Additional resources</span></span>

* [<span data-ttu-id="1634a-172">Merkezler</span><span class="sxs-lookup"><span data-stu-id="1634a-172">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1634a-173">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="1634a-173">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="1634a-174">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="1634a-174">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="1634a-175">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="1634a-175">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
