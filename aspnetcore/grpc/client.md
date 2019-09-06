---
title: .NET istemcisiyle gRPC hizmetlerini çağırma
author: jamesnk
description: .NET gRPC istemcisiyle gRPC hizmetlerini çağırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 5518a221c4641ba0d1da051a14750e3165944455
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310676"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="cc43f-103">.NET istemcisiyle gRPC hizmetlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="cc43f-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="cc43f-104">[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet paketinde bir .net GRPC istemci kitaplığı mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="cc43f-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="cc43f-105">Bu belgede nasıl yapılacağı açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="cc43f-105">This document explains how to:</span></span>

* <span data-ttu-id="cc43f-106">GRPC istemcisini, gRPC hizmetlerini çağırmak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="cc43f-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="cc43f-107">GRPC 'yi birli, sunucu akışı, istemci akışı ve iki yönlü akış yöntemlerine göre çağırır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="cc43f-108">GRPC istemcisini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cc43f-108">Configure gRPC client</span></span>

<span data-ttu-id="cc43f-109">GRPC istemcileri [  *\*. proto* dosyalarından oluşturulan](xref:grpc/basics#generated-c-assets)somut istemci türleridir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="cc43f-110">Somut GRPC istemcisinde  *\*. proto* dosyasındaki GRPC hizmetine çeviren yöntemler vardır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="cc43f-111">Bir kanaldan gRPC istemcisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cc43f-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="cc43f-112">' I kullanarak `GrpcChannel.ForAddress` bir kanal oluşturun ve ardından bir GRPC istemcisi oluşturmak için kanalı kullanın:</span><span class="sxs-lookup"><span data-stu-id="cc43f-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="cc43f-113">Kanal, gRPC hizmetine uzun süreli bir bağlantıyı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="cc43f-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="cc43f-114">Bir kanal oluşturulduğunda, hizmet çağırma ile ilgili seçeneklerle yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="cc43f-115">Örneğin, `HttpClient` çağrı yapmak için kullanılan, en fazla ileti ve alma iletisi boyutu, ve ' de üzerinde `GrpcChannelOptions` `GrpcChannel.ForAddress`günlüğe kaydetme belirlenebilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="cc43f-116">Seçeneklerin tamamı listesi için bkz. [istemci yapılandırma seçenekleri](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="cc43f-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="cc43f-117">Kanal oluşturma maliyetli bir işlem olabilir ve gRPC çağrıları için bir kanalı yeniden kullanmak performans avantajları sunar.</span><span class="sxs-lookup"><span data-stu-id="cc43f-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="cc43f-118">Farklı türlerde istemciler de dahil olmak üzere bir kanaldan birden fazla somut gRPC istemcisi oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="cc43f-119">Somut gRPC istemci türleri basit nesnelerdir ve gerektiğinde oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="cc43f-120">`GrpcChannel.ForAddress`gRPC istemcisi oluşturmak için tek seçenek değildir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="cc43f-121">GRPC hizmetlerini bir ASP.NET Core uygulamasından arıyorsanız, [GRPC istemci fabrikası tümleştirmesini](xref:grpc/clientfactory)göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="cc43f-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="cc43f-122">ile GRPC tümleştirmesi `HttpClientFactory` , GRPC istemcileri oluşturmaya yönelik merkezi bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="cc43f-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="cc43f-123">GRPC çağrıları yapma</span><span class="sxs-lookup"><span data-stu-id="cc43f-123">Make gRPC calls</span></span>

<span data-ttu-id="cc43f-124">Bir gRPC çağrısı, istemcideki bir yöntem çağırarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-124">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="cc43f-125">GRPC istemcisi, ileti serileştirme işlemini işleyecek ve doğru hizmete gRPC çağrısını ele alacak.</span><span class="sxs-lookup"><span data-stu-id="cc43f-125">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="cc43f-126">gRPC farklı türde yöntemlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-126">gRPC has different types of methods.</span></span> <span data-ttu-id="cc43f-127">Bir gRPC çağrısını yapmak için istemcisini nasıl kullandığınız, çağırdığınız yöntemin türüne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-127">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="cc43f-128">GRPC Yöntem türleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cc43f-128">The gRPC method types are:</span></span>

* <span data-ttu-id="cc43f-129">Li</span><span class="sxs-lookup"><span data-stu-id="cc43f-129">Unary</span></span>
* <span data-ttu-id="cc43f-130">Sunucu akışı</span><span class="sxs-lookup"><span data-stu-id="cc43f-130">Server streaming</span></span>
* <span data-ttu-id="cc43f-131">İstemci akışı</span><span class="sxs-lookup"><span data-stu-id="cc43f-131">Client streaming</span></span>
* <span data-ttu-id="cc43f-132">İki yönlü akış</span><span class="sxs-lookup"><span data-stu-id="cc43f-132">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="cc43f-133">Birli çağrı</span><span class="sxs-lookup"><span data-stu-id="cc43f-133">Unary call</span></span>

<span data-ttu-id="cc43f-134">Birli çağrı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="cc43f-134">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="cc43f-135">Hizmet bittiğinde bir yanıt iletisi döndürülür.</span><span class="sxs-lookup"><span data-stu-id="cc43f-135">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="cc43f-136">. Proto dosyasındaki her birli hizmet yöntemi  *\** , yöntemi çağırmak için somut GRPC istemci türünde iki .NET yöntemi oluşmasına neden olur: zaman uyumsuz bir yöntem ve engelleyici bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="cc43f-136">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="cc43f-137">Örneğin, `GreeterClient` iki çağırma `SayHello`yöntemi vardır:</span><span class="sxs-lookup"><span data-stu-id="cc43f-137">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="cc43f-138">`GreeterClient.SayHelloAsync`-hizmeti `Greeter.SayHello` zaman uyumsuz olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-138">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="cc43f-139">Beklenmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-139">Can be awaited.</span></span>
* <span data-ttu-id="cc43f-140">`GreeterClient.SayHello`-tamamlanana `Greeter.SayHello` kadar hizmeti ve blokları çağırır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-140">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="cc43f-141">Zaman uyumsuz kodda kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="cc43f-141">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="cc43f-142">Sunucu akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="cc43f-142">Server streaming call</span></span>

<span data-ttu-id="cc43f-143">Sunucu akış çağrısı, istemci isteği iletisi gönderen ile başlar.</span><span class="sxs-lookup"><span data-stu-id="cc43f-143">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="cc43f-144">`ResponseStream.MoveNext()`hizmetten akan iletileri okur.</span><span class="sxs-lookup"><span data-stu-id="cc43f-144">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="cc43f-145">' İ `ResponseStream.MoveNext()` döndüğünde `false`sunucu akış çağrısı tamamlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-145">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // Greeting: Hello World" is streamed multiple times
    }
}
```

<span data-ttu-id="cc43f-146">C# 8 veya sonraki bir sürümünü kullanıyorsanız, `await foreach` söz konusu sözdizimi iletileri okumak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-146">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="cc43f-147">`IAsyncStreamReader<T>.ReadAllAsync()` Uzantı yöntemi, yanıt akışından gelen tüm iletileri okur:</span><span class="sxs-lookup"><span data-stu-id="cc43f-147">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is streamed multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="cc43f-148">İstemci akışı çağrısı</span><span class="sxs-lookup"><span data-stu-id="cc43f-148">Client streaming call</span></span>

<span data-ttu-id="cc43f-149">İstemci akış *çağrısı, istemci* İleti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-149">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cc43f-150">İstemci, ile `RequestStream.WriteAsync`ileti gönderip göndermemeyi seçebilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-150">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cc43f-151">İstemcinin iletiyi göndermesi tamamlandığında, hizmete bildirimde `RequestStream.CompleteAsync` bulunan iletiler çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-151">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="cc43f-152">Hizmet bir yanıt iletisi döndürdüğünde çağrı tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-152">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="cc43f-153">İki yönlü akış çağrısı</span><span class="sxs-lookup"><span data-stu-id="cc43f-153">Bi-directional streaming call</span></span>

<span data-ttu-id="cc43f-154">Çift yönlü bir akış *çağrısı, istemci* bir ileti göndermeden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-154">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="cc43f-155">İstemci, ile `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-155">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="cc43f-156">Hizmetten akışa alınan iletilere veya `ResponseStream.MoveNext()` `ResponseStream.ReadAllAsync()`ile erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-156">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="cc43f-157">İki yönlü akış çağrısı, `ResponseStream` daha fazla ileti olmadığında tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="cc43f-157">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="cc43f-158">Çift yönlü bir akış araması sırasında, istemci ve hizmet herhangi bir zamanda iletileri birbirlerine gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-158">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="cc43f-159">Çift yönlü bir çağrı ile etkileşimde bulunmak için en iyi istemci mantığı, hizmet mantığına bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="cc43f-159">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc43f-160">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cc43f-160">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
