---
title: ASP.NET core'da kestrel web sunucusu uygulaması
author: rick-anderson
description: Kestrel'i, ASP.NET Core için platformlar arası web sunucusu hakkında bilgi edinin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 218b6429462991659ba02804fdc8fbb99b69f1a6
ms.sourcegitcommit: 599ebae5c2d6fcb22dfa6ae7d1f4bdfcacb79af4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47211097"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="63def-103">ASP.NET core'da kestrel web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="63def-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="63def-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="63def-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="63def-105">Kestrel'i olduğu bir platformlar arası [ASP.NET Core web sunucusu](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="63def-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="63def-106">Kestrel'i ASP.NET Core proje şablonları, varsayılan olarak bulunan bir web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="63def-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="63def-107">Kestrel'i aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="63def-107">Kestrel supports the following features:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="63def-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="63def-108">HTTPS</span></span>
* <span data-ttu-id="63def-109">Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="63def-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="63def-110">Yüksek performans Ngınx arkasında UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="63def-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="63def-111">HTTP/2 (Macos'ta dışındaki&dagger;)</span><span class="sxs-lookup"><span data-stu-id="63def-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="63def-112">&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="63def-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="63def-113">HTTPS</span><span class="sxs-lookup"><span data-stu-id="63def-113">HTTPS</span></span>
* <span data-ttu-id="63def-114">Donuk yükseltme etkinleştirmek için kullanılan [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="63def-114">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="63def-115">Yüksek performans Ngınx arkasında UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="63def-115">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="63def-116">Kestrel'i tüm platformlarda ve .NET Core destekleyen sürümler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="63def-116">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="63def-117">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63def-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="63def-118">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="63def-118">HTTP/2 support</span></span>

<span data-ttu-id="63def-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki gereksinimleri dayandırırsanız ASP.NET Core uygulamaları karşılanması için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="63def-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="63def-120">İşletim Sistemi&dagger;</span><span class="sxs-lookup"><span data-stu-id="63def-120">Operating system&dagger;</span></span>
  * <span data-ttu-id="63def-121">Windows Server 2012 R2/Windows 8.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="63def-121">Windows Server 2012 R2/Windows 8.1 or later</span></span>
  * <span data-ttu-id="63def-122">Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="63def-122">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="63def-123">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="63def-123">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="63def-124">[Uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı</span><span class="sxs-lookup"><span data-stu-id="63def-124">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="63def-125">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="63def-125">TLS 1.2 or later connection</span></span>

<span data-ttu-id="63def-126">&dagger;HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="63def-126">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="63def-127">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="63def-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="63def-128">HTTP/2 varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="63def-129">Yapılandırma hakkında daha fazla bilgi için bkz. [Kestrel seçenekleri](#kestrel-options) ve [uç nokta Yapılandırması](#endpoint-configuration) bölümler.</span><span class="sxs-lookup"><span data-stu-id="63def-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [Endpoint configuration](#endpoint-configuration) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="63def-130">Ne zaman Kestrel ters Ara sunucu ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63def-130">When to use Kestrel with a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-131">Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters Ara sunucu*IIS, Ngınx veya Apache gibi.</span><span class="sxs-lookup"><span data-stu-id="63def-131">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="63def-132">Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="63def-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="63def-135">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="63def-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="63def-136">Bir uygulama yalnızca bir iç ağ gelen istekleri kabul ederse Kestrel doğrudan uygulamanın sunucusu olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-136">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel'i doğrudan iç ağınıza ile iletişim kurar.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="63def-138">Uygulamanıza Internet kullanıma sunma, IIS, Ngınx veya Apache olarak kullanın. bir *ters Ara sunucu*.</span><span class="sxs-lookup"><span data-stu-id="63def-138">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="63def-139">Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="63def-139">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="63def-141">Güvenlik nedenleriyle edge dağıtımları (trafiği Internet'ten kullanıma sunulur) için ters Ara sunucu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="63def-141">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="63def-142">Kestrel'i 1.x sürümlerini tamamlayıcı uygun zaman aşımları, boyut sınırları ve eş zamanlı bağlantı sınırları gibi saldırılara karşı savunma yoktur.</span><span class="sxs-lookup"><span data-stu-id="63def-142">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="63def-143">Aynı IP adresini ve bağlantı noktası tek bir sunucu üzerinde çalışan paylaşan birden çok uygulama olduğunda bir ters proxy senaryosu bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="63def-143">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="63def-144">Aynı IP adresini ve bağlantı noktası arasında birden çok işlem paylaşımı Kestrel desteklemediğinden bu senaryo kestrel desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="63def-144">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="63def-145">Kestrel'i bir bağlantı noktasında dinleyecek şekilde yapılandırıldığında, Kestrel tüm istekleri ana bilgisayar üstbilgisi bağımsız olarak bu bağlantı noktası trafiğini işler.</span><span class="sxs-lookup"><span data-stu-id="63def-145">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="63def-146">Bağlantı noktalarını paylaşan bir ters proxy Kestrel benzersiz bir IP ve bağlantı noktası isteklerini iletmek için özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="63def-146">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="63def-147">Ters proxy sunucusu gerekli olmasa bile bir ters proxy sunucusu kullanarak iyi bir seçim olabilir:</span><span class="sxs-lookup"><span data-stu-id="63def-147">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="63def-148">Bu, barındırdığı uygulamaları kullanıma sunulan ortak yüzey alanını sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63def-148">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="63def-149">Bu, ek bir yapılandırma ve koruma katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="63def-149">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="63def-150">Daha iyi var olan altyapınızla tümleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63def-150">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="63def-151">Yük Dengeleme ve SSL yapılandırmasını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="63def-151">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="63def-152">Ters proxy sunucusu yalnızca bir SSL sertifikası gerektirir ve bu sunucu, düz HTTP kullanarak iç ağ üzerinde uygulama sunucularınız ile iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-152">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="63def-153">Etkin bir ters proxy filtreleme konakla kullanılmıyorsa [filtreleme konak](#host-filtering) etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="63def-153">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="63def-154">ASP.NET Core uygulamalarında Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="63def-154">How to use Kestrel in ASP.NET Core apps</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-155">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="63def-155">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="63def-156">ASP.NET Core proje şablonları, varsayılan olarak Kestrel kullanın.</span><span class="sxs-lookup"><span data-stu-id="63def-156">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="63def-157">İçinde *Program.cs*, şablon kod çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), çağıran [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) arka planda.</span><span class="sxs-lookup"><span data-stu-id="63def-157">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="63def-158">Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, kullanın `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="63def-158">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

<span data-ttu-id="63def-159">Arama sonra ek bir yapılandırma sağlamak üzere `CreateDefaultBuilder`, çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="63def-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="63def-160">Yükleme [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="63def-160">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="63def-161">Çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) genişletme yöntemini [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) içinde `Main` herhangi belirtme yöntemi [Kestrel seçenekleri](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) , sonraki bölümde gösterildiği gibi gerekli.</span><span class="sxs-lookup"><span data-stu-id="63def-161">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="63def-162">Kestrel'i seçenekleri</span><span class="sxs-lookup"><span data-stu-id="63def-162">Kestrel options</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-163">Kestrel'i web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı olan kısıtlaması yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="63def-163">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="63def-164">Özelleştirilebilir birkaç önemli sınırları:</span><span class="sxs-lookup"><span data-stu-id="63def-164">A few important limits that can be customized:</span></span>

* <span data-ttu-id="63def-165">En fazla istemci bağlantısı</span><span class="sxs-lookup"><span data-stu-id="63def-165">Maximum client connections</span></span>
* <span data-ttu-id="63def-166">En fazla istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="63def-166">Maximum request body size</span></span>
* <span data-ttu-id="63def-167">En az bir istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="63def-167">Minimum request body data rate</span></span>

<span data-ttu-id="63def-168">Bunlar ve diğer kısıtlamaları ayarlamak [sınırları](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) özelliği [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="63def-168">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="63def-169">`Limits` Özelliği bir örneğini tutan [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="63def-169">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="63def-170">En fazla istemci bağlantısı</span><span class="sxs-lookup"><span data-stu-id="63def-170">Maximum client connections</span></span>

[<span data-ttu-id="63def-171">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="63def-171">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="63def-172">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="63def-172">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

<span data-ttu-id="63def-173">Aşağıdaki kod ile tüm uygulama için eşzamanlı açık TCP bağlantıları sayısı ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="63def-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

<span data-ttu-id="63def-174">HTTP veya HTTPS, başka bir protokol (örneğin, WebSockets istek üzerine) a yükseltilmiştir bağlantıları için ayrı bir sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="63def-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="63def-175">Bir bağlantı yükseltildikten sonra karşı sayılır değil `MaxConcurrentConnections` sınırı.</span><span class="sxs-lookup"><span data-stu-id="63def-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-176">Bağlantı sayısı, varsayılan olarak sınırsız (null) olur.</span><span class="sxs-lookup"><span data-stu-id="63def-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="63def-177">En fazla istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="63def-177">Maximum request body size</span></span>

[<span data-ttu-id="63def-178">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="63def-178">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="63def-179">Varsayılan en büyük istek gövdesi boyutu 30.000.000, yaklaşık 28.6 MB bayttır.</span><span class="sxs-lookup"><span data-stu-id="63def-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="63def-180">Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yaklaşımdır [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) özniteliği bir eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="63def-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

<span data-ttu-id="63def-181">Her istek için uygulama kısıtlama yapılandırma gösteren bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="63def-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="63def-182">Belirli bir istekte Ara yazılımında ayarı geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="63def-182">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-183">Uygulama isteği okumak başlatıldıktan sonra bir istekte sınırını yapılandırmak çalışırsanız, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63def-183">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="63def-184">Var. bir `IsReadOnly` gösterir özelliği `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="63def-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="63def-185">En az bir istek gövdesi veri hızı</span><span class="sxs-lookup"><span data-stu-id="63def-185">Minimum request body data rate</span></span>

[<span data-ttu-id="63def-186">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="63def-186">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="63def-187">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="63def-187">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="63def-188">Veri içinde belirtilen hızda bayt/saniye gelen, saniyede kestrel denetler.</span><span class="sxs-lookup"><span data-stu-id="63def-188">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="63def-189">Hızı en düşerse, bağlantının zaman aşımına uğradı. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme hızını artırmak için istemci verdiğini süreyi belirtir; Bu süre boyunca oranı işaretlenmemiş.</span><span class="sxs-lookup"><span data-stu-id="63def-189">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="63def-190">Yetkisiz kullanım süresi, başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderen bağlantıları verilmemesini yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="63def-190">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="63def-191">Varsayılan en az 5 saniye yetkisiz kullanım süresi ile 240 bayt/saniye oranıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-191">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="63def-192">En düşük bir ücretle yanıt için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="63def-192">A minimum rate also applies to the response.</span></span> <span data-ttu-id="63def-193">İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynıdır `RequestBody` veya `Response` özelliği ve arabirimi adları.</span><span class="sxs-lookup"><span data-stu-id="63def-193">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="63def-194">En az veriyi hızları yapılandırma gösteren bir örnek aşağıdadır *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="63def-194">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="63def-195">İstek başına ücretler Ara yazılımında yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="63def-195">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="63def-196">Bağlantı başına en fazla akış</span><span class="sxs-lookup"><span data-stu-id="63def-196">Maximum streams per connection</span></span>

<span data-ttu-id="63def-197">`Http2.MaxStreamsPerConnection` HTTP/2 bağlantı başına akış eş zamanlı istek sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="63def-197">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="63def-198">Aşırı akışları çevrilir.</span><span class="sxs-lookup"><span data-stu-id="63def-198">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="63def-199">Varsayılan değer 100’dür.</span><span class="sxs-lookup"><span data-stu-id="63def-199">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="63def-200">Üst bilgi tablosu boyutu</span><span class="sxs-lookup"><span data-stu-id="63def-200">Header table size</span></span>

<span data-ttu-id="63def-201">HPACK kod çözücü, HTTP/2 bağlantılar için HTTP üstbilgileri açar.</span><span class="sxs-lookup"><span data-stu-id="63def-201">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="63def-202">`Http2.HeaderTableSize` HPACK kod çözücü kullanan üst bilgi sıkıştırma tablonun boyutunu sınırlar.</span><span class="sxs-lookup"><span data-stu-id="63def-202">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="63def-203">Değer, sekizlik tabanda sağlanır ve sıfır (0) büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-203">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="63def-204">Varsayılan değer 4096'dır.</span><span class="sxs-lookup"><span data-stu-id="63def-204">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="63def-205">En büyük çerçeve boyutu</span><span class="sxs-lookup"><span data-stu-id="63def-205">Maximum frame size</span></span>

<span data-ttu-id="63def-206">`Http2.MaxFrameSize` en büyük boyutunu almak için HTTP/2 bağlantı çerçeve yükü gösterir.</span><span class="sxs-lookup"><span data-stu-id="63def-206">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="63def-207">Değer sekizlik tabanda sağlanır ve 2 arasında olmalıdır ^ (16,384) 14. ve 2 ^ 24-1 (16.777.215).</span><span class="sxs-lookup"><span data-stu-id="63def-207">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="63def-208">Varsayılan değer olan 2 ^ 14 (16,384).</span><span class="sxs-lookup"><span data-stu-id="63def-208">The default value is 2^14 (16,384).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-209">Diğer seçenekleri Kestrel ve sınırları hakkında daha fazla bilgi için bkz:</span><span class="sxs-lookup"><span data-stu-id="63def-209">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="63def-210">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="63def-210">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="63def-211">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="63def-211">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="63def-212">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="63def-212">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="63def-213">Kestrel'i seçenekleri ve sınırları hakkında daha fazla bilgi için bkz:</span><span class="sxs-lookup"><span data-stu-id="63def-213">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="63def-214">KestrelServerOptions sınıfı</span><span class="sxs-lookup"><span data-stu-id="63def-214">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="63def-215">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="63def-215">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a><span data-ttu-id="63def-216">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="63def-216">Endpoint configuration</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="63def-217">Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="63def-217">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="63def-218">Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlerde [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="63def-218">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="63def-219">`UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır, ancak daha sonra bu bölümde belirtilen kısıtlamalara sahip.</span><span class="sxs-lookup"><span data-stu-id="63def-219">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="63def-220">`urls` Ana bilgisayar yapılandırma anahtarı konak yapılandırması, uygulama yapılandırmasını gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="63def-220">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="63def-221">Ekleme bir `urls` anahtarı ve değeri *appsettings.json* konak yapılandırması, yapılandırma dosyasından okunur zamanı tarafından tamamen başlatılmış olduğundan ana bilgisayar yapılandırması etkilemez.</span><span class="sxs-lookup"><span data-stu-id="63def-221">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="63def-222">Ancak, bir `urls` anahtarını *appsettings.json* kullanılabilir [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) konağı yapılandırmak için konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="63def-222">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="63def-223">Varsayılan olarak, ASP.NET Core bağlar:</span><span class="sxs-lookup"><span data-stu-id="63def-223">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="63def-224">`https://localhost:5001` (yerel geliştirme sertifikası mevcut olduğunda)</span><span class="sxs-lookup"><span data-stu-id="63def-224">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="63def-225">Bir geliştirme sertifikası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="63def-225">A development certificate is created:</span></span>

* <span data-ttu-id="63def-226">Zaman [.NET Core SDK'sı](/dotnet/core/sdk) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="63def-226">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="63def-227">[Dev-certs aracını](xref:aspnetcore-2.1#https) bir sertifika oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63def-227">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="63def-228">Bazı tarayıcılar, tarayıcının yerel geliştirme sertifikasına güvenmek açık izin vermenizi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63def-228">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="63def-229">ASP.NET Core 2.1 ve üzeri proje şablonları varsayılan olarak HTTPS üzerinde çalışır ve dahil etmek için uygulamaları yapılandırma [HTTPS yeniden yönlendirmesi ve HSTS desteği](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="63def-229">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="63def-230">Çağrı [dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) veya [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) yöntemlerde [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) Kestrel için URL ön ekleri ve bağlantı noktalarını yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="63def-230">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="63def-231">`UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı ve `ASPNETCORE_URLS` ortam değişkeni de çalışır ancak daha sonra (bir varsayılan sertifika için HTTPS uç noktasının kullanılabilir olmalıdır, bu bölümde belirtilen kısıtlamalara sahip Yapılandırma).</span><span class="sxs-lookup"><span data-stu-id="63def-231">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="63def-232">ASP.NET Core 2.1 `KestrelServerOptions` yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="63def-232">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="63def-233">ConfigureEndpointDefaults (Eylem&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="63def-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="63def-234">Bir yapılandırma belirtir `Action` belirtilen her uç nokta için çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="63def-234">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="63def-235">Çağırma `ConfigureEndpointDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.</span><span class="sxs-lookup"><span data-stu-id="63def-235">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="63def-236">ConfigureHttpsDefaults (Eylem&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="63def-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="63def-237">Bir yapılandırma belirtir `Action` her HTTPS uç noktası için çalıştırılacak.</span><span class="sxs-lookup"><span data-stu-id="63def-237">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="63def-238">Çağırma `ConfigureHttpsDefaults` birden çok kez önceki değiştirir `Action`son s `Action` belirtilen.</span><span class="sxs-lookup"><span data-stu-id="63def-238">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="63def-239">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="63def-239">Configure(IConfiguration)</span></span>

<span data-ttu-id="63def-240">Alan Kestrel ayarlamak için bir yapılandırma yükleyicisi oluşturur bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) giriş olarak.</span><span class="sxs-lookup"><span data-stu-id="63def-240">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="63def-241">Yapılandırma için yapılandırma bölümü için Kestrel kapsamlandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-241">The configuration must be scoped to the configuration section for Kestrel.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="listenoptionsusehttps"></a><span data-ttu-id="63def-242">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="63def-242">ListenOptions.UseHttps</span></span>

<span data-ttu-id="63def-243">Kestrel'i HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="63def-243">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="63def-244">`ListenOptions.UseHttps` uzantılar:</span><span class="sxs-lookup"><span data-stu-id="63def-244">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="63def-245">`UseHttps` &ndash; Kestrel'i varsayılan sertifika ile HTTPS kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="63def-245">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="63def-246">Varsayılan Sertifika yapılandırılmışsa, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63def-246">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="63def-247">`ListenOptions.UseHttps` Parametreler:</span><span class="sxs-lookup"><span data-stu-id="63def-247">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="63def-248">`filename` uygulamanın içerik dosyaları içeren dizine göre bir sertifika dosyası yolu ve dosya adını ' dir.</span><span class="sxs-lookup"><span data-stu-id="63def-248">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="63def-249">`password` X.509 Sertifika verilere erişmek için gerekli paroladır.</span><span class="sxs-lookup"><span data-stu-id="63def-249">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="63def-250">`configureOptions` olan bir `Action` yapılandırmak için `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="63def-250">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="63def-251">Döndürür `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="63def-251">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="63def-252">`storeName` sertifika deposundan sertifikayı yüklemek için ' dir.</span><span class="sxs-lookup"><span data-stu-id="63def-252">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="63def-253">`subject` sertifika için konu adı var.</span><span class="sxs-lookup"><span data-stu-id="63def-253">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="63def-254">`allowInvalid` Geçersiz sertifikaları, otomatik olarak imzalanan sertifikaları gibi düşünülmesi gereken olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="63def-254">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="63def-255">`location` sertifikası yüklemek için depo konumudur.</span><span class="sxs-lookup"><span data-stu-id="63def-255">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="63def-256">`serverCertificate` X.509 sertifikasıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-256">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="63def-257">Üretim ortamında, HTTPS açıkça yapılandırılmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63def-257">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="63def-258">En az bir varsayılan sertifika sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-258">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="63def-259">Sonraki bölümde açıklandığı desteklenen yapılandırmalar:</span><span class="sxs-lookup"><span data-stu-id="63def-259">Supported configurations described next:</span></span>

* <span data-ttu-id="63def-260">Yapılandırma yok</span><span class="sxs-lookup"><span data-stu-id="63def-260">No configuration</span></span>
* <span data-ttu-id="63def-261">Varsayılan Sertifika yapılandırmasından değiştirin</span><span class="sxs-lookup"><span data-stu-id="63def-261">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="63def-262">Kodda Varsayılanları Değiştir</span><span class="sxs-lookup"><span data-stu-id="63def-262">Change the defaults in code</span></span>

<span data-ttu-id="63def-263">*Yapılandırma yok*</span><span class="sxs-lookup"><span data-stu-id="63def-263">*No configuration*</span></span>

<span data-ttu-id="63def-264">Kestrel'i dinlediği `http://localhost:5000` ve `https://localhost:5001` (varsayılan sertifika varsa).</span><span class="sxs-lookup"><span data-stu-id="63def-264">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="63def-265">Kullanarak URL'leri belirtin:</span><span class="sxs-lookup"><span data-stu-id="63def-265">Specify URLs using the:</span></span>

* <span data-ttu-id="63def-266">`ASPNETCORE_URLS` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="63def-266">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="63def-267">`--urls` komut satırı bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="63def-267">`--urls` command-line argument.</span></span>
* <span data-ttu-id="63def-268">`urls` ana bilgisayar yapılandırma anahtarı.</span><span class="sxs-lookup"><span data-stu-id="63def-268">`urls` host configuration key.</span></span>
* <span data-ttu-id="63def-269">`UseUrls` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="63def-269">`UseUrls` extension method.</span></span>

<span data-ttu-id="63def-270">Daha fazla bilgi için [sunucu URL'leri](xref:fundamentals/host/web-host#server-urls) ve [geçersiz kılma yapılandırmasını](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="63def-270">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="63def-271">Bu yaklaşımları kullanarak sağlanan değer, bir veya daha fazla HTTP ve HTTPS uç noktası (varsayılan sertifika varsa HTTPS) olabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-271">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="63def-272">Değer noktalı virgülle ayrılmış listesini yapılandırın (örneğin, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="63def-272">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="63def-273">*Varsayılan Sertifika yapılandırmasından değiştirin*</span><span class="sxs-lookup"><span data-stu-id="63def-273">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="63def-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="63def-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="63def-275">Bir varsayılan HTTPS uygulama ayarları yapılandırma şeması Kestrel için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-275">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="63def-276">Disk üzerindeki bir dosyadan veya bir sertifika deposu kullanmak için URL'leri ve sertifikalar dahil olmak üzere birden fazla uç noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="63def-276">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="63def-277">Aşağıdaki *appsettings.json* örneği:</span><span class="sxs-lookup"><span data-stu-id="63def-277">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="63def-278">Ayarlama **AllowInvalid** için `true` geçersiz sertifikaları (örneğin, otomatik olarak imzalanan sertifikalar) izin verilir.</span><span class="sxs-lookup"><span data-stu-id="63def-278">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="63def-279">Bir sertifika belirtmeyen herhangi bir HTTPS uç noktası (**HttpsDefaultCert** örnekte) bölümünde tanımlanan sertifika için geri döner **sertifikaları** > **varsayılan**  veya geliştirme sertifikası.</span><span class="sxs-lookup"><span data-stu-id="63def-279">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="63def-280">Kullanmaya alternatif **yolu** ve **parola** herhangi bir sertifikayı sertifika deposuna alanlarını kullanarak sertifikasını belirtmek için düğümüdür.</span><span class="sxs-lookup"><span data-stu-id="63def-280">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="63def-281">Örneğin, **sertifikaları** > **varsayılan** olarak sertifika belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="63def-281">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="63def-282">Şema Notlar:</span><span class="sxs-lookup"><span data-stu-id="63def-282">Schema notes:</span></span>

* <span data-ttu-id="63def-283">Uç noktaları adları büyük/küçük harfe duyarsızdır.</span><span class="sxs-lookup"><span data-stu-id="63def-283">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="63def-284">Örneğin, `HTTPS` ve `Https` geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="63def-284">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="63def-285">`Url` Parametresi her uç nokta için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="63def-285">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="63def-286">Bu parametre için aynı üst düzey biçimdir `Urls` yapılandırma parametresi dışında olan sınırlı için tek bir değer.</span><span class="sxs-lookup"><span data-stu-id="63def-286">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="63def-287">Bu uç noktaları üst düzey tanımlanan değiştirin `Urls` ekleyerek bunları yerine yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="63def-287">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="63def-288">Uç noktaları aracılığıyla kod içinde tanımlanan `Listen` yapılandırma bölümünde tanımlanan uç noktaları ile bunların toplamı olur.</span><span class="sxs-lookup"><span data-stu-id="63def-288">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="63def-289">`Certificate` Bölümüne, isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-289">The `Certificate` section is optional.</span></span> <span data-ttu-id="63def-290">Varsa `Certificate` bölüm belirtilmediyse, önceki senaryoda tanımlanan varsayılan değerler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63def-290">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="63def-291">Varsayılan değer mevcutsa, sunucunun bir özel durum oluşturur ve başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="63def-291">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="63def-292">`Certificate` Bölümü destekler **yolu**&ndash;**parola** ve **konu**&ndash;**Store** sertifikalar.</span><span class="sxs-lookup"><span data-stu-id="63def-292">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="63def-293">Bağlantı noktası çakışmalara neden olmayan sürece herhangi bir sayıda uç noktaları bu şekilde tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-293">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="63def-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` döndürür bir `KestrelConfigurationLoader` ile bir `.Endpoint(string name, options => { })` yapılandırılmış bir uç noktanın ayarları desteklemek için kullanılan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="63def-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="63def-295">Ayrıca doğrudan erişebilirsiniz `KestrelServerOptions.ConfigurationLoader` tarafından sağlanan gibi mevcut yükleyiciyi yineleme tutmak için [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="63def-295">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="63def-296">Her uç nokta yapılandırma bölümü bir seçenekler kullanılabilir `Endpoint` yöntemi böylece özel ayarlarını okuyun.</span><span class="sxs-lookup"><span data-stu-id="63def-296">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="63def-297">Birden fazla yapılandırması çağırarak yüklenmemiş olabilir `options.Configure(context.Configuration.GetSection("Kestrel"))` yeniden başka bir bölüme sahip.</span><span class="sxs-lookup"><span data-stu-id="63def-297">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="63def-298">Sürece yalnızca son yapılandırma kullanıldığını `Load` önceki örnekleri üzerinde açıkça çağrılır.</span><span class="sxs-lookup"><span data-stu-id="63def-298">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="63def-299">Metapackage çağrı değil `Load` böylece kendi varsayılan yapılandırma bölümü değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="63def-299">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="63def-300">`KestrelConfigurationLoader` yansıtmalar `Listen` API'lerinden ailesi `KestrelServerOptions` olarak `Endpoint` yüklemeleri için aynı yerde kodda ve yapılandırma uç noktaları yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-300">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="63def-301">Bu aşırı yüklemeler olmayan adlar kullanın ve yalnızca varsayılan yapılandırma ayarlarından kullanma.</span><span class="sxs-lookup"><span data-stu-id="63def-301">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="63def-302">*Kodda Varsayılanları Değiştir*</span><span class="sxs-lookup"><span data-stu-id="63def-302">*Change the defaults in code*</span></span>

<span data-ttu-id="63def-303">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` varsayılan ayarlarını değiştirmek için kullanılan `ListenOptions` ve `HttpsConnectionAdapterOptions`, önceki senaryoda belirtilen varsayılan sertifika geçersiz kılma dahil.</span><span class="sxs-lookup"><span data-stu-id="63def-303">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="63def-304">`ConfigureEndpointDefaults` ve `ConfigureHttpsDefaults` tüm uç noktaları yapılandırılmış önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-304">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="63def-305">*SNI kestrel desteği*</span><span class="sxs-lookup"><span data-stu-id="63def-305">*Kestrel support for SNI*</span></span>

<span data-ttu-id="63def-306">[Sunucu adı belirtme (SNI)](https://tools.ietf.org/html/rfc6066#section-3) birden çok etki alanı aynı IP adresini ve bağlantı noktası üzerinde barındırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-306">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="63def-307">Sunucunun doğru sertifikayı sağlayabilmesi işlevine SNI için TLS anlaşması sırasında sunucusuna güvenli oturum için konak adı istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="63def-307">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="63def-308">İstemci, TLS el sıkışma izleyen güvenli oturum sırasında sunucu ile şifreli iletişim için furnished sertifikayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="63def-308">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="63def-309">SNI aracılığıyla kestrel destekler `ServerCertificateSelector` geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="63def-309">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="63def-310">Geri çağırma ana bilgisayar adını denetleyin ve uygun sertifikayı seçmek izin vermek için bağlantı bir kez çağrılır.</span><span class="sxs-lookup"><span data-stu-id="63def-310">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="63def-311">SNI desteği gerektirir:</span><span class="sxs-lookup"><span data-stu-id="63def-311">SNI support requires:</span></span>

* <span data-ttu-id="63def-312">Hedef framework üzerinde çalışan `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="63def-312">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="63def-313">Üzerinde `netcoreapp2.0` ve `net461`, geri çağırma çağrılır ancak `name` her zaman `null`.</span><span class="sxs-lookup"><span data-stu-id="63def-313">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="63def-314">`name` De `null` istemci TLS anlaşması name parametresinde konak sağlamıyorsa.</span><span class="sxs-lookup"><span data-stu-id="63def-314">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="63def-315">Tüm Web sitelerinin aynı Kestrel örneğinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="63def-315">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="63def-316">Kestrel'i ters Ara sunucu olmadan birden çok örneğinde bir IP adresi ve bağlantı noktası paylaşımı desteklemez.</span><span class="sxs-lookup"><span data-stu-id="63def-316">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker-end

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

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="63def-317">Bir TCP yuva için bağlama</span><span class="sxs-lookup"><span data-stu-id="63def-317">Bind to a TCP socket</span></span>

<span data-ttu-id="63def-318">[Dinleme](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) yöntemi için bir TCP yuva bağlar ve SSL sertifika yapılandırma seçenekleri lambda verir:</span><span class="sxs-lookup"><span data-stu-id="63def-318">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Loopback, 8000);
            options.Listen(IPAddress.Loopback, 8001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-319">Örnek, bir uç nokta için SSL yapılandırır [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="63def-319">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="63def-320">Özel uç noktaları diğer Kestrel ayarlarını yapılandırmak için aynı API kullanın.</span><span class="sxs-lookup"><span data-stu-id="63def-320">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="63def-321">Bir UNIX yuvası için bağlama</span><span class="sxs-lookup"><span data-stu-id="63def-321">Bind to a Unix socket</span></span>

<span data-ttu-id="63def-322">Bir UNIX yuvasıyla dinleyecek [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) Bu örnekte gösterildiği gibi Ngınx ile Gelişmiş performans için:</span><span class="sxs-lookup"><span data-stu-id="63def-322">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a><span data-ttu-id="63def-323">Bağlantı noktası 0</span><span class="sxs-lookup"><span data-stu-id="63def-323">Port 0</span></span>

<span data-ttu-id="63def-324">Zaman bağlantı noktası numarasını `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar.</span><span class="sxs-lookup"><span data-stu-id="63def-324">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="63def-325">Aşağıdaki örnek, Kestrel çalışma zamanında gerçekten bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="63def-325">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="63def-326">Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:</span><span class="sxs-lookup"><span data-stu-id="63def-326">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="63def-327">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="63def-327">Limitations</span></span>

<span data-ttu-id="63def-328">Uç noktaları ile aşağıdaki yaklaşımlardan yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="63def-328">Configure endpoints with the following approaches:</span></span>

* [<span data-ttu-id="63def-329">UseUrls</span><span class="sxs-lookup"><span data-stu-id="63def-329">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* <span data-ttu-id="63def-330">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="63def-330">`--urls` command-line argument</span></span>
* <span data-ttu-id="63def-331">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="63def-331">`urls` host configuration key</span></span>
* <span data-ttu-id="63def-332">`ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="63def-332">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="63def-333">Bu yöntemler, kod Kestrel dışında sunucuları ile iş yapmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-333">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="63def-334">Ancak, aşağıdaki sınırlamaları unutmayın:</span><span class="sxs-lookup"><span data-stu-id="63def-334">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="63def-335">SSL HTTPS uç nokta yapılandırmasında bir varsayılan sertifika sağlanmadığı sürece bu yaklaşımların ile kullanılamaz (örneğin, kullanarak `KestrelServerOptions` yapılandırma veya bu konuda daha önce gösterildiği gibi bir yapılandırma dosyası).</span><span class="sxs-lookup"><span data-stu-id="63def-335">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="63def-336">Hem `Listen` ve `UseUrls` yaklaşımları eşzamanlı olarak kullanılan `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.</span><span class="sxs-lookup"><span data-stu-id="63def-336">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="63def-337">IIS bitiş noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="63def-337">IIS endpoint configuration</span></span>

<span data-ttu-id="63def-338">IIS geçersiz kılmak için IIS, URL bağlamaları kullanırken bağlamalar tarafından ayarlanan `Listen` veya `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="63def-338">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="63def-339">Daha fazla bilgi için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.</span><span class="sxs-lookup"><span data-stu-id="63def-339">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="63def-340">Varsayılan olarak, ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="63def-340">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="63def-341">URL ön ekleri ve Kestrel kullanarak bağlantı noktalarını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="63def-341">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="63def-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) genişletme yöntemi</span><span class="sxs-lookup"><span data-stu-id="63def-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="63def-343">`--urls` komut satırı bağımsız değişkeni</span><span class="sxs-lookup"><span data-stu-id="63def-343">`--urls` command-line argument</span></span>
* <span data-ttu-id="63def-344">`urls` ana bilgisayar yapılandırma anahtarı</span><span class="sxs-lookup"><span data-stu-id="63def-344">`urls` host configuration key</span></span>
* <span data-ttu-id="63def-345">ASP.NET Core yapılandırma sistemi dahil olmak üzere `ASPNETCORE_URLS` ortam değişkeni</span><span class="sxs-lookup"><span data-stu-id="63def-345">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="63def-346">Bu yöntemler hakkında daha fazla bilgi için bkz. [barındırma](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="63def-346">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="63def-347">IIS bitiş noktası yapılandırması</span><span class="sxs-lookup"><span data-stu-id="63def-347">IIS endpoint configuration</span></span>

<span data-ttu-id="63def-348">IIS kullanırken, IIS için URL bağlamaları bağlamaları belirlediği geçersiz kılma `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="63def-348">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="63def-349">Daha fazla bilgi için [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) konu.</span><span class="sxs-lookup"><span data-stu-id="63def-349">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="63def-350">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="63def-350">ListenOptions.Protocols</span></span>

<span data-ttu-id="63def-351">`Protocols` Özelliği kurar HTTP protokollerini (`HttpProtocols`) bir bağlantı uç noktası veya sunucu için etkin.</span><span class="sxs-lookup"><span data-stu-id="63def-351">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="63def-352">Bir değer atayın `Protocols` özelliğinden `HttpProtocols` sabit listesi.</span><span class="sxs-lookup"><span data-stu-id="63def-352">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="63def-353">`HttpProtocols` Sabit listesi değeri</span><span class="sxs-lookup"><span data-stu-id="63def-353">`HttpProtocols` enum value</span></span> | <span data-ttu-id="63def-354">İzin verilen bağlantı protokolü</span><span class="sxs-lookup"><span data-stu-id="63def-354">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="63def-355">HTTP/1.1 yalnızca.</span><span class="sxs-lookup"><span data-stu-id="63def-355">HTTP/1.1 only.</span></span> <span data-ttu-id="63def-356">İle veya olmadan TLS kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63def-356">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="63def-357">HTTP/2 yalnızca.</span><span class="sxs-lookup"><span data-stu-id="63def-357">HTTP/2 only.</span></span> <span data-ttu-id="63def-358">TLS ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63def-358">Primarily used with TLS.</span></span> <span data-ttu-id="63def-359">Yalnızca istemci uygulamalarını destekliyorsa, TLS kullanılabilir bir [bilgisi modu](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="63def-359">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="63def-360">HTTP/1.1 ve HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="63def-360">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="63def-361">Bir TLS gerektirir ve [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) bağlantı HTTP/2; anlaşmak üzere bağlantı, HTTP/1.1 Aksi takdirde, varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="63def-361">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="63def-362">Varsayılan, HTTP/1.1 protokolüdür.</span><span class="sxs-lookup"><span data-stu-id="63def-362">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="63def-363">HTTP/2 için TLS kısıtlamaları:</span><span class="sxs-lookup"><span data-stu-id="63def-363">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="63def-364">TLS 1.2 veya sonraki bir sürümü</span><span class="sxs-lookup"><span data-stu-id="63def-364">TLS version 1.2 or later</span></span>
* <span data-ttu-id="63def-365">Devre dışı yeniden anlaşma</span><span class="sxs-lookup"><span data-stu-id="63def-365">Renegotiation disabled</span></span>
* <span data-ttu-id="63def-366">Sıkıştırma devre dışı</span><span class="sxs-lookup"><span data-stu-id="63def-366">Compression disabled</span></span>
* <span data-ttu-id="63def-367">En düşük kısa ömürlü anahtar değişimi boyutları:</span><span class="sxs-lookup"><span data-stu-id="63def-367">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="63def-368">Eliptik Eğri Diffie-Hellman (ECDHE) &lbrack; [RFC4492](https://www.ietf.org/rfc/rfc4492.txt) &rbrack; &ndash; 224 BITS en düşük</span><span class="sxs-lookup"><span data-stu-id="63def-368">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="63def-369">Sınırlı alanda Diffie-Hellman (DHE) &lbrack; `TLS12` &rbrack; &ndash; en az 2048 bit</span><span class="sxs-lookup"><span data-stu-id="63def-369">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="63def-370">Şifre paketini değil kara listede</span><span class="sxs-lookup"><span data-stu-id="63def-370">Cipher suite not blacklisted</span></span>

<span data-ttu-id="63def-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; p-256 Eliptik Eğri ile &lbrack; `FIPS186` &rbrack; varsayılan olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="63def-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="63def-372">Aşağıdaki örnek, HTTP/1.1 ve 8000 numaralı bağlantı noktasındaki HTTP/2 bağlantılarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="63def-372">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="63def-373">Bağlantılar TLS tarafından sağlanan bir sertifika ile güvenli hale getirilir:</span><span class="sxs-lookup"><span data-stu-id="63def-373">Connections are secured by TLS with a supplied certificate:</span></span>

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
        }
```

<span data-ttu-id="63def-374">İsteğe bağlı olarak bir `IConnectionAdapter` TLS el sıkışma belirli şifrelemeleri için bağlantı başına temelinde filtre uygulamak için uygulama:</span><span class="sxs-lookup"><span data-stu-id="63def-374">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        }
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

<span data-ttu-id="63def-375">*Protokol yapılandırmasını ayarlayın*</span><span class="sxs-lookup"><span data-stu-id="63def-375">*Set the protocol from configuration*</span></span>

<span data-ttu-id="63def-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağrıları `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Kestrel yapılandırmasını varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="63def-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="63def-377">Aşağıdaki *appsettings.json* örnek, bir varsayılan bağlantı protokol (HTTP/1.1 ve HTTP/2) kurulmuş tüm Kestrel'ın uç noktaları için:</span><span class="sxs-lookup"><span data-stu-id="63def-377">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="63def-378">Aşağıdaki yapılandırma dosyası örneği, belirli bir uç noktası için bir bağlantı protokol oluşturur:</span><span class="sxs-lookup"><span data-stu-id="63def-378">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="63def-379">Kodda belirtilen protokoller, yapılandırma tarafından ayarlanan değerleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="63def-379">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="63def-380">Aktarım yapılandırma</span><span class="sxs-lookup"><span data-stu-id="63def-380">Transport configuration</span></span>

<span data-ttu-id="63def-381">ASP.NET Core 2.1 sürümünde Kestrel'ın varsayılan aktarım artık Libuv üzerinde temel ancak bunun yerine yönetilen yuvalarda göre.</span><span class="sxs-lookup"><span data-stu-id="63def-381">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="63def-382">Bu, bir ASP.NET Core 2.0 uygulamaları çağıran 2.1 yükseltme için değişiklik [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) ve aşağıdaki paketlerden birini bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="63def-382">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="63def-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (doğrudan paket Başvurusu)</span><span class="sxs-lookup"><span data-stu-id="63def-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="63def-384">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="63def-384">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="63def-385">ASP.NET Core 2.1 veya üzerini kullanan projeleri [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ve Libuv kullanılmasını gerektirir:</span><span class="sxs-lookup"><span data-stu-id="63def-385">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="63def-386">İçin bağımlılık ekleme [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) uygulamanın proje dosyasına paket:</span><span class="sxs-lookup"><span data-stu-id="63def-386">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="63def-387">Çağrı [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="63def-387">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

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

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="63def-388">URL ön ekleri</span><span class="sxs-lookup"><span data-stu-id="63def-388">URL prefixes</span></span>

<span data-ttu-id="63def-389">Kullanırken `UseUrls`, `--urls` komut satırı bağımsız değişkeni `urls` ana bilgisayar yapılandırma anahtarı veya `ASPNETCORE_URLS` ortam değişkeni URL ön ekleri olabilir aşağıdaki biçimlerden birini.</span><span class="sxs-lookup"><span data-stu-id="63def-389">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-390">Yalnızca HTTP URL ön ekleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="63def-390">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="63def-391">Kullanarak URL bağlamaları yapılandırma sırasında kestrel SSL'yi desteklemez `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="63def-391">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="63def-392">Bağlantı noktası numarası ile IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="63def-392">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="63def-393">`0.0.0.0` tüm IPv4 adreslerine bağlayan bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="63def-393">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="63def-394">Bağlantı noktası numarası ile IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="63def-394">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="63def-395">`[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="63def-395">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="63def-396">Bağlantı noktası numarası ile ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="63def-396">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="63def-397">Ana makine adları, `*`, ve `+`, özel değildir.</span><span class="sxs-lookup"><span data-stu-id="63def-397">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="63def-398">Herhangi bir şey geçerli bir IP adresi tanınmıyor veya `localhost` tüm IPv4 ve IPv6 IP bağlar.</span><span class="sxs-lookup"><span data-stu-id="63def-398">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="63def-399">Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [HTTP.sys](xref:fundamentals/servers/httpsys) veya IIS, Ngınx veya Apache gibi bir ters proxy sunucusu.</span><span class="sxs-lookup"><span data-stu-id="63def-399">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="63def-400">Etkin bir ters proxy filtreleme konakla kullanmayan etkinleştirirseniz [filtreleme konak](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="63def-400">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="63def-401">Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad</span><span class="sxs-lookup"><span data-stu-id="63def-401">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="63def-402">Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır.</span><span class="sxs-lookup"><span data-stu-id="63def-402">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="63def-403">İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="63def-403">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="63def-404">Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="63def-404">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="63def-405">Bağlantı noktası numarası ile IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="63def-405">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="63def-406">`0.0.0.0` tüm IPv4 adreslerine bağlayan bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="63def-406">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="63def-407">Bağlantı noktası numarası ile IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="63def-407">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="63def-408">`[::]` IPv4 IPv6 eşdeğerdir `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="63def-408">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="63def-409">Bağlantı noktası numarası ile ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="63def-409">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="63def-410">Ana makine adları, `*`, ve `+` özel değildir.</span><span class="sxs-lookup"><span data-stu-id="63def-410">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="63def-411">Tanınan bir IP adresi olmayan herhangi bir şey veya `localhost` tüm IPv4 ve IPv6 IP bağlar.</span><span class="sxs-lookup"><span data-stu-id="63def-411">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="63def-412">Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak için kullanın [WebListener](xref:fundamentals/servers/weblistener) veya IIS, Ngınx veya Apache gibi bir ters proxy sunucusu.</span><span class="sxs-lookup"><span data-stu-id="63def-412">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="63def-413">Konak `localhost` bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte ad</span><span class="sxs-lookup"><span data-stu-id="63def-413">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="63def-414">Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır.</span><span class="sxs-lookup"><span data-stu-id="63def-414">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="63def-415">İstenen bağlantı noktası başka bir hizmette ya da geri döngü arabirimine tarafından kullanılıyor Kestrel başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="63def-415">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="63def-416">Ya da geri döngü arabirimine başka bir nedenle kullanılamıyorsa (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="63def-416">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="63def-417">UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="63def-417">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="63def-418">**Bağlantı noktası 0**</span><span class="sxs-lookup"><span data-stu-id="63def-418">**Port 0**</span></span>

<span data-ttu-id="63def-419">Bağlantı noktası numarasını olduğunda `0` belirtilirse, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar.</span><span class="sxs-lookup"><span data-stu-id="63def-419">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="63def-420">Bağlantı noktası bağlama `0` herhangi bir ana bilgisayar adı veya IP dışında için izin verilen `localhost`.</span><span class="sxs-lookup"><span data-stu-id="63def-420">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="63def-421">Uygulamayı çalıştırdığınızda, konsol penceresi çıktısı dinamik bağlantı noktası uygulama nereden ulaşabileceğinizi gösterir:</span><span class="sxs-lookup"><span data-stu-id="63def-421">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="63def-422">**SSL için URL ön ekleri**</span><span class="sxs-lookup"><span data-stu-id="63def-422">**URL prefixes for SSL**</span></span>

<span data-ttu-id="63def-423">Çağırma varsa `UseHttps` genişletme yöntemi ile URL ön ekleri eklediğinizden emin olun `https:`:</span><span class="sxs-lookup"><span data-stu-id="63def-423">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> <span data-ttu-id="63def-424">Aynı bağlantı noktasında HTTPS ve HTTP barındırılamaz.</span><span class="sxs-lookup"><span data-stu-id="63def-424">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a><span data-ttu-id="63def-425">Konak filtreleme</span><span class="sxs-lookup"><span data-stu-id="63def-425">Host filtering</span></span>

<span data-ttu-id="63def-426">Kestrel'i yapılandırma gibi önekleri temel alarak desteklerken `http://example.com:5000`, Kestrel büyük ölçüde ana bilgisayar adı yok sayar.</span><span class="sxs-lookup"><span data-stu-id="63def-426">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="63def-427">Konak `localhost` bağlama geri döngü adresi için kullanılan özel bir durum.</span><span class="sxs-lookup"><span data-stu-id="63def-427">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="63def-428">Daha açık bir IP adresi tüm genel IP adresine bağlar herhangi diğer barındırın.</span><span class="sxs-lookup"><span data-stu-id="63def-428">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="63def-429">Bu bilgilerin hiçbiri isteği doğrulamak için kullanılan `Host` üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="63def-429">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="63def-430">Geçici bir çözüm olarak, ana bilgisayar üst bilgisi filtrelemeyle ters Ara sunucu arkasındaki barındırın.</span><span class="sxs-lookup"><span data-stu-id="63def-430">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="63def-431">ASP.NET core'da Kestrel için desteklenen tek senaryo budur 1.x.</span><span class="sxs-lookup"><span data-stu-id="63def-431">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="63def-432">Geçici çözüm olarak, istekleri filtrelemek için Ara yazılımları kullanmayı `Host` üst bilgi:</span><span class="sxs-lookup"><span data-stu-id="63def-432">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="63def-433">Önceki kaydetme `HostFilteringMiddleware` içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="63def-433">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="63def-434">Unutmayın [ara yazılımı kaydı sıralama](xref:fundamentals/middleware/index#order) önemlidir.</span><span class="sxs-lookup"><span data-stu-id="63def-434">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="63def-435">Kayıt hemen tanılama ara yazılım kayıttan sonra gerçekleşmesi (örneğin, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="63def-435">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="63def-436">Ara yazılım bekliyor bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="63def-436">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="63def-437">Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:</span><span class="sxs-lookup"><span data-stu-id="63def-437">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="63def-438">Geçici çözüm olarak, konak filtreleme ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="63def-438">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="63def-439">Konak filtreleme ara yazılımı tarafından sağlanan [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="63def-439">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="63def-440">Ara yazılım tarafından eklenen [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), çağıran [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="63def-440">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="63def-441">Konak filtreleme ara yazılım, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="63def-441">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="63def-442">Ara yazılım etkinleştirmek için tanımladığınız bir `AllowedHosts` anahtarını *appsettings.json*/*appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="63def-442">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="63def-443">Bağlantı noktası numaraları olmadan konak adları, noktalı virgülle ayrılmış listesini değerdir:</span><span class="sxs-lookup"><span data-stu-id="63def-443">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="63def-444">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="63def-444">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="63def-445">[Üst bilgileri ara yazılım iletilen](xref:host-and-deploy/proxy-load-balancer) de sahip bir [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) seçeneği.</span><span class="sxs-lookup"><span data-stu-id="63def-445">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="63def-446">İletilen üstbilgileri ara yazılım ve konak filtreleme ara yazılım farklı senaryolar için benzer bir işlevsellik vardır.</span><span class="sxs-lookup"><span data-stu-id="63def-446">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="63def-447">Ayar `AllowedHosts` iletilen üstbilgileri Ara yazılımla barındırma üst bilgisi bir tersine Ara sunucunun istekleri iletirken korunmaz zaman uygun yük dengeleyici mı.</span><span class="sxs-lookup"><span data-stu-id="63def-447">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="63def-448">Ayarı `AllowedHosts` Kestrel bir uç sunucusu kullanıldığında veya ne zaman ana bilgisayar üst bilgisini doğrudan iletilen konak filtreleme Ara yazılımla uygundur.</span><span class="sxs-lookup"><span data-stu-id="63def-448">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as an edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="63def-449">İletilen üstbilgileri ara yazılım hakkında daha fazla bilgi için bkz. [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="63def-449">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="63def-450">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63def-450">Additional resources</span></span>

* [<span data-ttu-id="63def-451">HTTPS'yi Zorunlu Kılma</span><span class="sxs-lookup"><span data-stu-id="63def-451">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="63def-452">Kestrel'i kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="63def-452">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="63def-453">RFC 7230: İleti söz dizimi ve yönlendirme (Bölüm 5.4: ana bilgisayar)</span><span class="sxs-lookup"><span data-stu-id="63def-453">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="63def-454">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="63def-454">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
