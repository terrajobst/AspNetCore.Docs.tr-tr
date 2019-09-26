---
title: GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme
author: juntaoluo
description: Mevcut bir C çekirdekli tabanlı gRPC uygulamasını ASP.NET Core yığının üstünde çalışacak şekilde taşımayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 8f0d9dd980fa3281f30dc29d329d10ccd352ae72
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278708"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="0586a-103">GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="0586a-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="0586a-104">[John Luo](https://github.com/juntaoluo) tarafından</span><span class="sxs-lookup"><span data-stu-id="0586a-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="0586a-105">Temel alınan yığının uygulanması nedeniyle, tüm özellikler [C Core tabanlı GRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar arasında aynı şekilde çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="0586a-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="0586a-106">Bu belgede, iki yığın arasında geçiş yapmak için önemli farklılıklar vurgulanmıştır.</span><span class="sxs-lookup"><span data-stu-id="0586a-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="0586a-107">gRPC hizmeti uygulama ömrü</span><span class="sxs-lookup"><span data-stu-id="0586a-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="0586a-108">ASP.NET Core yığınında, varsayılan olarak gRPC Hizmetleri [kapsamlı bir ömür](xref:fundamentals/dependency-injection#service-lifetimes)ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0586a-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="0586a-109">Buna karşılık, gRPC C-Core varsayılan olarak [tek bir yaşam süresine](xref:fundamentals/dependency-injection#service-lifetimes)sahip bir hizmete bağlanır.</span><span class="sxs-lookup"><span data-stu-id="0586a-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="0586a-110">Kapsamlı ömür, hizmet uygulamasının kapsamlı ömürlerle diğer hizmetleri çözümlemesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="0586a-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="0586a-111">Örneğin, kapsamlı bir yaşam süresi aynı zamanda, `DbContext` Oluşturucu ekleme yoluyla dı kapsayıcısından da çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="0586a-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="0586a-112">Kapsamlı ömür kullanımı:</span><span class="sxs-lookup"><span data-stu-id="0586a-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="0586a-113">Her istek için hizmet uygulamasının yeni bir örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="0586a-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="0586a-114">Uygulama türündeki örnek üyeleri aracılığıyla istekler arasında durum paylaşmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="0586a-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="0586a-115">Beklentisi, paylaşılan durumları dı kapsayıcısında tek bir hizmette depobir biçimde depokadır.</span><span class="sxs-lookup"><span data-stu-id="0586a-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="0586a-116">Depolanan paylaşılan durumlar, gRPC hizmet uygulamasının oluşturucusunda çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="0586a-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="0586a-117">Hizmet yaşam süreleri hakkında daha fazla bilgi için <xref:fundamentals/dependency-injection#service-lifetimes>bkz.</span><span class="sxs-lookup"><span data-stu-id="0586a-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="0586a-118">Tek bir hizmet ekleyin</span><span class="sxs-lookup"><span data-stu-id="0586a-118">Add a singleton service</span></span>

<span data-ttu-id="0586a-119">GRPC C çekirdekli bir uygulamadan ASP.NET Core geçişi kolaylaştırmak için, hizmet uygulamasının hizmet ömrünü kapsamlı olarak tek başına değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="0586a-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="0586a-120">Bu, bir hizmet uygulamasının bir örneğini dı kapsayıcısına eklemeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="0586a-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="0586a-121">Ancak, tek bir yaşam süresine sahip bir hizmet uygulamasının kapsamı, kapsamlı hizmetleri Oluşturucu ekleme yoluyla çözemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="0586a-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="0586a-122">GRPC Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0586a-122">Configure gRPC services options</span></span>

<span data-ttu-id="0586a-123">C Core tabanlı uygulamalarda `grpc.max_receive_message_length` , ve `grpc.max_send_message_length` gibi ayarlar [sunucu örneği oluşturulurken](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)ile `ChannelOption` yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="0586a-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="0586a-124">ASP.NET Core, GRPC, `GrpcServiceOptions` tür aracılığıyla yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="0586a-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="0586a-125">Örneğin, gRPC hizmetinin en büyük gelen ileti boyutu ile `AddGrpc`yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0586a-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="0586a-126">Aşağıdaki örnek 4 MB ile 16 `ReceiveMaxMessageSize` MB arasında varsayılan değer değiştirir:</span><span class="sxs-lookup"><span data-stu-id="0586a-126">The following example changes the default `ReceiveMaxMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="0586a-127">Yapılandırma hakkında daha fazla bilgi için bkz <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="0586a-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="0586a-128">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="0586a-128">Logging</span></span>

<span data-ttu-id="0586a-129">C çekirdekli tabanlı uygulamalar, `GrpcEnvironment` hata ayıklama amacıyla [günlükçüsü yapılandırmak](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) için kullanır.</span><span class="sxs-lookup"><span data-stu-id="0586a-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="0586a-130">ASP.NET Core Stack, bu işlevselliği [günlüğe kaydetme API 'si](xref:fundamentals/logging/index)aracılığıyla sağlar.</span><span class="sxs-lookup"><span data-stu-id="0586a-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="0586a-131">Örneğin, gRPC hizmetine Oluşturucu ekleme yoluyla bir günlükçü eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="0586a-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="0586a-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="0586a-132">HTTPS</span></span>

<span data-ttu-id="0586a-133">C-Core tabanlı uygulamalar, [Server. Ports özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)aracılığıyla https 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="0586a-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="0586a-134">Benzer bir kavram, ASP.NET Core sunucuları yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0586a-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="0586a-135">Örneğin, Kestrel Bu işlevsellik için [uç nokta yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) kullanır.</span><span class="sxs-lookup"><span data-stu-id="0586a-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="0586a-136">Yakalayıcılar ve ara yazılım</span><span class="sxs-lookup"><span data-stu-id="0586a-136">Interceptors and Middleware</span></span>

<span data-ttu-id="0586a-137">ASP.NET Core [Ara yazılım](xref:fundamentals/middleware/index) , C çekirdekli tabanlı GRPC uygulamalarındaki yakalayıcılar ile karşılaştırıldığında benzer işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="0586a-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="0586a-138">Bir gRPC isteğini işleyen bir işlem hattı oluşturmak için, ara yazılım ve yakalayıcılar kavramsal olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0586a-138">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="0586a-139">Bunlar her ikisi de iş hattındaki bir sonraki bileşenden önce veya sonra çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="0586a-139">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="0586a-140">Ancak ASP.NET Core, ana bilgisayar, temel alınan HTTP/2 iletilerinde çalışır, ancak dinleyici yöneticileri [Servercallcontext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)kullanarak GRPC soyutlama katmanı üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="0586a-140">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0586a-141">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="0586a-141">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
