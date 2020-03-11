---
title: .NET Core 'da gRPC istemci fabrikası tümleştirmesi
author: jamesnk
description: İstemci fabrikasını kullanarak gRPC istemcilerini oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667169"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>.NET Core 'da gRPC istemci fabrikası tümleştirmesi

`HttpClientFactory` ile gRPC tümleştirmesi, gRPC istemcilerinin oluşturulması için merkezi bir yol sunar. [Tek başına gRPC istemci örneklerini yapılandırmaya](xref:grpc/client)alternatif olarak kullanılabilir. Fabrika tümleştirmesi, [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet paketinde bulunabilir.

Fabrika aşağıdaki avantajları sunar:

* Mantıksal gRPC istemci örneklerini yapılandırmak için merkezi bir konum sağlar
* Temel alınan `HttpClientMessageHandler` ömrünü yönetir
* ASP.NET Core gRPC hizmetinde son tarih ve iptali otomatik olarak yayma

## <a name="register-grpc-clients"></a>GRPC istemcilerini kaydetme

Bir gRPC istemcisini kaydetmek için genel `AddGrpcClient` uzantısı yöntemi, `Startup.ConfigureServices`içinde, gRPC türü belirtilmiş istemci sınıfı ve hizmet adresi belirtilerek kullanılabilir:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

GRPC istemci türü, bağımlılık ekleme (dı) ile geçici olarak kaydedilir. İstemci artık, DI tarafından oluşturulan türlere eklenebilir ve doğrudan tüketilebilir. ASP.NET Core MVC denetleyicileri, SignalR hub 'lar ve gRPC Hizmetleri, gRPC istemcilerinin otomatik olarak eklenmesi için gereken yerdir:

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>HttpClient 'ı yapılandırma

`HttpClientFactory`, gRPC istemcisi tarafından kullanılan `HttpClient` oluşturur. Standart `HttpClientFactory` Yöntemler, giden istek ara yazılımı eklemek veya `HttpClient`temel `HttpClientHandler` yapılandırmak için kullanılabilir:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

Daha fazla bilgi için bkz. [ıhttpclientfactory kullanarak http Istekleri oluşturma](xref:fundamentals/http-requests).

## <a name="configure-channel-and-interceptors"></a>Kanalı ve Yakacıları yapılandırma

gRPC 'ye özgü Yöntemler şu şekilde kullanılabilir:

* Bir gRPC istemcisinin temel kanalını yapılandırın.
* GRPC çağrıları yaparken İstemcinin kullanacağı `Interceptor` örnekleri ekleyin.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>Son Tarih ve iptal yayma

bir gRPC hizmetinde fabrika tarafından oluşturulan gRPC istemcileri, son tarih ve iptal belirtecini alt çağrılara otomatik olarak yaymak için `EnableCallContextPropagation()` ile yapılandırılabilir. `EnableCallContextPropagation()` uzantısı yöntemi [GRPC. AspNetCore. Server. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet paketinde bulunur.

Çağrı bağlamı yayma, geçerli gRPC isteği bağlamından son tarih ve iptal belirtecini okuyarak ve bunları otomatik olarak gRPC istemcisi tarafından yapılan giden çağrılara yayarak işe yarar. Çağrı bağlamı yayma, karmaşık, iç içe gRPC senaryolarının her zaman son tarihi ve iptali yaymasını sağlamaya yönelik mükemmel bir yoldur.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

Son tarihler ve RPC iptali hakkında daha fazla bilgi için bkz. [RPC yaşam döngüsü](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
