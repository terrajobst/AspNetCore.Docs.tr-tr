---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: a955cc98dc60d2f8178cb771f31a8b243f2567f3
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833598"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="336d1-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="336d1-103">ASP.NET Core Module</span></span>

<span data-ttu-id="336d1-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="336d1-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-105">ASP.NET Core modülü ya da IIS kanala takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="336d1-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="336d1-106">IIS çalışan işlemi içinde ASP.NET Core uygulaması ana bilgisayar (`w3wp.exe`) adlı [işlem içi barındırma modeli](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="336d1-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="336d1-107">Arka uç, çalışan bir ASP.NET Core uygulaması için web istekleri iletmek [Kestrel sunucu](xref:fundamentals/servers/kestrel)adlı [işlem dışı barındırma modeli](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="336d1-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="336d1-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="336d1-108">Supported Windows versions:</span></span>

* <span data-ttu-id="336d1-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="336d1-109">Windows 7 or later</span></span>
* <span data-ttu-id="336d1-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="336d1-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="336d1-111">İşlem içi barındırırken modülü bir işlem sunucusu uygulama IIS HTTP sunucusu olarak adlandırılır, IIS için kullanır. (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="336d1-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="336d1-112">İşlem dışı barındırırken, modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="336d1-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="336d1-113">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="336d1-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="336d1-114">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="336d1-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="336d1-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="336d1-115">In-process hosting model</span></span>

<span data-ttu-id="336d1-116">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreHostingModel>` özellik değerini içeren uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma ile ayarlanır `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="336d1-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="336d1-117">İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="336d1-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="336d1-118">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="336d1-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="336d1-119">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="336d1-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="336d1-120">IIS HTTP sunucusu (`IISHttpServer`) yerine kullanılan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="336d1-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="336d1-121">İşlem içinde için [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> için:</span><span class="sxs-lookup"><span data-stu-id="336d1-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="336d1-122">Kayıt `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="336d1-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="336d1-123">Sunucu, ASP.NET Core modülü çalıştırırken dinleyecek temel yolu ve bağlantı noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="336d1-124">Başlatma hataları yakalamak için ana yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="336d1-125">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="336d1-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="336d1-126">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="336d1-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="336d1-127">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="336d1-127">Use one app pool per app.</span></span>

* <span data-ttu-id="336d1-128">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="336d1-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="336d1-129">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="336d1-130">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="336d1-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="336d1-131">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="336d1-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="336d1-132">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="336d1-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="336d1-133">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="336d1-133">Client disconnects are detected.</span></span> <span data-ttu-id="336d1-134">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="336d1-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="336d1-135">ASP.NET core'da 2.2.1 veya daha önce <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine yerine IIS tarafından başlatılan işlem alt dizinini döndürür (örneğin, *C:\Windows\System32\inetsrv* için *w3wp.exe*) .</span><span class="sxs-lookup"><span data-stu-id="336d1-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="336d1-136">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="336d1-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="336d1-137">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="336d1-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="336d1-138">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="336d1-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>
  
* <span data-ttu-id="336d1-139">İşlem içinde barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="336d1-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="336d1-140">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="336d1-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="336d1-141">Talepleri ile dönüştürülürken bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulaması, çağrı <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> kimlik doğrulama hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="336d1-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }
  
  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="336d1-142">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="336d1-142">Out-of-process hosting model</span></span>

<span data-ttu-id="336d1-143">İşlem dışı barındırmak için bir uygulamayı yapılandırmak için proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="336d1-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="336d1-144">Belirtmeyin `<AspNetCoreHostingModel>` özelliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="336d1-145">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="336d1-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="336d1-146">Değerini `<AspNetCoreHostingModel>` özelliğini `OutOfProcess` (işlem içi barındırma ile ayarlanır `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="336d1-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="336d1-147">[Kestrel'i](xref:fundamentals/servers/kestrel) sunucusu yerine IIS HTTP sunucusu kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="336d1-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="336d1-148">Çıkış işlemini için [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> için:</span><span class="sxs-lookup"><span data-stu-id="336d1-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="336d1-149">Sunucu, ASP.NET Core modülü çalıştırırken dinleyecek temel yolu ve bağlantı noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="336d1-150">Başlatma hataları yakalamak için ana yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-150">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="336d1-151">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="336d1-151">Hosting model changes</span></span>

<span data-ttu-id="336d1-152">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="336d1-152">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="336d1-153">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="336d1-153">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="336d1-154">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="336d1-154">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="336d1-155">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="336d1-155">Process name</span></span>

<span data-ttu-id="336d1-156">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="336d1-156">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="336d1-157">ASP.NET Core modülü, uygulama arka ucu ASP.NET Core web isteklerini iletmek için IIS ardışık düzende takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="336d1-157">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="336d1-158">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="336d1-158">Supported Windows versions:</span></span>

* <span data-ttu-id="336d1-159">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="336d1-159">Windows 7 or later</span></span>
* <span data-ttu-id="336d1-160">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="336d1-160">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="336d1-161">Modül yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="336d1-161">The module only works with Kestrel.</span></span> <span data-ttu-id="336d1-162">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="336d1-162">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="336d1-163">Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="336d1-163">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="336d1-164">İlk istek ulaştığında ve onu bir çökme gerçekleşirse, uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar.</span><span class="sxs-lookup"><span data-stu-id="336d1-164">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="336d1-165">Bu aslında aynı işlemde çalışan ASP.NET 4.x uygulamalar ile görüldüğü gibi davranıştır tarafından yönetildiği IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="336d1-165">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="336d1-166">Aşağıdaki diyagramda, IIS, ASP.NET Core modülü ve uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="336d1-166">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="336d1-168">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="336d1-168">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="336d1-169">Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="336d1-169">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="336d1-170">Modül Kestrel rastgele bağlantı noktası için 80 veya 443 bağlantı noktası olmadığından uygulama isteklerini iletir.</span><span class="sxs-lookup"><span data-stu-id="336d1-170">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="336d1-171">Modülü başlatma sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve IIS tümleştirme ara yazılımı üzerinde dinlemek üzere yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="336d1-171">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="336d1-172">Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-172">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="336d1-173">İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="336d1-173">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="336d1-174">Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-174">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="336d1-175">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="336d1-175">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="336d1-176">IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="336d1-176">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="336d1-177">Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-177">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="336d1-178">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="336d1-178">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="336d1-179">ASP.NET Core modülü içeren etkin IIS modülleri hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="336d1-179">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="336d1-180">ASP.NET Core modülü de yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="336d1-180">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="336d1-181">Çalışan işlemi için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="336d1-181">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="336d1-182">Çıkış başlatma sorunlarını gidermek için dosya depolama için stdout'u günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="336d1-182">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="336d1-183">Windows kimlik doğrulama belirteçlerinizi iletin.</span><span class="sxs-lookup"><span data-stu-id="336d1-183">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="336d1-184">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="336d1-184">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="336d1-185">Yükleme ve ASP.NET Core modülü kullanma hakkında yönergeler için bkz. <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="336d1-185">For instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="336d1-186">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="336d1-186">Configuration with web.config</span></span>

<span data-ttu-id="336d1-187">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="336d1-187">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="336d1-188">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="336d1-188">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="336d1-189">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="336d1-189">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="336d1-190"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="336d1-190">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="336d1-191">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="336d1-191">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="336d1-192">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="336d1-192">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="336d1-193">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="336d1-193">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="336d1-194">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="336d1-194">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="336d1-195">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="336d1-195">Attribute</span></span> | <span data-ttu-id="336d1-196">Açıklama</span><span class="sxs-lookup"><span data-stu-id="336d1-196">Description</span></span> | <span data-ttu-id="336d1-197">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="336d1-197">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="336d1-198">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-198">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-199">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="336d1-199">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="336d1-200">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-200">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-201">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-201">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="336d1-202">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-202">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-203">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-203">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="336d1-204">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="336d1-204">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="336d1-205">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-205">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-206">Barındırma modeli işlemdeki belirtir (`InProcess`) veya işlem dışı (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="336d1-206">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="336d1-207">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-207">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-208">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="336d1-208">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="336d1-209">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="336d1-209">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="336d1-210">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-210">Default: `1`</span></span><br><span data-ttu-id="336d1-211">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-211">Min: `1`</span></span><br><span data-ttu-id="336d1-212">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="336d1-212">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="336d1-213">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-213">Required string attribute.</span></span></p><p><span data-ttu-id="336d1-214">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-214">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="336d1-215">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="336d1-215">Relative paths are supported.</span></span> <span data-ttu-id="336d1-216">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-216">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="336d1-217">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-217">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-218">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="336d1-218">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="336d1-219">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="336d1-219">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="336d1-220">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="336d1-220">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="336d1-221">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-221">Default: `10`</span></span><br><span data-ttu-id="336d1-222">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-222">Min: `0`</span></span><br><span data-ttu-id="336d1-223">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="336d1-223">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="336d1-224">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-224">Optional timespan attribute.</span></span></p><p><span data-ttu-id="336d1-225">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="336d1-225">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="336d1-226">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-226">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="336d1-227">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="336d1-227">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="336d1-228">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="336d1-228">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="336d1-229">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-229">Default: `00:02:00`</span></span><br><span data-ttu-id="336d1-230">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-230">Min: `00:00:00`</span></span><br><span data-ttu-id="336d1-231">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-231">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="336d1-232">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-232">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-233">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="336d1-233">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="336d1-234">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-234">Default: `10`</span></span><br><span data-ttu-id="336d1-235">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-235">Min: `0`</span></span><br><span data-ttu-id="336d1-236">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="336d1-236">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="336d1-237">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-237">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-238">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="336d1-238">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="336d1-239">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="336d1-239">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="336d1-240">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="336d1-240">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="336d1-241">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="336d1-241">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="336d1-242">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="336d1-242">Default: `120`</span></span><br><span data-ttu-id="336d1-243">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-243">Min: `0`</span></span><br><span data-ttu-id="336d1-244">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="336d1-244">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="336d1-245">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-245">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-246">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="336d1-246">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="336d1-247">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-247">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-248">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-248">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="336d1-249">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="336d1-249">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="336d1-250">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-250">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="336d1-251">Günlük dosyası oluşturulduğunda yolunda sağlanan herhangi bir klasörde modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="336d1-251">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="336d1-252">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-252">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="336d1-253">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="336d1-253">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="336d1-254">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="336d1-254">Attribute</span></span> | <span data-ttu-id="336d1-255">Açıklama</span><span class="sxs-lookup"><span data-stu-id="336d1-255">Description</span></span> | <span data-ttu-id="336d1-256">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="336d1-256">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="336d1-257">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-257">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-258">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="336d1-258">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="336d1-259">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-259">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-260">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-260">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="336d1-261">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-262">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-262">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="336d1-263">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="336d1-263">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="336d1-264">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-264">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-265">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="336d1-265">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="336d1-266">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-266">Default: `1`</span></span><br><span data-ttu-id="336d1-267">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-267">Min: `1`</span></span><br><span data-ttu-id="336d1-268">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="336d1-268">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="336d1-269">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-269">Required string attribute.</span></span></p><p><span data-ttu-id="336d1-270">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-270">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="336d1-271">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="336d1-271">Relative paths are supported.</span></span> <span data-ttu-id="336d1-272">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-272">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="336d1-273">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-273">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-274">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="336d1-274">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="336d1-275">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="336d1-275">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="336d1-276">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-276">Default: `10`</span></span><br><span data-ttu-id="336d1-277">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-277">Min: `0`</span></span><br><span data-ttu-id="336d1-278">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="336d1-278">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="336d1-279">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-279">Optional timespan attribute.</span></span></p><p><span data-ttu-id="336d1-280">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="336d1-280">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="336d1-281">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-281">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="336d1-282">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-282">Default: `00:02:00`</span></span><br><span data-ttu-id="336d1-283">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-283">Min: `00:00:00`</span></span><br><span data-ttu-id="336d1-284">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-284">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="336d1-285">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-285">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-286">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="336d1-286">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="336d1-287">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-287">Default: `10`</span></span><br><span data-ttu-id="336d1-288">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-288">Min: `0`</span></span><br><span data-ttu-id="336d1-289">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="336d1-289">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="336d1-290">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-290">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-291">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="336d1-291">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="336d1-292">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="336d1-292">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="336d1-293">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="336d1-293">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="336d1-294">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="336d1-294">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="336d1-295">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="336d1-295">Default: `120`</span></span><br><span data-ttu-id="336d1-296">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-296">Min: `0`</span></span><br><span data-ttu-id="336d1-297">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="336d1-297">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="336d1-298">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-298">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-299">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="336d1-299">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="336d1-300">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-300">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-301">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-301">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="336d1-302">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="336d1-302">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="336d1-303">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-303">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="336d1-304">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="336d1-304">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="336d1-305">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-305">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="336d1-306">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="336d1-306">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="336d1-307">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="336d1-307">Attribute</span></span> | <span data-ttu-id="336d1-308">Açıklama</span><span class="sxs-lookup"><span data-stu-id="336d1-308">Description</span></span> | <span data-ttu-id="336d1-309">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="336d1-309">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="336d1-310">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-310">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-311">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="336d1-311">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="336d1-312">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-312">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-313">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-313">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="336d1-314">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-314">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-315">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-315">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="336d1-316">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="336d1-316">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="336d1-317">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-317">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-318">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="336d1-318">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="336d1-319">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-319">Default: `1`</span></span><br><span data-ttu-id="336d1-320">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="336d1-320">Min: `1`</span></span><br><span data-ttu-id="336d1-321">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="336d1-321">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="336d1-322">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-322">Required string attribute.</span></span></p><p><span data-ttu-id="336d1-323">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-323">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="336d1-324">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="336d1-324">Relative paths are supported.</span></span> <span data-ttu-id="336d1-325">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-325">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="336d1-326">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-326">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-327">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="336d1-327">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="336d1-328">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="336d1-328">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="336d1-329">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-329">Default: `10`</span></span><br><span data-ttu-id="336d1-330">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-330">Min: `0`</span></span><br><span data-ttu-id="336d1-331">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="336d1-331">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="336d1-332">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-332">Optional timespan attribute.</span></span></p><p><span data-ttu-id="336d1-333">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="336d1-333">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="336d1-334">ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-334">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="336d1-335">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-335">Default: `00:02:00`</span></span><br><span data-ttu-id="336d1-336">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-336">Min: `00:00:00`</span></span><br><span data-ttu-id="336d1-337">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="336d1-337">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="336d1-338">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-338">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-339">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="336d1-339">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="336d1-340">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="336d1-340">Default: `10`</span></span><br><span data-ttu-id="336d1-341">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-341">Min: `0`</span></span><br><span data-ttu-id="336d1-342">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="336d1-342">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="336d1-343">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-343">Optional integer attribute.</span></span></p><p><span data-ttu-id="336d1-344">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="336d1-344">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="336d1-345">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="336d1-345">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="336d1-346">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="336d1-346">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="336d1-347">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="336d1-347">Default: `120`</span></span><br><span data-ttu-id="336d1-348">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="336d1-348">Min: `0`</span></span><br><span data-ttu-id="336d1-349">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="336d1-349">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="336d1-350">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-350">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="336d1-351">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="336d1-351">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="336d1-352">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-352">Optional string attribute.</span></span></p><p><span data-ttu-id="336d1-353">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-353">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="336d1-354">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="336d1-354">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="336d1-355">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-355">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="336d1-356">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="336d1-356">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="336d1-357">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-357">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="336d1-358">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="336d1-358">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="336d1-359">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="336d1-359">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="336d1-360">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-360">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="336d1-361">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="336d1-361">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="336d1-362">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-362">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="336d1-363">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-363">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="336d1-364">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="336d1-364">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="336d1-365">Aynı ada sahip bu bölümde çakışma sistem ortam değişkenlerini ayarlamak ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="336d1-365">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="336d1-366">Hem de bir ortam değişkenine ayarlanmışsa *web.config* dosya ve sistem düzeyi, Windows, değerinden *web.config* dosya sistemi ortam değişken değerini (eklenmiş olur Örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`), önleyen uygulamanın başlatılmasını.</span><span class="sxs-lookup"><span data-stu-id="336d1-366">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="336d1-367">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="336d1-367">The following example sets two environment variables.</span></span> <span data-ttu-id="336d1-368">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="336d1-368">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="336d1-369">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="336d1-369">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="336d1-370">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-370">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="336d1-371">Doğrudan bir ortam ayarını bir alternatif *web.config* eklemektir `<EnvironmentName>` yayımlama profilini özelliğinde (*.pubxml*) ya da proje dosyası.</span><span class="sxs-lookup"><span data-stu-id="336d1-371">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="336d1-372">Bu yaklaşım ortamı ayarlar *web.config* proje yayımlandığında ne zaman:</span><span class="sxs-lookup"><span data-stu-id="336d1-372">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="336d1-373">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="336d1-373">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="336d1-374">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="336d1-374">app_offline.htm</span></span>

<span data-ttu-id="336d1-375">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="336d1-375">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="336d1-376">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="336d1-376">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="336d1-377">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="336d1-377">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="336d1-378">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="336d1-378">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-379">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="336d1-379">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="336d1-380">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-380">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="336d1-381">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="336d1-381">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-382">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="336d1-382">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="336d1-383">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="336d1-383">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="336d1-384">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="336d1-384">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="336d1-385">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="336d1-385">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="336d1-386">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-386">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="336d1-387">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="336d1-387">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="336d1-388">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="336d1-388">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="336d1-389">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="336d1-389">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="336d1-390">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="336d1-390">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="336d1-392">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="336d1-392">Log creation and redirection</span></span>

<span data-ttu-id="336d1-393">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="336d1-393">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="336d1-394">Herhangi bir klasörde `stdoutLogFile` günlük dosyası oluşturulduğunda, yol modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="336d1-394">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="336d1-395">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="336d1-395">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="336d1-396">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="336d1-396">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="336d1-397">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="336d1-397">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="336d1-398">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-398">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="336d1-399">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="336d1-399">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="336d1-400">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="336d1-400">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="336d1-401">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="336d1-401">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="336d1-402">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="336d1-402">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="336d1-403">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="336d1-403">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="336d1-404">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="336d1-404">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-405">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="336d1-405">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="336d1-406">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="336d1-406">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="336d1-407">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="336d1-407">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="336d1-408">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-408">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="336d1-409">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="336d1-409">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="336d1-410">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="336d1-410">Enhanced diagnostic logs</span></span>

<span data-ttu-id="336d1-411">ASP.NET Core modülü Gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-411">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="336d1-412">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="336d1-412">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="336d1-413">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="336d1-413">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="336d1-414">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="336d1-414">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="336d1-415">HATA</span><span class="sxs-lookup"><span data-stu-id="336d1-415">ERROR</span></span>
* <span data-ttu-id="336d1-416">UYARI</span><span class="sxs-lookup"><span data-stu-id="336d1-416">WARNING</span></span>
* <span data-ttu-id="336d1-417">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="336d1-417">INFO</span></span>
* <span data-ttu-id="336d1-418">TRACE</span><span class="sxs-lookup"><span data-stu-id="336d1-418">TRACE</span></span>

<span data-ttu-id="336d1-419">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="336d1-419">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="336d1-420">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="336d1-420">CONSOLE</span></span>
* <span data-ttu-id="336d1-421">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="336d1-421">EVENTLOG</span></span>
* <span data-ttu-id="336d1-422">DOSYA</span><span class="sxs-lookup"><span data-stu-id="336d1-422">FILE</span></span>

<span data-ttu-id="336d1-423">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="336d1-423">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="336d1-424">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="336d1-424">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="336d1-425">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="336d1-425">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="336d1-426">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="336d1-426">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="336d1-427">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="336d1-427">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="336d1-428">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="336d1-428">The size of the log isn't limited.</span></span> <span data-ttu-id="336d1-429">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="336d1-429">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="336d1-430">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="336d1-430">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="336d1-431">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="336d1-431">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-432">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="336d1-432">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="336d1-433">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="336d1-433">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="336d1-434">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="336d1-434">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="336d1-435">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="336d1-435">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="336d1-436">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="336d1-436">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="336d1-437">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="336d1-437">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="336d1-438">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="336d1-438">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="336d1-439">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="336d1-439">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="336d1-440">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="336d1-440">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="336d1-441">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="336d1-441">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="336d1-442">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="336d1-442">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="336d1-443">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="336d1-443">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="336d1-444">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **TrustedInstaller** hesabı.</span><span class="sxs-lookup"><span data-stu-id="336d1-444">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="336d1-445">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken oluşturur *applicationHost.config*  Dosya paylaşımındaki.</span><span class="sxs-lookup"><span data-stu-id="336d1-445">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="336d1-446">IIS paylaşılan yapılandırmaya, IIS yüklemesi ile aynı makinede kullanırken, ASP.NET Core barındırma Paket Yükleyici ile çalıştırma `OPT_NO_SHARED_CONFIG_CHECK` parametresini `1`:</span><span class="sxs-lookup"><span data-stu-id="336d1-446">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="336d1-447">Paylaşılan yapılandırma yolu IIS yüklemesi ile aynı makinede olmadığı durumlarda, aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="336d1-447">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="336d1-448">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="336d1-448">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="336d1-449">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-449">Run the installer.</span></span>
1. <span data-ttu-id="336d1-450">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="336d1-450">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="336d1-451">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="336d1-451">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="336d1-452">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="336d1-452">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="336d1-453">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="336d1-453">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="336d1-454">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="336d1-454">Run the installer.</span></span>
1. <span data-ttu-id="336d1-455">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="336d1-455">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="336d1-456">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="336d1-456">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="application-initialization"></a><span data-ttu-id="336d1-457">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="336d1-457">Application Initialization</span></span>

<span data-ttu-id="336d1-458">[IIS uygulama başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) uygulama havuzunu başlatır veya geri dönüştürüldüğünde, uygulamaya bir HTTP isteği gönderir, bir IIS özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="336d1-458">[IIS Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) is an IIS feature that sends an HTTP request to the app when the app pool starts or is recycled.</span></span> <span data-ttu-id="336d1-459">İstek, uygulamanın başlatılması tetikler.</span><span class="sxs-lookup"><span data-stu-id="336d1-459">The request triggers the app to start.</span></span> <span data-ttu-id="336d1-460">Uygulama başlatma her ikisi için de kullanılabilir [işlem içi barındırma modeli](xref:fundamentals/servers/index#in-process-hosting-model) ve [işlem dışı barındırma modeli](xref:fundamentals/servers/index#out-of-process-hosting-model) ile ASP.NET Core modülü sürüm 2.</span><span class="sxs-lookup"><span data-stu-id="336d1-460">Application Initialization can be used by both the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model) and [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) with the ASP.NET Core Module version 2.</span></span>

<span data-ttu-id="336d1-461">Uygulama başlatma etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="336d1-461">To enable Application Initialization:</span></span>

1. <span data-ttu-id="336d1-462">IIS uygulama başlatma rolü özelliği etkin olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="336d1-462">Confirm that the IIS Application Initialization role feature in enabled:</span></span>
   * <span data-ttu-id="336d1-463">Windows 7 veya sonraki sürümlerde: Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="336d1-463">On Windows 7 or later: Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="336d1-464">Açık **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="336d1-464">Open **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="336d1-465">Onay kutusunu seçin **uygulama başlatma**.</span><span class="sxs-lookup"><span data-stu-id="336d1-465">Select the check box for **Application Initialization**.</span></span>
   * <span data-ttu-id="336d1-466">Windows Server 2008 R2 veya sonraki sürümlerde, açık **Ekle roller ve Özellikler Sihirbazı**.</span><span class="sxs-lookup"><span data-stu-id="336d1-466">On Windows Server 2008 R2 or later, open the **Add Roles and Features Wizard**.</span></span> <span data-ttu-id="336d1-467">Ulaştığınızda **rol hizmetlerini seçin** paneli, açık **uygulama geliştirme** düğümünü seçip alt **uygulama başlatma** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="336d1-467">When you reach the **Select role services** panel, open the **Application Development** node and select the **Application Initialization** check box.</span></span>
1. <span data-ttu-id="336d1-468">IIS Yöneticisi'nde **uygulama havuzları** içinde **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="336d1-468">In IIS Manager, select **Application Pools** in the **Connections** panel.</span></span>
1. <span data-ttu-id="336d1-469">Listede uygulamanın uygulama havuzunu seçin.</span><span class="sxs-lookup"><span data-stu-id="336d1-469">Select the app's app pool in the list.</span></span>
1. <span data-ttu-id="336d1-470">Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="336d1-470">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="336d1-471">Ayarlama **başlangıç modu** için **AlwaysRunning**.</span><span class="sxs-lookup"><span data-stu-id="336d1-471">Set **Start Mode** to **AlwaysRunning**.</span></span>
1. <span data-ttu-id="336d1-472">Açık **siteleri** düğümünde **bağlantıları** paneli.</span><span class="sxs-lookup"><span data-stu-id="336d1-472">Open the **Sites** node in the **Connections** panel.</span></span>
1. <span data-ttu-id="336d1-473">Uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="336d1-473">Select the app.</span></span>
1. <span data-ttu-id="336d1-474">Seçin **Gelişmiş ayarlar** altında **Web sitesini Yönet** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="336d1-474">Select **Advanced Settings** under **Manage Website** in the **Actions** panel.</span></span>
1. <span data-ttu-id="336d1-475">Ayarlama **önceden yükleme etkin** için **True**.</span><span class="sxs-lookup"><span data-stu-id="336d1-475">Set **Preload Enabled** to **True**.</span></span>

<span data-ttu-id="336d1-476">Daha fazla bilgi için [IIS 8.0 uygulama başlatma](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span><span class="sxs-lookup"><span data-stu-id="336d1-476">For more information, see [IIS 8.0 Application Initialization](/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization).</span></span>

<span data-ttu-id="336d1-477">Kullanan uygulamalar [işlem dışı barındırma modeli](xref:fundamentals/servers/index#out-of-process-hosting-model) uygulamanın çalışmasını sağlamak için düzenli aralıklarla ping göndermek için bir dış hizmet kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="336d1-477">Apps that use the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model) must use an external service to periodically ping the app in order to keep it running.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="336d1-478">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="336d1-478">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="336d1-479">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="336d1-479">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="336d1-480">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="336d1-480">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="336d1-481">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="336d1-481">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="336d1-482">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="336d1-482">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="336d1-483">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="336d1-483">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="336d1-484">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="336d1-484">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="336d1-485">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="336d1-485">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="336d1-486">Modül</span><span class="sxs-lookup"><span data-stu-id="336d1-486">Module</span></span>

<span data-ttu-id="336d1-487">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="336d1-487">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="336d1-488">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-488">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="336d1-489">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-489">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="336d1-490">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-490">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="336d1-491">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-491">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="336d1-492">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="336d1-492">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="336d1-493">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-493">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="336d1-494">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-494">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="336d1-495">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-495">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="336d1-496">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="336d1-496">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="336d1-497">Şema</span><span class="sxs-lookup"><span data-stu-id="336d1-497">Schema</span></span>

<span data-ttu-id="336d1-498">**IIS**</span><span class="sxs-lookup"><span data-stu-id="336d1-498">**IIS**</span></span>

   * <span data-ttu-id="336d1-499">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="336d1-499">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="336d1-500">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="336d1-500">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="336d1-501">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="336d1-501">**IIS Express**</span></span>

   * <span data-ttu-id="336d1-502">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="336d1-502">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="336d1-503">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="336d1-503">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="336d1-504">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="336d1-504">Configuration</span></span>

<span data-ttu-id="336d1-505">**IIS**</span><span class="sxs-lookup"><span data-stu-id="336d1-505">**IIS**</span></span>

   * <span data-ttu-id="336d1-506">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="336d1-506">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="336d1-507">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="336d1-507">**IIS Express**</span></span>

   * <span data-ttu-id="336d1-508">Visual Studio: {Uygulama KÖKÜ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="336d1-508">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>
   
   * <span data-ttu-id="336d1-509">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="336d1-509">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="336d1-510">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="336d1-510">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="336d1-511">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="336d1-511">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="336d1-512">ASP.NET Core modülü GitHub deposu (başvuru kaynağı)</span><span class="sxs-lookup"><span data-stu-id="336d1-512">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
