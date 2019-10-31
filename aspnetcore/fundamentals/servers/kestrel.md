---
title: ASP.NET Core Web sunucusu uygulamasını Kestrel
author: guardrex
description: ASP.NET Core platformlar arası Web sunucusu olan Kestrel hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: beaf6ac49359adfdc2dc24221eab04cc853646a9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143447"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3e904-103">ASP.NET Core Web sunucusu uygulamasını Kestrel</span><span class="sxs-lookup"><span data-stu-id="3e904-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3e904-104">[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher)No, [Stephen halter](https://twitter.com/halter73)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="3e904-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3e904-105">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3e904-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3e904-106">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3e904-107">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="3e904-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3e904-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3e904-108">HTTPS</span></span>
* <span data-ttu-id="3e904-109">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="3e904-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3e904-110">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="3e904-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3e904-111">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="3e904-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3e904-112">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3e904-113">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3e904-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3e904-114">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3e904-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3e904-115">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="3e904-115">HTTP/2 support</span></span>

<span data-ttu-id="3e904-116">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3e904-117">İşletim sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="3e904-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="3e904-118">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3e904-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3e904-119">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="3e904-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3e904-120">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3e904-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3e904-121">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="3e904-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3e904-122">TLS 1,2 veya üzeri bağlantı</span><span class="sxs-lookup"><span data-stu-id="3e904-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3e904-123">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3e904-124">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3e904-125">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3e904-126">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3e904-127">HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3e904-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3e904-128">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3e904-129">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3e904-130">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="3e904-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3e904-131">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3e904-132">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="3e904-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3e904-133">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3e904-135">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3e904-137">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="3e904-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3e904-138">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3e904-139">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="3e904-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3e904-140">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3e904-141">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3e904-142">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="3e904-142">A reverse proxy:</span></span>

* <span data-ttu-id="3e904-143">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3e904-144">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3e904-145">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3e904-146">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3e904-147">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-148">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3e904-149">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="3e904-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3e904-150">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3e904-151">*Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3e904-152">`CreateDefaultBuilder` ve ana bilgisayarı oluşturma hakkında daha fazla bilgi için <xref:fundamentals/host/generic-host#set-up-a-host>*konak ayarlama* ve *Varsayılan Oluşturucu ayarları* bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="3e904-153">`CreateDefaultBuilder` ve `ConfigureWebHostDefaults`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:</span><span class="sxs-lookup"><span data-stu-id="3e904-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

<span data-ttu-id="3e904-154">Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, `ConfigureKestrel`çağrılmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="3e904-155">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-155">Kestrel options</span></span>

<span data-ttu-id="3e904-156">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3e904-157"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3e904-158">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3e904-159">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="3e904-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3e904-160">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3e904-161">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3e904-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="3e904-162">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="3e904-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3e904-163">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3e904-164">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3e904-165">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3e904-166">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3e904-167">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3e904-168">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="3e904-169">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3e904-170">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3e904-171">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3e904-172">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="3e904-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3e904-173">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="3e904-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3e904-174">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3e904-175">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="3e904-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3e904-176">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3e904-177">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="3e904-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3e904-178">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3e904-179">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="3e904-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3e904-180">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="3e904-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3e904-181">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3e904-182">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="3e904-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3e904-183">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3e904-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3e904-184">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3e904-185">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3e904-186">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="3e904-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3e904-187">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3e904-188">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="3e904-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3e904-189">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3e904-190">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3e904-191">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3e904-192">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3e904-193">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="3e904-194">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="3e904-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3e904-195">, İstek çoğullama için protokol desteği nedeniyle, önceki örnekte başvurulan `HttpContext.Features`, HTTP/2 istekleri için  ' de mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="3e904-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3e904-196">Ancak, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature>, HTTP/2 istekleri için `HttpContext.Features` ' i hala mevcut olduğundan, bir HTTP/2 isteği için bile `IHttpMinRequestBodyDataRateFeature.MinDataRate` ' e `null` ' e ayarlanarak, okuma hızı sınırı tamamen istek temelli olarak *devre dışı* bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="3e904-197">`IHttpMinRequestBodyDataRateFeature.MinDataRate` okumaya veya `null` dışında bir değere ayarlamaya çalışırken, bir HTTP/2 isteği verilen bir `NotSupportedException` atılmaya neden olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="3e904-198">`KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3e904-199">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3e904-200">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3e904-201">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="3e904-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3e904-202">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="3e904-202">Maximum streams per connection</span></span>

<span data-ttu-id="3e904-203">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3e904-204">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="3e904-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="3e904-205">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="3e904-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3e904-206">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-206">Header table size</span></span>

<span data-ttu-id="3e904-207">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="3e904-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3e904-208">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3e904-209">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="3e904-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="3e904-210">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3e904-211">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-211">Maximum frame size</span></span>

<span data-ttu-id="3e904-212">`Http2.MaxFrameSize`, sunucu tarafından alınan veya gönderilen HTTP/2 bağlantı çerçevesi yükünün izin verilen en büyük boyutunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="3e904-213">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="3e904-214">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="3e904-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3e904-215">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-215">Maximum request header size</span></span>

<span data-ttu-id="3e904-216">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3e904-217">Bu sınır, sıkıştırılmış ve sıkıştırılmamış temsillerinde hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3e904-218">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="3e904-219">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3e904-220">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-220">Initial connection window size</span></span>

<span data-ttu-id="3e904-221">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3e904-222">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3e904-223">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="3e904-224">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="3e904-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3e904-225">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-225">Initial stream window size</span></span>

<span data-ttu-id="3e904-226">`Http2.InitialStreamWindowSize`, istek (Stream) başına tek seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3e904-227">İstekler aynı zamanda `Http2.InitialConnectionWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="3e904-228">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="3e904-229">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="3e904-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3e904-230">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="3e904-230">Synchronous IO</span></span>

<span data-ttu-id="3e904-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3e904-232">Varsayılan değer `false` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3e904-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-233">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3e904-234">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3e904-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3e904-235">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="3e904-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3e904-236">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="3e904-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3e904-237">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-237">Endpoint configuration</span></span>

<span data-ttu-id="3e904-238">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="3e904-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3e904-239">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="3e904-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3e904-240">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="3e904-240">Specify URLs using the:</span></span>

* <span data-ttu-id="3e904-241">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3e904-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3e904-242">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="3e904-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3e904-243">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="3e904-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="3e904-244">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e904-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="3e904-245">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3e904-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3e904-246">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3e904-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3e904-247">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3e904-248">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="3e904-248">A development certificate is created:</span></span>

* <span data-ttu-id="3e904-249">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="3e904-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3e904-250">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3e904-251">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3e904-252">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3e904-253">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3e904-254">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="3e904-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3e904-255">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="3e904-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3e904-256">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3e904-257">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3e904-258">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3e904-259">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3e904-260">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3e904-261">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="3e904-262">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="3e904-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="3e904-263">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3e904-264">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3e904-265">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="3e904-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3e904-266">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3e904-267">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="3e904-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3e904-268">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3e904-269">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-269">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3e904-270">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="3e904-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3e904-271">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3e904-272">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="3e904-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3e904-273">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3e904-274">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="3e904-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3e904-275">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3e904-276">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3e904-277">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3e904-278">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3e904-279">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3e904-280">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3e904-281">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3e904-282">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-282">Supported configurations described next:</span></span>

* <span data-ttu-id="3e904-283">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="3e904-283">No configuration</span></span>
* <span data-ttu-id="3e904-284">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="3e904-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3e904-285">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="3e904-285">Change the defaults in code</span></span>

<span data-ttu-id="3e904-286">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="3e904-286">*No configuration*</span></span>

<span data-ttu-id="3e904-287">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="3e904-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3e904-288">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="3e904-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3e904-289">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3e904-290">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3e904-291">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3e904-292">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="3e904-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3e904-293">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3e904-294">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="3e904-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3e904-295">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3e904-296">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3e904-297">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="3e904-297">Schema notes:</span></span>

* <span data-ttu-id="3e904-298">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3e904-299">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="3e904-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3e904-300">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3e904-301">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3e904-302">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3e904-303">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3e904-304">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="3e904-305">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3e904-306">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3e904-307">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3e904-308">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3e904-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="3e904-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="3e904-310">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3e904-311">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3e904-312">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3e904-313">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3e904-314">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3e904-315">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3e904-316">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3e904-317">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="3e904-317">*Change the defaults in code*</span></span>

<span data-ttu-id="3e904-318">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3e904-319">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="3e904-320">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="3e904-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3e904-321">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3e904-322">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="3e904-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3e904-323">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3e904-324">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3e904-325">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3e904-326">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="3e904-326">SNI support requires:</span></span>

* <span data-ttu-id="3e904-327">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="3e904-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3e904-328">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="3e904-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3e904-329">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="3e904-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3e904-330">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3e904-331">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="3e904-332">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="3e904-332">Connection logging</span></span>

<span data-ttu-id="3e904-333">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-333">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3e904-334">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-334">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3e904-335">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-335">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3e904-336">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-336">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3e904-337">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-337">Bind to a TCP socket</span></span>

<span data-ttu-id="3e904-338"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="3e904-338">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3e904-339">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-339">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3e904-340">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-340">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3e904-341">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-341">Bind to a Unix socket</span></span>

<span data-ttu-id="3e904-342">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-342">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3e904-343">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="3e904-343">Port 0</span></span>

<span data-ttu-id="3e904-344">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-344">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3e904-345">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="3e904-345">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3e904-346">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="3e904-346">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3e904-347">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="3e904-347">Limitations</span></span>

<span data-ttu-id="3e904-348">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-348">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3e904-349">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-349">`--urls` command-line argument</span></span>
* <span data-ttu-id="3e904-350">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="3e904-350">`urls` host configuration key</span></span>
* <span data-ttu-id="3e904-351">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-351">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3e904-352">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-352">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3e904-353">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="3e904-353">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3e904-354">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-354">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3e904-355">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="3e904-355">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3e904-356">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-356">IIS endpoint configuration</span></span>

<span data-ttu-id="3e904-357">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-357">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3e904-358">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-358">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3e904-359">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="3e904-359">ListenOptions.Protocols</span></span>

<span data-ttu-id="3e904-360">`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler.</span><span class="sxs-lookup"><span data-stu-id="3e904-360">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3e904-361">`HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-361">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3e904-362">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="3e904-362">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3e904-363">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="3e904-363">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3e904-364">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3e904-364">HTTP/1.1 only.</span></span> <span data-ttu-id="3e904-365">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-365">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3e904-366">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3e904-366">HTTP/2 only.</span></span> <span data-ttu-id="3e904-367">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-367">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3e904-368">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3e904-368">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3e904-369">HTTP/2, istemcinin TLS [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) el sıkışmasında http/2 seçmesini gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-369">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3e904-370">Herhangi bir uç nokta için varsayılan `ListenOptions.Protocols` değeri `HttpProtocols.Http1AndHttp2` ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-370">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="3e904-371">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="3e904-371">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3e904-372">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3e904-372">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3e904-373">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="3e904-373">Renegotiation disabled</span></span>
* <span data-ttu-id="3e904-374">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="3e904-374">Compression disabled</span></span>
* <span data-ttu-id="3e904-375">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="3e904-375">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3e904-376">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="3e904-376">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3e904-377">Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="3e904-377">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3e904-378">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="3e904-378">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3e904-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3e904-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3e904-380">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="3e904-380">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3e904-381">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="3e904-381">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="3e904-382">Gerektiğinde belirli şifrelemeler için TLS el sıkışmaları için bağlantı temelinde filtre uygulamak üzere bağlantı ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-382">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="3e904-383">Aşağıdaki örnek, uygulamanın desteklemediği herhangi bir şifre algoritması için <xref:System.NotSupportedException> ' i oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-383">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="3e904-384">Alternatif olarak, [ılshandshakefeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) öğesini kabul edilebilir şifreleme paketleri listesi ile tanımlayın ve karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-384">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="3e904-385">[CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) şifre algoritması ile hiçbir şifreleme kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-385">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="3e904-386">Bağlantı filtrelemesi, bir <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda aracılığıyla da yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-386">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="3e904-387">Linux 'ta <xref:System.Net.Security.CipherSuitesPolicy>, TLS el sıkışmaları her bağlantı temelinde filtrelemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-387">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="3e904-388">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="3e904-388">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3e904-389">Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-389">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3e904-390">Aşağıdaki *appSettings. JSON* örneği, tüm uç noktalar için varsayılan bağlantı protokolü olarak http/1.1 oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3e904-390">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="3e904-391">Aşağıdaki *appSettings. JSON* örneği, belirli bir uç nokta için http/1.1 bağlantı protokolünü belirler:</span><span class="sxs-lookup"><span data-stu-id="3e904-391">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="3e904-392">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="3e904-392">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3e904-393">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-393">Transport configuration</span></span>

<span data-ttu-id="3e904-394">Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="3e904-394">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="3e904-395">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-395">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="3e904-396">`IWebHostBuilder`<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-396">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3e904-397">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-397">URL prefixes</span></span>

<span data-ttu-id="3e904-398">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-398">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3e904-399">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-399">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3e904-400">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-400">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3e904-401">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-401">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3e904-402">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-402">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3e904-403">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-403">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3e904-404">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="3e904-404">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3e904-405">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-405">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3e904-406">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="3e904-406">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3e904-407">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-407">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3e904-408">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-408">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3e904-409">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-409">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3e904-410">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-410">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3e904-411">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-411">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3e904-412">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-412">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3e904-413">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3e904-413">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3e904-414">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="3e904-414">Host filtering</span></span>

<span data-ttu-id="3e904-415">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-415">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3e904-416">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-416">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3e904-417">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-417">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3e904-418">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="3e904-418">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3e904-419">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-419">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3e904-420">Ana bilgisayar filtreleme ara yazılımı, ASP.NET Core uygulamaları için örtük olarak sunulan [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-420">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="3e904-421">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="3e904-421">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3e904-422">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-422">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3e904-423">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="3e904-423">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3e904-424">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="3e904-424">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3e904-425">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="3e904-425">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3e904-426">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-426">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3e904-427">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-427">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3e904-428">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-428">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3e904-429">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="3e904-429">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3e904-430">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3e904-430">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="3e904-431">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3e904-431">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3e904-432">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-432">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3e904-433">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="3e904-433">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3e904-434">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3e904-434">HTTPS</span></span>
* <span data-ttu-id="3e904-435">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="3e904-435">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3e904-436">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="3e904-436">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3e904-437">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="3e904-437">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3e904-438">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-438">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3e904-439">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3e904-439">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3e904-440">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3e904-440">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3e904-441">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="3e904-441">HTTP/2 support</span></span>

<span data-ttu-id="3e904-442">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3e904-443">İşletim sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="3e904-443">Operating system&dagger;</span></span>
  * <span data-ttu-id="3e904-444">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3e904-444">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3e904-445">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="3e904-445">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3e904-446">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3e904-446">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3e904-447">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="3e904-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3e904-448">TLS 1,2 veya üzeri bağlantı</span><span class="sxs-lookup"><span data-stu-id="3e904-448">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3e904-449">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-449">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3e904-450">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-450">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3e904-451">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-451">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3e904-452">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-452">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3e904-453">HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3e904-453">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3e904-454">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-454">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3e904-455">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-455">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3e904-456">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="3e904-456">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3e904-457">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-457">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3e904-458">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="3e904-458">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3e904-459">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-459">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3e904-461">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-461">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3e904-463">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="3e904-463">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3e904-464">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-464">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3e904-465">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="3e904-465">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3e904-466">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-466">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3e904-467">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-467">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3e904-468">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="3e904-468">A reverse proxy:</span></span>

* <span data-ttu-id="3e904-469">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-469">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3e904-470">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-470">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3e904-471">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-471">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3e904-472">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-472">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3e904-473">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-473">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-474">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-474">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3e904-475">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="3e904-475">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3e904-476">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="3e904-476">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3e904-477">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-477">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3e904-478">*Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-478">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3e904-479">`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-479">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="3e904-480">`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:</span><span class="sxs-lookup"><span data-stu-id="3e904-480">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="3e904-481">Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, `ConfigureKestrel`çağrılmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-481">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="3e904-482">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-482">Kestrel options</span></span>

<span data-ttu-id="3e904-483">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-483">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3e904-484"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-484">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3e904-485">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-485">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3e904-486">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="3e904-486">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3e904-487">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-487">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3e904-488">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3e904-488">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="3e904-489">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="3e904-489">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3e904-490">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-490">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3e904-491">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-491">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3e904-492">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-492">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3e904-493">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-493">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3e904-494">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-494">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3e904-495">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-495">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="3e904-496">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-496">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3e904-497">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-497">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3e904-498">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-498">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3e904-499">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="3e904-499">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3e904-500">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="3e904-500">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3e904-501">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-501">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3e904-502">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="3e904-502">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3e904-503">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-503">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3e904-504">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="3e904-504">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3e904-505">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-505">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3e904-506">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="3e904-506">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3e904-507">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="3e904-507">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3e904-508">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-508">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3e904-509">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="3e904-509">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3e904-510">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3e904-510">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3e904-511">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-511">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3e904-512">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-512">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3e904-513">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="3e904-513">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3e904-514">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-514">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3e904-515">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="3e904-515">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3e904-516">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-516">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3e904-517">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-517">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3e904-518">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-518">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3e904-519">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-519">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3e904-520">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-520">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="3e904-521">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="3e904-521">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3e904-522">İstek çoğullama için protokol desteğinin desteklenmediği için, önceki örnekte başvurulan oran özelliği, http/2 istekleri için `HttpContext.Features` ' da mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="3e904-522">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3e904-523">`KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-523">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3e904-524">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-524">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3e904-525">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-525">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3e904-526">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="3e904-526">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3e904-527">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="3e904-527">Maximum streams per connection</span></span>

<span data-ttu-id="3e904-528">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-528">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3e904-529">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="3e904-529">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="3e904-530">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="3e904-530">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3e904-531">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-531">Header table size</span></span>

<span data-ttu-id="3e904-532">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="3e904-532">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3e904-533">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-533">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3e904-534">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="3e904-534">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="3e904-535">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-535">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3e904-536">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-536">Maximum frame size</span></span>

<span data-ttu-id="3e904-537">`Http2.MaxFrameSize`, alacak HTTP/2 bağlantı çerçevesi yükünün en büyük boyutunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-537">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="3e904-538">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-538">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="3e904-539">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="3e904-539">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3e904-540">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-540">Maximum request header size</span></span>

<span data-ttu-id="3e904-541">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-541">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3e904-542">Bu sınır, hem sıkıştırılmış hem de sıkıştırılmamış temsillerinde birlikte hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-542">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3e904-543">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-543">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="3e904-544">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-544">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3e904-545">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-545">Initial connection window size</span></span>

<span data-ttu-id="3e904-546">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-546">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3e904-547">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3e904-548">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="3e904-549">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="3e904-549">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3e904-550">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-550">Initial stream window size</span></span>

<span data-ttu-id="3e904-551">`Http2.InitialStreamWindowSize`, istek (Stream) başına tek seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-551">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3e904-552">İstekler aynı zamanda `Http2.InitialStreamWindowSize` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-552">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3e904-553">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-553">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="3e904-554">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="3e904-554">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3e904-555">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="3e904-555">Synchronous IO</span></span>

<span data-ttu-id="3e904-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3e904-557">Varsayılan değer `true` ' dır.</span><span class="sxs-lookup"><span data-stu-id="3e904-557">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-558">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-558">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3e904-559">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3e904-559">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3e904-560">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="3e904-560">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3e904-561">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="3e904-561">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3e904-562">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-562">Endpoint configuration</span></span>

<span data-ttu-id="3e904-563">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="3e904-563">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3e904-564">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="3e904-564">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3e904-565">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="3e904-565">Specify URLs using the:</span></span>

* <span data-ttu-id="3e904-566">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3e904-566">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3e904-567">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="3e904-567">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3e904-568">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="3e904-568">`urls` host configuration key.</span></span>
* <span data-ttu-id="3e904-569">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e904-569">`UseUrls` extension method.</span></span>

<span data-ttu-id="3e904-570">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3e904-570">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3e904-571">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3e904-571">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3e904-572">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-572">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3e904-573">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="3e904-573">A development certificate is created:</span></span>

* <span data-ttu-id="3e904-574">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="3e904-574">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3e904-575">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-575">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3e904-576">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-576">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3e904-577">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-577">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3e904-578">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-578">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3e904-579">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="3e904-579">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3e904-580">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="3e904-580">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3e904-581">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3e904-582">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-582">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3e904-583">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-583">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3e904-584">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3e904-585">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-585">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3e904-586">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-586">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="3e904-587">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="3e904-587">Configure(IConfiguration)</span></span>

<span data-ttu-id="3e904-588">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-588">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3e904-589">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-589">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3e904-590">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="3e904-590">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3e904-591">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-591">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3e904-592">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="3e904-592">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3e904-593">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-593">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3e904-594">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-594">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3e904-595">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="3e904-595">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3e904-596">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-596">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3e904-597">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="3e904-597">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3e904-598">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-598">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3e904-599">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="3e904-599">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3e904-600">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-600">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3e904-601">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-601">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3e904-602">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-602">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3e904-603">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-603">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3e904-604">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-604">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3e904-605">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-605">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3e904-606">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-606">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3e904-607">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-607">Supported configurations described next:</span></span>

* <span data-ttu-id="3e904-608">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="3e904-608">No configuration</span></span>
* <span data-ttu-id="3e904-609">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="3e904-609">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3e904-610">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="3e904-610">Change the defaults in code</span></span>

<span data-ttu-id="3e904-611">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="3e904-611">*No configuration*</span></span>

<span data-ttu-id="3e904-612">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="3e904-612">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3e904-613">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="3e904-613">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3e904-614">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-614">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3e904-615">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-615">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3e904-616">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-616">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3e904-617">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="3e904-617">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3e904-618">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-618">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3e904-619">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="3e904-619">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3e904-620">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-620">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3e904-621">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-621">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3e904-622">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="3e904-622">Schema notes:</span></span>

* <span data-ttu-id="3e904-623">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-623">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3e904-624">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="3e904-624">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3e904-625">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-625">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3e904-626">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-626">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3e904-627">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-627">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3e904-628">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-628">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3e904-629">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-629">The `Certificate` section is optional.</span></span> <span data-ttu-id="3e904-630">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-630">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3e904-631">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-631">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3e904-632">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-632">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3e904-633">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-633">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3e904-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="3e904-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="3e904-635">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-635">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3e904-636">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-636">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3e904-637">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-637">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3e904-638">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-638">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3e904-639">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-639">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3e904-640">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-640">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3e904-641">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-641">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3e904-642">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="3e904-642">*Change the defaults in code*</span></span>

<span data-ttu-id="3e904-643">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3e904-644">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-644">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="3e904-645">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="3e904-645">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3e904-646">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-646">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3e904-647">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="3e904-647">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3e904-648">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-648">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3e904-649">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-649">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3e904-650">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-650">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3e904-651">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="3e904-651">SNI support requires:</span></span>

* <span data-ttu-id="3e904-652">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="3e904-652">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3e904-653">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="3e904-653">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3e904-654">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="3e904-654">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3e904-655">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-655">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3e904-656">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-656">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="3e904-657">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="3e904-657">Connection logging</span></span>

<span data-ttu-id="3e904-658">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-658">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3e904-659">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-659">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3e904-660">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-660">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3e904-661">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-661">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3e904-662">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-662">Bind to a TCP socket</span></span>

<span data-ttu-id="3e904-663"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="3e904-663">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3e904-664">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-664">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3e904-665">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-665">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3e904-666">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-666">Bind to a Unix socket</span></span>

<span data-ttu-id="3e904-667">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-667">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3e904-668">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="3e904-668">Port 0</span></span>

<span data-ttu-id="3e904-669">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-669">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3e904-670">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="3e904-670">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3e904-671">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="3e904-671">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3e904-672">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="3e904-672">Limitations</span></span>

<span data-ttu-id="3e904-673">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-673">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3e904-674">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-674">`--urls` command-line argument</span></span>
* <span data-ttu-id="3e904-675">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="3e904-675">`urls` host configuration key</span></span>
* <span data-ttu-id="3e904-676">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-676">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3e904-677">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-677">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3e904-678">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="3e904-678">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3e904-679">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-679">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3e904-680">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="3e904-680">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3e904-681">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-681">IIS endpoint configuration</span></span>

<span data-ttu-id="3e904-682">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-682">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3e904-683">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-683">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3e904-684">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="3e904-684">ListenOptions.Protocols</span></span>

<span data-ttu-id="3e904-685">`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler.</span><span class="sxs-lookup"><span data-stu-id="3e904-685">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3e904-686">`HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-686">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3e904-687">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="3e904-687">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3e904-688">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="3e904-688">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3e904-689">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3e904-689">HTTP/1.1 only.</span></span> <span data-ttu-id="3e904-690">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-690">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3e904-691">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3e904-691">HTTP/2 only.</span></span> <span data-ttu-id="3e904-692">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-692">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3e904-693">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3e904-693">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3e904-694">HTTP/2 bir TLS ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-694">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3e904-695">Varsayılan protokol HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-695">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="3e904-696">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="3e904-696">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3e904-697">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="3e904-697">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3e904-698">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="3e904-698">Renegotiation disabled</span></span>
* <span data-ttu-id="3e904-699">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="3e904-699">Compression disabled</span></span>
* <span data-ttu-id="3e904-700">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="3e904-700">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3e904-701">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="3e904-701">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3e904-702">Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="3e904-702">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3e904-703">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="3e904-703">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3e904-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3e904-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3e904-705">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="3e904-705">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3e904-706">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="3e904-706">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="3e904-707">İsteğe bağlı olarak, belirli şifrelemeler için bağlantı başına TLS el sıkışmaları filtrelemek üzere `IConnectionAdapter` bir uygulama oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3e904-707">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="3e904-708">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="3e904-708">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3e904-709">Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3e904-710">Aşağıdaki *appSettings. JSON* örneğinde, tüm Kestrel uç noktaları için varsayılan bir bağlantı protokolü (http/1.1 ve http/2) oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="3e904-710">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="3e904-711">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç nokta için bağlantı protokolü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3e904-711">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="3e904-712">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="3e904-712">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3e904-713">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-713">Transport configuration</span></span>

<span data-ttu-id="3e904-714">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-714">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3e904-715">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ' a çağrı yapan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamalarının önemli bir değişikliği olur ve aşağıdaki paketlerden birine bağımlıdır:</span><span class="sxs-lookup"><span data-stu-id="3e904-715">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3e904-716">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="3e904-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3e904-717">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="3e904-717">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3e904-718">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="3e904-718">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3e904-719">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-719">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3e904-720">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="3e904-720">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3e904-721">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-721">URL prefixes</span></span>

<span data-ttu-id="3e904-722">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-722">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3e904-723">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-723">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3e904-724">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-724">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3e904-725">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-725">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3e904-726">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-726">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3e904-727">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-727">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3e904-728">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="3e904-728">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3e904-729">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-729">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3e904-730">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="3e904-730">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3e904-731">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-731">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3e904-732">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-732">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3e904-733">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-733">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3e904-734">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-734">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3e904-735">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-735">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3e904-736">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-736">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3e904-737">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3e904-737">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3e904-738">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="3e904-738">Host filtering</span></span>

<span data-ttu-id="3e904-739">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-739">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3e904-740">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-740">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3e904-741">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-741">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3e904-742">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="3e904-742">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3e904-743">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-743">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3e904-744">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-744">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3e904-745">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="3e904-745">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3e904-746">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-746">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3e904-747">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="3e904-747">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3e904-748">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="3e904-748">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3e904-749">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="3e904-749">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3e904-750">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-750">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3e904-751">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-751">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3e904-752">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-752">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3e904-753">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="3e904-753">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3e904-754">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3e904-754">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="3e904-755">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3e904-755">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3e904-756">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-756">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3e904-757">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="3e904-757">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3e904-758">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3e904-758">HTTPS</span></span>
* <span data-ttu-id="3e904-759">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="3e904-759">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3e904-760">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="3e904-760">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="3e904-761">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3e904-761">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3e904-762">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3e904-762">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3e904-763">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="3e904-763">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3e904-764">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-764">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3e904-765">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="3e904-765">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3e904-766">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-766">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3e904-768">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3e904-768">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3e904-770">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="3e904-770">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3e904-771">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-771">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3e904-772">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel ' `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="3e904-772">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3e904-773">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-773">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3e904-774">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-774">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3e904-775">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="3e904-775">A reverse proxy:</span></span>

* <span data-ttu-id="3e904-776">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-776">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3e904-777">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-777">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3e904-778">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-778">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3e904-779">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-779">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3e904-780">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-780">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-781">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-781">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3e904-782">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="3e904-782">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3e904-783">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="3e904-783">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3e904-784">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-784">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3e904-785">*Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-785">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="3e904-786">`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-786">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="3e904-787">`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-787">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="3e904-788">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-788">Kestrel options</span></span>

<span data-ttu-id="3e904-789">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-789">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3e904-790"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-790">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3e904-791">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-791">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3e904-792">Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanı kullanılır:</span><span class="sxs-lookup"><span data-stu-id="3e904-792">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3e904-793">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-793">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3e904-794">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3e904-794">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="3e904-795">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="3e904-795">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3e904-796">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-796">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3e904-797">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-797">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3e904-798">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-798">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3e904-799">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e904-799">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3e904-800">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-800">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3e904-801">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-801">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="3e904-802">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-802">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3e904-803">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-803">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3e904-804">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-804">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3e904-805">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="3e904-805">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="3e904-806">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="3e904-806">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3e904-807">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-807">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="3e904-808">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="3e904-808">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3e904-809">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-809">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="3e904-810">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="3e904-810">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3e904-811">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="3e904-811">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3e904-812">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="3e904-812">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3e904-813">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="3e904-813">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3e904-814">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-814">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="3e904-815">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="3e904-815">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3e904-816">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3e904-816">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3e904-817">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-817">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3e904-818">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-818">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3e904-819">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="3e904-819">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3e904-820">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-820">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3e904-821">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="3e904-821">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3e904-822">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-822">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3e904-823">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-823">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3e904-824">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-824">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3e904-825">İstek sınırı ve yanıt sınırı ayarlanacak kod, özellik ve arabirim adlarında `RequestBody` veya `Response` olması dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-825">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3e904-826">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3e904-826">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="3e904-827">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="3e904-827">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3e904-828">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-828">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3e904-829">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="3e904-829">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="3e904-830">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="3e904-830">Synchronous IO</span></span>

<span data-ttu-id="3e904-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="3e904-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3e904-832">Varsayılan değer `true` ' dır.</span><span class="sxs-lookup"><span data-stu-id="3e904-832">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3e904-833">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-833">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3e904-834">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3e904-834">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3e904-835">Aşağıdaki örnek, zaman uyumlu GÇ 'yi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="3e904-835">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="3e904-836">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="3e904-836">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3e904-837">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-837">Endpoint configuration</span></span>

<span data-ttu-id="3e904-838">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="3e904-838">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3e904-839">`https://localhost:5001` (yerel bir geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="3e904-839">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3e904-840">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="3e904-840">Specify URLs using the:</span></span>

* <span data-ttu-id="3e904-841">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3e904-841">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3e904-842">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="3e904-842">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3e904-843">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="3e904-843">`urls` host configuration key.</span></span>
* <span data-ttu-id="3e904-844">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e904-844">`UseUrls` extension method.</span></span>

<span data-ttu-id="3e904-845">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3e904-845">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3e904-846">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3e904-846">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3e904-847">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-847">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3e904-848">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="3e904-848">A development certificate is created:</span></span>

* <span data-ttu-id="3e904-849">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="3e904-849">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3e904-850">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-850">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3e904-851">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-851">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3e904-852">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-852">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3e904-853">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> üzerinde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-853">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3e904-854">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="3e904-854">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3e904-855">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="3e904-855">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3e904-856">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3e904-857">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-857">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3e904-858">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-858">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3e904-859">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="3e904-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3e904-860">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="3e904-860">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3e904-861">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-861">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="3e904-862">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="3e904-862">Configure(IConfiguration)</span></span>

<span data-ttu-id="3e904-863">Giriş olarak <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-863">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3e904-864">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-864">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3e904-865">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="3e904-865">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3e904-866">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-866">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3e904-867">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="3e904-867">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3e904-868">`UseHttps` &ndash; Kestrel varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-868">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3e904-869">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3e904-869">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3e904-870">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="3e904-870">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3e904-871">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yol ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-871">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3e904-872">`password`, X. 509.440 sertifika verilerine erişmek için gereken paroladır.</span><span class="sxs-lookup"><span data-stu-id="3e904-872">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3e904-873">`configureOptions`, `HttpsConnectionAdapterOptions` ' i yapılandırmak için bir `Action` ' dir.</span><span class="sxs-lookup"><span data-stu-id="3e904-873">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3e904-874">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="3e904-874">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3e904-875">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-875">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3e904-876">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-876">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3e904-877">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e904-877">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3e904-878">`location` ' dan sertifikayı yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="3e904-878">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3e904-879">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-879">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3e904-880">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e904-880">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3e904-881">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-881">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3e904-882">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3e904-882">Supported configurations described next:</span></span>

* <span data-ttu-id="3e904-883">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="3e904-883">No configuration</span></span>
* <span data-ttu-id="3e904-884">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="3e904-884">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3e904-885">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="3e904-885">Change the defaults in code</span></span>

<span data-ttu-id="3e904-886">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="3e904-886">*No configuration*</span></span>

<span data-ttu-id="3e904-887">Kestrel `http://localhost:5000` ve `https://localhost:5001` ' i dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="3e904-887">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3e904-888">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="3e904-888">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3e904-889">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-889">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3e904-890">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-890">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3e904-891">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-891">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3e904-892">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="3e904-892">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3e904-893">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e904-893">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3e904-894">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="3e904-894">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3e904-895">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="3e904-895">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3e904-896">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="3e904-896">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3e904-897">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="3e904-897">Schema notes:</span></span>

* <span data-ttu-id="3e904-898">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-898">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3e904-899">Örneğin, `HTTPS` ve `Https` geçerli.</span><span class="sxs-lookup"><span data-stu-id="3e904-899">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3e904-900">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-900">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3e904-901">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-901">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3e904-902">Bu uç noktalar, en üst düzey `Urls` yapılandırmasında tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-902">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3e904-903">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-903">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3e904-904">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-904">The `Certificate` section is optional.</span></span> <span data-ttu-id="3e904-905">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-905">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3e904-906">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-906">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3e904-907">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-907">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3e904-908">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-908">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3e904-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemine sahip bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="3e904-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="3e904-910">`KestrelServerOptions.ConfigurationLoader`, mevcut yükleyicde (örneğin, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından sağlandıysa) yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-910">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3e904-911">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-911">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3e904-912">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-912">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3e904-913">Önceki örneklerde `Load` açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-913">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3e904-914">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` ' a çağrı yapmaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-914">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3e904-915">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-915">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3e904-916">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-916">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3e904-917">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="3e904-917">*Change the defaults in code*</span></span>

<span data-ttu-id="3e904-918">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions` için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3e904-919">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-919">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="3e904-920">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="3e904-920">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3e904-921">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-921">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3e904-922">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="3e904-922">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3e904-923">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-923">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3e904-924">Kestrel, `ServerCertificateSelector` geri çağırması aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="3e904-924">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3e904-925">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3e904-925">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3e904-926">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="3e904-926">SNI support requires:</span></span>

* <span data-ttu-id="3e904-927">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="3e904-927">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3e904-928">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="3e904-928">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3e904-929">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="3e904-929">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3e904-930">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-930">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3e904-931">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-931">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="3e904-932">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="3e904-932">Connection logging</span></span>

<span data-ttu-id="3e904-933">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="3e904-933">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3e904-934">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-934">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3e904-935">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-935">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3e904-936">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-936">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3e904-937">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-937">Bind to a TCP socket</span></span>

<span data-ttu-id="3e904-938"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="3e904-938">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="3e904-939">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="3e904-939">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3e904-940">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-940">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3e904-941">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="3e904-941">Bind to a Unix socket</span></span>

<span data-ttu-id="3e904-942">Aşağıdaki örnekte gösterildiği gibi NGINX ile iyileştirilmiş performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ile UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-942">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="3e904-943">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="3e904-943">Port 0</span></span>

<span data-ttu-id="3e904-944">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-944">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3e904-945">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="3e904-945">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3e904-946">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="3e904-946">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3e904-947">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="3e904-947">Limitations</span></span>

<span data-ttu-id="3e904-948">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="3e904-948">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3e904-949">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-949">`--urls` command-line argument</span></span>
* <span data-ttu-id="3e904-950">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="3e904-950">`urls` host configuration key</span></span>
* <span data-ttu-id="3e904-951">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="3e904-951">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3e904-952">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-952">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3e904-953">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="3e904-953">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3e904-954">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya bir yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-954">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3e904-955">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="3e904-955">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3e904-956">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-956">IIS endpoint configuration</span></span>

<span data-ttu-id="3e904-957">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-957">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3e904-958">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="3e904-958">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3e904-959">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="3e904-959">Transport configuration</span></span>

<span data-ttu-id="3e904-960">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="3e904-960">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3e904-961">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ' a çağrı yapan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamalarının önemli bir değişikliği olur ve aşağıdaki paketlerden birine bağımlıdır:</span><span class="sxs-lookup"><span data-stu-id="3e904-961">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3e904-962">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="3e904-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3e904-963">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="3e904-963">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3e904-964">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="3e904-964">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3e904-965">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3e904-965">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3e904-966">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="3e904-966">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3e904-967">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="3e904-967">URL prefixes</span></span>

<span data-ttu-id="3e904-968">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e904-968">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3e904-969">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e904-969">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3e904-970">Kestrel, `UseUrls` kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="3e904-970">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3e904-971">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-971">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3e904-972">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-972">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3e904-973">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="3e904-973">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3e904-974">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="3e904-974">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3e904-975">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-975">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3e904-976">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="3e904-976">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3e904-977">Geçerli bir IP adresi veya `localhost` olarak tanınmayan her türlü tüm IPv4 ve IPv6 IP 'lerine bağlar.</span><span class="sxs-lookup"><span data-stu-id="3e904-977">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3e904-978">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-978">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3e904-979">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e904-979">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3e904-980">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="3e904-980">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3e904-981">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="3e904-981">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3e904-982">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="3e904-982">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3e904-983">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="3e904-983">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3e904-984">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="3e904-984">Host filtering</span></span>

<span data-ttu-id="3e904-985">Kestrel, `http://example.com:5000` gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="3e904-985">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3e904-986">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="3e904-986">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3e904-987">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-987">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3e904-988">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="3e904-988">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3e904-989">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e904-989">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3e904-990">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3e904-990">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3e904-991">Ara yazılım <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tarafından eklenir ve <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="3e904-991">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3e904-992">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="3e904-992">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3e904-993">Ara yazılımı etkinleştirmek için *appSettings. json*/ appsettings içinde `AllowedHosts` anahtarı tanımlayın. *\<EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="3e904-993">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3e904-994">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="3e904-994">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3e904-995">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="3e904-995">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3e904-996">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği içerir.</span><span class="sxs-lookup"><span data-stu-id="3e904-996">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3e904-997">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3e904-997">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3e904-998">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="3e904-998">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3e904-999">Kestrel, genel kullanıma yönelik bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara sunucusu ile `AllowedHosts` ' nın ayarlanması uygundur.</span><span class="sxs-lookup"><span data-stu-id="3e904-999">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3e904-1000">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3e904-1000">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3e904-1001">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3e904-1001">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="3e904-1002">RFC 7230: Ileti sözdizimi ve yönlendirme (Bölüm 5,4: Ana bilgisayar)</span><span class="sxs-lookup"><span data-stu-id="3e904-1002">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
