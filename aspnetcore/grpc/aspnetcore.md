---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: ASP.NET Core ile gRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 02e443dfecf2f7464a8ecabfc0cac67854d63232
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412479"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="a5cd8-103">ASP.NET Core içeren gRPC Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="a5cd8-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="a5cd8-104">Bu belgede, ASP.NET Core kullanarak gRPC Hizmetleri ile çalışmaya başlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5cd8-105">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a5cd8-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a5cd8-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5cd8-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a5cd8-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a5cd8-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a5cd8-108">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5cd8-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="a5cd8-109">ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a5cd8-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="a5cd8-110">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a5cd8-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a5cd8-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5cd8-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a5cd8-112">GRPC projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [GRPC hizmetlerini kullanmaya başlama](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="a5cd8-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a5cd8-113">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5cd8-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a5cd8-114">Komut `dotnet new grpc -o GrpcGreeter` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="a5cd8-115">ASP.NET Core uygulamasına gRPC Hizmetleri ekleme</span><span class="sxs-lookup"><span data-stu-id="a5cd8-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="a5cd8-116">gRPC, [GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="a5cd8-117">GRPC 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a5cd8-117">Configure gRPC</span></span>

<span data-ttu-id="a5cd8-118">GRPC, `AddGrpc` yöntemiyle etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="a5cd8-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="a5cd8-119">Her GRPC hizmeti yönlendirme ardışık düzenine `MapGrpcService` yöntemi aracılığıyla eklenir:</span><span class="sxs-lookup"><span data-stu-id="a5cd8-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="a5cd8-120">ASP.NET Core middlewares ve Özellikler yönlendirme işlem hattını paylaşır, bu nedenle uygulama ek istek işleyicileri sunacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="a5cd8-121">MVC denetleyicileri gibi ek istek işleyicileri, yapılandırılmış gRPC hizmetleriyle paralel olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="a5cd8-122">ASP.NET Core API 'Leri ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="a5cd8-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="a5cd8-123">gRPC Hizmetleri, [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğe kaydetme](xref:fundamentals/logging/index)gibi ASP.NET Core özelliklerine tam erişime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="a5cd8-124">Örneğin, hizmet uygulama, Oluşturucu aracılığıyla bir günlükçü hizmetini dı kapsayıcısından çözümleyebilir:</span><span class="sxs-lookup"><span data-stu-id="a5cd8-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="a5cd8-125">Varsayılan olarak, gRPC hizmeti uygulama diğer dı hizmetlerini herhangi bir yaşam süresi (tek, kapsamlı veya geçici) ile çözümleyebilir.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="a5cd8-126">GRPC yöntemlerinde HttpContext 'i çözümle</span><span class="sxs-lookup"><span data-stu-id="a5cd8-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="a5cd8-127">GRPC API 'SI, yöntem, ana bilgisayar, üst bilgi ve tanıtımları gibi bazı HTTP/2 ileti verilerine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="a5cd8-128">Erişim, her GRPC yöntemine geçirilen bağımsızdeğişkendir:`ServerCallContext`</span><span class="sxs-lookup"><span data-stu-id="a5cd8-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="a5cd8-129">`ServerCallContext`tüm ASP.NET API 'lerinde tam erişim `HttpContext` sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="a5cd8-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="a5cd8-130">Genişletme yöntemi, ASP.NET API 'lerinde temel alınan `HttpContext` http/2 iletisini temsil eden öğesine tam erişim sağlar: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="a5cd8-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="a5cd8-131">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a5cd8-131">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
