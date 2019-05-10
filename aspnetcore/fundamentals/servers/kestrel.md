---
title: ASP.NET core'da kestrel web sunucusu uygulaması
author: guardrex
description: Kestrel'i, ASP.NET Core için platformlar arası web sunucusu hakkında bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/28/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b5b05dbd553124cecac2ec7ddb55c939cb91c8ad
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450997"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="a1b48-103">ASP.NET core'da kestrel web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="a1b48-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="a1b48-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="a1b48-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a1b48-105">Bu konuda 1.1 sürümü için indirme [Kestrel web server (sürüm 1.1, PDF) ASP.NET Core uygulamasında](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="a1b48-105">For the 1.1 version of this topic, download [Kestrel web server implementation in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="a1b48-106">Kestrel'i olduğu bir platformlar arası [ASP.NET Core web sunucusu](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="a1b48-106">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="a1b48-107">Kestrel'i ASP.NET Core proje şablonları, varsayılan olarak bulunan bir web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-107">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="a1b48-108">Kestrel'i aşağıdaki senaryoları destekler:</span><span class="sxs-lookup"><span data-stu-id="a1b48-108">Kestrel supports the following scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a1b48-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a1b48-109">HTTPS</span></span>
* <span data-ttu-id="a1b48-110">Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="a1b48-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a1b48-111">Yüksek performans Ngınx arkasında UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="a1b48-111">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="a1b48-112">HTTP/2 (Macos'ta dışındaki&dagger;)</span><span class="sxs-lookup"><span data-stu-id="a1b48-112">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="a1b48-113">&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-113">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="a1b48-114">HTTPS</span><span class="sxs-lookup"><span data-stu-id="a1b48-114">HTTPS</span></span>
* <span data-ttu-id="a1b48-115">Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="a1b48-115">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="a1b48-116">Yüksek performans Ngınx arkasında UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="a1b48-116">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="a1b48-117">Kestrel'i tüm platformlarda ve .NET Core destekleyen sürümler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-117">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="a1b48-118">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a1b48-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="a1b48-119">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="a1b48-119">HTTP/2 support</span></span>

<span data-ttu-id="a1b48-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki gereksinimleri dayandırırsanız ASP.NET Core uygulamaları karşılanması için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-120">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="a1b48-121">İşletim Sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="a1b48-121">Operating system&dagger;</span></span>
  * <span data-ttu-id="a1b48-122">Windows Server 2016/Windows 10 veya üzeri&Dagger;</span><span class="sxs-lookup"><span data-stu-id="a1b48-122">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="a1b48-123">Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="a1b48-123">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="a1b48-124">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a1b48-124">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="a1b48-125">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı</span><span class="sxs-lookup"><span data-stu-id="a1b48-125">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="a1b48-126">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="a1b48-126">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a1b48-127">&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-127">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="a1b48-128">&Dagger;Kestrel'i HTTP/2 Windows Server 2012 R2 ve Windows 8.1 için destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-128">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="a1b48-129">Bu işletim sistemlerinde desteklenen TLS şifre paketlerinin listesini sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-129">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="a1b48-130">Bir Eliptik Eğri Dijital imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika, TLS bağlantıları güvenli hale getirmek için gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-130">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="a1b48-131">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-131">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="a1b48-132">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-132">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="a1b48-133">Yapılandırma hakkında daha fazla bilgi için bkz. [Kestrel seçenekleri](#kestrel-options) ve [ListenOptions.Protocols](#listenoptionsprotocols) bölümler.</span><span class="sxs-lookup"><span data-stu-id="a1b48-133">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="a1b48-134">Ne zaman Kestrel ters Ara sunucu ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-134">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="a1b48-135">Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters Ara sunucu*, gibi [Internet Information Services (IIS)](https://www.iis.net/), [Ngınx](http://nginx.org), veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a1b48-135">You can use Kestrel by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a1b48-136">Ters Ara sunucu HTTP isteklerinin ağdan alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-136">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="a1b48-137">Bir edge (Internet'e yönelik) web sunucusu olarak kullanılan kestrel:</span><span class="sxs-lookup"><span data-stu-id="a1b48-137">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="a1b48-139">Ters proxy yapılandırmasında kullanılan kestrel:</span><span class="sxs-lookup"><span data-stu-id="a1b48-139">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a1b48-141">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir desteklenen barındırma ASP.NET Core 2.1 veya Internet'ten isteklerini alacak sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-141">Either configuration&mdash;with or without a reverse proxy server&mdash;is a supported hosting configuration for ASP.NET Core 2.1 or later apps that receive requests from the Internet.</span></span>

<span data-ttu-id="a1b48-142">Kestrel'i ters Ara sunucu olmadan bir uç sunucusu olarak kullanılan aynı IP adresini ve bağlantı noktası arasında birden çok işlem paylaşımı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a1b48-142">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="a1b48-143">Kestrel'i bir bağlantı noktasında dinleyecek şekilde yapılandırıldığında, Kestrel tüm istekleri bağımsız olarak bu bağlantı noktası trafiğini işler `Host` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="a1b48-143">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="a1b48-144">Bağlantı noktalarını paylaşan bir ters proxy Kestrel benzersiz bir IP ve bağlantı noktası isteklerini iletmek için özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-144">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="a1b48-145">Ters proxy sunucusu gerekli olmasa bile bir ters proxy sunucusu kullanarak iyi bir seçim olabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-145">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="a1b48-146">Ters proxy:</span><span class="sxs-lookup"><span data-stu-id="a1b48-146">A reverse proxy:</span></span>

* <span data-ttu-id="a1b48-147">Barındırdığı uygulamaları kullanıma sunulan ortak yüzey alanını sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a1b48-147">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="a1b48-148">Ek bir yapılandırma ve koruma katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-148">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="a1b48-149">Var olan altyapınızla tümleştirebilirsiniz daha iyi.</span><span class="sxs-lookup"><span data-stu-id="a1b48-149">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="a1b48-150">Yük Dengeleme ve güvenli (HTTPS) yapılandırma basitleştirin.</span><span class="sxs-lookup"><span data-stu-id="a1b48-150">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="a1b48-151">Ters proxy sunucusu yalnızca bir X.509 sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak iç ağ üzerinde uygulama sunucularınız ile iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-151">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="a1b48-152">Ters proxy yapılandırmasında barındırma gerektirir [filtreleme konak](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="a1b48-152">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="a1b48-153">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="a1b48-153">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="a1b48-154">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="a1b48-154">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="a1b48-155">ASP.NET Core proje şablonları, varsayılan olarak Kestrel kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-155">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="a1b48-156">İçinde *Program.cs*, şablon kod çağrıları <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, çağıran <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> arka planda.</span><span class="sxs-lookup"><span data-stu-id="a1b48-156">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a1b48-157">Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, kullanın `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="a1b48-157">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

<span data-ttu-id="a1b48-158">Uygulama çağırırsanız değil `CreateDefaultBuilder` ana bilgisayar'kurmak için çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **önce** çağırma `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="a1b48-158">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        })
        .Build();

    host.Run();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a1b48-159">Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="a1b48-159">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="a1b48-160">Kestrel'i seçenekleri</span><span class="sxs-lookup"><span data-stu-id="a1b48-160">Kestrel options</span></span>

<span data-ttu-id="a1b48-161">Kestrel'i web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı olan kısıtlaması yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-161">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="a1b48-162">Kısıtlamaları ayarlama <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> özelliği <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a1b48-162">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="a1b48-163">`Limits` Özelliği bir örneğini tutan <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a1b48-163">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="a1b48-164">En fazla istemci bağlantısı</span><span class="sxs-lookup"><span data-stu-id="a1b48-164">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>  
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="a1b48-165">Aşağıdaki kod ile tüm uygulama için eşzamanlı açık TCP bağlantıları sayısı ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-165">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="a1b48-166">HTTP veya HTTPS, başka bir protokol (örneğin, WebSockets istek üzerine) a yükseltilmiştir bağlantıları için ayrı bir sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-166">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="a1b48-167">Bir bağlantı yükseltildikten sonra karşı sayılır değil `MaxConcurrentConnections` sınırı.</span><span class="sxs-lookup"><span data-stu-id="a1b48-167">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="a1b48-168">Bağlantı sayısı, varsayılan olarak sınırsız (null) olur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-168">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="a1b48-169">En fazla istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="a1b48-169">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="a1b48-170">Varsayılan en büyük istek gövdesi boyutu 30.000.000, yaklaşık 28.6 MB bayttır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-170">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="a1b48-171">Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşımdır <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> özniteliği bir eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a1b48-171">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="a1b48-172">Her istek için uygulama kısıtlama yapılandırma gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-172">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="a1b48-173">Belirli bir istekte Ara yazılımında ayarı geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a1b48-173">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

<span data-ttu-id="a1b48-174">Uygulama isteği okumak başlatıldıktan sonra bir istekte sınırını yapılandırmak çalışırsanız, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-174">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="a1b48-175">Var. bir `IsReadOnly` gösterir özelliği `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-175">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="a1b48-176">En az bir istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="a1b48-176">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>  
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="a1b48-177">Veri içinde belirtilen hızda bayt/saniye gelen, saniyede kestrel denetler.</span><span class="sxs-lookup"><span data-stu-id="a1b48-177">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="a1b48-178">Hızı en düşerse, bağlantının zaman aşımına uğradı. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme hızını artırmak için istemci verdiğini süreyi belirtir; Bu süre boyunca oranı işaretlenmemiş.</span><span class="sxs-lookup"><span data-stu-id="a1b48-178">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="a1b48-179">Yetkisiz kullanım süresi, başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderen bağlantıları verilmemesini yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-179">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="a1b48-180">Varsayılan en az 5 saniye yetkisiz kullanım süresi ile 240 bayt/saniye oranıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-180">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="a1b48-181">En düşük bir ücretle yanıt için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-181">A minimum rate also applies to the response.</span></span> <span data-ttu-id="a1b48-182">İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynıdır `RequestBody` veya `Response` özelliği ve arabirimi adları.</span><span class="sxs-lookup"><span data-stu-id="a1b48-182">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="a1b48-183">En az veriyi hızları yapılandırma gösteren bir örnek aşağıdadır *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a1b48-183">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

<span data-ttu-id="a1b48-184">Ara yazılım istek başına en düşük oran sınırları geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a1b48-184">You can override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a1b48-185">Önceki örnekte başvurulan hiçbiri oranı özelliği mevcut `HttpContext.Features` istek başına temelinde oran sınırlarını değiştirme HTTP/2 için istek çoğullama protokolün desteği nedeniyle desteklenmediğinden, HTTP/2 istekleri için.</span><span class="sxs-lookup"><span data-stu-id="a1b48-185">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="a1b48-186">Sunucu çapında hız sınırları ile yapılandırılmış `KestrelServerOptions.Limits` HTTP/1.x hem de HTTP/2 bağlantıları için hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-186">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="a1b48-187">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="a1b48-187">Maximum streams per connection</span></span>

<span data-ttu-id="a1b48-188">`Http2.MaxStreamsPerConnection` HTTP/2 bağlantı başına akış eş zamanlı istek sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-188">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="a1b48-189">Aşırı akışları çevrilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-189">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="a1b48-190">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="a1b48-190">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="a1b48-191">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="a1b48-191">Header table size</span></span>

<span data-ttu-id="a1b48-192">HPACK kod çözücü, HTTP/2 bağlantılar için HTTP üstbilgileri açar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-192">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="a1b48-193">`Http2.HeaderTableSize` HPACK kod çözücü kullanan üst bilgi sıkıştırma tablonun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-193">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="a1b48-194">Değer, sekizlik tabanda sağlanır ve sıfır (0) büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-194">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="a1b48-195">Varsayılan değer 4096'dır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-195">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="a1b48-196">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="a1b48-196">Maximum frame size</span></span>

<span data-ttu-id="a1b48-197">`Http2.MaxFrameSize` en büyük boyutunu almak için HTTP/2 bağlantı çerçeve yükü gösterir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-197">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="a1b48-198">Değer sekizlik tabanda sağlanır ve 2 arasında olmalıdır ^ (16,384) 14. ve 2 ^ 24-1 (16.777.215).</span><span class="sxs-lookup"><span data-stu-id="a1b48-198">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="a1b48-199">Varsayılan değer olan 2 ^ 14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="a1b48-199">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="a1b48-200">En büyük istek üst bilgi boyutu</span><span class="sxs-lookup"><span data-stu-id="a1b48-200">Maximum request header size</span></span>

<span data-ttu-id="a1b48-201">`Http2.MaxRequestHeaderFieldSize` izin verilen maksimum boyut sekizli istek üstbilgi değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-201">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="a1b48-202">Bu sınır, hem adı hem de birlikte bunların sıkıştırılmış ve sıkıştırılmamış gösterimleri değeri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-202">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="a1b48-203">Değer (0) sıfırdan büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-203">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="a1b48-204">Olmak üzere 8.192 varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-204">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="a1b48-205">İlk bağlantı pencere boyutu</span><span class="sxs-lookup"><span data-stu-id="a1b48-205">Initial connection window size</span></span>

<span data-ttu-id="a1b48-206">`Http2.InitialConnectionWindowSize` Sunucu arabellekleri üzerinden tüm istekler (akışları) bağlantı başına tek seferde toplu bayt cinsinden en büyük istek gövdesi veriler gösterir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-206">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="a1b48-207">İstek tarafından da sınırlıdır `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-207">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="a1b48-208">Değer büyüktür veya eşittir 65.535 ve 2'den az olmalıdır. ^ 31 (2,147,483,648).</span><span class="sxs-lookup"><span data-stu-id="a1b48-208">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="a1b48-209">128 KB (131,072) varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-209">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="a1b48-210">Akış başlangıç boyutu penceresi</span><span class="sxs-lookup"><span data-stu-id="a1b48-210">Initial stream window size</span></span>

<span data-ttu-id="a1b48-211">`Http2.InitialStreamWindowSize` İstek (akışı) başına tek seferde sunucu arabelleğinin bayt cinsinden en büyük istek gövde verilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-211">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="a1b48-212">İstek tarafından da sınırlıdır `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-212">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="a1b48-213">Değer büyüktür veya eşittir 65.535 ve 2'den az olmalıdır. ^ 31 (2,147,483,648).</span><span class="sxs-lookup"><span data-stu-id="a1b48-213">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="a1b48-214">96 KB'lık (98,304) varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-214">The default value is 96 KB (98,304).</span></span>

::: moniker-end

<span data-ttu-id="a1b48-215">Diğer seçenekleri Kestrel ve sınırları hakkında daha fazla bilgi için bkz:</span><span class="sxs-lookup"><span data-stu-id="a1b48-215">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="a1b48-216">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a1b48-216">Endpoint configuration</span></span>

<span data-ttu-id="a1b48-217">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="a1b48-217">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="a1b48-218">`https://localhost:5001` (yerel geliştirme sertifikası mevcut olduğunda)</span><span class="sxs-lookup"><span data-stu-id="a1b48-218">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="a1b48-219">Kullanarak URL'leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="a1b48-219">Specify URLs using the:</span></span>

* <span data-ttu-id="a1b48-220">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="a1b48-220">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="a1b48-221">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="a1b48-221">`--urls` command-line argument.</span></span>
* <span data-ttu-id="a1b48-222">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="a1b48-222">`urls` host configuration key.</span></span>
* <span data-ttu-id="a1b48-223">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a1b48-223">`UseUrls` extension method.</span></span>

<span data-ttu-id="a1b48-224">Bu yaklaşımları kullanarak sağlanan değer, bir veya daha fazla HTTP ve HTTPS uç noktası (varsayılan sertifika varsa HTTPS) olabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-224">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="a1b48-225">Değer noktalı virgülle ayrılmış listesini yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="a1b48-225">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="a1b48-226">Bu yaklaşımlar hakkında daha fazla bilgi için bkz. [sunucu URL'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırmasını](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="a1b48-226">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="a1b48-227">Bir geliştirme sertifikası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="a1b48-227">A development certificate is created:</span></span>

* <span data-ttu-id="a1b48-228">Zaman [.NET Core SDK'sı](/dotnet/core/sdk) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-228">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="a1b48-229">[Dev-certs aracını](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-229">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="a1b48-230">Bazı tarayıcılar, tarayıcının yerel geliştirme sertifikasına güvenmek açık izin vermenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-230">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="a1b48-231">ASP.NET Core 2.1 ve üzeri proje şablonları varsayılan olarak HTTPS üzerinde çalışır ve dahil etmek için uygulamaları yapılandırma [HTTPS yeniden yönlendirmesi ve HSTS desteği](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="a1b48-231">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a1b48-232">Çağrı <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> veya <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> yöntemlerde <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="a1b48-232">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="a1b48-233">`UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır ancak daha sonra (bir varsayılan sertifika için HTTPS uç noktasının kullanılabilir olmalıdır, bu bölümde belirtilen kısıtlamalara sahip Yapılandırma).</span><span class="sxs-lookup"><span data-stu-id="a1b48-233">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="a1b48-234">ASP.NET Core 2.1 veya üzeri `KestrelServerOptions` yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="a1b48-234">ASP.NET Core 2.1 or later `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="a1b48-235">ConfigureEndpointDefaults (Eylem&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="a1b48-235">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="a1b48-236">Bir yapılandırma belirtir `Action` belirtilen her uç nokta için çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-236">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="a1b48-237">Çağırma `ConfigureEndpointDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.</span><span class="sxs-lookup"><span data-stu-id="a1b48-237">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureEndpointDefaults(configureOptions =>
            {
                configureOptions.NoDelay = true;
            });
        });
```

::: moniker-end

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="a1b48-238">ConfigureHttpsDefaults (Eylem&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="a1b48-238">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="a1b48-239">Bir yapılandırma belirtir `Action` her HTTPS uç noktası için çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-239">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="a1b48-240">Çağırma `ConfigureHttpsDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.</span><span class="sxs-lookup"><span data-stu-id="a1b48-240">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.ConfigureKestrel(serverOptions =>
        {
            serverOptions.ConfigureHttpsDefaults(options =>
            {
                // certificate is an X509Certificate2
                options.ServerCertificate = certificate;
            });
        });
        webBuilder.UseStartup<Startup>();
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ConfigureHttpsDefaults(httpsOptions =>
            {
                // certificate is an X509Certificate2
                httpsOptions.ServerCertificate = certificate;
            });
        });
```

::: moniker-end

### <a name="configureiconfiguration"></a><span data-ttu-id="a1b48-241">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="a1b48-241">Configure(IConfiguration)</span></span>

<span data-ttu-id="a1b48-242">Alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur bir <xref:Microsoft.Extensions.Configuration.IConfiguration> giriş olarak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-242">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="a1b48-243">Yapılandırma için yapılandırma bölümü için Kestrel kapsamlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-243">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="a1b48-244">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="a1b48-244">ListenOptions.UseHttps</span></span>

<span data-ttu-id="a1b48-245">Kestrel'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-245">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="a1b48-246">`ListenOptions.UseHttps` uzantılar:</span><span class="sxs-lookup"><span data-stu-id="a1b48-246">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="a1b48-247">`UseHttps` &ndash; Kestrel'i varsayılan sertifika ile HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-247">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="a1b48-248">Varsayılan Sertifika yapılandırılmışsa, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-248">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="a1b48-249">`ListenOptions.UseHttps` Parametreler:</span><span class="sxs-lookup"><span data-stu-id="a1b48-249">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="a1b48-250">`filename` uygulamanın içerik dosyaları içeren dizine göre bir sertifika dosyası yolu ve dosya adını ' dir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-250">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="a1b48-251">`password` X.509 Sertifika verilere erişmek için gerekli paroladır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-251">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="a1b48-252">`configureOptions` olan bir `Action` yapılandırmak için `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-252">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="a1b48-253">Döndürür `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-253">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="a1b48-254">`storeName` sertifika deposundan sertifikayı yüklemek için ' dir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-254">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="a1b48-255">`subject` sertifika için konu adı var.</span><span class="sxs-lookup"><span data-stu-id="a1b48-255">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="a1b48-256">`allowInvalid` Geçersiz sertifikaları, otomatik olarak imzalanan sertifikaları gibi düşünülmesi gereken olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-256">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="a1b48-257">`location` sertifikası yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-257">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="a1b48-258">`serverCertificate` X.509 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-258">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="a1b48-259">Üretim ortamında, HTTPS açıkça yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-259">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="a1b48-260">En az bir varsayılan sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-260">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="a1b48-261">Sonraki bölümde açıklandığı desteklenen yapılandırmalar:</span><span class="sxs-lookup"><span data-stu-id="a1b48-261">Supported configurations described next:</span></span>

* <span data-ttu-id="a1b48-262">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="a1b48-262">No configuration</span></span>
* <span data-ttu-id="a1b48-263">Varsayılan Sertifika yapılandırmasından değiştirin</span><span class="sxs-lookup"><span data-stu-id="a1b48-263">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="a1b48-264">Kodda Varsayılanları Değiştir</span><span class="sxs-lookup"><span data-stu-id="a1b48-264">Change the defaults in code</span></span>

<span data-ttu-id="a1b48-265">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="a1b48-265">*No configuration*</span></span>

<span data-ttu-id="a1b48-266">Kestrel'i dinlediği `http://localhost:5000` ve `https://localhost:5001` (varsayılan sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="a1b48-266">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="a1b48-267">*Varsayılan Sertifika yapılandırmasından değiştirin*</span><span class="sxs-lookup"><span data-stu-id="a1b48-267">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="a1b48-268"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-268"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="a1b48-269">Bir varsayılan HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-269">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="a1b48-270">Disk üzerindeki bir dosyadan veya bir sertifika deposu kullanmak için URL'leri ve sertifikalar dahil olmak üzere birden fazla uç noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-270">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="a1b48-271">Aşağıdaki *appsettings.json* örneği:</span><span class="sxs-lookup"><span data-stu-id="a1b48-271">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="a1b48-272">Ayarlama **AllowInvalid** için `true` geçersiz sertifikaları (örneğin, otomatik olarak imzalanan sertifikalar) izin verilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-272">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="a1b48-273">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (**HttpsDefaultCert** örnekte) bölümünde tanımlanan sertifika için geri döner **sertifikaları** > **varsayılan** veya geliştirme sertifikası.</span><span class="sxs-lookup"><span data-stu-id="a1b48-273">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
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
        "Store": "<certificate store; defaults to My>",
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

<span data-ttu-id="a1b48-274">Kullanmaya alternatif **yolu** ve **parola** herhangi bir sertifikayı sertifika deposuna alanlarını kullanarak sertifikasını belirtmek için düğümüdür.</span><span class="sxs-lookup"><span data-stu-id="a1b48-274">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="a1b48-275">Örneğin, **sertifikaları** > **varsayılan** olarak sertifika belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-275">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="a1b48-276">Şema Notlar:</span><span class="sxs-lookup"><span data-stu-id="a1b48-276">Schema notes:</span></span>

* <span data-ttu-id="a1b48-277">Uç noktaları adları büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-277">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="a1b48-278">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-278">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="a1b48-279">`Url` Parametresi her uç nokta için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-279">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="a1b48-280">Bu parametre için aynı üst düzey biçimdir `Urls` yapılandırma parametresi dışında olan sınırlı için tek bir değer.</span><span class="sxs-lookup"><span data-stu-id="a1b48-280">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="a1b48-281">Bu uç noktaları üst düzey tanımlanan değiştirin `Urls` ekleyerek bunları yerine yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="a1b48-281">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="a1b48-282">Uç noktaları aracılığıyla kod içinde tanımlanan `Listen` yapılandırma bölümünde tanımlanan uç noktaları ile bunların toplamı olur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-282">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="a1b48-283">`Certificate` Bölümüne, isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-283">The `Certificate` section is optional.</span></span> <span data-ttu-id="a1b48-284">Varsa `Certificate` bölüm belirtilmediyse, önceki senaryoda tanımlanan varsayılan değerler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-284">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="a1b48-285">Varsayılan değer mevcutsa, sunucunun bir özel durum oluşturur ve başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-285">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="a1b48-286">`Certificate` Bölümü destekler **yolu**&ndash;**parola** ve **konu**&ndash;**Store** sertifikalar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-286">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="a1b48-287">Bağlantı noktası çakışmalara neden olmayan sürece herhangi bir sayıda uç noktaları bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-287">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="a1b48-288">`options.Configure(context.Configuration.GetSection("Kestrel"))` döndürür bir `KestrelConfigurationLoader` ile bir `.Endpoint(string name, options => { })` yapılandırılmış bir uç noktanın ayarları desteklemek için kullanılan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a1b48-288">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="a1b48-289">Ayrıca doğrudan erişebilirsiniz `KestrelServerOptions.ConfigurationLoader` tarafından sağlanan gibi mevcut yükleyiciyi yineleme tutmak için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="a1b48-289">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="a1b48-290">Her uç nokta yapılandırma bölümü bir seçenekler kullanılabilir `Endpoint` yöntemi böylece özel ayarlarını okuyun.</span><span class="sxs-lookup"><span data-stu-id="a1b48-290">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="a1b48-291">Birden fazla yapılandırması çağırarak yüklenmemiş olabilir `options.Configure(context.Configuration.GetSection("Kestrel"))` yeniden başka bir bölüme sahip.</span><span class="sxs-lookup"><span data-stu-id="a1b48-291">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="a1b48-292">Sürece yalnızca son yapılandırma kullanıldığını `Load` önceki örnekleri üzerinde açıkça çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-292">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="a1b48-293">Metapackage çağrı değil `Load` böylece kendi varsayılan yapılandırma bölümü değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-293">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="a1b48-294">`KestrelConfigurationLoader` yansıtmalar `Listen` API'lerinden ailesi `KestrelServerOptions` olarak `Endpoint` yüklemeleri için aynı yerde kodda ve yapılandırma uç noktaları yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-294">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="a1b48-295">Bu aşırı yüklemeler olmayan adlar kullanın ve yalnızca varsayılan yapılandırma ayarlarından kullanma.</span><span class="sxs-lookup"><span data-stu-id="a1b48-295">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="a1b48-296">*Kodda Varsayılanları Değiştir*</span><span class="sxs-lookup"><span data-stu-id="a1b48-296">*Change the defaults in code*</span></span>

<span data-ttu-id="a1b48-297">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` varsayılan ayarlarını değiştirmek için kullanılan `ListenOptions` ve `HttpsConnectionAdapterOptions`, önceki senaryoda belirtilen varsayılan sertifika geçersiz kılma dahil.</span><span class="sxs-lookup"><span data-stu-id="a1b48-297">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="a1b48-298">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` tüm uç noktaları yapılandırılmış önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-298">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="a1b48-299">*SNI kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="a1b48-299">*Kestrel support for SNI*</span></span>

<span data-ttu-id="a1b48-300">[Sunucu adı belirtme (SNI)](https://tools.ietf.org/html/rfc6066#section-3) birden çok etki alanı aynı IP adresini ve bağlantı noktası üzerinde barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-300">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="a1b48-301">Sunucunun doğru sertifikayı sağlayabilmesi işlevine SNI için TLS anlaşması sırasında sunucusuna güvenli oturum için konak adı istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-301">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="a1b48-302">İstemci, TLS el sıkışma izleyen güvenli oturum sırasında sunucu ile şifreli iletişim için furnished sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-302">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="a1b48-303">SNI aracılığıyla kestrel destekler `ServerCertificateSelector` geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="a1b48-303">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="a1b48-304">Geri çağırma ana bilgisayar adını denetleyin ve uygun sertifikayı seçmek izin vermek için bağlantı bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-304">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="a1b48-305">SNI desteği gerektirir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-305">SNI support requires:</span></span>

* <span data-ttu-id="a1b48-306">Hedef framework üzerinde çalışan `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-306">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="a1b48-307">Üzerinde `netcoreapp2.0` ve `net461`, geri çağırma çağrılır ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-307">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="a1b48-308">`name` De `null` istemci TLS anlaşması name parametresinde konak sağlamıyorsa.</span><span class="sxs-lookup"><span data-stu-id="a1b48-308">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="a1b48-309">Tüm Web sitelerinin aynı Kestrel örneğinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-309">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="a1b48-310">Kestrel'i ters Ara sunucu olmadan birden çok örneğinde bir IP adresi ve bağlantı noktası paylaşımı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a1b48-310">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
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

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="a1b48-311">Bir TCP yuva için bağlama</span><span class="sxs-lookup"><span data-stu-id="a1b48-311">Bind to a TCP socket</span></span>

<span data-ttu-id="a1b48-312"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> Yöntemi için bir TCP yuva bağlar ve X.509 Sertifika yapılandırma seçenekleri lambda verir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-312">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
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
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

<span data-ttu-id="a1b48-313">Örnek, bir uç nokta için HTTPS yapılandırır <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="a1b48-313">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="a1b48-314">Özel uç noktaları diğer Kestrel ayarlarını yapılandırmak için aynı API kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-314">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="a1b48-315">Bir UNIX yuvası için bağlama</span><span class="sxs-lookup"><span data-stu-id="a1b48-315">Bind to a Unix socket</span></span>

<span data-ttu-id="a1b48-316">Bir UNIX yuvasıyla dinleyecek <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> Bu örnekte gösterildiği gibi Ngınx ile Gelişmiş performans için:</span><span class="sxs-lookup"><span data-stu-id="a1b48-316">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a><span data-ttu-id="a1b48-317">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="a1b48-317">Port 0</span></span>

<span data-ttu-id="a1b48-318">Zaman bağlantı noktası numarasını `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-318">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="a1b48-319">Aşağıdaki örnek, Kestrel çalışma zamanında gerçekten bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-319">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="a1b48-320">Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-320">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="a1b48-321">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="a1b48-321">Limitations</span></span>

<span data-ttu-id="a1b48-322">Uç noktaları ile aşağıdaki yaklaşımlardan yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="a1b48-322">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="a1b48-323">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="a1b48-323">`--urls` command-line argument</span></span>
* <span data-ttu-id="a1b48-324">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="a1b48-324">`urls` host configuration key</span></span>
* <span data-ttu-id="a1b48-325">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="a1b48-325">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="a1b48-326">Bu yöntemler, kod Kestrel dışında sunucuları ile iş yapmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-326">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="a1b48-327">Ancak, aşağıdaki sınırlamaları unutmayın:</span><span class="sxs-lookup"><span data-stu-id="a1b48-327">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="a1b48-328">HTTPS varsayılan sertifikayı HTTPS uç nokta yapılandırmasında sağlanmadığı sürece bu yaklaşımların ile kullanılamaz (örneğin, kullanarak `KestrelServerOptions` yapılandırma veya bu konuda daha önce gösterildiği gibi bir yapılandırma dosyası).</span><span class="sxs-lookup"><span data-stu-id="a1b48-328">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="a1b48-329">Hem `Listen` ve `UseUrls` yaklaşımları eşzamanlı olarak kullanılan `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.</span><span class="sxs-lookup"><span data-stu-id="a1b48-329">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="a1b48-330">IIS bitiş noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a1b48-330">IIS endpoint configuration</span></span>

<span data-ttu-id="a1b48-331">IIS geçersiz kılmak için IIS, URL bağlamaları kullanırken bağlamalar tarafından ayarlanan `Listen` veya `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-331">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="a1b48-332">Daha fazla bilgi için [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) konu.</span><span class="sxs-lookup"><span data-stu-id="a1b48-332">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="a1b48-333">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="a1b48-333">ListenOptions.Protocols</span></span>

<span data-ttu-id="a1b48-334">`Protocols` Özelliği kurar HTTP protokollerini (`HttpProtocols`) bir bağlantı uç noktası veya sunucu için etkin.</span><span class="sxs-lookup"><span data-stu-id="a1b48-334">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="a1b48-335">Bir değer atayın `Protocols` özelliğinden `HttpProtocols` sabit listesi.</span><span class="sxs-lookup"><span data-stu-id="a1b48-335">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="a1b48-336">`HttpProtocols` Sabit listesi değeri</span><span class="sxs-lookup"><span data-stu-id="a1b48-336">`HttpProtocols` enum value</span></span> | <span data-ttu-id="a1b48-337">İzin verilen bağlantı protokolü</span><span class="sxs-lookup"><span data-stu-id="a1b48-337">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="a1b48-338">HTTP/1.1 yalnızca.</span><span class="sxs-lookup"><span data-stu-id="a1b48-338">HTTP/1.1 only.</span></span> <span data-ttu-id="a1b48-339">İle veya olmadan TLS kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-339">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="a1b48-340">HTTP/2 yalnızca.</span><span class="sxs-lookup"><span data-stu-id="a1b48-340">HTTP/2 only.</span></span> <span data-ttu-id="a1b48-341">TLS ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-341">Primarily used with TLS.</span></span> <span data-ttu-id="a1b48-342">Yalnızca istemci uygulamalarını destekliyorsa, TLS kullanılabilir bir [bilgisi modu](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="a1b48-342">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="a1b48-343">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="a1b48-343">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="a1b48-344">Bir TLS gerektirir ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı HTTP/2; anlaşmak üzere bağlantı, HTTP/1.1 Aksi takdirde, varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-344">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="a1b48-345">Varsayılan, HTTP/1.1 protokolüdür.</span><span class="sxs-lookup"><span data-stu-id="a1b48-345">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="a1b48-346">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="a1b48-346">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="a1b48-347">TLS 1.2 veya sonraki bir sürümü</span><span class="sxs-lookup"><span data-stu-id="a1b48-347">TLS version 1.2 or later</span></span>
* <span data-ttu-id="a1b48-348">Devre dışı yeniden anlaşma</span><span class="sxs-lookup"><span data-stu-id="a1b48-348">Renegotiation disabled</span></span>
* <span data-ttu-id="a1b48-349">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="a1b48-349">Compression disabled</span></span>
* <span data-ttu-id="a1b48-350">En düşük kısa ömürlü anahtar değişimi boyutları:</span><span class="sxs-lookup"><span data-stu-id="a1b48-350">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="a1b48-351">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 BITS en düşük</span><span class="sxs-lookup"><span data-stu-id="a1b48-351">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="a1b48-352">Sınırlı alanda Diffie-Hellman (DHE) &lbrack; `TLS12` &rbrack; &ndash; en az 2048 bit</span><span class="sxs-lookup"><span data-stu-id="a1b48-352">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="a1b48-353">Şifre paketini değil kara listede</span><span class="sxs-lookup"><span data-stu-id="a1b48-353">Cipher suite not blacklisted</span></span>

<span data-ttu-id="a1b48-354">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; p-256 Eliptik Eğri ile &lbrack; `FIPS186` &rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-354">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="a1b48-355">Aşağıdaki örnek, HTTP/1.1 ve 8000 numaralı bağlantı noktasındaki HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-355">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="a1b48-356">Bağlantılar TLS tarafından sağlanan bir sertifika ile güvenli hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-356">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="a1b48-357">İsteğe bağlı olarak bir `IConnectionAdapter` TLS el sıkışma belirli şifrelemeleri için bağlantı başına temelinde filtre uygulamak için uygulama:</span><span class="sxs-lookup"><span data-stu-id="a1b48-357">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
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

        // Throw NotSupportedException for any cipher algorithm that you don't 
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

<span data-ttu-id="a1b48-358">*Protokol yapılandırmasını ayarlayın*</span><span class="sxs-lookup"><span data-stu-id="a1b48-358">*Set the protocol from configuration*</span></span>

<span data-ttu-id="a1b48-359"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="a1b48-359"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="a1b48-360">Aşağıdaki *appsettings.json* örnek, bir varsayılan bağlantı protokol (HTTP/1.1 ve HTTP/2) kurulmuş tüm Kestrel'ın uç noktaları için:</span><span class="sxs-lookup"><span data-stu-id="a1b48-360">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="a1b48-361">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç noktası için bir bağlantı protokol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="a1b48-361">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="a1b48-362">Kodda belirtilen protokoller, yapılandırma tarafından ayarlanan değerleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="a1b48-362">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

## <a name="transport-configuration"></a><span data-ttu-id="a1b48-363">Aktarım yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a1b48-363">Transport configuration</span></span>

<span data-ttu-id="a1b48-364">ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre.</span><span class="sxs-lookup"><span data-stu-id="a1b48-364">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="a1b48-365">Bu, bir ASP.NET Core 2.0 uygulamaları çağıran 2.1 yükseltme için değişiklik <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> ve aşağıdaki paketlerden birini bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="a1b48-365">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="a1b48-366">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket Başvurusu)</span><span class="sxs-lookup"><span data-stu-id="a1b48-366">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="a1b48-367">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="a1b48-367">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="a1b48-368">ASP.NET Core 2.1 veya üzerini kullanan projeleri [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve Libuv kullanılmasını gerektirir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-368">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="a1b48-369">İçin bağımlılık ekleme [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) uygulamanın proje dosyasına paket:</span><span class="sxs-lookup"><span data-stu-id="a1b48-369">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="a1b48-370">Çağrı <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="a1b48-370">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="a1b48-371">URL ön ekleri</span><span class="sxs-lookup"><span data-stu-id="a1b48-371">URL prefixes</span></span>

<span data-ttu-id="a1b48-372">Kullanırken `UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni URL ön ekleri olabilir aşağıdaki biçimlerden birini.</span><span class="sxs-lookup"><span data-stu-id="a1b48-372">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="a1b48-373">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-373">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="a1b48-374">Kullanarak URL bağlamaları yapılandırma sırasında kestrel HTTPS desteklemiyor `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-374">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="a1b48-375">Bağlantı noktası numarası ile IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="a1b48-375">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="a1b48-376">`0.0.0.0` tüm IPv4 adreslerine bağlayan bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-376">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="a1b48-377">Bağlantı noktası numarası ile IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="a1b48-377">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="a1b48-378">`[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="a1b48-378">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="a1b48-379">Bağlantı noktası numarası ile ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="a1b48-379">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="a1b48-380">Ana makine adları, `*`, ve `+`, özel değildir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-380">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="a1b48-381">Herhangi bir şey geçerli bir IP adresi tanınmıyor veya `localhost` tüm IPv4 ve IPv6 IP bağlar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-381">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="a1b48-382">Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [HTTP.sys](xref:fundamentals/servers/httpsys) veya IIS, Ngınx veya Apache gibi bir ters proxy sunucusu.</span><span class="sxs-lookup"><span data-stu-id="a1b48-382">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="a1b48-383">Ters proxy yapılandırmasında barındırma gerektirir [filtreleme konak](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="a1b48-383">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="a1b48-384">Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad</span><span class="sxs-lookup"><span data-stu-id="a1b48-384">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="a1b48-385">Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-385">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="a1b48-386">İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a1b48-386">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="a1b48-387">Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a1b48-387">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="a1b48-388">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="a1b48-388">Host filtering</span></span>

<span data-ttu-id="a1b48-389">Kestrel'i yapılandırma gibi önekleri temel alarak desteklerken `http://example.com:5000`, Kestrel büyük ölçüde ana bilgisayar adı yok sayar.</span><span class="sxs-lookup"><span data-stu-id="a1b48-389">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="a1b48-390">Konak `localhost` bağlama geri döngü adresi için kullanılan özel bir durum.</span><span class="sxs-lookup"><span data-stu-id="a1b48-390">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="a1b48-391">Daha açık bir IP adresi tüm genel IP adresine bağlar herhangi diğer barındırın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-391">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="a1b48-392">`Host` üst bilgileri doğrulanmış değil.</span><span class="sxs-lookup"><span data-stu-id="a1b48-392">`Host` headers aren't validated.</span></span>

<span data-ttu-id="a1b48-393">Geçici çözüm olarak, konak filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a1b48-393">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="a1b48-394">Konak filtreleme ara yazılımı tarafından sağlanan [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="a1b48-394">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="a1b48-395">Ara yazılım tarafından eklenen <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, çağıran <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="a1b48-395">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="a1b48-396">Konak filtreleme ara yazılım, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-396">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="a1b48-397">Ara yazılım etkinleştirmek için tanımladığınız bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="a1b48-397">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="a1b48-398">Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:</span><span class="sxs-lookup"><span data-stu-id="a1b48-398">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="a1b48-399">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a1b48-399">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="a1b48-400">[Üst bilgileri ara yazılım iletilen](xref:host-and-deploy/proxy-load-balancer) de sahip bir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a1b48-400">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="a1b48-401">İletilen üstbilgileri ara yazılım ve konak filtreleme ara yazılım farklı senaryolar için benzer bir işlevsellik vardır.</span><span class="sxs-lookup"><span data-stu-id="a1b48-401">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="a1b48-402">Ayarı `AllowedHosts` iletilen üstbilgileri Ara yazılımla uygundur `Host` üstbilgi ters Ara sunucu veya yük dengeleyici isteklerle iletirken korunur değil.</span><span class="sxs-lookup"><span data-stu-id="a1b48-402">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="a1b48-403">Ayarı `AllowedHosts` konak filtreleme Ara yazılımla uygundur Kestrel genel kullanıma yönelik bir uç sunucusu kullanıldığında veya zaman `Host` başlığı doğrudan iletilir.</span><span class="sxs-lookup"><span data-stu-id="a1b48-403">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="a1b48-404">İletilen üstbilgileri ara yazılım hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="a1b48-404">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a1b48-405">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a1b48-405">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="a1b48-406">Kestrel'i kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="a1b48-406">Kestrel source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Servers/Kestrel)
* [<span data-ttu-id="a1b48-407">RFC 7230: İleti söz dizimi ve yönlendirme (Bölüm 5.4: Ana bilgisayarı)</span><span class="sxs-lookup"><span data-stu-id="a1b48-407">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
