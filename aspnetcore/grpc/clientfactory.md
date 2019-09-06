---
title: .NET Core 'da gRPC istemci fabrikası tümleştirmesi
author: jamesnk
description: İstemci fabrikasını kullanarak gRPC istemcilerini oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/clientfactory
ms.openlocfilehash: a52fd397a7ed3e327938b0847af7f4e6a4a79400
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310648"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="394cf-103">.NET Core 'da gRPC istemci fabrikası tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="394cf-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="394cf-104">ile GRPC tümleştirmesi `HttpClientFactory` , GRPC istemcileri oluşturmaya yönelik merkezi bir yöntem sunar.</span><span class="sxs-lookup"><span data-stu-id="394cf-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="394cf-105">[Tek başına gRPC istemci örneklerini yapılandırmaya](xref:grpc/client)alternatif olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="394cf-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="394cf-106">Fabrika tümleştirmesi, [GRPC .net. ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet paketinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="394cf-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="394cf-107">Fabrika aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="394cf-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="394cf-108">Mantıksal gRPC istemci örneklerini yapılandırmak için merkezi bir konum sağlar</span><span class="sxs-lookup"><span data-stu-id="394cf-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="394cf-109">Temeldeki bir yaşam süresini yönetir`HttpClientMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="394cf-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="394cf-110">ASP.NET Core gRPC hizmetinde son tarih ve iptali otomatik olarak yayma</span><span class="sxs-lookup"><span data-stu-id="394cf-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="394cf-111">GRPC istemcilerini kaydetme</span><span class="sxs-lookup"><span data-stu-id="394cf-111">Register gRPC clients</span></span>

<span data-ttu-id="394cf-112">Bir GRPC istemcisini kaydetmek için, genel `AddGrpcClient` genişletme yöntemi içinde `Startup.ConfigureServices`, GRPC türü belirtilmiş istemci sınıfı ve hizmet adresi belirtilerek kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="394cf-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("http://localhost:5001");
});
```

<span data-ttu-id="394cf-113">GRPC istemci türü, bağımlılık ekleme (dı) ile geçici olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="394cf-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="394cf-114">İstemci artık, DI tarafından oluşturulan türlere eklenebilir ve doğrudan tüketilebilir.</span><span class="sxs-lookup"><span data-stu-id="394cf-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="394cf-115">ASP.NET Core MVC denetleyicileri, SignalR hub 'ları ve gRPC Hizmetleri, gRPC istemcilerinin otomatik olarak eklenebilir yer lardır:</span><span class="sxs-lookup"><span data-stu-id="394cf-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

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

## <a name="configure-httpclient"></a><span data-ttu-id="394cf-116">HttpClient 'ı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="394cf-116">Configure HttpClient</span></span>

<span data-ttu-id="394cf-117">`HttpClientFactory`GRPC istemcisi tarafından kullanılan öğesini oluşturur. `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="394cf-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="394cf-118">Standart `HttpClientFactory` Yöntemler `HttpClientHandler` ,`HttpClient`giden istek ara yazılımı eklemek veya öğesinin temelini yapılandırmak için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="394cf-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.BaseAddress = new Uri("http://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

<span data-ttu-id="394cf-119">Daha fazla bilgi için bkz. [ıhttpclientfactory kullanarak http Istekleri oluşturma](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="394cf-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="394cf-120">Kanalı ve Yakacıları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="394cf-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="394cf-121">gRPC 'ye özgü Yöntemler şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="394cf-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="394cf-122">Bir gRPC istemcisinin temel kanalını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="394cf-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="394cf-123">İstemcisinin `Interceptor` GRPC çağrıları yaparken kullanacağı örnekleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="394cf-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("http://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="394cf-124">Son Tarih ve iptal yayma</span><span class="sxs-lookup"><span data-stu-id="394cf-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="394cf-125">bir GRPC hizmetinde fabrika tarafından oluşturulan GRPC istemcileri, son tarih ve iptal belirtecini `EnableCallContextPropagation()` alt çağrılara otomatik olarak yaymak üzere ile yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="394cf-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="394cf-126">Genişletme yöntemi [GRPC. aspnetcore. Server. clientfactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet paketinde bulunur. `EnableCallContextPropagation()`</span><span class="sxs-lookup"><span data-stu-id="394cf-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="394cf-127">Çağrı bağlamı yayma, geçerli gRPC isteği bağlamından son tarih ve iptal belirtecini okuyarak ve bunları otomatik olarak gRPC istemcisi tarafından yapılan giden çağrılara yayarak işe yarar.</span><span class="sxs-lookup"><span data-stu-id="394cf-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="394cf-128">Çağrı bağlamı yayma, karmaşık, iç içe gRPC senaryolarının her zaman son tarihi ve iptali yaymasını sağlamaya yönelik mükemmel bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="394cf-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("http://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="394cf-129">Son tarihler ve RPC iptali hakkında daha fazla bilgi için bkz. [RPC yaşam döngüsü](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span><span class="sxs-lookup"><span data-stu-id="394cf-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="394cf-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="394cf-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
