---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: bf035bc65f0f120f52e55effe4d413bfecdf735d
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75952080"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="a077f-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="a077f-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="a077f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a077f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a077f-105">ASP.NET Core uygulamasını bir IIS sunucusuna yayımlamaya yönelik bir öğretici deneyimi için, bkz. <xref:tutorials/publish-to-iis>.</span><span class="sxs-lookup"><span data-stu-id="a077f-105">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="a077f-106">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="a077f-106">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="a077f-107">Supported operating systems</span><span class="sxs-lookup"><span data-stu-id="a077f-107">Supported operating systems</span></span>

<span data-ttu-id="a077f-108">Aşağıdaki işletim sistemleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="a077f-108">The following operating systems are supported:</span></span>

* <span data-ttu-id="a077f-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a077f-109">Windows 7 or later</span></span>
* <span data-ttu-id="a077f-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a077f-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a077f-111">[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eskiden webListener olarak adlandırılır), IIS ile ters proxy yapılandırmasında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="a077f-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="a077f-112">Kullanım [Kestrel sunucu](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="a077f-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="a077f-113">Azure'da barındırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="a077f-113">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="a077f-114">Sorun giderme kılavuzu için bkz. <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="a077f-114">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a077f-115">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="a077f-115">Supported platforms</span></span>

<span data-ttu-id="a077f-116">32-bit (x86) veya 64 bit (x64) dağıtımı için yayımlanan uygulamalar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-116">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="a077f-117">Uygulama dışında 32 bitlik bir uygulamayı 32 bit (x86) .NET Core SDK dağıtma:</span><span class="sxs-lookup"><span data-stu-id="a077f-117">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="a077f-118">64 bitlik bir uygulama için kullanılabilir daha büyük sanal bellek adres alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-118">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="a077f-119">Daha büyük IIS yığın boyutunu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-119">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="a077f-120">64 bitlik yerel bağımlılıklara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a077f-120">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="a077f-121">64 bit uygulama yayımlamak için 64 bit (x64) .NET Core SDK kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-121">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="a077f-122">Ana bilgisayar sisteminde 64 bit çalışma zamanı bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-122">A 64-bit runtime must be present on the host system.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-models"></a><span data-ttu-id="a077f-123">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="a077f-123">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a077f-124">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="a077f-124">In-process hosting model</span></span>

<span data-ttu-id="a077f-125">ASP.NET Core bir uygulama, işlem içi barındırma kullanarak IIS çalışan işlemiyle aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a077f-125">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="a077f-126">İşlem içi barındırma, istek dışı barındırmak için gelişmiş performans sağlar çünkü istekler, giden ağ trafiği ile aynı makineye geri döndürülen bir ağ arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-126">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="a077f-127">IIS, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)ile işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="a077f-127">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a077f-128">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="a077f-128">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="a077f-129">Uygulama başlatmayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-129">Performs app initialization.</span></span>
  * <span data-ttu-id="a077f-130">[CoreCLR](/dotnet/standard/glossary#coreclr)'yi yükler.</span><span class="sxs-lookup"><span data-stu-id="a077f-130">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="a077f-131">`Program.Main`çağırır.</span><span class="sxs-lookup"><span data-stu-id="a077f-131">Calls `Program.Main`.</span></span>
* <span data-ttu-id="a077f-132">IIS yerel isteğinin ömrünü işler.</span><span class="sxs-lookup"><span data-stu-id="a077f-132">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="a077f-133">İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a077f-133">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="a077f-134">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve süreçte barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a077f-134">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![İşlem içi barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-inprocess.png)

<span data-ttu-id="a077f-136">Web 'den çekirdek modu HTTP. sys sürücüsüne bir istek ulaşır.</span><span class="sxs-lookup"><span data-stu-id="a077f-136">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a077f-137">Sürücü, yerel isteği Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a077f-137">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a077f-138">ASP.NET Core modülü yerel isteği alır ve IIS HTTP sunucusuna (`IISHttpServer`) geçirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-138">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="a077f-139">IIS HTTP sunucusu, isteği yerelden yönetilene dönüştüren bir IIS için işlem içi sunucu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-139">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="a077f-140">IIS HTTP sunucusu isteği işlediğinde, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-140">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a077f-141">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-141">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a077f-142">Uygulamanın yanıtı IIS HTTP sunucusu aracılığıyla IIS 'e geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-142">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="a077f-143">IIS yanıtı, isteği başlatan istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="a077f-143">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="a077f-144">İşlem içi barındırma, mevcut uygulamalar için kabul ediyor, ancak tüm IIS ve IIS Express senaryoları için varsayılan [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar, işlem içi barındırma modeli için varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-144">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="a077f-145">`CreateDefaultBuilder`, [CoreCLR](/dotnet/standard/glossary#coreclr) 'yi önyüklemek ve uygulamayı IIS çalışan işleminin (*W3wp. exe* veya *iisexpress. exe*) içinde barındırmak için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> metodunu çağırarak bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> örneği ekler.</span><span class="sxs-lookup"><span data-stu-id="a077f-145">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="a077f-146">Performans testleri belirten bir .NET Core uygulaması işlem içi barındırma için uygulama işlem dışı ve proxy isteklerini barındırma kıyasla önemli ölçüde daha yüksek istek üretilen işini teslim [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="a077f-146">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!NOTE]
> <span data-ttu-id="a077f-147">Tek bir dosya yürütülebilir dosyası olarak yayınlanan uygulamalar, işlem içi barındırma modeliyle yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="a077f-147">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a077f-148">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="a077f-148">Out-of-process hosting model</span></span>

<span data-ttu-id="a077f-149">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, ASP.NET Core modülü işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="a077f-149">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="a077f-150">Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="a077f-150">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a077f-151">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-151">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a077f-152">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a077f-152">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![İşlem dışı barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-outofprocess.png)

<span data-ttu-id="a077f-154">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="a077f-154">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a077f-155">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a077f-155">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a077f-156">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="a077f-156">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a077f-157">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> uzantısı sunucuyu `http://localhost:{PORT}`dinlemek üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a077f-157">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="a077f-158">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-158">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a077f-159">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-159">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a077f-160">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-160">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a077f-161">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-161">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a077f-162">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-162">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a077f-163">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-163">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a077f-164">ASP.NET Core, varsayılan, platformlar arası bir HTTP sunucusu olan [Kestrel Server](xref:fundamentals/servers/kestrel)ile birlikte gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-164">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="a077f-165">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)kullanırken, uygulama IIS çalışan işleminden (*işlem dışı*) [Kestrel sunucusu](xref:fundamentals/servers/index#kestrel)ile ayrı bir işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="a077f-165">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="a077f-166">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="a077f-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="a077f-167">Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="a077f-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="a077f-168">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-168">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a077f-169">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a077f-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core Modülü](index/_static/ancm-outofprocess.png)

<span data-ttu-id="a077f-171">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="a077f-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a077f-172">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a077f-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a077f-173">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="a077f-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a077f-174">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu `http://localhost:{port}`dinlemek üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a077f-174">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a077f-175">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a077f-176">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a077f-177">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a077f-178">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a077f-179">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a077f-180">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a077f-181">`CreateDefaultBuilder` yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve bağlantı noktası ve temel yolunu yapılandırarak IIS tümleştirme sağlar [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a077f-181">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a077f-182">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a077f-182">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="a077f-183">`CreateDefaultBuilder` çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a077f-183">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="a077f-184">`UseIISIntegration` Kestrel'i localhost IP adresi dinamik bir bağlantı noktası dinleyecek şekilde yapılandırır (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="a077f-184">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="a077f-185">Dinamik bağlantı noktası 1234 ise sırasında Kestrel dinler `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="a077f-185">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="a077f-186">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="a077f-186">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="a077f-187">Kestrel'i'nın dinleme API</span><span class="sxs-lookup"><span data-stu-id="a077f-187">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="a077f-188">[Yapılandırma](xref:fundamentals/configuration/index) (veya [--URL'leri komut satırı seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="a077f-188">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="a077f-189">Çağrılar `UseUrls` veya Kestrel'ın `Listen` Modülü'nü kullanırken API gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a077f-189">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="a077f-190">Varsa `UseUrls` veya `Listen` çağrılır, yalnızca IIS gerekmeden uygulamayı çalıştırırken belirtilen bağlantı noktasında dinleyen Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a077f-190">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

<span data-ttu-id="a077f-191">ASP.NET Core modülü yapılandırma kılavuzu için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a077f-191">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="a077f-192">Barındırma ile ilgili daha fazla bilgi için bkz: [ASP.NET Core ana](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="a077f-192">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="a077f-193">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a077f-193">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="a077f-194">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="a077f-194">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a077f-195">`CreateHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="a077f-195">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="a077f-196">`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#default-builder-settings>.</span><span class="sxs-lookup"><span data-stu-id="a077f-196">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a077f-197">`CreateWebHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="a077f-197">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="a077f-198">`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="a077f-198">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

::: moniker-end

### <a name="iis-options"></a><span data-ttu-id="a077f-199">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="a077f-199">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a077f-200">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="a077f-200">**In-process hosting model**</span></span>

<span data-ttu-id="a077f-201">IIS sunucu seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISServerOptions> için bir hizmet yapılandırması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-201">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="a077f-202">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="a077f-202">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="a077f-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="a077f-203">Option</span></span>                         | <span data-ttu-id="a077f-204">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a077f-204">Default</span></span> | <span data-ttu-id="a077f-205">Ayar</span><span class="sxs-lookup"><span data-stu-id="a077f-205">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a077f-206">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-206">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a077f-207">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a077f-207">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a077f-208">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="a077f-208">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a077f-209">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-209">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a077f-210">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-210">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="a077f-211">`HttpContext.Request` ve `HttpContext.Response`için zaman uyumlu GÇ izin verilip verilmeyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="a077f-211">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="a077f-212">`HttpRequest`için en büyük istek gövdesi boyutunu alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-212">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="a077f-213">IIS 'nin `IISServerOptions``MaxRequestBodySize` ayarlamadan önce işlenecek sınır `maxAllowedContentLength` sahip olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-213">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="a077f-214">`MaxRequestBodySize` değiştirmek `maxAllowedContentLength`etkilemez.</span><span class="sxs-lookup"><span data-stu-id="a077f-214">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="a077f-215">`maxAllowedContentLength`artırmak için, *Web. config* dosyasına daha yüksek bir değere `maxAllowedContentLength` ayarlamak üzere bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-215">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="a077f-216">Daha fazla ayrıntı için bkz. [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span><span class="sxs-lookup"><span data-stu-id="a077f-216">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="a077f-217">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="a077f-217">**Out-of-process hosting model**</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="a077f-218">Seçenek</span><span class="sxs-lookup"><span data-stu-id="a077f-218">Option</span></span>                         | <span data-ttu-id="a077f-219">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a077f-219">Default</span></span> | <span data-ttu-id="a077f-220">Ayar</span><span class="sxs-lookup"><span data-stu-id="a077f-220">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a077f-221">Varsa `true`, IIS sunucusu ayarlar `HttpContext.User` tarafından kimliği doğrulanmış [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-221">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a077f-222">Varsa `false`, sunucu için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a077f-222">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a077f-223">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="a077f-223">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a077f-224">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-224">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a077f-225">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-225">Sets the display name shown to users on login pages.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a077f-226">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="a077f-226">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="a077f-227">IIS seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISOptions> için bir hizmet yapılandırması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-227">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="a077f-228">Aşağıdaki örnek uygulamayı doldurmasını önler `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="a077f-228">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="a077f-229">Seçenek</span><span class="sxs-lookup"><span data-stu-id="a077f-229">Option</span></span>                         | <span data-ttu-id="a077f-230">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a077f-230">Default</span></span> | <span data-ttu-id="a077f-231">Ayar</span><span class="sxs-lookup"><span data-stu-id="a077f-231">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="a077f-232">`true`, [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components) [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-232">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="a077f-233">Varsa `false`, ara yazılım için bir kimlik yalnızca sağlar `HttpContext.User` ve açıkça tarafından istendiğinde zorlukları yanıtlar `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="a077f-233">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="a077f-234">Windows kimlik doğrulaması etkin, IIS için `AutomaticAuthentication` işlevi.</span><span class="sxs-lookup"><span data-stu-id="a077f-234">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="a077f-235">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konu.</span><span class="sxs-lookup"><span data-stu-id="a077f-235">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="a077f-236">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-236">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="a077f-237">Varsa `true` ve `MS-ASPNETCORE-CLIENTCERT` istek üstbilgisi mevcutsa, `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="a077f-237">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a077f-238">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="a077f-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a077f-239">Iletilen üstbilgiler ara yazılımını yapılandıran [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components)ve ASP.NET Core modülü, DÜZENI (http/https) ve isteğin KAYNAKLANDıĞı uzak IP adresini iletecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="a077f-239">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="a077f-240">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-240">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="a077f-241">Daha fazla bilgi için [proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="a077f-241">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="a077f-242">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="a077f-242">web.config file</span></span>

<span data-ttu-id="a077f-243">*Web.config* dosya yapılandırır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a077f-243">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="a077f-244">Oluşturma, dönüştürme ve yayımlama *web.config* dosya bir MSBuild hedefi tarafından gerçekleştirilir (`_TransformWebConfig`) projeyi ne zaman yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-244">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="a077f-245">Bu Web SDK'sı hedeflerin mevcut hedefidir (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="a077f-245">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="a077f-246">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="a077f-246">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="a077f-247">Bir *Web. config* dosyası projede yoksa, dosya ASP.NET Core modülünü yapılandırmak Için doğru *processPath* ve *bağımsız değişkenlerle* oluşturulur ve [yayımlanan çıktıya](xref:host-and-deploy/directory-structure)taşınır.</span><span class="sxs-lookup"><span data-stu-id="a077f-247">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="a077f-248">Varsa bir *web.config* dosyası projesinde, dosyanın doğru dönüştürülür *processPath* ve *bağımsız değişkenleri* ASP.NET Core modülü yapılandırmak için ve azure'a taşındı yayımlanmış çıktısı.</span><span class="sxs-lookup"><span data-stu-id="a077f-248">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="a077f-249">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="a077f-249">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="a077f-250">*Web.config* etkin IIS modüllerini denetleyen ek IIS yapılandırması ayarları dosyası sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-250">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="a077f-251">ASP.NET Core uygulamaları istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz: [IIS modüllerini](xref:host-and-deploy/iis/modules) konu.</span><span class="sxs-lookup"><span data-stu-id="a077f-251">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="a077f-252">Dönüştürme gelen Web SDK'sı önlemek için *web.config* dosya, kullanın **\<IsTransformWebConfigDisabled >** özelliği proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="a077f-252">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="a077f-253">Dosya dönüştürme gelen Web SDK'yı devre dışı bırakılırken *processPath* ve *bağımsız değişkenleri* geliştirici tarafından el ile ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a077f-253">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="a077f-254">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a077f-254">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="a077f-255">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="a077f-255">web.config file location</span></span>

<span data-ttu-id="a077f-256">[ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) doğru bir şekilde ayarlamak için, *Web. config* dosyası dağıtılan uygulamanın [içerik kök](xref:fundamentals/index#content-root) yolunda (genellikle uygulama temel yolu) bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-256">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="a077f-257">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="a077f-257">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="a077f-258">*Web.config* dosya, Web dağıtımı kullanarak birden fazla uygulama yayımlamayı etkinleştirmek için uygulamanın kök dizininde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-258">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="a077f-259">Mevcut uygulamanın fiziksel yola gibi hassas dosyalar  *\<derleme >. runtimeconfig.json*,  *\<derleme > .xml* (XML belgeleri açıklamaları) ve  *\<derleme >. deps.json*.</span><span class="sxs-lookup"><span data-stu-id="a077f-259">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="a077f-260">*Web. config* dosyası mevcut olduğunda ve site normal olarak başlatıldığında, istenirse IIS bu hassas dosyaları sunmaz.</span><span class="sxs-lookup"><span data-stu-id="a077f-260">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="a077f-261">Varsa *web.config* dosyası eksik, yanlış adlandırılan veya site için normal başlangıç yapılandırılamıyor, IIS hassas dosyalar genel olarak hizmet verebilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-261">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="a077f-262">***Web. config* dosyasının her zaman dağıtımda mevcut olması, doğru şekilde adlandırılması ve siteyi normal başlangıç için yapılandırabiliyor olması gerekir. *Web. config* dosyasını bir üretim dağıtımından hiçbir şekilde kaldırmayın.**</span><span class="sxs-lookup"><span data-stu-id="a077f-262">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="a077f-263">Web.config’i dönüştürme</span><span class="sxs-lookup"><span data-stu-id="a077f-263">Transform web.config</span></span>

<span data-ttu-id="a077f-264">Yayımlama sırasında *Web. config* ' i dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz. <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="a077f-264">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="a077f-265">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="a077f-265">IIS configuration</span></span>

<span data-ttu-id="a077f-266">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="a077f-266">**Windows Server operating systems**</span></span>

<span data-ttu-id="a077f-267">Etkinleştirme **Web sunucusu (IIS)** sunucu rolü ve rol hizmetlerini oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a077f-267">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="a077f-268">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="a077f-268">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="a077f-269">Üzerinde **sunucu rolleri** kutusunu işaretleyin, adım **Web sunucusu (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="a077f-269">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="a077f-271">Sonra **özellikleri** adım **rol hizmetlerini** adım, Web sunucusu (IIS) yükler.</span><span class="sxs-lookup"><span data-stu-id="a077f-271">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="a077f-272">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="a077f-272">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="a077f-274">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="a077f-274">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="a077f-275">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="a077f-275">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="a077f-276">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a077f-276">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="a077f-277">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-277">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="a077f-278">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="a077f-278">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="a077f-279">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-279">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="a077f-280">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="a077f-280">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="a077f-281">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a077f-281">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="a077f-282">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="a077f-282">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="a077f-283">Aracılığıyla devam **onay** Hizmetleri ve web sunucusu rolü yüklemek için adım.</span><span class="sxs-lookup"><span data-stu-id="a077f-283">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="a077f-284">Yükledikten sonra sunucu/IIS yeniden başlatma gerekli değilse **Web sunucusu (IIS)** rol.</span><span class="sxs-lookup"><span data-stu-id="a077f-284">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="a077f-285">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="a077f-285">**Windows desktop operating systems**</span></span>

<span data-ttu-id="a077f-286">Etkinleştirme **IIS Yönetim Konsolu** ve **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="a077f-286">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="a077f-287">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="a077f-287">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="a077f-288">Açık **Internet Information Services** düğümü.</span><span class="sxs-lookup"><span data-stu-id="a077f-288">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="a077f-289">Açık **Web yönetimi araçları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="a077f-289">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="a077f-290">İçin kutuyu **IIS Yönetim Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a077f-290">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="a077f-291">İçin kutuyu **World Wide Web Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="a077f-291">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="a077f-292">Varsayılan özelliklerini kabul **World Wide Web Hizmetleri** veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a077f-292">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="a077f-293">**Windows kimlik doğrulaması (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="a077f-293">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="a077f-294">Windows kimlik doğrulamasını etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="a077f-294">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="a077f-295">Seçin **Windows kimlik doğrulaması** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a077f-295">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="a077f-296">Daha fazla bilgi için [Windows kimlik doğrulaması \<ServiceCredentials >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-296">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="a077f-297">**WebSockets (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="a077f-297">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="a077f-298">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-298">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="a077f-299">WebSockets etkinleştirmek için aşağıdaki düğümleri genişletin: **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="a077f-299">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="a077f-300">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a077f-300">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="a077f-301">Daha fazla bilgi için [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="a077f-301">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="a077f-302">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a077f-302">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="a077f-304">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="a077f-304">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="a077f-305">Yükleme *.NET Core barındırma paket* barındıran sistemde.</span><span class="sxs-lookup"><span data-stu-id="a077f-305">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="a077f-306">.NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a077f-306">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="a077f-307">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-307">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a077f-308">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-308">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="a077f-309">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a077f-309">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="a077f-310">.NET Core 'un 64 bit (x64) sürümünü yükledikten sonra barındırma paketi yüklenirse, SDK 'lar eksik gibi görünebilir ([hiçbir .NET Core SDK 'sı algılanmadı](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="a077f-310">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="a077f-311">Sorunu çözmek için bkz. <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span><span class="sxs-lookup"><span data-stu-id="a077f-311">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="a077f-312">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="a077f-312">Direct download (current version)</span></span>

<span data-ttu-id="a077f-313">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="a077f-313">Download the installer using the following link:</span></span>

[<span data-ttu-id="a077f-314">Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="a077f-314">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="a077f-315">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="a077f-315">Earlier versions of the installer</span></span>

<span data-ttu-id="a077f-316">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="a077f-316">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="a077f-317">Gidin [.NET indirme arşivleri](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="a077f-317">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="a077f-318">Altında **.NET Core**, .NET Core sürümünüzü seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-318">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="a077f-319">İçinde **uygulamaları - çalışma zamanı çalıştırma** sütun, istenen .NET Core çalışma zamanı sürümü satırını bulur.</span><span class="sxs-lookup"><span data-stu-id="a077f-319">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="a077f-320">Kullanarak yükleyiciyi indirin **çalışma zamanı & paketi barındırma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="a077f-320">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="a077f-321">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a077f-321">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="a077f-322">Daha fazla bilgi için [Destek İlkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="a077f-322">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="a077f-323">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="a077f-323">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="a077f-324">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a077f-324">Run the installer on the server.</span></span> <span data-ttu-id="a077f-325">Aşağıdaki parametreler, yükleyiciyi yönetici komut kabuğu 'ndan çalıştırırken kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="a077f-325">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="a077f-326">`OPT_NO_ANCM=1` &ndash; ASP.NET Core modülü yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="a077f-326">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="a077f-327">`OPT_NO_RUNTIME=1` &ndash; .NET Core çalışma zamanı yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="a077f-327">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="a077f-328">Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-328">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="a077f-329">`OPT_NO_SHAREDFX=1` &ndash; ASP.NET paylaşılan Framework (ASP.NET çalışma zamanı) yükleme atlanıyor.</span><span class="sxs-lookup"><span data-stu-id="a077f-329">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="a077f-330">Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-330">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="a077f-331">`OPT_NO_X86=1` &ndash; X86 yükleme atlanıyor çalışma zamanları.</span><span class="sxs-lookup"><span data-stu-id="a077f-331">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="a077f-332">32 bitlik uygulamalar barındırmayabildiğinizi bildiğiniz durumlarda bu parametreyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-332">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="a077f-333">Gelecekte 32-bit ve 64 bit uygulamaları barındırabilmeniz gereken herhangi bir şansınız varsa, bu parametreyi kullanmayın ve her iki çalışma zamanını da yüklemeyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-333">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="a077f-334">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash;, paylaşılan yapılandırma (*ApplicationHost. config*) IIS yüklemesiyle aynı MAKINELI olduğunda IIS paylaşılan yapılandırması kullanma denetimini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="a077f-334">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="a077f-335">*Yalnızca ASP.NET Core 2,2 veya sonraki bir sürümü Paketcisi yükleyicilerini barındırmak için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="a077f-335">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="a077f-336">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a077f-336">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="a077f-337">Sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="a077f-337">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="a077f-338">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="a077f-338">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a077f-339">ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için geri iletme davranışını benimsemez.</span><span class="sxs-lookup"><span data-stu-id="a077f-339">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="a077f-340">Yeni bir barındırma paketi yükleyerek paylaşılan çerçeveyi yükselttikten sonra, sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="a077f-340">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a077f-341">Barındırma paketini yüklerken IIS 'deki bireysel siteleri el ile durdurmanız gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="a077f-341">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="a077f-342">Barındırılan uygulamalar (IIS siteleri) IIS yeniden başlatıldığında yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-342">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="a077f-343">Uygulamalar, [uygulama başlatma modülünden](#application-initialization-module-and-idle-timeout)da dahil olmak üzere ilk isteklerini alırken yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-343">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="a077f-344">ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için ileri sarma davranışını benimseme.</span><span class="sxs-lookup"><span data-stu-id="a077f-344">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="a077f-345">IIS ile IIS tarafından barındırılan uygulamalar IIS ile yeniden başlatıldığında, ilk isteklerini alırken başvurulan paketlerinin en son düzeltme eki sürümleriyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-345">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="a077f-346">IIS yeniden başlatılırsa, uygulamalar yeniden başlatılır ve çalışan işlemlerine geri dönüştürüldüğünde geri alma davranışını gösterir ve ilk isteklerini alırlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-346">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a077f-347">IIS paylaşılan yapılandırması hakkında daha fazla bilgi için bkz: [IIS paylaşılan yapılandırması ile ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="a077f-347">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="a077f-348">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="a077f-348">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="a077f-349">Uygulamaları olan sunuculara dağıtırken [Web dağıtımı](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), sunucuda Web Dağıtımı'nın en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-349">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="a077f-350">Web Dağıtımı'nı yüklemek için kullanın [Web Platformu Yükleyicisi (Webpı)](https://www.microsoft.com/web/downloads/platform.aspx) veya doğrudan bir yükleyicisini [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="a077f-350">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="a077f-351">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a077f-351">The preferred method is to use WebPI.</span></span> <span data-ttu-id="a077f-352">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-352">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="a077f-353">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="a077f-353">Create the IIS site</span></span>

1. <span data-ttu-id="a077f-354">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a077f-354">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="a077f-355">Aşağıdaki adımda, klasörün yolu, uygulamanın fiziksel yolu olarak IIS 'ye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-355">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="a077f-356">Uygulamanın dağıtım klasörü ve dosya düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="a077f-356">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="a077f-357">IIS Yöneticisi 'nde, **Bağlantılar** panelinde sunucunun düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-357">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="a077f-358">Sağ **siteleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="a077f-358">Right-click the **Sites** folder.</span></span> <span data-ttu-id="a077f-359">Seçin **Web sitesi Ekle** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="a077f-359">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="a077f-360">Sağlayan bir **Site adı** ayarlayıp **fiziksel yolu** uygulamanın dağıtım klasörüne.</span><span class="sxs-lookup"><span data-stu-id="a077f-360">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="a077f-361">Sağlamak **bağlama** yapılandırma ve seçerek Web sitesi oluşturma **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="a077f-361">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="a077f-363">Üst düzey joker bağlamaları (`http://*:80/` ve `http://+:80`) gereken **değil** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-363">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="a077f-364">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a077f-364">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="a077f-365">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-365">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="a077f-366">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-366">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="a077f-367">Alt etki alanı joker bağlama (örneğin, `*.mysub.com`) tüm üst etki alanını denetimi bu güvenlik riski yok (başlangıcı yerine sonundan `*.com`, güvenlik açığı olan).</span><span class="sxs-lookup"><span data-stu-id="a077f-367">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="a077f-368">Bkz: [rfc7230 bölümü-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="a077f-368">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="a077f-369">Sunucu düğümü altında seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="a077f-369">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="a077f-370">Sitenin uygulama havuzu sağ tıklayıp **temel ayarları** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="a077f-370">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="a077f-371">İçinde **uygulama havuzunu Düzenle** penceresinde **.NET CLR sürümü** için **yönetilen kod yok**:</span><span class="sxs-lookup"><span data-stu-id="a077f-371">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="a077f-373">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="a077f-373">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="a077f-374">ASP.NET Core, masaüstü CLR 'nin (.NET CLR) yüklenmemesine&mdash;.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı çalışan işlemde barındırmak için önyüklenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-374">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="a077f-375">**.NET CLR sürümünün** **yönetilen kod olmadan** ayarlanması isteğe bağlıdır, ancak önerilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-375">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="a077f-376">*ASP.NET Core 2.2 veya üzeri*: için bir 64 bit (x 64) [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) kullanan [işlem içi barındırma modeli](#in-process-hosting-model), 32 bit (x 86) işlemleri için uygulama havuzunu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="a077f-376">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="a077f-377">IIS Yöneticisi > **uygulama havuzlarının** **Eylemler** kenar çubuğunda, **uygulama havuzu varsayılanlarını** veya **Gelişmiş ayarları**ayarla ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-377">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="a077f-378">Bulun **etkinleştirme 32-Bit uygulamaları** ve değerine `False`.</span><span class="sxs-lookup"><span data-stu-id="a077f-378">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="a077f-379">Bu ayar için dağıtılan uygulamaları etkilemez [barındırma işlemi çıkış](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a077f-379">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="a077f-380">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-380">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="a077f-381">Varsayılan uygulama havuzu kimliğini (**işlem modeli** > **kimlik**) değiştirilirse **ApplicationPoolIdentity** başka bir kimlik için doğrulayın Yeni kimlik uygulamanın klasörünü, veritabanı ve diğer gerekli kaynakları erişmek için gerekli izinlere sahip.</span><span class="sxs-lookup"><span data-stu-id="a077f-381">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="a077f-382">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a077f-382">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="a077f-383">**Windows kimlik doğrulaması yapılandırma (isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="a077f-383">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="a077f-384">Daha fazla bilgi için [yapılandırma Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="a077f-384">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="a077f-385">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="a077f-385">Deploy the app</span></span>

<span data-ttu-id="a077f-386">Uygulamayı [IIS sitesi oluşturma](#create-the-iis-site) bölümünde oluşturulan IIS **fiziksel yol** klasörüne dağıtın.</span><span class="sxs-lookup"><span data-stu-id="a077f-386">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="a077f-387">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtım için önerilen mekanizmadır, ancak uygulamayı projenin *Publish* klasöründen barındırma sisteminin dağıtım klasörüne taşımak için çeşitli seçenekler mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="a077f-387">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="a077f-388">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="a077f-388">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="a077f-389">Bkz: [uygulama dağıtımı ASP.NET Core için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konu için Web dağıtımı ile kullanmak için bir yayımlama profili oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a077f-389">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="a077f-390">Barındırma sağlayıcısı, bir yayımlama profili veya oluşturma desteği sağlıyorsa, profilini indirin ve Visual Studio **Yayımlama** iletişim kutusunu kullanarak içeri aktarın:</span><span class="sxs-lookup"><span data-stu-id="a077f-390">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="a077f-392">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="a077f-392">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="a077f-393">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) Visual Studio'nun dışında komut satırından da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-393">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="a077f-394">Daha fazla bilgi için [Web dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="a077f-394">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="a077f-395">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="a077f-395">Alternatives to Web Deploy</span></span>

<span data-ttu-id="a077f-396">Uygulamayı barındırma sistemine taşımak için el ile kopyalama, [xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy)veya [PowerShell](/powershell/)gibi çeşitli yöntemlerden birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-396">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="a077f-397">IIS'ye ASP.NET Core dağıtımı hakkında daha fazla bilgi için bkz. [IIS Yöneticiler için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a077f-397">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="a077f-398">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="a077f-398">Browse the website</span></span>

<span data-ttu-id="a077f-399">Uygulama barındırma sistemine dağıtıldıktan sonra, uygulamanın genel uç noktalarından birine bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a077f-399">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="a077f-400">Aşağıdaki örnekte site, **bağlantı noktası** `80``www.mysite.com` IIS **ana bilgisayar adına** bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-400">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="a077f-401">`http://www.mysite.com`bir istek yapıldı:</span><span class="sxs-lookup"><span data-stu-id="a077f-401">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="a077f-403">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="a077f-403">Locked deployment files</span></span>

<span data-ttu-id="a077f-404">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="a077f-404">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="a077f-405">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="a077f-405">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="a077f-406">Uygulama havuzunu kullanan bir dağıtımda kilitli dosyalar serbest bırakmak için Durdur **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="a077f-406">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="a077f-407">Web dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="a077f-407">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="a077f-408">Bir *app_offline.htm* dosya, web uygulama dizini kökünde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-408">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="a077f-409">Dosya varsa, ASP.NET Core modülü düzgün bir şekilde uygulamayı kapatır ve hizmet *app_offline.htm* dağıtımı sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="a077f-409">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="a077f-410">Daha fazla bilgi için [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="a077f-410">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="a077f-411">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="a077f-411">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="a077f-412">*App_offline. htm* dosyasını bırakmak için PowerShell kullanın (PowerShell 5 veya üzerini gerektirir):</span><span class="sxs-lookup"><span data-stu-id="a077f-412">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="a077f-413">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="a077f-413">Data protection</span></span>

<span data-ttu-id="a077f-414">[ASP.NET Core veri koruma yığın](xref:security/data-protection/introduction) birkaç ASP.NET Core tarafından kullanılan [middlewares](xref:fundamentals/middleware/index), kimlik doğrulamasında kullanılan ara yazılımı dahil olmak üzere.</span><span class="sxs-lookup"><span data-stu-id="a077f-414">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="a077f-415">Veri koruma API'lerini kullanıcı kodu tarafından çağrılan değildir olsa bile, veri koruma dağıtım betiği ile veya bir kalıcı oluşturmak için kullanıcı kodunda yapılandırılmalıdır şifreleme [anahtar deposu](xref:security/data-protection/implementation/key-management).</span><span class="sxs-lookup"><span data-stu-id="a077f-415">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="a077f-416">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="a077f-416">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="a077f-417">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="a077f-417">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="a077f-418">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="a077f-418">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="a077f-419">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a077f-419">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="a077f-420">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-420">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="a077f-421">Bu içerebilir [CSRF belirteçleri](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="a077f-421">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="a077f-422">Veri koruma anahtarı halka kalıcı hale getirmek için IIS altında yapılandırmak için kullanın **bir** aşağıdaki yaklaşımlardan biri:</span><span class="sxs-lookup"><span data-stu-id="a077f-422">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="a077f-423">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="a077f-423">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="a077f-424">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-424">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="a077f-425">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a077f-425">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="a077f-426">Tek başına, webfarm olmayan IIS yüklemeleri [veri koruması sağlama AutoGenKeys.ps1 PowerShell Betiği](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) ile ASP.NET Core uygulaması kullanılan her bir uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-426">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="a077f-427">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a077f-427">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="a077f-428">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-428">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="a077f-429">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-429">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="a077f-430">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="a077f-430">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="a077f-431">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="a077f-431">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="a077f-432">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-432">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="a077f-433">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="a077f-433">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="a077f-434">Bkz: [ASP.NET Core veri koruma yapılandırma](xref:security/data-protection/configuration/overview) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="a077f-434">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="a077f-435">**Kullanıcı profili yüklemek için IIS uygulama havuzu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="a077f-435">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="a077f-436">Bu ayar **işlem modeli** bölümüne **Gelişmiş ayarlar** uygulama havuzu için.</span><span class="sxs-lookup"><span data-stu-id="a077f-436">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="a077f-437">**Kullanıcı profilini** `True`için Yükle ' ye ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-437">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="a077f-438">`True`olarak ayarlandığında anahtarlar Kullanıcı profili dizininde depolanır ve Kullanıcı hesabına özgü bir anahtarla DPAPI kullanılarak korunur.</span><span class="sxs-lookup"><span data-stu-id="a077f-438">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="a077f-439">Anahtarlar *% LocalAppData%/ASP.exe. net/DataProtection-Keys* klasörüne kalıcıdır.</span><span class="sxs-lookup"><span data-stu-id="a077f-439">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="a077f-440">Uygulama havuzunun [Setprofileenvironment özniteliğinin](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) de etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a077f-440">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="a077f-441">`setProfileEnvironment` varsayılan değeri `true`.</span><span class="sxs-lookup"><span data-stu-id="a077f-441">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="a077f-442">Bazı senaryolarda (örneğin, Windows işletim sistemi), `setProfileEnvironment` `false`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-442">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="a077f-443">Anahtarlar beklenen şekilde Kullanıcı profili dizininde depolanmıyorsa:</span><span class="sxs-lookup"><span data-stu-id="a077f-443">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="a077f-444">*% Windir%/system32/inetsrv/config* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="a077f-444">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="a077f-445">*ApplicationHost. config* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-445">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="a077f-446">`<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesini bulun.</span><span class="sxs-lookup"><span data-stu-id="a077f-446">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="a077f-447">`setProfileEnvironment` özniteliğinin mevcut olmadığını, bu değerin varsayılan olarak `true`veya özniteliğin değerini `true`olarak ayarlamış olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-447">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="a077f-448">**Dosya sistemi anahtarı halkası deposu olarak kullanın**</span><span class="sxs-lookup"><span data-stu-id="a077f-448">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="a077f-449">Uygulama kodu ayarlamak [dosya sistemi bir anahtar halkası deposu olarak kullanırsınız](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="a077f-449">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="a077f-450">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="a077f-450">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="a077f-451">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="a077f-451">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="a077f-452">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="a077f-452">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="a077f-453">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-453">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="a077f-454">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="a077f-454">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="a077f-455">Yapılandırma [kod veri koruması](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="a077f-455">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="a077f-456">**Veri koruma için makine genelinde bir ilke ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="a077f-456">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="a077f-457">Veri koruma sisteminde bir varsayılan ayarı desteği sınırlıdır [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy) veri koruma API'lerini kullanan tüm uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="a077f-457">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="a077f-458">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a077f-458">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="a077f-459">Sanal dizinler</span><span class="sxs-lookup"><span data-stu-id="a077f-459">Virtual Directories</span></span>

<span data-ttu-id="a077f-460">[IIS sanal dizinlerinin](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ile ASP.NET Core uygulamaları desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a077f-460">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="a077f-461">Bir uygulama olarak barındırılan bir [alt uygulama](#sub-applications).</span><span class="sxs-lookup"><span data-stu-id="a077f-461">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="a077f-462">Alt uygulamalar</span><span class="sxs-lookup"><span data-stu-id="a077f-462">Sub-applications</span></span>

<span data-ttu-id="a077f-463">ASP.NET Core uygulaması olarak barındırılan bir [IIS alt uygulama (uygulama içi sub)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span><span class="sxs-lookup"><span data-stu-id="a077f-463">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="a077f-464">Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="a077f-464">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a077f-465">Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-465">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="a077f-466">Modül bir alt uygulamasının işleyici olarak eklenip eklenmediğini *web.config* dosyası bir *iç sunucu hatası 500.19* hatalı yapılandırma dosyasına başvuran alındığında alt uygulama göz atmak çalışırken.</span><span class="sxs-lookup"><span data-stu-id="a077f-466">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="a077f-467">Aşağıdaki örnek bir yayımlanan gösterir *web.config* dosyası için bir ASP.NET Core alt uygulama:</span><span class="sxs-lookup"><span data-stu-id="a077f-467">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="a077f-468">ASP.NET Core uygulaması altında ASP.NET Core sub-uygulama barındırma, devralınan işleyici sub-uygulamanın açıkça Kaldır *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="a077f-468">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="a077f-469">Alt uygulama içindeki statik varlık bağlantılar tilde eğik çizgi kullanılmalıdır (`~/`) gösterimi.</span><span class="sxs-lookup"><span data-stu-id="a077f-469">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="a077f-470">Tilde eğik çizgi gösterimi Tetikleyiciler bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) işlenmiş göreli bağlantısını için alt-uygulamanın pathbase önüne eklediğinizden.</span><span class="sxs-lookup"><span data-stu-id="a077f-470">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="a077f-471">Alt uygulama için `/subapp_path`, bir görüntü ile bağlantılı `src="~/image.png"` olarak işlenen `src="/subapp_path/image.png"`.</span><span class="sxs-lookup"><span data-stu-id="a077f-471">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="a077f-472">Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor.</span><span class="sxs-lookup"><span data-stu-id="a077f-472">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="a077f-473">İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-473">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="a077f-474">Statik bir varlık, ın `src` özniteliği için mutlak bir yol ayarlayın (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase işlenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-474">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="a077f-475">Kök uygulamanın statik dosya ara yazılımı, statik varlık kök uygulamadan kullanılabilir olmadığı takdirde *404-bulunamayan* bir Yanıt ile sonuçlanır. Bu, kök uygulamanın [Web kökünden](xref:fundamentals/index#web-root)varlık sunmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a077f-475">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="a077f-476">ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:</span><span class="sxs-lookup"><span data-stu-id="a077f-476">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="a077f-477">Alt uygulama için bir uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a077f-477">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="a077f-478">.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı masaüstü CLR (.NET CLR) değil, çalışan işlemde barındırmak için önyüklenirken **.NET CLR sürümünü** **yönetilen kod olmadan** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-478">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="a077f-479">IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a077f-479">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="a077f-480">IIS Yöneticisi'nde alt uygulama klasörüne sağ tıklayıp **uygulamasına dönüştürün**.</span><span class="sxs-lookup"><span data-stu-id="a077f-480">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="a077f-481">İçinde **uygulama Ekle** iletişim kutusunda, kullanmak **seçin** için düğme **uygulama havuzu** alt uygulama için oluşturduğunuz uygulama havuzuna atanamıyor.</span><span class="sxs-lookup"><span data-stu-id="a077f-481">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="a077f-482">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a077f-482">Select **OK**.</span></span>

<span data-ttu-id="a077f-483">Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.</span><span class="sxs-lookup"><span data-stu-id="a077f-483">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="a077f-484">İşlem içi barındırma modeli ve ASP.NET Core modülünü yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a077f-484">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="a077f-485">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a077f-485">Configuration of IIS with web.config</span></span>

<span data-ttu-id="a077f-486">IIS yapılandırması tarafından etkilenir `<system.webServer>` bölümünü *web.config* işlevsel ASP.NET Core modülü ile ASP.NET Core uygulamaları için IIS senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="a077f-486">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="a077f-487">Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="a077f-487">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="a077f-488">IIS dinamik sıkıştırması kullanmak için sunucu düzeyinde yapılandırılmışsa `<urlCompression>` uygulamanın öğesinde *web.config* dosya devre dışı bırakabilir, ASP.NET Core uygulaması için.</span><span class="sxs-lookup"><span data-stu-id="a077f-488">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="a077f-489">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="a077f-489">For more information, see the following topics:</span></span>

* [<span data-ttu-id="a077f-490">\<System. webServer > için yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="a077f-490">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="a077f-491">(IIS 10.0 veya üzeri desteklenir) yalıtılmış uygulama havuzlarında çalışmakta olan tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgeleri.</span><span class="sxs-lookup"><span data-stu-id="a077f-491">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="a077f-492">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="a077f-492">Configuration sections of web.config</span></span>

<span data-ttu-id="a077f-493">ASP.NET 4.x uygulamalarında yapılandırma bölümlerini *web.config* yapılandırma için ASP.NET Core uygulamaları tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="a077f-493">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="a077f-494">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a077f-494">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="a077f-495">Daha fazla bilgi için [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a077f-495">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="a077f-496">Uygulama Havuzları</span><span class="sxs-lookup"><span data-stu-id="a077f-496">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a077f-497">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="a077f-497">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="a077f-498">İşlemdeki barındırma &ndash; uygulamalarını ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-498">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="a077f-499">Barındırma işlemi çıkış &ndash; her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="a077f-499">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="a077f-500">IIS **Web sitesi Ekle** iletişim varsayılan olarak bir uygulama başına tek bir uygulama havuzu.</span><span class="sxs-lookup"><span data-stu-id="a077f-500">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="a077f-501">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="a077f-501">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="a077f-502">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a077f-502">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a077f-503">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="a077f-503">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="a077f-504">IIS **Web sitesi Ekle** iletişim varsayılan olarak bu yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="a077f-504">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="a077f-505">Olduğunda bir **Site adı** metni otomatik olarak aktarılır sağlanır **uygulama havuzu** metin.</span><span class="sxs-lookup"><span data-stu-id="a077f-505">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="a077f-506">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a077f-506">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="a077f-507">Uygulama Havuzu Kimliği</span><span class="sxs-lookup"><span data-stu-id="a077f-507">Application Pool Identity</span></span>

<span data-ttu-id="a077f-508">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-508">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="a077f-509">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="a077f-509">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="a077f-510">IIS Yönetim konsolunda altında **Gelişmiş ayarlar** emin olmak için uygulama havuzu, **kimlik** kullanmak üzere ayarlanmış **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="a077f-510">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="a077f-512">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a077f-512">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="a077f-513">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-513">Resources can be secured using this identity.</span></span> <span data-ttu-id="a077f-514">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="a077f-514">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="a077f-515">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a077f-515">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="a077f-516">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="a077f-516">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="a077f-517">Sağ tıklatın ve dizin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="a077f-517">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="a077f-518">Altında **güvenlik** sekmesinde **Düzenle** düğmesine ve ardından **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a077f-518">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="a077f-519">Seçin **konumları** düğmesine tıklayın ve sistem seçildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="a077f-519">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="a077f-520">ENTER **IIS uygulama havuzu\\< app_pool_name >** içinde **Seçilecek nesne adlarını girin** alan.</span><span class="sxs-lookup"><span data-stu-id="a077f-520">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="a077f-521">Seçin **Adları Denetle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a077f-521">Select the **Check Names** button.</span></span> <span data-ttu-id="a077f-522">İçin *DefaultAppPool* kullanarak adları denetle **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="a077f-522">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="a077f-523">Zaman **Adları Denetle** düğmesi seçili değerini **DefaultAppPool** nesne adları alanında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-523">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="a077f-524">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="a077f-524">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="a077f-525">Kullanım **IIS uygulama havuzu\\< app_pool_name >** biçimlendirmek için nesne adı denetlenirken.</span><span class="sxs-lookup"><span data-stu-id="a077f-525">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "DefaultAppPool" uygulama havuzu adı eklenir "IIS uygulama havuzu\" "Adları Denetle."seçmeden önce nesne adları alanında](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="a077f-527">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a077f-527">Select **OK**.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="a077f-529">Okuma &amp; Yürütme izinleri varsayılan verilmesi.</span><span class="sxs-lookup"><span data-stu-id="a077f-529">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="a077f-530">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="a077f-530">Provide additional permissions as needed.</span></span>

<span data-ttu-id="a077f-531">Erişim de verilebilir bir komut istemi kullanarak **ICACLS** aracı.</span><span class="sxs-lookup"><span data-stu-id="a077f-531">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="a077f-532">Kullanarak *DefaultAppPool* örnek olarak, aşağıdaki komutu kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a077f-532">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="a077f-533">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls) konu.</span><span class="sxs-lookup"><span data-stu-id="a077f-533">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="a077f-534">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="a077f-534">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a077f-535">[HTTP/2](https://httpwg.org/specs/rfc7540.html) ile ASP.NET Core aşağıdaki IIS dağıtım senaryolarında desteklenir:</span><span class="sxs-lookup"><span data-stu-id="a077f-535">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="a077f-536">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="a077f-536">In-process</span></span>
  * <span data-ttu-id="a077f-537">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a077f-537">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a077f-538">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="a077f-538">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="a077f-539">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="a077f-539">Out-of-process</span></span>
  * <span data-ttu-id="a077f-540">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a077f-540">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a077f-541">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-541">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a077f-542">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="a077f-542">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a077f-543">Bir HTTP/2 bağlantı kurulduğunda, işlem içi dağıtımı için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="a077f-543">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="a077f-544">Bir HTTP/2 bağlantı kurulduğunda, bir işlem dışı dağıtım için [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="a077f-544">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="a077f-545">İşlem içi ve işlem dışı barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a077f-545">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a077f-546">[HTTP/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki temel gereksinimlere işlem dışı dağıtımları için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="a077f-546">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="a077f-547">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a077f-547">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="a077f-548">Genel kullanıma yönelik uç sunucu bağlantıları için ters Ara sunucu bağlantısı ancak HTTP/2 kullanın [Kestrel sunucu](xref:fundamentals/servers/kestrel) HTTP/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="a077f-548">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="a077f-549">Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="a077f-549">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="a077f-550">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="a077f-550">TLS 1.2 or later connection</span></span>

<span data-ttu-id="a077f-551">Bir HTTP/2 bağlantı kurulur, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporları `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="a077f-551">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="a077f-552">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="a077f-552">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="a077f-553">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="a077f-553">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="a077f-554">IIS dağıtımları olan HTTP/2 yapılandırma hakkında daha fazla bilgi için bkz. [HTTP/2 IIS'de](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="a077f-554">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="a077f-555">CORS ön denetim istekleri</span><span class="sxs-lookup"><span data-stu-id="a077f-555">CORS preflight requests</span></span>

<span data-ttu-id="a077f-556">*Bu bölüm, yalnızca .NET Framework hedefleyen ASP.NET Core uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="a077f-556">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="a077f-557">.NET Framework hedefleyen ASP.NET Core bir uygulama için, Seçenekler istekleri uygulamaya varsayılan olarak IIS 'de aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="a077f-557">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="a077f-558">*Web. config* DOSYASıNDAKI uygulama IIS işleyicilerini seçenek isteklerini geçecek şekilde yapılandırma hakkında bilgi edinmek için bkz. [ASP.NET Web API 2 ' de çapraz kaynak ISTEKLERINI etkinleştirme: CORS 'nin nasıl çalıştığı](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span><span class="sxs-lookup"><span data-stu-id="a077f-558">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="a077f-559">Uygulama başlatma modülü ve boşta kalma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="a077f-559">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="a077f-560">ASP.NET Core modülü sürüm 2 tarafından IIS 'de barındırıldığında:</span><span class="sxs-lookup"><span data-stu-id="a077f-560">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="a077f-561">[Uygulama başlatma modülü](#application-initialization-module) &ndash; uygulamanın barındırılan veya [işlem dışı](#out-of-process-hosting-model) bir [çalışan işlem yeniden](#in-process-hosting-model) başlatma veya sunucu yeniden başlatma üzerinde otomatik olarak başlayacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-561">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="a077f-562">[Boşta kalma zaman aşımı](#idle-timeout) &ndash; uygulamanın barındırılan, [işlem](#in-process-hosting-model) yapılmayan dönemler sırasında zaman aşımına uğramaması için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a077f-562">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="a077f-563">Uygulama başlatma modülü</span><span class="sxs-lookup"><span data-stu-id="a077f-563">Application Initialization Module</span></span>

<span data-ttu-id="a077f-564">*İşlem içi ve işlem dışı barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="a077f-564">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="a077f-565">[IIS uygulaması başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) , uygulama havuzu başlatıldığında veya geri DÖNÜŞTÜRÜLDÜĞÜNDE uygulamaya http isteği gönderen bir IIS özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="a077f-565">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="a077f-566">İstek, uygulamayı başlatmak üzere tetikler.</span><span class="sxs-lookup"><span data-stu-id="a077f-566">The request triggers the app to start.</span></span> <span data-ttu-id="a077f-567">Varsayılan olarak IIS, uygulamayı başlatmak için uygulamanın kök URL 'SI (`/`) için bir istek yayınlar (yapılandırma hakkında daha fazla bilgi için [ek kaynaklara](#application-initialization-module-and-idle-timeout-additional-resources) bakın).</span><span class="sxs-lookup"><span data-stu-id="a077f-567">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="a077f-568">IIS uygulama başlatma rolü özelliğinin etkin olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="a077f-568">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="a077f-569">IIS 'yi yerel olarak kullanırken Windows 7 veya üzeri masaüstü sistemlerinde:</span><span class="sxs-lookup"><span data-stu-id="a077f-569">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="a077f-570">> programlar ve Özellikler > **Programlar** **ve Özellikler** ' **e gidin >** **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).</span><span class="sxs-lookup"><span data-stu-id="a077f-570">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="a077f-571">**Uygulama geliştirme özellikleri**> **Internet Information Services** > **World Wide Web Hizmetleri** 'ni açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-571">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="a077f-572">**Uygulama başlatma**onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-572">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="a077f-573">Windows Server 2008 R2 veya sonraki sürümlerde:</span><span class="sxs-lookup"><span data-stu-id="a077f-573">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="a077f-574">**Rol ve Özellik Ekleme Sihirbazı 'nı**açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-574">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="a077f-575">**Rol hizmetlerini Seç** panelinde, **uygulama geliştirme** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-575">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="a077f-576">**Uygulama başlatma**onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-576">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="a077f-577">Site için uygulama başlatma modülünü etkinleştirmek üzere aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a077f-577">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="a077f-578">IIS Yöneticisi 'Ni kullanma:</span><span class="sxs-lookup"><span data-stu-id="a077f-578">Using IIS Manager:</span></span>

  1. <span data-ttu-id="a077f-579">**Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-579">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="a077f-580">Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-580">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="a077f-581">Varsayılan **Başlangıç modu** **OnDemand**' dir.</span><span class="sxs-lookup"><span data-stu-id="a077f-581">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="a077f-582">**Başlangıç modunu** **AlwaysRunning**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-582">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="a077f-583">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a077f-583">Select **OK**.</span></span>
  1. <span data-ttu-id="a077f-584">**Bağlantılar** panelinde **siteler** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="a077f-584">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="a077f-585">Uygulamaya sağ tıklayın ve **Gelişmiş ayarlar**> **Web sitesini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-585">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="a077f-586">Varsayılan **önyükleme etkin** ayarı **false**şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="a077f-586">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="a077f-587">**Önyükleme etkin** ' i **true**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-587">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="a077f-588">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a077f-588">Select **OK**.</span></span>

* <span data-ttu-id="a077f-589">*Web. config*kullanarak, uygulamanın *Web. config* dosyasındaki `<system.webServer>` öğelerine `true` `doAppInitAfterRestart` ayarlanmış `<applicationInitialization>` öğesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a077f-589">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <applicationInitialization doAppInitAfterRestart="true" />
      </system.webServer>
    </location>
  </configuration>
  ```

### <a name="idle-timeout"></a><span data-ttu-id="a077f-590">Boşta Kalma Zaman Aşımı</span><span class="sxs-lookup"><span data-stu-id="a077f-590">Idle Timeout</span></span>

<span data-ttu-id="a077f-591">*Yalnızca işlem sırasında barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="a077f-591">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="a077f-592">Uygulamanın çalışmasını engellemek için, IIS Yöneticisi 'Ni kullanarak uygulama havuzunun boşta kalma zaman aşımını ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a077f-592">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="a077f-593">**Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-593">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="a077f-594">Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a077f-594">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="a077f-595">Varsayılan **boşta kalma süresi (dakika)** **20** dakikadır.</span><span class="sxs-lookup"><span data-stu-id="a077f-595">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="a077f-596">**Boşta kalma zaman aşımını (dakika)** **0** (sıfır) olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a077f-596">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="a077f-597">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="a077f-597">Select **OK**.</span></span>
1. <span data-ttu-id="a077f-598">Çalışan işlemini geri dönüştür.</span><span class="sxs-lookup"><span data-stu-id="a077f-598">Recycle the worker process.</span></span>

<span data-ttu-id="a077f-599">[İşlem dışı](#out-of-process-hosting-model) barındırılan uygulamaların zaman aşımına uğramasını engellemek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a077f-599">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="a077f-600">Uygulamayı çalışır durumda tutmak için bir dış hizmetten ping yapın.</span><span class="sxs-lookup"><span data-stu-id="a077f-600">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="a077f-601">Uygulama yalnızca arka plan hizmetleri barındırıyorsa, IIS barındırmaktan kaçının ve [ASP.NET Core uygulamasını barındırmak için bir Windows hizmeti](xref:host-and-deploy/windows-service)kullanın.</span><span class="sxs-lookup"><span data-stu-id="a077f-601">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="a077f-602">Uygulama başlatma modülü ve boşta kalma zaman aşımı ek kaynakları</span><span class="sxs-lookup"><span data-stu-id="a077f-602">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="a077f-603">IIS 8,0 uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="a077f-603">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="a077f-604">[Uygulama başlatma \<applicationınitialization >](/iis/configuration/system.webserver/applicationinitialization/).</span><span class="sxs-lookup"><span data-stu-id="a077f-604">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="a077f-605">[Bir uygulama havuzu Için Işlem modeli ayarları \<processModel >](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span><span class="sxs-lookup"><span data-stu-id="a077f-605">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="a077f-606">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="a077f-606">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="a077f-607">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="a077f-607">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="a077f-608">IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="a077f-608">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="a077f-609">.NET core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="a077f-609">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="a077f-610">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a077f-610">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="a077f-611">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="a077f-611">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="a077f-612">Windows Server Teknik İçerik Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="a077f-612">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="a077f-613">IIS HTTP/2</span><span class="sxs-lookup"><span data-stu-id="a077f-613">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
