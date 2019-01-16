---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: a39ece169199d256b2914d6b1a6d24dbf6a276d6
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341790"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="57ecb-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="57ecb-103">ASP.NET Core Module</span></span>

<span data-ttu-id="57ecb-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="57ecb-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="57ecb-105">ASP.NET Core modülü ya da IIS kanala takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="57ecb-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="57ecb-106">IIS çalışan işlemi içinde ASP.NET Core uygulaması ana bilgisayar (`w3wp.exe`) adlı [işlem içi barındırma modeli](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="57ecb-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="57ecb-107">Arka uç, çalışan bir ASP.NET Core uygulaması için web istekleri iletmek [Kestrel sunucu](xref:fundamentals/servers/kestrel)adlı [işlem dışı barındırma modeli](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="57ecb-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="57ecb-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="57ecb-108">Supported Windows versions:</span></span>

* <span data-ttu-id="57ecb-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="57ecb-109">Windows 7 or later</span></span>
* <span data-ttu-id="57ecb-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="57ecb-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="57ecb-111">İşlem içi barındırırken modülü bir işlem sunucusu uygulama IIS HTTP sunucusu olarak adlandırılır, IIS için kullanır. (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="57ecb-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="57ecb-112">İşlem dışı barındırırken, modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="57ecb-113">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="57ecb-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="57ecb-114">Barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="57ecb-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="57ecb-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="57ecb-115">In-process hosting model</span></span>

<span data-ttu-id="57ecb-116">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreHostingModel>` özellik değerini içeren uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma ile ayarlanır `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="57ecb-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="57ecb-117">İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="57ecb-118">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="57ecb-119">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="57ecb-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="57ecb-120">IIS HTTP sunucusu (`IISHttpServer`) yerine kullanılan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

* <span data-ttu-id="57ecb-121">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-121">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="57ecb-122">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="57ecb-122">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="57ecb-123">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-123">Use one app pool per app.</span></span>

* <span data-ttu-id="57ecb-124">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="57ecb-124">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="57ecb-125">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-125">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="57ecb-126">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-126">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="57ecb-127">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-127">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="57ecb-128">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-128">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="57ecb-129">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-129">Client disconnects are detected.</span></span> <span data-ttu-id="57ecb-130">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="57ecb-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="57ecb-131"><xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine yerine IIS tarafından başlatılan işlem alt dizinini döndürür (örneğin, *C:\Windows\System32\inetsrv* için *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="57ecb-131"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="57ecb-132">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="57ecb-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="57ecb-133">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="57ecb-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="57ecb-134">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="57ecb-135">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="57ecb-135">Out-of-process hosting model</span></span>

<span data-ttu-id="57ecb-136">İşlem dışı barındırmak için bir uygulamayı yapılandırmak için proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="57ecb-136">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="57ecb-137">Belirtmeyin `<AspNetCoreHostingModel>` özelliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-137">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="57ecb-138">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-138">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="57ecb-139">Değerini `<AspNetCoreHostingModel>` özelliğini `OutOfProcess` (işlem içi barındırma ile ayarlanır `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="57ecb-139">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="57ecb-140">[Kestrel'i](xref:fundamentals/servers/kestrel) sunucusu yerine IIS HTTP sunucusu kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="57ecb-140">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="57ecb-141">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="57ecb-141">Hosting model changes</span></span>

<span data-ttu-id="57ecb-142">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-142">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="57ecb-143">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="57ecb-143">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="57ecb-144">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-144">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="57ecb-145">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="57ecb-145">Process name</span></span>

<span data-ttu-id="57ecb-146">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="57ecb-146">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="57ecb-147">ASP.NET Core modülü, uygulama arka ucu ASP.NET Core web isteklerini iletmek için IIS ardışık düzende takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-147">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="57ecb-148">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="57ecb-148">Supported Windows versions:</span></span>

* <span data-ttu-id="57ecb-149">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="57ecb-149">Windows 7 or later</span></span>
* <span data-ttu-id="57ecb-150">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="57ecb-150">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="57ecb-151">Modül yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-151">The module only works with Kestrel.</span></span> <span data-ttu-id="57ecb-152">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="57ecb-152">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="57ecb-153">Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="57ecb-153">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="57ecb-154">İlk istek ulaştığında ve onu bir çökme gerçekleşirse, uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar.</span><span class="sxs-lookup"><span data-stu-id="57ecb-154">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="57ecb-155">Bu aslında aynı işlemde çalışan ASP.NET 4.x uygulamalar ile görüldüğü gibi davranıştır tarafından yönetildiği IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="57ecb-155">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="57ecb-156">Aşağıdaki diyagramda, IIS, ASP.NET Core modülü ve uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="57ecb-156">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="57ecb-158">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-158">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="57ecb-159">Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-159">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="57ecb-160">Modül Kestrel rastgele bağlantı noktası için 80 veya 443 bağlantı noktası olmadığından uygulama isteklerini iletir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-160">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="57ecb-161">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-161">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="57ecb-162">Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-162">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="57ecb-163">İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-163">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="57ecb-164">Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-164">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="57ecb-165">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="57ecb-165">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="57ecb-166">IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-166">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="57ecb-167">Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-167">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="57ecb-168">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-168">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="57ecb-169">ASP.NET Core modülü içeren etkin IIS modülleri hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="57ecb-169">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="57ecb-170">ASP.NET Core modülü de yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="57ecb-170">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="57ecb-171">Çalışan işlemi için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-171">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="57ecb-172">Çıkış başlatma sorunlarını gidermek için dosya depolama için stdout'u günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="57ecb-172">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="57ecb-173">Windows kimlik doğrulama belirteçlerinizi iletin.</span><span class="sxs-lookup"><span data-stu-id="57ecb-173">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="57ecb-174">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="57ecb-174">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="57ecb-175">Yükleme ve ASP.NET Core modülü kullanma hakkında yönergeler için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="57ecb-175">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="57ecb-176">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="57ecb-176">Configuration with web.config</span></span>

<span data-ttu-id="57ecb-177">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="57ecb-177">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="57ecb-178">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="57ecb-178">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="57ecb-179">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="57ecb-179">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="57ecb-180"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="57ecb-180">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="57ecb-181">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-181">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="57ecb-182">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="57ecb-182">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="57ecb-183">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="57ecb-183">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="57ecb-184">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="57ecb-184">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="57ecb-185">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="57ecb-185">Attribute</span></span> | <span data-ttu-id="57ecb-186">Açıklama</span><span class="sxs-lookup"><span data-stu-id="57ecb-186">Description</span></span> | <span data-ttu-id="57ecb-187">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="57ecb-187">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="57ecb-188">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-188">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-189">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-189">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="57ecb-190">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-190">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-191">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-191">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="57ecb-192">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-192">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-193">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-193">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="57ecb-194">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-194">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="57ecb-195">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-195">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-196">Barındırma modeli işlemdeki belirtir (`InProcess`) veya işlem dışı (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="57ecb-196">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="57ecb-197">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-197">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-198">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="57ecb-198">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="57ecb-199">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-199">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="57ecb-200">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-200">Default: `1`</span></span><br><span data-ttu-id="57ecb-201">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-201">Min: `1`</span></span><br><span data-ttu-id="57ecb-202">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="57ecb-202">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="57ecb-203">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-203">Required string attribute.</span></span></p><p><span data-ttu-id="57ecb-204">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-204">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="57ecb-205">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-205">Relative paths are supported.</span></span> <span data-ttu-id="57ecb-206">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-206">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="57ecb-207">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-208">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-208">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="57ecb-209">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-209">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="57ecb-210">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-210">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="57ecb-211">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-211">Default: `10`</span></span><br><span data-ttu-id="57ecb-212">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-212">Min: `0`</span></span><br><span data-ttu-id="57ecb-213">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="57ecb-213">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="57ecb-214">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-214">Optional timespan attribute.</span></span></p><p><span data-ttu-id="57ecb-215">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-215">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="57ecb-216">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-216">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="57ecb-217">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-217">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="57ecb-218">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="57ecb-218">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="57ecb-219">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-219">Default: `00:02:00`</span></span><br><span data-ttu-id="57ecb-220">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-220">Min: `00:00:00`</span></span><br><span data-ttu-id="57ecb-221">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="57ecb-222">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-223">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="57ecb-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="57ecb-224">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-224">Default: `10`</span></span><br><span data-ttu-id="57ecb-225">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-225">Min: `0`</span></span><br><span data-ttu-id="57ecb-226">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="57ecb-227">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-228">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="57ecb-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="57ecb-229">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-229">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="57ecb-230">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="57ecb-230">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="57ecb-231">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="57ecb-231">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="57ecb-232">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="57ecb-232">Default: `120`</span></span><br><span data-ttu-id="57ecb-233">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-233">Min: `0`</span></span><br><span data-ttu-id="57ecb-234">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-234">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="57ecb-235">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-235">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-236">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-236">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="57ecb-237">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-237">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-238">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-238">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="57ecb-239">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-239">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="57ecb-240">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-240">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="57ecb-241">Günlük dosyası oluşturulduğunda yolunda sağlanan herhangi bir klasörde modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-241">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="57ecb-242">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-242">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="57ecb-243">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="57ecb-243">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="57ecb-244">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="57ecb-244">Attribute</span></span> | <span data-ttu-id="57ecb-245">Açıklama</span><span class="sxs-lookup"><span data-stu-id="57ecb-245">Description</span></span> | <span data-ttu-id="57ecb-246">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="57ecb-246">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="57ecb-247">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-247">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-248">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-248">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="57ecb-249">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-249">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-250">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-250">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="57ecb-251">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-251">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-252">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-252">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="57ecb-253">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-253">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="57ecb-254">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-254">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-255">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="57ecb-255">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="57ecb-256">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-256">Default: `1`</span></span><br><span data-ttu-id="57ecb-257">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-257">Min: `1`</span></span><br><span data-ttu-id="57ecb-258">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="57ecb-258">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="57ecb-259">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-259">Required string attribute.</span></span></p><p><span data-ttu-id="57ecb-260">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-260">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="57ecb-261">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-261">Relative paths are supported.</span></span> <span data-ttu-id="57ecb-262">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-262">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="57ecb-263">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-264">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-264">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="57ecb-265">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-265">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="57ecb-266">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-266">Default: `10`</span></span><br><span data-ttu-id="57ecb-267">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-267">Min: `0`</span></span><br><span data-ttu-id="57ecb-268">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="57ecb-268">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="57ecb-269">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-269">Optional timespan attribute.</span></span></p><p><span data-ttu-id="57ecb-270">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-270">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="57ecb-271">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-271">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="57ecb-272">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-272">Default: `00:02:00`</span></span><br><span data-ttu-id="57ecb-273">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-273">Min: `00:00:00`</span></span><br><span data-ttu-id="57ecb-274">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-274">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="57ecb-275">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-276">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="57ecb-276">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="57ecb-277">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-277">Default: `10`</span></span><br><span data-ttu-id="57ecb-278">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-278">Min: `0`</span></span><br><span data-ttu-id="57ecb-279">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-279">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="57ecb-280">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-280">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-281">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="57ecb-281">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="57ecb-282">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-282">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="57ecb-283">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="57ecb-283">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="57ecb-284">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="57ecb-284">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="57ecb-285">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="57ecb-285">Default: `120`</span></span><br><span data-ttu-id="57ecb-286">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-286">Min: `0`</span></span><br><span data-ttu-id="57ecb-287">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-287">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="57ecb-288">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-288">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-289">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-289">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="57ecb-290">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-290">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-291">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-291">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="57ecb-292">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-292">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="57ecb-293">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-293">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="57ecb-294">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-294">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="57ecb-295">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-295">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="57ecb-296">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="57ecb-296">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="57ecb-297">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="57ecb-297">Attribute</span></span> | <span data-ttu-id="57ecb-298">Açıklama</span><span class="sxs-lookup"><span data-stu-id="57ecb-298">Description</span></span> | <span data-ttu-id="57ecb-299">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="57ecb-299">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="57ecb-300">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-300">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-301">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-301">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="57ecb-302">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-302">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-303">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-303">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="57ecb-304">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-304">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-305">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-305">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="57ecb-306">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-306">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="57ecb-307">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-307">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-308">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="57ecb-308">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="57ecb-309">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-309">Default: `1`</span></span><br><span data-ttu-id="57ecb-310">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="57ecb-310">Min: `1`</span></span><br><span data-ttu-id="57ecb-311">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="57ecb-311">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="57ecb-312">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-312">Required string attribute.</span></span></p><p><span data-ttu-id="57ecb-313">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-313">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="57ecb-314">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-314">Relative paths are supported.</span></span> <span data-ttu-id="57ecb-315">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-315">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="57ecb-316">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-316">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-317">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="57ecb-317">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="57ecb-318">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-318">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="57ecb-319">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-319">Default: `10`</span></span><br><span data-ttu-id="57ecb-320">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-320">Min: `0`</span></span><br><span data-ttu-id="57ecb-321">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="57ecb-321">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="57ecb-322">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-322">Optional timespan attribute.</span></span></p><p><span data-ttu-id="57ecb-323">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-323">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="57ecb-324">ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-324">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="57ecb-325">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-325">Default: `00:02:00`</span></span><br><span data-ttu-id="57ecb-326">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-326">Min: `00:00:00`</span></span><br><span data-ttu-id="57ecb-327">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="57ecb-327">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="57ecb-328">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-328">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-329">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="57ecb-329">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="57ecb-330">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="57ecb-330">Default: `10`</span></span><br><span data-ttu-id="57ecb-331">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-331">Min: `0`</span></span><br><span data-ttu-id="57ecb-332">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-332">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="57ecb-333">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-333">Optional integer attribute.</span></span></p><p><span data-ttu-id="57ecb-334">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="57ecb-334">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="57ecb-335">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-335">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="57ecb-336">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="57ecb-336">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="57ecb-337">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="57ecb-337">Default: `120`</span></span><br><span data-ttu-id="57ecb-338">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="57ecb-338">Min: `0`</span></span><br><span data-ttu-id="57ecb-339">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="57ecb-339">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="57ecb-340">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-340">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="57ecb-341">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="57ecb-341">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="57ecb-342">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-342">Optional string attribute.</span></span></p><p><span data-ttu-id="57ecb-343">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-343">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="57ecb-344">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-344">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="57ecb-345">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-345">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="57ecb-346">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-346">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="57ecb-347">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-347">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="57ecb-348">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="57ecb-348">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="57ecb-349">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="57ecb-349">Setting environment variables</span></span>

<span data-ttu-id="57ecb-350">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-350">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="57ecb-351">Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi bir `environmentVariables` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="57ecb-351">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="57ecb-352">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-352">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="57ecb-353">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="57ecb-353">The following example sets two environment variables.</span></span> <span data-ttu-id="57ecb-354">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="57ecb-354">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="57ecb-355">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="57ecb-355">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="57ecb-356">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-356">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="57ecb-357">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="57ecb-357">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="57ecb-358">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="57ecb-358">app_offline.htm</span></span>

<span data-ttu-id="57ecb-359">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-359">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="57ecb-360">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-360">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="57ecb-361">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="57ecb-361">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="57ecb-362">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-362">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="57ecb-363">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="57ecb-363">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="57ecb-364">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-364">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="57ecb-365">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="57ecb-365">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="57ecb-366">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="57ecb-366">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="57ecb-367">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-367">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="57ecb-368">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-368">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="57ecb-369">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-369">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="57ecb-370">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-370">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="57ecb-371">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="57ecb-371">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="57ecb-372">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="57ecb-372">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="57ecb-373">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57ecb-373">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="57ecb-374">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="57ecb-374">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="57ecb-376">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="57ecb-376">Log creation and redirection</span></span>

<span data-ttu-id="57ecb-377">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-377">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="57ecb-378">Herhangi bir klasörde `stdoutLogFile` günlük dosyası oluşturulduğunda, yol modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-378">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="57ecb-379">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="57ecb-379">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="57ecb-380">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="57ecb-380">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="57ecb-381">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-381">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="57ecb-382">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-382">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="57ecb-383">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-383">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="57ecb-384">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="57ecb-385">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="57ecb-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="57ecb-386">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-386">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="57ecb-387">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="57ecb-387">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="57ecb-388">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="57ecb-388">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="57ecb-389">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="57ecb-389">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="57ecb-390">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-390">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="57ecb-391">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-391">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="57ecb-392">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-392">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="57ecb-393">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-393">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="57ecb-394">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="57ecb-394">Enhanced diagnostic logs</span></span>

<span data-ttu-id="57ecb-395">ASP.NET Core modülü sağlayan gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-395">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="57ecb-396">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="57ecb-396">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="57ecb-397">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-397">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="57ecb-398">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="57ecb-398">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="57ecb-399">HATA</span><span class="sxs-lookup"><span data-stu-id="57ecb-399">ERROR</span></span>
* <span data-ttu-id="57ecb-400">UYARI</span><span class="sxs-lookup"><span data-stu-id="57ecb-400">WARNING</span></span>
* <span data-ttu-id="57ecb-401">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="57ecb-401">INFO</span></span>
* <span data-ttu-id="57ecb-402">TRACE</span><span class="sxs-lookup"><span data-stu-id="57ecb-402">TRACE</span></span>

<span data-ttu-id="57ecb-403">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="57ecb-403">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="57ecb-404">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="57ecb-404">CONSOLE</span></span>
* <span data-ttu-id="57ecb-405">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="57ecb-405">EVENTLOG</span></span>
* <span data-ttu-id="57ecb-406">DOSYA</span><span class="sxs-lookup"><span data-stu-id="57ecb-406">FILE</span></span>

<span data-ttu-id="57ecb-407">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="57ecb-407">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="57ecb-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="57ecb-408">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="57ecb-409">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="57ecb-409">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="57ecb-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="57ecb-410">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="57ecb-411">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-411">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="57ecb-412">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="57ecb-412">The size of the log isn't limited.</span></span> <span data-ttu-id="57ecb-413">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="57ecb-413">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="57ecb-414">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="57ecb-414">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="57ecb-415">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-415">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="57ecb-416">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="57ecb-416">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="57ecb-417">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-417">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="57ecb-418">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-418">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="57ecb-419">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="57ecb-419">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="57ecb-420">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57ecb-420">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="57ecb-421">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="57ecb-421">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="57ecb-422">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="57ecb-422">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="57ecb-423">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="57ecb-423">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="57ecb-424">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="57ecb-424">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="57ecb-425">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="57ecb-425">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="57ecb-426">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="57ecb-426">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="57ecb-427">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="57ecb-427">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="57ecb-428">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **sistem** hesabı.</span><span class="sxs-lookup"><span data-stu-id="57ecb-428">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="57ecb-429">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken İsabetleri *applicationHost.config* paylaşımda.</span><span class="sxs-lookup"><span data-stu-id="57ecb-429">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="57ecb-430">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="57ecb-430">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="57ecb-431">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-431">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="57ecb-432">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="57ecb-432">Run the installer.</span></span>
1. <span data-ttu-id="57ecb-433">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="57ecb-433">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="57ecb-434">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="57ecb-434">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="57ecb-435">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="57ecb-435">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="57ecb-436">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="57ecb-436">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="57ecb-437">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="57ecb-437">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="57ecb-438">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="57ecb-438">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="57ecb-439">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="57ecb-439">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="57ecb-440">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57ecb-440">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="57ecb-441">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="57ecb-441">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="57ecb-442">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="57ecb-442">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="57ecb-443">Modül</span><span class="sxs-lookup"><span data-stu-id="57ecb-443">Module</span></span>

<span data-ttu-id="57ecb-444">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="57ecb-444">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="57ecb-445">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-445">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="57ecb-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-446">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="57ecb-447">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-447">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="57ecb-448">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-448">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="57ecb-449">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="57ecb-449">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="57ecb-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-450">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="57ecb-451">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-451">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="57ecb-452">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-452">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="57ecb-453">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="57ecb-453">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="57ecb-454">Şema</span><span class="sxs-lookup"><span data-stu-id="57ecb-454">Schema</span></span>

<span data-ttu-id="57ecb-455">**IIS**</span><span class="sxs-lookup"><span data-stu-id="57ecb-455">**IIS**</span></span>

   * <span data-ttu-id="57ecb-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="57ecb-456">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="57ecb-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="57ecb-457">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="57ecb-458">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="57ecb-458">**IIS Express**</span></span>

   * <span data-ttu-id="57ecb-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="57ecb-459">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="57ecb-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="57ecb-460">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="57ecb-461">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="57ecb-461">Configuration</span></span>

<span data-ttu-id="57ecb-462">**IIS**</span><span class="sxs-lookup"><span data-stu-id="57ecb-462">**IIS**</span></span>

   * <span data-ttu-id="57ecb-463">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="57ecb-463">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="57ecb-464">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="57ecb-464">**IIS Express**</span></span>

   * <span data-ttu-id="57ecb-465">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="57ecb-465">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="57ecb-466">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="57ecb-466">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="57ecb-467">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="57ecb-467">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="57ecb-468">ASP.NET Core modülü GitHub deposu (başvuru kaynağı)</span><span class="sxs-lookup"><span data-stu-id="57ecb-468">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>