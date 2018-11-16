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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="e5f47-102">ASP.NET Core SignalR öğesinde akışı</span><span class="sxs-lookup"><span data-stu-id="e5f47-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e5f47-103">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="e5f47-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="e5f47-104">ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler.</span><span class="sxs-lookup"><span data-stu-id="e5f47-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="e5f47-105">Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5f47-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="e5f47-106">Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="e5f47-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="e5f47-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e5f47-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="e5f47-108">Hub'ı ayarladınız</span><span class="sxs-lookup"><span data-stu-id="e5f47-108">Set up the hub</span></span>

<span data-ttu-id="e5f47-109">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="e5f47-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="e5f47-110">Akış verileri istemciye temelleri gösteren bir örnek aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="e5f47-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="e5f47-111">Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="e5f47-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="e5f47-112">Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="e5f47-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="e5f47-113">Yazma `ChannelReader` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk.</span><span class="sxs-lookup"><span data-stu-id="e5f47-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="e5f47-114">Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="e5f47-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> <span data-ttu-id="e5f47-115">ASP.NET Core 2.2 veya sonraki sürümlerde, Hub yöntemlerini akış kabul edebilen bir `CancellationToken` akıştan istemci aboneliği kaldırma tetiklenip parametre.</span><span class="sxs-lookup"><span data-stu-id="e5f47-115">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="e5f47-116">Sunucu işlemi durdurmak ve akışın sonuna önce istemcinin bağlantısı kesilirse tüm kaynakları serbest bırakmak için bu belirteci kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5f47-116">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="e5f47-117">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="e5f47-117">.NET client</span></span>

<span data-ttu-id="e5f47-118">`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e5f47-118">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="e5f47-119">Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="e5f47-119">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e5f47-120">Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="e5f47-120">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e5f47-121">A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e5f47-121">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="e5f47-122">Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5f47-122">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="e5f47-123">Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="e5f47-123">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="e5f47-124">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="e5f47-124">JavaScript client</span></span>

<span data-ttu-id="e5f47-125">JavaScript istemciler çağrı akış yöntemleri hub'ları kullanarak `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="e5f47-125">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="e5f47-126">`stream` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="e5f47-126">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="e5f47-127">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="e5f47-127">The name of the hub method.</span></span> <span data-ttu-id="e5f47-128">Aşağıdaki örnekte, hub'yöntemini addır `Counter`.</span><span class="sxs-lookup"><span data-stu-id="e5f47-128">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="e5f47-129">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="e5f47-129">Arguments defined in the hub method.</span></span> <span data-ttu-id="e5f47-130">Aşağıdaki örnekte, bağımsız değişkenleri şunlardır: akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısı için bir sayı.</span><span class="sxs-lookup"><span data-stu-id="e5f47-130">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="e5f47-131">`connection.stream` döndürür bir `IStreamResult` içeren bir `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5f47-131">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="e5f47-132">Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.</span><span class="sxs-lookup"><span data-stu-id="e5f47-132">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="e5f47-133">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5f47-133">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e5f47-134">İstemci akıştan sona erdirmek için çağrı `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5f47-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="e5f47-135">Bu yöntem neden arama `CancellationToken` (bir sağlandıysa) Hub yönteminin parametresi iptal edilecek.</span><span class="sxs-lookup"><span data-stu-id="e5f47-135">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="e5f47-136">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e5f47-136">Related resources</span></span>

* [<span data-ttu-id="e5f47-137">Merkezler</span><span class="sxs-lookup"><span data-stu-id="e5f47-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e5f47-138">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="e5f47-138">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e5f47-139">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="e5f47-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e5f47-140">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="e5f47-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
