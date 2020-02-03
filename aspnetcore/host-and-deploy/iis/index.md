---
title: Windows IIS üzerinde ASP.NET Core barındırma
author: guardrex
description: ASP.NET Core uygulamaları Windows Server Internet Information Services (IIS) üzerinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/index
ms.openlocfilehash: 146a204509856186a2696b770cae2249d348fa34
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726830"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="17cd3-103">Windows IIS üzerinde ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="17cd3-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="17cd3-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="17cd3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="17cd3-105">ASP.NET Core uygulamasını bir IIS sunucusuna yayımlamaya yönelik bir öğretici deneyimi için, bkz. <xref:tutorials/publish-to-iis>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-105">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

[<span data-ttu-id="17cd3-106">.NET Core barındırma paketi 'ni yükler</span><span class="sxs-lookup"><span data-stu-id="17cd3-106">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="17cd3-107">Desteklenen işletim sistemleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-107">Supported operating systems</span></span>

<span data-ttu-id="17cd3-108">Aşağıdaki işletim sistemleri desteklenmektedir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-108">The following operating systems are supported:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="17cd3-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-109">Windows 7 or later</span></span>
* <span data-ttu-id="17cd3-110">Windows Server 2012 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-110">Windows Server 2012 R2 or later</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="17cd3-111">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-111">Windows 7 or later</span></span>
* <span data-ttu-id="17cd3-112">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-112">Windows Server 2008 R2 or later</span></span>

::: moniker-end

<span data-ttu-id="17cd3-113">[Http. sys sunucusu](xref:fundamentals/servers/httpsys) (eskiden webListener olarak adlandırılır), IIS ile ters proxy yapılandırmasında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-113">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="17cd3-114">[Kestrel sunucusunu](xref:fundamentals/servers/kestrel)kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-114">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="17cd3-115">Azure 'da barındırma hakkında bilgi için bkz. <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-115">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

<span data-ttu-id="17cd3-116">Sorun giderme kılavuzu için bkz. <xref:test/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-116">For troubleshooting guidance, see <xref:test/troubleshoot>.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="17cd3-117">Desteklenen platformlar</span><span class="sxs-lookup"><span data-stu-id="17cd3-117">Supported platforms</span></span>

<span data-ttu-id="17cd3-118">32-bit (x86) veya 64 bit (x64) dağıtımı için yayımlanan uygulamalar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-118">Apps published for 32-bit (x86) or 64-bit (x64) deployment are supported.</span></span> <span data-ttu-id="17cd3-119">Uygulama dışında 32 bitlik bir uygulamayı 32 bit (x86) .NET Core SDK dağıtma:</span><span class="sxs-lookup"><span data-stu-id="17cd3-119">Deploy a 32-bit app with a 32-bit (x86) .NET Core SDK unless the app:</span></span>

* <span data-ttu-id="17cd3-120">64 bitlik bir uygulama için kullanılabilir daha büyük sanal bellek adres alanını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-120">Requires the larger virtual memory address space available to a 64-bit app.</span></span>
* <span data-ttu-id="17cd3-121">Daha büyük IIS yığın boyutunu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-121">Requires the larger IIS stack size.</span></span>
* <span data-ttu-id="17cd3-122">64 bitlik yerel bağımlılıklara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-122">Has 64-bit native dependencies.</span></span>

<span data-ttu-id="17cd3-123">64 bit uygulama yayımlamak için 64 bit (x64) .NET Core SDK kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-123">Use a 64-bit (x64) .NET Core SDK to publish a 64-bit app.</span></span> <span data-ttu-id="17cd3-124">Ana bilgisayar sisteminde 64 bit çalışma zamanı bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-124">A 64-bit runtime must be present on the host system.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-models"></a><span data-ttu-id="17cd3-125">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-125">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="17cd3-126">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="17cd3-126">In-process hosting model</span></span>

<span data-ttu-id="17cd3-127">ASP.NET Core bir uygulama, işlem içi barındırma kullanarak IIS çalışan işlemiyle aynı işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-127">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="17cd3-128">İşlem içi barındırma, istek dışı barındırmak için gelişmiş performans sağlar çünkü istekler, giden ağ trafiği ile aynı makineye geri döndürülen bir ağ arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-128">In-process hosting provides improved performance over out-of-process hosting because requests aren't proxied over the loopback adapter, a network interface that returns outgoing network traffic back to the same machine.</span></span> <span data-ttu-id="17cd3-129">IIS, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)ile işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-129">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="17cd3-130">[ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="17cd3-130">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module):</span></span>

* <span data-ttu-id="17cd3-131">Uygulama başlatmayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-131">Performs app initialization.</span></span>
  * <span data-ttu-id="17cd3-132">[CoreCLR](/dotnet/standard/glossary#coreclr)'yi yükler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-132">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="17cd3-133">`Program.Main`çağırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-133">Calls `Program.Main`.</span></span>
* <span data-ttu-id="17cd3-134">IIS yerel isteğinin ömrünü işler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-134">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="17cd3-135">İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-135">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="17cd3-136">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve süreçte barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-136">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![İşlem içi barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-inprocess.png)

<span data-ttu-id="17cd3-138">Web 'den çekirdek modu HTTP. sys sürücüsüne bir istek ulaşır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-138">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="17cd3-139">Sürücü, yerel isteği Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="17cd3-139">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="17cd3-140">ASP.NET Core modülü yerel isteği alır ve IIS HTTP sunucusuna (`IISHttpServer`) geçirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-140">The ASP.NET Core Module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="17cd3-141">IIS HTTP sunucusu, isteği yerelden yönetilene dönüştüren bir IIS için işlem içi sunucu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-141">IIS HTTP Server is an in-process server implementation for IIS that converts the request from native to managed.</span></span>

<span data-ttu-id="17cd3-142">IIS HTTP sunucusu isteği işlediğinde, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-142">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="17cd3-143">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-143">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="17cd3-144">Uygulamanın yanıtı IIS HTTP sunucusu aracılığıyla IIS 'e geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-144">The app's response is passed back to IIS through IIS HTTP Server.</span></span> <span data-ttu-id="17cd3-145">IIS yanıtı, isteği başlatan istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-145">IIS sends the response to the client that initiated the request.</span></span>

<span data-ttu-id="17cd3-146">İşlem içi barındırma, mevcut uygulamalar için kabul ediyor, ancak tüm IIS ve IIS Express senaryoları için varsayılan [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar, işlem içi barındırma modeli için varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-146">In-process hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="17cd3-147">`CreateDefaultBuilder`, [CoreCLR](/dotnet/standard/glossary#coreclr) 'yi önyüklemek ve uygulamayı IIS çalışan işleminin (*W3wp. exe* veya *iisexpress. exe*) içinde barındırmak için <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> metodunu çağırarak bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> örneği ekler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-147">`CreateDefaultBuilder` adds an <xref:Microsoft.AspNetCore.Hosting.Server.IServer> instance by calling the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="17cd3-148">Performans testleri, bir .NET Core uygulamasını işlem içinde barındıran uygulamanın, uygulamayı işlem dışı ve [Kestrel](xref:fundamentals/servers/kestrel) sunucusuna proxy alma isteklerinin barındırılmasına kıyasla önemli ölçüde daha yüksek istek aktarım hızı sağladığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-148">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!NOTE]
> <span data-ttu-id="17cd3-149">Tek bir dosya yürütülebilir dosyası olarak yayınlanan uygulamalar, işlem içi barındırma modeliyle yüklenemez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-149">Apps published as a single file executable can't be loaded by the in-process hosting model.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="17cd3-150">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="17cd3-150">Out-of-process hosting model</span></span>

<span data-ttu-id="17cd3-151">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, ASP.NET Core modülü işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-151">Because ASP.NET Core apps run in a process separate from the IIS worker process, the ASP.NET Core Module handles process management.</span></span> <span data-ttu-id="17cd3-152">Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-152">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="17cd3-153">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-153">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="17cd3-154">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-154">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![İşlem dışı barındırma senaryosunda modül ASP.NET Core](index/_static/ancm-outofprocess.png)

<span data-ttu-id="17cd3-156">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-156">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="17cd3-157">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="17cd3-157">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="17cd3-158">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-158">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="17cd3-159">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> uzantısı sunucuyu `http://localhost:{PORT}`dinlemek üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-159">The module specifies the port via an environment variable at startup, and the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> extension configures the server to listen on `http://localhost:{PORT}`.</span></span> <span data-ttu-id="17cd3-160">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-160">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="17cd3-161">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-161">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="17cd3-162">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-162">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="17cd3-163">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-163">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="17cd3-164">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-164">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="17cd3-165">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-165">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17cd3-166">ASP.NET Core, varsayılan, platformlar arası bir HTTP sunucusu olan [Kestrel Server](xref:fundamentals/servers/kestrel)ile birlikte gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-166">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), a default, cross-platform HTTP server.</span></span>

<span data-ttu-id="17cd3-167">[IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)kullanırken, uygulama IIS çalışan işleminden (*işlem dışı*) [Kestrel sunucusu](xref:fundamentals/servers/index#kestrel)ile ayrı bir işlemde çalışır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-167">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the app runs in a process separate from the IIS worker process (*out-of-process*) with the [Kestrel server](xref:fundamentals/servers/index#kestrel).</span></span>

<span data-ttu-id="17cd3-168">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini işler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-168">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="17cd3-169">Modül, ilk istek ulaştığında ASP.NET Core uygulama için işlemi başlatır ve kapanırsa veya kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-169">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="17cd3-170">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen işlem içi uygulamalarla birlikte görülen davranışdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-170">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="17cd3-171">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve işlem dışı barındırılan bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-171">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core Modülü](index/_static/ancm-outofprocess.png)

<span data-ttu-id="17cd3-173">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-173">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="17cd3-174">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="17cd3-174">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="17cd3-175">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-175">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="17cd3-176">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu `http://localhost:{port}`dinlemek üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-176">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="17cd3-177">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-177">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="17cd3-178">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-178">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="17cd3-179">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-179">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="17cd3-180">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-180">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="17cd3-181">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-181">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="17cd3-182">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-182">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="17cd3-183">`CreateDefaultBuilder`, Web sunucusu olarak [Kestrel](xref:fundamentals/servers/kestrel) sunucusunu yapılandırır ve [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module)için temel yolu ve bağlantı noktasını yapılandırarak IIS tümleştirmesini mümkün yapar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-183">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and enables IIS Integration by configuring the base path and port for the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="17cd3-184">ASP.NET Core modülü arka uç işleme atamak için dinamik bir bağlantı noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-184">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="17cd3-185">`CreateDefaultBuilder` <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-185">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="17cd3-186">`UseIISIntegration`, Kestrel 'yi localhost IP adresinde (`127.0.0.1`) dinamik bağlantı noktasında dinleyecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-186">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="17cd3-187">Dinamik bağlantı noktası 1234 ise, Kestrel `127.0.0.1:1234`dinler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-187">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="17cd3-188">Bu yapılandırma tarafından sağlanan diğer URL'yi yapılandırmaları değiştirir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-188">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="17cd3-189">Kestrel 'in dinleme API 'SI</span><span class="sxs-lookup"><span data-stu-id="17cd3-189">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="17cd3-190">[Yapılandırma](xref:fundamentals/configuration/index) (veya [komut satırı--URL seçeneği](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="17cd3-190">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="17cd3-191">Modül kullanılırken `UseUrls` veya Kestrel 'un `Listen` API 'sine yapılan çağrılar gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-191">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="17cd3-192">`UseUrls` veya `Listen` çağrılırsa, Kestrel yalnızca uygulamayı IIS olmadan çalıştırırken belirtilen bağlantı noktasını dinler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-192">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

<span data-ttu-id="17cd3-193">ASP.NET Core modülü yapılandırma kılavuzu için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-193">For ASP.NET Core Module configuration guidance, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="17cd3-194">Barındırma hakkında daha fazla bilgi için bkz. [ASP.NET Core ana bilgisayar](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="17cd3-194">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/index#host).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="17cd3-195">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="17cd3-195">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="17cd3-196">IISIntegration bileşenlerini etkinleştir</span><span class="sxs-lookup"><span data-stu-id="17cd3-196">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="17cd3-197">`CreateHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-197">When building a host in `CreateHostBuilder` (*Program.cs*), call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="17cd3-198">`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#default-builder-settings>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-198">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/generic-host#default-builder-settings>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="17cd3-199">`CreateWebHostBuilder` (*program.cs*) içinde bir konak oluştururken, IIS tümleştirmesini etkinleştirmek için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-199">When building a host in `CreateWebHostBuilder` (*Program.cs*), call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to enable IIS integration:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

<span data-ttu-id="17cd3-200">`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-200">For more information on `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

::: moniker-end

### <a name="iis-options"></a><span data-ttu-id="17cd3-201">IIS seçenekleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-201">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17cd3-202">**İşlem içi barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="17cd3-202">**In-process hosting model**</span></span>

<span data-ttu-id="17cd3-203">IIS sunucu seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISServerOptions> için bir hizmet yapılandırması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-203">To configure IIS Server options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISServerOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="17cd3-204">Aşağıdaki örnek AutomaticAuthentication devre dışı bırakır:</span><span class="sxs-lookup"><span data-stu-id="17cd3-204">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="17cd3-205">Seçenek</span><span class="sxs-lookup"><span data-stu-id="17cd3-205">Option</span></span>                         | <span data-ttu-id="17cd3-206">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="17cd3-206">Default</span></span> | <span data-ttu-id="17cd3-207">Ayar</span><span class="sxs-lookup"><span data-stu-id="17cd3-207">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="17cd3-208">`true`, IIS sunucusu, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-208">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="17cd3-209">`false`, sunucu yalnızca `HttpContext.User` için bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-209">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="17cd3-210">`AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-210">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="17cd3-211">Daha fazla bilgi için bkz. [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="17cd3-211">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="17cd3-212">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-212">Sets the display name shown to users on login pages.</span></span> |
| `AllowSynchronousIO`           | `false` | <span data-ttu-id="17cd3-213">`HttpContext.Request` ve `HttpContext.Response`için zaman uyumlu GÇ izin verilip verilmeyeceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-213">Whether synchronous IO is allowed for the `HttpContext.Request` and the `HttpContext.Response`.</span></span> |
| `MaxRequestBodySize`           | `30000000`  | <span data-ttu-id="17cd3-214">`HttpRequest`için en büyük istek gövdesi boyutunu alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-214">Gets or sets the max request body size for the `HttpRequest`.</span></span> <span data-ttu-id="17cd3-215">IIS 'nin `IISServerOptions``MaxRequestBodySize` ayarlamadan önce işlenecek sınır `maxAllowedContentLength` sahip olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-215">Note that IIS itself has the limit `maxAllowedContentLength` which will be processed before the `MaxRequestBodySize` set in the `IISServerOptions`.</span></span> <span data-ttu-id="17cd3-216">`MaxRequestBodySize` değiştirmek `maxAllowedContentLength`etkilemez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-216">Changing the `MaxRequestBodySize` won't affect the `maxAllowedContentLength`.</span></span> <span data-ttu-id="17cd3-217">`maxAllowedContentLength`artırmak için, *Web. config* dosyasına daha yüksek bir değere `maxAllowedContentLength` ayarlamak üzere bir giriş ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-217">To increase `maxAllowedContentLength`, add an entry in the *web.config* to set `maxAllowedContentLength` to a higher value.</span></span> <span data-ttu-id="17cd3-218">Daha fazla ayrıntı için bkz. [yapılandırma](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span><span class="sxs-lookup"><span data-stu-id="17cd3-218">For more details, see [Configuration](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/#configuration).</span></span> |

<span data-ttu-id="17cd3-219">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="17cd3-219">**Out-of-process hosting model**</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="17cd3-220">Seçenek</span><span class="sxs-lookup"><span data-stu-id="17cd3-220">Option</span></span>                         | <span data-ttu-id="17cd3-221">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="17cd3-221">Default</span></span> | <span data-ttu-id="17cd3-222">Ayar</span><span class="sxs-lookup"><span data-stu-id="17cd3-222">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="17cd3-223">`true`, IIS sunucusu, [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-223">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="17cd3-224">`false`, sunucu yalnızca `HttpContext.User` için bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-224">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="17cd3-225">`AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-225">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="17cd3-226">Daha fazla bilgi için bkz. [Windows kimlik doğrulaması](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="17cd3-226">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="17cd3-227">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-227">Sets the display name shown to users on login pages.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17cd3-228">**İşlem dışı barındırma modeli**</span><span class="sxs-lookup"><span data-stu-id="17cd3-228">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="17cd3-229">IIS seçeneklerini yapılandırmak için <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*><xref:Microsoft.AspNetCore.Builder.IISOptions> için bir hizmet yapılandırması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-229">To configure IIS options, include a service configuration for <xref:Microsoft.AspNetCore.Builder.IISOptions> in <xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*>.</span></span> <span data-ttu-id="17cd3-230">Aşağıdaki örnek, uygulamanın `HttpContext.Connection.ClientCertificate`doldurmasını engeller:</span><span class="sxs-lookup"><span data-stu-id="17cd3-230">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="17cd3-231">Seçenek</span><span class="sxs-lookup"><span data-stu-id="17cd3-231">Option</span></span>                         | <span data-ttu-id="17cd3-232">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="17cd3-232">Default</span></span> | <span data-ttu-id="17cd3-233">Ayar</span><span class="sxs-lookup"><span data-stu-id="17cd3-233">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="17cd3-234">`true`, [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components) [Windows kimlik doğrulaması](xref:security/authentication/windowsauth)tarafından kimliği doğrulanmış `HttpContext.User` ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-234">If `true`, [IIS Integration Middleware](#enable-the-iisintegration-components) sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="17cd3-235">`false`, ara yazılım yalnızca `HttpContext.User` bir kimlik sağlar ve `AuthenticationScheme`tarafından açıkça istendiğinde güçlüklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-235">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="17cd3-236">`AutomaticAuthentication` çalışması için IIS 'de Windows kimlik doğrulamasının etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-236">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="17cd3-237">Daha fazla bilgi için [Windows kimlik doğrulaması](xref:security/authentication/windowsauth) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-237">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="17cd3-238">Oturum açma sayfaları kullanıcılara gösterilen görünen adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-238">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="17cd3-239">`true` ve `MS-ASPNETCORE-CLIENTCERT` istek üst bilgisi varsa `HttpContext.Connection.ClientCertificate` doldurulur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-239">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="17cd3-240">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="17cd3-240">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="17cd3-241">Iletilen üstbilgiler ara yazılımını yapılandıran [IIS tümleştirme ara yazılımı](#enable-the-iisintegration-components)ve ASP.NET Core modülü, DÜZENI (http/https) ve isteğin KAYNAKLANDıĞı uzak IP adresini iletecek şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-241">The [IIS Integration Middleware](#enable-the-iisintegration-components), which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="17cd3-242">Ek Ara sunucuları ve yük dengeleyici barındırılan uygulamalar için ek yapılandırma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-242">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="17cd3-243">Daha fazla bilgi için bkz. [proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="17cd3-243">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="17cd3-244">Web.config dosyası</span><span class="sxs-lookup"><span data-stu-id="17cd3-244">web.config file</span></span>

<span data-ttu-id="17cd3-245">*Web. config* dosyası [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-245">The *web.config* file configures the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="17cd3-246">*Web. config* dosyası oluşturma, dönüştürme ve yayımlama, proje yayımlandığında bir MSBuild hedefi (`_TransformWebConfig`) tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-246">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="17cd3-247">Bu hedef, Web SDK hedeflerinde (`Microsoft.NET.Sdk.Web`) bulunur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-247">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="17cd3-248">SDK'sı, proje dosyasının en üstüne ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="17cd3-248">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="17cd3-249">Bir *Web. config* dosyası projede yoksa, dosya ASP.NET Core modülünü yapılandırmak Için doğru *processPath* ve *bağımsız değişkenlerle* oluşturulur ve [yayımlanan çıktıya](xref:host-and-deploy/directory-structure)taşınır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-249">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="17cd3-250">Projede bir *Web. config* dosyası varsa, dosya ASP.NET Core modülünü yapılandırmak ve yayımlanan çıktıya taşınmak Için doğru *processPath* ve *bağımsız değişkenlerle* birlikte dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="17cd3-250">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="17cd3-251">Dönüştürme, IIS yapılandırma ayarları dosyasında değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-251">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="17cd3-252">*Web. config* dosyası, etkin IIS modüllerini DENETLEYEN ek IIS yapılandırma ayarları verebilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-252">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="17cd3-253">ASP.NET Core uygulamalarla istekleri işleyebilen IIS modülleri hakkında daha fazla bilgi için bkz. [IIS modules](xref:host-and-deploy/iis/modules) konusu.</span><span class="sxs-lookup"><span data-stu-id="17cd3-253">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="17cd3-254">Web SDK 'sının *Web. config* dosyasını dönüştürmasını engellemek için, proje dosyasında **\<ıstransformwebconfigdisabled >** özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-254">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="17cd3-255">Web SDK 'sının dosyayı dönüştürmesiyle devre dışı bırakıldığında, *processPath* ve *bağımsız değişkenler* geliştirici tarafından el ile ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-255">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="17cd3-256">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-256">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="17cd3-257">Web.config dosyası konumu</span><span class="sxs-lookup"><span data-stu-id="17cd3-257">web.config file location</span></span>

<span data-ttu-id="17cd3-258">[ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module) doğru bir şekilde ayarlamak için, *Web. config* dosyası dağıtılan uygulamanın [içerik kök](xref:fundamentals/index#content-root) yolunda (genellikle uygulama temel yolu) bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-258">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the [content root](xref:fundamentals/index#content-root) path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="17cd3-259">IIS için sağlanan Web sitesi fiziksel yol ile aynı konumda budur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-259">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="17cd3-260">Web Dağıtımı kullanarak birden çok uygulama yayımlamayı etkinleştirmek için uygulamanın kökünde *Web. config* dosyası gereklidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-260">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="17cd3-261">Uygulamanın fiziksel yolunda ( *\<derlemesi >. runtimeconfig. JSON*, *\<bütünleştirilmiş kod >. xml* (XML belge açıklamaları) ve *\<derlemesi >. Deps. JSON*gibi hassas dosyalar bulunur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-261">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="17cd3-262">*Web. config* dosyası mevcut olduğunda ve site normal olarak başlatıldığında, istenirse IIS bu hassas dosyaları sunmaz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-262">When the *web.config* file is present and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="17cd3-263">*Web. config* dosyası eksikse, yanlış şekilde adlandırılmış veya site normal başlatma için YAPıLANDıRıMıŞSA IIS hassas dosyalara genel olarak sunabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-263">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="17cd3-264">***Web. config* dosyasının her zaman dağıtımda mevcut olması, doğru şekilde adlandırılması ve siteyi normal başlangıç için yapılandırabiliyor olması gerekir. *Web. config* dosyasını bir üretim dağıtımından hiçbir şekilde kaldırmayın.**</span><span class="sxs-lookup"><span data-stu-id="17cd3-264">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

### <a name="transform-webconfig"></a><span data-ttu-id="17cd3-265">Web.config’i dönüştürme</span><span class="sxs-lookup"><span data-stu-id="17cd3-265">Transform web.config</span></span>

<span data-ttu-id="17cd3-266">Yayımlama sırasında *Web. config* ' i dönüştürmeniz gerekiyorsa (örneğin, yapılandırma, profil veya ortama göre ortam değişkenlerini ayarlayın), bkz. <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-266">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="17cd3-267">IIS yapılandırması</span><span class="sxs-lookup"><span data-stu-id="17cd3-267">IIS configuration</span></span>

<span data-ttu-id="17cd3-268">**Windows Server işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="17cd3-268">**Windows Server operating systems**</span></span>

<span data-ttu-id="17cd3-269">**Web sunucusu (IIS)** sunucu rolünü etkinleştirin ve rol hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-269">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="17cd3-270">**Yönet** menüsündeki **rol ve özellik ekleme** sihirbazı ' nı veya **Sunucu Yöneticisi**bağlantısındaki bağlantıyı kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-270">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="17cd3-271">**Sunucu rolleri** adımında, **Web sunucusu (IIS)** kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-271">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Web sunucusu IIS rolünü sunucu rolleri adımda seçilir.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="17cd3-273">**Özellikler** adımından sonra, Web sunucusu (IIS) için **rol hizmetleri** adımı yüklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-273">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="17cd3-274">İstenen IIS rol hizmetlerini seçin veya sağlanan varsayılan rol hizmetlerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-274">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![Varsayılan rol hizmetlerini seçin rol hizmetleri adımda seçilir.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="17cd3-276">**Windows kimlik doğrulaması (Isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="17cd3-276">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="17cd3-277">Windows kimlik doğrulamasını etkinleştirmek için şu düğümleri genişletin: **Web sunucusu** > **güvenliği**.</span><span class="sxs-lookup"><span data-stu-id="17cd3-277">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="17cd3-278">**Windows kimlik doğrulama** özelliğini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-278">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="17cd3-279">Daha fazla bilgi için bkz. [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="17cd3-279">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="17cd3-280">**WebSockets (Isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="17cd3-280">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="17cd3-281">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-281">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="17cd3-282">WebSockets etkinleştirmek için şu düğümleri genişletin: **Web sunucusu** > **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="17cd3-282">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="17cd3-283">**WebSocket protokolü** özelliğini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-283">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="17cd3-284">Daha fazla bilgi için bkz. [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="17cd3-284">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="17cd3-285">Web sunucusu rolü ve hizmetlerini yüklemek için **onay** adımına ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-285">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="17cd3-286">**Web sunucusu (IIS)** rolü yüklendikten sonra sunucu/IIS yeniden başlatması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-286">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="17cd3-287">**Windows masaüstü işletim sistemleri**</span><span class="sxs-lookup"><span data-stu-id="17cd3-287">**Windows desktop operating systems**</span></span>

<span data-ttu-id="17cd3-288">**IIS Yönetim Konsolu** ve **World Wide Web hizmetlerini**etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-288">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="17cd3-289"> > programlar ve Özellikler > **Programlar** **ve Özellikler** '\ \**e gidin >\ \** **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).</span><span class="sxs-lookup"><span data-stu-id="17cd3-289">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="17cd3-290">**Internet Information Services** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-290">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="17cd3-291">**Web yönetimi araçları** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-291">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="17cd3-292">**IIS Yönetim Konsolu**kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-292">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="17cd3-293">**World Wide Web Hizmetleri**kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-293">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="17cd3-294">**World Wide Web Hizmetleri** için varsayılan özellikleri kabul edın veya IIS özelliklerini özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-294">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="17cd3-295">**Windows kimlik doğrulaması (Isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="17cd3-295">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="17cd3-296">Windows kimlik doğrulamasını etkinleştirmek için şu düğümleri genişletin: **World Wide Web hizmetleri** > **güvenlik**.</span><span class="sxs-lookup"><span data-stu-id="17cd3-296">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="17cd3-297">**Windows kimlik doğrulama** özelliğini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-297">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="17cd3-298">Daha fazla bilgi için bkz. [Windows kimlik doğrulaması \<windowsAuthentication >](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) ve [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="17cd3-298">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="17cd3-299">**WebSockets (Isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="17cd3-299">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="17cd3-300">WebSockets, ASP.NET Core 1.1 veya sonraki sürümlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-300">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="17cd3-301">WebSockets etkinleştirmek için şu düğümleri genişletin: **World Wide Web hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="17cd3-301">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="17cd3-302">**WebSocket protokolü** özelliğini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-302">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="17cd3-303">Daha fazla bilgi için bkz. [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="17cd3-303">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="17cd3-304">IIS yüklemeyi yeniden başlatma gerektirirse, sistemi yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-304">If the IIS installation requires a restart, restart the system.</span></span>

![Windows özellikleri, IIS Yönetim Konsolu ve World Wide Web Hizmetleri seçilir.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="17cd3-306">Paket barındırma .NET Core'u yükleme</span><span class="sxs-lookup"><span data-stu-id="17cd3-306">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="17cd3-307">*.NET Core barındırma paketi* 'ni barındırma sistemine yükler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-307">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="17cd3-308">Paket, .NET Core çalışma zamanı, .NET Core kitaplığı ve [ASP.NET Core modülünü](xref:host-and-deploy/aspnet-core-module)de yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="17cd3-308">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="17cd3-309">Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-309">The module allows ASP.NET Core apps to run behind IIS.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17cd3-310">Barındırma paket önce IIS yüklü değilse, paket yükleme onarılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-310">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="17cd3-311">IIS yeniden yükledikten sonra paket barındırma yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-311">Run the Hosting Bundle installer again after installing IIS.</span></span>
>
> <span data-ttu-id="17cd3-312">.NET Core 'un 64 bit (x64) sürümünü yükledikten sonra barındırma paketi yüklenirse, SDK 'lar eksik gibi görünebilir ([hiçbir .NET Core SDK 'sı algılanmadı](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="17cd3-312">If the Hosting Bundle is installed after installing the 64-bit (x64) version of .NET Core, SDKs might appear to be missing ([No .NET Core SDKs were detected](xref:test/troubleshoot#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="17cd3-313">Sorunu çözmek için bkz. <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-313">To resolve the problem, see <xref:test/troubleshoot#missing-sdk-after-installing-the-net-core-hosting-bundle>.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="17cd3-314">Doğrudan indirme (geçerli sürüm)</span><span class="sxs-lookup"><span data-stu-id="17cd3-314">Direct download (current version)</span></span>

<span data-ttu-id="17cd3-315">Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:</span><span class="sxs-lookup"><span data-stu-id="17cd3-315">Download the installer using the following link:</span></span>

[<span data-ttu-id="17cd3-316">Geçerli .NET Core barındırma paketi yükleyicisi (doğrudan indirme)</span><span class="sxs-lookup"><span data-stu-id="17cd3-316">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="17cd3-317">Yükleyici önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-317">Earlier versions of the installer</span></span>

<span data-ttu-id="17cd3-318">Yükleyici önceki bir sürümünü almak için:</span><span class="sxs-lookup"><span data-stu-id="17cd3-318">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="17cd3-319">[.Net indirme arşivleri](https://www.microsoft.com/net/download/archives)' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-319">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="17cd3-320">**.NET Core**altında .NET Core sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-320">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="17cd3-321">**Uygulamaları Çalıştır-çalışma zamanı** sütununda, Istenen .NET Core çalışma zamanı sürümünün satırını bulun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-321">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="17cd3-322">**Çalışma zamanı & barındırma paketi** bağlantısını kullanarak yükleyiciyi indirin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-322">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="17cd3-323">Bazı yükleyiciler, artık Microsoft tarafından desteklenir ve bunların sona erecek (EOL'ye) ulaştınız yayın sürümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-323">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="17cd3-324">Daha fazla bilgi için bkz. [destek ilkesi](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="17cd3-324">For more information, see the [support policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="17cd3-325">Barındırma paket yükleme</span><span class="sxs-lookup"><span data-stu-id="17cd3-325">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="17cd3-326">Sunucuda yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-326">Run the installer on the server.</span></span> <span data-ttu-id="17cd3-327">Aşağıdaki parametreler, yükleyiciyi yönetici komut kabuğu 'ndan çalıştırırken kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-327">The following parameters are available when running the installer from an administrator command shell:</span></span>

   * <span data-ttu-id="17cd3-328">ASP.NET Core modülünü yüklemeyi `OPT_NO_ANCM=1` &ndash; atlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-328">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="17cd3-329">.NET Core çalışma zamanını yüklemeyi `OPT_NO_RUNTIME=1` &ndash; atlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-329">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span> <span data-ttu-id="17cd3-330">Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-330">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="17cd3-331">ASP.NET paylaşılan çerçevesini yüklemeyi `OPT_NO_SHAREDFX=1` &ndash; atlayın (ASP.NET Runtime).</span><span class="sxs-lookup"><span data-stu-id="17cd3-331">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span> <span data-ttu-id="17cd3-332">Sunucu yalnızca [kendi kendine içerilen dağıtımları (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd)barındırdığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-332">Used when the server only hosts [self-contained deployments (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>
   * <span data-ttu-id="17cd3-333">`OPT_NO_X86=1` &ndash; x86 çalışma zamanları yüklemeyi atlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-333">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="17cd3-334">32 bitlik uygulamalar barındırmayabildiğinizi bildiğiniz durumlarda bu parametreyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-334">Use this parameter when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="17cd3-335">Gelecekte 32-bit ve 64 bit uygulamaları barındırabilmeniz gereken herhangi bir şansınız varsa, bu parametreyi kullanmayın ve her iki çalışma zamanını da yüklemeyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-335">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this parameter and install both runtimes.</span></span>
   * <span data-ttu-id="17cd3-336">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash;, paylaşılan yapılandırma (*ApplicationHost. config*) IIS yüklemesiyle aynı MAKINELI olduğunda IIS paylaşılan yapılandırması kullanma denetimini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-336">`OPT_NO_SHARED_CONFIG_CHECK=1` &ndash; Disable the check for using an IIS Shared Configuration when the shared configuration (*applicationHost.config*) is on the same machine as the IIS installation.</span></span> <span data-ttu-id="17cd3-337">*Yalnızca ASP.NET Core 2,2 veya sonraki bir sürümü Paketcisi yükleyicilerini barındırmak için kullanılabilir.*</span><span class="sxs-lookup"><span data-stu-id="17cd3-337">*Only available for ASP.NET Core 2.2 or later Hosting Bundler installers.*</span></span> <span data-ttu-id="17cd3-338">Daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-338">For more information, see <xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration>.</span></span>
1. <span data-ttu-id="17cd3-339">Sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="17cd3-339">Restart the system or execute the following commands in a command shell:</span></span>

   ```console
   net stop was /y
   net start w3svc
   ```
   <span data-ttu-id="17cd3-340">Sistemde bir değişiklik'kurmak IIS çekme yeniden bir ortam değişkenidir, yol yapılan yükleyicisi tarafından.</span><span class="sxs-lookup"><span data-stu-id="17cd3-340">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="17cd3-341">ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için geri iletme davranışını benimsemez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-341">ASP.NET Core doesn't adopt roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="17cd3-342">Yeni bir barındırma paketi yükleyerek paylaşılan çerçeveyi yükselttikten sonra, sistemi yeniden başlatın veya komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="17cd3-342">After upgrading the shared framework by installing a new hosting bundle, restart the system or execute the following commands in a command shell:</span></span>

```console
net stop was /y
net start w3svc
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="17cd3-343">Barındırma paketini yüklerken IIS 'deki bireysel siteleri el ile durdurmanız gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-343">It isn't necessary to manually stop individual sites in IIS when installing the Hosting Bundle.</span></span> <span data-ttu-id="17cd3-344">Barındırılan uygulamalar (IIS siteleri) IIS yeniden başlatıldığında yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-344">Hosted apps (IIS sites) restart when IIS restarts.</span></span> <span data-ttu-id="17cd3-345">Uygulamalar, [uygulama başlatma modülünden](#application-initialization-module-and-idle-timeout)da dahil olmak üzere ilk isteklerini alırken yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-345">Apps start up again when they receive their first request, including from the [Application Initialization Module](#application-initialization-module-and-idle-timeout).</span></span>

<span data-ttu-id="17cd3-346">ASP.NET Core, paylaşılan çerçeve paketlerinin yama sürümleri için ileri sarma davranışını benimseme.</span><span class="sxs-lookup"><span data-stu-id="17cd3-346">ASP.NET Core adopts roll-forward behavior for patch releases of shared framework packages.</span></span> <span data-ttu-id="17cd3-347">IIS ile IIS tarafından barındırılan uygulamalar IIS ile yeniden başlatıldığında, ilk isteklerini alırken başvurulan paketlerinin en son düzeltme eki sürümleriyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-347">When apps hosted by IIS restart with IIS, the apps load with the latest patch releases of their referenced packages when they receive their first request.</span></span> <span data-ttu-id="17cd3-348">IIS yeniden başlatılırsa, uygulamalar yeniden başlatılır ve çalışan işlemlerine geri dönüştürüldüğünde geri alma davranışını gösterir ve ilk isteklerini alırlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-348">If IIS isn't restarted, apps restart and exhibit roll-forward behavior when their worker processes are recycled and they receive their first request.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="17cd3-349">IIS paylaşılan Yapılandırması hakkında bilgi için bkz. [IIS paylaşılan yapılandırması ile ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="17cd3-349">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="17cd3-350">Visual Studio ile yayımlama sırasında Web Dağıtımı'nı yükleyin</span><span class="sxs-lookup"><span data-stu-id="17cd3-350">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="17cd3-351">Uygulamaları [Web dağıtımı](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later)olan sunuculara dağıttığınızda, en son Web dağıtımı sürümünü sunucuya yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-351">When deploying apps to servers with [Web Deploy](/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="17cd3-352">Web Dağıtımı yüklemek için [Web Platformu Yükleyicisi 'ni (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) kullanın veya doğrudan [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=43717)'nden bir yükleyici edinin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-352">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="17cd3-353">Tercih edilen yöntem, Webpı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-353">The preferred method is to use WebPI.</span></span> <span data-ttu-id="17cd3-354">Webpı, tek başına bir kurulum ve barındırma sağlayıcıları için bir yapılandırma sağlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-354">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="17cd3-355">IIS sitesi oluştur</span><span class="sxs-lookup"><span data-stu-id="17cd3-355">Create the IIS site</span></span>

1. <span data-ttu-id="17cd3-356">Barındıran sistemde, uygulamanın yayımlanmış klasörleri ve dosyaları saklamak için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-356">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="17cd3-357">Aşağıdaki adımda, klasörün yolu, uygulamanın fiziksel yolu olarak IIS 'ye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-357">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span> <span data-ttu-id="17cd3-358">Uygulamanın dağıtım klasörü ve dosya düzeni hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-358">For more information on an app's deployment folder and file layout, see <xref:host-and-deploy/directory-structure>.</span></span>

1. <span data-ttu-id="17cd3-359">IIS Yöneticisi 'nde, **Bağlantılar** panelinde sunucunun düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-359">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="17cd3-360">**Siteler** klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-360">Right-click the **Sites** folder.</span></span> <span data-ttu-id="17cd3-361">Bağlamsal menüden **Web sitesi Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-361">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="17cd3-362">Bir **site adı** belirtin ve **fiziksel yolu** uygulamanın dağıtım klasörüne ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-362">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="17cd3-363">**Bağlama** yapılandırmasını sağlayın ve **Tamam**' ı seçerek Web sitesini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="17cd3-363">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Site adı, fiziksel yolunu ve ana bilgisayar adı Web sitesi Ekle adımda sağlayın.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="17cd3-365">Üst düzey joker karakter bağlamaları (`http://*:80/` ve `http://+:80` **) kullanılmamalıdır.**</span><span class="sxs-lookup"><span data-stu-id="17cd3-365">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="17cd3-366">Üst düzey joker bağlamaları uygulamanızı güvenlik açıklarından açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-366">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="17cd3-367">Bu, güçlü ve zayıf joker karakterler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-367">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="17cd3-368">Joker karakterler yerine açık bir ana bilgisayar adları kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-368">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="17cd3-369">Alt etki alanı joker karakteri bağlama (örneğin, `*.mysub.com`), tüm üst etki alanını (güvenlik açığı olan `*.com`aksine) kontrol ediyorsanız bu güvenlik riskine sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-369">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="17cd3-370">Daha fazla bilgi için bkz. [rfc7230 Section-5,4](https://tools.ietf.org/html/rfc7230#section-5.4) .</span><span class="sxs-lookup"><span data-stu-id="17cd3-370">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="17cd3-371">Sunucu düğümünün altında **uygulama havuzları**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-371">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="17cd3-372">Sitenin uygulama havuzuna sağ tıklayın ve bağlam menüsünden **temel ayarlar** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-372">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="17cd3-373">**Uygulama havuzunu Düzenle** penceresinde, **.NET CLR sürümünü** **yönetilen kod olmadan**ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-373">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Yönetilen kod yok, .NET CLR sürümü için ayarlayın.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="17cd3-375">ASP.NET Core, ayrı bir işlemde çalıştırır ve çalışma zamanı yönetir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-375">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="17cd3-376">ASP.NET Core, masaüstü CLR 'nin (.NET CLR) yüklenmemesine&mdash;.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı çalışan işlemde barındırmak için önyüklenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-376">ASP.NET Core doesn't rely on loading the desktop CLR (.NET CLR)&mdash;the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process.</span></span> <span data-ttu-id="17cd3-377">**.NET CLR sürümünün** **yönetilen kod olmadan** ayarlanması isteğe bağlıdır, ancak önerilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-377">Setting the **.NET CLR version** to **No Managed Code** is optional but recommended.</span></span>

1. <span data-ttu-id="17cd3-378">*ASP.NET Core 2,2 veya üzeri*: [işlem içi barındırma modelini](#in-process-hosting-model)kullanan 64 bit (x64) [tabanlı bir dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) için, 32-bit (x86) işlemleri için uygulama havuzunu devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-378">*ASP.NET Core 2.2 or later*: For a 64-bit (x64) [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) that uses the [in-process hosting model](#in-process-hosting-model), disable the app pool for 32-bit (x86) processes.</span></span>

   <span data-ttu-id="17cd3-379">IIS Yöneticisi > **uygulama havuzlarının** **Eylemler** kenar çubuğunda, **uygulama havuzu varsayılanlarını** veya **Gelişmiş ayarları**ayarla ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-379">In the **Actions** sidebar of IIS Manager > **Application Pools**, select **Set Application Pool Defaults** or **Advanced Settings**.</span></span> <span data-ttu-id="17cd3-380">**32 bitlik uygulamaları etkinleştir** ' i bulun ve değeri `False`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-380">Locate **Enable 32-Bit Applications** and set the value to `False`.</span></span> <span data-ttu-id="17cd3-381">Bu ayar [, işlem dışı barındırma](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model)için dağıtılan uygulamaları etkilemez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-381">This setting doesn't affect apps deployed for [out-of-process hosting](xref:host-and-deploy/aspnet-core-module#out-of-process-hosting-model).</span></span>

1. <span data-ttu-id="17cd3-382">İşlem modeli kimliği uygun izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-382">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="17cd3-383">Uygulama havuzunun varsayılan kimliği (**Işlem modeli** > **kimliği**), **applicationpokaydentity** 'den başka bir kimliğe değiştiyse, yeni kimliğin uygulamanın klasörüne, veritabanına ve diğer gerekli kaynaklara erişmek için gerekli izinlere sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-383">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="17cd3-384">Örneğin, uygulama havuzu, okuma ve yazma erişimi burada uygulama okur ve dosyalarını yazar klasörlerine gerektirir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-384">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="17cd3-385">**Windows kimlik doğrulama yapılandırması (Isteğe bağlı)**</span><span class="sxs-lookup"><span data-stu-id="17cd3-385">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="17cd3-386">Daha fazla bilgi için bkz. [Windows kimlik doğrulamasını yapılandırma](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="17cd3-386">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="17cd3-387">Uygulamayı dağıtma</span><span class="sxs-lookup"><span data-stu-id="17cd3-387">Deploy the app</span></span>

<span data-ttu-id="17cd3-388">Uygulamayı [IIS sitesi oluşturma](#create-the-iis-site) bölümünde oluşturulan IIS **fiziksel yol** klasörüne dağıtın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-388">Deploy the app to the IIS **Physical path** folder that was established in the [Create the IIS site](#create-the-iis-site) section.</span></span> <span data-ttu-id="17cd3-389">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) dağıtım için önerilen mekanizmadır, ancak uygulamayı projenin *Publish* klasöründen barındırma sisteminin dağıtım klasörüne taşımak için çeşitli seçenekler mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-389">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment, but several options exist for moving the app from the project's *publish* folder to the hosting system's deployment folder.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="17cd3-390">Visual Studio ile Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="17cd3-390">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="17cd3-391">Web Dağıtımı kullanılacak bir yayımlama profili oluşturma hakkında bilgi edinmek için bkz. [ASP.NET Core App Deployment Için Visual Studio yayımlama profilleri](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-391">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="17cd3-392">Barındırma sağlayıcısı, bir yayımlama profili veya oluşturma desteği sağlıyorsa, profilini indirin ve Visual Studio **Yayımlama** iletişim kutusunu kullanarak içeri aktarın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-392">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog:</span></span>

![Yayımlama iletişim kutusu sayfası](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="17cd3-394">Web dağıtımı Visual Studio'nun dışında</span><span class="sxs-lookup"><span data-stu-id="17cd3-394">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="17cd3-395">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) , komut satırından Visual Studio dışında da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-395">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="17cd3-396">Daha fazla bilgi için bkz. [Web Dağıtım aracı](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="17cd3-396">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="17cd3-397">Alternatif Web dağıtımı</span><span class="sxs-lookup"><span data-stu-id="17cd3-397">Alternatives to Web Deploy</span></span>

<span data-ttu-id="17cd3-398">Uygulamayı barındırma sistemine taşımak için el ile kopyalama, [xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy)veya [PowerShell](/powershell/)gibi çeşitli yöntemlerden birini kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-398">Use any of several methods to move the app to the hosting system, such as manual copy, [Xcopy](/windows-server/administration/windows-commands/xcopy), [Robocopy](/windows-server/administration/windows-commands/robocopy), or [PowerShell](/powershell/).</span></span>

<span data-ttu-id="17cd3-399">IIS 'ye dağıtım ASP.NET Core hakkında daha fazla bilgi için, [IIS yöneticileri Için dağıtım kaynakları](#deployment-resources-for-iis-administrators) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-399">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="17cd3-400">Web sitesine Gözat</span><span class="sxs-lookup"><span data-stu-id="17cd3-400">Browse the website</span></span>

<span data-ttu-id="17cd3-401">Uygulama barındırma sistemine dağıtıldıktan sonra, uygulamanın genel uç noktalarından birine bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-401">After the app is deployed to the hosting system, make a request to one of the app's public endpoints.</span></span>

<span data-ttu-id="17cd3-402">Aşağıdaki örnekte site, **bağlantı noktası** `80``www.mysite.com` IIS **ana bilgisayar adına** bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-402">In the following example, the site is bound to an IIS **Host name** of `www.mysite.com` on **Port** `80`.</span></span> <span data-ttu-id="17cd3-403">`http://www.mysite.com`bir istek yapıldı:</span><span class="sxs-lookup"><span data-stu-id="17cd3-403">A request is made to `http://www.mysite.com`:</span></span>

![Microsoft Edge tarayıcısı IIS başlangıç sayfası yüklendi.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="17cd3-405">Kilitli dağıtım dosyaları</span><span class="sxs-lookup"><span data-stu-id="17cd3-405">Locked deployment files</span></span>

<span data-ttu-id="17cd3-406">Uygulama çalışırken, dağıtım klasörü dosyalar kilitli olmadığı.</span><span class="sxs-lookup"><span data-stu-id="17cd3-406">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="17cd3-407">Dağıtım sırasında kilitli dosyalar üzerine yazılamaz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-407">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="17cd3-408">Kilitli dosyaları bir dağıtımda serbest bırakmak için aşağıdaki yaklaşımlardan **birini** kullanarak uygulama havuzunu durdurun:</span><span class="sxs-lookup"><span data-stu-id="17cd3-408">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="17cd3-409">Proje dosyasında Web Dağıtımı ve başvuru `Microsoft.NET.Sdk.Web` kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-409">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="17cd3-410">Bir *app_offline. htm* dosyası Web uygulaması dizininin köküne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-410">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="17cd3-411">Dosya olduğunda, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatır ve dağıtım sırasında *app_offline. htm* dosyasına hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-411">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="17cd3-412">Daha fazla bilgi için [ASP.NET Core modülü yapılandırma başvurusuna](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-412">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="17cd3-413">El ile uygulama havuzu IIS Yöneticisi'nde, sunucu üzerinde durdurun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-413">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="17cd3-414">*App_offline. htm* dosyasını bırakmak için PowerShell kullanın (PowerShell 5 veya üzerini gerektirir):</span><span class="sxs-lookup"><span data-stu-id="17cd3-414">Use PowerShell to drop *app_offline.htm* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="17cd3-415">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="17cd3-415">Data protection</span></span>

<span data-ttu-id="17cd3-416">[ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction) , kimlik doğrulamasında kullanılan ara yazılımlar dahil olmak üzere birkaç ASP.NET Core [middlewares](xref:fundamentals/middleware/index)tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-416">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="17cd3-417">Veri koruma API 'Leri Kullanıcı kodu tarafından çağrılmasa bile, kalıcı bir şifreleme [anahtarı deposu](xref:security/data-protection/implementation/key-management)oluşturmak için veri koruma bir dağıtım betiği veya Kullanıcı kodu ile yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-417">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="17cd3-418">Veri koruma yapılandırılmamışsa, anahtarlar bellekte tutulur ve uygulama yeniden başlatıldığında atılan.</span><span class="sxs-lookup"><span data-stu-id="17cd3-418">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="17cd3-419">Uygulama yeniden başlatıldığında anahtar halkası bellekte depolanıyorsa:</span><span class="sxs-lookup"><span data-stu-id="17cd3-419">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="17cd3-420">Tüm tanımlama bilgisi tabanlı kimlik doğrulama belirteçlerini geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-420">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="17cd3-421">Kullanıcıların, bir sonraki istekte tekrar oturum açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-421">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="17cd3-422">Anahtar halkası ile korunan tüm veriler artık şifresi çözülebilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-422">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="17cd3-423">Bu, [CSRF belirteçlerini](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) ve [ASP.NET Core MVC TempData tanımlama bilgilerini](xref:fundamentals/app-state#tempdata)içerebilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-423">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="17cd3-424">Anahtar halkasını sürdürmek için IIS altındaki veri korumasını yapılandırmak için aşağıdaki yaklaşımlardan **birini** kullanın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-424">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="17cd3-425">**Veri koruma kayıt defteri anahtarları oluşturma**</span><span class="sxs-lookup"><span data-stu-id="17cd3-425">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="17cd3-426">ASP.NET Core uygulamaları tarafından kullanılan veri koruma anahtarları, uygulamalar için dış kayıt defterinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-426">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="17cd3-427">Belirli bir uygulamanın anahtarları kalıcı hale getirmek için uygulama havuzu için kayıt defteri anahtarları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-427">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="17cd3-428">Tek başına, Web grubu olmayan IIS yüklemeleri için [Data Protection provision-AutoGenKeys. ps1 PowerShell betiği](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) , bir ASP.NET Core uygulamasıyla kullanılan her uygulama havuzu için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-428">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/dotnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="17cd3-429">Bu betik, yalnızca çalışan işlem hesabı uygulamanın uygulama havuzunun kimliği için erişilebilir HKLM Kayıt defterinde bir kayıt defteri anahtarı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-429">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="17cd3-430">Anahtarları, makine genelindeki anahtarla DPAPI kullanılarak, bekleme sırasında şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-430">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="17cd3-431">Web grubu senaryolarda, uygulama kendi veri koruma anahtarı halkası depolamak için bir UNC yolu kullanmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-431">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="17cd3-432">Varsayılan olarak, veri koruma anahtarları şifreli değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-432">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="17cd3-433">Dosya izinleri ağ paylaşımı için uygulamanın çalıştığı Windows hesabı sınırlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-433">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="17cd3-434">X X509 bekleyen anahtarlarınızı korumak için sertifika kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-434">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="17cd3-435">Kullanıcıların sertifikaları karşıya yüklemesine imkan tanıyan bir mekanizmayı göz önünde bulundurun: kullanıcının güvenilen sertifika içine Yerleştir sertifikaları depolamak ve bunlar tüm makinelerde kullanılabilir kullanıcının uygulama çalıştığı emin olun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-435">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="17cd3-436">Ayrıntılar için bkz. [ASP.NET Core veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) .</span><span class="sxs-lookup"><span data-stu-id="17cd3-436">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="17cd3-437">**Kullanıcı profilini yüklemek için IIS uygulama havuzunu yapılandırma**</span><span class="sxs-lookup"><span data-stu-id="17cd3-437">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="17cd3-438">Bu ayar, uygulama havuzunun **Gelişmiş ayarları** altındaki **işlem modeli** bölümünde bulunur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-438">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="17cd3-439">**Kullanıcı profilini** `True`için Yükle ' ye ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-439">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="17cd3-440">`True`olarak ayarlandığında anahtarlar Kullanıcı profili dizininde depolanır ve Kullanıcı hesabına özgü bir anahtarla DPAPI kullanılarak korunur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-440">When set to `True`, keys are stored in the user profile directory and protected using DPAPI with a key specific to the user account.</span></span> <span data-ttu-id="17cd3-441">Anahtarlar *% LocalAppData%/ASP.exe. net/DataProtection-Keys* klasörüne kalıcıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-441">Keys are persisted to the *%LOCALAPPDATA%/ASP.NET/DataProtection-Keys* folder.</span></span>

  <span data-ttu-id="17cd3-442">Uygulama havuzunun [Setprofileenvironment özniteliğinin](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) de etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-442">The app pool's [setProfileEnvironment attribute](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) must also be enabled.</span></span> <span data-ttu-id="17cd3-443">`setProfileEnvironment` varsayılan değeri `true`.</span><span class="sxs-lookup"><span data-stu-id="17cd3-443">The default value of `setProfileEnvironment` is `true`.</span></span> <span data-ttu-id="17cd3-444">Bazı senaryolarda (örneğin, Windows işletim sistemi), `setProfileEnvironment` `false`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-444">In some scenarios (for example, Windows OS), `setProfileEnvironment` is set to `false`.</span></span> <span data-ttu-id="17cd3-445">Anahtarlar beklenen şekilde Kullanıcı profili dizininde depolanmıyorsa:</span><span class="sxs-lookup"><span data-stu-id="17cd3-445">If keys aren't stored in the user profile directory as expected:</span></span>

  1. <span data-ttu-id="17cd3-446">*% Windir%/system32/inetsrv/config* klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-446">Navigate to the *%windir%/system32/inetsrv/config* folder.</span></span>
  1. <span data-ttu-id="17cd3-447">*ApplicationHost. config* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-447">Open the *applicationHost.config* file.</span></span>
  1. <span data-ttu-id="17cd3-448">`<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` öğesini bulun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-448">Locate the `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` element.</span></span>
  1. <span data-ttu-id="17cd3-449">`setProfileEnvironment` özniteliğinin mevcut olmadığını, bu değerin varsayılan olarak `true`veya özniteliğin değerini `true`olarak ayarlamış olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-449">Confirm that the `setProfileEnvironment` attribute isn't present, which defaults the value to `true`, or explicitly set the attribute's value to `true`.</span></span>

* <span data-ttu-id="17cd3-450">**Dosya sistemini anahtar halka deposu olarak kullanma**</span><span class="sxs-lookup"><span data-stu-id="17cd3-450">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="17cd3-451">[Dosya sistemini anahtar halka deposu olarak kullanmak](xref:security/data-protection/configuration/overview)için uygulama kodunu ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-451">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="17cd3-452">Kullanım X509 bir sertifika anahtar halkası korumak ve sertifika, güvenilen bir sertifika olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-452">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="17cd3-453">Otomatik olarak imzalanan sertifika ise, sertifikayı güvenilen kök deponuza koyun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-453">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="17cd3-454">IIS web grubunda kullanırken:</span><span class="sxs-lookup"><span data-stu-id="17cd3-454">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="17cd3-455">Tüm makineler erişebileceği bir dosya paylaşımı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-455">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="17cd3-456">X X509 dağıtma her makine için sertifika.</span><span class="sxs-lookup"><span data-stu-id="17cd3-456">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="17cd3-457">[Kodda veri korumasını](xref:security/data-protection/configuration/overview)yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-457">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="17cd3-458">**Veri koruma için makineye özel bir ilke ayarlama**</span><span class="sxs-lookup"><span data-stu-id="17cd3-458">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="17cd3-459">Veri koruma sistemi, veri koruma API 'Lerini kullanan tüm uygulamalar için varsayılan [makine genelinde bir ilke](xref:security/data-protection/configuration/machine-wide-policy) ayarlamak için sınırlı destek içerir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-459">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="17cd3-460">Daha fazla bilgi için bkz. <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-460">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="virtual-directories"></a><span data-ttu-id="17cd3-461">Sanal dizinler</span><span class="sxs-lookup"><span data-stu-id="17cd3-461">Virtual Directories</span></span>

<span data-ttu-id="17cd3-462">[IIS sanal dizinleri](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) ASP.NET Core uygulamalarla desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="17cd3-462">[IIS Virtual Directories](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories) aren't supported with ASP.NET Core apps.</span></span> <span data-ttu-id="17cd3-463">Bir uygulama, bir [alt uygulama](#sub-applications)olarak barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-463">An app can be hosted as a [sub-application](#sub-applications).</span></span>

## <a name="sub-applications"></a><span data-ttu-id="17cd3-464">Alt uygulamalar</span><span class="sxs-lookup"><span data-stu-id="17cd3-464">Sub-applications</span></span>

<span data-ttu-id="17cd3-465">ASP.NET Core bir uygulama, bir [IIS alt uygulaması (alt uygulama)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications)olarak barındırılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-465">An ASP.NET Core app can be hosted as an [IIS sub-application (sub-app)](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#applications).</span></span> <span data-ttu-id="17cd3-466">Sub uygulamanın yolu, uygulama kök URL'SİNİN bir parçası haline gelir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-466">The sub-app's path becomes part of the root app's URL.</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17cd3-467">Bir alt uygulama ASP.NET Core modülü bir işleyici içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-467">A sub-app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="17cd3-468">Modül bir alt uygulamanın *Web. config* dosyasına bir işleyici olarak eklenirse, alt uygulamaya gözatmaya çalışılırken hatalı yapılandırma dosyasına başvuran *500,19 iç sunucu hatası* alınır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-468">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="17cd3-469">Aşağıdaki örnek, bir ASP.NET Core alt uygulaması için yayımlanmış bir *Web. config* dosyası gösterir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-469">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="17cd3-470">Bir ASP.NET Core uygulamasının altında bir non-ASP.NET Core alt uygulaması barındırırken, alt uygulamanın *Web. config* dosyasında devralınan işleyiciyi açıkça kaldırın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-470">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app's *web.config* file:</span></span>

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

<span data-ttu-id="17cd3-471">Alt uygulama içindeki statik varlık bağlantıları, tilde işareti (`~/`) gösterimini kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-471">Static asset links within the sub-app should use tilde-slash (`~/`) notation.</span></span> <span data-ttu-id="17cd3-472">Tilde işareti gösterimi, alt uygulamanın pathbase 'i işlenmiş göreli bağlantıya eklemek için bir [etiket yardımcısını](xref:mvc/views/tag-helpers/intro) tetikler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-472">Tilde-slash notation triggers a [Tag Helper](xref:mvc/views/tag-helpers/intro) to prepend the sub-app's pathbase to the rendered relative link.</span></span> <span data-ttu-id="17cd3-473">`/subapp_path`bir alt uygulama için, `src="~/image.png"` ile bağlantılı bir görüntü `src="/subapp_path/image.png"`olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-473">For a sub-app at `/subapp_path`, an image linked with `src="~/image.png"` is rendered as `src="/subapp_path/image.png"`.</span></span> <span data-ttu-id="17cd3-474">Kök uygulamanın statik dosya ara yazılımlarını statik dosya istek işlemiyor.</span><span class="sxs-lookup"><span data-stu-id="17cd3-474">The root app's Static File Middleware doesn't process the static file request.</span></span> <span data-ttu-id="17cd3-475">İstek, alt uygulamanın statik dosya ara yazılımı tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-475">The request is processed by the sub-app's Static File Middleware.</span></span>

<span data-ttu-id="17cd3-476">Statik bir varlığın `src` özniteliği mutlak bir yola ayarlandıysa (örneğin, `src="/image.png"`), bağlantı alt uygulamanın pathbase olmadan işlenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-476">If a static asset's `src` attribute is set to an absolute path (for example, `src="/image.png"`), the link is rendered without the sub-app's pathbase.</span></span> <span data-ttu-id="17cd3-477">Kök uygulamanın statik dosya ara yazılımı, statik varlık kök uygulamadan kullanılabilir olmadığı takdirde *404-bulunamayan* bir Yanıt ile sonuçlanır. Bu, kök uygulamanın [Web kökünden](xref:fundamentals/index#web-root)varlık sunmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-477">The root app's Static File Middleware attempts to serve the asset from the root app's [web root](xref:fundamentals/index#web-root), which results in a *404 - Not Found* response unless the static asset is available from the root app.</span></span>

<span data-ttu-id="17cd3-478">ASP.NET Core uygulaması başka bir ASP.NET Core uygulaması altında bir alt uygulama olarak barındırmak için:</span><span class="sxs-lookup"><span data-stu-id="17cd3-478">To host an ASP.NET Core app as a sub-app under another ASP.NET Core app:</span></span>

1. <span data-ttu-id="17cd3-479">Alt uygulama için bir uygulama havuzu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-479">Establish an app pool for the sub-app.</span></span> <span data-ttu-id="17cd3-480">.NET Core için çekirdek ortak dil çalışma zamanı (CoreCLR), uygulamayı masaüstü CLR (.NET CLR) değil, çalışan işlemde barındırmak için önyüklenirken **.NET CLR sürümünü** **yönetilen kod olmadan** ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-480">Set the **.NET CLR Version** to **No Managed Code** because the Core Common Language Runtime (CoreCLR) for .NET Core is booted to host the app in the worker process, not the desktop CLR (.NET CLR).</span></span>

1. <span data-ttu-id="17cd3-481">IIS Yöneticisi'nde kök sitenin kök site altında bir klasöre alt uygulama ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-481">Add the root site in IIS Manager with the sub-app in a folder under the root site.</span></span>

1. <span data-ttu-id="17cd3-482">IIS Yöneticisi 'ndeki alt uygulama klasörüne sağ tıklayın ve **uygulamaya Dönüştür**' ü seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-482">Right-click the sub-app folder in IIS Manager and select **Convert to Application**.</span></span>

1. <span data-ttu-id="17cd3-483">**Uygulama Ekle** iletişim kutusunda, alt uygulama için oluşturduğunuz uygulama havuzunu atamak üzere **uygulama havuzunun** **Seç** düğmesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-483">In the **Add Application** dialog, use the **Select** button for the **Application Pool** to assign the app pool that you created for the sub-app.</span></span> <span data-ttu-id="17cd3-484">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-484">Select **OK**.</span></span>

<span data-ttu-id="17cd3-485">Bir alt uygulama ayrı bir uygulama havuzuna atamasını işlem içi barındırma modeli kullanılırken zorunludur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-485">The assignment of a separate app pool to the sub-app is a requirement when using the in-process hosting model.</span></span>

<span data-ttu-id="17cd3-486">İşlem içi barındırma modeli ve ASP.NET Core modülünü yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-486">For more information on the in-process hosting model and configuring the ASP.NET Core Module, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="17cd3-487">Web.config ile IIS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17cd3-487">Configuration of IIS with web.config</span></span>

<span data-ttu-id="17cd3-488">IIS yapılandırması, ASP.NET Core modüllü ASP.NET Core uygulamalar için işlevsel olan IIS senaryoları için *Web. config* 'in `<system.webServer>` bölümünden etkilenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-488">IIS configuration is influenced by the `<system.webServer>` section of *web.config* for IIS scenarios that are functional for ASP.NET Core apps with the ASP.NET Core Module.</span></span> <span data-ttu-id="17cd3-489">Örneğin, IIS için dinamik sıkıştırma çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-489">For example, IIS configuration is functional for dynamic compression.</span></span> <span data-ttu-id="17cd3-490">IIS sunucu düzeyinde dinamik sıkıştırma kullanmak üzere yapılandırıldıysa, uygulamanın *Web. config* dosyasındaki `<urlCompression>` öğesi, ASP.NET Core bir uygulama için devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-490">If IIS is configured at the server level to use dynamic compression, the `<urlCompression>` element in the app's *web.config* file can disable it for an ASP.NET Core app.</span></span>

<span data-ttu-id="17cd3-491">Daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-491">For more information, see the following topics:</span></span>

* [<span data-ttu-id="17cd3-492">\<System. webServer > için yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="17cd3-492">Configuration reference for \<system.webServer></span></span>](/iis/configuration/system.webServer/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>

<span data-ttu-id="17cd3-493">Yalıtılmış uygulama havuzlarında çalışan ayrı uygulamalara yönelik ortam değişkenlerini ayarlamak için (IIS 10,0 veya üzeri için desteklenir), IIS başvuru belgelerindeki [ortam değişkenleri \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-493">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="17cd3-494">Web.config yapılandırma bölümlerini</span><span class="sxs-lookup"><span data-stu-id="17cd3-494">Configuration sections of web.config</span></span>

<span data-ttu-id="17cd3-495">*Web. config* dosyasındaki ASP.NET 4. x uygulamalarının yapılandırma bölümleri yapılandırma için ASP.NET Core uygulamalar tarafından kullanılmaz:</span><span class="sxs-lookup"><span data-stu-id="17cd3-495">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* `<system.web>`
* `<appSettings>`
* `<connectionStrings>`
* `<location>`

<span data-ttu-id="17cd3-496">ASP.NET Core uygulamaları, diğer yapılandırma sağlayıcıları kullanılarak yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-496">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="17cd3-497">Daha fazla bilgi için bkz. [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="17cd3-497">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="17cd3-498">Uygulama havuzları</span><span class="sxs-lookup"><span data-stu-id="17cd3-498">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17cd3-499">Uygulama havuzu yalıtımı barındırma modeli tarafından belirlenir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-499">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="17cd3-500">İşlem içi barındırma &ndash; uygulamalar ayrı uygulama havuzlarında çalıştırmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-500">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="17cd3-501">İşlem dışı barındırma &ndash;, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmayı öneririz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-501">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="17cd3-502">IIS **Web sitesi ekleme** iletişim kutusu varsayılan olarak uygulama başına tek bir uygulama havuzu olur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-502">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="17cd3-503">Bir **site adı** sağlandığında, metin otomatik olarak **uygulama havuzu** metin kutusuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-503">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="17cd3-504">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-504">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17cd3-505">Bir sunucuda birden fazla Web sitesi barındırma, her uygulamayı kendi uygulama havuzunda çalıştırarak uygulamaları birbirinden yalıtmak öneririz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-505">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="17cd3-506">IIS **Web sitesi ekleme** iletişim kutusu varsayılan olarak bu yapılandırmaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-506">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="17cd3-507">Bir **site adı** sağlandığında, metin otomatik olarak **uygulama havuzu** metin kutusuna aktarılır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-507">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="17cd3-508">Yeni bir uygulama havuzu, bir site eklendiğinde site adı kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-508">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="17cd3-509">Uygulama havuzu kimliği</span><span class="sxs-lookup"><span data-stu-id="17cd3-509">Application Pool Identity</span></span>

<span data-ttu-id="17cd3-510">Bir uygulama havuzu kimlik hesabının etki alanı veya yerel hesap oluşturmak ve yönetmek zorunda kalmadan benzersiz bir hesabı altında çalıştırılacak bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-510">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="17cd3-511">IIS 8.0 veya sonraki sürümlerde, IIS Yönetici çalışan işlemi (WAS) yeni uygulama havuzu adı ile bir sanal hesabı oluşturur ve uygulama havuzunun çalışan işlemlerini bu hesap altında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-511">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="17cd3-512">Uygulama havuzunun **Gelişmiş ayarlar** altındaki IIS Yönetim Konsolu 'Nda, **kimliğin** **applicationpokaydentity**kullanılacak şekilde ayarlandığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="17cd3-512">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Uygulama havuzu Gelişmiş Ayarlar iletişim kutusu](index/_static/apppool-identity.png)

<span data-ttu-id="17cd3-514">IIS yönetim işlemi, Windows güvenlik sistemi, uygulama havuzunun adı ile güvenli bir tanıtıcı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17cd3-514">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="17cd3-515">Bu kimlik kullanarak kaynakları güvenliği sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-515">Resources can be secured using this identity.</span></span> <span data-ttu-id="17cd3-516">Ancak, bu kimlik bir gerçek kullanıcı hesabı değildir ve Windows kullanıcı yönetim konsolunda gösterilmesini.</span><span class="sxs-lookup"><span data-stu-id="17cd3-516">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="17cd3-517">IIS çalışan işlemi Uygulamayı yükseltilmiş erişim gerektiriyorsa, uygulamayı içeren dizine erişim denetimi listesi (ACL) değiştirin:</span><span class="sxs-lookup"><span data-stu-id="17cd3-517">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="17cd3-518">Windows Gezgini'ni açın ve dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-518">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="17cd3-519">Dizine sağ tıklayıp **Özellikler**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-519">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="17cd3-520">**Güvenlik** sekmesinde, **Düzenle** düğmesini ve ardından **Ekle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-520">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="17cd3-521">**Konumlar** düğmesini seçin ve sistemin seçili olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="17cd3-521">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="17cd3-522">**IIS AppPool\\< app_pool_name >** **Seçilecek nesne adlarını girin** alanına girin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-522">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="17cd3-523">**Adları denetle** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-523">Select the **Check Names** button.</span></span> <span data-ttu-id="17cd3-524">*DefaultAppPool* için **IIS APPPOOL\DefaultAppPool**kullanarak adları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-524">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="17cd3-525">**Adları denetle** düğmesi seçildiğinde, nesne adları alanında bir **DefaultAppPool** değeri gösterilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-525">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="17cd3-526">Uygulama havuzu adı doğrudan nesne adları alanına girmeniz mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-526">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="17cd3-527">Nesne adı denetlenirken **app_pool_name > biçim\\< IIS AppPool** 'ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-527">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Uygulama klasörü için Kullanıcı veya Grup Seç iletişim kutusu: "ad denetle" seçmeden önce nesne adları alanında "DefaultAppPool" uygulama havuzu adı "IIS AppPool\" eklenir.](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="17cd3-529">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-529">Select **OK**.</span></span>

   ![Kullanıcıları veya Grupları Seç iletişim uygulama klasörü için: "Adları denetle"'i seçtikten sonra "DefaultAppPool" nesnesinde gösterilen nesne adı ad alanı.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="17cd3-531">Okuma &amp; yürütme izinleri varsayılan olarak verilmelidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-531">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="17cd3-532">Gerektiğinde ek izinler sağlar.</span><span class="sxs-lookup"><span data-stu-id="17cd3-532">Provide additional permissions as needed.</span></span>

<span data-ttu-id="17cd3-533">Erişim, **ıacl 'ler** Aracı kullanılarak bir komut isteminde de verilebilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-533">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="17cd3-534">Örnek olarak *DefaultAppPool* kullanarak, aşağıdaki komut kullanılır:</span><span class="sxs-lookup"><span data-stu-id="17cd3-534">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="17cd3-535">Daha fazla bilgi için [ıcacl 'ler](/windows-server/administration/windows-commands/icacls) konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-535">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="17cd3-536">HTTP/2 desteği</span><span class="sxs-lookup"><span data-stu-id="17cd3-536">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17cd3-537">[Http/2](https://httpwg.org/specs/rfc7540.html) aşağıdaki IIS dağıtım senaryolarında ASP.NET Core desteklenir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-537">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="17cd3-538">İşlem içi</span><span class="sxs-lookup"><span data-stu-id="17cd3-538">In-process</span></span>
  * <span data-ttu-id="17cd3-539">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-539">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="17cd3-540">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="17cd3-540">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="17cd3-541">İşlem dışı</span><span class="sxs-lookup"><span data-stu-id="17cd3-541">Out-of-process</span></span>
  * <span data-ttu-id="17cd3-542">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-542">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="17cd3-543">Herkese açık uç sunucu bağlantıları HTTP/2 kullanır, ancak [Kestrel sunucusuyla](xref:fundamentals/servers/kestrel) ters proxy bağlantısı http/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-543">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="17cd3-544">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="17cd3-544">TLS 1.2 or later connection</span></span>

<span data-ttu-id="17cd3-545">Bir HTTP/2 bağlantısı oluşturulduğunda, bir işlem içi dağıtım için, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="17cd3-545">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="17cd3-546">Bir HTTP/2 bağlantısı oluşturulduğunda, bir işlem dışı dağıtımı için, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="17cd3-546">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="17cd3-547">İşlem içi ve işlem dışı barındırma modelleri hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="17cd3-547">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17cd3-548">[Http/2](https://httpwg.org/specs/rfc7540.html) , aşağıdaki temel gereksinimleri karşılayan işlem dışı dağıtımlar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="17cd3-548">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="17cd3-549">Windows Server 2016/Windows 10 veya üzeri; IIS 10 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="17cd3-549">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="17cd3-550">Herkese açık uç sunucu bağlantıları HTTP/2 kullanır, ancak [Kestrel sunucusuyla](xref:fundamentals/servers/kestrel) ters proxy bağlantısı http/1.1 kullanır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-550">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="17cd3-551">Hedef çerçeve: uygulanamaz işlem dışı dağıtımlar için bu yana HTTP/2 bağlantı tamamen IIS tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-551">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="17cd3-552">TLS 1.2 veya sonraki bir bağlantı</span><span class="sxs-lookup"><span data-stu-id="17cd3-552">TLS 1.2 or later connection</span></span>

<span data-ttu-id="17cd3-553">Bir HTTP/2 bağlantısı kurulduysa, [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) Reports `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="17cd3-553">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="17cd3-554">HTTP/2 varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-554">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="17cd3-555">Bir HTTP/2 bağlantı değil, bağlantılar, HTTP/1.1 geri döner.</span><span class="sxs-lookup"><span data-stu-id="17cd3-555">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="17cd3-556">IIS dağıtımları ile HTTP/2 yapılandırması hakkında daha fazla bilgi için bkz. [IIS 'de http/2](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="17cd3-556">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="cors-preflight-requests"></a><span data-ttu-id="17cd3-557">CORS ön denetim istekleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-557">CORS preflight requests</span></span>

<span data-ttu-id="17cd3-558">*Bu bölüm, yalnızca .NET Framework hedefleyen ASP.NET Core uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="17cd3-558">*This section only applies to ASP.NET Core apps that target the .NET Framework.*</span></span>

<span data-ttu-id="17cd3-559">.NET Framework hedefleyen ASP.NET Core bir uygulama için, Seçenekler istekleri uygulamaya varsayılan olarak IIS 'de aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="17cd3-559">For an ASP.NET Core app that targets the .NET Framework, OPTIONS requests aren't passed to the app by default in IIS.</span></span> <span data-ttu-id="17cd3-560">*Web. config* DOSYASıNDAKI uygulama IIS işleyicilerini seçenek isteklerini geçecek şekilde yapılandırma hakkında bilgi edinmek için bkz. [ASP.NET Web API 2 ' de çapraz kaynak ISTEKLERINI etkinleştirme: CORS 'nin nasıl çalıştığı](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span><span class="sxs-lookup"><span data-stu-id="17cd3-560">To learn how to configure the app's IIS handlers in *web.config* to pass OPTIONS requests, see [Enable cross-origin requests in ASP.NET Web API 2: How CORS Works](/aspnet/web-api/overview/security/enabling-cross-origin-requests-in-web-api#how-cors-works).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization-module-and-idle-timeout"></a><span data-ttu-id="17cd3-561">Uygulama başlatma modülü ve boşta kalma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="17cd3-561">Application Initialization Module and Idle Timeout</span></span>

<span data-ttu-id="17cd3-562">ASP.NET Core modülü sürüm 2 tarafından IIS 'de barındırıldığında:</span><span class="sxs-lookup"><span data-stu-id="17cd3-562">When hosted in IIS by the ASP.NET Core Module version 2:</span></span>

* <span data-ttu-id="17cd3-563">[Uygulama başlatma modülü](#application-initialization-module) &ndash; uygulamanın barındırılan veya [işlem dışı](#out-of-process-hosting-model) bir [çalışan işlem yeniden](#in-process-hosting-model) başlatma veya sunucu yeniden başlatma üzerinde otomatik olarak başlayacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-563">[Application Initialization Module](#application-initialization-module) &ndash; App's hosted [in-process](#in-process-hosting-model) or [out-of-process](#out-of-process-hosting-model) can be configured to start automatically on a worker process restart or server restart.</span></span>
* <span data-ttu-id="17cd3-564">[Boşta kalma zaman aşımı](#idle-timeout) &ndash; uygulamanın barındırılan, [işlem](#in-process-hosting-model) yapılmayan dönemler sırasında zaman aşımına uğramaması için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-564">[Idle Timeout](#idle-timeout) &ndash; App's hosted [in-process](#in-process-hosting-model) can be configured not to timeout during periods of inactivity.</span></span>

### <a name="application-initialization-module"></a><span data-ttu-id="17cd3-565">Uygulama başlatma modülü</span><span class="sxs-lookup"><span data-stu-id="17cd3-565">Application Initialization Module</span></span>

<span data-ttu-id="17cd3-566">*İşlem içi ve işlem dışı barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="17cd3-566">*Applies to apps hosted in-process and out-of-process.*</span></span>

<span data-ttu-id="17cd3-567">[IIS uygulaması başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) , uygulama havuzu başlatıldığında veya geri DÖNÜŞTÜRÜLDÜĞÜNDE uygulamaya http isteği gönderen bir IIS özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-567">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="17cd3-568">İstek, uygulamayı başlatmak üzere tetikler.</span><span class="sxs-lookup"><span data-stu-id="17cd3-568">The request triggers the app to start.</span></span> <span data-ttu-id="17cd3-569">Varsayılan olarak IIS, uygulamayı başlatmak için uygulamanın kök URL 'SI (`/`) için bir istek yayınlar (yapılandırma hakkında daha fazla bilgi için [ek kaynaklara](#application-initialization-module-and-idle-timeout-additional-resources) bakın).</span><span class="sxs-lookup"><span data-stu-id="17cd3-569">By default, IIS issues a request to the app's root URL (`/`) to initialize the app (see the [additional resources](#application-initialization-module-and-idle-timeout-additional-resources) for more details on configuration).</span></span>

<span data-ttu-id="17cd3-570">IIS uygulama başlatma rolü özelliğinin etkin olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-570">Confirm that the IIS Application Initialization role feature in enabled:</span></span>

<span data-ttu-id="17cd3-571">IIS 'yi yerel olarak kullanırken Windows 7 veya üzeri masaüstü sistemlerinde:</span><span class="sxs-lookup"><span data-stu-id="17cd3-571">On Windows 7 or later desktop systems when using IIS locally:</span></span>

1. <span data-ttu-id="17cd3-572">> programlar ve Özellikler > **Programlar** **ve Özellikler** ' **e gidin >** **Windows özelliklerini açın veya kapatın** (ekranın sol tarafında).</span><span class="sxs-lookup"><span data-stu-id="17cd3-572">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="17cd3-573">**Uygulama geliştirme özellikleri**> **Internet Information Services** > **World Wide Web Hizmetleri** 'ni açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-573">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="17cd3-574">**Uygulama başlatma**onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-574">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="17cd3-575">Windows Server 2008 R2 veya sonraki sürümlerde:</span><span class="sxs-lookup"><span data-stu-id="17cd3-575">On Windows Server 2008 R2 or later:</span></span>

1. <span data-ttu-id="17cd3-576">**Rol ve Özellik Ekleme Sihirbazı 'nı**açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-576">Open the **Add Roles and Features Wizard**.</span></span>
1. <span data-ttu-id="17cd3-577">**Rol hizmetlerini Seç** panelinde, **uygulama geliştirme** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-577">In the **Select role services** panel, open the **Application Development** node.</span></span>
1. <span data-ttu-id="17cd3-578">**Uygulama başlatma**onay kutusunu seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-578">Select the check box for **Application Initialization**.</span></span>

<span data-ttu-id="17cd3-579">Site için uygulama başlatma modülünü etkinleştirmek üzere aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-579">Use either of the following approaches to enable the Application Initialization Module for the site:</span></span>

* <span data-ttu-id="17cd3-580">IIS Yöneticisi 'Ni kullanma:</span><span class="sxs-lookup"><span data-stu-id="17cd3-580">Using IIS Manager:</span></span>

  1. <span data-ttu-id="17cd3-581">**Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-581">Select **Application Pools** in the **Connections** panel.</span></span>
  1. <span data-ttu-id="17cd3-582">Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-582">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
  1. <span data-ttu-id="17cd3-583">Varsayılan **Başlangıç modu** **OnDemand**' dir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-583">The default **Start Mode** is **OnDemand**.</span></span> <span data-ttu-id="17cd3-584">**Başlangıç modunu** **AlwaysRunning**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-584">Set the **Start Mode** to **AlwaysRunning**.</span></span> <span data-ttu-id="17cd3-585">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-585">Select **OK**.</span></span>
  1. <span data-ttu-id="17cd3-586">**Bağlantılar** panelinde **siteler** düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-586">Open the **Sites** node in the **Connections** panel.</span></span>
  1. <span data-ttu-id="17cd3-587">Uygulamaya sağ tıklayın ve **Gelişmiş ayarlar**> **Web sitesini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-587">Right-click the app and select **Manage Website** > **Advanced Settings**.</span></span>
  1. <span data-ttu-id="17cd3-588">Varsayılan **önyükleme etkin** ayarı **false**şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="17cd3-588">The default **Preload Enabled** setting is **False**.</span></span> <span data-ttu-id="17cd3-589">**Önyükleme etkin** ' i **true**olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-589">Set **Preload Enabled** to **True**.</span></span> <span data-ttu-id="17cd3-590">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-590">Select **OK**.</span></span>

* <span data-ttu-id="17cd3-591">*Web. config*kullanarak, uygulamanın *Web. config* dosyasındaki `<system.webServer>` öğelerine `true` `doAppInitAfterRestart` ayarlanmış `<applicationInitialization>` öğesini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="17cd3-591">Using *web.config*, add the `<applicationInitialization>` element with `doAppInitAfterRestart` set to `true` to the `<system.webServer>` elements in the app's *web.config* file:</span></span>

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

### <a name="idle-timeout"></a><span data-ttu-id="17cd3-592">Boşta Kalma Süresi Zaman Aşımı</span><span class="sxs-lookup"><span data-stu-id="17cd3-592">Idle Timeout</span></span>

<span data-ttu-id="17cd3-593">*Yalnızca işlem sırasında barındırılan uygulamalar için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="17cd3-593">*Only applies to apps hosted in-process.*</span></span>

<span data-ttu-id="17cd3-594">Uygulamanın çalışmasını engellemek için, IIS Yöneticisi 'Ni kullanarak uygulama havuzunun boşta kalma zaman aşımını ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-594">To prevent the app from idling, set the app pool's idle timeout using IIS Manager:</span></span>

1. <span data-ttu-id="17cd3-595">**Bağlantılar** panelinde **uygulama havuzları** ' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-595">Select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="17cd3-596">Listedeki uygulamanın uygulama havuzuna sağ tıklayın ve **Gelişmiş ayarlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-596">Right-click the app's app pool in the list and select **Advanced Settings**.</span></span>
1. <span data-ttu-id="17cd3-597">Varsayılan **boşta kalma süresi (dakika)** **20** dakikadır.</span><span class="sxs-lookup"><span data-stu-id="17cd3-597">The default **Idle Time-out (minutes)** is **20** minutes.</span></span> <span data-ttu-id="17cd3-598">**Boşta kalma zaman aşımını (dakika)** **0** (sıfır) olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-598">Set the **Idle Time-out (minutes)** to **0** (zero).</span></span> <span data-ttu-id="17cd3-599">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="17cd3-599">Select **OK**.</span></span>
1. <span data-ttu-id="17cd3-600">Çalışan işlemini geri dönüştür.</span><span class="sxs-lookup"><span data-stu-id="17cd3-600">Recycle the worker process.</span></span>

<span data-ttu-id="17cd3-601">[İşlem dışı](#out-of-process-hosting-model) barındırılan uygulamaların zaman aşımına uğramasını engellemek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="17cd3-601">To prevent apps hosted [out-of-process](#out-of-process-hosting-model) from timing out, use either of the following approaches:</span></span>

* <span data-ttu-id="17cd3-602">Uygulamayı çalışır durumda tutmak için bir dış hizmetten ping yapın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-602">Ping the app from an external service in order to keep it running.</span></span>
* <span data-ttu-id="17cd3-603">Uygulama yalnızca arka plan hizmetleri barındırıyorsa, IIS barındırmaktan kaçının ve [ASP.NET Core uygulamasını barındırmak için bir Windows hizmeti](xref:host-and-deploy/windows-service)kullanın.</span><span class="sxs-lookup"><span data-stu-id="17cd3-603">If the app only hosts background services, avoid IIS hosting and use a [Windows Service to host the ASP.NET Core app](xref:host-and-deploy/windows-service).</span></span>

### <a name="application-initialization-module-and-idle-timeout-additional-resources"></a><span data-ttu-id="17cd3-604">Uygulama başlatma modülü ve boşta kalma zaman aşımı ek kaynakları</span><span class="sxs-lookup"><span data-stu-id="17cd3-604">Application Initialization Module and Idle Timeout additional resources</span></span>

* [<span data-ttu-id="17cd3-605">IIS 8,0 uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="17cd3-605">IIS 8.0 Application Initialization</span></span>](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization)
* <span data-ttu-id="17cd3-606">[Uygulama başlatma \<applicationınitialization >](/iis/configuration/system.webserver/applicationinitialization/).</span><span class="sxs-lookup"><span data-stu-id="17cd3-606">[Application Initialization \<applicationInitialization>](/iis/configuration/system.webserver/applicationinitialization/).</span></span>
* <span data-ttu-id="17cd3-607">[Bir uygulama havuzu Için Işlem modeli ayarları \<processModel >](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span><span class="sxs-lookup"><span data-stu-id="17cd3-607">[Process Model Settings for an Application Pool \<processModel>](/iis/configuration/system.applicationhost/applicationpools/add/processmodel).</span></span>

::: moniker-end

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="17cd3-608">IIS Yöneticiler için dağıtım kaynakları</span><span class="sxs-lookup"><span data-stu-id="17cd3-608">Deployment resources for IIS administrators</span></span>

* [<span data-ttu-id="17cd3-609">IIS belgeleri</span><span class="sxs-lookup"><span data-stu-id="17cd3-609">IIS documentation</span></span>](/iis)
* [<span data-ttu-id="17cd3-610">IIS 'de IIS Yöneticisi 'Ni kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="17cd3-610">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="17cd3-611">.NET Core uygulama dağıtımı</span><span class="sxs-lookup"><span data-stu-id="17cd3-611">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:host-and-deploy/iis/modules>
* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>

## <a name="additional-resources"></a><span data-ttu-id="17cd3-612">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="17cd3-612">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:index>
* [<span data-ttu-id="17cd3-613">Resmi Microsoft IIS sitesi</span><span class="sxs-lookup"><span data-stu-id="17cd3-613">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="17cd3-614">Windows Server teknik içerik kitaplığı</span><span class="sxs-lookup"><span data-stu-id="17cd3-614">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="17cd3-615">IIS üzerinde HTTP/2</span><span class="sxs-lookup"><span data-stu-id="17cd3-615">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
* <xref:host-and-deploy/iis/transform-webconfig>
