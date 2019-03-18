---
title: Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core
author: juntaoluo
description: ASP.NET Core yığının en üstünde çalıştırmak için mevcut bir C çekirdek tabanlı gRPC uygulamayı taşımayı öğreneceksiniz.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/migration
ms.openlocfilehash: 520318cfe87708cf5c453c88a5eb54027350db4b
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142626"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="cac1f-103">Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cac1f-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="cac1f-104">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="cac1f-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="cac1f-105">Temel alınan yığın uygulanması nedeniyle, tüm özellikler arasında aynı şekilde çalışır. [C-çekirdek tabanlı gRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-105">Due to implementation of the underlying stack, not all features work in the same way between [C-core based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core based apps.</span></span> <span data-ttu-id="cac1f-106">Bu belge iki yığınları arasında geçiş yaparken dikkat edilecek önemli farklar vurgulanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cac1f-106">This document highlights the key differences to note when migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="cac1f-107">gRPC hizmet uygulama ömrü</span><span class="sxs-lookup"><span data-stu-id="cac1f-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="cac1f-108">ASP.NET Core yığınında gRPC Hizmetleri, varsayılan olarak, ile oluşturulur bir [kapsamındaki ömrü](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cac1f-108">In the ASP.NET Core stack, gRPC services, by default, will be created with a [Scoped lifetime](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cac1f-109">Buna karşılık, varsayılan olarak gRPC C çekirdekli bir Singleton yaşam süresi ile bir hizmeti bağlar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-109">In contrast, gRPC C-core by default binds to a service with a Singleton lifetime.</span></span>

<span data-ttu-id="cac1f-110">Kapsamındaki ömrü kapsamındaki ömürleriyle diğer hizmetlerin çözümlenmesi hizmet uygulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-110">A Scoped lifetime allows the service implementation to resolve other services with Scoped lifetimes.</span></span> <span data-ttu-id="cac1f-111">Örneğin kapsamındaki yaşam süresi de çözebilirsiniz `DBContext`, oluşturucu ekleme yoluyla DI kapsayıcısından.</span><span class="sxs-lookup"><span data-stu-id="cac1f-111">For example Scoped lifetime can also resolve `DBContext`, from the DI container through constructor injection.</span></span> <span data-ttu-id="cac1f-112">Kapsamındaki ömrü kullanarak:</span><span class="sxs-lookup"><span data-stu-id="cac1f-112">Using Scoped lifetime:</span></span>

* <span data-ttu-id="cac1f-113">Hizmet uygulaması yeni bir örneğini her istek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cac1f-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="cac1f-114">Uygulama türü üzerindeki örnek üyelerinden keşfi arasında durum paylaşma mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="cac1f-114">It's not possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="cac1f-115">Paylaşılan durumlar DI kapsayıcıdaki tek bir hizmet depolamak için kullanılan beklenir.</span><span class="sxs-lookup"><span data-stu-id="cac1f-115">The expectation is to store shared states in a Singleton service in the DI container.</span></span> <span data-ttu-id="cac1f-116">Depolanan paylaşılan durumlar gRPC hizmet uygulamasının oluşturucuda çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="cac1f-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span> 

<span data-ttu-id="cac1f-117">Kapsamındaki ve tekil ömrü hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="cac1f-117">For more information on Scoped and Singleton lifetime, see <xref:fundamentals/dependency-injection>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="cac1f-118">Tek bir hizmet ekleyin</span><span class="sxs-lookup"><span data-stu-id="cac1f-118">Add a singleton service</span></span>

<span data-ttu-id="cac1f-119">GRPC C çekirdek uygulamadan ASP.NET Core geçişi kolaylaştırmak için hizmet uygulamasının hizmet ömrü kapsamındaki tekliye değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="cac1f-119">To facilitate the transition from gRPC C-core implementation to ASP.NET Core, it is possible to change the service lifetime of the service implementation from Scoped to Singleton.</span></span> <span data-ttu-id="cac1f-120">Bu hizmet uygulaması örneğini DI kapsayıcıya ekleme içerir:</span><span class="sxs-lookup"><span data-stu-id="cac1f-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="cac1f-121">Ancak, tekil ömrü ile hizmet uygulaması artık kapsamındaki Hizmetleri Oluşturucu ekleme yoluyla çözmek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cac1f-121">However, service implementation with Singleton lifetime will no longer be able to resolve Scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="cac1f-122">GRPC Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cac1f-122">Configure gRPC services options</span></span>

<span data-ttu-id="cac1f-123">C-çekirdek tabanlı uygulamalar, ayarlar gibi `grpc.max_receive_message_length` ve `grpc.max_send_message_length` ile yapılandırılmış `ChannelOption` olduğunda [oluşturmak `Server` örneği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="cac1f-123">In C-core based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the `Server` instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="cac1f-124">ASP.NET core'da `GrpcServiceOptions` bu ayarları yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="cac1f-125">Ayarları, genel olarak tüm gRPC hizmetlere ya da bir bireysel hizmet uygulama türü için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="cac1f-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="cac1f-126">Bireysel hizmet uygulaması türleri için belirtilen seçenekler yapılandırıldığında genel ayarları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-126">Options specified for individual service implementation types will override global settings when configured.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a><span data-ttu-id="cac1f-127">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="cac1f-127">Logging</span></span>

<span data-ttu-id="cac1f-128">C / çekirdek tabanlı uygulamalarınızı Bel `GrpcEnvironment` için [günlükçüden yapılandırma](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="cac1f-128">C-core based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="cac1f-129">ASP.NET Core yığını aracılığıyla bu işlevselliği sağlar [günlüğe kaydetme API'si](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="cac1f-129">The ASP.NET Core stack provides this functionality through the [logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="cac1f-130">Örneğin bir Günlükçü Oluşturucu ekleme yoluyla gRPC hizmeti eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="cac1f-130">For example a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="cac1f-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cac1f-131">HTTPS</span></span>

<span data-ttu-id="cac1f-132">C / çekirdek tabanlı uygulamalarınızı yapılandırma HTTPS üzerinden [ `Server.Ports` özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="cac1f-132">C-core based apps configure HTTPS through the [`Server.Ports` property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="cac1f-133">Benzer bir kavram, ASP.NET Core sunucularını yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cac1f-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="cac1f-134">Örneğin, Kestrel kullanır [uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) bu işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="cac1f-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middlewares"></a><span data-ttu-id="cac1f-135">Kesiciler ve Middlewares</span><span class="sxs-lookup"><span data-stu-id="cac1f-135">Interceptors and Middlewares</span></span>

<span data-ttu-id="cac1f-136">ASP.NET Core [middlewares](xref:fundamentals/middleware/index) tabanlı gRPC uygulamaları C core'da dinleyicileri karşılaştırdığınızda benzer işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="cac1f-136">ASP.NET Core [middlewares](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core based gRPC apps.</span></span> <span data-ttu-id="cac1f-137">GRPC isteğini işleyen bir pipleline oluşturmak için kullanılan her ikisi de olarak Middlewares ve dinleyicileri kavramsal olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="cac1f-137">Middlewares and interceptors are conceptually the same as both are used to construct a pipleline that handles a gRPC request.</span></span> <span data-ttu-id="cac1f-138">Her ikisi de, önce veya sonra ardışık düzende sonraki bileşene gerçekleştirilecek işin izin verin.</span><span class="sxs-lookup"><span data-stu-id="cac1f-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="cac1f-139">ASP.NET Core middlewares dinleyicileri soyutlama kullanmanın gRPC katmanda çalışır ancak temel alınan HTTP/2 iletileri ancak çalışması [ `ServerCallContext` ](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="cac1f-139">However, ASP.NET Core middlewares operate on the underlying HTTP/2 messages whereas interceptors operate on the gRPC layer of abstraction using the [`ServerCallContext`](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cac1f-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="cac1f-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
