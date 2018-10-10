---
title: ASP.NET Core Web sunucu uygulamalarında
author: rick-anderson
description: ASP.NET Core için web sunucuları Kestrel ve HTTP.sys keşfedin. Bir sunucu seçin ve ne zaman bir ters proxy sunucusu kullanmayı öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/index
ms.openlocfilehash: f9a6f1ee1d080732f6a379f5be791c9e225ae0a5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911953"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="df7a4-104">ASP.NET Core Web sunucu uygulamalarında</span><span class="sxs-lookup"><span data-stu-id="df7a4-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="df7a4-105">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), ve [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="df7a4-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="df7a4-106">ASP.NET Core uygulaması bir işlemde HTTP sunucusu uygulamasını ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="df7a4-107">HTTP istekleri ve bunları uygulamaya kümesi ortaya çıkarır sunucusu uygulaması dinler [istek özellikleri](xref:fundamentals/request-features) içine oluşan bir <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="df7a4-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="df7a4-108">ASP.NET Core üç sunucu uygulamaları ile birlikte gelir:</span><span class="sxs-lookup"><span data-stu-id="df7a4-108">ASP.NET Core ships three server implementations:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="df7a4-109">[Kestrel'i](xref:fundamentals/servers/kestrel) platformlar arası bir HTTP sunucusu, varsayılan ASP.NET Core için olan.</span><span class="sxs-lookup"><span data-stu-id="df7a4-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="df7a4-110">`IISHttpServer` ile kullanılan [işlem içi barındırma modeli](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) ve [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Windows üzerinde.</span><span class="sxs-lookup"><span data-stu-id="df7a4-110">`IISHttpServer` is used with the [in-process hosting model](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) on Windows.</span></span>
* <span data-ttu-id="df7a4-111">[HTTP.sys](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="df7a4-111">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="df7a4-112">(HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="df7a4-112">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="df7a4-113">[Kestrel'i](xref:fundamentals/servers/kestrel) platformlar arası bir HTTP sunucusu, varsayılan ASP.NET Core için olan.</span><span class="sxs-lookup"><span data-stu-id="df7a4-113">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="df7a4-114">[HTTP.sys](xref:fundamentals/servers/httpsys) yalnızca Windows HTTP sunucu dayanır [HTTP.sys çekirdek sürücüsü ve HTTP Sunucusu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="df7a4-114">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="df7a4-115">(HTTP.sys çağrılır [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.)</span><span class="sxs-lookup"><span data-stu-id="df7a4-115">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="df7a4-116">Kestrel'i</span><span class="sxs-lookup"><span data-stu-id="df7a4-116">Kestrel</span></span>

<span data-ttu-id="df7a4-117">Kestrel'i ASP.NET Core proje şablonları dahil varsayılan web sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="df7a4-117">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="df7a4-118">Tek başına veya birlikte kestrel kullanılabilir bir *ters Ara sunucu*IIS, Ngınx veya Apache gibi.</span><span class="sxs-lookup"><span data-stu-id="df7a4-118">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="df7a4-119">Ters Ara sunucu, Internet'ten HTTP isteklerini alır ve bunları Kestrel için bazı ön işleme sonra iletir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel'i ters Ara sunucu olmadan Internet ile doğrudan iletişim kurar](kestrel/_static/kestrel-to-internet2.png)

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="df7a4-122">Her iki yapılandırma&mdash;ile veya ters Ara sunucu olmadan&mdash;bir geçerli ve desteklenen barındırma ASP.NET Core 2.0 veya sonraki uygulamalar için bir yapılandırmadır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-122">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="df7a4-123">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="df7a4-123">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="df7a4-124">Uygulamayı yalnızca bir iç ağ gelen istekleri kabul ederse Kestrel kendisi tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-124">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel'i iç ağa ile doğrudan iletişim kurar.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="df7a4-126">Uygulamayı Internet erişimine açıktır, IIS, Ngınx veya Apache olarak Kestrel kullanmalısınız bir *ters Ara sunucu*.</span><span class="sxs-lookup"><span data-stu-id="df7a4-126">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="df7a4-127">Bir tersine Ara sunucunun Internet'ten HTTP isteklerini alıp Kestrel için aşağıdaki diyagramda gösterildiği gibi bazı ön işleme sonra ileten:</span><span class="sxs-lookup"><span data-stu-id="df7a4-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel'i dolaylı olarak IIS, Ngınx veya Apache gibi bir ters Ara sunucu üzerinden Internet ile iletişim kurar](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="df7a4-129">Güvenlik edge dağıtımları (trafiği Internet'ten kullanıma sunulur) için ters Ara sunucu kullanmak için en önemli nedenidir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-129">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="df7a4-130">Kestrel'i 1.x sürümlerini Internet'ten saldırılarına karşı korumak için önemli güvenlik özelliklerine sahip değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="df7a4-130">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="df7a4-131">Bu, içerir, ancak bunlarla sınırlı uygun bir zaman aşımı, istek boyutu sınırları ve eş zamanlı bağlantı sınırları değildir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-131">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="df7a4-132">Daha fazla bilgi için [Kestrel ters Ara sunucu ile kullanmak ne zaman](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="df7a4-132">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="df7a4-133">IIS, Ngınx ve Apache Kestrel kullanılamaz veya [özel sunucu uygulaması](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="df7a4-133">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="df7a4-134">ASP.NET Core platformlar arasında tutarlı bir şekilde davranabilir böylece kendi işlem içinde çalıştırmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-134">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="df7a4-135">IIS, Ngınx ve Apache kendi başlangıç yordamı ve ortam gerektirir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-135">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="df7a4-136">Bu sunucu teknolojileri kullanmak için doğrudan, ASP.NET Core her bir sunucunun gereksinimlerine uyum sağlamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-136">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="df7a4-137">Kestrel'i gibi bir web sunucusu uygulaması kullanarak ASP.NET Core başlatma işlemi ve farklı sunucu teknolojileri üzerinde barındırıldığında ortam üzerinde denetimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="df7a4-137">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="df7a4-138">IIS ile Kestrel</span><span class="sxs-lookup"><span data-stu-id="df7a4-138">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="df7a4-139">Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), ASP.NET Core uygulaması ya da IIS çalışan işlemi ile aynı işlemde çalışan ( *işlem içi* barındırma modeli) veya ayrı bir işlemde IIS çalışan işlemi ( *işlem dışı* barındırma modeli).</span><span class="sxs-lookup"><span data-stu-id="df7a4-139">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="df7a4-140">[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlem içi IIS Http sunucusu ya da işlem dışı Kestrel sunucusu arasında yerel IIS istekleri işleyen yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="df7a4-140">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS Http Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="df7a4-141">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="df7a4-141">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="df7a4-142">Kullanırken [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) IIS çalışan işleminden ayrı bir işlemde ASP.NET Core uygulaması ASP.NET Core için ters Ara sunucu çalışır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-142">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="df7a4-143">IIS işleminde [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) ters proxy ilişki düzenler.</span><span class="sxs-lookup"><span data-stu-id="df7a4-143">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="df7a4-144">ASP.NET Core modülü temel işlevleri, ASP.NET Core uygulaması başlatın, bu kilitlendiğinde uygulamayı yeniden başlatın ve uygulama HTTP trafiği ilettiği üzeresiniz.</span><span class="sxs-lookup"><span data-stu-id="df7a4-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="df7a4-145">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="df7a4-145">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="df7a4-146">Ngınx Kestrel ile</span><span class="sxs-lookup"><span data-stu-id="df7a4-146">Nginx with Kestrel</span></span>

<span data-ttu-id="df7a4-147">Ngınx Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Nginx ile Linux'ta barındırma](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="df7a4-147">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="df7a4-148">Kestrel'i Apache</span><span class="sxs-lookup"><span data-stu-id="df7a4-148">Apache with Kestrel</span></span>

<span data-ttu-id="df7a4-149">Apache Linux üzerinde bir ters proxy sunucusu olarak Kestrel için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Apache ile Linux'ta barındırma](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="df7a4-149">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="df7a4-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="df7a4-150">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="df7a4-151">Windows üzerinde çalışan ASP.NET Core uygulamaları, HTTP.sys Kestrel alternatif olur.</span><span class="sxs-lookup"><span data-stu-id="df7a4-151">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="df7a4-152">Kestrel'i genellikle en iyi performans için önerilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-152">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="df7a4-153">HTTP.sys burada uygulama Internet erişimine açıktır ve gerekli özellikleri HTTP.sys ancak değil Kestrel tarafından desteklenen senaryolarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-153">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="df7a4-154">HTTP.sys hakkında daha fazla bilgi için bkz: [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="df7a4-154">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys doğrudan Internet ile iletişim kurar.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="df7a4-156">HTTP.sys, yalnızca iç ağa sunulan uygulamalar için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-156">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys iç ağa ile doğrudan iletişim kurar.](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="df7a4-158">HTTP.sys adlı [WebListener](xref:fundamentals/servers/weblistener) içinde ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="df7a4-158">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="df7a4-159">Windows üzerinde ASP.NET Core uygulamaları çalıştırırsanız, WebListener burada IIS ana bilgisayar uygulamaları için kullanılabilir senaryolar için alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-159">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener doğrudan Internet ile iletişim kurar.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="df7a4-161">WebListener de Kestrel yerine yalnızca bir iç ağ için sunulan uygulamalar için özellikleri WebListener ancak değil Kestrel tarafından desteklenen gerekirse kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-161">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="df7a4-162">WebListener hakkında daha fazla bilgi için bkz: [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="df7a4-162">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener iç ağa ile doğrudan iletişim kurar.](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="df7a4-164">ASP.NET Core sunucu altyapısı</span><span class="sxs-lookup"><span data-stu-id="df7a4-164">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="df7a4-165">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) bulunan `Startup.Configure` yöntemi kullanıma sunan [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) türünün özelliği [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="df7a4-165">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="df7a4-166">Kestrel ve HTTP.sys (WebListener içinde ASP.NET Core 1.x) yalnızca tek bir özelliği her kullanıma [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ancak farklı bir sunucu uygulamaları, ek işlevler sunarsınız.</span><span class="sxs-lookup"><span data-stu-id="df7a4-166">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="df7a4-167">`IServerAddressesFeature` Sunucu Uygulama çalışma zamanında bağlı bağlantı noktasını bulmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-167">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="df7a4-168">Özel sunucuları</span><span class="sxs-lookup"><span data-stu-id="df7a4-168">Custom servers</span></span>

<span data-ttu-id="df7a4-169">Özel sunucu uygulaması, yerleşik sunucuları uygulamanın gereksinimleri karşılamıyorsanız oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-169">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="df7a4-170">[.NET (OWIN) kılavuzu için açık Web arabirimi](xref:fundamentals/owin) nasıl yazılacağını gösteren bir [Nowin](https://github.com/Bobris/Nowin)-tabanlı [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="df7a4-170">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="df7a4-171">Uygulamanın kullandığı özellik arabirimler uygulama, en az olsa gerektirir. yalnızca [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) ve [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) desteklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-171">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="df7a4-172">Sunucu başlangıç</span><span class="sxs-lookup"><span data-stu-id="df7a4-172">Server startup</span></span>

<span data-ttu-id="df7a4-173">Kullanırken [Visual Studio](https://www.visualstudio.com/vs/), [Mac için Visual Studio](https://www.visualstudio.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/), sunucu uygulama tarafından tümleşik geliştirme ortamı (olarak başlatıldığında başlatılır IDE).</span><span class="sxs-lookup"><span data-stu-id="df7a4-173">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="df7a4-174">Windows üzerinde Visual Studio'da başlatma profilleri uygulama ve sunucu ile başlatmak için kullanılabilir [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) veya Konsolu.</span><span class="sxs-lookup"><span data-stu-id="df7a4-174">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="df7a4-175">Visual Studio Code'da uygulama ve sunucu tarafından başlatılan [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), CoreCLR hata ayıklayıcı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-175">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="df7a4-176">Mac için Visual Studio kullanarak, uygulama ve sunucu tarafından başlatılan [Mono modu geçici hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="df7a4-176">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="df7a4-177">Proje klasöründeki bir komut isteminden bir uygulamayı başlatırken [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulama ve sunucu (Kestrel ve yalnızca HTTP.sys) başlatır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-177">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="df7a4-178">Yapılandırması tarafından belirtilen `-c|--configuration` ayarlandığından seçeneği `Debug` (varsayılan) veya `Release`.</span><span class="sxs-lookup"><span data-stu-id="df7a4-178">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="df7a4-179">Başlatma profili varsa bir *launchSettings.json* dosya, kullanın `--launch-profile <NAME>` başlatma profili ayarlamak için seçeneği (örneğin, `Development` veya `Production`).</span><span class="sxs-lookup"><span data-stu-id="df7a4-179">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="df7a4-180">Daha fazla bilgi için [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) ve [.NET Core dağıtımı paketleme](/dotnet/core/build/distribution-packaging) konuları.</span><span class="sxs-lookup"><span data-stu-id="df7a4-180">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="df7a4-181">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="df7a4-181">HTTP/2 support</span></span>

<span data-ttu-id="df7a4-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="df7a4-182">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="df7a4-183">Kestrel</span><span class="sxs-lookup"><span data-stu-id="df7a4-183">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="df7a4-184">İşletim sistemi</span><span class="sxs-lookup"><span data-stu-id="df7a4-184">Operating system</span></span>
    * <span data-ttu-id="df7a4-185">Windows Server 2012 R2/Windows 8.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-185">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="df7a4-186">Linux OpenSSL 1.0.2 veya daha sonra (örneğin, Ubuntu 16.04 veya üzeri)</span><span class="sxs-lookup"><span data-stu-id="df7a4-186">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="df7a4-187">HTTP/2 macos'ta gelecek sürümlerde desteklenecektir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-187">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="df7a4-188">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-188">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="df7a4-189">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="df7a4-189">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="df7a4-190">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-190">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="df7a4-191">Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-191">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="df7a4-192">IIS (işlem içi)</span><span class="sxs-lookup"><span data-stu-id="df7a4-192">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="df7a4-193">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-193">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="df7a4-194">Hedef çerçeve: .NET Core 2.2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-194">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="df7a4-195">IIS (giden işlem)</span><span class="sxs-lookup"><span data-stu-id="df7a4-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="df7a4-196">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="df7a4-197">HTTP/2 uç bağlantıları kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-197">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="df7a4-198">Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="df7a4-199">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="df7a4-199">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="df7a4-200">Windows Server 2016/Windows 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-200">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="df7a4-201">Hedef çerçeve: HTTP.sys dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-201">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="df7a4-202">IIS (giden işlem)</span><span class="sxs-lookup"><span data-stu-id="df7a4-202">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="df7a4-203">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="df7a4-203">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="df7a4-204">HTTP/2 uç bağlantıları kullanın, ancak HTTP/1.1 Kestrel ters proxy bağlantı kullanır.</span><span class="sxs-lookup"><span data-stu-id="df7a4-204">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="df7a4-205">Hedef çerçeve: IIS işlem dışı dağıtımlar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="df7a4-205">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="df7a4-206">Bir HTTP/2 bağlantı kullanmalısınız [uygulama katmanı protokol anlaşması (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) ve TLS 1.2 veya sonraki bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="df7a4-206">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="df7a4-207">Daha fazla bilgi için sunucu dağıtım senaryolarınız için ilgili konulara bakın.</span><span class="sxs-lookup"><span data-stu-id="df7a4-207">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df7a4-208">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="df7a4-208">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="df7a4-209"><xref:fundamentals/servers/httpsys> (ASP.NET Core 1.x sürümüne bkz <xref:fundamentals/servers/weblistener>)</span><span class="sxs-lookup"><span data-stu-id="df7a4-209"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
