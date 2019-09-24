---
title: .NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama
author: jamesnk
description: .NET 'teki gRPC uygulamanızdan tanılamayı nasıl toplayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204347"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="3cc2e-103">.NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama</span><span class="sxs-lookup"><span data-stu-id="3cc2e-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="3cc2e-104">, [James bAyKiNg](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="3cc2e-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="3cc2e-105">Bu makalede, sorunları gidermeye yardımcı olmak için gRPC uygulamanızdan tanılamayı toplamaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="3cc2e-106">gRPC Hizmetleri günlüğü</span><span class="sxs-lookup"><span data-stu-id="3cc2e-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="3cc2e-107">Sunucu tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="3cc2e-108">Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="3cc2e-109">GRPC Hizmetleri ASP.NET Core üzerinde barındırıldığından, bu, ASP.NET Core günlük sistemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="3cc2e-110">Varsayılan yapılandırmada, gRPC çok az bilgiyi günlüğe kaydeder, ancak bu yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="3cc2e-111">ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#configuration) hakkındaki belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="3cc2e-112">GRPC, `Grpc` kategori altına Günlükler ekler.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="3cc2e-113">GRPC 'den ayrıntılı günlükleri etkinleştirmek için, aşağıdaki öğeleri `Grpc` `LogLevel` içindeki `Logging`alt bölümüne `Debug` ekleyerek, ön ekleri *appSettings. JSON* dosyanızdaki düzeye yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7)]

<span data-ttu-id="3cc2e-114">Bunu, *Startup.cs* `ConfigureLogging`içinde şu şekilde de yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

<span data-ttu-id="3cc2e-115">JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="3cc2e-116">İç içe yapılandırma değerlerinin nasıl belirleneceğini belirlemek için yapılandırma sisteminizin belgelerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="3cc2e-117">Örneğin, ortam değişkenleri kullanılırken, yerine `_` `:` iki karakter kullanılır (örneğin, `Logging__LogLevel__Grpc`).</span><span class="sxs-lookup"><span data-stu-id="3cc2e-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="3cc2e-118">Uygulamanız için daha ayrıntılı `Debug` tanılama toplanırken düzeyin kullanılmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="3cc2e-119">Düzey `Trace` , çok düşük düzey Tanılamalar üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="3cc2e-120">Örnek günlüğe kaydetme çıkışı</span><span class="sxs-lookup"><span data-stu-id="3cc2e-120">Sample logging output</span></span>

<span data-ttu-id="3cc2e-121">Aşağıda, bir GRPC hizmeti `Debug` düzeyinde konsol çıkışının bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a><span data-ttu-id="3cc2e-122">Sunucu tarafı günlüklerine erişin</span><span class="sxs-lookup"><span data-stu-id="3cc2e-122">Access server-side logs</span></span>

<span data-ttu-id="3cc2e-123">Sunucu tarafı günlüklerine erişme, çalıştırdığınız ortama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="3cc2e-124">Konsol uygulaması olarak</span><span class="sxs-lookup"><span data-stu-id="3cc2e-124">As a console app</span></span>

<span data-ttu-id="3cc2e-125">Konsol uygulamasında çalıştırıyorsanız, [konsol günlükçüsü](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="3cc2e-126">gRPC günlükleri konsolunda görünür.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="3cc2e-127">Diğer ortamlar</span><span class="sxs-lookup"><span data-stu-id="3cc2e-127">Other environments</span></span>

<span data-ttu-id="3cc2e-128">Uygulama başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) dağıtılırsa, ortama uygun günlük sağlayıcılarının nasıl <xref:fundamentals/logging/index> yapılandırılacağı hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="3cc2e-129">gRPC istemci günlüğü</span><span class="sxs-lookup"><span data-stu-id="3cc2e-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="3cc2e-130">İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="3cc2e-131">Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="3cc2e-132">.Net istemcisinden günlükleri almak için, `GrpcChannelOptions.LoggerFactory` özelliği istemci kanalının oluşturulduğu zaman ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="3cc2e-133">Bir ASP.NET Core uygulamasından gRPC hizmetini arıyorsanız, günlükçü fabrikası bağımlılık ekleme (DI) ' dan çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="3cc2e-134">İstemci günlüğünü etkinleştirmenin alternatif bir yolu, istemci oluşturmak için [GRPC istemci fabrikası](xref:grpc/clientfactory) kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="3cc2e-135">İstemci fabrikasına kayıtlı ve dı 'den çözümlenen bir gRPC istemcisi, otomatik olarak uygulamanın yapılandırılmış günlüğünü kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="3cc2e-136">Uygulamanız dı kullanıyorsa, `ILoggerFactory` [loggerfactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)ile yeni bir örnek oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="3cc2e-137">Bu yönteme erişmek için [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) paketini uygulamanıza ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3cc2e-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="3cc2e-138">Örnek günlüğe kaydetme çıkışı</span><span class="sxs-lookup"><span data-stu-id="3cc2e-138">Sample logging output</span></span>

<span data-ttu-id="3cc2e-139">Aşağıda, bir GRPC istemcisinin `Debug` düzeyindeki konsol çıkışının bir örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3cc2e-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="3cc2e-140">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3cc2e-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
