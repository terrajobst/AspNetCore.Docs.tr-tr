---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: ASP.NET Core ile gRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130700"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="6e92a-103">ASP.NET Core içeren gRPC Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="6e92a-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="6e92a-104">Bu belgede, ASP.NET Core kullanarak gRPC Hizmetleri ile çalışmaya başlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e92a-105">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6e92a-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e92a-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e92a-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e92a-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e92a-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e92a-108">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e92a-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="6e92a-109">ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="6e92a-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="6e92a-110">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6e92a-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e92a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e92a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e92a-112">GRPC projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [GRPC hizmetlerini kullanmaya başlama](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="6e92a-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e92a-113">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e92a-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6e92a-114">Komut `dotnet new grpc -o GrpcGreeter` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6e92a-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="6e92a-115">ASP.NET Core uygulamasına gRPC Hizmetleri ekleme</span><span class="sxs-lookup"><span data-stu-id="6e92a-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="6e92a-116">gRPC, [GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="6e92a-117">GRPC 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6e92a-117">Configure gRPC</span></span>

<span data-ttu-id="6e92a-118">*Startup.cs*içinde:</span><span class="sxs-lookup"><span data-stu-id="6e92a-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="6e92a-119">GRPC, `AddGrpc` yöntemiyle etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="6e92a-120">Her GRPC hizmeti, yönlendirme ardışık düzenine `MapGrpcService` yöntemi aracılığıyla eklenir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="6e92a-121">ASP.NET Core middlewares ve Özellikler yönlendirme işlem hattını paylaşır, bu nedenle uygulama ek istek işleyicileri sunacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="6e92a-122">MVC denetleyicileri gibi ek istek işleyicileri, yapılandırılmış gRPC hizmetleriyle paralel olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="6e92a-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="6e92a-123">Kestrel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6e92a-123">Configure Kestrel</span></span>

<span data-ttu-id="6e92a-124">Kestrel gRPC uç noktaları:</span><span class="sxs-lookup"><span data-stu-id="6e92a-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="6e92a-125">HTTP/2 gerektir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-125">Require HTTP/2.</span></span>
* <span data-ttu-id="6e92a-126">HTTPS ile güvenli hale getirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="6e92a-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="6e92a-127">HTTP/2</span></span>

<span data-ttu-id="6e92a-128">Kestrel çoğu modern işletim sisteminde [http/2 destekler](xref:fundamentals/servers/kestrel#http2-support) .</span><span class="sxs-lookup"><span data-stu-id="6e92a-128">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="6e92a-129">Kestrel uç noktaları, varsayılan olarak HTTP/1.1 ve HTTP/2 bağlantılarını destekleyecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="6e92a-129">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

> [!NOTE]
> <span data-ttu-id="6e92a-130">macOS, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)Ile ASP.NET Core GRPC 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="6e92a-130">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="6e92a-131">MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-131">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="6e92a-132">Daha fazla bilgi için bkz. [macOS üzerinde gRPC uygulaması ASP.NET Core başlatılamıyor](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="6e92a-132">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

#### <a name="https"></a><span data-ttu-id="6e92a-133">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6e92a-133">HTTPS</span></span>

<span data-ttu-id="6e92a-134">GRPC için kullanılan Kestrel uç noktaları HTTPS ile güvenli hale gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-134">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="6e92a-135">Geliştirme aşamasında, ASP.NET Core geliştirme sertifikası mevcut `https://localhost:5001` olduğunda otomatik olarak bir HTTPS uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6e92a-135">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="6e92a-136">Yapılandırma gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e92a-136">No configuration is required.</span></span>

<span data-ttu-id="6e92a-137">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-137">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="6e92a-138">Aşağıdaki *appSettings. JSON* ÖRNEĞINDE, https ile GÜVENLI bir http/2 uç noktası sağlanır:</span><span class="sxs-lookup"><span data-stu-id="6e92a-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="6e92a-139">Alternatif olarak, Kestrel endspoints *program.cs*içinde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="6e92a-139">Alternatively, Kestrel endspoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="6e92a-140">HTTP/2 ve HTTPS 'yi Kestrel ile etkinleştirme hakkında daha fazla bilgi için bkz. [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="6e92a-140">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="6e92a-141">ASP.NET Core API 'Leri ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="6e92a-141">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="6e92a-142">gRPC Hizmetleri, [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğe kaydetme](xref:fundamentals/logging/index)gibi ASP.NET Core özelliklerine tam erişime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-142">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="6e92a-143">Örneğin, hizmet uygulama, Oluşturucu aracılığıyla bir günlükçü hizmetini dı kapsayıcısından çözümleyebilir:</span><span class="sxs-lookup"><span data-stu-id="6e92a-143">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="6e92a-144">Varsayılan olarak, gRPC hizmeti uygulama diğer dı hizmetlerini herhangi bir yaşam süresi (tek, kapsamlı veya geçici) ile çözümleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6e92a-144">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="6e92a-145">GRPC yöntemlerinde HttpContext 'i çözümle</span><span class="sxs-lookup"><span data-stu-id="6e92a-145">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="6e92a-146">GRPC API 'SI, yöntem, ana bilgisayar, üst bilgi ve tanıtımları gibi bazı HTTP/2 ileti verilerine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="6e92a-146">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="6e92a-147">Erişim, her GRPC yöntemine geçirilen bağımsızdeğişkendir:`ServerCallContext`</span><span class="sxs-lookup"><span data-stu-id="6e92a-147">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="6e92a-148">`ServerCallContext`tüm ASP.NET API 'lerinde tam erişim `HttpContext` sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="6e92a-148">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="6e92a-149">Genişletme yöntemi, ASP.NET API 'lerinde temel alınan `HttpContext` http/2 iletisini temsil eden öğesine tam erişim sağlar: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="6e92a-149">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="6e92a-150">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6e92a-150">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
