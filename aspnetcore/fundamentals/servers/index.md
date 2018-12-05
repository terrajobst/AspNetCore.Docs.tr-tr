---
title: ASP.NET Core Web sunucu uygulamalarında
author: guardrex
description: ASP.NET Core için web sunucuları Kestrel ve HTTP.sys keşfedin. Bir sunucu seçin ve ne zaman bir ters proxy sunucusu kullanmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861362"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="675b2-104">ASP.NET Core Web sunucu uygulamalarında</span><span class="sxs-lookup"><span data-stu-id="675b2-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="675b2-105">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="675b2-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="675b2-106">ASP.NET Core uygulaması bir işlemde HTTP sunucusu uygulamasını ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="675b2-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="675b2-107">HTTP istekleri ve bunları uygulamaya kümesi ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="675b2-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="675b2-108">Windows</span><span class="sxs-lookup"><span data-stu-id="675b2-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="675b2-109">ASP.NET Core aşağıdaki ile birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="675b2-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="675b2-110">[Kestrel'i sunucu](xref:fundamentals/servers/kestrel) olduğundan varsayılan, platformlar arası bir HTTP sunucusu.</span><span class="sxs-lookup"><span data-stu-id="675b2-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="675b2-111">IIS HTTP sunucusu (`IISHttpServer`) olan bir [IIS işlem sunucusu](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) ile kullanılan uygulama [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="675b2-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="675b2-112">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="675b2-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="675b2-113">HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="675b2-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="675b2-114">macOS</span><span class="sxs-lookup"><span data-stu-id="675b2-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="675b2-115">ASP.NET Core ile birlikte gelir [Kestrel sunucu](xref:fundamentals/servers/kestrel), platformlar arası bir HTTP sunucusu, varsayılan olan.</span><span class="sxs-lookup"><span data-stu-id="675b2-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="675b2-116">Linux</span><span class="sxs-lookup"><span data-stu-id="675b2-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="675b2-117">ASP.NET Core ile birlikte gelir [Kestrel sunucu](xref:fundamentals/servers/kestrel), platformlar arası bir HTTP sunucusu, varsayılan olan.</span><span class="sxs-lookup"><span data-stu-id="675b2-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="675b2-118">Windows</span><span class="sxs-lookup"><span data-stu-id="675b2-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="675b2-119">ASP.NET Core aşağıdaki ile birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="675b2-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="675b2-120">[Kestrel'i sunucu](xref:fundamentals/servers/kestrel) olduğundan varsayılan, platformlar arası bir HTTP sunucusu.</span><span class="sxs-lookup"><span data-stu-id="675b2-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="675b2-121">[HTTP.sys sunucu](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="675b2-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="675b2-122">HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="675b2-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="675b2-123">macOS</span><span class="sxs-lookup"><span data-stu-id="675b2-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="675b2-124">ASP.NET Core ile birlikte gelir [Kestrel sunucu](xref:fundamentals/servers/kestrel), platformlar arası bir HTTP sunucusu, varsayılan olan.</span><span class="sxs-lookup"><span data-stu-id="675b2-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="675b2-125">Linux</span><span class="sxs-lookup"><span data-stu-id="675b2-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="675b2-126">ASP.NET Core ile birlikte gelir [Kestrel sunucu](xref:fundamentals/servers/kestrel), platformlar arası bir HTTP sunucusu, varsayılan olan.</span><span class="sxs-lookup"><span data-stu-id="675b2-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="675b2-127">Kestrel'i</span><span class="sxs-lookup"><span data-stu-id="675b2-127">Kestrel</span></span>

<span data-ttu-id="675b2-128">Kestrel'i ASP.NET Core proje şablonları dahil varsayılan web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="675b2-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="675b2-129">Kestrel'i kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="675b2-129">Kestrel can be used:</span></span>

* <span data-ttu-id="675b2-130">Tek başına bir uç sunucusu olarak doğrudan Internet dahil olmak üzere, bir ağ isteği işleniyor.</span><span class="sxs-lookup"><span data-stu-id="675b2-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="675b2-131">İle bir *ters Ara sunucu*, gibi [Internet Information Services (IIS)](https://www.iis.net/), [Ngınx](http://nginx.org), veya [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="675b2-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="675b2-132">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="675b2-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="675b2-135">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="675b2-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="675b2-136">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="675b2-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="675b2-137">Uygulamayı yalnızca bir iç ağ gelen istekleri kabul ederse Kestrel kendisi tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel'i iç ağa ile doğrudan iletişim kurar.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="675b2-139">Uygulamayı Internet erişimine açıktır, Kestrel kullanmalısınız bir *ters Ara sunucu*, gibi [Internet Information Services (IIS)](https://www.iis.net/), [Ngınx](http://nginx.org), veya [Apache ](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="675b2-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="675b2-140">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alır ve bunları Kestrel için iletir.</span><span class="sxs-lookup"><span data-stu-id="675b2-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="675b2-142">Internet güvenlik olduğundan doğrudan sunulan genel kullanıma yönelik uç sunucusu dağıtımları için ters Ara sunucu kullanmak için en önemli nedeni.</span><span class="sxs-lookup"><span data-stu-id="675b2-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="675b2-143">Kestrel'i 1.x sürümlerini Internet'ten saldırılarına karşı korumak için önemli güvenlik özellikleri dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="675b2-144">Bu, içerir, ancak bunlarla sınırlı uygun bir zaman aşımı, istek boyutu sınırları ve eş zamanlı bağlantı sınırları değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="675b2-145">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="675b2-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="675b2-146">IIS ile Kestrel</span><span class="sxs-lookup"><span data-stu-id="675b2-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="675b2-147">Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), ASP.NET Core uygulaması ya da IIS çalışan işlemi ile aynı işlemde çalışan ( *işlem içi* barındırma modeli) veya ayrı bir işlemde IIS çalışan işlemi ( *işlem dışı* barındırma modeli).</span><span class="sxs-lookup"><span data-stu-id="675b2-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="675b2-148">[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlem içi IIS HTTP sunucusu ya da işlem dışı Kestrel sunucusu arasında yerel IIS istekleri işleyen yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="675b2-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="675b2-149">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="675b2-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="675b2-150">Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) IIS çalışan işleminden ayrı bir işlemde ASP.NET Core uygulaması ASP.NET Core için ters Ara sunucu çalışır.</span><span class="sxs-lookup"><span data-stu-id="675b2-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="675b2-151">IIS işleminde [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ters proxy ilişki düzenler.</span><span class="sxs-lookup"><span data-stu-id="675b2-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="675b2-152">ASP.NET Core modülü temel işlevleri, uygulamayı başlatın, bu kilitlendiğinde uygulamayı yeniden başlatın ve uygulama HTTP trafiği ilettiği üzeresiniz.</span><span class="sxs-lookup"><span data-stu-id="675b2-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="675b2-153">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="675b2-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="675b2-154">Ngınx Kestrel ile</span><span class="sxs-lookup"><span data-stu-id="675b2-154">Nginx with Kestrel</span></span>

<span data-ttu-id="675b2-155">Ngınx Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="675b2-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="675b2-156">Kestrel'i Apache</span><span class="sxs-lookup"><span data-stu-id="675b2-156">Apache with Kestrel</span></span>

<span data-ttu-id="675b2-157">Apache Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="675b2-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="675b2-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="675b2-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="675b2-159">Windows üzerinde çalışan ASP.NET Core uygulamaları, HTTP.sys Kestrel alternatif olur.</span><span class="sxs-lookup"><span data-stu-id="675b2-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="675b2-160">Kestrel'i genellikle en iyi performans için önerilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="675b2-161">HTTP.sys burada uygulama Internet erişimine açıktır ve gerekli özellikleri HTTP.sys ancak değil Kestrel tarafından desteklenen senaryolarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="675b2-162">Daha fazla bilgi için bkz. <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="675b2-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys doğrudan Internet ile iletişim kurar.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="675b2-164">HTTP.sys, yalnızca iç ağa sunulan uygulamalar için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys iç ağa ile doğrudan iletişim kurar.](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="675b2-166">HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="675b2-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="675b2-167">Windows üzerinde ASP.NET Core uygulamaları çalıştırırsanız, WebListener burada IIS ana bilgisayar uygulamaları için kullanılabilir senaryolar için alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="675b2-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="675b2-169">WebListener de Kestrel yerine yalnızca bir iç ağ için sunulan uygulamalar için özellikleri WebListener ancak değil Kestrel tarafından desteklenen gerekirse kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="675b2-170">WebListener hakkında daha fazla bilgi için bkz: [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="675b2-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener iç ağa ile doğrudan iletişim kurar.](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="675b2-172">ASP.NET Core sunucu altyapısı</span><span class="sxs-lookup"><span data-stu-id="675b2-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="675b2-173">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) bulunan `Startup.Configure` yöntemi kullanıma sunan [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) türünün özelliği [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="675b2-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="675b2-174">Kestrel ve HTTP.sys (WebListener içinde ASP.NET Core 1.x) yalnızca tek bir özelliği her kullanıma [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı bir sunucu uygulamaları, ek işlevler sunarsınız.</span><span class="sxs-lookup"><span data-stu-id="675b2-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="675b2-175">`IServerAddressesFeature` Sunucu Uygulama çalışma zamanında bağlı bağlantı noktasını bulmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="675b2-176">Özel sunucuları</span><span class="sxs-lookup"><span data-stu-id="675b2-176">Custom servers</span></span>

<span data-ttu-id="675b2-177">Özel sunucu uygulaması, yerleşik sunucuları uygulamanın gereksinimleri karşılamıyorsanız oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="675b2-178">[.NET (OWIN) kılavuzu için açık Web arabirimi](xref:fundamentals/owin) nasıl yazılacağını gösteren bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="675b2-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="675b2-179">Uygulamanın kullandığı özellik arabirimler uygulama, en az olsa gerektirir. yalnızca [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) desteklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="675b2-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="675b2-180">Sunucu başlangıç</span><span class="sxs-lookup"><span data-stu-id="675b2-180">Server startup</span></span>

<span data-ttu-id="675b2-181">Kullanırken [Visual Studio](https://www.visualstudio.com/vs/), [Mac için Visual Studio](https://www.visualstudio.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/), sunucu uygulama tarafından tümleşik geliştirme ortamı (olarak başlatıldığında başlatılır IDE).</span><span class="sxs-lookup"><span data-stu-id="675b2-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="675b2-182">Windows üzerinde Visual Studio'da başlatma profilleri uygulama ve sunucu ile başlatmak için kullanılabilir [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) veya Konsolu.</span><span class="sxs-lookup"><span data-stu-id="675b2-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="675b2-183">Visual Studio Code'da uygulama ve sunucu tarafından başlatılan [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), CoreCLR hata ayıklayıcı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="675b2-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="675b2-184">Mac için Visual Studio kullanarak, uygulama ve sunucu tarafından başlatılan [Mono modu geçici hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="675b2-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="675b2-185">Proje klasöründeki bir komut isteminden bir uygulamayı başlatırken [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulama ve sunucu (Kestrel ve yalnızca HTTP.sys) başlatır.</span><span class="sxs-lookup"><span data-stu-id="675b2-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="675b2-186">Yapılandırması tarafından belirtilen `-c|--configuration` ayarlandığından seçeneği `Debug` (varsayılan) veya `Release`.</span><span class="sxs-lookup"><span data-stu-id="675b2-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="675b2-187">Başlatma profili varsa bir *launchSettings.json* dosya, kullanın `--launch-profile <NAME>` başlatma profili ayarlamak için seçeneği (örneğin, `Development` veya `Production`).</span><span class="sxs-lookup"><span data-stu-id="675b2-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="675b2-188">Daha fazla bilgi için [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) ve [.NET Core dağıtımı paketleme](/dotnet/core/build/distribution-packaging) konuları.</span><span class="sxs-lookup"><span data-stu-id="675b2-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="675b2-189">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="675b2-189">HTTP/2 support</span></span>

<span data-ttu-id="675b2-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="675b2-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="675b2-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="675b2-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="675b2-192">İşletim sistemi</span><span class="sxs-lookup"><span data-stu-id="675b2-192">Operating system</span></span>
    * <span data-ttu-id="675b2-193">Windows Server 2016/Windows 10 veya üzeri&dagger;</span><span class="sxs-lookup"><span data-stu-id="675b2-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="675b2-194">Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="675b2-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="675b2-195">HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="675b2-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="675b2-196">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="675b2-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="675b2-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="675b2-198">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="675b2-199">Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="675b2-200">IIS (işlem içi)</span><span class="sxs-lookup"><span data-stu-id="675b2-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="675b2-201">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="675b2-202">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="675b2-203">IIS (giden işlem)</span><span class="sxs-lookup"><span data-stu-id="675b2-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="675b2-204">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="675b2-205">HTTP/2 genel kullanıma yönelik uç sunucu bağlantılarını kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.</span><span class="sxs-lookup"><span data-stu-id="675b2-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="675b2-206">Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="675b2-207">&dagger;Kestrel'i HTTP/2 Windows Server 2012 R2 ve Windows 8.1 için destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="675b2-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="675b2-208">Bu işletim sistemlerinde desteklenen TLS şifre paketlerinin listesini sınırlı olduğundan destek sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="675b2-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="675b2-209">Bir Eliptik Eğri Dijital imza algoritması (ECDSA) kullanılarak oluşturulan bir sertifika, TLS bağlantıları güvenli hale getirmek için gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="675b2-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="675b2-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="675b2-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="675b2-211">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="675b2-212">Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="675b2-213">IIS (giden işlem)</span><span class="sxs-lookup"><span data-stu-id="675b2-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="675b2-214">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="675b2-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="675b2-215">HTTP/2 genel kullanıma yönelik uç sunucu bağlantılarını kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.</span><span class="sxs-lookup"><span data-stu-id="675b2-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="675b2-216">Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="675b2-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="675b2-217">Bir HTTP/2 bağlantı kullanmalısınız [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) ve TLS 1.2 veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="675b2-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="675b2-218">Daha fazla bilgi için sunucu dağıtım senaryolarınız için ilgili konulara bakın.</span><span class="sxs-lookup"><span data-stu-id="675b2-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="675b2-219">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="675b2-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="675b2-220"><xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x sürümüne bkz <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="675b2-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
