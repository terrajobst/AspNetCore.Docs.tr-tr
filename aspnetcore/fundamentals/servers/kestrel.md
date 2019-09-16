---
title: ASP.NET Core Web sunucusu uygulamasını Kestrel
author: guardrex
description: ASP.NET Core platformlar arası Web sunucusu olan Kestrel hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011172"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="025cc-103">ASP.NET Core Web sunucusu uygulamasını Kestrel</span><span class="sxs-lookup"><span data-stu-id="025cc-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="025cc-104">[Tom Dykstra](https://github.com/tdykstra), [Chris](https://github.com/Tratcher)No, [Stephen halter](https://twitter.com/halter73)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="025cc-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="025cc-105">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="025cc-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="025cc-106">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="025cc-107">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="025cc-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="025cc-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="025cc-108">HTTPS</span></span>
* <span data-ttu-id="025cc-109">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="025cc-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="025cc-110">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="025cc-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="025cc-111">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="025cc-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="025cc-112">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="025cc-113">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="025cc-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="025cc-114">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="025cc-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="025cc-115">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="025cc-115">HTTP/2 support</span></span>

<span data-ttu-id="025cc-116">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="025cc-117">İşletim Sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="025cc-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="025cc-118">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="025cc-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="025cc-119">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="025cc-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="025cc-120">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="025cc-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="025cc-121">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="025cc-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="025cc-122">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="025cc-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="025cc-123">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="025cc-124">&Dagger;Kestrel, Windows Server 2012 R2 ve Windows 8.1 'de HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="025cc-125">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="025cc-126">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="025cc-127">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="025cc-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="025cc-128">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="025cc-129">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="025cc-130">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="025cc-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="025cc-131">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="025cc-132">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="025cc-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="025cc-133">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="025cc-135">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="025cc-137">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="025cc-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="025cc-138">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="025cc-139">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel isteklerin `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="025cc-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="025cc-140">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="025cc-141">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="025cc-142">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="025cc-142">A reverse proxy:</span></span>

* <span data-ttu-id="025cc-143">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="025cc-144">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="025cc-145">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="025cc-146">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="025cc-147">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-148">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="025cc-149">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="025cc-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="025cc-150">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="025cc-151">*Program.cs*' de, şablon kodu çağırır <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>ve bu da <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> arka planda çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="025cc-152">`CreateDefaultBuilder` `ConfigureKestrel`Ve çağrıldıktansonraekyapılandırmasağlamakiçinşunukullanın`ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="025cc-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

<span data-ttu-id="025cc-153">Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, çağrılmadan `ConfigureKestrel` **önce** çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> :</span><span class="sxs-lookup"><span data-stu-id="025cc-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="025cc-154">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-154">Kestrel options</span></span>

<span data-ttu-id="025cc-155">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="025cc-156"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> Sınıfının özelliğinde<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="025cc-157">Özelliği `Limits` , <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="025cc-158">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="025cc-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="025cc-159">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="025cc-160">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="025cc-161">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="025cc-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="025cc-162">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="025cc-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="025cc-163">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="025cc-164">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="025cc-165">Bir bağlantı yükseltildikten sonra, bu `MaxConcurrentConnections` sınıra göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="025cc-166">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="025cc-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="025cc-167">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="025cc-168">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="025cc-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="025cc-169">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="025cc-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="025cc-170">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="025cc-171">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="025cc-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="025cc-172">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="025cc-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="025cc-173">Özelliğin salt okuma `IsReadOnly` durumunda olup olmadığını belirten bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir. `MaxRequestBodySize`</span><span class="sxs-lookup"><span data-stu-id="025cc-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="025cc-174">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="025cc-175">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="025cc-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="025cc-176">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="025cc-177">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="025cc-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="025cc-178">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="025cc-179">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="025cc-180">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="025cc-181">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında olduğu gibi aynı `RequestBody` `Response` olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="025cc-182">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="025cc-183">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="025cc-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="025cc-184">Önceki <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> örnekte başvurulan, http/2 isteklerinde hız sınırlarını `HttpContext.Features` değiştirmek, protokolün istek çoğullama desteği nedeniyle http/2 için genel olarak desteklenmediği için, http/2 istekleri için ' de mevcut değildir.</span><span class="sxs-lookup"><span data-stu-id="025cc-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="025cc-185">`null` `HttpContext.Features` `IHttpMinRequestBodyDataRateFeature.MinDataRate` Ancak, http/2 istekleri için yine de vardır, çünkü okuma hızı sınırı, bir http/2 isteği için de olarak ayarlanarak tamamen istek temelli olarak devre dışı bırakılabilir. <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature></span><span class="sxs-lookup"><span data-stu-id="025cc-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="025cc-186">Bunun dışında bir değere `null` ayarlamaya çalışılması `NotSupportedException` veyabirhttp/2isteğiverilmeye`IHttpMinRequestBodyDataRateFeature.MinDataRate` neden olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="025cc-187">Http/1. x ve http `KestrelServerOptions.Limits` /2 bağlantılarına hala uygulanan sunucu genelindeki hız sınırları.</span><span class="sxs-lookup"><span data-stu-id="025cc-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="025cc-188">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="025cc-189">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="025cc-190">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="025cc-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="025cc-191">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="025cc-191">Maximum streams per connection</span></span>

<span data-ttu-id="025cc-192">`Http2.MaxStreamsPerConnection`HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="025cc-193">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="025cc-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="025cc-194">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="025cc-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="025cc-195">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-195">Header table size</span></span>

<span data-ttu-id="025cc-196">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="025cc-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="025cc-197">`Http2.HeaderTableSize`HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="025cc-198">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="025cc-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="025cc-199">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="025cc-200">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-200">Maximum frame size</span></span>

<span data-ttu-id="025cc-201">`Http2.MaxFrameSize`sunucu tarafından alınan veya gönderilen HTTP/2 bağlantı çerçevesi yükünün izin verilen en büyük boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="025cc-202">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="025cc-203">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="025cc-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="025cc-204">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-204">Maximum request header size</span></span>

<span data-ttu-id="025cc-205">`Http2.MaxRequestHeaderFieldSize`istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="025cc-206">Bu sınır, sıkıştırılmış ve sıkıştırılmamış temsillerinde hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="025cc-207">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="025cc-208">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="025cc-209">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-209">Initial connection window size</span></span>

<span data-ttu-id="025cc-210">`Http2.InitialConnectionWindowSize`sunucu, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="025cc-211">İstekleri ile `Http2.InitialStreamWindowSize`de sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="025cc-212">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="025cc-213">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="025cc-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="025cc-214">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-214">Initial stream window size</span></span>

<span data-ttu-id="025cc-215">`Http2.InitialStreamWindowSize`sunucu, istek başına bir kez (Stream) arabelleğe alınan en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="025cc-216">İstekleri ile `Http2.InitialConnectionWindowSize`de sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="025cc-217">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="025cc-218">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="025cc-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="025cc-219">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="025cc-219">Synchronous IO</span></span>

<span data-ttu-id="025cc-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="025cc-221">Varsayılan değer `false` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="025cc-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-222">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="025cc-223">Yalnızca zaman `AllowSynchronousIO` uyumsuz GÇ desteklemeyen bir kitaplık kullanılırken etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="025cc-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="025cc-224">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="025cc-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="025cc-225">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="025cc-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="025cc-226">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-226">Endpoint configuration</span></span>

<span data-ttu-id="025cc-227">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="025cc-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="025cc-228">`https://localhost:5001`(bir yerel geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="025cc-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="025cc-229">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="025cc-229">Specify URLs using the:</span></span>

* <span data-ttu-id="025cc-230">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="025cc-231">`--urls`komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="025cc-232">`urls`Ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="025cc-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="025cc-233">`UseUrls`genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="025cc-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="025cc-234">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="025cc-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="025cc-235">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="025cc-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="025cc-236">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="025cc-237">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="025cc-237">A development certificate is created:</span></span>

* <span data-ttu-id="025cc-238">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="025cc-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="025cc-239">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="025cc-240">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="025cc-241">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="025cc-242">URL <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> öneklerini <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ve bağlantı <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> noktalarını Kestrel için yapılandırmak üzere üzerinde çağrı veya Yöntemler.</span><span class="sxs-lookup"><span data-stu-id="025cc-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="025cc-243">`UseUrls`, komut satırı bağımsız değişkeni, `urls` ana bilgisayar `ASPNETCORE_URLS` yapılandırma anahtarı ve ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (https uç noktası için varsayılan sertifika kullanılabilir olmalıdır `--urls` yapılandırma).</span><span class="sxs-lookup"><span data-stu-id="025cc-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="025cc-244">`KestrelServerOptions`yapılandırmada</span><span class="sxs-lookup"><span data-stu-id="025cc-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="025cc-245">Configureendpointdefaults Varsayılanları (\<eylem listenoptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="025cc-246">Belirtilen her bir `Action` uç nokta için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="025cc-247">Birden `ConfigureEndpointDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="025cc-248">Configurehttpsdefaults (ACTION\<httpsconnectionadapteroptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="025cc-249">Her HTTPS uç `Action` noktası için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="025cc-250">Birden `ConfigureHttpsDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="025cc-251">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="025cc-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="025cc-252">Bir <xref:Microsoft.Extensions.Configuration.IConfiguration> as girişi alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="025cc-253">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="025cc-254">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="025cc-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="025cc-255">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="025cc-256">`ListenOptions.UseHttps`uzantılardan</span><span class="sxs-lookup"><span data-stu-id="025cc-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="025cc-257">`UseHttps`&ndash; Kestrel 'i varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="025cc-258">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-258">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="025cc-259">`ListenOptions.UseHttps`parametrelere</span><span class="sxs-lookup"><span data-stu-id="025cc-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="025cc-260">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="025cc-261">`password`X. 509.440 sertifika verilerine erişmek için parola gereklidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="025cc-262">`configureOptions``Action` ,`HttpsConnectionAdapterOptions`öğesini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="025cc-263">`ListenOptions`Döndürür.</span><span class="sxs-lookup"><span data-stu-id="025cc-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="025cc-264">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="025cc-265">`subject`, sertifika için konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="025cc-266">`allowInvalid`geçersiz sertifikaların, otomatik olarak imzalanan sertifikalar gibi göz önünde bulundurulmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="025cc-267">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="025cc-268">`serverCertificate`, X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="025cc-269">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="025cc-270">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="025cc-271">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="025cc-271">Supported configurations described next:</span></span>

* <span data-ttu-id="025cc-272">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="025cc-272">No configuration</span></span>
* <span data-ttu-id="025cc-273">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="025cc-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="025cc-274">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="025cc-274">Change the defaults in code</span></span>

<span data-ttu-id="025cc-275">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="025cc-275">*No configuration*</span></span>

<span data-ttu-id="025cc-276">Kestrel tarihinde `http://localhost:5000` dinler ve `https://localhost:5001` (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="025cc-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="025cc-277">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="025cc-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="025cc-278">`CreateDefaultBuilder`Kestrel `Configure(context.Configuration.GetSection("Kestrel"))` yapılandırmasını yüklemek için varsayılan olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="025cc-279">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="025cc-280">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="025cc-281">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="025cc-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="025cc-282">Geçersiz sertifikaların kullanılmasına izin vermek `true` için **allowwınvalid** ' i ayarlayın (örneğin, otomatik olarak imzalanan sertifikalar).</span><span class="sxs-lookup"><span data-stu-id="025cc-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="025cc-283">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="025cc-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="025cc-284">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="025cc-285">Örneğin, **sertifika** > **varsayılan** sertifikası şu şekilde belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="025cc-286">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="025cc-286">Schema notes:</span></span>

* <span data-ttu-id="025cc-287">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="025cc-288">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="025cc-289">Her uç nokta için parametresigereklidir.`Url`</span><span class="sxs-lookup"><span data-stu-id="025cc-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="025cc-290">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="025cc-291">Bu uç noktalar, üst düzey `Urls` yapılandırmada tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="025cc-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="025cc-292">Kullanılarak `Listen` kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="025cc-293">Bu `Certificate` bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="025cc-294">`Certificate` Bölüm belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="025cc-295">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="025cc-296">&ndash;&ndash; Bu bölüm hem yol parolasını hem de konu deposu sertifikalarını destekler. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="025cc-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="025cc-297">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="025cc-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))`yapılandırılmış bir `KestrelConfigurationLoader` uç noktanın `.Endpoint(string name, listenOptions => { })` ayarlarını tamamlamak için kullanılabilecek bir yöntemi olan bir döndürür:</span><span class="sxs-lookup"><span data-stu-id="025cc-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="025cc-299">`KestrelServerOptions.ConfigurationLoader`, tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>sağlana gibi var olan yükleyicisindeki yinelemeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="025cc-300">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="025cc-301">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` yeniden çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="025cc-302">Önceki örneklerde açıkça çağrılmadığı takdirde `Load` , yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="025cc-303">Metapackage, varsayılan yapılandırma `Load` bölümünün değiştirilmesini sağlayacak şekilde çağırmıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="025cc-304">`KestrelConfigurationLoader`API ailesini aşırı yükleme `Endpoint` olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. `KestrelServerOptions` `Listen`</span><span class="sxs-lookup"><span data-stu-id="025cc-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="025cc-305">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="025cc-306">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="025cc-306">*Change the defaults in code*</span></span>

<span data-ttu-id="025cc-307">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` , önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma `ListenOptions` dahil `HttpsConnectionAdapterOptions`, ve için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="025cc-308">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
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
        });
```

<span data-ttu-id="025cc-309">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="025cc-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="025cc-310">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="025cc-311">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="025cc-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="025cc-312">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="025cc-313">Kestrel, `ServerCertificateSelector` geri çağırma aracılığıyla SNI destekler.</span><span class="sxs-lookup"><span data-stu-id="025cc-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="025cc-314">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="025cc-315">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="025cc-315">SNI support requires:</span></span>

* <span data-ttu-id="025cc-316">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="025cc-317">Veya sonraki sürümlerde, geri çağırma çağrılır, `name` ancak her zaman `null`olur. `net461`</span><span class="sxs-lookup"><span data-stu-id="025cc-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="025cc-318">Ayrıca `name` , istemci `null` , TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="025cc-319">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="025cc-320">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
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
            })
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="025cc-321">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-321">Bind to a TCP socket</span></span>

<span data-ttu-id="025cc-322"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="025cc-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="025cc-323">Örnek, HTTPS 'yi ile <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>bir uç nokta için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="025cc-324">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="025cc-325">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-325">Bind to a Unix socket</span></span>

<span data-ttu-id="025cc-326">Aşağıdaki örnekte gösterildiği gibi NGINX ile gelişmiş performans için ile birlikte <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="025cc-327">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="025cc-327">Port 0</span></span>

<span data-ttu-id="025cc-328">Bağlantı noktası numarası `0` belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="025cc-329">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="025cc-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="025cc-330">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="025cc-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="025cc-331">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="025cc-331">Limitations</span></span>

<span data-ttu-id="025cc-332">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="025cc-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="025cc-333">`--urls`komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="025cc-334">`urls`Ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="025cc-334">`urls` host configuration key</span></span>
* <span data-ttu-id="025cc-335">`ASPNETCORE_URLS`ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="025cc-336">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="025cc-337">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="025cc-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="025cc-338">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece https bu yaklaşımlar ile kullanılamaz (örneğin, bu konunun önceki kısımlarında `KestrelServerOptions` gösterildiği gibi yapılandırma veya yapılandırma dosyası kullanılıyor).</span><span class="sxs-lookup"><span data-stu-id="025cc-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="025cc-339">Hem `Listen` hem `UseUrls` de yaklaşımları aynı anda kullanıldığında `UseUrls` , `Listen` uç noktalar uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="025cc-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="025cc-340">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-340">IIS endpoint configuration</span></span>

<span data-ttu-id="025cc-341">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları veya `Listen` `UseUrls`ya da tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="025cc-342">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="025cc-343">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="025cc-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="025cc-344">Özelliği, bir bağlantı uç noktasında veya`HttpProtocols`sunucu için etkin HTTP protokollerini () belirler. `Protocols`</span><span class="sxs-lookup"><span data-stu-id="025cc-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="025cc-345">Sabit listesinden özelliğe`Protocols` bir değer atayın. `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="025cc-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="025cc-346">`HttpProtocols`sabit listesi değeri</span><span class="sxs-lookup"><span data-stu-id="025cc-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="025cc-347">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="025cc-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="025cc-348">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="025cc-348">HTTP/1.1 only.</span></span> <span data-ttu-id="025cc-349">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="025cc-350">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="025cc-350">HTTP/2 only.</span></span> <span data-ttu-id="025cc-351">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="025cc-352">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="025cc-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="025cc-353">HTTP/2, istemcinin TLS [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) el sıkışmasında http/2 seçmesini gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="025cc-354">Herhangi bir `ListenOptions.Protocols` `HttpProtocols.Http1AndHttp2`uç nokta için varsayılan değer.</span><span class="sxs-lookup"><span data-stu-id="025cc-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="025cc-355">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="025cc-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="025cc-356">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="025cc-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="025cc-357">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="025cc-357">Renegotiation disabled</span></span>
* <span data-ttu-id="025cc-358">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="025cc-358">Compression disabled</span></span>
* <span data-ttu-id="025cc-359">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="025cc-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="025cc-360">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="025cc-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="025cc-361">Sınırlı alan Diffie-Hellman (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="025cc-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="025cc-362">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="025cc-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="025cc-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`P-256eliptik&lbrack; eğrisi Varsayılanolarak&lbrack;desteklenir. `FIPS186` &rbrack; `TLS-ECDHE` &rbrack;</span><span class="sxs-lookup"><span data-stu-id="025cc-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="025cc-364">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="025cc-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="025cc-365">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="025cc-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="025cc-366">İsteğe bağlı olarak, belirli şifrelemeler için bağlantı temelinde TLS el sıkışmaları filtrelemek için bağlantı ara yazılımını kullanın:</span><span class="sxs-lookup"><span data-stu-id="025cc-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
        // wish to support. Alternatively, define and compare
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="025cc-367">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="025cc-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="025cc-368">`CreateDefaultBuilder`Kestrel `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` yapılandırmasını yüklemek için varsayılan olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="025cc-369">Aşağıdaki *appSettings. JSON* örneği, tüm uç noktalar için varsayılan bağlantı protokolü olarak http/1.1 oluşturur:</span><span class="sxs-lookup"><span data-stu-id="025cc-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="025cc-370">Aşağıdaki *appSettings. JSON* örneği, belirli bir uç nokta için http/1.1 bağlantı protokolünü belirler:</span><span class="sxs-lookup"><span data-stu-id="025cc-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="025cc-371">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="025cc-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="025cc-372">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-372">Transport configuration</span></span>

<span data-ttu-id="025cc-373">Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>) kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="025cc-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="025cc-374">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="025cc-375">Şunu çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>: `IWebHostBuilder`</span><span class="sxs-lookup"><span data-stu-id="025cc-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="025cc-376">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-376">URL prefixes</span></span>

<span data-ttu-id="025cc-377">, `UseUrls` `urls` `ASPNETCORE_URLS` Komut satırı bağımsız değişkeni, ana bilgisayar yapılandırma anahtarı veya ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir. `--urls`</span><span class="sxs-lookup"><span data-stu-id="025cc-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="025cc-378">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="025cc-379">Kestrel, kullanılarak `UseUrls`URL bağlamaları yapılandırılırken https 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="025cc-380">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="025cc-381">`0.0.0.0`Tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="025cc-382">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="025cc-383">`[::]`, IPv4 `0.0.0.0`'un IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="025cc-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="025cc-384">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="025cc-385">, Ve `*` `+`ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="025cc-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="025cc-386">Geçerli bir IP adresi olarak tanınmayan veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="025cc-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="025cc-387">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="025cc-388">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="025cc-389">Port `localhost` numarası veya bağlantı noktası numarası ile geri döngü IP 'si olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="025cc-390">`localhost` Belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="025cc-391">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="025cc-392">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="025cc-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="025cc-393">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="025cc-393">Host filtering</span></span>

<span data-ttu-id="025cc-394">Kestrel gibi önekleri `http://example.com:5000`temel alarak yapılandırmayı desteklese de, Kestrel büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="025cc-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="025cc-395">Ana `localhost` bilgisayar, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="025cc-396">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="025cc-397">`Host`Üstbilgiler doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="025cc-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="025cc-398">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="025cc-399">Ana bilgisayar filtreleme ara yazılımı, ASP.NET Core uygulamaları için örtük olarak sunulan [Microsoft. AspNetCore. HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="025cc-400">Ara yazılım tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>eklenir ve şunları çağırır <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="025cc-401">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="025cc-402">Ara yazılımı etkinleştirmek için `AllowedHosts` *appSettings. JSON*/appSettings içinde bir anahtar tanımlayın *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="025cc-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="025cc-403">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="025cc-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="025cc-404">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="025cc-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="025cc-405">[İletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) da bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçenek içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="025cc-406">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="025cc-407">İletilen `AllowedHosts` üstbilgiler ara yazılımı ile, istekler ters bir `Host` ara sunucu veya yük dengeleyici ile iletilirken üst bilgi korunurken, bu işlem için uygun bir ayar vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="025cc-408">Ana `AllowedHosts` bilgisayar filtreleme ara yazılımı ile ayarlama, Kestrel herkese açık bir uç sunucu olarak veya `Host` üst bilgi doğrudan iletildiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="025cc-409">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için <xref:host-and-deploy/proxy-load-balancer>bkz.</span><span class="sxs-lookup"><span data-stu-id="025cc-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="025cc-410">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="025cc-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="025cc-411">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="025cc-412">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="025cc-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="025cc-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="025cc-413">HTTPS</span></span>
* <span data-ttu-id="025cc-414">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="025cc-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="025cc-415">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="025cc-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="025cc-416">HTTP/2 (macOS&dagger;hariç)</span><span class="sxs-lookup"><span data-stu-id="025cc-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="025cc-417">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="025cc-418">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="025cc-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="025cc-419">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="025cc-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="025cc-420">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="025cc-420">HTTP/2 support</span></span>

<span data-ttu-id="025cc-421">Aşağıdaki temel gereksinimler karşılanıyorsa, [http/2](https://httpwg.org/specs/rfc7540.html) ASP.NET Core uygulamalar için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="025cc-422">İşletim Sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="025cc-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="025cc-423">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="025cc-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="025cc-424">OpenSSL 1.0.2 veya üzerini içeren Linux (örneğin, Ubuntu 16,04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="025cc-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="025cc-425">Hedef Framework: .NET Core 2,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="025cc-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="025cc-426">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı</span><span class="sxs-lookup"><span data-stu-id="025cc-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="025cc-427">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="025cc-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="025cc-428">&dagger;HTTP/2, gelecek sürümlerde macOS 'ta desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="025cc-429">&Dagger;Kestrel, Windows Server 2012 R2 ve Windows 8.1 'de HTTP/2 için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="025cc-430">Bu işletim sistemlerinde kullanılabilir olan desteklenen TLS şifre paketlerinin listesi sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="025cc-431">TLS bağlantılarının güvenliğini sağlamak için Eliptik Eğri dijital Imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="025cc-432">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="025cc-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="025cc-433">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="025cc-434">Yapılandırma hakkında daha fazla bilgi için [Kestrel Options](#kestrel-options) ve [Listenoptions. Protocols](#listenoptionsprotocols) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="025cc-435">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="025cc-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="025cc-436">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="025cc-437">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="025cc-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="025cc-438">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="025cc-440">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="025cc-442">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="025cc-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="025cc-443">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="025cc-444">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel isteklerin `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="025cc-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="025cc-445">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="025cc-446">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="025cc-447">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="025cc-447">A reverse proxy:</span></span>

* <span data-ttu-id="025cc-448">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="025cc-449">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="025cc-450">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="025cc-451">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="025cc-452">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-453">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="025cc-454">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="025cc-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="025cc-455">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="025cc-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="025cc-456">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="025cc-457">*Program.cs*' de, şablon kodu çağırır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>ve bu da <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> arka planda çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="025cc-458">Çağrıldıktan `CreateDefaultBuilder`sonra ek yapılandırma sağlamak için şunu kullanın `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="025cc-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="025cc-459">Uygulama, Konağı kurmak için `CreateDefaultBuilder` çağırmazsa, çağrılmadan `ConfigureKestrel` **önce** çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> :</span><span class="sxs-lookup"><span data-stu-id="025cc-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="025cc-460">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-460">Kestrel options</span></span>

<span data-ttu-id="025cc-461">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="025cc-462"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> Sınıfının özelliğinde<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="025cc-463">Özelliği `Limits` , <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="025cc-464">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="025cc-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="025cc-465">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="025cc-466">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="025cc-467">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="025cc-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="025cc-468">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="025cc-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="025cc-469">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="025cc-470">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="025cc-471">Bir bağlantı yükseltildikten sonra, bu `MaxConcurrentConnections` sınıra göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="025cc-472">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="025cc-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="025cc-473">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="025cc-474">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="025cc-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="025cc-475">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="025cc-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="025cc-476">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="025cc-477">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="025cc-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="025cc-478">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="025cc-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="025cc-479">Özelliğin salt okuma `IsReadOnly` durumunda olup olmadığını belirten bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir. `MaxRequestBodySize`</span><span class="sxs-lookup"><span data-stu-id="025cc-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="025cc-480">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="025cc-481">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="025cc-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="025cc-482">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="025cc-483">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="025cc-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="025cc-484">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="025cc-485">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="025cc-486">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="025cc-487">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında olduğu gibi aynı `RequestBody` `Response` olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="025cc-488">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="025cc-489">Ara yazılım içindeki istek başına düşen minimum hız sınırlarını geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="025cc-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="025cc-490">İstek çoğullama için protokol desteği nedeniyle, http/2 `HttpContext.Features` için bir istek başına hız sınırlarını değiştirmek, http/2 istekleri için, önceki örnekte başvurulan hiçbir oran özelliği mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="025cc-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="025cc-491">Http/1. x ve http `KestrelServerOptions.Limits` /2 bağlantılarına hala uygulanan sunucu genelindeki hız sınırları.</span><span class="sxs-lookup"><span data-stu-id="025cc-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="025cc-492">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="025cc-493">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="025cc-494">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="025cc-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="025cc-495">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="025cc-495">Maximum streams per connection</span></span>

<span data-ttu-id="025cc-496">`Http2.MaxStreamsPerConnection`HTTP/2 bağlantısı başına eşzamanlı istek akışı sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="025cc-497">Fazlalık akışlar reddedildi.</span><span class="sxs-lookup"><span data-stu-id="025cc-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="025cc-498">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="025cc-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="025cc-499">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-499">Header table size</span></span>

<span data-ttu-id="025cc-500">HPACK kod çözücüsü HTTP/2 bağlantıları için HTTP üstbilgilerini açar.</span><span class="sxs-lookup"><span data-stu-id="025cc-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="025cc-501">`Http2.HeaderTableSize`HPACK kod çözücüsünün kullandığı üst bilgi sıkıştırma tablosunun boyutunu sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="025cc-502">Değer sekizli cinsinden sağlanır ve sıfırdan büyük olmalıdır (0).</span><span class="sxs-lookup"><span data-stu-id="025cc-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="025cc-503">Varsayılan değer 4096 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="025cc-504">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-504">Maximum frame size</span></span>

<span data-ttu-id="025cc-505">`Http2.MaxFrameSize`alacak HTTP/2 bağlantı çerçevesi yükünün en büyük boyutunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="025cc-506">Değer sekizli cinsinden sağlanır ve 2 ^ 14 (16.384) ile 2 ^ 24-1 (16.777.215) arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="025cc-507">Varsayılan değer 2 ^ 14 ' dir (16.384).</span><span class="sxs-lookup"><span data-stu-id="025cc-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="025cc-508">En fazla istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-508">Maximum request header size</span></span>

<span data-ttu-id="025cc-509">`Http2.MaxRequestHeaderFieldSize`istek üst bilgisi değerlerinin sekizlisi cinsinden izin verilen en büyük boyutu belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="025cc-510">Bu sınır, hem sıkıştırılmış hem de sıkıştırılmamış temsillerinde birlikte hem ad hem de değer için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="025cc-511">Değer sıfırdan büyük (0) olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="025cc-512">Varsayılan değer 8.192 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="025cc-513">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-513">Initial connection window size</span></span>

<span data-ttu-id="025cc-514">`Http2.InitialConnectionWindowSize`sunucu, bağlantı başına tüm istekler (akışlar) genelinde toplanan tek seferde sunucunun arabelleğe aldığı en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="025cc-515">İstekleri ile `Http2.InitialStreamWindowSize`de sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="025cc-516">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="025cc-517">Varsayılan değer 128 KB 'tır (131.072).</span><span class="sxs-lookup"><span data-stu-id="025cc-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="025cc-518">İlk akış pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-518">Initial stream window size</span></span>

<span data-ttu-id="025cc-519">`Http2.InitialStreamWindowSize`sunucu, istek başına bir kez (Stream) arabelleğe alınan en fazla istek gövde verilerini bayt cinsinden gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="025cc-520">İstekleri ile `Http2.InitialStreamWindowSize`de sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="025cc-521">Değer, 65.535 değerinden büyük veya buna eşit ve 2 ^ 31 (2.147.483.648) değerinden küçük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="025cc-522">Varsayılan değer 96 KB 'tır (98.304).</span><span class="sxs-lookup"><span data-stu-id="025cc-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="025cc-523">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="025cc-523">Synchronous IO</span></span>

<span data-ttu-id="025cc-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="025cc-525">Varsayılan değer `true`.</span><span class="sxs-lookup"><span data-stu-id="025cc-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-526">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="025cc-527">Yalnızca zaman `AllowSynchronousIO` uyumsuz GÇ desteklemeyen bir kitaplık kullanılırken etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="025cc-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="025cc-528">Aşağıdaki örnek, zaman uyumlu GÇ 'yi sunar:</span><span class="sxs-lookup"><span data-stu-id="025cc-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="025cc-529">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="025cc-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="025cc-530">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-530">Endpoint configuration</span></span>

<span data-ttu-id="025cc-531">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="025cc-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="025cc-532">`https://localhost:5001`(bir yerel geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="025cc-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="025cc-533">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="025cc-533">Specify URLs using the:</span></span>

* <span data-ttu-id="025cc-534">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="025cc-535">`--urls`komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="025cc-536">`urls`Ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="025cc-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="025cc-537">`UseUrls`genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="025cc-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="025cc-538">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="025cc-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="025cc-539">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="025cc-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="025cc-540">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="025cc-541">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="025cc-541">A development certificate is created:</span></span>

* <span data-ttu-id="025cc-542">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="025cc-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="025cc-543">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="025cc-544">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="025cc-545">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="025cc-546">URL <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> öneklerini <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ve bağlantı <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> noktalarını Kestrel için yapılandırmak üzere üzerinde çağrı veya Yöntemler.</span><span class="sxs-lookup"><span data-stu-id="025cc-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="025cc-547">`UseUrls`, komut satırı bağımsız değişkeni, `urls` ana bilgisayar `ASPNETCORE_URLS` yapılandırma anahtarı ve ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (https uç noktası için varsayılan sertifika kullanılabilir olmalıdır `--urls` yapılandırma).</span><span class="sxs-lookup"><span data-stu-id="025cc-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="025cc-548">`KestrelServerOptions`yapılandırmada</span><span class="sxs-lookup"><span data-stu-id="025cc-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="025cc-549">Configureendpointdefaults Varsayılanları (\<eylem listenoptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="025cc-550">Belirtilen her bir `Action` uç nokta için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="025cc-551">Birden `ConfigureEndpointDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="025cc-552">Configurehttpsdefaults (ACTION\<httpsconnectionadapteroptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="025cc-553">Her HTTPS uç `Action` noktası için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="025cc-554">Birden `ConfigureHttpsDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="025cc-555">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="025cc-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="025cc-556">Bir <xref:Microsoft.Extensions.Configuration.IConfiguration> as girişi alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="025cc-557">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="025cc-558">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="025cc-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="025cc-559">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="025cc-560">`ListenOptions.UseHttps`uzantılardan</span><span class="sxs-lookup"><span data-stu-id="025cc-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="025cc-561">`UseHttps`&ndash; Kestrel 'i varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="025cc-562">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-562">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="025cc-563">`ListenOptions.UseHttps`parametrelere</span><span class="sxs-lookup"><span data-stu-id="025cc-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="025cc-564">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="025cc-565">`password`X. 509.440 sertifika verilerine erişmek için parola gereklidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="025cc-566">`configureOptions``Action` ,`HttpsConnectionAdapterOptions`öğesini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="025cc-567">`ListenOptions`Döndürür.</span><span class="sxs-lookup"><span data-stu-id="025cc-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="025cc-568">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="025cc-569">`subject`, sertifika için konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="025cc-570">`allowInvalid`geçersiz sertifikaların, otomatik olarak imzalanan sertifikalar gibi göz önünde bulundurulmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="025cc-571">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="025cc-572">`serverCertificate`, X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="025cc-573">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="025cc-574">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="025cc-575">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="025cc-575">Supported configurations described next:</span></span>

* <span data-ttu-id="025cc-576">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="025cc-576">No configuration</span></span>
* <span data-ttu-id="025cc-577">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="025cc-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="025cc-578">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="025cc-578">Change the defaults in code</span></span>

<span data-ttu-id="025cc-579">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="025cc-579">*No configuration*</span></span>

<span data-ttu-id="025cc-580">Kestrel tarihinde `http://localhost:5000` dinler ve `https://localhost:5001` (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="025cc-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="025cc-581">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="025cc-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="025cc-582">`CreateDefaultBuilder`Kestrel `Configure(context.Configuration.GetSection("Kestrel"))` yapılandırmasını yüklemek için varsayılan olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="025cc-583">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="025cc-584">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="025cc-585">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="025cc-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="025cc-586">Geçersiz sertifikaların kullanılmasına izin vermek `true` için **allowwınvalid** ' i ayarlayın (örneğin, otomatik olarak imzalanan sertifikalar).</span><span class="sxs-lookup"><span data-stu-id="025cc-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="025cc-587">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="025cc-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="025cc-588">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="025cc-589">Örneğin, **sertifika** > **varsayılan** sertifikası şu şekilde belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="025cc-590">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="025cc-590">Schema notes:</span></span>

* <span data-ttu-id="025cc-591">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="025cc-592">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="025cc-593">Her uç nokta için parametresigereklidir.`Url`</span><span class="sxs-lookup"><span data-stu-id="025cc-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="025cc-594">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="025cc-595">Bu uç noktalar, üst düzey `Urls` yapılandırmada tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="025cc-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="025cc-596">Kullanılarak `Listen` kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="025cc-597">Bu `Certificate` bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="025cc-598">`Certificate` Bölüm belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="025cc-599">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="025cc-600">&ndash;&ndash; Bu bölüm hem yol parolasını hem de konu deposu sertifikalarını destekler. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="025cc-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="025cc-601">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="025cc-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))`yapılandırılmış bir `KestrelConfigurationLoader` uç noktanın `.Endpoint(string name, listenOptions => { })` ayarlarını tamamlamak için kullanılabilecek bir yöntemi olan bir döndürür:</span><span class="sxs-lookup"><span data-stu-id="025cc-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="025cc-603">`KestrelServerOptions.ConfigurationLoader`, tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>sağlana gibi var olan yükleyicisindeki yinelemeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="025cc-604">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="025cc-605">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` yeniden çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="025cc-606">Önceki örneklerde açıkça çağrılmadığı takdirde `Load` , yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="025cc-607">Metapackage, varsayılan yapılandırma `Load` bölümünün değiştirilmesini sağlayacak şekilde çağırmıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="025cc-608">`KestrelConfigurationLoader`API ailesini aşırı yükleme `Endpoint` olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. `KestrelServerOptions` `Listen`</span><span class="sxs-lookup"><span data-stu-id="025cc-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="025cc-609">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="025cc-610">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="025cc-610">*Change the defaults in code*</span></span>

<span data-ttu-id="025cc-611">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` , önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma `ListenOptions` dahil `HttpsConnectionAdapterOptions`, ve için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="025cc-612">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="025cc-613">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="025cc-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="025cc-614">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="025cc-615">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="025cc-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="025cc-616">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="025cc-617">Kestrel, `ServerCertificateSelector` geri çağırma aracılığıyla SNI destekler.</span><span class="sxs-lookup"><span data-stu-id="025cc-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="025cc-618">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="025cc-619">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="025cc-619">SNI support requires:</span></span>

* <span data-ttu-id="025cc-620">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="025cc-621">Veya sonraki sürümlerde, geri çağırma çağrılır, `name` ancak her zaman `null`olur. `net461`</span><span class="sxs-lookup"><span data-stu-id="025cc-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="025cc-622">Ayrıca `name` , istemci `null` , TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="025cc-623">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="025cc-624">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="025cc-625">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-625">Bind to a TCP socket</span></span>

<span data-ttu-id="025cc-626"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="025cc-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="025cc-627">Örnek, HTTPS 'yi ile <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>bir uç nokta için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="025cc-628">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="025cc-629">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-629">Bind to a Unix socket</span></span>

<span data-ttu-id="025cc-630">Aşağıdaki örnekte gösterildiği gibi NGINX ile gelişmiş performans için ile birlikte <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="025cc-631">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="025cc-631">Port 0</span></span>

<span data-ttu-id="025cc-632">Bağlantı noktası numarası `0` belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="025cc-633">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="025cc-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="025cc-634">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="025cc-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="025cc-635">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="025cc-635">Limitations</span></span>

<span data-ttu-id="025cc-636">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="025cc-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="025cc-637">`--urls`komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="025cc-638">`urls`Ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="025cc-638">`urls` host configuration key</span></span>
* <span data-ttu-id="025cc-639">`ASPNETCORE_URLS`ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="025cc-640">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="025cc-641">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="025cc-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="025cc-642">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece https bu yaklaşımlar ile kullanılamaz (örneğin, bu konunun önceki kısımlarında `KestrelServerOptions` gösterildiği gibi yapılandırma veya yapılandırma dosyası kullanılıyor).</span><span class="sxs-lookup"><span data-stu-id="025cc-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="025cc-643">Hem `Listen` hem `UseUrls` de yaklaşımları aynı anda kullanıldığında `UseUrls` , `Listen` uç noktalar uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="025cc-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="025cc-644">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-644">IIS endpoint configuration</span></span>

<span data-ttu-id="025cc-645">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları veya `Listen` `UseUrls`ya da tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="025cc-646">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="025cc-647">ListenOptions. Protocols</span><span class="sxs-lookup"><span data-stu-id="025cc-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="025cc-648">Özelliği, bir bağlantı uç noktasında veya`HttpProtocols`sunucu için etkin HTTP protokollerini () belirler. `Protocols`</span><span class="sxs-lookup"><span data-stu-id="025cc-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="025cc-649">Sabit listesinden özelliğe`Protocols` bir değer atayın. `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="025cc-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="025cc-650">`HttpProtocols`sabit listesi değeri</span><span class="sxs-lookup"><span data-stu-id="025cc-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="025cc-651">Bağlantı protokolü izin verildi</span><span class="sxs-lookup"><span data-stu-id="025cc-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="025cc-652">Yalnızca HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="025cc-652">HTTP/1.1 only.</span></span> <span data-ttu-id="025cc-653">, TLS olmadan veya ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="025cc-654">Yalnızca HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="025cc-654">HTTP/2 only.</span></span> <span data-ttu-id="025cc-655">Yalnızca istemci [önceki bir bilgi modunu](https://tools.ietf.org/html/rfc7540#section-3.4)DESTEKLIYORSA, TLS olmadan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="025cc-656">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="025cc-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="025cc-657">HTTP/2 bir TLS ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantısı gerektirir; Aksi takdirde, bağlantı varsayılan olarak HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="025cc-658">Varsayılan protokol HTTP/1.1 ' dir.</span><span class="sxs-lookup"><span data-stu-id="025cc-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="025cc-659">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="025cc-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="025cc-660">TLS sürüm 1,2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="025cc-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="025cc-661">Yeniden anlaşma devre dışı</span><span class="sxs-lookup"><span data-stu-id="025cc-661">Renegotiation disabled</span></span>
* <span data-ttu-id="025cc-662">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="025cc-662">Compression disabled</span></span>
* <span data-ttu-id="025cc-663">En az kısa ömürlü anahtar değişim boyutları:</span><span class="sxs-lookup"><span data-stu-id="025cc-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="025cc-664">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 bit minimum</span><span class="sxs-lookup"><span data-stu-id="025cc-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="025cc-665">Sınırlı alan Diffie-Hellman (DHE) &lbrack; `TLS12` &rbrack; &ndash; 2048 bit minimum</span><span class="sxs-lookup"><span data-stu-id="025cc-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="025cc-666">Şifre paketi kara listede değil</span><span class="sxs-lookup"><span data-stu-id="025cc-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="025cc-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`P-256eliptik&lbrack; eğrisi Varsayılanolarak&lbrack;desteklenir. `FIPS186` &rbrack; `TLS-ECDHE` &rbrack;</span><span class="sxs-lookup"><span data-stu-id="025cc-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="025cc-668">Aşağıdaki örnek, 8000 numaralı bağlantı noktasında HTTP/1.1 ve HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="025cc-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="025cc-669">Bağlantılar, sağlanan bir sertifikayla TLS ile güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="025cc-669">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="025cc-670">İsteğe bağlı olarak `IConnectionAdapter` , belirli şifrelemeler için bağlantı başına TLS el sıkışmaları filtrelemek üzere bir uygulama oluşturun:</span><span class="sxs-lookup"><span data-stu-id="025cc-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null
        // indicates that no cipher algorithm supported by Kestrel matches the
        // requested algorithm(s).
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

<span data-ttu-id="025cc-671">*Protokolü yapılandırmadan ayarla*</span><span class="sxs-lookup"><span data-stu-id="025cc-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="025cc-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>Kestrel `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` yapılandırmasını yüklemek için varsayılan olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="025cc-673">Aşağıdaki *appSettings. JSON* örneğinde, tüm Kestrel uç noktaları için varsayılan bir bağlantı protokolü (http/1.1 ve http/2) oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="025cc-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="025cc-674">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç nokta için bağlantı protokolü oluşturur:</span><span class="sxs-lookup"><span data-stu-id="025cc-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="025cc-675">Yapılandırma tarafından ayarlanan kod geçersiz kılma değerlerinde belirtilen protokoller.</span><span class="sxs-lookup"><span data-stu-id="025cc-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="025cc-676">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-676">Transport configuration</span></span>

<span data-ttu-id="025cc-677">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="025cc-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="025cc-678">Bu, çağrıyı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> yapan ve aşağıdaki paketlerden birine bağlı olan ASP.NET Core 2,0 2,1 uygulamalarının önemli bir değişikliği olur:</span><span class="sxs-lookup"><span data-stu-id="025cc-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="025cc-679">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="025cc-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="025cc-680">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="025cc-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="025cc-681">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="025cc-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="025cc-682">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="025cc-683">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="025cc-684">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-684">URL prefixes</span></span>

<span data-ttu-id="025cc-685">, `UseUrls` `urls` `ASPNETCORE_URLS` Komut satırı bağımsız değişkeni, ana bilgisayar yapılandırma anahtarı veya ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir. `--urls`</span><span class="sxs-lookup"><span data-stu-id="025cc-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="025cc-686">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="025cc-687">Kestrel, kullanılarak `UseUrls`URL bağlamaları yapılandırılırken https 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="025cc-688">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="025cc-689">`0.0.0.0`Tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="025cc-690">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="025cc-691">`[::]`, IPv4 `0.0.0.0`'un IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="025cc-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="025cc-692">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="025cc-693">, Ve `*` `+`ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="025cc-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="025cc-694">Geçerli bir IP adresi olarak tanınmayan veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="025cc-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="025cc-695">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="025cc-696">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="025cc-697">Port `localhost` numarası veya bağlantı noktası numarası ile geri döngü IP 'si olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="025cc-698">`localhost` Belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="025cc-699">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="025cc-700">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="025cc-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="025cc-701">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="025cc-701">Host filtering</span></span>

<span data-ttu-id="025cc-702">Kestrel gibi önekleri `http://example.com:5000`temel alarak yapılandırmayı desteklese de, Kestrel büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="025cc-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="025cc-703">Ana `localhost` bilgisayar, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="025cc-704">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="025cc-705">`Host`Üstbilgiler doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="025cc-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="025cc-706">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="025cc-707">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="025cc-708">Ara yazılım tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>eklenir ve şunları çağırır <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="025cc-709">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="025cc-710">Ara yazılımı etkinleştirmek için `AllowedHosts` *appSettings. JSON*/appSettings içinde bir anahtar tanımlayın *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="025cc-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="025cc-711">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="025cc-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="025cc-712">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="025cc-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="025cc-713">[İletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) da bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçenek içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="025cc-714">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="025cc-715">İletilen `AllowedHosts` üstbilgiler ara yazılımı ile, istekler ters bir `Host` ara sunucu veya yük dengeleyici ile iletilirken üst bilgi korunurken, bu işlem için uygun bir ayar vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="025cc-716">Ana `AllowedHosts` bilgisayar filtreleme ara yazılımı ile ayarlama, Kestrel herkese açık bir uç sunucu olarak veya `Host` üst bilgi doğrudan iletildiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="025cc-717">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için <xref:host-and-deploy/proxy-load-balancer>bkz.</span><span class="sxs-lookup"><span data-stu-id="025cc-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="025cc-718">Kestrel, ASP.NET Core için platformlar arası [Web sunucusudur](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="025cc-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="025cc-719">Kestrel, ASP.NET Core proje şablonlarında varsayılan olarak bulunan Web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="025cc-720">Kestrel aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="025cc-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="025cc-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="025cc-721">HTTPS</span></span>
* <span data-ttu-id="025cc-722">[WebSockets](https://github.com/aspnet/websockets) 'i etkinleştirmek için kullanılan donuk yükseltme</span><span class="sxs-lookup"><span data-stu-id="025cc-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="025cc-723">NGINX 'in arkasında yüksek performans için UNIX Yuvaları</span><span class="sxs-lookup"><span data-stu-id="025cc-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="025cc-724">Kestrel, .NET Core 'un desteklediği tüm platformlarda ve sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="025cc-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="025cc-725">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="025cc-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="025cc-726">Ters ara sunucu ile Kestrel ne zaman kullanılır?</span><span class="sxs-lookup"><span data-stu-id="025cc-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="025cc-727">Kestrel, kendisi veya [Internet Information Services (IIS)](https://www.iis.net/), [NGINX](https://nginx.org)veya [Apache](https://httpd.apache.org/)gibi bir *ters ara sunucu*ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="025cc-728">Ters proxy sunucusu, ağdan gelen HTTP isteklerini alır ve Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="025cc-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="025cc-729">Sınır (Internet 'e yönelik) Web sunucusu olarak kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel, ters proxy sunucusu olmadan doğrudan Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="025cc-731">Ters Proxy yapılandırmasında kullanılan Kestrel:</span><span class="sxs-lookup"><span data-stu-id="025cc-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel, IIS, NGINX veya Apache gibi bir ters ara sunucu üzerinden Internet ile dolaylı olarak iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="025cc-733">İki yapılandırma de, ters ara sunucu sunucusuyla veya olmadan, desteklenen bir barındırma yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="025cc-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="025cc-734">Ters proxy sunucusu olmayan bir uç sunucu olarak kullanılan Kestrel, aynı IP ve bağlantı noktasının birden çok işlem arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="025cc-735">Kestrel bir bağlantı noktasını dinlemek üzere yapılandırıldığında, Kestrel isteklerin `Host` üst bilgilerinden bağımsız olarak bu bağlantı noktası için tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="025cc-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="025cc-736">Bağlantı noktalarını paylaşabilen bir ters proxy, istekleri benzersiz bir IP ve bağlantı noktası üzerinde Kestrel 'e iletme yeteneğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="025cc-737">Ters proxy sunucusu gerekli olmasa bile, ters proxy sunucu kullanılması iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="025cc-738">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="025cc-738">A reverse proxy:</span></span>

* <span data-ttu-id="025cc-739">, Barındırdığı uygulamaların açığa çıkarılan genel yüzey alanını sınırlayabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="025cc-740">Ek bir yapılandırma ve savunma katmanı sağlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="025cc-741">, Mevcut altyapıyla daha iyi tümleşebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="025cc-742">Yük dengelemeyi ve güvenli iletişim (HTTPS) yapılandırmasını kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="025cc-743">Yalnızca ters proxy sunucusu bir X. 509.440 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak, iç ağdaki uygulamanın sunucularıyla iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-744">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="025cc-745">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="025cc-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="025cc-746">Microsoft. [AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paketi [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="025cc-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="025cc-747">ASP.NET Core proje şablonları varsayılan olarak Kestrel kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="025cc-748">*Program.cs*' de, şablon kodu çağırır <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>ve bu da <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> arka planda çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="025cc-749">Çağrıldıktan `CreateDefaultBuilder`sonra ek yapılandırma sağlamak için şunu çağırın <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="025cc-750">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-750">Kestrel options</span></span>

<span data-ttu-id="025cc-751">Kestrel Web sunucusu, Internet 'e yönelik dağıtımlarda özellikle yararlı olan kısıtlama yapılandırma seçeneklerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="025cc-752"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> Sınıfının özelliğinde<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> kısıtlamaları ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="025cc-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="025cc-753">Özelliği `Limits` , <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfının bir örneğini barındırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="025cc-754">Aşağıdaki örnekler <xref:Microsoft.AspNetCore.Server.Kestrel.Core> ad alanını kullanır:</span><span class="sxs-lookup"><span data-stu-id="025cc-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="025cc-755">Etkin tut zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="025cc-756">[Etkin tutma zaman aşımını](https://tools.ietf.org/html/rfc7230#section-6.5)alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="025cc-757">Varsayılan olarak 2 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="025cc-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="025cc-758">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="025cc-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="025cc-759">En fazla eş zamanlı açık TCP bağlantısı sayısı tüm uygulama için aşağıdaki kodla ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="025cc-760">HTTP veya HTTPS 'den başka bir protokole (örneğin, bir WebSockets isteğinde) yükseltilen bağlantılara yönelik ayrı bir sınır vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="025cc-761">Bir bağlantı yükseltildikten sonra, bu `MaxConcurrentConnections` sınıra göre sayılmaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="025cc-762">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null).</span><span class="sxs-lookup"><span data-stu-id="025cc-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="025cc-763">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="025cc-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="025cc-764">Varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu değer yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="025cc-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="025cc-765">ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşım, bir eylem yönteminde <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliğini kullanmaktır:</span><span class="sxs-lookup"><span data-stu-id="025cc-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="025cc-766">Her istekte uygulama için kısıtlamanın nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="025cc-767">Ara yazılım içindeki belirli bir istek üzerindeki ayarı geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="025cc-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="025cc-768">Uygulama isteği okumaya başladıktan sonra bir istek üzerindeki sınırı yapılandırırsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="025cc-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="025cc-769">Özelliğin salt okuma `IsReadOnly` durumunda olup olmadığını belirten bir özellik vardır. Bu, sınırı yapılandırmanın çok geç olduğunu gösterir. `MaxRequestBodySize`</span><span class="sxs-lookup"><span data-stu-id="025cc-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="025cc-770">Bir uygulama [ASP.NET Core modülünün](xref:host-and-deploy/aspnet-core-module)arkasında [çalıştırıldığında](xref:host-and-deploy/iis/index#out-of-process-hosting-model) , IIS sınırı zaten ayarladığı için Kestrel 'nin istek gövdesi boyut sınırı devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="025cc-771">En az istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="025cc-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="025cc-772">Kestrel bayt/saniye cinsinden belirtilen fiyata ulaşan her saniye sonra denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="025cc-773">Oran en düşük değerin altına düşerse bağlantı zaman aşımına uğrar. Yetkisiz kullanım süresi, Kestrel 'in istemciye gönderme oranını en düşük süreye kadar artırabilme Bu süre boyunca fiyat denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="025cc-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="025cc-774">Yetkisiz kullanım süresi, TCP yavaş başlatma nedeniyle başlangıçta verileri yavaş bir hızda gönderen bağlantıların bırakılmasını önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="025cc-775">Varsayılan en düşük oran, 5 saniyelik bir yetkisiz kullanım süresi ile 240 bayt/saniye olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="025cc-776">Yanıt için bir minimum oran da geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="025cc-777">İstek sınırını ayarlamaya yönelik kod ve yanıt sınırı, özellik ve arabirim adlarında olduğu gibi aynı `RequestBody` `Response` olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="025cc-778">*Program.cs*içinde en düşük veri hızlarının nasıl yapılandırılacağını gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="025cc-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="025cc-779">İstek üst bilgileri zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="025cc-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="025cc-780">Sunucunun istek üst bilgilerini alması için harcadığı en uzun süreyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="025cc-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="025cc-781">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="025cc-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="025cc-782">Zaman uyumlu GÇ</span><span class="sxs-lookup"><span data-stu-id="025cc-782">Synchronous IO</span></span>

<span data-ttu-id="025cc-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO>istek ve yanıt için zaman uyumlu GÇ 'ye izin verilip verilmediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="025cc-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="025cc-784">Varsayılan değer `true`.</span><span class="sxs-lookup"><span data-stu-id="025cc-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="025cc-785">Çok sayıda engelleme zaman uyumlu GÇ işlemi, iş parçacığı havuzuna neden olabilir ve bu da uygulamanın yanıt vermemesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="025cc-786">Yalnızca zaman `AllowSynchronousIO` uyumsuz GÇ desteklemeyen bir kitaplık kullanılırken etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="025cc-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="025cc-787">Aşağıdaki örnek, zaman uyumlu GÇ 'yi devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="025cc-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="025cc-788">Diğer Kestrel seçenekleri ve limitleri hakkında daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="025cc-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="025cc-789">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-789">Endpoint configuration</span></span>

<span data-ttu-id="025cc-790">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="025cc-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="025cc-791">`https://localhost:5001`(bir yerel geliştirme sertifikası varsa)</span><span class="sxs-lookup"><span data-stu-id="025cc-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="025cc-792">Kullanarak URL 'Leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="025cc-792">Specify URLs using the:</span></span>

* <span data-ttu-id="025cc-793">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="025cc-794">`--urls`komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="025cc-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="025cc-795">`urls`Ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="025cc-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="025cc-796">`UseUrls`genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="025cc-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="025cc-797">Bu yaklaşımlar kullanılarak sağlanan değer bir veya daha fazla HTTP ve HTTPS uç noktası olabilir (varsayılan bir sertifika varsa HTTPS).</span><span class="sxs-lookup"><span data-stu-id="025cc-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="025cc-798">Değeri noktalı virgülle ayrılmış bir liste olarak yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="025cc-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="025cc-799">Bu yaklaşımlar hakkında daha fazla bilgi için [sunucu URL 'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırması](xref:fundamentals/host/web-host#override-configuration)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="025cc-800">Geliştirme sertifikası oluşturuldu:</span><span class="sxs-lookup"><span data-stu-id="025cc-800">A development certificate is created:</span></span>

* <span data-ttu-id="025cc-801">[.NET Core SDK](/dotnet/core/sdk) yüklendiği zaman.</span><span class="sxs-lookup"><span data-stu-id="025cc-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="025cc-802">[Geliştirme-CERT aracı](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="025cc-803">Bazı tarayıcılarda yerel geliştirme sertifikasına güvenmek için açık izin verilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="025cc-804">Proje şablonları, uygulamaları HTTPS üzerinde varsayılan olarak çalışacak şekilde yapılandırır ve [https yeniden yönlendirme ve HSTS desteği](xref:security/enforcing-ssl)içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="025cc-805">URL <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> öneklerini <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> ve bağlantı <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> noktalarını Kestrel için yapılandırmak üzere üzerinde çağrı veya Yöntemler.</span><span class="sxs-lookup"><span data-stu-id="025cc-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="025cc-806">`UseUrls`, komut satırı bağımsız değişkeni, `urls` ana bilgisayar `ASPNETCORE_URLS` yapılandırma anahtarı ve ortam değişkeni de çalışır, ancak bu bölümün ilerleyen kısımlarında belirtilen sınırlamalara sahiptir (https uç noktası için varsayılan sertifika kullanılabilir olmalıdır `--urls` yapılandırma).</span><span class="sxs-lookup"><span data-stu-id="025cc-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="025cc-807">`KestrelServerOptions`yapılandırmada</span><span class="sxs-lookup"><span data-stu-id="025cc-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="025cc-808">Configureendpointdefaults Varsayılanları (\<eylem listenoptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="025cc-809">Belirtilen her bir `Action` uç nokta için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="025cc-810">Birden `ConfigureEndpointDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="025cc-811">Configurehttpsdefaults (ACTION\<httpsconnectionadapteroptions >)</span><span class="sxs-lookup"><span data-stu-id="025cc-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="025cc-812">Her HTTPS uç `Action` noktası için çalıştırılacak bir yapılandırma belirtir.</span><span class="sxs-lookup"><span data-stu-id="025cc-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="025cc-813">Birden `ConfigureHttpsDefaults` çok kez çağırma önceki `Action`s 'yi son `Action` belirtilen ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="025cc-814">Yapılandırma (Iconation)</span><span class="sxs-lookup"><span data-stu-id="025cc-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="025cc-815">Bir <xref:Microsoft.Extensions.Configuration.IConfiguration> as girişi alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="025cc-816">Yapılandırma, Kestrel için yapılandırma bölümünün kapsamına alınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="025cc-817">ListenOptions. UseHttps</span><span class="sxs-lookup"><span data-stu-id="025cc-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="025cc-818">Kestrel 'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="025cc-819">`ListenOptions.UseHttps`uzantılardan</span><span class="sxs-lookup"><span data-stu-id="025cc-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="025cc-820">`UseHttps`&ndash; Kestrel 'i varsayılan sertifikayla HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="025cc-821">Varsayılan sertifika yapılandırılmamışsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="025cc-821">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="025cc-822">`ListenOptions.UseHttps`parametrelere</span><span class="sxs-lookup"><span data-stu-id="025cc-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="025cc-823">`filename`, uygulamanın içerik dosyalarını içeren dizine göre bir sertifika dosyasının yolu ve dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="025cc-824">`password`X. 509.440 sertifika verilerine erişmek için parola gereklidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="025cc-825">`configureOptions``Action` ,`HttpsConnectionAdapterOptions`öğesini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="025cc-826">`ListenOptions`Döndürür.</span><span class="sxs-lookup"><span data-stu-id="025cc-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="025cc-827">`storeName`, sertifikanın yükleneceği sertifika deposudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="025cc-828">`subject`, sertifika için konu adıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="025cc-829">`allowInvalid`geçersiz sertifikaların, otomatik olarak imzalanan sertifikalar gibi göz önünde bulundurulmayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="025cc-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="025cc-830">`location`, sertifikanın yükleneceği mağaza konumudur.</span><span class="sxs-lookup"><span data-stu-id="025cc-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="025cc-831">`serverCertificate`, X. 509.440 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="025cc-832">Üretimde HTTPS 'nin açıkça yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="025cc-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="025cc-833">En azından, varsayılan bir sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="025cc-834">Daha sonra açıklanan desteklenen yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="025cc-834">Supported configurations described next:</span></span>

* <span data-ttu-id="025cc-835">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="025cc-835">No configuration</span></span>
* <span data-ttu-id="025cc-836">Varsayılan sertifikayı yapılandırmadan Değiştir</span><span class="sxs-lookup"><span data-stu-id="025cc-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="025cc-837">Koddaki varsayılanları değiştirme</span><span class="sxs-lookup"><span data-stu-id="025cc-837">Change the defaults in code</span></span>

<span data-ttu-id="025cc-838">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="025cc-838">*No configuration*</span></span>

<span data-ttu-id="025cc-839">Kestrel tarihinde `http://localhost:5000` dinler ve `https://localhost:5001` (varsayılan bir sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="025cc-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="025cc-840">*Varsayılan sertifikayı yapılandırmadan Değiştir*</span><span class="sxs-lookup"><span data-stu-id="025cc-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="025cc-841">`CreateDefaultBuilder`Kestrel `Configure(context.Configuration.GetSection("Kestrel"))` yapılandırmasını yüklemek için varsayılan olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="025cc-842">Varsayılan bir HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="025cc-843">Disk üzerindeki bir dosyadan ya da bir sertifika deposundan kullanılacak URL 'Ler ve Sertifikalar dahil olmak üzere birden çok uç nokta yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="025cc-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="025cc-844">Aşağıdaki *appSettings. JSON* örneğinde:</span><span class="sxs-lookup"><span data-stu-id="025cc-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="025cc-845">Geçersiz sertifikaların kullanılmasına izin vermek `true` için **allowwınvalid** ' i ayarlayın (örneğin, otomatik olarak imzalanan sertifikalar).</span><span class="sxs-lookup"><span data-stu-id="025cc-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="025cc-846">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (aşağıdaki örnekte bulunan**httpsdefaultcert** ), **Sertifikalar** > **varsayılan** veya geliştirme sertifikası altında tanımlanan sertifikaya geri döner.</span><span class="sxs-lookup"><span data-stu-id="025cc-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="025cc-847">Herhangi bir sertifika düğümü için **yol** ve **parola** kullanmanın alternatifi sertifika deposu alanlarını kullanarak sertifikayı belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="025cc-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="025cc-848">Örneğin, **sertifika** > **varsayılan** sertifikası şu şekilde belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="025cc-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="025cc-849">Şema notları:</span><span class="sxs-lookup"><span data-stu-id="025cc-849">Schema notes:</span></span>

* <span data-ttu-id="025cc-850">Uç nokta adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="025cc-851">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="025cc-852">Her uç nokta için parametresigereklidir.`Url`</span><span class="sxs-lookup"><span data-stu-id="025cc-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="025cc-853">Bu parametrenin biçimi, en üst düzey `Urls` yapılandırma parametresiyle aynıdır, ancak tek bir değerle sınırlı olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="025cc-854">Bu uç noktalar, üst düzey `Urls` yapılandırmada tanımlananlar yerine bunlara ekleme yerine bunların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="025cc-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="025cc-855">Kullanılarak `Listen` kodda tanımlanan uç noktalar yapılandırma bölümünde tanımlanan uç noktalar ile birikimlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="025cc-856">Bu `Certificate` bölüm isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="025cc-857">`Certificate` Bölüm belirtilmemişse, önceki senaryolarda tanımlanan varsayılanlar kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="025cc-858">Kullanılabilir varsayılan değer yoksa, sunucu bir özel durum oluşturur ve başlayamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="025cc-859">&ndash;&ndash; Bu bölüm hem yol parolasını hem de konu deposu sertifikalarını destekler. `Certificate`</span><span class="sxs-lookup"><span data-stu-id="025cc-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="025cc-860">Herhangi bir sayıda uç nokta, bağlantı noktası çakışmalarına neden olmadıkları sürece bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="025cc-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))`yapılandırılmış bir `KestrelConfigurationLoader` uç noktanın `.Endpoint(string name, listenOptions => { })` ayarlarını tamamlamak için kullanılabilecek bir yöntemi olan bir döndürür:</span><span class="sxs-lookup"><span data-stu-id="025cc-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="025cc-862">`KestrelServerOptions.ConfigurationLoader`, tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>sağlana gibi var olan yükleyicisindeki yinelemeye devam etmek için doğrudan erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="025cc-863">Her uç noktanın yapılandırma bölümü, özel ayarların okunabilmesi için `Endpoint` yöntemindeki seçeneklerde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="025cc-864">Birden çok yapılandırma, başka bir bölümle `options.Configure(context.Configuration.GetSection("{SECTION}"))` yeniden çağırarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="025cc-865">Önceki örneklerde açıkça çağrılmadığı takdirde `Load` , yalnızca son yapılandırma kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="025cc-866">Metapackage, varsayılan yapılandırma `Load` bölümünün değiştirilmesini sağlayacak şekilde çağırmıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="025cc-867">`KestrelConfigurationLoader`API ailesini aşırı yükleme `Endpoint` olarak yansıtır, bu nedenle kod ve yapılandırma uç noktaları aynı yerde yapılandırılabilir. `KestrelServerOptions` `Listen`</span><span class="sxs-lookup"><span data-stu-id="025cc-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="025cc-868">Bu aşırı yüklemeler adları kullanmaz ve yalnızca yapılandırmadan varsayılan ayarları kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="025cc-869">*Koddaki varsayılanları değiştirme*</span><span class="sxs-lookup"><span data-stu-id="025cc-869">*Change the defaults in code*</span></span>

<span data-ttu-id="025cc-870">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` , önceki senaryoda belirtilen varsayılan sertifikayı geçersiz kılma `ListenOptions` dahil `HttpsConnectionAdapterOptions`, ve için varsayılan ayarları değiştirmek üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="025cc-871">`ConfigureEndpointDefaults`ve `ConfigureHttpsDefaults` herhangi bir uç nokta yapılandırılmadan önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="025cc-872">*SNı için Kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="025cc-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="025cc-873">[Sunucu adı belirtme (SNı)](https://tools.ietf.org/html/rfc6066#section-3) , aynı IP adresi ve bağlantı noktası üzerinde birden fazla etki alanını barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="025cc-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="025cc-874">SNı 'nin çalışması için, istemci, TLS el sıkışması sırasında güvenli oturum ana bilgisayar adını, sunucunun doğru sertifikayı sağlayabilmesi için gönderir.</span><span class="sxs-lookup"><span data-stu-id="025cc-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="025cc-875">İstemci, TLS anlaşmasını izleyen güvenli oturum sırasında sunucuyla şifreli iletişim için bulunan sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="025cc-876">Kestrel, `ServerCertificateSelector` geri çağırma aracılığıyla SNI destekler.</span><span class="sxs-lookup"><span data-stu-id="025cc-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="025cc-877">Geri çağırma, uygulamanın ana bilgisayar adını incelemesine ve uygun sertifikayı seçmesini sağlamak için bağlantı başına bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="025cc-878">SNı desteği şunları gerektirir:</span><span class="sxs-lookup"><span data-stu-id="025cc-878">SNI support requires:</span></span>

* <span data-ttu-id="025cc-879">Hedef Framework `netcoreapp2.1` veya sonraki sürümlerde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="025cc-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="025cc-880">Veya sonraki sürümlerde, geri çağırma çağrılır, `name` ancak her zaman `null`olur. `net461`</span><span class="sxs-lookup"><span data-stu-id="025cc-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="025cc-881">Ayrıca `name` , istemci `null` , TLS el sıkışmasının ana bilgisayar adı parametresini sağlamıyorsa de olur.</span><span class="sxs-lookup"><span data-stu-id="025cc-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="025cc-882">Tüm Web siteleri aynı Kestrel örneğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="025cc-883">Kestrel, bir IP adresi ve bağlantı noktasının bir ters proxy olmadan birden çok örnek arasında paylaşılmasını desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="025cc-884">TCP yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-884">Bind to a TCP socket</span></span>

<span data-ttu-id="025cc-885"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Yöntemi bir TCP yuvasına bağlanır ve bir seçenek lambda X. 509.440 sertifika yapılandırmasına izin verir:</span><span class="sxs-lookup"><span data-stu-id="025cc-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="025cc-886">Örnek, HTTPS 'yi ile <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>bir uç nokta için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="025cc-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="025cc-887">Belirli uç noktalar için diğer Kestrel ayarlarını yapılandırmak üzere aynı API 'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="025cc-888">UNIX yuvasına bağlama</span><span class="sxs-lookup"><span data-stu-id="025cc-888">Bind to a Unix socket</span></span>

<span data-ttu-id="025cc-889">Aşağıdaki örnekte gösterildiği gibi NGINX ile gelişmiş performans için ile birlikte <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> bir UNIX yuvası dinleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="025cc-890">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="025cc-890">Port 0</span></span>

<span data-ttu-id="025cc-891">Bağlantı noktası numarası `0` belirtildiğinde, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="025cc-892">Aşağıdaki örnek, çalışma zamanında Kestrel gerçekte hangi bağlantı noktasının gerçekten bağlandığını nasıl belirleyeceğini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="025cc-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="025cc-893">Uygulama çalıştırıldığında konsol penceresi çıkışı, uygulamanın erişilebileceği dinamik bağlantı noktasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="025cc-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="025cc-894">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="025cc-894">Limitations</span></span>

<span data-ttu-id="025cc-895">Aşağıdaki yaklaşımlar ile uç noktaları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="025cc-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="025cc-896">`--urls`komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="025cc-897">`urls`Ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="025cc-897">`urls` host configuration key</span></span>
* <span data-ttu-id="025cc-898">`ASPNETCORE_URLS`ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="025cc-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="025cc-899">Bu yöntemler, kodun Kestrel dışındaki sunucularla çalışmasını sağlamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="025cc-900">Ancak, aşağıdaki sınırlamalara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="025cc-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="025cc-901">HTTPS uç noktası yapılandırmasında varsayılan bir sertifika sağlanmadığı sürece https bu yaklaşımlar ile kullanılamaz (örneğin, bu konunun önceki kısımlarında `KestrelServerOptions` gösterildiği gibi yapılandırma veya yapılandırma dosyası kullanılıyor).</span><span class="sxs-lookup"><span data-stu-id="025cc-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="025cc-902">Hem `Listen` hem `UseUrls` de yaklaşımları aynı anda kullanıldığında `UseUrls` , `Listen` uç noktalar uç noktaları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="025cc-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="025cc-903">IIS uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-903">IIS endpoint configuration</span></span>

<span data-ttu-id="025cc-904">IIS kullanırken, IIS geçersiz kılma bağlamaları için URL bağlamaları veya `Listen` `UseUrls`ya da tarafından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="025cc-905">Daha fazla bilgi için [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="025cc-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="025cc-906">Aktarım yapılandırması</span><span class="sxs-lookup"><span data-stu-id="025cc-906">Transport configuration</span></span>

<span data-ttu-id="025cc-907">ASP.NET Core 2,1 sürümü ile Kestrel 'in varsayılan taşıması artık libuv ' d i temel değildir ancak bunun yerine yönetilen yuvaları temel alır.</span><span class="sxs-lookup"><span data-stu-id="025cc-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="025cc-908">Bu, çağrıyı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> yapan ve aşağıdaki paketlerden birine bağlı olan ASP.NET Core 2,0 2,1 uygulamalarının önemli bir değişikliği olur:</span><span class="sxs-lookup"><span data-stu-id="025cc-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="025cc-909">[Microsoft. AspNetCore. Server. Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket başvurusu)</span><span class="sxs-lookup"><span data-stu-id="025cc-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="025cc-910">Microsoft. AspNetCore. app</span><span class="sxs-lookup"><span data-stu-id="025cc-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="025cc-911">Libuv kullanımını gerektiren projeler için:</span><span class="sxs-lookup"><span data-stu-id="025cc-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="025cc-912">Uygulamanın proje dosyasına [Microsoft. AspNetCore. Server. Kestrel. Transport. libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) paketi için bir bağımlılık ekleyin:</span><span class="sxs-lookup"><span data-stu-id="025cc-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="025cc-913">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="025cc-914">URL önekleri</span><span class="sxs-lookup"><span data-stu-id="025cc-914">URL prefixes</span></span>

<span data-ttu-id="025cc-915">, `UseUrls` `urls` `ASPNETCORE_URLS` Komut satırı bağımsız değişkeni, ana bilgisayar yapılandırma anahtarı veya ortam değişkeni kullanılırken, URL önekleri aşağıdaki biçimlerden birinde olabilir. `--urls`</span><span class="sxs-lookup"><span data-stu-id="025cc-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="025cc-916">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="025cc-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="025cc-917">Kestrel, kullanılarak `UseUrls`URL bağlamaları yapılandırılırken https 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="025cc-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="025cc-918">Bağlantı noktası numarası olan IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="025cc-919">`0.0.0.0`Tüm IPv4 adreslerine bağlanan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="025cc-920">Bağlantı noktası numarasına sahip IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="025cc-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="025cc-921">`[::]`, IPv4 `0.0.0.0`'un IPv6 eşdeğeridir.</span><span class="sxs-lookup"><span data-stu-id="025cc-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="025cc-922">Bağlantı noktası numarası olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="025cc-923">, Ve `*` `+`ana bilgisayar adları özel değildir.</span><span class="sxs-lookup"><span data-stu-id="025cc-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="025cc-924">Geçerli bir IP adresi olarak tanınmayan veya `localhost` tüm IPv4 ve IPv6 IP 'lerine bağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="025cc-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="025cc-925">Farklı ana bilgisayar adlarını aynı bağlantı noktasında farklı ASP.NET Core uygulamalarına bağlamak için, [http. sys](xref:fundamentals/servers/httpsys) veya IIS, NGINX veya Apache gibi bir ters proxy sunucusu kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="025cc-926">Ters Proxy yapılandırmasında barındırma, [konak filtrelemeyi](#host-filtering)gerektirir.</span><span class="sxs-lookup"><span data-stu-id="025cc-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="025cc-927">Port `localhost` numarası veya bağlantı noktası numarası ile geri döngü IP 'si olan ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="025cc-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="025cc-928">`localhost` Belirtildiğinde, Kestrel hem IPv4 hem de IPv6 geri döngü arabirimlerini bağlamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="025cc-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="025cc-929">İstenen bağlantı noktası, herhangi bir geri döngü arabiriminde başka bir hizmet tarafından kullanılıyorsa, Kestrel başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="025cc-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="025cc-930">Herhangi bir geri döngü arabirimi başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediği için), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="025cc-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="025cc-931">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="025cc-931">Host filtering</span></span>

<span data-ttu-id="025cc-932">Kestrel gibi önekleri `http://example.com:5000`temel alarak yapılandırmayı desteklese de, Kestrel büyük ölçüde ana bilgisayar adını yoksayar.</span><span class="sxs-lookup"><span data-stu-id="025cc-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="025cc-933">Ana `localhost` bilgisayar, geri döngü adreslerine bağlama için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="025cc-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="025cc-934">Açık IP adresi dışındaki tüm ana bilgisayar tüm genel IP adreslerine bağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="025cc-935">`Host`Üstbilgiler doğrulanmadı.</span><span class="sxs-lookup"><span data-stu-id="025cc-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="025cc-936">Geçici bir çözüm olarak, ana bilgisayar filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="025cc-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="025cc-937">Ana bilgisayar filtreleme ara yazılımı, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 veya 2,2) ' de yer alan [Microsoft. Aspnetcore. hostfiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="025cc-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="025cc-938">Ara yazılım tarafından <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>eklenir ve şunları çağırır <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="025cc-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="025cc-939">Ana bilgisayar filtreleme ara yazılımı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="025cc-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="025cc-940">Ara yazılımı etkinleştirmek için `AllowedHosts` *appSettings. JSON*/appSettings içinde bir anahtar tanımlayın *.\< EnvironmentName >. JSON*.</span><span class="sxs-lookup"><span data-stu-id="025cc-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="025cc-941">Değer, bağlantı noktası numaraları olmayan ana bilgisayar adlarının noktalı virgülle ayrılmış listesidir:</span><span class="sxs-lookup"><span data-stu-id="025cc-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="025cc-942">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="025cc-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="025cc-943">[İletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer) da bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçenek içerir.</span><span class="sxs-lookup"><span data-stu-id="025cc-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="025cc-944">İletilen üstbilgiler ara yazılımı ve ana bilgisayar filtreleme ara yazılımı, farklı senaryolar için benzer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="025cc-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="025cc-945">İletilen `AllowedHosts` üstbilgiler ara yazılımı ile, istekler ters bir `Host` ara sunucu veya yük dengeleyici ile iletilirken üst bilgi korunurken, bu işlem için uygun bir ayar vardır.</span><span class="sxs-lookup"><span data-stu-id="025cc-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="025cc-946">Ana `AllowedHosts` bilgisayar filtreleme ara yazılımı ile ayarlama, Kestrel herkese açık bir uç sunucu olarak veya `Host` üst bilgi doğrudan iletildiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="025cc-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="025cc-947">Iletilen üstbilgiler ara yazılımı hakkında daha fazla bilgi için <xref:host-and-deploy/proxy-load-balancer>bkz.</span><span class="sxs-lookup"><span data-stu-id="025cc-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="025cc-948">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="025cc-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="025cc-949">RFC 7230: İleti sözdizimi ve yönlendirme (Bölüm 5,4: Konağının</span><span class="sxs-lookup"><span data-stu-id="025cc-949">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
