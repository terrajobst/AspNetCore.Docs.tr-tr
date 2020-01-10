---
title: .NET istemcisiyle gRPC hizmetlerini çağırma
author: jamesnk
description: .NET gRPC istemcisiyle gRPC hizmetlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829107"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="dfada-103">.NET istemcisiyle gRPC hizmetlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="dfada-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="dfada-104">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet paketinde bir .net GRPC istemci kitaplığı mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="dfada-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="dfada-105">Bu belgede nasıl yapılacağı açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="dfada-105">This document explains how to:</span></span>

* <span data-ttu-id="dfada-106">GRPC istemcisini, gRPC hizmetlerini çağırmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dfada-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="dfada-107">GRPC 'yi birli, sunucu akışı, istemci akışı ve iki yönlü akış yöntemlerine göre çağırır.</span><span class="sxs-lookup"><span data-stu-id="dfada-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="dfada-108">GRPC istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dfada-108">Configure gRPC client</span></span>

<span data-ttu-id="dfada-109">gRPC istemcileri [ *\*. proto* dosyalarından oluşturulan](xref:grpc/basics#generated-c-assets)somut istemci türleridir.</span><span class="sxs-lookup"><span data-stu-id="dfada-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="dfada-110">Somut gRPC istemcisinde *\*. proto* dosyasındaki GRPC hizmetine çeviren yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="dfada-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="dfada-111">Bir kanaldan gRPC istemcisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfada-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="dfada-112">Kanal oluşturmak için `GrpcChannel.ForAddress` kullanarak başlayın ve ardından bir gRPC istemcisi oluşturmak için kanalı kullanın:</span><span class="sxs-lookup"><span data-stu-id="dfada-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="dfada-113">Kanal, gRPC hizmetine uzun süreli bir bağlantıyı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="dfada-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="dfada-114">Bir kanal oluşturulduğunda, hizmet çağırma ile ilgili seçeneklerle yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="dfada-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="dfada-115">Örneğin, çağrı yapmak için kullanılan `HttpClient`, en fazla ileti ve alma iletisi boyutu ve günlüğe kaydetme `GrpcChannelOptions` ve `GrpcChannel.ForAddress`ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="dfada-116">Seçeneklerin tamamı listesi için bkz. [istemci yapılandırma seçenekleri](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="dfada-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="dfada-117">Kanal ve istemci performansı ve kullanımı:</span><span class="sxs-lookup"><span data-stu-id="dfada-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="dfada-118">Kanal oluşturma maliyetli bir işlem olabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="dfada-119">GRPC çağrıları için bir kanalı yeniden kullanmak performans avantajları sağlar.</span><span class="sxs-lookup"><span data-stu-id="dfada-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="dfada-120">gRPC istemcileri kanallarla oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfada-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="dfada-121">gRPC istemcileri hafif nesnelerdir ve önbelleğe alınması veya yeniden kullanılması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="dfada-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="dfada-122">Farklı istemci türleri dahil olmak üzere bir kanaldan birden çok gRPC istemcisi oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="dfada-123">Kanaldan oluşturulan bir kanal ve istemciler, birden çok iş parçacığı tarafından güvenli bir şekilde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="dfada-124">Kanaldan oluşturulan istemciler birden çok eş zamanlı çağrı yapabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="dfada-125">`GrpcChannel.ForAddress`, gRPC istemcisi oluşturmak için tek seçenek değildir.</span><span class="sxs-lookup"><span data-stu-id="dfada-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="dfada-126">GRPC hizmetlerini bir ASP.NET Core uygulamasından arıyorsanız, [GRPC istemci fabrikası tümleştirmesini](xref:grpc/clientfactory)göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="dfada-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="dfada-127">`HttpClientFactory` ile gRPC tümleştirmesi, gRPC istemcileri oluşturmaya yönelik merkezi bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="dfada-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="dfada-128">[.Net istemcisiyle güvenli olmayan gRPC hizmetlerini çağırmak](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="dfada-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="dfada-129">GRPC çağrıları yapma</span><span class="sxs-lookup"><span data-stu-id="dfada-129">Make gRPC calls</span></span>

<span data-ttu-id="dfada-130">Bir gRPC çağrısı, istemcideki bir yöntem çağırarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="dfada-130">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="dfada-131">GRPC istemcisi, ileti serileştirme işlemini işleyecek ve doğru hizmete gRPC çağrısını ele alacak.</span><span class="sxs-lookup"><span data-stu-id="dfada-131">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="dfada-132">gRPC farklı türde yöntemlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="dfada-132">gRPC has different types of methods.</span></span> <span data-ttu-id="dfada-133">Bir gRPC çağrısını yapmak için istemcisini nasıl kullandığınız, çağırdığınız yöntemin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="dfada-133">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="dfada-134">GRPC Yöntem türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="dfada-134">The gRPC method types are:</span></span>

* <span data-ttu-id="dfada-135">Li</span><span class="sxs-lookup"><span data-stu-id="dfada-135">Unary</span></span>
* <span data-ttu-id="dfada-136">Sunucu akışı</span><span class="sxs-lookup"><span data-stu-id="dfada-136">Server streaming</span></span>
* <span data-ttu-id="dfada-137">İstemci akışı</span><span class="sxs-lookup"><span data-stu-id="dfada-137">Client streaming</span></span>
* <span data-ttu-id="dfada-138">İki yönlü akış</span><span class="sxs-lookup"><span data-stu-id="dfada-138">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="dfada-139">Birli çağrı</span><span class="sxs-lookup"><span data-stu-id="dfada-139">Unary call</span></span>

<span data-ttu-id="dfada-140">Birli çağrı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="dfada-140">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="dfada-141">Hizmet bittiğinde bir yanıt iletisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="dfada-141">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="dfada-142">*\*. proto* dosyasındaki her birli hizmet yöntemi, yöntemi çağırmak Için somut GRPC istemci türünde iki .net yönteminin oluşmasına neden olur: zaman uyumsuz bir yöntem ve engelleyici bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="dfada-142">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="dfada-143">Örneğin, `GreeterClient` `SayHello`çağırmanın iki yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="dfada-143">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="dfada-144">`GreeterClient.SayHelloAsync`-`Greeter.SayHello` hizmeti zaman uyumsuz olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="dfada-144">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="dfada-145">Beklenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-145">Can be awaited.</span></span>
* <span data-ttu-id="dfada-146">`GreeterClient.SayHello`-`Greeter.SayHello` hizmeti ve blokları tamamlanana kadar çağırır.</span><span class="sxs-lookup"><span data-stu-id="dfada-146">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="dfada-147">Zaman uyumsuz kodda kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="dfada-147">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="dfada-148">Sunucu akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="dfada-148">Server streaming call</span></span>

<span data-ttu-id="dfada-149">Sunucu akış çağrısı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="dfada-149">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="dfada-150">`ResponseStream.MoveNext()` hizmetten akan iletileri okur.</span><span class="sxs-lookup"><span data-stu-id="dfada-150">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="dfada-151">`ResponseStream.MoveNext()` `false`döndürdüğünde sunucu akış çağrısı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="dfada-151">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="dfada-152">C# 8 veya üzeri bir sürüm kullanıyorsanız, `await foreach` söz dizimi iletileri okumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-152">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="dfada-153">`IAsyncStreamReader<T>.ReadAllAsync()` uzantısı yöntemi, yanıt akışındaki tüm iletileri okur:</span><span class="sxs-lookup"><span data-stu-id="dfada-153">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="dfada-154">İstemci akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="dfada-154">Client streaming call</span></span>

<span data-ttu-id="dfada-155">İstemci akış *çağrısı, istemci* İleti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="dfada-155">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="dfada-156">İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="dfada-157">İstemci ileti göndermeyi bitirdiğinde `RequestStream.CompleteAsync` hizmete bildirmek için çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dfada-157">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="dfada-158">Hizmet bir yanıt iletisi döndürdüğünde çağrı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="dfada-158">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="dfada-159">İki yönlü akış çağrısı</span><span class="sxs-lookup"><span data-stu-id="dfada-159">Bi-directional streaming call</span></span>

<span data-ttu-id="dfada-160">Çift yönlü bir akış *çağrısı, istemci* bir ileti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="dfada-160">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="dfada-161">İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-161">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="dfada-162">Hizmetten akışa alınan iletilere `ResponseStream.MoveNext()` veya `ResponseStream.ReadAllAsync()`erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-162">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="dfada-163">`ResponseStream` daha fazla ileti olmadığında çift yönlü akış çağrısı tamamlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dfada-163">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="dfada-164">Çift yönlü bir akış araması sırasında, istemci ve hizmet herhangi bir zamanda iletileri birbirlerine gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="dfada-164">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="dfada-165">Çift yönlü bir çağrı ile etkileşimde bulunmak için en iyi istemci mantığı, hizmet mantığına bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="dfada-165">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfada-166">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dfada-166">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
