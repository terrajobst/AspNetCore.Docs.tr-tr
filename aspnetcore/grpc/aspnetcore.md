---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: ASP.NET Core ile gRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 28e6b8589bbe0b6a3723b64736c723c883302571
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238170"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="31300-103">ASP.NET Core içeren gRPC Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="31300-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="31300-104">Bu belgede, ASP.NET Core kullanarak gRPC Hizmetleri ile çalışmaya başlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="31300-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31300-105">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="31300-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31300-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31300-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="31300-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31300-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="31300-108">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31300-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="31300-109">ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="31300-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="31300-110">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="31300-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31300-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31300-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="31300-112">GRPC projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [GRPC hizmetlerini kullanmaya başlama](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="31300-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="31300-113">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31300-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="31300-114">Komut `dotnet new grpc -o GrpcGreeter` satırından çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="31300-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="31300-115">ASP.NET Core uygulamasına gRPC Hizmetleri ekleme</span><span class="sxs-lookup"><span data-stu-id="31300-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="31300-116">gRPC, [GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) paketini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="31300-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="31300-117">GRPC 'yi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="31300-117">Configure gRPC</span></span>

<span data-ttu-id="31300-118">*Startup.cs*içinde:</span><span class="sxs-lookup"><span data-stu-id="31300-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="31300-119">GRPC, `AddGrpc` yöntemiyle etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="31300-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="31300-120">Her GRPC hizmeti, yönlendirme ardışık düzenine `MapGrpcService` yöntemi aracılığıyla eklenir.</span><span class="sxs-lookup"><span data-stu-id="31300-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="31300-121">ASP.NET Core middlewares ve Özellikler yönlendirme işlem hattını paylaşır, bu nedenle uygulama ek istek işleyicileri sunacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="31300-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="31300-122">MVC denetleyicileri gibi ek istek işleyicileri, yapılandırılmış gRPC hizmetleriyle paralel olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="31300-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="31300-123">Kestrel yapılandırma</span><span class="sxs-lookup"><span data-stu-id="31300-123">Configure Kestrel</span></span>

<span data-ttu-id="31300-124">Kestrel gRPC uç noktaları:</span><span class="sxs-lookup"><span data-stu-id="31300-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="31300-125">HTTP/2 gerektir.</span><span class="sxs-lookup"><span data-stu-id="31300-125">Require HTTP/2.</span></span>
* <span data-ttu-id="31300-126">HTTPS ile güvenli hale getirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="31300-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="31300-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="31300-127">HTTP/2</span></span>

<span data-ttu-id="31300-128">gRPC, HTTP/2 gerektirir.</span><span class="sxs-lookup"><span data-stu-id="31300-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="31300-129">ASP.NET Core için gRPC, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) olduğunu `HTTP/2`doğrular.</span><span class="sxs-lookup"><span data-stu-id="31300-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="31300-130">Kestrel çoğu modern işletim sisteminde [http/2 destekler](xref:fundamentals/servers/kestrel#http2-support) .</span><span class="sxs-lookup"><span data-stu-id="31300-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="31300-131">Kestrel uç noktaları, varsayılan olarak HTTP/1.1 ve HTTP/2 bağlantılarını destekleyecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="31300-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="https"></a><span data-ttu-id="31300-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="31300-132">HTTPS</span></span>

<span data-ttu-id="31300-133">GRPC için kullanılan Kestrel uç noktaları HTTPS ile güvenli hale gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="31300-133">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="31300-134">Geliştirme aşamasında, ASP.NET Core geliştirme sertifikası mevcut `https://localhost:5001` olduğunda otomatik olarak bir HTTPS uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="31300-134">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="31300-135">Yapılandırma gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="31300-135">No configuration is required.</span></span>

<span data-ttu-id="31300-136">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="31300-136">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="31300-137">Aşağıdaki *appSettings. JSON* ÖRNEĞINDE, https ile GÜVENLI bir http/2 uç noktası sağlanır:</span><span class="sxs-lookup"><span data-stu-id="31300-137">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

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

<span data-ttu-id="31300-138">Alternatif olarak, Kestrel uç noktaları *program.cs*içinde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="31300-138">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

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

<span data-ttu-id="31300-139">HTTP/2 uç noktası HTTPS olmadan yapılandırıldığında, uç noktanın [Listenoptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) olarak `HttpProtocols.Http2`ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="31300-139">When an HTTP/2 endpoint is configured without HTTPS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="31300-140">`HttpProtocols.Http1AndHttp2`HTTP/2 üzerinde anlaşmak için HTTPS gerekli olduğundan kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="31300-140">`HttpProtocols.Http1AndHttp2` can't be used because HTTPS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="31300-141">HTTPS olmadan, uç nokta varsayılan HTTP/1.1 ve gRPC çağrıları için tüm bağlantılar başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="31300-141">Without HTTPS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="31300-142">HTTP/2 ve HTTPS 'yi Kestrel ile etkinleştirme hakkında daha fazla bilgi için bkz. [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="31300-142">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="31300-143">macOS, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)Ile ASP.NET Core GRPC 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="31300-143">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="31300-144">MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="31300-144">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="31300-145">Daha fazla bilgi için bkz. [macOS üzerinde gRPC uygulaması ASP.NET Core başlatılamıyor](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="31300-145">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="31300-146">ASP.NET Core API 'Leri ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="31300-146">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="31300-147">gRPC Hizmetleri, [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğe kaydetme](xref:fundamentals/logging/index)gibi ASP.NET Core özelliklerine tam erişime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="31300-147">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="31300-148">Örneğin, hizmet uygulama, Oluşturucu aracılığıyla bir günlükçü hizmetini dı kapsayıcısından çözümleyebilir:</span><span class="sxs-lookup"><span data-stu-id="31300-148">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="31300-149">Varsayılan olarak, gRPC hizmeti uygulama diğer dı hizmetlerini herhangi bir yaşam süresi (tek, kapsamlı veya geçici) ile çözümleyebilir.</span><span class="sxs-lookup"><span data-stu-id="31300-149">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="31300-150">GRPC yöntemlerinde HttpContext 'i çözümle</span><span class="sxs-lookup"><span data-stu-id="31300-150">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="31300-151">GRPC API 'SI, yöntem, ana bilgisayar, üst bilgi ve tanıtımları gibi bazı HTTP/2 ileti verilerine erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="31300-151">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="31300-152">Erişim, her GRPC yöntemine geçirilen bağımsızdeğişkendir:`ServerCallContext`</span><span class="sxs-lookup"><span data-stu-id="31300-152">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="31300-153">`ServerCallContext`tüm ASP.NET API 'lerinde tam erişim `HttpContext` sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="31300-153">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="31300-154">Genişletme yöntemi, ASP.NET API 'lerinde temel alınan `HttpContext` http/2 iletisini temsil eden öğesine tam erişim sağlar: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="31300-154">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="31300-155">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="31300-155">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
