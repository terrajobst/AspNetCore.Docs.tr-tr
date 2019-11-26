---
title: ASP.NET Core Web sunucusu uygulamasını Kestrel
author: guardrex
description: ASP.NET Core platformlar arası Web sunucusu olan Kestrel hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 6fba6689f72f7a565e28d80f6770765ab097cf11
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289098"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fceba-103">ASP.NET Core Web sunucusu uygulamasını Kestrel</span><span class="sxs-lookup"><span data-stu-id="fceba-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fceba-104">[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher)No, [Stephen halter](https://twitter.com/halter73)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="fceba-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fceba-105">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fceba-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fceba-106">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fceba-107">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="fceba-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fceba-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fceba-108">HTTPS</span></span>
* <span data-ttu-id="fceba-109">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="fceba-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fceba-110">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="fceba-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="fceba-111">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="fceba-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="fceba-112">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="fceba-113">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fceba-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fceba-114">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fceba-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="fceba-115">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="fceba-115">HTTP/2 support</span></span>

<span data-ttu-id="fceba-116">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="fceba-117">İşletim sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="fceba-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="fceba-118">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="fceba-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="fceba-119">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="fceba-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="fceba-120">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fceba-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="fceba-121">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="fceba-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="fceba-122">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="fceba-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fceba-123">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="fceba-124">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="fceba-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="fceba-125">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="fceba-126">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="fceba-127">Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fceba-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="fceba-128">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="fceba-129">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fceba-130">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="fceba-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fceba-131">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fceba-132">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="fceba-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fceba-133">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fceba-135">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fceba-137">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="fceba-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fceba-138">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fceba-139">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="fceba-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fceba-140">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fceba-141">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fceba-142">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="fceba-142">A reverse proxy:</span></span>

* <span data-ttu-id="fceba-143">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fceba-144">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fceba-145">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fceba-146">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fceba-147">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-148">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fceba-149">ASP.NET Core uygulamalarda Kestrel</span><span class="sxs-lookup"><span data-stu-id="fceba-149">Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fceba-150">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fceba-151">*Program.cs*içinde <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> yöntemi <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>çağırır:</span><span class="sxs-lookup"><span data-stu-id="fceba-151">In *Program.cs*, the <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults*> method calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=8)]

<span data-ttu-id="fceba-152">Konak oluşturma hakkında daha fazla bilgi için <xref:fundamentals/host/generic-host#set-up-a-host>*ana bilgisayar* ve *Varsayılan Oluşturucu ayarlarını* ayarlama bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-152">For more information on building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="fceba-153">`ConfigureWebHostDefaults`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:</span><span class="sxs-lookup"><span data-stu-id="fceba-153">To provide additional configuration after calling `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="fceba-154">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-154">Kestrel options</span></span>

<span data-ttu-id="fceba-155">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fceba-156"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fceba-157">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fceba-158">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="fceba-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="fceba-159">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-159">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="fceba-160">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fceba-160">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="fceba-161">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="fceba-161">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="fceba-162">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-162">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="fceba-163">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-163">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="fceba-164">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-164">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="fceba-165">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-165">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="fceba-166">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-166">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="fceba-167">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-167">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="fceba-168">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-168">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="fceba-169">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-169">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fceba-170">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-170">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fceba-171">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="fceba-171">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="fceba-172">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="fceba-172">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fceba-173">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="fceba-174">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fceba-175">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="fceba-176">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="fceba-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fceba-177">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-177">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fceba-178">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="fceba-178">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fceba-179">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="fceba-179">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fceba-180">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-180">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="fceba-181">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="fceba-181">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fceba-182">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fceba-182">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fceba-183">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-183">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fceba-184">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-184">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fceba-185">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="fceba-185">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fceba-186">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-186">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fceba-187">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="fceba-187">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fceba-188">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-188">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fceba-189">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-189">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fceba-190">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-190">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fceba-191">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-191">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fceba-192">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-192">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="fceba-193">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="fceba-193">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="fceba-194">Önceki örnekte başvurulan <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> HTTP/2 istekleri için `HttpContext.Features` mevcut değildir, çünkü bir istek temelli olarak hız sınırlarını değiştirmek, protokolün istek çoğullama desteği nedeniyle HTTP/2 için genel olarak desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="fceba-194">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="fceba-195">Ancak, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> HTTP/2 istekleri için `HttpContext.Features` olmaya devam eder, çünkü okuma hızı sınırı, bir HTTP/2 isteği için bile `IHttpMinRequestBodyDataRateFeature.MinDataRate` `null` olarak ayarlanarak tamamen istek temelli olarak *devre dışı* bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-195">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="fceba-196">`IHttpMinRequestBodyDataRateFeature.MinDataRate` okumaya veya `null` dışında bir değere ayarlamaya çalışırken, bir HTTP/2 isteği verilen bir `NotSupportedException` atılmaya neden olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-196">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="fceba-197">`KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-197">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="fceba-198">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-198">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fceba-199">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-199">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fceba-200">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="fceba-200">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="fceba-201">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="fceba-201">Maximum streams per connection</span></span>

<span data-ttu-id="fceba-202">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-202">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="fceba-203">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="fceba-203">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="fceba-204">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="fceba-204">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="fceba-205">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-205">Header table size</span></span>

<span data-ttu-id="fceba-206">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="fceba-206">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="fceba-207">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-207">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="fceba-208">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="fceba-208">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="fceba-209">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-209">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="fceba-210">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-210">Maximum frame size</span></span>

<span data-ttu-id="fceba-211">`Http2.MaxFrameSize`, sunucu tarafından alınan veya gönderilen HTTP/2 bağlantı çerçevesi yükünün izin verilen en büyük boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-211">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="fceba-212">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-212">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="fceba-213">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="fceba-213">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="fceba-214">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-214">Maximum request header size</span></span>

<span data-ttu-id="fceba-215">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-215">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="fceba-216">Bu sınır, sıkıştırılmış ve sıkıştırılmamış temsillerinde hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-216">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="fceba-217">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-217">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="fceba-218">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-218">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="fceba-219">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-219">Initial connection window size</span></span>

<span data-ttu-id="fceba-220">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-220">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="fceba-221">İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-221">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fceba-222">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-222">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="fceba-223">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="fceba-223">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="fceba-224">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-224">Initial stream window size</span></span>

<span data-ttu-id="fceba-225">`Http2.InitialStreamWindowSize`, istek (Stream) başına bir seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-225">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="fceba-226">İstekler aynı zamanda `Http2.InitialConnectionWindowSize`ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-226">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="fceba-227">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-227">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="fceba-228">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="fceba-228">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="fceba-229">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="fceba-229">Synchronous IO</span></span>

<span data-ttu-id="fceba-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-230"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fceba-231">Varsayılan değer `false` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="fceba-231">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-232">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-232">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fceba-233">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="fceba-233">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fceba-234">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="fceba-234">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="fceba-235">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="fceba-235">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fceba-236">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-236">Endpoint configuration</span></span>

<span data-ttu-id="fceba-237">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="fceba-237">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fceba-238">`https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)</span><span class="sxs-lookup"><span data-stu-id="fceba-238">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fceba-239">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="fceba-239">Specify URLs using the:</span></span>

* <span data-ttu-id="fceba-240">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fceba-240">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fceba-241">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="fceba-241">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fceba-242">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="fceba-242">`urls` host configuration key.</span></span>
* <span data-ttu-id="fceba-243">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fceba-243">`UseUrls` extension method.</span></span>

<span data-ttu-id="fceba-244">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fceba-244">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fceba-245">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fceba-245">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fceba-246">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-246">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fceba-247">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="fceba-247">A development certificate is created:</span></span>

* <span data-ttu-id="fceba-248">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="fceba-248">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fceba-249">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-249">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fceba-250">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-250">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fceba-251">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="fceba-251">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fceba-252">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-252">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fceba-253">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="fceba-253">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fceba-254">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="fceba-254">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fceba-255">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-255">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fceba-256">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-256">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fceba-257">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-257">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

> [!NOTE]
> <span data-ttu-id="fceba-258"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-258">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fceba-259">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fceba-260">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fceba-261">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="fceba-262"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-262">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="fceba-263">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="fceba-263">Configure(IConfiguration)</span></span>

<span data-ttu-id="fceba-264">Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-264">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fceba-265">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-265">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fceba-266">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="fceba-266">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fceba-267">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-267">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fceba-268">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="fceba-268">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fceba-269">`UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-269">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fceba-270">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-270">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fceba-271">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="fceba-271">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fceba-272">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-272">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fceba-273">X. 509.440 sertifika verilerine erişmek için gereken parola `password`.</span><span class="sxs-lookup"><span data-stu-id="fceba-273">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fceba-274">`configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`.</span><span class="sxs-lookup"><span data-stu-id="fceba-274">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fceba-275">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="fceba-275">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fceba-276">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-276">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fceba-277">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-277">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fceba-278">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-278">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fceba-279">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-279">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fceba-280">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-280">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fceba-281">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-281">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fceba-282">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-282">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fceba-283">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-283">Supported configurations described next:</span></span>

* <span data-ttu-id="fceba-284">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="fceba-284">No configuration</span></span>
* <span data-ttu-id="fceba-285">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="fceba-285">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fceba-286">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="fceba-286">Change the defaults in code</span></span>

<span data-ttu-id="fceba-287">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="fceba-287">*No configuration*</span></span>

<span data-ttu-id="fceba-288">Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="fceba-288">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fceba-289">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="fceba-289">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fceba-290">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-290">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fceba-291">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-291">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fceba-292">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-292">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fceba-293">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="fceba-293">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fceba-294">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-294">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fceba-295">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="fceba-295">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="fceba-296">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-296">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fceba-297">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-297">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fceba-298">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="fceba-298">Schema notes:</span></span>

* <span data-ttu-id="fceba-299">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-299">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fceba-300">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-300">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fceba-301">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-301">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fceba-302">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-302">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fceba-303">Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-303">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fceba-304">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-304">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fceba-305">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-305">The `Certificate` section is optional.</span></span> <span data-ttu-id="fceba-306">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-306">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fceba-307">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-307">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fceba-308">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-308">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fceba-309">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-309">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fceba-310">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="fceba-310">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="fceba-311">`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-311">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fceba-312">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-312">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fceba-313">`options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-313">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fceba-314">`Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-314">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fceba-315">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-315">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fceba-316">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-316">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fceba-317">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-317">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fceba-318">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="fceba-318">*Change the defaults in code*</span></span>

<span data-ttu-id="fceba-319">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fceba-320">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-320">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="fceba-321">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="fceba-321">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fceba-322">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-322">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fceba-323">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="fceba-323">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fceba-324">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-324">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fceba-325">Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-325">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fceba-326">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-326">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fceba-327">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="fceba-327">SNI support requires:</span></span>

* <span data-ttu-id="fceba-328">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="fceba-328">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fceba-329">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="fceba-329">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fceba-330">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="fceba-330">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fceba-331">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-331">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fceba-332">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-332">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="fceba-333">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="fceba-333">Connection logging</span></span>

<span data-ttu-id="fceba-334">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-334">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="fceba-335">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-335">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="fceba-336">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-336">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="fceba-337">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-337">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fceba-338">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-338">Bind to a TCP socket</span></span>

<span data-ttu-id="fceba-339"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="fceba-339">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="fceba-340">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-340">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fceba-341">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-341">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fceba-342">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-342">Bind to a Unix socket</span></span>

<span data-ttu-id="fceba-343">Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-343">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="fceba-344">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="fceba-344">Port 0</span></span>

<span data-ttu-id="fceba-345">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-345">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fceba-346">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fceba-346">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fceba-347">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="fceba-347">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fceba-348">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="fceba-348">Limitations</span></span>

<span data-ttu-id="fceba-349">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-349">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fceba-350">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-350">`--urls` command-line argument</span></span>
* <span data-ttu-id="fceba-351">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="fceba-351">`urls` host configuration key</span></span>
* <span data-ttu-id="fceba-352">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-352">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fceba-353">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-353">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fceba-354">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="fceba-354">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fceba-355">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-355">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fceba-356">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fceba-356">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fceba-357">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-357">IIS endpoint configuration</span></span>

<span data-ttu-id="fceba-358">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-358">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fceba-359">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-359">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="fceba-360">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="fceba-360">ListenOptions.Protocols</span></span>

<span data-ttu-id="fceba-361">`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler.</span><span class="sxs-lookup"><span data-stu-id="fceba-361">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="fceba-362">`HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-362">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="fceba-363">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="fceba-363">`HttpProtocols` enum value</span></span> | <span data-ttu-id="fceba-364">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="fceba-364">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="fceba-365">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fceba-365">HTTP/1.1 only.</span></span> <span data-ttu-id="fceba-366">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-366">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="fceba-367">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fceba-367">HTTP/2 only.</span></span> <span data-ttu-id="fceba-368">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-368">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="fceba-369">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fceba-369">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="fceba-370">HTTP/2, istemcinin TLS [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) el sıkışmasında http/2 seçmesini gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-370">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="fceba-371">Herhangi bir uç nokta için varsayılan `ListenOptions.Protocols` değeri `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="fceba-371">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="fceba-372">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="fceba-372">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="fceba-373">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fceba-373">TLS version 1.2 or later</span></span>
* <span data-ttu-id="fceba-374">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="fceba-374">Renegotiation disabled</span></span>
* <span data-ttu-id="fceba-375">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="fceba-375">Compression disabled</span></span>
* <span data-ttu-id="fceba-376">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="fceba-376">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="fceba-377">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="fceba-377">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="fceba-378">Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="fceba-378">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="fceba-379">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="fceba-379">Cipher suite not blacklisted</span></span>

<span data-ttu-id="fceba-380">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fceba-380">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="fceba-381">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fceba-381">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="fceba-382">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="fceba-382">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="fceba-383">Gerektiğinde belirli şifrelemeler için TLS el sıkışmaları için bağlantı temelinde filtre uygulamak üzere bağlantı ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-383">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="fceba-384">Aşağıdaki örnek, uygulamanın desteklemediği tüm şifre algoritmaların <xref:System.NotSupportedException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-384">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="fceba-385">Alternatif olarak, [ılshandshakefeature. CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) öğesini kabul edilebilir şifreleme paketleri listesi ile tanımlayın ve karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-385">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="fceba-386">[CipherAlgorithmType. null](xref:System.Security.Authentication.CipherAlgorithmType) şifre algoritması ile hiçbir şifreleme kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-386">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

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

<span data-ttu-id="fceba-387">Bağlantı filtrelemesi, bir <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda aracılığıyla da yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-387">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

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

<span data-ttu-id="fceba-388">Linux 'ta <xref:System.Net.Security.CipherSuitesPolicy>, TLS el sıkışmaları her bağlantı temelinde filtrelemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-388">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

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

<span data-ttu-id="fceba-389">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="fceba-389">*Set the protocol from configuration*</span></span>

<span data-ttu-id="fceba-390">Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-390">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="fceba-391">Aşağıdaki *appSettings. JSON* örneği, tüm uç noktalar için varsayılan bağlantı protokolü olarak http/1.1 oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fceba-391">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="fceba-392">Aşağıdaki *appSettings. JSON* örneği, belirli bir uç nokta için http/1.1 bağlantı protokolünü belirler:</span><span class="sxs-lookup"><span data-stu-id="fceba-392">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="fceba-393">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="fceba-393">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fceba-394">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-394">Transport configuration</span></span>

<span data-ttu-id="fceba-395">Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="fceba-395">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="fceba-396">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-396">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="fceba-397">`IWebHostBuilder`<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-397">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="fceba-398">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-398">URL prefixes</span></span>

<span data-ttu-id="fceba-399">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-399">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fceba-400">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-400">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fceba-401">Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-401">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fceba-402">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-402">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fceba-403">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-403">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fceba-404">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-404">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fceba-405">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="fceba-405">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fceba-406">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-406">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fceba-407">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="fceba-407">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fceba-408">Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-408">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fceba-409">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-409">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fceba-410">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-410">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fceba-411">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-411">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fceba-412">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-412">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fceba-413">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-413">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fceba-414">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="fceba-414">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fceba-415">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="fceba-415">Host filtering</span></span>

<span data-ttu-id="fceba-416">Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-416">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fceba-417">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-417">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fceba-418">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-418">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fceba-419">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="fceba-419">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fceba-420">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-420">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fceba-421">Ana bilgisayar filtreleme ara yazılımı, ASP.NET Core uygulamaları için örtük olarak sunulan [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-421">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="fceba-422">Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:</span><span class="sxs-lookup"><span data-stu-id="fceba-422">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fceba-423">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-423">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fceba-424">Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="fceba-424">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fceba-425">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="fceba-425">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fceba-426">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="fceba-426">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fceba-427">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-427">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fceba-428">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-428">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fceba-429">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-429">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fceba-430">Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.</span><span class="sxs-lookup"><span data-stu-id="fceba-430">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fceba-431">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fceba-431">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fceba-432">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fceba-432">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fceba-433">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-433">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fceba-434">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="fceba-434">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fceba-435">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fceba-435">HTTPS</span></span>
* <span data-ttu-id="fceba-436">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="fceba-436">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fceba-437">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="fceba-437">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="fceba-438">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="fceba-438">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="fceba-439">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-439">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="fceba-440">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fceba-440">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fceba-441">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fceba-441">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="fceba-442">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="fceba-442">HTTP/2 support</span></span>

<span data-ttu-id="fceba-443">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-443">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="fceba-444">İşletim sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="fceba-444">Operating system&dagger;</span></span>
  * <span data-ttu-id="fceba-445">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="fceba-445">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="fceba-446">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="fceba-446">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="fceba-447">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fceba-447">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="fceba-448">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="fceba-448">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="fceba-449">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="fceba-449">TLS 1.2 or later connection</span></span>

<span data-ttu-id="fceba-450">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-450">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="fceba-451">&Dagger;Kestrel Windows Server 2012 R2 ve Windows 8.1 üzerinde HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="fceba-451">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="fceba-452">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-452">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="fceba-453">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-453">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="fceba-454">Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="fceba-454">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="fceba-455">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-455">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="fceba-456">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-456">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fceba-457">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="fceba-457">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fceba-458">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-458">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fceba-459">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="fceba-459">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fceba-460">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-460">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fceba-462">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-462">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fceba-464">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="fceba-464">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fceba-465">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-465">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fceba-466">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="fceba-466">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fceba-467">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-467">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fceba-468">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-468">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fceba-469">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="fceba-469">A reverse proxy:</span></span>

* <span data-ttu-id="fceba-470">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-470">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fceba-471">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-471">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fceba-472">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-472">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fceba-473">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-473">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fceba-474">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-474">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-475">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-475">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fceba-476">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="fceba-476">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fceba-477">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="fceba-477">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fceba-478">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-478">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fceba-479">*Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-479">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="fceba-480">`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-480">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="fceba-481">`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için `ConfigureKestrel`kullanın:</span><span class="sxs-lookup"><span data-stu-id="fceba-481">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="fceba-482">Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, `ConfigureKestrel`çağrılmadan **önce** <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-482">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="fceba-483">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-483">Kestrel options</span></span>

<span data-ttu-id="fceba-484">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-484">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fceba-485"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-485">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fceba-486">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-486">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fceba-487">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="fceba-487">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="fceba-488">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-488">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="fceba-489">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fceba-489">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="fceba-490">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="fceba-490">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="fceba-491">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-491">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="fceba-492">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-492">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="fceba-493">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-493">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="fceba-494">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-494">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="fceba-495">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-495">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="fceba-496">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-496">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="fceba-497">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-497">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="fceba-498">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-498">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fceba-499">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-499">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fceba-500">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="fceba-500">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="fceba-501">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="fceba-501">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fceba-502">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-502">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="fceba-503">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-503">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fceba-504">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-504">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="fceba-505">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="fceba-505">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fceba-506">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-506">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fceba-507">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="fceba-507">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fceba-508">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="fceba-508">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fceba-509">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-509">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="fceba-510">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="fceba-510">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fceba-511">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fceba-511">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fceba-512">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-512">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fceba-513">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-513">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fceba-514">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="fceba-514">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fceba-515">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-515">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fceba-516">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="fceba-516">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fceba-517">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-517">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fceba-518">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-518">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fceba-519">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-519">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fceba-520">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-520">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fceba-521">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-521">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="fceba-522">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="fceba-522">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="fceba-523">İstek çoğullama için protokol desteğinin desteklenmediği için, önceki örnekte başvurulan oran özelliği http/2 istekleri için `HttpContext.Features`, HTTP/2 için de hız sınırlarını değiştirmek için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="fceba-523">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="fceba-524">`KestrelServerOptions.Limits` aracılığıyla yapılandırılan sunucu genelindeki hız sınırları hem HTTP/1. x hem de HTTP/2 bağlantıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-524">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="fceba-525">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-525">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fceba-526">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-526">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fceba-527">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="fceba-527">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="fceba-528">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="fceba-528">Maximum streams per connection</span></span>

<span data-ttu-id="fceba-529">`Http2.MaxStreamsPerConnection`, HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-529">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="fceba-530">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="fceba-530">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="fceba-531">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="fceba-531">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="fceba-532">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-532">Header table size</span></span>

<span data-ttu-id="fceba-533">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="fceba-533">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="fceba-534">`Http2.HeaderTableSize`, HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-534">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="fceba-535">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="fceba-535">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="fceba-536">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-536">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="fceba-537">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-537">Maximum frame size</span></span>

<span data-ttu-id="fceba-538">`Http2.MaxFrameSize`, alacak HTTP/2 bağlantı çerçevesi yükünün en büyük boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-538">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="fceba-539">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-539">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="fceba-540">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="fceba-540">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="fceba-541">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-541">Maximum request header size</span></span>

<span data-ttu-id="fceba-542">`Http2.MaxRequestHeaderFieldSize`, istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-542">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="fceba-543">Bu sınır, hem sıkıştırılmış hem de sıkıştırılmamış temsillerinde birlikte hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-543">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="fceba-544">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-544">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="fceba-545">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-545">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="fceba-546">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-546">Initial connection window size</span></span>

<span data-ttu-id="fceba-547">`Http2.InitialConnectionWindowSize`, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-547">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="fceba-548">İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-548">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fceba-549">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-549">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="fceba-550">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="fceba-550">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="fceba-551">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-551">Initial stream window size</span></span>

<span data-ttu-id="fceba-552">`Http2.InitialStreamWindowSize`, istek (Stream) başına bir seferde sunucu arabelleklerinin bayt cinsinden en yüksek istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-552">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="fceba-553">İstekler aynı zamanda `Http2.InitialStreamWindowSize`ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-553">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="fceba-554">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-554">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="fceba-555">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="fceba-555">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="fceba-556">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="fceba-556">Synchronous IO</span></span>

<span data-ttu-id="fceba-557"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-557"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fceba-558">Varsayılan değer `true`.</span><span class="sxs-lookup"><span data-stu-id="fceba-558">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-559">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-559">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fceba-560">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="fceba-560">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fceba-561">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="fceba-561">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="fceba-562">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="fceba-562">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fceba-563">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-563">Endpoint configuration</span></span>

<span data-ttu-id="fceba-564">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="fceba-564">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fceba-565">`https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)</span><span class="sxs-lookup"><span data-stu-id="fceba-565">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fceba-566">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="fceba-566">Specify URLs using the:</span></span>

* <span data-ttu-id="fceba-567">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fceba-567">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fceba-568">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="fceba-568">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fceba-569">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="fceba-569">`urls` host configuration key.</span></span>
* <span data-ttu-id="fceba-570">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fceba-570">`UseUrls` extension method.</span></span>

<span data-ttu-id="fceba-571">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fceba-571">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fceba-572">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fceba-572">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fceba-573">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-573">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fceba-574">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="fceba-574">A development certificate is created:</span></span>

* <span data-ttu-id="fceba-575">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="fceba-575">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fceba-576">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-576">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fceba-577">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-577">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fceba-578">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="fceba-578">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fceba-579">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-579">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fceba-580">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="fceba-580">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fceba-581">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="fceba-581">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fceba-582">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-582">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fceba-583">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-583">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fceba-584">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-584">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="fceba-585"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-585">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fceba-586">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-586">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fceba-587">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-587">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fceba-588">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-588">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="fceba-589"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-589">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>


### <a name="configureiconfiguration"></a><span data-ttu-id="fceba-590">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="fceba-590">Configure(IConfiguration)</span></span>

<span data-ttu-id="fceba-591">Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-591">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fceba-592">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-592">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fceba-593">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="fceba-593">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fceba-594">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-594">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fceba-595">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="fceba-595">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fceba-596">`UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-596">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fceba-597">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-597">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fceba-598">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="fceba-598">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fceba-599">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-599">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fceba-600">X. 509.440 sertifika verilerine erişmek için gereken parola `password`.</span><span class="sxs-lookup"><span data-stu-id="fceba-600">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fceba-601">`configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`.</span><span class="sxs-lookup"><span data-stu-id="fceba-601">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fceba-602">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="fceba-602">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fceba-603">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-603">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fceba-604">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-604">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fceba-605">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-605">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fceba-606">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-606">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fceba-607">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-607">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fceba-608">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-608">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fceba-609">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-609">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fceba-610">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-610">Supported configurations described next:</span></span>

* <span data-ttu-id="fceba-611">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="fceba-611">No configuration</span></span>
* <span data-ttu-id="fceba-612">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="fceba-612">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fceba-613">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="fceba-613">Change the defaults in code</span></span>

<span data-ttu-id="fceba-614">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="fceba-614">*No configuration*</span></span>

<span data-ttu-id="fceba-615">Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="fceba-615">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fceba-616">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="fceba-616">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fceba-617">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-617">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fceba-618">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-618">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fceba-619">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-619">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fceba-620">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="fceba-620">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fceba-621">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-621">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fceba-622">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="fceba-622">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="fceba-623">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-623">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fceba-624">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-624">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fceba-625">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="fceba-625">Schema notes:</span></span>

* <span data-ttu-id="fceba-626">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-626">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fceba-627">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-627">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fceba-628">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-628">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fceba-629">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-629">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fceba-630">Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-630">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fceba-631">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-631">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fceba-632">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-632">The `Certificate` section is optional.</span></span> <span data-ttu-id="fceba-633">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-633">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fceba-634">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-634">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fceba-635">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-635">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fceba-636">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-636">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fceba-637">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="fceba-637">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="fceba-638">`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-638">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fceba-639">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-639">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fceba-640">`options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-640">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fceba-641">`Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-641">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fceba-642">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-642">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fceba-643">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-643">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fceba-644">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-644">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fceba-645">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="fceba-645">*Change the defaults in code*</span></span>

<span data-ttu-id="fceba-646">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-646">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fceba-647">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-647">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="fceba-648">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="fceba-648">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fceba-649">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-649">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fceba-650">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="fceba-650">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fceba-651">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-651">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fceba-652">Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-652">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fceba-653">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-653">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fceba-654">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="fceba-654">SNI support requires:</span></span>

* <span data-ttu-id="fceba-655">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="fceba-655">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fceba-656">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="fceba-656">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fceba-657">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="fceba-657">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fceba-658">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-658">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fceba-659">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-659">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="fceba-660">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="fceba-660">Connection logging</span></span>

<span data-ttu-id="fceba-661">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-661">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="fceba-662">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-662">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="fceba-663">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-663">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="fceba-664">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-664">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fceba-665">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-665">Bind to a TCP socket</span></span>

<span data-ttu-id="fceba-666"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="fceba-666">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="fceba-667">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-667">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fceba-668">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-668">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fceba-669">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-669">Bind to a Unix socket</span></span>

<span data-ttu-id="fceba-670">Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-670">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="fceba-671">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="fceba-671">Port 0</span></span>

<span data-ttu-id="fceba-672">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-672">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fceba-673">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fceba-673">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fceba-674">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="fceba-674">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fceba-675">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="fceba-675">Limitations</span></span>

<span data-ttu-id="fceba-676">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-676">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fceba-677">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-677">`--urls` command-line argument</span></span>
* <span data-ttu-id="fceba-678">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="fceba-678">`urls` host configuration key</span></span>
* <span data-ttu-id="fceba-679">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-679">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fceba-680">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-680">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fceba-681">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="fceba-681">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fceba-682">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-682">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fceba-683">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fceba-683">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fceba-684">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-684">IIS endpoint configuration</span></span>

<span data-ttu-id="fceba-685">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-685">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fceba-686">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-686">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="fceba-687">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="fceba-687">ListenOptions.Protocols</span></span>

<span data-ttu-id="fceba-688">`Protocols` özelliği, bir bağlantı uç noktasında veya sunucu için etkin HTTP protokollerini (`HttpProtocols`) belirler.</span><span class="sxs-lookup"><span data-stu-id="fceba-688">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="fceba-689">`HttpProtocols` numaralandırmasından `Protocols` özelliğine bir değer atayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-689">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="fceba-690">`HttpProtocols` Enum değeri</span><span class="sxs-lookup"><span data-stu-id="fceba-690">`HttpProtocols` enum value</span></span> | <span data-ttu-id="fceba-691">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="fceba-691">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="fceba-692">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="fceba-692">HTTP/1.1 only.</span></span> <span data-ttu-id="fceba-693">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-693">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="fceba-694">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fceba-694">HTTP/2 only.</span></span> <span data-ttu-id="fceba-695">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-695">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="fceba-696">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="fceba-696">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="fceba-697">HTTP/2 bir TLS ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-697">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="fceba-698">Varsayılan protokol HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="fceba-698">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="fceba-699">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="fceba-699">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="fceba-700">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fceba-700">TLS version 1.2 or later</span></span>
* <span data-ttu-id="fceba-701">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="fceba-701">Renegotiation disabled</span></span>
* <span data-ttu-id="fceba-702">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="fceba-702">Compression disabled</span></span>
* <span data-ttu-id="fceba-703">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="fceba-703">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="fceba-704">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="fceba-704">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="fceba-705">Sınırlı alan Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="fceba-705">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="fceba-706">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="fceba-706">Cipher suite not blacklisted</span></span>

<span data-ttu-id="fceba-707">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;&rbrack; `TLS-ECDHE`, P-256 eliptik eğrisi &lbrack;`FIPS186`&rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fceba-707">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="fceba-708">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fceba-708">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="fceba-709">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="fceba-709">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="fceba-710">İsteğe bağlı olarak, belirli şifrelemeler için bağlantı başına TLS el sıkışmaları filtrelemek üzere `IConnectionAdapter` bir uygulama oluşturun:</span><span class="sxs-lookup"><span data-stu-id="fceba-710">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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

<span data-ttu-id="fceba-711">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="fceba-711">*Set the protocol from configuration*</span></span>

<span data-ttu-id="fceba-712">Kestrel yapılandırmasını yüklemek için varsayılan olarak `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-712"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="fceba-713">Aşağıdaki *appSettings. JSON* örneğinde, tüm Kestrel uç noktaları için varsayılan bir bağlantı protokolü (http/1.1 ve http/2) oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="fceba-713">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="fceba-714">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç nokta için bağlantı protokolü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fceba-714">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="fceba-715">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="fceba-715">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fceba-716">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-716">Transport configuration</span></span>

<span data-ttu-id="fceba-717">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-717">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="fceba-718">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağıran ve aşağıdaki paketlerden birine bağlı olan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamaları için bir son değişiklik değildir:</span><span class="sxs-lookup"><span data-stu-id="fceba-718">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="fceba-719">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="fceba-719">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="fceba-720">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="fceba-720">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="fceba-721">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="fceba-721">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="fceba-722">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-722">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="fceba-723"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="fceba-723">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="fceba-724">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-724">URL prefixes</span></span>

<span data-ttu-id="fceba-725">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-725">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fceba-726">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-726">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fceba-727">Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-727">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fceba-728">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-728">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fceba-729">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-729">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fceba-730">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-730">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fceba-731">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="fceba-731">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fceba-732">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-732">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fceba-733">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="fceba-733">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fceba-734">Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-734">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fceba-735">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-735">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fceba-736">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-736">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fceba-737">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-737">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fceba-738">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-738">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fceba-739">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-739">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fceba-740">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="fceba-740">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fceba-741">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="fceba-741">Host filtering</span></span>

<span data-ttu-id="fceba-742">Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-742">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fceba-743">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-743">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fceba-744">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-744">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fceba-745">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="fceba-745">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fceba-746">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-746">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fceba-747">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-747">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="fceba-748">Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:</span><span class="sxs-lookup"><span data-stu-id="fceba-748">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fceba-749">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-749">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fceba-750">Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="fceba-750">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fceba-751">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="fceba-751">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fceba-752">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="fceba-752">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fceba-753">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-753">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fceba-754">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-754">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fceba-755">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-755">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fceba-756">Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.</span><span class="sxs-lookup"><span data-stu-id="fceba-756">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fceba-757">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fceba-757">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fceba-758">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="fceba-758">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="fceba-759">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-759">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="fceba-760">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="fceba-760">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="fceba-761">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fceba-761">HTTPS</span></span>
* <span data-ttu-id="fceba-762">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="fceba-762">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="fceba-763">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="fceba-763">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="fceba-764">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="fceba-764">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="fceba-765">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fceba-765">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fceba-766">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="fceba-766">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="fceba-767">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-767">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="fceba-768">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="fceba-768">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="fceba-769">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-769">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="fceba-771">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="fceba-771">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fceba-773">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="fceba-773">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="fceba-774">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-774">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="fceba-775">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel `Host` ' den bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="fceba-775">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="fceba-776">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-776">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fceba-777">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-777">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="fceba-778">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="fceba-778">A reverse proxy:</span></span>

* <span data-ttu-id="fceba-779">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-779">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="fceba-780">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-780">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fceba-781">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-781">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fceba-782">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-782">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="fceba-783">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-783">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-784">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-784">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fceba-785">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="fceba-785">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="fceba-786">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="fceba-786">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fceba-787">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-787">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fceba-788">*Program.cs*' de, şablon kodu, arka planda <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-788">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="fceba-789">`CreateDefaultBuilder`çağrıldıktan sonra ek yapılandırma sağlamak için, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>çağırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-789">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="fceba-790">`CreateDefaultBuilder` ve konak oluşturma hakkında daha fazla bilgi için, <xref:fundamentals/host/web-host#set-up-a-host>*bir konak ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-790">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="fceba-791">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-791">Kestrel options</span></span>

<span data-ttu-id="fceba-792">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-792">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="fceba-793"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfının <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliğindeki kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-793">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="fceba-794">`Limits` özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-794">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="fceba-795">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="fceba-795">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="fceba-796">Aşağıdaki örneklerde C# kodda yapılandırılan Kestrel seçenekleri de bir [yapılandırma sağlayıcısı](xref:fundamentals/configuration/index)kullanılarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-796">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="fceba-797">Örneğin, dosya yapılandırma sağlayıcısı bir *appSettings. JSON* veya appSettings 'ten Kestrel yapılandırmasını yükleyebilir *. { Environment}. JSON* dosyası:</span><span class="sxs-lookup"><span data-stu-id="fceba-797">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

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

<span data-ttu-id="fceba-798">Aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="fceba-798">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="fceba-799">`Startup.ConfigureServices`'de Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-799">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="fceba-800">`Startup` sınıfına bir `IConfiguration` örneği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-800">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="fceba-801">Aşağıdaki örnek, eklenen yapılandırmanın `Configuration` özelliğine atandığını varsayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-801">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="fceba-802">`Startup.ConfigureServices`, yapılandırmanın `Kestrel` bölümünü Kestrel 'in yapılandırmasına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="fceba-802">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="fceba-803">Ana bilgisayarı oluştururken Kestrel yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-803">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="fceba-804">*Program.cs*' de, yapılandırmanın `Kestrel` bölümünü Kestrel yapılandırmasına yükleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-804">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

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

<span data-ttu-id="fceba-805">Yukarıdaki yaklaşımların her ikisi de herhangi bir [yapılandırma sağlayıcısıyla](xref:fundamentals/configuration/index)çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-805">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="fceba-806">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-806">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="fceba-807">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-807">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="fceba-808">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="fceba-808">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="fceba-809">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="fceba-809">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="fceba-810">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-810">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="fceba-811">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-811">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fceba-812">Bir bağlantı yükseltildikten sonra, `MaxConcurrentConnections` sınırına göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-812">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="fceba-813">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="fceba-813">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="fceba-814">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="fceba-814">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="fceba-815">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="fceba-815">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="fceba-816">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="fceba-816">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fceba-817">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-817">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="fceba-818">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="fceba-818">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fceba-819">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fceba-819">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="fceba-820">`MaxRequestBodySize` özelliğinin salt okuma durumunda olup olmadığını belirten `IsReadOnly` bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-820">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fceba-821">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-821">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="fceba-822">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="fceba-822">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="fceba-823">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-823">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="fceba-824">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="fceba-824">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fceba-825">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-825">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fceba-826">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-826">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="fceba-827">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-827">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fceba-828">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında `RequestBody` veya `Response` sahip olma dışında aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-828">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="fceba-829">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fceba-829">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="fceba-830">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="fceba-830">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="fceba-831">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fceba-831">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="fceba-832">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="fceba-832">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="fceba-833">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="fceba-833">Synchronous IO</span></span>

<span data-ttu-id="fceba-834"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>, istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="fceba-834"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="fceba-835">Varsayılan değer `true`.</span><span class="sxs-lookup"><span data-stu-id="fceba-835">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="fceba-836">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-836">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="fceba-837">Yalnızca zaman uyumsuz GÇ desteklemeyen bir kitaplık kullanırken `AllowSynchronousIO` etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="fceba-837">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="fceba-838">Aşağıdaki örnek, zaman uyumlu GÇ 'yi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="fceba-838">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="fceba-839">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="fceba-839">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="fceba-840">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-840">Endpoint configuration</span></span>

<span data-ttu-id="fceba-841">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="fceba-841">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="fceba-842">`https://localhost:5001` (yerel bir geliştirme sertifikası mevcut olduğunda)</span><span class="sxs-lookup"><span data-stu-id="fceba-842">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="fceba-843">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="fceba-843">Specify URLs using the:</span></span>

* <span data-ttu-id="fceba-844">ortam değişkeni `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="fceba-844">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="fceba-845">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="fceba-845">`--urls` command-line argument.</span></span>
* <span data-ttu-id="fceba-846">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="fceba-846">`urls` host configuration key.</span></span>
* <span data-ttu-id="fceba-847">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fceba-847">`UseUrls` extension method.</span></span>

<span data-ttu-id="fceba-848">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fceba-848">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="fceba-849">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="fceba-849">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="fceba-850">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-850">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="fceba-851">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="fceba-851">A development certificate is created:</span></span>

* <span data-ttu-id="fceba-852">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="fceba-852">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="fceba-853">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-853">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="fceba-854">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-854">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="fceba-855">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="fceba-855">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="fceba-856">Kestrel için URL öneklerini ve bağlantı noktalarını yapılandırmak üzere <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-856">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="fceba-857">`UseUrls`, `--urls` komut satırı bağımsız değişkeni, `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (HTTPS uç noktası yapılandırması için varsayılan sertifika kullanılabilir olmalıdır).</span><span class="sxs-lookup"><span data-stu-id="fceba-857">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="fceba-858">`KestrelServerOptions` yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="fceba-858">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="fceba-859">ConfigureEndpointDefaults Varsayılanları (eylem\<ListenOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-859">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="fceba-860">Belirtilen her bitiş noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-860">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="fceba-861">`ConfigureEndpointDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-861">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="fceba-862"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-862">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults*> won't have the defaults applied.</span></span>

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="fceba-863">ConfigureHttpsDefaults (eylem\<HttpsConnectionAdapterOptions >)</span><span class="sxs-lookup"><span data-stu-id="fceba-863">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="fceba-864">Her HTTPS uç noktası için çalıştırılacak bir yapılandırma `Action` belirtir.</span><span class="sxs-lookup"><span data-stu-id="fceba-864">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="fceba-865">`ConfigureHttpsDefaults` birden çok kez çağrılması, önceki `Action`s 'in belirtilen son `Action` yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-865">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

> [!NOTE]
> <span data-ttu-id="fceba-866"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> çağrılmadan oluşturulan bitiş noktaları, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> çağrılmadan **önce** , varsayılan olarak uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-866">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="fceba-867">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="fceba-867">Configure(IConfiguration)</span></span>

<span data-ttu-id="fceba-868">Giriş olarak bir <xref:Microsoft.Extensions.Configuration.IConfiguration> alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-868">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="fceba-869">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-869">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="fceba-870">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="fceba-870">ListenOptions.UseHttps</span></span>

<span data-ttu-id="fceba-871">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-871">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="fceba-872">`ListenOptions.UseHttps` uzantıları:</span><span class="sxs-lookup"><span data-stu-id="fceba-872">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="fceba-873">`UseHttps` &ndash; varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-873">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="fceba-874">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fceba-874">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="fceba-875">`ListenOptions.UseHttps` parametreleri:</span><span class="sxs-lookup"><span data-stu-id="fceba-875">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="fceba-876">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-876">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="fceba-877">X. 509.440 sertifika verilerine erişmek için gereken parola `password`.</span><span class="sxs-lookup"><span data-stu-id="fceba-877">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="fceba-878">`configureOptions`, `HttpsConnectionAdapterOptions`yapılandırmak için `Action`.</span><span class="sxs-lookup"><span data-stu-id="fceba-878">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="fceba-879">`ListenOptions`döndürür.</span><span class="sxs-lookup"><span data-stu-id="fceba-879">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="fceba-880">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-880">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="fceba-881">`subject`, sertifikanın konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-881">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="fceba-882">`allowInvalid`, otomatik olarak imzalanan sertifikalar gibi geçersiz sertifikaların dikkate alınıp alınmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fceba-882">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="fceba-883">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="fceba-883">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="fceba-884">`serverCertificate` X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-884">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="fceba-885">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fceba-885">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="fceba-886">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-886">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="fceba-887">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="fceba-887">Supported configurations described next:</span></span>

* <span data-ttu-id="fceba-888">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="fceba-888">No configuration</span></span>
* <span data-ttu-id="fceba-889">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="fceba-889">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="fceba-890">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="fceba-890">Change the defaults in code</span></span>

<span data-ttu-id="fceba-891">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="fceba-891">*No configuration*</span></span>

<span data-ttu-id="fceba-892">Kestrel `http://localhost:5000` ve `https://localhost:5001` dinler (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="fceba-892">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="fceba-893">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="fceba-893">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="fceba-894">Kestrel yapılandırmasını yüklemek için varsayılan olarak `Configure(context.Configuration.GetSection("Kestrel"))` `CreateDefaultBuilder` çağırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-894">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="fceba-895">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-895">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="fceba-896">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-896">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="fceba-897">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="fceba-897">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="fceba-898">Geçersiz sertifikaların (örneğin, otomatik olarak imzalanan sertifikalar) kullanılmasına izin vermek için, **Allowwınvalid** öğesini `true` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fceba-898">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="fceba-899">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**Httpsdefaultcert** ), **varsayılan** veya geliştirme sertifikası > **Sertifikalar** altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="fceba-899">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="fceba-900">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="fceba-900">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="fceba-901">Örneğin, **sertifikalar** > **varsayılan** sertifika şöyle belirlenebilir:</span><span class="sxs-lookup"><span data-stu-id="fceba-901">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="fceba-902">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="fceba-902">Schema notes:</span></span>

* <span data-ttu-id="fceba-903">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-903">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="fceba-904">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-904">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="fceba-905">Her uç nokta için `Url` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-905">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="fceba-906">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-906">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="fceba-907">Bu uç noktalar, üst düzey `Urls` yapılandırmasında tanımlananlara eklemek yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-907">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="fceba-908">`Listen` aracılığıyla kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-908">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="fceba-909">`Certificate` bölümü isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-909">The `Certificate` section is optional.</span></span> <span data-ttu-id="fceba-910">`Certificate` bölümü belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-910">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="fceba-911">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-911">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="fceba-912">`Certificate` bölümü, hem **yol**&ndash;**parolasını** hem de **Konu**&ndash;**Mağaza** sertifikalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-912">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="fceba-913">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-913">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="fceba-914">`options.Configure(context.Configuration.GetSection("{SECTION}"))`, yapılandırılmış bir uç noktanın ayarlarını tamamlamak için kullanılabilecek `.Endpoint(string name, listenOptions => { })` yöntemi ile bir `KestrelConfigurationLoader` döndürür:</span><span class="sxs-lookup"><span data-stu-id="fceba-914">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="fceba-915">`KestrelServerOptions.ConfigurationLoader`, <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından sağlana gibi var olan yükleyicisindeki yinelenmeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-915">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="fceba-916">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-916">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="fceba-917">`options.Configure(context.Configuration.GetSection("{SECTION}"))` başka bir bölümle yeniden çağırarak birden çok yapılandırma yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-917">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="fceba-918">`Load` önceki örneklerde açıkça çağrılmamışsa yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-918">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="fceba-919">Metapackage, varsayılan yapılandırma bölümünün değiştirilebilmesi için `Load` çağırmaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-919">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="fceba-920">`KestrelConfigurationLoader`, API 'lerin `Listen` ailesini `KestrelServerOptions` `Endpoint` aşırı yükleme olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-920">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="fceba-921">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-921">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="fceba-922">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="fceba-922">*Change the defaults in code*</span></span>

<span data-ttu-id="fceba-923">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults`, önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma da dahil olmak üzere `ListenOptions` ve `HttpsConnectionAdapterOptions`için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-923">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="fceba-924">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-924">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="fceba-925">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="fceba-925">*Kestrel support for SNI*</span></span>

<span data-ttu-id="fceba-926">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-926">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="fceba-927">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="fceba-927">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="fceba-928">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-928">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="fceba-929">Kestrel `ServerCertificateSelector` geri çağırma aracılığıyla SNı destekler.</span><span class="sxs-lookup"><span data-stu-id="fceba-929">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="fceba-930">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fceba-930">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="fceba-931">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="fceba-931">SNI support requires:</span></span>

* <span data-ttu-id="fceba-932">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="fceba-932">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="fceba-933">`net461` veya sonraki sürümlerde, geri çağırma çağrılır, ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="fceba-933">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="fceba-934">Ayrıca, istemci, TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa da `null` `name`.</span><span class="sxs-lookup"><span data-stu-id="fceba-934">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="fceba-935">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-935">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="fceba-936">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-936">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="fceba-937">Bağlantı günlüğü</span><span class="sxs-lookup"><span data-stu-id="fceba-937">Connection logging</span></span>

<span data-ttu-id="fceba-938">Bir bağlantıda bayt düzeyinde iletişim için hata ayıklama düzeyi günlüklerini yayma <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="fceba-938">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="fceba-939">Bağlantı günlüğü, TLS şifreleme ve proxy 'nin arkasındaki gibi alt düzey iletişimde sorunları gidermeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-939">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="fceba-940">`UseConnectionLogging` `UseHttps`önce yerleştirilmişse, şifrelenmiş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-940">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="fceba-941">`UseConnectionLogging` `UseHttps`sonra yerleştirilmişse, şifresi çözülmüş trafik günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-941">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="fceba-942">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-942">Bind to a TCP socket</span></span>

<span data-ttu-id="fceba-943"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="fceba-943">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="fceba-944">Örnek, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>olan bir uç nokta için HTTPS 'yi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="fceba-944">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="fceba-945">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-945">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="fceba-946">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="fceba-946">Bind to a Unix socket</span></span>

<span data-ttu-id="fceba-947">Aşağıdaki örnekte gösterildiği gibi NGINX ile daha iyi performans için <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-947">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="fceba-948">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="fceba-948">Port 0</span></span>

<span data-ttu-id="fceba-949">`0` bağlantı noktası numarası belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-949">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fceba-950">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fceba-950">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="fceba-951">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="fceba-951">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="fceba-952">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="fceba-952">Limitations</span></span>

<span data-ttu-id="fceba-953">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="fceba-953">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="fceba-954">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-954">`--urls` command-line argument</span></span>
* <span data-ttu-id="fceba-955">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="fceba-955">`urls` host configuration key</span></span>
* <span data-ttu-id="fceba-956">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="fceba-956">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="fceba-957">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-957">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="fceba-958">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="fceba-958">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="fceba-959">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece (örneğin, bu konuda daha önce gösterildiği gibi `KestrelServerOptions` yapılandırması veya yapılandırma dosyası kullanılarak), HTTPS bu yaklaşımlar ile kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-959">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="fceba-960">`Listen` ve `UseUrls` yaklaşımlarının her ikisi de aynı anda kullanıldığında `Listen` uç noktaları `UseUrls` uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fceba-960">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="fceba-961">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-961">IIS endpoint configuration</span></span>

<span data-ttu-id="fceba-962">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları `Listen` veya `UseUrls`tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-962">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fceba-963">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="fceba-963">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="fceba-964">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="fceba-964">Transport configuration</span></span>

<span data-ttu-id="fceba-965">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="fceba-965">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="fceba-966">Bu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> çağıran ve aşağıdaki paketlerden birine bağlı olan 2,1 ' ye yükselten ASP.NET Core 2,0 uygulamaları için bir son değişiklik değildir:</span><span class="sxs-lookup"><span data-stu-id="fceba-966">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="fceba-967">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="fceba-967">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="fceba-968">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="fceba-968">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="fceba-969">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="fceba-969">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="fceba-970">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fceba-970">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="fceba-971"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="fceba-971">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="fceba-972">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="fceba-972">URL prefixes</span></span>

<span data-ttu-id="fceba-973">`UseUrls`, `--urls` komut satırı bağımsız değişkenini, `urls` ana bilgisayar yapılandırma anahtarını veya `ASPNETCORE_URLS` ortam değişkenini kullanırken, URL önekleri aşağıdaki biçimlerden birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="fceba-973">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="fceba-974">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fceba-974">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="fceba-975">Kestrel, `UseUrls`kullanılarak URL bağlamaları yapılandırılırken HTTPS 'YI desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fceba-975">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="fceba-976">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-976">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fceba-977">`0.0.0.0`, tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-977">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="fceba-978">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="fceba-978">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="fceba-979">`[::]`, IPv4 `0.0.0.0`IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="fceba-979">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="fceba-980">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-980">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fceba-981">Ana bilgisayar adları, `*`ve `+`özel değildir.</span><span class="sxs-lookup"><span data-stu-id="fceba-981">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="fceba-982">Geçerli bir IP adresi olarak tanınmayan bir şey veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-982">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fceba-983">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-983">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="fceba-984">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fceba-984">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="fceba-985">Port numarası veya bağlantı noktası numarası ile geri döngü IP 'si `localhost` ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="fceba-985">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fceba-986">`localhost` belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="fceba-986">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fceba-987">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="fceba-987">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fceba-988">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="fceba-988">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="fceba-989">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="fceba-989">Host filtering</span></span>

<span data-ttu-id="fceba-990">Kestrel, `http://example.com:5000`gibi önekleri temel alarak yapılandırmayı desteklese de, büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="fceba-990">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="fceba-991">Ana bilgisayar `localhost`, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="fceba-991">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="fceba-992">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-992">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="fceba-993">`Host` üstbilgileri doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="fceba-993">`Host` headers aren't validated.</span></span>

<span data-ttu-id="fceba-994">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fceba-994">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="fceba-995">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fceba-995">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="fceba-996">Ara yazılım, <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>çağıran <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>tarafından eklenir:</span><span class="sxs-lookup"><span data-stu-id="fceba-996">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="fceba-997">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fceba-997">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="fceba-998">Ara yazılımı etkinleştirmek için *appSettings. json*/appSettings içinde bir `AllowedHosts` anahtarı tanımlayın *.\<environmentname >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="fceba-998">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="fceba-999">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="fceba-999">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="fceba-1000">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="fceba-1000">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="fceba-1001">[Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) ayrıca bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği de vardır.</span><span class="sxs-lookup"><span data-stu-id="fceba-1001">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="fceba-1002">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fceba-1002">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="fceba-1003">Iletilen üst bilgi ara sunucusu ile `AllowedHosts` ayarlama, istekleri bir ters ara sunucu veya yük dengeleyici ile iletirken, `Host` üst bilgisi korunurken uygun olur.</span><span class="sxs-lookup"><span data-stu-id="fceba-1003">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="fceba-1004">Kestrel genel kullanıma açık bir uç sunucu olarak veya `Host` üstbilgisi doğrudan iletildiğinde, ana bilgisayar filtreleme ara yazılımı ile `AllowedHosts` ayarlama uygundur.</span><span class="sxs-lookup"><span data-stu-id="fceba-1004">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="fceba-1005">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fceba-1005">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fceba-1006">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fceba-1006">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="fceba-1007">RFC 7230: Ileti sözdizimi ve yönlendirme (Bölüm 5,4: Ana bilgisayar)</span><span class="sxs-lookup"><span data-stu-id="fceba-1007">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
