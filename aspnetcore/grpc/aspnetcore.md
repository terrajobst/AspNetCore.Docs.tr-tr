---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: GRPC Hizmetleri ile ASP.NET Core yazarken, temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 1f019fac23982a95fa37d43099522f4b3e9d107a
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039278"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="78434-103">ASP.NET Core içeren gRPC Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="78434-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="78434-104">Bu belge, ASP.NET Core kullanarak gRPC Services'i kullanmaya başlama işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="78434-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="78434-105">ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="78434-105">Get started with gRPC service in ASP.NET Core</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="78434-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78434-106">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="78434-107">Bkz: [gRPC Services'i kullanmaya başlama](xref:tutorials/grpc/grpc-start) gRPC proje oluşturma konusunda ayrıntılı yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="78434-107">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="78434-108">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="78434-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="78434-109">Çalıştırma `dotnet new grpc -o GrpcGreeter` komut satırından.</span><span class="sxs-lookup"><span data-stu-id="78434-109">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="78434-110">GRPC Hizmetleri için ASP.NET Core uygulaması Ekle</span><span class="sxs-lookup"><span data-stu-id="78434-110">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="78434-111">gRPC aşağıdaki paketler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="78434-111">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="78434-112">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="78434-112">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="78434-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) API'leri için protobuf iletisi.</span><span class="sxs-lookup"><span data-stu-id="78434-113">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="78434-114">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="78434-114">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="78434-115">GRPC yapılandırın</span><span class="sxs-lookup"><span data-stu-id="78434-115">Configure gRPC</span></span>

<span data-ttu-id="78434-116">gRPC ile etkin `AddGrpc` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="78434-116">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="78434-117">Yönlendirme işlem hattı her gRPC hizmet eklenir `MapGrpcService` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="78434-117">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

<span data-ttu-id="78434-118">ASP.NET Core middlewares ve özellikleri yönlendirme işlem hattı paylaşın ve bu nedenle uygulama ek istek işleyicileri sunmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="78434-118">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="78434-119">MVC denetleyicileri gibi ek istek işleyicileri yapılandırılmış gRPC hizmetleriyle paralel çalışır.</span><span class="sxs-lookup"><span data-stu-id="78434-119">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="78434-120">ASP.NET Core API'ları ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="78434-120">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="78434-121">gRPC Hizmetleri olan ASP.NET Core özelliklerine tam erişim gibi [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğü](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="78434-121">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="78434-122">Örneğin, hizmet uygulaması DI kapsayıcı Oluşturucu Günlükçü hizmetinden çözebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="78434-122">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="78434-123">Varsayılan olarak, diğer tüm yaşam süresi (Singleton, kapsamındaki veya geçici) DI hizmetleriyle gRPC hizmet uygulamasında çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="78434-123">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="78434-124">GRPC yöntemleri HttpContext çözümleyin</span><span class="sxs-lookup"><span data-stu-id="78434-124">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="78434-125">GRPC API yöntemi, konak, başlığı ve tanıtımları gibi bazı HTTP/2 ileti verilere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="78434-125">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="78434-126">Erişimi olan aracılığıyla `ServerCallContext` her gRPC yöntemine geçirilen bağımsız değişken:</span><span class="sxs-lookup"><span data-stu-id="78434-126">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="78434-127">`ServerCallContext` tam erişim sağlamaz `HttpContext` tüm ASP.NET API'lerindeki.</span><span class="sxs-lookup"><span data-stu-id="78434-127">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="78434-128">`GetHttpContext` Genişletme yöntemi için tam erişim sağlar `HttpContext` ASP.NET API'leri temel alınan HTTP/2 iletiyi temsil eden:</span><span class="sxs-lookup"><span data-stu-id="78434-128">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="78434-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="78434-129">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
