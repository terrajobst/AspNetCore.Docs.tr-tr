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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="13028-102">ASP.NET Core SignalR öğesinde akışı</span><span class="sxs-lookup"><span data-stu-id="13028-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="13028-103">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="13028-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="13028-104">ASP.NET Core SignalR sunucusunu yöntemlerin dönüş değerlerini akış destekler.</span><span class="sxs-lookup"><span data-stu-id="13028-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="13028-105">Bu, zaman içinde veri parçalarını burada gelir senaryolar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="13028-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="13028-106">Dönüş değeri, istemciye akışla, bu duruma hemen sonra her parça için istemci kullanılabilir yerine tüm verileri kullanılabilir hale gelmesi bekleniyor gönderilir.</span><span class="sxs-lookup"><span data-stu-id="13028-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="13028-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13028-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="13028-108">Hub'ı ayarladınız</span><span class="sxs-lookup"><span data-stu-id="13028-108">Set up the hub</span></span>

<span data-ttu-id="13028-109">Bir hub yöntemini döndürür, otomatik olarak bir akış hub'yöntemini olur. bir `ChannelReader<T>` veya `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="13028-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="13028-110">Akış verileri istemciye temelleri gösteren bir örnek aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="13028-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="13028-111">Her bir nesne yazılır `ChannelReader` söz konusu nesne hemen istemciye gönderilir.</span><span class="sxs-lookup"><span data-stu-id="13028-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="13028-112">Sonunda, `ChannelReader` Akış kapalı istemci bildirmek için tamamlandı.</span><span class="sxs-lookup"><span data-stu-id="13028-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="13028-113">Yazma `ChannelReader` arka plan iş parçacığı ve dönüş `ChannelReader` olabildiğince çabuk.</span><span class="sxs-lookup"><span data-stu-id="13028-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="13028-114">Diğer hub çağrılarını kadar engellenecek bir `ChannelReader` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="13028-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="13028-115">.NET istemci</span><span class="sxs-lookup"><span data-stu-id="13028-115">.NET client</span></span>

<span data-ttu-id="13028-116">`StreamAsChannelAsync` Metodunda `HubConnection` akış bir yöntem çağırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13028-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="13028-117">Hub yönteminin adı ve bağımsız değişkenler için hub yönteminin tanımlandığı `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="13028-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="13028-118">Genel parametre üzerinde `StreamAsChannelAsync<T>` akış yöntemi tarafından döndürülen nesne türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="13028-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="13028-119">A `ChannelReader<T>` stream çağrısından döndürülen ve istemcinin akışta temsil eder.</span><span class="sxs-lookup"><span data-stu-id="13028-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="13028-120">Verileri okumak için yaygın bir düzen üzerinden döngü olduğu `WaitToReadAsync` ve çağrı `TryRead` veriler ne zaman kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13028-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="13028-121">Akış, sunucu tarafından kapatıldı veya iptal belirtecini geçirilen döngü sona erecek `StreamAsChannelAsync` iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="13028-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="13028-122">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="13028-122">JavaScript client</span></span>

<span data-ttu-id="13028-123">JavaScript istemciler çağrı akış yöntemleri hub'ları kullanarak `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="13028-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="13028-124">`stream` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="13028-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="13028-125">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="13028-125">The name of the hub method.</span></span> <span data-ttu-id="13028-126">Aşağıdaki örnekte, hub'yöntemini addır `Counter`.</span><span class="sxs-lookup"><span data-stu-id="13028-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="13028-127">Hub yönteminin içinde tanımlanan bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="13028-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="13028-128">Aşağıdaki örnekte, bağımsız değişkenleri şunlardır: akış öğeleri almaya ve akış öğeleri arasındaki gecikmeyi sayısı için bir sayı.</span><span class="sxs-lookup"><span data-stu-id="13028-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="13028-129">`connection.stream` döndürür bir `IStreamResult` içeren bir `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13028-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="13028-130">Başarılı bir `IStreamSubscriber` için `subscribe` ve `next`, `error`, ve `complete` bildirimleri almak için geri çağırmaları `stream` çağırma.</span><span class="sxs-lookup"><span data-stu-id="13028-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="13028-131">İstemci çağrısı akıştan sonuna `dispose` metodunda `ISubscription` sağlayıcıdan döndürülen `subscribe` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="13028-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="13028-132">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="13028-132">Related resources</span></span>

* [<span data-ttu-id="13028-133">Merkezler</span><span class="sxs-lookup"><span data-stu-id="13028-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="13028-134">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="13028-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="13028-135">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="13028-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="13028-136">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="13028-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
