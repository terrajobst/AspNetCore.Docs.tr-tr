---
title: .NET istemcisiyle gRPC hizmetlerini çağırma
author: jamesnk
description: .NET gRPC istemcisiyle gRPC hizmetlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667176"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="318c8-103">.NET istemcisiyle gRPC hizmetlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="318c8-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="318c8-104">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet paketinde bir .net GRPC istemci kitaplığı mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="318c8-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="318c8-105">Bu belgede nasıl yapılacağı açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="318c8-105">This document explains how to:</span></span>

* <span data-ttu-id="318c8-106">GRPC istemcisini, gRPC hizmetlerini çağırmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="318c8-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="318c8-107">GRPC 'yi birli, sunucu akışı, istemci akışı ve iki yönlü akış yöntemlerine göre çağırır.</span><span class="sxs-lookup"><span data-stu-id="318c8-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="318c8-108">GRPC istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="318c8-108">Configure gRPC client</span></span>

<span data-ttu-id="318c8-109">gRPC istemcileri [ *\*. proto* dosyalarından oluşturulan](xref:grpc/basics#generated-c-assets)somut istemci türleridir.</span><span class="sxs-lookup"><span data-stu-id="318c8-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="318c8-110">Somut gRPC istemcisinde *\*. proto* dosyasındaki GRPC hizmetine çeviren yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="318c8-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="318c8-111">Bir kanaldan gRPC istemcisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="318c8-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="318c8-112">Kanal oluşturmak için `GrpcChannel.ForAddress` kullanarak başlayın ve ardından bir gRPC istemcisi oluşturmak için kanalı kullanın:</span><span class="sxs-lookup"><span data-stu-id="318c8-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="318c8-113">Kanal, gRPC hizmetine uzun süreli bir bağlantıyı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="318c8-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="318c8-114">Bir kanal oluşturulduğunda, hizmet çağırma ile ilgili seçeneklerle yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="318c8-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="318c8-115">Örneğin, çağrı yapmak için kullanılan `HttpClient`, en fazla ileti ve alma iletisi boyutu ve günlüğe kaydetme `GrpcChannelOptions` ve `GrpcChannel.ForAddress`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="318c8-116">Seçeneklerin tamamı listesi için bkz. [istemci yapılandırma seçenekleri](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="318c8-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="318c8-117">Kanal ve istemci performansı ve kullanımı:</span><span class="sxs-lookup"><span data-stu-id="318c8-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="318c8-118">Kanal oluşturma maliyetli bir işlem olabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="318c8-119">GRPC çağrıları için bir kanalı yeniden kullanmak performans avantajları sağlar.</span><span class="sxs-lookup"><span data-stu-id="318c8-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="318c8-120">gRPC istemcileri kanallarla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="318c8-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="318c8-121">gRPC istemcileri hafif nesnelerdir ve önbelleğe alınması veya yeniden kullanılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="318c8-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="318c8-122">Farklı istemci türleri dahil olmak üzere bir kanaldan birden çok gRPC istemcisi oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="318c8-123">Kanaldan oluşturulan bir kanal ve istemciler, birden çok iş parçacığı tarafından güvenli bir şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="318c8-124">Kanaldan oluşturulan istemciler birden çok eş zamanlı çağrı yapabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="318c8-125">`GrpcChannel.ForAddress`, gRPC istemcisi oluşturmak için tek seçenek değildir.</span><span class="sxs-lookup"><span data-stu-id="318c8-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="318c8-126">GRPC hizmetlerini bir ASP.NET Core uygulamasından arıyorsanız, [GRPC istemci fabrikası tümleştirmesini](xref:grpc/clientfactory)göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="318c8-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="318c8-127">`HttpClientFactory` ile gRPC tümleştirmesi, gRPC istemcileri oluşturmaya yönelik merkezi bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="318c8-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="318c8-128">[.Net istemcisiyle güvenli olmayan gRPC hizmetlerini çağırmak](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="318c8-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="318c8-129">`Grpc.Net.Client` ile HTTP/2 üzerinden gRPC çağırma, Xamarin üzerinde şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="318c8-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="318c8-130">Gelecek bir Xamarin sürümünde HTTP/2 desteğini geliştirmek için çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="318c8-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="318c8-131">[GRPC. Core](https://www.nuget.org/packages/Grpc.Core) ve [GRPC-Web](xref:grpc/browser) , bugün çalışan önemli alternatiflerdir.</span><span class="sxs-lookup"><span data-stu-id="318c8-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="318c8-132">GRPC çağrıları yapma</span><span class="sxs-lookup"><span data-stu-id="318c8-132">Make gRPC calls</span></span>

<span data-ttu-id="318c8-133">Bir gRPC çağrısı, istemcideki bir yöntem çağırarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="318c8-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="318c8-134">GRPC istemcisi, ileti serileştirme işlemini işleyecek ve doğru hizmete gRPC çağrısını ele alacak.</span><span class="sxs-lookup"><span data-stu-id="318c8-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="318c8-135">gRPC farklı türde yöntemlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="318c8-135">gRPC has different types of methods.</span></span> <span data-ttu-id="318c8-136">Bir gRPC çağrısını yapmak için istemcisini nasıl kullandığınız, çağırdığınız yöntemin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="318c8-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="318c8-137">GRPC Yöntem türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="318c8-137">The gRPC method types are:</span></span>

* <span data-ttu-id="318c8-138">Li</span><span class="sxs-lookup"><span data-stu-id="318c8-138">Unary</span></span>
* <span data-ttu-id="318c8-139">Sunucu akışı</span><span class="sxs-lookup"><span data-stu-id="318c8-139">Server streaming</span></span>
* <span data-ttu-id="318c8-140">İstemci akışı</span><span class="sxs-lookup"><span data-stu-id="318c8-140">Client streaming</span></span>
* <span data-ttu-id="318c8-141">İki yönlü akış</span><span class="sxs-lookup"><span data-stu-id="318c8-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="318c8-142">Birli çağrı</span><span class="sxs-lookup"><span data-stu-id="318c8-142">Unary call</span></span>

<span data-ttu-id="318c8-143">Birli çağrı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="318c8-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="318c8-144">Hizmet bittiğinde bir yanıt iletisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="318c8-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="318c8-145">*\*. proto* dosyasındaki her birli hizmet yöntemi, yöntemi çağırmak Için somut GRPC istemci türünde iki .net yönteminin oluşmasına neden olur: zaman uyumsuz bir yöntem ve engelleyici bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="318c8-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="318c8-146">Örneğin, `GreeterClient` `SayHello`çağırmanın iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="318c8-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="318c8-147">`GreeterClient.SayHelloAsync`-`Greeter.SayHello` hizmeti zaman uyumsuz olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="318c8-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="318c8-148">Beklenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-148">Can be awaited.</span></span>
* <span data-ttu-id="318c8-149">`GreeterClient.SayHello`-`Greeter.SayHello` hizmeti ve blokları tamamlanana kadar çağırır.</span><span class="sxs-lookup"><span data-stu-id="318c8-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="318c8-150">Zaman uyumsuz kodda kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="318c8-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="318c8-151">Sunucu akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="318c8-151">Server streaming call</span></span>

<span data-ttu-id="318c8-152">Sunucu akış çağrısı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="318c8-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="318c8-153">`ResponseStream.MoveNext()` hizmetten akan iletileri okur.</span><span class="sxs-lookup"><span data-stu-id="318c8-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="318c8-154">`ResponseStream.MoveNext()` `false`döndürdüğünde sunucu akış çağrısı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="318c8-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

<span data-ttu-id="318c8-155">C# 8 veya üzeri bir sürüm kullanıyorsanız, `await foreach` söz dizimi iletileri okumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="318c8-156">`IAsyncStreamReader<T>.ReadAllAsync()` uzantısı yöntemi, yanıt akışındaki tüm iletileri okur:</span><span class="sxs-lookup"><span data-stu-id="318c8-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="318c8-157">İstemci akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="318c8-157">Client streaming call</span></span>

<span data-ttu-id="318c8-158">İstemci akış *çağrısı, istemci* İleti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="318c8-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="318c8-159">İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="318c8-160">İstemci ileti göndermeyi bitirdiğinde `RequestStream.CompleteAsync` hizmete bildirmek için çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="318c8-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="318c8-161">Hizmet bir yanıt iletisi döndürdüğünde çağrı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="318c8-161">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="318c8-162">İki yönlü akış çağrısı</span><span class="sxs-lookup"><span data-stu-id="318c8-162">Bi-directional streaming call</span></span>

<span data-ttu-id="318c8-163">Çift yönlü bir akış *çağrısı, istemci* bir ileti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="318c8-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="318c8-164">İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="318c8-165">Hizmetten akışa alınan iletilere `ResponseStream.MoveNext()` veya `ResponseStream.ReadAllAsync()`erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="318c8-166">`ResponseStream` daha fazla ileti olmadığında çift yönlü akış çağrısı tamamlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="318c8-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

<span data-ttu-id="318c8-167">Çift yönlü bir akış araması sırasında, istemci ve hizmet herhangi bir zamanda iletileri birbirlerine gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="318c8-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="318c8-168">Çift yönlü bir çağrı ile etkileşimde bulunmak için en iyi istemci mantığı, hizmet mantığına bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="318c8-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="318c8-169">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="318c8-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
