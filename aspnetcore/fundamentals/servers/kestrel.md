---
title: "ASP.NET Core kestrel web sunucusu uygulaması"
author: tdykstra
description: "ASP.NET Core üzerinde libuv tabanlı için Kestrel, platformlar arası web sunucusu tanıtır."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: be465c9e8803e4d348cdd14181b4ea147f75e1a0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b9718-103">ASP.NET Core kestrel web sunucusu uygulaması</span><span class="sxs-lookup"><span data-stu-id="b9718-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b9718-104">Tarafından [zel Dykstra](https://github.com/tdykstra), [Chris fillerin](https://github.com/Tratcher), ve [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="b9718-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="b9718-105">Kestrel olan bir platformlar arası [ASP.NET Core için web sunucusu](index.md) göre [libuv](https://github.com/libuv/libuv), platformlar arası zaman uyumsuz g/ç kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="b9718-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="b9718-106">Kestrel varsayılan ASP.NET Core proje şablonları olarak dahil edilen web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="b9718-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="b9718-107">Kestrel aşağıdaki özellikleri destekler:</span><span class="sxs-lookup"><span data-stu-id="b9718-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="b9718-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="b9718-108">HTTPS</span></span>
  * <span data-ttu-id="b9718-109">Etkinleştirmek için kullanılan opak yükseltme [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="b9718-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="b9718-110">Yüksek performanslı Nginx arkasında UNIX yuvaları</span><span class="sxs-lookup"><span data-stu-id="b9718-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="b9718-111">Kestrel tüm platformlar ve .NET Core destekleyen sürümler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="b9718-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-113">[Görüntülemek veya karşıdan 2.x için örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9718-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9718-115">[Görüntülemek veya karşıdan 1.x için örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9718-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="b9718-116">Ters proxy ile Kestrel kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="b9718-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-118">Tek başına veya birlikte Kestrel kullanabileceğiniz bir *ters proxy sunucusu*, IIS, Nginx ya da Apache gibi.</span><span class="sxs-lookup"><span data-stu-id="b9718-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="b9718-119">Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="b9718-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel bir ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b9718-122">Her iki yapılandırma &mdash; ile veya bir ters Ara sunucu olmadan &mdash; Kestrel yalnızca bir iç ağa açılırsa de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b9718-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9718-124">Uygulamanızı bir iç ağ yalnızca gelen istekleri kabul ederse, tek başına Kestrel kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9718-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel, iç ağınız ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="b9718-126">Uygulamanızı internet kullanıma sunma, IIS, Nginx ya da Apache olarak kullanmalısınız bir *ters proxy sunucu*.</span><span class="sxs-lookup"><span data-stu-id="b9718-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="b9718-127">Ters proxy sunucusu Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="b9718-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel dolaylı olarak IIS, Nginx ya da Apache gibi bir ters proxy sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="b9718-129">Güvenlik nedenleriyle (trafiğini Internet'ten gösterilen) kenar dağıtımlar için ters Ara sunucu gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b9718-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="b9718-130">Kestrel 1.x sürümleri tamamlayıcı saldırılarına karşı savunma yoktur.</span><span class="sxs-lookup"><span data-stu-id="b9718-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="b9718-131">Bu içerir ancak uygun zaman aşımları, boyut sınırları ve eş zamanlı bağlantı sınırlarını sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b9718-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="b9718-132">Aynı IP adresini ve bağlantı noktası tek bir sunucu üzerinde çalışan paylaşan birden fazla uygulamanız varsa, ters proxy gerektiren bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="b9718-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="b9718-133">Aynı IP adresini ve birden çok işlem arasında bağlantı noktası paylaşımı Kestrel desteklemediğinden, doğrudan Kestrel ile işe yaramaz.</span><span class="sxs-lookup"><span data-stu-id="b9718-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="b9718-134">Bir bağlantı noktasında dinleyecek şekilde Kestrel yapılandırdığınızda, bu bağlantı noktası ana bilgisayar üstbilgisi bağımsız olarak tüm trafiği işler.</span><span class="sxs-lookup"><span data-stu-id="b9718-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="b9718-135">Bağlantı noktalarını paylaşabilirsiniz ters bir proxy, ardından, benzersiz bir IP ve bağlantı noktası Kestrel iletmelidir.</span><span class="sxs-lookup"><span data-stu-id="b9718-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="b9718-136">Kullanarak bir ters proxy sunucusu gerekli olmasa bile, iyi bir seçimdir diğer nedenleri olabilir:</span><span class="sxs-lookup"><span data-stu-id="b9718-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="b9718-137">Sunulan yüzey alanınızı sınırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9718-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="b9718-138">Yapılandırma ve savunma isteğe bağlı bir ek katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b9718-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="b9718-139">Mevcut altyapısına daha iyi tümleştirme.</span><span class="sxs-lookup"><span data-stu-id="b9718-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="b9718-140">Yük Dengeleme ve SSL kurulumu basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="b9718-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="b9718-141">Yalnızca ters Ara sunucunuz bir SSL sertifikası gerektirir ve bu sunucu uygulama sunucularınızda düz HTTP kullanarak iç ağ ile iletişim kurabilir.</span><span class="sxs-lookup"><span data-stu-id="b9718-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="b9718-142">Ana bilgisayar etkin filtre ile ters Ara sunucu kullanmıyorsanız, etkinleştirmelisiniz [ana bilgisayar filtre](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="b9718-142">If you're not using a reverse proxy with host filtering enabled, you must enable [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="b9718-143">ASP.NET Core uygulamaları Kestrel kullanma</span><span class="sxs-lookup"><span data-stu-id="b9718-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paket dahil [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="b9718-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="b9718-146">ASP.NET Core proje şablonlarını Kestrel varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9718-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="b9718-147">İçinde *Program.cs*, şablonu kodu çağrılarının `CreateDefaultBuilder`, çağıran [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) arka planda.</span><span class="sxs-lookup"><span data-stu-id="b9718-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="b9718-148">Kestrel seçeneklerini yapılandırmak gerekirse, çağrı `UseKestrel` içinde *Program.cs* aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="b9718-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9718-150">Yükleme [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="b9718-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="b9718-151">Çağrı [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) genişletme yöntemi `WebHostBuilder` içinde `Main` herhangi belirtme yöntemi [Kestrel seçenekleri](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) sonraki bölümde gösterildiği gibi gereken.</span><span class="sxs-lookup"><span data-stu-id="b9718-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="b9718-152">Kestrel seçenekleri</span><span class="sxs-lookup"><span data-stu-id="b9718-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-154">Kestrel web sunucusu Internet'e yönelik dağıtımlarda özellikle yararlı kısıtlaması yapılandırma seçenekleri vardır.</span><span class="sxs-lookup"><span data-stu-id="b9718-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="b9718-155">Ayarlayabileceğiniz sınırları bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b9718-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="b9718-156">En fazla istemci bağlantıları</span><span class="sxs-lookup"><span data-stu-id="b9718-156">Maximum client connections</span></span>
- <span data-ttu-id="b9718-157">En büyük istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="b9718-157">Maximum request body size</span></span>
- <span data-ttu-id="b9718-158">En düşük istek gövdesi veri oranı</span><span class="sxs-lookup"><span data-stu-id="b9718-158">Minimum request body data rate</span></span>

<span data-ttu-id="b9718-159">Bu kısıtlamaların ve diğerleri ayarlamak `Limits` özelliği [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b9718-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="b9718-160">`Limits` Özelliği bir örneğini tutan [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b9718-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="b9718-161">**En fazla istemci bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="b9718-161">**Maximum client connections**</span></span>

<span data-ttu-id="b9718-162">Aşağıdaki kod ile tüm uygulama için en fazla eş zamanlı açık TCP bağlantı sayısını ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="b9718-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="b9718-163">Başka bir protokol (örneğin, WebSockets istek üzerine) için HTTP veya HTTPS yükseltildi bağlantıları için ayrı bir sınır yoktur.</span><span class="sxs-lookup"><span data-stu-id="b9718-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="b9718-164">Bir bağlantı yükseltildikten sonra onu karşı sayılan değil `MaxConcurrentConnections` sınırı.</span><span class="sxs-lookup"><span data-stu-id="b9718-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="b9718-165">En fazla bağlantı sayısı, varsayılan olarak sınırsız (null) olur.</span><span class="sxs-lookup"><span data-stu-id="b9718-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="b9718-166">**En büyük istek gövdesi boyutu**</span><span class="sxs-lookup"><span data-stu-id="b9718-166">**Maximum request body size**</span></span>

<span data-ttu-id="b9718-167">Varsayılan en büyük istek gövdesi boyutu 30.000.000, hangi 28.6 MB'dir bayttır.</span><span class="sxs-lookup"><span data-stu-id="b9718-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="b9718-168">Bir ASP.NET Core MVC uygulamasında sınırı geçersiz kılmak için önerilen yöntem kullanmaktır [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) bir eylem yönteminin özniteliği:</span><span class="sxs-lookup"><span data-stu-id="b9718-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="b9718-169">Aşağıda, tüm uygulama, her istek için kısıtlamayı yapılandırmak nasıl oluşturulduğunu gösteren bir örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b9718-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="b9718-170">Belirli bir istek Ara yazılımında ayarı geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b9718-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="b9718-171">İstek okunurken uygulama başlatıldıktan sonra bir istekte sınırı yapılandırmayı denerseniz özel durum oluşur.</span><span class="sxs-lookup"><span data-stu-id="b9718-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="b9718-172">Var. bir `IsReadOnly` belirten özelliği, `MaxRequestBodySize` özelliği olan salt okunur durumda olduğu çok geç sınırını yapılandırmak için anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="b9718-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="b9718-173">**En düşük istek gövdesi veri oranı**</span><span class="sxs-lookup"><span data-stu-id="b9718-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="b9718-174">Veri içinde belirtilen hızda bayt/saniye geliyorsa kestrel saniyede denetler.</span><span class="sxs-lookup"><span data-stu-id="b9718-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="b9718-175">Hızı en düşerse, bağlantı zaman aşımına. Yetkisiz kullanım süresi Kestrel en düşük kadar gönderme oranını artırmak için istemcinin verir süreyi belirtir; oranı, bu süre boyunca işaretli değil.</span><span class="sxs-lookup"><span data-stu-id="b9718-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="b9718-176">Yetkisiz kullanım süresi başlangıçta TCP yavaş başlatma nedeniyle yavaş bir hızda veri gönderme bağlantıları bırakarak önlemeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="b9718-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="b9718-177">Varsayılan minimum 240 bayt/saniye, 5 saniye yetkisiz kullanım süresi ile hızıdır.</span><span class="sxs-lookup"><span data-stu-id="b9718-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="b9718-178">Minimum oran yanıt için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b9718-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="b9718-179">İstek sınırı ve yanıt sınırı ayarlamak için kod olması dışında aynı olduğundan `RequestBody` veya `Response` özelliği ve arabirim adlarını.</span><span class="sxs-lookup"><span data-stu-id="b9718-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="b9718-180">En az veri hızları yapılandırmak nasıl oluşturulduğunu gösteren bir örnek şudur *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b9718-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="b9718-181">İstek başına oranları Ara yazılımında yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b9718-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="b9718-182">Aşağıdaki sınıflar diğer Kestrel seçenekleri hakkında daha fazla bilgi için bkz:</span><span class="sxs-lookup"><span data-stu-id="b9718-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="b9718-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="b9718-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="b9718-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="b9718-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="b9718-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="b9718-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9718-187">Kestrel seçenekleri hakkında daha fazla bilgi için bkz: [KestrelServerOptions sınıfı](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="b9718-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="b9718-188">Uç nokta yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b9718-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-190">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b9718-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b9718-191">URL öneklerini ve Kestrel çağırarak üzerinde dinleme bağlantı noktalarını yapılandırma `Listen` veya `ListenUnixSocket` yöntemlere `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="b9718-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="b9718-192">(`UseUrls`, `urls` komut satırı bağımsız değişkeni ve ayrıca iş ASPNETCORE_URLS ortam değişkeni not ettiğiniz sınırlamaları sahip ancak [bu makalenin ilerisinde yer](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="b9718-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="b9718-193">**Bir TCP yuvası bağlama**</span><span class="sxs-lookup"><span data-stu-id="b9718-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="b9718-194">`Listen` Yöntemi bir TCP yuvasını bağlar ve seçenekleri lambda bir SSL sertifikası yapılandırmanıza olanak sağlar:</span><span class="sxs-lookup"><span data-stu-id="b9718-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="b9718-195">Nasıl Bu örnek SSL için özel bir uç nokta kullanarak yapılandırır fark [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="b9718-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="b9718-196">Aynı API belirli uç noktaları diğer Kestrel ayarlarını yapılandırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b9718-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

<span data-ttu-id="b9718-197">**Bir UNIX yuvası bağlama**</span><span class="sxs-lookup"><span data-stu-id="b9718-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="b9718-198">Bir UNIX yuvada ile Nginx, Gelişmiş performans için bu örnekte gösterildiği gibi dinleme:</span><span class="sxs-lookup"><span data-stu-id="b9718-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="b9718-199">**Bağlantı noktası 0**</span><span class="sxs-lookup"><span data-stu-id="b9718-199">**Port 0**</span></span>

<span data-ttu-id="b9718-200">Bağlantı noktası numarası 0 belirtirseniz, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar.</span><span class="sxs-lookup"><span data-stu-id="b9718-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b9718-201">Aşağıdaki örnek, Kestrel gerçekten çalışma zamanında bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b9718-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="b9718-202">**UseUrls sınırlamaları**</span><span class="sxs-lookup"><span data-stu-id="b9718-202">**UseUrls limitations**</span></span>

<span data-ttu-id="b9718-203">Uç noktaları çağırarak yapılandırabileceğiniz `UseUrls` yöntemi veya kullanarak `urls` komut satırı bağımsız değişkeni veya ASPNETCORE_URLS ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="b9718-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="b9718-204">Bu yöntemler Kestrel dışında sunucularıyla çalışmak için kodunuzu istiyorsanız kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="b9718-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="b9718-205">Ancak, bu sınırlamaları unutmayın:</span><span class="sxs-lookup"><span data-stu-id="b9718-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="b9718-206">Bu yöntemlerle SSL kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="b9718-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="b9718-207">Her ikisi de kullanırsanız, `Listen` yöntemi ve `UseUrls`, `Listen` uç noktaları geçersiz kılma `UseUrls` uç noktaları.</span><span class="sxs-lookup"><span data-stu-id="b9718-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="b9718-208">**IIS için uç nokta yapılandırması**</span><span class="sxs-lookup"><span data-stu-id="b9718-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="b9718-209">IIS kullanırsanız, IIS için URL bağlamaları ya da çağırarak ayarladığınız hiçbir bağlama geçersiz kılma `Listen` veya `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b9718-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="b9718-210">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b9718-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b9718-212">Varsayılan olarak ASP.NET Core bağlar `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b9718-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b9718-213">URL öneklerini ve Kestrel kullanarak üzerinde dinleme bağlantı noktalarını yapılandırabilirsiniz `UseUrls` genişletme yöntemi, `urls` komut satırı bağımsız değişkeni ya da ASP.NET Core yapılandırma sistemi.</span><span class="sxs-lookup"><span data-stu-id="b9718-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b9718-214">Bu yöntemler hakkında daha fazla bilgi için bkz: [barındırma](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="b9718-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="b9718-215">Ters proxy IIS kullandığınızda URL bağlama nasıl çalıştığı hakkında daha fazla bilgi için bkz: [ASP.NET Core Modülü](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="b9718-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="b9718-216">URL öneklerini</span><span class="sxs-lookup"><span data-stu-id="b9718-216">URL prefixes</span></span>

<span data-ttu-id="b9718-217">Çağırırsanız `UseUrls` veya `urls` komut satırı bağımsız değişkeni veya ASPNETCORE_URLS ortam değişkeni, URL öneklerini olabilir aşağıdaki biçimlerden birini.</span><span class="sxs-lookup"><span data-stu-id="b9718-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b9718-219">Yalnızca HTTP URL öneklerini geçerlidir; Kestrel SSL'yi desteklemez kullanarak URL bağlamaları yapılandırdığınızda `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="b9718-219">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="b9718-220">Bağlantı noktası numarası ile birlikte IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="b9718-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="b9718-221">0.0.0.0 tüm IPv4 adresleri bağlayan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="b9718-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b9718-222">Bağlantı noktası numarası ile birlikte IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="b9718-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="b9718-223">[:] IPv4, IPv6 aynıdır 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b9718-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b9718-224">Bağlantı noktası numarası ile ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="b9718-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="b9718-225">Ana makine adları, \*, ve +, özel değildir.</span><span class="sxs-lookup"><span data-stu-id="b9718-225">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="b9718-226">Tanınan bir IP adresi veya "localhost" değil herhangi bir şey tüm IPv4 ve IPv6 IP bağlarsınız.</span><span class="sxs-lookup"><span data-stu-id="b9718-226">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b9718-227">Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak gereksinim duyarsanız kullanın [HTTP.sys](httpsys.md) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.</span><span class="sxs-lookup"><span data-stu-id="b9718-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="b9718-228">Ana bilgisayar etkin filtre ile ters Ara sunucu kullanmıyorsanız, etkinleştirmelisiniz [ana bilgisayar filtre](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="b9718-228">If you're not using a reverse proxy with host filtering enabled, you must enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="b9718-229">Bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte "Localhost" adı</span><span class="sxs-lookup"><span data-stu-id="b9718-229">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b9718-230">Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır.</span><span class="sxs-lookup"><span data-stu-id="b9718-230">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b9718-231">İstenen bağlantı noktası başka bir hizmet ya da geri döngü arabirimde tarafından kullanılıyor Kestrel başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="b9718-231">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b9718-232">Herhangi bir nedenden dolayı ya da geri döngü arabirimi kullanılabilir olup olmadığını (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b9718-232">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="b9718-234">Bağlantı noktası numarası ile birlikte IPv4 adresi</span><span class="sxs-lookup"><span data-stu-id="b9718-234">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="b9718-235">0.0.0.0 tüm IPv4 adresleri bağlayan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="b9718-235">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="b9718-236">Bağlantı noktası numarası ile birlikte IPv6 adresi</span><span class="sxs-lookup"><span data-stu-id="b9718-236">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="b9718-237">[:] IPv4, IPv6 aynıdır 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="b9718-237">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="b9718-238">Bağlantı noktası numarası ile ana bilgisayar adı</span><span class="sxs-lookup"><span data-stu-id="b9718-238">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="b9718-239">Ana makine adları, \*, ve + özel değildir.</span><span class="sxs-lookup"><span data-stu-id="b9718-239">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="b9718-240">Tanınan bir IP adresi veya "localhost" olmayan herhangi bir şey tüm IPv4 ve IPv6 IP bağlar.</span><span class="sxs-lookup"><span data-stu-id="b9718-240">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="b9718-241">Farklı ana bilgisayar adları aynı bağlantı noktasında farklı ASP.NET Core uygulamaları bağlamak gereksinim duyarsanız kullanın [WebListener](weblistener.md) veya IIS, Nginx ya da Apache gibi bir ters proxy sunucusu.</span><span class="sxs-lookup"><span data-stu-id="b9718-241">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="b9718-242">Bağlantı noktası numarası veya geri döngü IP bağlantı noktası numarası ile birlikte "Localhost" adı</span><span class="sxs-lookup"><span data-stu-id="b9718-242">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="b9718-243">Zaman `localhost` belirtilirse, IPv4 ve IPv6 geri döngü arabirimlere bağlamak Kestrel çalışır.</span><span class="sxs-lookup"><span data-stu-id="b9718-243">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="b9718-244">İstenen bağlantı noktası başka bir hizmet ya da geri döngü arabirimde tarafından kullanılıyor Kestrel başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="b9718-244">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="b9718-245">Herhangi bir nedenden dolayı ya da geri döngü arabirimi kullanılabilir olup olmadığını (genellikle IPv6 desteklenmediğinden çoğu), Kestrel bir uyarı kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b9718-245">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="b9718-246">UNIX yuva</span><span class="sxs-lookup"><span data-stu-id="b9718-246">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="b9718-247">**Bağlantı noktası 0**</span><span class="sxs-lookup"><span data-stu-id="b9718-247">**Port 0**</span></span>

<span data-ttu-id="b9718-248">Bağlantı noktası numarası 0 belirtirseniz, Kestrel dinamik olarak kullanılabilir bir bağlantı noktasına bağlar.</span><span class="sxs-lookup"><span data-stu-id="b9718-248">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="b9718-249">0 bağlantı noktası bağlama için kullanılabilir, herhangi bir ana bilgisayar adı veya IP dışında `localhost` adı.</span><span class="sxs-lookup"><span data-stu-id="b9718-249">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="b9718-250">Aşağıdaki örnek, Kestrel gerçekten çalışma zamanında bağlı hangi bağlantı noktasını belirlemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="b9718-250">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="b9718-251">**SSL için URL öneklerini**</span><span class="sxs-lookup"><span data-stu-id="b9718-251">**URL prefixes for SSL**</span></span>

<span data-ttu-id="b9718-252">URL öneklerini ile eklediğinizden emin olun `https:` çağırırsanız `UseHttps` genişletme yöntemi, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="b9718-252">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="b9718-253">Aynı bağlantı noktasında HTTPS ve HTTP barındırılamaz.</span><span class="sxs-lookup"><span data-stu-id="b9718-253">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a><span data-ttu-id="b9718-254">Ana bilgisayar filtre</span><span class="sxs-lookup"><span data-stu-id="b9718-254">Host filtering</span></span>

<span data-ttu-id="b9718-255">Kestrel gibi ön eklerine göre yapılandırma desteklerken `http://example.com:5000`, büyük ölçüde ana bilgisayar adı yok sayar.</span><span class="sxs-lookup"><span data-stu-id="b9718-255">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, it largely ignores the host name.</span></span> <span data-ttu-id="b9718-256">Localhost bağlama geri döngü adresleri için kullanılan özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="b9718-256">Localhost is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="b9718-257">Açık bir IP adresi tüm ortak IP adresine bağlar daha herhangi diğer barındırır.</span><span class="sxs-lookup"><span data-stu-id="b9718-257">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="b9718-258">Bu bilgilerin hiçbiri doğrulamak için kullanılan istek konak üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="b9718-258">None of this information is used to validate request Host headers.</span></span>

<span data-ttu-id="b9718-259">İki olası geçici çözümler vardır:</span><span class="sxs-lookup"><span data-stu-id="b9718-259">There are two possible workarounds:</span></span>

* <span data-ttu-id="b9718-260">Ana bilgisayar üstbilgisi filtreleme ile ters Ara sunucu arkasındaki ana bilgisayarı.</span><span class="sxs-lookup"><span data-stu-id="b9718-260">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="b9718-261">ASP.NET Core Kestrel için desteklenen tek senaryo, bu 1.x.</span><span class="sxs-lookup"><span data-stu-id="b9718-261">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="b9718-262">Ana bilgisayar üstbilgisi tarafından istekleri filtrelemek için bir ara yazılımı kullanın.</span><span class="sxs-lookup"><span data-stu-id="b9718-262">Use a middleware to filter requests by the host header.</span></span> <span data-ttu-id="b9718-263">Bir örnek ara yazılımı aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="b9718-263">A sample middleware follows:</span></span>

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
            _logger.LogDebug("Request rejected due to incorrect host header.");
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
            // Http/1.0 does not require the host header.
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

<span data-ttu-id="b9718-264">Yukarıdaki kayıt `HostFilteringMiddleware` içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b9718-264">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="b9718-265">Unutmayın [ara yazılım kaydı sıralama](xref:fundamentals/middleware/index#ordering) önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b9718-265">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="b9718-266">Kayıt hemen tanılama Ara yazılımdan sonra gerçekleşmelidir (örneğin, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="b9718-266">Registration should occur immediately after the diagnostic middleware (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="b9718-267">Önceki Ara bekliyor bir `AllowedHosts` anahtarını *appsettings.\< EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="b9718-267">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="b9718-268">Bu anahtarın değeri, bağlantı noktası numaralarını olmadan ana bilgisayar adlarını noktalı virgülle ayrılmış bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="b9718-268">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="b9718-269">Dahil `AllowedHosts` anahtar-değer çifti *appsettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="b9718-269">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="b9718-270">Localhost yapılandırma dosyası *appsettings. Development.JSON*, şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="b9718-270">The localhost configuration file, *appsettings.Development.json*, looks like this:</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="next-steps"></a><span data-ttu-id="b9718-271">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b9718-271">Next steps</span></span>

<span data-ttu-id="b9718-272">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="b9718-272">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b9718-273">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b9718-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="b9718-274">2.x için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="b9718-274">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="b9718-275">Kestrel kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="b9718-275">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b9718-276">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b9718-276">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="b9718-277">1.x için örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="b9718-277">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="b9718-278">Kestrel kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="b9718-278">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
