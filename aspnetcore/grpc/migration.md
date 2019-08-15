---
title: GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme
author: juntaoluo
description: Mevcut bir C çekirdekli tabanlı gRPC uygulamasını ASP.NET Core yığının üstünde çalışacak şekilde taşımayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 39aa711a1a47cf11ec5b08903b4130c7caa1501c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022295"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="a07de-103">GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="a07de-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="a07de-104">[John Luo](https://github.com/juntaoluo) tarafından</span><span class="sxs-lookup"><span data-stu-id="a07de-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a07de-105">Temel alınan yığının uygulanması nedeniyle, tüm özellikler [C Core tabanlı GRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar arasında aynı şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="a07de-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="a07de-106">Bu belgede, iki yığın arasında geçiş yapmak için önemli farklılıklar vurgulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a07de-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="a07de-107">gRPC hizmeti uygulama ömrü</span><span class="sxs-lookup"><span data-stu-id="a07de-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="a07de-108">ASP.NET Core yığınında, varsayılan olarak gRPC Hizmetleri [kapsamlı bir ömür](xref:fundamentals/dependency-injection#service-lifetimes)ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a07de-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a07de-109">Buna karşılık, gRPC C-Core varsayılan olarak [tek bir yaşam süresine](xref:fundamentals/dependency-injection#service-lifetimes)sahip bir hizmete bağlanır.</span><span class="sxs-lookup"><span data-stu-id="a07de-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="a07de-110">Kapsamlı ömür, hizmet uygulamasının kapsamlı ömürlerle diğer hizmetleri çözümlemesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="a07de-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="a07de-111">Örneğin, kapsamlı bir yaşam süresi aynı zamanda, `DbContext` Oluşturucu ekleme yoluyla dı kapsayıcısından da çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="a07de-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="a07de-112">Kapsamlı ömür kullanımı:</span><span class="sxs-lookup"><span data-stu-id="a07de-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="a07de-113">Her istek için hizmet uygulamasının yeni bir örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a07de-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="a07de-114">Uygulama türündeki örnek üyeleri aracılığıyla istekler arasında durum paylaşmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="a07de-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="a07de-115">Beklentisi, paylaşılan durumları dı kapsayıcısında tek bir hizmette depobir biçimde depokadır.</span><span class="sxs-lookup"><span data-stu-id="a07de-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="a07de-116">Depolanan paylaşılan durumlar, gRPC hizmet uygulamasının oluşturucusunda çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="a07de-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="a07de-117">Hizmet yaşam süreleri hakkında daha fazla bilgi için <xref:fundamentals/dependency-injection#service-lifetimes>bkz.</span><span class="sxs-lookup"><span data-stu-id="a07de-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="a07de-118">Tek bir hizmet ekleyin</span><span class="sxs-lookup"><span data-stu-id="a07de-118">Add a singleton service</span></span>

<span data-ttu-id="a07de-119">GRPC C çekirdekli bir uygulamadan ASP.NET Core geçişi kolaylaştırmak için, hizmet uygulamasının hizmet ömrünü kapsamlı olarak tek başına değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="a07de-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="a07de-120">Bu, bir hizmet uygulamasının bir örneğini dı kapsayıcısına eklemeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="a07de-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="a07de-121">Ancak, tek bir yaşam süresine sahip bir hizmet uygulamasının kapsamı, kapsamlı hizmetleri Oluşturucu ekleme yoluyla çözemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="a07de-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="a07de-122">GRPC Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a07de-122">Configure gRPC services options</span></span>

<span data-ttu-id="a07de-123">C Core tabanlı uygulamalarda `grpc.max_receive_message_length` , ve `grpc.max_send_message_length` gibi ayarlar [sunucu örneği oluşturulurken](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)ile `ChannelOption` yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a07de-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="a07de-124">ASP.NET Core, GRPC, `GrpcServiceOptions` tür aracılığıyla yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a07de-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="a07de-125">Örneğin, bir gRPC hizmetinin en büyük gelen ileti boyutu şu kullanılarak `AddGrpc`yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="a07de-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="a07de-126">Yapılandırma hakkında daha fazla bilgi için bkz <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="a07de-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="a07de-127">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="a07de-127">Logging</span></span>

<span data-ttu-id="a07de-128">C çekirdekli tabanlı uygulamalar, `GrpcEnvironment` hata ayıklama amacıyla [günlükçüsü yapılandırmak](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a07de-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="a07de-129">ASP.NET Core Stack, bu işlevselliği [günlüğe kaydetme API 'si](xref:fundamentals/logging/index)aracılığıyla sağlar.</span><span class="sxs-lookup"><span data-stu-id="a07de-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="a07de-130">Örneğin, gRPC hizmetine Oluşturucu ekleme yoluyla bir günlükçü eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="a07de-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="a07de-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a07de-131">HTTPS</span></span>

<span data-ttu-id="a07de-132">C-Core tabanlı uygulamalar, [Server. Ports özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)aracılığıyla https 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a07de-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="a07de-133">Benzer bir kavram, ASP.NET Core sunucuları yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a07de-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="a07de-134">Örneğin, Kestrel Bu işlevsellik için [uç nokta yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a07de-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="a07de-135">Yakalayıcılar ve ara yazılım</span><span class="sxs-lookup"><span data-stu-id="a07de-135">Interceptors and Middleware</span></span>

<span data-ttu-id="a07de-136">ASP.NET Core [Ara yazılım](xref:fundamentals/middleware/index) , C çekirdekli tabanlı GRPC uygulamalarındaki yakalayıcılar ile karşılaştırıldığında benzer işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="a07de-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="a07de-137">Bir gRPC isteğini işleyen bir işlem hattı oluşturmak için, ara yazılım ve yakalayıcılar kavramsal olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="a07de-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="a07de-138">Bunlar her ikisi de iş hattındaki bir sonraki bileşenden önce veya sonra çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a07de-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="a07de-139">Ancak ASP.NET Core, ana bilgisayar, temel alınan HTTP/2 iletilerinde çalışır, ancak dinleyici yöneticileri [Servercallcontext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)kullanarak GRPC soyutlama katmanı üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a07de-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a07de-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a07de-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
