---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core modülü Kestrel web sunucusunu IIS veya IIS Express kullanacak şekilde nasıl olanak tanıdığını öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861465"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="c3fb6-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="c3fb6-103">ASP.NET Core Module</span></span>

<span data-ttu-id="c3fb6-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), ve [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c3fb6-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c3fb6-105">ASP.NET Core modülü ASP.NET Core uygulamalarını bir IIS çalışan işlemindeki çalışmasına izin verir. (*işlem içi*) veya bir ters proxy yapılandırması IIS'de arkasında (*işlem dışı*).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="c3fb6-106">IIS, Gelişmiş bir web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c3fb6-107">ASP.NET Core, ASP.NET Core modülü IIS bir ters proxy yapılandırmasında çalıştırılacak uygulamaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="c3fb6-108">IIS, Gelişmiş bir web uygulaması güvenlik ve yönetilebilirlik özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="c3fb6-109">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-109">Supported Windows versions:</span></span>

* <span data-ttu-id="c3fb6-110">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c3fb6-110">Windows 7 or later</span></span>
* <span data-ttu-id="c3fb6-111">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c3fb6-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c3fb6-112">Barındırma işlemi içinde IIS işlem sunucusu uygulaması, IIS HTTP sunucusu modülü kullanır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="c3fb6-113">İşlem dışı barındırırken, modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="c3fb6-114">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c3fb6-115">Modül yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-115">The module only works with Kestrel.</span></span> <span data-ttu-id="c3fb6-116">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="c3fb6-117">ASP.NET Core modülü açıklaması</span><span class="sxs-lookup"><span data-stu-id="c3fb6-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c3fb6-118">ASP.NET Core modülü ya da IIS kanala takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="c3fb6-119">IIS çalışan işlemi içinde ASP.NET Core uygulaması ana bilgisayar (`w3wp.exe`) adlı [işlem içi barındırma modeli](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="c3fb6-120">Arka uç, çalışan bir ASP.NET Core uygulaması için web istekleri iletmek [Kestrel sunucu](xref:fundamentals/servers/kestrel)adlı [işlem dışı barındırma modeli](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="c3fb6-121">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="c3fb6-121">In-process hosting model</span></span>

<span data-ttu-id="c3fb6-122">İşlemdeki barındırma, bir ASP.NET Core kullanarak uygulama IIS çalışan işlemi ile aynı işlemde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="c3fb6-123">Bu proxy isteklerin performans cezası geri döngü bağdaştırıcısından giden işlem barındırma modeli kullanılırken kaldırır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="c3fb6-124">IIS işleme Süreci Yönetimi ile [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="c3fb6-125">ASP.NET Core Modülü:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="c3fb6-126">Uygulama başlatmayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-126">Performs app initialization.</span></span>
  * <span data-ttu-id="c3fb6-127">Yükleri [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="c3fb6-128">Çağrıları `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="c3fb6-129">Yerel IIS istek ömrünü işler.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="c3fb6-130">Aşağıdaki diyagram IIS, ASP.NET Core modülü arasındaki ilişkiyi gösterir ve uygulama işlemde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="c3fb6-132">Bir istek için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="c3fb6-133">Sürücü IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yerel istek yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="c3fb6-134">Modülün yerel isteği alır ve IIS HTTP sunucusuna geçirir (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="c3fb6-135">IIS HTTP sunucusu isteği Yerelden yönetilene dönüştüren bir IIS işlemdeki sunucu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="c3fb6-136">IIS HTTP sunucusu isteği işledikten sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="c3fb6-137">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="c3fb6-138">Uygulamanın yanıt IIS, yeniden istek başlatılan istemciye hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="c3fb6-139">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="c3fb6-139">Out-of-process hosting model</span></span>

<span data-ttu-id="c3fb6-140">Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, işlem yönetimi modül işler.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="c3fb6-141">İlk istek ulaştığında ve kapatılır veya çöküyor uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="c3fb6-142">Bu aslında aynı işlemde çalışan tarafından yönetilen uygulamalarla görüldüğü gibi davranıştır [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="c3fb6-143">Aşağıdaki diyagram IIS, ASP.NET Core modülü arasındaki ilişkiyi gösterir ve uygulama barındırılan işlem dışı:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="c3fb6-145">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="c3fb6-146">Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="c3fb6-147">Modülün istekleri Kestrel rastgele bir bağlantı için bağlantı noktası değil uygulamanın iletir 80/443'tür.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="c3fb6-148">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="c3fb6-149">Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="c3fb6-150">İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="c3fb6-151">Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="c3fb6-152">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="c3fb6-153">IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="c3fb6-154">Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c3fb6-155">ASP.NET Core modülü, uygulama arka ucu ASP.NET Core web isteklerini iletmek için IIS ardışık düzende takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="c3fb6-156">Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="c3fb6-157">İlk istek ulaştığında ve onu bir çökme gerçekleşirse, uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="c3fb6-158">Bu aslında aynı işlemde çalışan ASP.NET 4.x uygulamalar ile görüldüğü gibi davranıştır tarafından yönetildiği IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="c3fb6-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="c3fb6-159">Aşağıdaki diyagramda, IIS, ASP.NET Core modülü ve uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="c3fb6-161">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="c3fb6-162">Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="c3fb6-163">Modülün istekleri Kestrel rastgele bir bağlantı için bağlantı noktası değil uygulamanın iletir 80/443'tür.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="c3fb6-164">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="c3fb6-165">Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="c3fb6-166">İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="c3fb6-167">Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="c3fb6-168">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="c3fb6-169">IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="c3fb6-170">Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="c3fb6-171">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="c3fb6-172">IIS modüllerini etkin modülüyle hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="c3fb6-173">ASP.NET Core modülü birkaç diğer işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="c3fb6-174">Modül yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c3fb6-174">The module can:</span></span>

* <span data-ttu-id="c3fb6-175">Çalışan işlemi için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="c3fb6-176">Çıkış başlatma sorunlarını gidermek için dosya depolama için stdout'u günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="c3fb6-177">Windows kimlik doğrulama belirteçlerinizi iletin.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="c3fb6-178">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="c3fb6-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="c3fb6-179">Yükleme ve ASP.NET Core modülü kullanma konusunda ayrıntılı yönergeler için bkz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="c3fb6-180">Modül yapılandırma hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="c3fb6-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3fb6-181">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c3fb6-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="c3fb6-182">ASP.NET Core modülü GitHub deposu (kaynak kodu)</span><span class="sxs-lookup"><span data-stu-id="c3fb6-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
