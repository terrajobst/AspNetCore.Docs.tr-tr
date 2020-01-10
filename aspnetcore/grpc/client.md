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
# <a name="call-grpc-services-with-the-net-client"></a>.NET istemcisiyle gRPC hizmetlerini çağırma

[GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet paketinde bir .net GRPC istemci kitaplığı mevcuttur. Bu belgede nasıl yapılacağı açıklanmaktadır:

* GRPC istemcisini, gRPC hizmetlerini çağırmak üzere yapılandırın.
* GRPC 'yi birli, sunucu akışı, istemci akışı ve iki yönlü akış yöntemlerine göre çağırır.

## <a name="configure-grpc-client"></a>GRPC istemcisini yapılandırma

gRPC istemcileri [ *\*. proto* dosyalarından oluşturulan](xref:grpc/basics#generated-c-assets)somut istemci türleridir. Somut gRPC istemcisinde *\*. proto* dosyasındaki GRPC hizmetine çeviren yöntemler vardır.

Bir kanaldan gRPC istemcisi oluşturulur. Kanal oluşturmak için `GrpcChannel.ForAddress` kullanarak başlayın ve ardından bir gRPC istemcisi oluşturmak için kanalı kullanın:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Kanal, gRPC hizmetine uzun süreli bir bağlantıyı temsil eder. Bir kanal oluşturulduğunda, hizmet çağırma ile ilgili seçeneklerle yapılandırılır. Örneğin, çağrı yapmak için kullanılan `HttpClient`, en fazla ileti ve alma iletisi boyutu ve günlüğe kaydetme `GrpcChannelOptions` ve `GrpcChannel.ForAddress`ile kullanılabilir. Seçeneklerin tamamı listesi için bkz. [istemci yapılandırma seçenekleri](xref:grpc/configuration#configure-client-options).

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

Kanal ve istemci performansı ve kullanımı:

* Kanal oluşturma maliyetli bir işlem olabilir. GRPC çağrıları için bir kanalı yeniden kullanmak performans avantajları sağlar.
* gRPC istemcileri kanallarla oluşturulur. gRPC istemcileri hafif nesnelerdir ve önbelleğe alınması veya yeniden kullanılması gerekmez.
* Farklı istemci türleri dahil olmak üzere bir kanaldan birden çok gRPC istemcisi oluşturulabilir.
* Kanaldan oluşturulan bir kanal ve istemciler, birden çok iş parçacığı tarafından güvenli bir şekilde kullanılabilir.
* Kanaldan oluşturulan istemciler birden çok eş zamanlı çağrı yapabilir.

`GrpcChannel.ForAddress`, gRPC istemcisi oluşturmak için tek seçenek değildir. GRPC hizmetlerini bir ASP.NET Core uygulamasından arıyorsanız, [GRPC istemci fabrikası tümleştirmesini](xref:grpc/clientfactory)göz önünde bulundurun. `HttpClientFactory` ile gRPC tümleştirmesi, gRPC istemcileri oluşturmaya yönelik merkezi bir alternatif sunar.

> [!NOTE]
> [.Net istemcisiyle güvenli olmayan gRPC hizmetlerini çağırmak](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)için ek yapılandırma gerekir.

## <a name="make-grpc-calls"></a>GRPC çağrıları yapma

Bir gRPC çağrısı, istemcideki bir yöntem çağırarak başlatılır. GRPC istemcisi, ileti serileştirme işlemini işleyecek ve doğru hizmete gRPC çağrısını ele alacak.

gRPC farklı türde yöntemlere sahiptir. Bir gRPC çağrısını yapmak için istemcisini nasıl kullandığınız, çağırdığınız yöntemin türüne bağlıdır. GRPC Yöntem türleri şunlardır:

* Li
* Sunucu akışı
* İstemci akışı
* İki yönlü akış

### <a name="unary-call"></a>Birli çağrı

Birli çağrı, istemci isteği iletisi gönderen ile başlar. Hizmet bittiğinde bir yanıt iletisi döndürülür.

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

*\*. proto* dosyasındaki her birli hizmet yöntemi, yöntemi çağırmak Için somut GRPC istemci türünde iki .net yönteminin oluşmasına neden olur: zaman uyumsuz bir yöntem ve engelleyici bir yöntem. Örneğin, `GreeterClient` `SayHello`çağırmanın iki yolu vardır:

* `GreeterClient.SayHelloAsync`-`Greeter.SayHello` hizmeti zaman uyumsuz olarak çağırır. Beklenmiş olabilir.
* `GreeterClient.SayHello`-`Greeter.SayHello` hizmeti ve blokları tamamlanana kadar çağırır. Zaman uyumsuz kodda kullanmayın.

### <a name="server-streaming-call"></a>Sunucu akışı çağrısı

Sunucu akış çağrısı, istemci isteği iletisi gönderen ile başlar. `ResponseStream.MoveNext()` hizmetten akan iletileri okur. `ResponseStream.MoveNext()` `false`döndürdüğünde sunucu akış çağrısı tamamlanır.

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

C# 8 veya üzeri bir sürüm kullanıyorsanız, `await foreach` söz dizimi iletileri okumak için kullanılabilir. `IAsyncStreamReader<T>.ReadAllAsync()` uzantısı yöntemi, yanıt akışındaki tüm iletileri okur:

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

### <a name="client-streaming-call"></a>İstemci akışı çağrısı

İstemci akış *çağrısı, istemci* İleti göndermeden başlatılır. İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir. İstemci ileti göndermeyi bitirdiğinde `RequestStream.CompleteAsync` hizmete bildirmek için çağrılmalıdır. Hizmet bir yanıt iletisi döndürdüğünde çağrı tamamlanır.

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

### <a name="bi-directional-streaming-call"></a>İki yönlü akış çağrısı

Çift yönlü bir akış *çağrısı, istemci* bir ileti göndermeden başlatılır. İstemci `RequestStream.WriteAsync`ileti göndermek için seçim yapabilir. Hizmetten akışa alınan iletilere `ResponseStream.MoveNext()` veya `ResponseStream.ReadAllAsync()`erişilebilir. `ResponseStream` daha fazla ileti olmadığında çift yönlü akış çağrısı tamamlanmıştır.

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

Çift yönlü bir akış araması sırasında, istemci ve hizmet herhangi bir zamanda iletileri birbirlerine gönderebilir. Çift yönlü bir çağrı ile etkileşimde bulunmak için en iyi istemci mantığı, hizmet mantığına bağlı olarak farklılık gösterir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
