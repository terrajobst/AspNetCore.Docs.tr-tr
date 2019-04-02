---
title: Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core
author: juntaoluo
description: ASP.NET Core yığının en üstünde çalıştırmak için mevcut bir C çekirdek tabanlı gRPC uygulamayı taşımayı öğreneceksiniz.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: ffe5ccbd99c6920e093eddc00fc60a9f66aab527
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809159"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="4798f-103">Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4798f-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="4798f-104">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="4798f-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="4798f-105">Temel alınan yığın uygulanması nedeniyle, tüm özellikler arasında aynı şekilde çalışır. [C-çekirdek tabanlı gRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="4798f-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="4798f-106">Bu belge, iki yığın arasında geçirmek için temel farklılıklar vurgular.</span><span class="sxs-lookup"><span data-stu-id="4798f-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="4798f-107">gRPC hizmet uygulama ömrü</span><span class="sxs-lookup"><span data-stu-id="4798f-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="4798f-108">ASP.NET Core yığınında gRPC Hizmetleri, varsayılan olarak, ile oluşturulan bir [kapsamlı ömrü](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="4798f-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="4798f-109">Buna karşılık, varsayılan olarak gRPC C çekirdekli bir hizmetle bağlar bir [tekil ömrü](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="4798f-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="4798f-110">Kapsamı belirlenmiş bir yaşam süresi, kapsamlı ömürleriyle diğer hizmetlerin çözümlenmesi hizmet uygulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="4798f-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="4798f-111">Örneğin, kapsamlı bir yaşam süresi de çözebilirsiniz `DBContext` Oluşturucu ekleme yoluyla DI kapsayıcısından.</span><span class="sxs-lookup"><span data-stu-id="4798f-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="4798f-112">Kapsamı belirlenmiş bir yaşam süresi'ı kullanma:</span><span class="sxs-lookup"><span data-stu-id="4798f-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="4798f-113">Hizmet uygulaması yeni bir örneğini her istek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4798f-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="4798f-114">Uygulama türü üzerindeki örnek üyelerinden keşfi arasında durum paylaşma mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4798f-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="4798f-115">Paylaşılan durumlar DI kapsayıcıdaki tek bir hizmet depolamak için kullanılan beklenir.</span><span class="sxs-lookup"><span data-stu-id="4798f-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="4798f-116">Depolanan paylaşılan durumlar gRPC hizmet uygulamasının oluşturucuda çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="4798f-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span> 

<span data-ttu-id="4798f-117">Hizmet ömrü hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="4798f-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="4798f-118">Tek bir hizmet ekleyin</span><span class="sxs-lookup"><span data-stu-id="4798f-118">Add a singleton service</span></span>

<span data-ttu-id="4798f-119">GRPC C çekirdek uygulamadan ASP.NET Core geçişi kolaylaştırmak için tekliye kapsamlı hizmet uygulamasından hizmet ömrü değiştirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4798f-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="4798f-120">Bu hizmet uygulaması örneğini DI kapsayıcıya ekleme içerir:</span><span class="sxs-lookup"><span data-stu-id="4798f-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="4798f-121">Ancak, bir singleton yaşam süresi ile bir hizmet uygulaması artık Oluşturucu ekleme kapsamlı hizmetleriyle çözümlemeleri değil.</span><span class="sxs-lookup"><span data-stu-id="4798f-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="4798f-122">GRPC Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4798f-122">Configure gRPC services options</span></span>

<span data-ttu-id="4798f-123">Ayarları gibi C-çekirdek tabanlı uygulamalarda `grpc.max_receive_message_length` ve `grpc.max_send_message_length` ile yapılandırılmış `ChannelOption` olduğunda [sunucu örneği oluşturmak](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="4798f-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="4798f-124">ASP.NET core'da `GrpcServiceOptions` bu ayarları yapılandırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="4798f-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="4798f-125">Ayarları, genel olarak tüm gRPC hizmetlere ya da bir bireysel hizmet uygulama türü için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4798f-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="4798f-126">Bireysel hizmet uygulaması türleri için belirtilen seçenekler yapılandırıldığında genel ayarları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4798f-126">Options specified for individual service implementation types override global settings when configured.</span></span>

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

## <a name="logging"></a><span data-ttu-id="4798f-127">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="4798f-127">Logging</span></span>

<span data-ttu-id="4798f-128">C-çekirdek tabanlı uygulamaları Bel `GrpcEnvironment` için [günlükçüden yapılandırma](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) hata ayıklama amacıyla.</span><span class="sxs-lookup"><span data-stu-id="4798f-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="4798f-129">ASP.NET Core yığını aracılığıyla bu işlevselliği sağlar [API'si günlük kaydını](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="4798f-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="4798f-130">Örneğin, bir Günlükçü Oluşturucu ekleme yoluyla gRPC hizmeti eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="4798f-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="4798f-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4798f-131">HTTPS</span></span>

<span data-ttu-id="4798f-132">C-çekirdek tabanlı uygulamalarını yapılandırma HTTPS üzerinden [Server.Ports özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="4798f-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="4798f-133">Benzer bir kavram, ASP.NET Core sunucularını yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4798f-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="4798f-134">Örneğin, Kestrel kullanır [uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) bu işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="4798f-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="4798f-135">Kesiciler ve ara yazılım</span><span class="sxs-lookup"><span data-stu-id="4798f-135">Interceptors and Middleware</span></span>

<span data-ttu-id="4798f-136">ASP.NET Core [ara yazılım](xref:fundamentals/middleware/index) gRPC C-çekirdek tabanlı uygulamalarda dinleyicileri ile karşılaştırıldığında benzer işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="4798f-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="4798f-137">GRPC isteğini işleyen bir işlem hattı oluşturmak için kullanılan her ikisi de olarak ara yazılım ve dinleyicileri kavramsal olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4798f-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="4798f-138">Her ikisi de, önce veya sonra ardışık düzende sonraki bileşene gerçekleştirilecek işin izin verin.</span><span class="sxs-lookup"><span data-stu-id="4798f-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="4798f-139">ASP.NET Core ara yazılım dinleyicileri soyutlama kullanmanın gRPC katmanda çalışması sırasında temel alınan HTTP/2 iletilerde ancak çalışır [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="4798f-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4798f-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4798f-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
