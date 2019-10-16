---
title: ASP.NET Core Web sunucusu uygulamasını Kestrel
author: guardrex
description: ASP.NET Core platformlar arası Web sunucusu olan Kestrel hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 5565011f6531ef5e95eb02f310e7107f9ed547b2
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378875"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="4db18-103">ASP.NET Core Web sunucusu uygulamasını Kestrel</span><span class="sxs-lookup"><span data-stu-id="4db18-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="4db18-104">[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher)No, [Stephen halter](https://twitter.com/halter73)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="4db18-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4db18-105">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="4db18-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="4db18-106">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="4db18-107">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="4db18-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="4db18-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4db18-108">HTTPS</span></span>
* <span data-ttu-id="4db18-109">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="4db18-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="4db18-110">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="4db18-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="4db18-111">HTTP/2 (macOS @ no__t-0 hariç)</span><span class="sxs-lookup"><span data-stu-id="4db18-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="4db18-112">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="4db18-113">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db18-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="4db18-114">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4db18-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="4db18-115">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="4db18-115">HTTP/2 support</span></span>

<span data-ttu-id="4db18-116">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="4db18-117">İşletim sistemi @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="4db18-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="4db18-118">Windows Server 2016/Windows 10 veya üzeri @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="4db18-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="4db18-119">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="4db18-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="4db18-120">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4db18-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="4db18-121">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="4db18-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="4db18-122">TLS 1,2 veya üzeri bağlantı</span><span class="sxs-lookup"><span data-stu-id="4db18-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="4db18-123">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="4db18-124">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="4db18-125">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="4db18-126">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="4db18-127">HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="4db18-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="4db18-128">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="4db18-129">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="4db18-130">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="4db18-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="4db18-131">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4db18-132">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="4db18-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="4db18-133">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="4db18-135">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4db18-137">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="4db18-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="4db18-138">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="4db18-139">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="4db18-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="4db18-140">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="4db18-141">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="4db18-142">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="4db18-142">A reverse proxy:</span></span>

* <span data-ttu-id="4db18-143">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="4db18-144">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="4db18-145">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="4db18-146">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="4db18-147">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-148">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="4db18-149">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="4db18-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="4db18-150">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="4db18-151">*Program.cs*' de, şablon kodu, arka planda @no__t 2 ' yi çağıran <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="4db18-152">@No__t-0 ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/generic-host#set-up-a-host> ' ün *ana bilgisayar* ve *Varsayılan Oluşturucu ayarları* bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="4db18-153">@No__t-0 ve `ConfigureWebHostDefaults` çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel` kullanın:</span><span class="sxs-lookup"><span data-stu-id="4db18-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="4db18-154">Uygulamayı ayarlamak için uygulama `CreateDefaultBuilder` ' ı çağırmazsa, @no__t 3 ' ü çağırmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ' i çağırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="4db18-155">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-155">Kestrel options</span></span>

<span data-ttu-id="4db18-156">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="4db18-157">@No__t-1 sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğinde kısıtlamalar ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="4db18-158">@No__t-0 özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="4db18-159">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4db18-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="4db18-160">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="4db18-161">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4db18-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="4db18-162">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="4db18-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="4db18-163">@No__t-0 ' da Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="4db18-164">@No__t-1 sınıfına `IConfiguration` ' ın bir örneğini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="4db18-165">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="4db18-166">@No__t-0 ' da, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="4db18-167">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="4db18-168">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="4db18-169">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="4db18-170">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="4db18-171">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="4db18-172">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4db18-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="4db18-173">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="4db18-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="4db18-174">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="4db18-175">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="4db18-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="4db18-176">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="4db18-177">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="4db18-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="4db18-178">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="4db18-179">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="4db18-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="4db18-180">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="4db18-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="4db18-181">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="4db18-182">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="4db18-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="4db18-183">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4db18-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="4db18-184">@No__t-1 özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` özelliği vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="4db18-185">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="4db18-186">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="4db18-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="4db18-187">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="4db18-188">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="4db18-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="4db18-189">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="4db18-190">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="4db18-191">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="4db18-192">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="4db18-193">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="4db18-194">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="4db18-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="4db18-195">, İstek çoğullama için protokol desteği nedeniyle, önceki örnekte başvurulan `HttpContext.Features`, HTTP/2 istekleri için  ' de mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="4db18-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="4db18-196">Ancak, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature>, HTTP/2 istekleri için `HttpContext.Features` ' i hala mevcut olduğundan, bir HTTP/2 isteği için bile `IHttpMinRequestBodyDataRateFeature.MinDataRate` ' e `null` ' e ayarlanarak, okuma hızı sınırı tamamen istek temelli olarak *devre dışı* bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="4db18-197">@No__t okumaya veya `null` ' den farklı bir değere ayarlamaya çalışmak, HTTP/2 isteği verildiğinde @no__t 2 ' nin oluşmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="4db18-198">@No__t-0 ile yapılandırılan sunucu genelindeki hız sınırları, HTTP/1. x ve HTTP/2 bağlantıları için hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="4db18-199">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="4db18-200">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="4db18-201">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="4db18-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="4db18-202">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="4db18-202">Maximum streams per connection</span></span>

<span data-ttu-id="4db18-203">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="4db18-204">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="4db18-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="4db18-205">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="4db18-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="4db18-206">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-206">Header table size</span></span>

<span data-ttu-id="4db18-207">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="4db18-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="4db18-208">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="4db18-209">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="4db18-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="4db18-210">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="4db18-211">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-211">Maximum frame size</span></span>

<span data-ttu-id="4db18-212">`Http2.MaxFrameSize`, sunucu tarafından alınan veya gönderilen HTTP/2 bağlantı çerçevesi yükünün izin verilen en büyük boyutunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="4db18-213">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="4db18-214">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="4db18-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="4db18-215">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-215">Maximum request header size</span></span>

<span data-ttu-id="4db18-216">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="4db18-217">Bu sınır, sıkıştırılmış ve sıkıştırılmamış temsillerinde hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="4db18-218">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="4db18-219">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="4db18-220">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-220">Initial connection window size</span></span>

<span data-ttu-id="4db18-221">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="4db18-222">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="4db18-223">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="4db18-224">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="4db18-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="4db18-225">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-225">Initial stream window size</span></span>

<span data-ttu-id="4db18-226">`Http2.InitialStreamWindowSize`, istek (Stream) başına tek seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="4db18-227">İstekler aynı zamanda `Http2.InitialConnectionWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="4db18-228">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="4db18-229">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="4db18-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="4db18-230">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="4db18-230">Synchronous IO</span></span>

<span data-ttu-id="4db18-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="4db18-232">Varsayılan değer `false` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="4db18-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-233">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="4db18-234">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4db18-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="4db18-235">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="4db18-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="4db18-236">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="4db18-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="4db18-237">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-237">Endpoint configuration</span></span>

<span data-ttu-id="4db18-238">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="4db18-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="4db18-239">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="4db18-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="4db18-240">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="4db18-240">Specify URLs using the:</span></span>

* <span data-ttu-id="4db18-241">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="4db18-242">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="4db18-243">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="4db18-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="4db18-244">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4db18-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="4db18-245">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4db18-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="4db18-246">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="4db18-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="4db18-247">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="4db18-248">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="4db18-248">A development certificate is created:</span></span>

* <span data-ttu-id="4db18-249">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="4db18-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="4db18-250">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="4db18-251">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="4db18-252">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="4db18-253">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="4db18-254">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="4db18-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="4db18-255">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="4db18-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="4db18-256">ConfigureEndpointDefaults Varsayılanları (eylem @ no__t-0ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="4db18-257">Belirtilen her bir uç nokta için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="4db18-258">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="4db18-259">ConfigureHttpsDefaults (eylem @ no__t-0HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="4db18-260">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="4db18-261">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

### <a name="configureiconfiguration"></a><span data-ttu-id="4db18-262">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="4db18-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="4db18-263">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="4db18-264">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="4db18-265">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="4db18-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="4db18-266">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="4db18-267">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="4db18-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="4db18-268">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="4db18-269">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-269">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="4db18-270">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="4db18-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="4db18-271">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="4db18-272">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="4db18-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="4db18-273">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="4db18-274">@No__t-0 döndürür.</span><span class="sxs-lookup"><span data-stu-id="4db18-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="4db18-275">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="4db18-276">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="4db18-277">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="4db18-278">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="4db18-279">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="4db18-280">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="4db18-281">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="4db18-282">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-282">Supported configurations described next:</span></span>

* <span data-ttu-id="4db18-283">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="4db18-283">No configuration</span></span>
* <span data-ttu-id="4db18-284">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="4db18-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="4db18-285">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="4db18-285">Change the defaults in code</span></span>

<span data-ttu-id="4db18-286">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="4db18-286">*No configuration*</span></span>

<span data-ttu-id="4db18-287">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="4db18-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="4db18-288">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="4db18-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="4db18-289">`CreateDefaultBuilder`, Kestrel yapılandırmasını yüklemek için varsayılan olarak-1 ' i @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="4db18-290">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="4db18-291">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="4db18-292">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="4db18-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="4db18-293">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="4db18-294">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="4db18-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
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

<span data-ttu-id="4db18-295">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="4db18-296">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="4db18-297">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="4db18-297">Schema notes:</span></span>

* <span data-ttu-id="4db18-298">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="4db18-299">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="4db18-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="4db18-300">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="4db18-301">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="4db18-302">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="4db18-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="4db18-303">@No__t-0 yoluyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="4db18-304">@No__t-0 bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="4db18-305">@No__t-0 bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="4db18-306">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="4db18-307">@No__t-0 bölümü **, hem @no__t**-2**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="4db18-308">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="4db18-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="4db18-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="4db18-310">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="4db18-311">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="4db18-312">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="4db18-313">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="4db18-314">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="4db18-315">`KestrelConfigurationLoader`, `Endpoint` aşırı yüklemeleri olarak `KestrelServerOptions` ' den API 'nin @no__t ailesini yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="4db18-316">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="4db18-317">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="4db18-317">*Change the defaults in code*</span></span>

<span data-ttu-id="4db18-318">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="4db18-319">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });

    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.SslProtocols = SslProtocols.Tls12;
    });
});
```

<span data-ttu-id="4db18-320">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="4db18-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="4db18-321">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="4db18-322">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="4db18-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="4db18-323">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="4db18-324">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="4db18-325">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="4db18-326">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="4db18-326">SNI support requires:</span></span>

* <span data-ttu-id="4db18-327">@No__t-0 veya üzeri hedef çerçeve üzerinde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="4db18-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="4db18-328">@No__t-0 veya sonraki bir sürümde, geri çağırma çağrılır, ancak `name` her zaman `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="4db18-329">@No__t-0, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="4db18-330">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="4db18-331">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ListenAnyIP(5005, listenOptions =>
    {
        listenOptions.UseHttps(httpsOptions =>
        {
            var localhostCert = CertificateLoader.LoadFromStoreCert(
                "localhost", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var exampleCert = CertificateLoader.LoadFromStoreCert(
                "example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var subExampleCert = CertificateLoader.LoadFromStoreCert(
                "sub.example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var certs = new Dictionary<string, X509Certificate2>(
                StringComparer.OrdinalIgnoreCase);
            certs["localhost"] = localhostCert;
            certs["example.com"] = exampleCert;
            certs["sub.example.com"] = subExampleCert;

            httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
            {
                if (name != null && certs.TryGetValue(name, out var cert))
                {
                    return cert;
                }

                return exampleCert;
            };
        });
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="4db18-332">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-332">Bind to a TCP socket</span></span>

<span data-ttu-id="4db18-333">@No__t-0 yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="4db18-333">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="4db18-334">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-334">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="4db18-335">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-335">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="4db18-336">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-336">Bind to a Unix socket</span></span>

<span data-ttu-id="4db18-337">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-337">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="4db18-338">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="4db18-338">Port 0</span></span>

<span data-ttu-id="4db18-339">@No__t-0 olan bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-339">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="4db18-340">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4db18-340">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="4db18-341">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4db18-341">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="4db18-342">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="4db18-342">Limitations</span></span>

<span data-ttu-id="4db18-343">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-343">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="4db18-344">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-344">`--urls` command-line argument</span></span>
* <span data-ttu-id="4db18-345">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="4db18-345">`urls` host configuration key</span></span>
* <span data-ttu-id="4db18-346">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-346">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="4db18-347">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-347">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="4db18-348">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="4db18-348">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="4db18-349">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-349">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="4db18-350">@No__t-0 ve `UseUrls` yaklaşımlarının ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4db18-350">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="4db18-351">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-351">IIS endpoint configuration</span></span>

<span data-ttu-id="4db18-352">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-352">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="4db18-353">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-353">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="4db18-354">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="4db18-354">ListenOptions.Protocols</span></span>

<span data-ttu-id="4db18-355">@No__t-0 özelliği, bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-355">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="4db18-356">@No__t-1 numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-356">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="4db18-357">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="4db18-357">`HttpProtocols` enum value</span></span> | <span data-ttu-id="4db18-358">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="4db18-358">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="4db18-359">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="4db18-359">HTTP/1.1 only.</span></span> <span data-ttu-id="4db18-360">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-360">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="4db18-361">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="4db18-361">HTTP/2 only.</span></span> <span data-ttu-id="4db18-362">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-362">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="4db18-363">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="4db18-363">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="4db18-364">HTTP/2, istemcinin TLS [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) el sıkışmasında http/2 seçmesini gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-364">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="4db18-365">Herhangi bir uç nokta için varsayılan `ListenOptions.Protocols` değeri `HttpProtocols.Http1AndHttp2` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-365">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="4db18-366">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="4db18-366">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="4db18-367">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4db18-367">TLS version 1.2 or later</span></span>
* <span data-ttu-id="4db18-368">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="4db18-368">Renegotiation disabled</span></span>
* <span data-ttu-id="4db18-369">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="4db18-369">Compression disabled</span></span>
* <span data-ttu-id="4db18-370">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="4db18-370">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="4db18-371">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="4db18-371">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="4db18-372">Sınırlı alan Diffie-Hellman (DHE) &lbrack; @ no__t-1 @ no__t-2 &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="4db18-372">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="4db18-373">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="4db18-373">Cipher suite not blacklisted</span></span>

<span data-ttu-id="4db18-374">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` `TLS-ECDHE` @ no__t-3 ' ü P-256 eliptik eğrisi &lbrack; @ no__t-5 @ no__t-6 varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db18-374">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="4db18-375">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="4db18-375">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="4db18-376">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="4db18-376">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="4db18-377">Gerektiğinde belirli şifrelemeler için TLS el sıkışmaları için bağlantı temelinde filtre uygulamak üzere bağlantı ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-377">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="4db18-378">Aşağıdaki örnek, uygulamanın desteklemediği herhangi bir şifre algoritması için <xref:System.NotSupportedException> ' i oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-378">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="4db18-379">Alternatif olarak, [ılshandshakefeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) öğesini kabul edilebilir şifreleme paketleri listesi ile tanımlayın ve karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-379">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="4db18-380">[CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) şifre algoritması ile hiçbir şifreleme kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-380">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="4db18-381">Bağlantı filtrelemesi, bir <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda aracılığıyla da yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-381">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="4db18-382">Linux 'ta <xref:System.Net.Security.CipherSuitesPolicy>, TLS el sıkışmaları her bağlantı temelinde filtrelemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-382">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="4db18-383">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="4db18-383">*Set the protocol from configuration*</span></span>

<span data-ttu-id="4db18-384">`CreateDefaultBuilder`, Kestrel yapılandırmasını yüklemek için varsayılan olarak-1 ' i @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-384">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="4db18-385">Aşağıdaki *appSettings. JSON* örneği, tüm uç noktalar için varsayılan bağlantı protokolü olarak http/1.1 oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4db18-385">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="4db18-386">Aşağıdaki *appSettings. JSON* örneği, belirli bir uç nokta için http/1.1 bağlantı protokolünü belirler:</span><span class="sxs-lookup"><span data-stu-id="4db18-386">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="4db18-387">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="4db18-387">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="4db18-388">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-388">Transport configuration</span></span>

<span data-ttu-id="4db18-389">Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="4db18-389">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="4db18-390">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-390">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="4db18-391">@No__t-1 üzerinde <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-391">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="4db18-392">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-392">URL prefixes</span></span>

<span data-ttu-id="4db18-393">@No__t-0, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-393">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="4db18-394">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-394">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="4db18-395">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-395">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="4db18-396">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-396">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="4db18-397">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-397">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="4db18-398">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-398">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="4db18-399">`[::]`, IPv4 `0.0.0.0` ' in IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="4db18-399">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="4db18-400">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-400">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="4db18-401">@No__t-0 ve `+` ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="4db18-401">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="4db18-402">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-402">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="4db18-403">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-403">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="4db18-404">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-404">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="4db18-405">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si @no__t ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-405">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="4db18-406">@No__t-0 belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-406">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="4db18-407">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-407">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="4db18-408">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4db18-408">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="4db18-409">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="4db18-409">Host filtering</span></span>

<span data-ttu-id="4db18-410">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-410">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="4db18-411">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-411">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="4db18-412">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-412">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="4db18-413">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="4db18-413">`Host` headers aren't validated.</span></span>

<span data-ttu-id="4db18-414">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-414">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="4db18-415">Ana bilgisayar filtreleme ara yazılımı, ASP.NET Core uygulamaları için örtük olarak sunulan [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-415">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="4db18-416">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4db18-416">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="4db18-417">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-417">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="4db18-418">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4db18-418">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="4db18-419">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="4db18-419">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="4db18-420">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4db18-420">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="4db18-421">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-421">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="4db18-422">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-422">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="4db18-423">@No__t-0 ' i Iletilen üstbilgiler ara yazılımı ile, istekleri bir ters ara sunucu veya yük dengeleyiciye iletirken `Host` üst bilgisi korunmazsa uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-423">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="4db18-424">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-424">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="4db18-425">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4db18-425">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="4db18-426">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="4db18-426">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="4db18-427">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-427">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="4db18-428">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="4db18-428">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="4db18-429">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4db18-429">HTTPS</span></span>
* <span data-ttu-id="4db18-430">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="4db18-430">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="4db18-431">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="4db18-431">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="4db18-432">HTTP/2 (macOS @ no__t-0 hariç)</span><span class="sxs-lookup"><span data-stu-id="4db18-432">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="4db18-433">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-433">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="4db18-434">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db18-434">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="4db18-435">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4db18-435">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="4db18-436">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="4db18-436">HTTP/2 support</span></span>

<span data-ttu-id="4db18-437">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-437">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="4db18-438">İşletim sistemi @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="4db18-438">Operating system&dagger;</span></span>
  * <span data-ttu-id="4db18-439">Windows Server 2016/Windows 10 veya üzeri @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="4db18-439">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="4db18-440">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="4db18-440">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="4db18-441">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4db18-441">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="4db18-442">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="4db18-442">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="4db18-443">TLS 1,2 veya üzeri bağlantı</span><span class="sxs-lookup"><span data-stu-id="4db18-443">TLS 1.2 or later connection</span></span>

<span data-ttu-id="4db18-444">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-444">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="4db18-445">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-445">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="4db18-446">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-446">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="4db18-447">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-447">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="4db18-448">HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="4db18-448">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="4db18-449">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-449">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="4db18-450">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-450">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="4db18-451">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="4db18-451">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="4db18-452">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-452">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4db18-453">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="4db18-453">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="4db18-454">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-454">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="4db18-456">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-456">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4db18-458">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="4db18-458">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="4db18-459">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-459">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="4db18-460">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="4db18-460">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="4db18-461">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-461">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="4db18-462">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-462">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="4db18-463">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="4db18-463">A reverse proxy:</span></span>

* <span data-ttu-id="4db18-464">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-464">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="4db18-465">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-465">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="4db18-466">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-466">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="4db18-467">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-467">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="4db18-468">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-468">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-469">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-469">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="4db18-470">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="4db18-470">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="4db18-471">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="4db18-471">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4db18-472">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-472">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="4db18-473">*Program.cs*' de, şablon kodu, arka planda @no__t 2 ' yi çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-473">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="4db18-474">@No__t-0 ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host> ' nin *konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-474">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="4db18-475">@No__t-0 ' ı çağırdıktan sonra ek yapılandırma sağlamak için, `ConfigureKestrel` ' i kullanın:</span><span class="sxs-lookup"><span data-stu-id="4db18-475">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="4db18-476">Uygulamayı ayarlamak için uygulama `CreateDefaultBuilder` ' ı çağırmazsa, @no__t 3 ' ü çağırmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ' i çağırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-476">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="4db18-477">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-477">Kestrel options</span></span>

<span data-ttu-id="4db18-478">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-478">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="4db18-479">@No__t-1 sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğinde kısıtlamalar ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-479">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="4db18-480">@No__t-0 özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-480">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="4db18-481">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4db18-481">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="4db18-482">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-482">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="4db18-483">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4db18-483">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="4db18-484">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="4db18-484">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="4db18-485">@No__t-0 ' da Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-485">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="4db18-486">@No__t-1 sınıfına `IConfiguration` ' ın bir örneğini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-486">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="4db18-487">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-487">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="4db18-488">@No__t-0 ' da, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-488">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="4db18-489">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-489">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="4db18-490">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-490">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="4db18-491">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-491">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="4db18-492">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-492">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="4db18-493">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-493">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="4db18-494">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4db18-494">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="4db18-495">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="4db18-495">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="4db18-496">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-496">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="4db18-497">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="4db18-497">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="4db18-498">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-498">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="4db18-499">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="4db18-499">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="4db18-500">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-500">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="4db18-501">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="4db18-501">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="4db18-502">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="4db18-502">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="4db18-503">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-503">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="4db18-504">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="4db18-504">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="4db18-505">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4db18-505">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="4db18-506">@No__t-1 özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` özelliği vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-506">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="4db18-507">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-507">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="4db18-508">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="4db18-508">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="4db18-509">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-509">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="4db18-510">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="4db18-510">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="4db18-511">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-511">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="4db18-512">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-512">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="4db18-513">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-513">A minimum rate also applies to the response.</span></span> <span data-ttu-id="4db18-514">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-514">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="4db18-515">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-515">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="4db18-516">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="4db18-516">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="4db18-517">İstek çoğullama için protokol desteğinin desteklenmediği için, önceki örnekte başvurulan oran özelliği, http/2 istekleri için `HttpContext.Features` ' da mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="4db18-517">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="4db18-518">@No__t-0 ile yapılandırılan sunucu genelindeki hız sınırları, HTTP/1. x ve HTTP/2 bağlantıları için hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-518">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="4db18-519">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-519">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="4db18-520">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-520">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="4db18-521">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="4db18-521">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="4db18-522">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="4db18-522">Maximum streams per connection</span></span>

<span data-ttu-id="4db18-523">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-523">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="4db18-524">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="4db18-524">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="4db18-525">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="4db18-525">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="4db18-526">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-526">Header table size</span></span>

<span data-ttu-id="4db18-527">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="4db18-527">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="4db18-528">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-528">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="4db18-529">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="4db18-529">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="4db18-530">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-530">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="4db18-531">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-531">Maximum frame size</span></span>

<span data-ttu-id="4db18-532">`Http2.MaxFrameSize`, alacak HTTP/2 bağlantı çerçevesi yükünün en büyük boyutunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-532">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="4db18-533">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-533">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="4db18-534">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="4db18-534">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="4db18-535">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-535">Maximum request header size</span></span>

<span data-ttu-id="4db18-536">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-536">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="4db18-537">Bu sınır, hem sıkıştırılmış hem de sıkıştırılmamış temsillerinde birlikte hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-537">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="4db18-538">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-538">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="4db18-539">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-539">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="4db18-540">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-540">Initial connection window size</span></span>

<span data-ttu-id="4db18-541">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-541">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="4db18-542">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-542">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="4db18-543">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-543">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="4db18-544">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="4db18-544">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="4db18-545">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-545">Initial stream window size</span></span>

<span data-ttu-id="4db18-546">`Http2.InitialStreamWindowSize`, istek (Stream) başına tek seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-546">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="4db18-547">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="4db18-548">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="4db18-549">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="4db18-549">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="4db18-550">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="4db18-550">Synchronous IO</span></span>

<span data-ttu-id="4db18-551"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-551"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="4db18-552">Varsayılan değer `true` ' dır.</span><span class="sxs-lookup"><span data-stu-id="4db18-552">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-553">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-553">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="4db18-554">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4db18-554">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="4db18-555">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="4db18-555">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="4db18-556">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="4db18-556">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="4db18-557">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-557">Endpoint configuration</span></span>

<span data-ttu-id="4db18-558">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="4db18-558">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="4db18-559">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="4db18-559">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="4db18-560">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="4db18-560">Specify URLs using the:</span></span>

* <span data-ttu-id="4db18-561">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-561">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="4db18-562">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-562">`--urls` command-line argument.</span></span>
* <span data-ttu-id="4db18-563">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="4db18-563">`urls` host configuration key.</span></span>
* <span data-ttu-id="4db18-564">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4db18-564">`UseUrls` extension method.</span></span>

<span data-ttu-id="4db18-565">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4db18-565">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="4db18-566">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="4db18-566">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="4db18-567">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-567">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="4db18-568">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="4db18-568">A development certificate is created:</span></span>

* <span data-ttu-id="4db18-569">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="4db18-569">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="4db18-570">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-570">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="4db18-571">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-571">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="4db18-572">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-572">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="4db18-573">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-573">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="4db18-574">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="4db18-574">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="4db18-575">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="4db18-575">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="4db18-576">ConfigureEndpointDefaults Varsayılanları (eylem @ no__t-0ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-576">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="4db18-577">Belirtilen her bir uç nokta için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-577">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="4db18-578">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-578">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="4db18-579">ConfigureHttpsDefaults (eylem @ no__t-0HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-579">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="4db18-580">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-580">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="4db18-581">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-581">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="4db18-582">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="4db18-582">Configure(IConfiguration)</span></span>

<span data-ttu-id="4db18-583">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-583">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="4db18-584">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-584">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="4db18-585">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="4db18-585">ListenOptions.UseHttps</span></span>

<span data-ttu-id="4db18-586">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-586">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="4db18-587">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="4db18-587">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="4db18-588">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-588">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="4db18-589">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-589">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="4db18-590">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="4db18-590">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="4db18-591">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-591">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="4db18-592">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="4db18-592">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="4db18-593">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-593">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="4db18-594">@No__t-0 döndürür.</span><span class="sxs-lookup"><span data-stu-id="4db18-594">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="4db18-595">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-595">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="4db18-596">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-596">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="4db18-597">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-597">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="4db18-598">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-598">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="4db18-599">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-599">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="4db18-600">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-600">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="4db18-601">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-601">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="4db18-602">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-602">Supported configurations described next:</span></span>

* <span data-ttu-id="4db18-603">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="4db18-603">No configuration</span></span>
* <span data-ttu-id="4db18-604">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="4db18-604">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="4db18-605">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="4db18-605">Change the defaults in code</span></span>

<span data-ttu-id="4db18-606">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="4db18-606">*No configuration*</span></span>

<span data-ttu-id="4db18-607">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="4db18-607">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="4db18-608">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="4db18-608">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="4db18-609">`CreateDefaultBuilder`, Kestrel yapılandırmasını yüklemek için varsayılan olarak-1 ' i @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-609">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="4db18-610">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-610">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="4db18-611">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-611">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="4db18-612">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="4db18-612">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="4db18-613">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-613">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="4db18-614">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="4db18-614">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
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

<span data-ttu-id="4db18-615">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-615">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="4db18-616">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-616">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="4db18-617">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="4db18-617">Schema notes:</span></span>

* <span data-ttu-id="4db18-618">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-618">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="4db18-619">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="4db18-619">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="4db18-620">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-620">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="4db18-621">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-621">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="4db18-622">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="4db18-622">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="4db18-623">@No__t-0 yoluyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-623">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="4db18-624">@No__t-0 bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-624">The `Certificate` section is optional.</span></span> <span data-ttu-id="4db18-625">@No__t-0 bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-625">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="4db18-626">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-626">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="4db18-627">@No__t-0 bölümü **, hem @no__t**-2**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-627">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="4db18-628">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-628">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="4db18-629">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="4db18-629">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="4db18-630">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-630">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="4db18-631">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-631">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="4db18-632">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-632">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="4db18-633">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-633">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="4db18-634">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-634">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="4db18-635">`KestrelConfigurationLoader`, `Endpoint` aşırı yüklemeleri olarak `KestrelServerOptions` ' den API 'nin @no__t ailesini yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-635">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="4db18-636">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-636">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="4db18-637">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="4db18-637">*Change the defaults in code*</span></span>

<span data-ttu-id="4db18-638">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-638">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="4db18-639">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-639">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="4db18-640">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="4db18-640">*Kestrel support for SNI*</span></span>

<span data-ttu-id="4db18-641">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-641">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="4db18-642">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="4db18-642">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="4db18-643">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-643">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="4db18-644">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-644">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="4db18-645">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-645">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="4db18-646">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="4db18-646">SNI support requires:</span></span>

* <span data-ttu-id="4db18-647">@No__t-0 veya üzeri hedef çerçeve üzerinde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="4db18-647">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="4db18-648">@No__t-0 veya sonraki bir sürümde, geri çağırma çağrılır, ancak `name` her zaman `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-648">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="4db18-649">@No__t-0, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-649">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="4db18-650">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-650">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="4db18-651">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-651">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="4db18-652">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-652">Bind to a TCP socket</span></span>

<span data-ttu-id="4db18-653">@No__t-0 yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="4db18-653">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="4db18-654">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-654">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="4db18-655">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-655">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="4db18-656">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-656">Bind to a Unix socket</span></span>

<span data-ttu-id="4db18-657">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-657">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="4db18-658">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="4db18-658">Port 0</span></span>

<span data-ttu-id="4db18-659">@No__t-0 olan bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-659">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="4db18-660">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4db18-660">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="4db18-661">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4db18-661">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="4db18-662">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="4db18-662">Limitations</span></span>

<span data-ttu-id="4db18-663">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-663">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="4db18-664">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-664">`--urls` command-line argument</span></span>
* <span data-ttu-id="4db18-665">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="4db18-665">`urls` host configuration key</span></span>
* <span data-ttu-id="4db18-666">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-666">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="4db18-667">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-667">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="4db18-668">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="4db18-668">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="4db18-669">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-669">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="4db18-670">@No__t-0 ve `UseUrls` yaklaşımlarının ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4db18-670">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="4db18-671">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-671">IIS endpoint configuration</span></span>

<span data-ttu-id="4db18-672">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-672">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="4db18-673">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-673">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="4db18-674">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="4db18-674">ListenOptions.Protocols</span></span>

<span data-ttu-id="4db18-675">@No__t-0 özelliği, bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-675">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="4db18-676">@No__t-1 numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-676">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="4db18-677">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="4db18-677">`HttpProtocols` enum value</span></span> | <span data-ttu-id="4db18-678">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="4db18-678">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="4db18-679">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="4db18-679">HTTP/1.1 only.</span></span> <span data-ttu-id="4db18-680">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-680">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="4db18-681">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="4db18-681">HTTP/2 only.</span></span> <span data-ttu-id="4db18-682">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-682">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="4db18-683">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="4db18-683">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="4db18-684">HTTP/2 bir TLS ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-684">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="4db18-685">Varsayılan protokol HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-685">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="4db18-686">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="4db18-686">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="4db18-687">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4db18-687">TLS version 1.2 or later</span></span>
* <span data-ttu-id="4db18-688">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="4db18-688">Renegotiation disabled</span></span>
* <span data-ttu-id="4db18-689">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="4db18-689">Compression disabled</span></span>
* <span data-ttu-id="4db18-690">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="4db18-690">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="4db18-691">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="4db18-691">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="4db18-692">Sınırlı alan Diffie-Hellman (DHE) &lbrack; @ no__t-1 @ no__t-2 &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="4db18-692">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="4db18-693">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="4db18-693">Cipher suite not blacklisted</span></span>

<span data-ttu-id="4db18-694">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` `TLS-ECDHE` @ no__t-3 ' ü P-256 eliptik eğrisi &lbrack; @ no__t-5 @ no__t-6 varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db18-694">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="4db18-695">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="4db18-695">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="4db18-696">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="4db18-696">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="4db18-697">İsteğe bağlı olarak, belirli şifrelemeler için bağlantı başına TLS el sıkışmaları filtrelemek üzere `IConnectionAdapter` bir uygulama oluşturun:</span><span class="sxs-lookup"><span data-stu-id="4db18-697">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
    });
});
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="4db18-698">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="4db18-698">*Set the protocol from configuration*</span></span>

<span data-ttu-id="4db18-699"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, Kestrel yapılandırmasını yüklemek için varsayılan olarak-1 ' i @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-699"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="4db18-700">Aşağıdaki *appSettings. JSON* örneğinde, tüm Kestrel uç noktaları için varsayılan bir bağlantı protokolü (http/1.1 ve http/2) oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="4db18-700">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="4db18-701">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç nokta için bağlantı protokolü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="4db18-701">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="4db18-702">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="4db18-702">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="4db18-703">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-703">Transport configuration</span></span>

<span data-ttu-id="4db18-704">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="4db18-704">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="4db18-705">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ' a çağrı yapan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamalarının önemli bir değişikliği olur ve aşağıdaki paketlerden birine bağımlıdır:</span><span class="sxs-lookup"><span data-stu-id="4db18-705">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="4db18-706">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="4db18-706">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="4db18-707">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="4db18-707">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="4db18-708">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="4db18-708">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="4db18-709">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-709">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="4db18-710">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="4db18-710">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a><span data-ttu-id="4db18-711">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-711">URL prefixes</span></span>

<span data-ttu-id="4db18-712">@No__t-0, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-712">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="4db18-713">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-713">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="4db18-714">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-714">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="4db18-715">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-715">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="4db18-716">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-716">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="4db18-717">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-717">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="4db18-718">`[::]`, IPv4 `0.0.0.0` ' in IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="4db18-718">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="4db18-719">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-719">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="4db18-720">@No__t-0 ve `+` ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="4db18-720">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="4db18-721">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-721">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="4db18-722">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-722">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="4db18-723">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-723">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="4db18-724">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si @no__t ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-724">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="4db18-725">@No__t-0 belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-725">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="4db18-726">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-726">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="4db18-727">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4db18-727">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="4db18-728">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="4db18-728">Host filtering</span></span>

<span data-ttu-id="4db18-729">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-729">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="4db18-730">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-730">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="4db18-731">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-731">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="4db18-732">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="4db18-732">`Host` headers aren't validated.</span></span>

<span data-ttu-id="4db18-733">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-733">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="4db18-734">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-734">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="4db18-735">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4db18-735">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="4db18-736">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-736">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="4db18-737">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4db18-737">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="4db18-738">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="4db18-738">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="4db18-739">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4db18-739">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="4db18-740">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-740">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="4db18-741">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-741">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="4db18-742">@No__t-0 ' i Iletilen üstbilgiler ara yazılımı ile, istekleri bir ters ara sunucu veya yük dengeleyiciye iletirken `Host` üst bilgisi korunmazsa uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-742">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="4db18-743">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-743">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="4db18-744">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4db18-744">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="4db18-745">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="4db18-745">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="4db18-746">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-746">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="4db18-747">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="4db18-747">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="4db18-748">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4db18-748">HTTPS</span></span>
* <span data-ttu-id="4db18-749">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="4db18-749">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="4db18-750">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="4db18-750">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="4db18-751">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4db18-751">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="4db18-752">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4db18-752">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="4db18-753">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="4db18-753">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="4db18-754">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-754">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="4db18-755">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="4db18-755">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="4db18-756">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-756">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="4db18-758">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="4db18-758">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4db18-760">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="4db18-760">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="4db18-761">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-761">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="4db18-762">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="4db18-762">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="4db18-763">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-763">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="4db18-764">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-764">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="4db18-765">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="4db18-765">A reverse proxy:</span></span>

* <span data-ttu-id="4db18-766">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-766">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="4db18-767">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-767">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="4db18-768">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-768">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="4db18-769">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-769">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="4db18-770">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-770">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-771">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-771">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="4db18-772">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="4db18-772">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="4db18-773">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="4db18-773">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4db18-774">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-774">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="4db18-775">*Program.cs*' de, şablon kodu, arka planda @no__t 2 ' yi çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ' i çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-775">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="4db18-776">@No__t-0 çağrıldıktan sonra ek yapılandırma sağlamak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> ' i çağırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-776">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="4db18-777">@No__t-0 ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host> ' nin *konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-777">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="4db18-778">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-778">Kestrel options</span></span>

<span data-ttu-id="4db18-779">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-779">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="4db18-780">@No__t-1 sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğinde kısıtlamalar ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-780">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="4db18-781">@No__t-0 özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-781">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="4db18-782">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4db18-782">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="4db18-783">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-783">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="4db18-784">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="4db18-784">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="4db18-785">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="4db18-785">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="4db18-786">@No__t-0 ' da Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-786">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="4db18-787">@No__t-1 sınıfına `IConfiguration` ' ın bir örneğini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-787">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="4db18-788">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-788">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="4db18-789">@No__t-0 ' da, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4db18-789">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="4db18-790">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-790">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="4db18-791">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-791">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="4db18-792">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-792">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="4db18-793">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-793">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="4db18-794">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-794">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="4db18-795">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4db18-795">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="4db18-796">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="4db18-796">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="4db18-797">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-797">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="4db18-798">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="4db18-798">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="4db18-799">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-799">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="4db18-800">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="4db18-800">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="4db18-801">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="4db18-801">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="4db18-802">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="4db18-802">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="4db18-803">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="4db18-803">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="4db18-804">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-804">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="4db18-805">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="4db18-805">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="4db18-806">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4db18-806">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="4db18-807">@No__t-1 özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` özelliği vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-807">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="4db18-808">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-808">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="4db18-809">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="4db18-809">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="4db18-810">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-810">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="4db18-811">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="4db18-811">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="4db18-812">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-812">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="4db18-813">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-813">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="4db18-814">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-814">A minimum rate also applies to the response.</span></span> <span data-ttu-id="4db18-815">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-815">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="4db18-816">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4db18-816">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="4db18-817">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="4db18-817">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="4db18-818">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-818">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="4db18-819">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="4db18-819">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="4db18-820">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="4db18-820">Synchronous IO</span></span>

<span data-ttu-id="4db18-821"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="4db18-821"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="4db18-822">Varsayılan değer `true` ' dır.</span><span class="sxs-lookup"><span data-stu-id="4db18-822">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4db18-823">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-823">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="4db18-824">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4db18-824">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="4db18-825">Aşağıdaki örnek, zaman uyumlu GÇ 'yi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="4db18-825">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="4db18-826">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="4db18-826">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="4db18-827">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-827">Endpoint configuration</span></span>

<span data-ttu-id="4db18-828">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="4db18-828">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="4db18-829">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="4db18-829">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="4db18-830">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="4db18-830">Specify URLs using the:</span></span>

* <span data-ttu-id="4db18-831">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-831">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="4db18-832">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4db18-832">`--urls` command-line argument.</span></span>
* <span data-ttu-id="4db18-833">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="4db18-833">`urls` host configuration key.</span></span>
* <span data-ttu-id="4db18-834">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4db18-834">`UseUrls` extension method.</span></span>

<span data-ttu-id="4db18-835">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4db18-835">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="4db18-836">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="4db18-836">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="4db18-837">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-837">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="4db18-838">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="4db18-838">A development certificate is created:</span></span>

* <span data-ttu-id="4db18-839">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="4db18-839">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="4db18-840">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-840">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="4db18-841">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-841">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="4db18-842">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-842">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="4db18-843">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-843">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="4db18-844">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="4db18-844">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="4db18-845">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="4db18-845">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="4db18-846">ConfigureEndpointDefaults Varsayılanları (eylem @ no__t-0ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-846">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="4db18-847">Belirtilen her bir uç nokta için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-847">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="4db18-848">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-848">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="4db18-849">ConfigureHttpsDefaults (eylem @ no__t-0HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="4db18-849">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="4db18-850">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma @no__t (0) belirtir.</span><span class="sxs-lookup"><span data-stu-id="4db18-850">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="4db18-851">@No__t-0 ' ın birden çok kez çağrılması, önceki `Action`s ' i belirtilen son @no__t 2 ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-851">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="4db18-852">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="4db18-852">Configure(IConfiguration)</span></span>

<span data-ttu-id="4db18-853">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-853">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="4db18-854">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-854">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="4db18-855">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="4db18-855">ListenOptions.UseHttps</span></span>

<span data-ttu-id="4db18-856">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-856">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="4db18-857">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="4db18-857">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="4db18-858">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-858">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="4db18-859">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4db18-859">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="4db18-860">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="4db18-860">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="4db18-861">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-861">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="4db18-862">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="4db18-862">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="4db18-863">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-863">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="4db18-864">@No__t-0 döndürür.</span><span class="sxs-lookup"><span data-stu-id="4db18-864">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="4db18-865">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-865">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="4db18-866">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-866">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="4db18-867">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="4db18-867">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="4db18-868">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="4db18-868">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="4db18-869">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-869">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="4db18-870">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4db18-870">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="4db18-871">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-871">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="4db18-872">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="4db18-872">Supported configurations described next:</span></span>

* <span data-ttu-id="4db18-873">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="4db18-873">No configuration</span></span>
* <span data-ttu-id="4db18-874">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="4db18-874">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="4db18-875">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="4db18-875">Change the defaults in code</span></span>

<span data-ttu-id="4db18-876">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="4db18-876">*No configuration*</span></span>

<span data-ttu-id="4db18-877">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="4db18-877">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="4db18-878">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="4db18-878">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="4db18-879">`CreateDefaultBuilder`, Kestrel yapılandırmasını yüklemek için varsayılan olarak-1 ' i @no__t çağırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-879">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="4db18-880">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-880">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="4db18-881">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4db18-881">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="4db18-882">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="4db18-882">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="4db18-883">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4db18-883">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="4db18-884">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="4db18-884">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },

      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },

      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },

      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },

      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
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

<span data-ttu-id="4db18-885">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="4db18-885">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="4db18-886">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="4db18-886">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="4db18-887">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="4db18-887">Schema notes:</span></span>

* <span data-ttu-id="4db18-888">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-888">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="4db18-889">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="4db18-889">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="4db18-890">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-890">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="4db18-891">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="4db18-891">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="4db18-892">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="4db18-892">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="4db18-893">@No__t-0 yoluyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-893">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="4db18-894">@No__t-0 bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-894">The `Certificate` section is optional.</span></span> <span data-ttu-id="4db18-895">@No__t-0 bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-895">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="4db18-896">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-896">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="4db18-897">@No__t-0 bölümü **, hem @no__t**-2**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-897">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="4db18-898">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-898">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="4db18-899">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="4db18-899">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="4db18-900">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-900">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="4db18-901">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-901">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="4db18-902">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-902">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="4db18-903">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-903">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="4db18-904">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-904">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="4db18-905">`KestrelConfigurationLoader`, `Endpoint` aşırı yüklemeleri olarak `KestrelServerOptions` ' den API 'nin @no__t ailesini yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-905">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="4db18-906">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-906">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="4db18-907">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="4db18-907">*Change the defaults in code*</span></span>

<span data-ttu-id="4db18-908">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-908">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="4db18-909">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-909">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="4db18-910">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="4db18-910">*Kestrel support for SNI*</span></span>

<span data-ttu-id="4db18-911">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-911">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="4db18-912">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="4db18-912">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="4db18-913">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-913">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="4db18-914">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="4db18-914">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="4db18-915">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4db18-915">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="4db18-916">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="4db18-916">SNI support requires:</span></span>

* <span data-ttu-id="4db18-917">@No__t-0 veya üzeri hedef çerçeve üzerinde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="4db18-917">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="4db18-918">@No__t-0 veya sonraki bir sürümde, geri çağırma çağrılır, ancak `name` her zaman `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-918">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="4db18-919">@No__t-0, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4db18-919">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="4db18-920">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-920">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="4db18-921">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-921">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser,
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="4db18-922">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-922">Bind to a TCP socket</span></span>

<span data-ttu-id="4db18-923">@No__t-0 yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="4db18-923">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="4db18-924">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4db18-924">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="4db18-925">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-925">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="4db18-926">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="4db18-926">Bind to a Unix socket</span></span>

<span data-ttu-id="4db18-927">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-927">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="4db18-928">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="4db18-928">Port 0</span></span>

<span data-ttu-id="4db18-929">@No__t-0 olan bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-929">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="4db18-930">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="4db18-930">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="4db18-931">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="4db18-931">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="4db18-932">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="4db18-932">Limitations</span></span>

<span data-ttu-id="4db18-933">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="4db18-933">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="4db18-934">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-934">`--urls` command-line argument</span></span>
* <span data-ttu-id="4db18-935">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="4db18-935">`urls` host configuration key</span></span>
* <span data-ttu-id="4db18-936">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="4db18-936">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="4db18-937">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-937">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="4db18-938">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="4db18-938">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="4db18-939">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-939">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="4db18-940">@No__t-0 ve `UseUrls` yaklaşımlarının ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktalarını geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4db18-940">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="4db18-941">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-941">IIS endpoint configuration</span></span>

<span data-ttu-id="4db18-942">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-942">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="4db18-943">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="4db18-943">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="4db18-944">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="4db18-944">Transport configuration</span></span>

<span data-ttu-id="4db18-945">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="4db18-945">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="4db18-946">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ' a çağrı yapan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamalarının önemli bir değişikliği olur ve aşağıdaki paketlerden birine bağımlıdır:</span><span class="sxs-lookup"><span data-stu-id="4db18-946">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="4db18-947">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="4db18-947">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="4db18-948">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="4db18-948">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="4db18-949">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="4db18-949">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="4db18-950">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4db18-950">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="4db18-951">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="4db18-951">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

  ```csharp
  public class Program
  {
      public static void Main(string[] args)
      {
          CreateWebHostBuilder(args).Build().Run();
      }

      public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
          WebHost.CreateDefaultBuilder(args)
              .UseLibuv()
              .UseStartup<Startup>();
  }
  ```

### <a name="url-prefixes"></a><span data-ttu-id="4db18-952">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="4db18-952">URL prefixes</span></span>

<span data-ttu-id="4db18-953">@No__t-0, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="4db18-953">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="4db18-954">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4db18-954">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="4db18-955">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="4db18-955">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="4db18-956">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-956">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="4db18-957">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-957">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="4db18-958">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="4db18-958">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="4db18-959">`[::]`, IPv4 `0.0.0.0` ' in IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="4db18-959">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="4db18-960">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-960">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="4db18-961">@No__t-0 ve `+` ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="4db18-961">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="4db18-962">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="4db18-962">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="4db18-963">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-963">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="4db18-964">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4db18-964">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="4db18-965">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si @no__t ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="4db18-965">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="4db18-966">@No__t-0 belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4db18-966">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="4db18-967">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="4db18-967">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="4db18-968">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4db18-968">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="4db18-969">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="4db18-969">Host filtering</span></span>

<span data-ttu-id="4db18-970">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="4db18-970">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="4db18-971">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="4db18-971">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="4db18-972">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-972">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="4db18-973">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="4db18-973">`Host` headers aren't validated.</span></span>

<span data-ttu-id="4db18-974">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4db18-974">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="4db18-975">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4db18-975">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="4db18-976">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4db18-976">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="4db18-977">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="4db18-977">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="4db18-978">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4db18-978">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="4db18-979">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="4db18-979">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="4db18-980">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="4db18-980">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="4db18-981">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="4db18-981">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="4db18-982">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4db18-982">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="4db18-983">@No__t-0 ' i Iletilen üstbilgiler ara yazılımı ile, istekleri bir ters ara sunucu veya yük dengeleyiciye iletirken `Host` üst bilgisi korunmazsa uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-983">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="4db18-984">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="4db18-984">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="4db18-985">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4db18-985">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4db18-986">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4db18-986">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="4db18-987">RFC 7230: Ileti sözdizimi ve yönlendirme (Bölüm 5,4: Ana bilgisayar)</span><span class="sxs-lookup"><span data-stu-id="4db18-987">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
