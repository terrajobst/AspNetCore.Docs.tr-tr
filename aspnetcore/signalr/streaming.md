---
title: ASP.NET Core SignalR akış kullanın
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275856"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="9c9b7-102">ASP.NET Core SignalR akış kullanın</span><span class="sxs-lookup"><span data-stu-id="9c9b7-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="9c9b7-103">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="9c9b7-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="9c9b7-104">ASP.NET Core SignalR sunucu yöntemlerin akış dönüş değerlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="9c9b7-105">Bu, zaman içinde veri parçalarını burada gelecektir senaryoları için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="9c9b7-106">Dönüş değeri istemciye akışı, bu duruma hemen her parça istemciye kullanılabilir yerine tüm veriler kullanılabilir duruma gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="9c9b7-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c9b7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="9c9b7-108">Hub'ı ayarlama</span><span class="sxs-lookup"><span data-stu-id="9c9b7-108">Set up the hub</span></span>

<span data-ttu-id="9c9b7-109">Bir hub yöntemini, geri döndüğünde otomatik olarak bir akış hub yöntemi olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="9c9b7-110">Aşağıda istemciye veri akış temelleri gösteren bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="9c9b7-111">Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="9c9b7-112">Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="9c9b7-113">Yazma `ChannelReader` arka plan iş parçacığı ve return `ChannelReader` mümkün olan en kısa sürede.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="9c9b7-114">Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="9c9b7-115">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="9c9b7-115">.NET client</span></span>

<span data-ttu-id="9c9b7-116">`StreamAsChannelAsync` Yöntemi `HubConnection` akış yöntemini çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="9c9b7-117">Hub yönteminin adını ve hub yönteminin tanımlanan bağımsız değişkenler geçirmek `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="9c9b7-118">Genel parametresini `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="9c9b7-119">A `ChannelReader<T>` akış çağrısından döndürülen ve istemci akışta temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="9c9b7-120">Verileri okumak için genel bir desen üzerinden döngü oluşturmaktır `WaitToReadAsync` ve arama `TryRead` veri olduğunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="9c9b7-121">Döngü akış sunucusu tarafından kapatıldı veya iptal belirteci geçirilen sona erer `StreamAsChannelAsync` iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="9c9b7-122">JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="9c9b7-122">JavaScript client</span></span>

<span data-ttu-id="9c9b7-123">JavaScript istemcileri aramak akış yöntemleri hub'larında kullanarak `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="9c9b7-124">`stream` Yöntemi iki bağımsız değişken kabul eder:</span><span class="sxs-lookup"><span data-stu-id="9c9b7-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="9c9b7-125">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-125">The name of the hub method.</span></span> <span data-ttu-id="9c9b7-126">Aşağıdaki örnekte, hub yöntemi addır `Counter`.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="9c9b7-127">Hub yönteminin tanımlanmış bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="9c9b7-128">Aşağıdaki örnekte, bağımsız değişkenler: almak için akış öğelerini ve akış öğeler arasındaki gecikme sayısı için bir sayı.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="9c9b7-129">`connection.stream` döndüren bir `IStreamResult` içeren bir `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="9c9b7-130">Geçirmek bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="9c9b7-131">İstemci çağrısı akıştan sonuna `dispose` yöntemi `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9c9b7-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="9c9b7-132">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9c9b7-132">Related resources</span></span>

* [<span data-ttu-id="9c9b7-133">Merkezler</span><span class="sxs-lookup"><span data-stu-id="9c9b7-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9c9b7-134">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="9c9b7-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="9c9b7-135">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="9c9b7-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="9c9b7-136">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="9c9b7-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)