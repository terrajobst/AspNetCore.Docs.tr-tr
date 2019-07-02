---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 4a360023cc7fab2f066d490f7f368fc35815703a
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500455"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="a2834-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="a2834-103">ASP.NET Core Module</span></span>

<span data-ttu-id="a2834-104">Tarafından [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [ Justin Kotalik](https://github.com/jkotalik), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a2834-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-105">ASP.NET Core modülü ya da IIS kanala takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="a2834-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="a2834-106">IIS çalışan işlemi içinde ASP.NET Core uygulaması ana bilgisayar (`w3wp.exe`) adlı [işlem içi barındırma modeli](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a2834-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="a2834-107">Arka uç, çalışan bir ASP.NET Core uygulaması için web istekleri iletmek [Kestrel sunucu](xref:fundamentals/servers/kestrel)adlı [işlem dışı barındırma modeli](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="a2834-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="a2834-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="a2834-108">Supported Windows versions:</span></span>

* <span data-ttu-id="a2834-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a2834-109">Windows 7 or later</span></span>
* <span data-ttu-id="a2834-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a2834-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a2834-111">İşlem içi barındırırken modülü bir işlem sunucusu uygulama IIS HTTP sunucusu olarak adlandırılır, IIS için kullanır. (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a2834-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a2834-112">İşlem dışı barındırırken, modülü yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2834-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="a2834-113">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a2834-113">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="a2834-114">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="a2834-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="a2834-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="a2834-115">In-process hosting model</span></span>

<span data-ttu-id="a2834-116">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreHostingModel>` özellik değerini içeren uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma ile ayarlanır `OutOfProcess`):</span><span class="sxs-lookup"><span data-stu-id="a2834-116">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="a2834-117">İşlem içi barındırma modeli, .NET Framework'ü hedefleyen ASP.NET Core uygulamaları için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-117">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="a2834-118">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="a2834-118">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="a2834-119">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="a2834-119">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="a2834-120">IIS HTTP sunucusu (`IISHttpServer`) yerine kullanılan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="a2834-120">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="a2834-121">İşlem içinde için [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> için:</span><span class="sxs-lookup"><span data-stu-id="a2834-121">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="a2834-122">Kayıt `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="a2834-122">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="a2834-123">Sunucu, ASP.NET Core modülü çalıştırırken dinleyecek temel yolu ve bağlantı noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-123">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="a2834-124">Başlatma hataları yakalamak için ana yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-124">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="a2834-125">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="a2834-125">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="a2834-126">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="a2834-126">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="a2834-127">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2834-127">Use one app pool per app.</span></span>

* <span data-ttu-id="a2834-128">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="a2834-128">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a2834-129">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-129">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="a2834-130">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2834-130">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="a2834-131">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="a2834-131">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="a2834-132">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a2834-132">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="a2834-133">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="a2834-133">Client disconnects are detected.</span></span> <span data-ttu-id="a2834-134">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="a2834-134">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="a2834-135">ASP.NET core'da 2.2.1 veya daha önce <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine yerine IIS tarafından başlatılan işlem alt dizinini döndürür (örneğin, *C:\Windows\System32\inetsrv* için *w3wp.exe*) .</span><span class="sxs-lookup"><span data-stu-id="a2834-135">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="a2834-136">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="a2834-136">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="a2834-137">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a2834-137">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="a2834-138">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a2834-138">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="a2834-139">İşlem içinde barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="a2834-139">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a2834-140">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="a2834-140">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a2834-141">Talepleri ile dönüştürülürken bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulaması, çağrı <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> kimlik doğrulama hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="a2834-141">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="a2834-142">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="a2834-142">Out-of-process hosting model</span></span>

<span data-ttu-id="a2834-143">İşlem dışı barındırmak için bir uygulamayı yapılandırmak için proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a2834-143">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="a2834-144">Belirtmeyin `<AspNetCoreHostingModel>` özelliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-144">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="a2834-145">Varsa `<AspNetCoreHostingModel>` özelliği dosyasında mevcut değilse, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="a2834-145">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="a2834-146">Değerini `<AspNetCoreHostingModel>` özelliğini `OutOfProcess` (işlem içi barındırma ile ayarlanır `InProcess`):</span><span class="sxs-lookup"><span data-stu-id="a2834-146">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="a2834-147">[Kestrel'i](xref:fundamentals/servers/kestrel) sunucusu yerine IIS HTTP sunucusu kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="a2834-147">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="a2834-148">Çıkış işlemini için [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) çağrıları <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> için:</span><span class="sxs-lookup"><span data-stu-id="a2834-148">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="a2834-149">Sunucu, ASP.NET Core modülü çalıştırırken dinleyecek temel yolu ve bağlantı noktası yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-149">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="a2834-150">Başlatma hataları yakalamak için ana yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-150">Configure the host to capture startup errors.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2834-151">Çıkış işlemini barındırırken <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak bir kullanıcı başlatmak için çağırılır değil.</span><span class="sxs-lookup"><span data-stu-id="a2834-151">When hosting out-of-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a2834-152">Bu nedenle, bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> her kimlik doğrulaması varsayılan olarak etkinleştirilmez sonra talepleri dönüştürmek için kullanılan uygulama.</span><span class="sxs-lookup"><span data-stu-id="a2834-152">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a2834-153">Talepleri ile dönüştürülürken bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulaması, çağrı <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> kimlik doğrulama hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="a2834-153">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
    services.AddAuthentication(IISDefaults.AuthenticationScheme);
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="hosting-model-changes"></a><span data-ttu-id="a2834-154">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="a2834-154">Hosting model changes</span></span>

<span data-ttu-id="a2834-155">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="a2834-155">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="a2834-156">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="a2834-156">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="a2834-157">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="a2834-157">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="a2834-158">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="a2834-158">Process name</span></span>

<span data-ttu-id="a2834-159">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="a2834-159">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a2834-160">ASP.NET Core modülü, uygulama arka ucu ASP.NET Core web isteklerini iletmek için IIS ardışık düzende takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="a2834-160">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="a2834-161">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="a2834-161">Supported Windows versions:</span></span>

* <span data-ttu-id="a2834-162">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a2834-162">Windows 7 or later</span></span>
* <span data-ttu-id="a2834-163">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="a2834-163">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a2834-164">Modül yalnızca Kestrel ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2834-164">The module only works with Kestrel.</span></span> <span data-ttu-id="a2834-165">Uyumlu bir modüldür [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a2834-165">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="a2834-166">Bir işlem içinde çalıştırmak, ASP.NET Core uygulamaları IIS çalışan işleminden ayrı olduğundan, modül işlem yönetimi da işler.</span><span class="sxs-lookup"><span data-stu-id="a2834-166">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="a2834-167">İlk istek ulaştığında ve onu bir çökme gerçekleşirse, uygulama yeniden başlatmalarını modülü ASP.NET Core uygulaması için bir işlem başlar.</span><span class="sxs-lookup"><span data-stu-id="a2834-167">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="a2834-168">Bu aslında aynı işlemde çalışan ASP.NET 4.x uygulamalar ile görüldüğü gibi davranıştır tarafından yönetildiği IIS'de [Windows İşlem Etkinleştirme Hizmeti (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a2834-168">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a2834-169">Aşağıdaki diyagramda, IIS, ASP.NET Core modülü ve uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a2834-169">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="a2834-171">İstekleri için çekirdek modu HTTP.sys sürücüsünü Web'den ulaşır.</span><span class="sxs-lookup"><span data-stu-id="a2834-171">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a2834-172">Sürücü istekler IIS Web sitesinin yapılandırılan bağlantı noktası, genellikle 80 (HTTP) veya 443 (HTTPS) üzerinde yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="a2834-172">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a2834-173">Modül Kestrel rastgele bağlantı noktası için 80 veya 443 bağlantı noktası olmadığından uygulama isteklerini iletir.</span><span class="sxs-lookup"><span data-stu-id="a2834-173">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="a2834-174">Modül, başlangıçta bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) dinleyecek şekilde yapılandırır `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="a2834-174">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a2834-175">Ek denetimler gerçekleştirilir ve modülünden değilsiniz kaynaklı istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-175">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a2834-176">İstekler HTTP üzerinden HTTPS üzerinden IIS tarafından alınan bile iletilir modülü HTTPS iletmeyi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a2834-176">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a2834-177">Modül istekten Kestrel seçer sonra ASP.NET Core ara yazılım ardışık düzende isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-177">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a2834-178">Ara yazılım ardışık düzenini isteği işler ve olarak geçirir bir `HttpContext` örneği uygulama mantığına.</span><span class="sxs-lookup"><span data-stu-id="a2834-178">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a2834-179">IIS tümleştirme tarafından eklenen bir ara yazılım istek için Kestrel iletmek için hesap için şema, uzak IP ve pathbase güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="a2834-179">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="a2834-180">Uygulamanın yanıt IIS, yeniden istek başlatılan HTTP istemcisi için hangi bildirim geçirilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-180">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="a2834-181">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="a2834-181">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a2834-182">ASP.NET Core modülü içeren etkin IIS modülleri hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="a2834-182">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="a2834-183">ASP.NET Core modülü de yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a2834-183">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="a2834-184">Çalışan işlemi için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a2834-184">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a2834-185">Çıkış başlatma sorunlarını gidermek için dosya depolama için stdout'u günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a2834-185">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a2834-186">Windows kimlik doğrulama belirteçlerinizi iletin.</span><span class="sxs-lookup"><span data-stu-id="a2834-186">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a2834-187">Yükleme ve ASP.NET Core modülü kullanın</span><span class="sxs-lookup"><span data-stu-id="a2834-187">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a2834-188">ASP.NET Core modülü yüklemek yönergeler için bkz: [.NET Core barındırma paketini yüklemeniz](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="a2834-188">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="a2834-189">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2834-189">Configuration with web.config</span></span>

<span data-ttu-id="a2834-190">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2834-190">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="a2834-191">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a2834-191">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="a2834-192">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="a2834-192">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="a2834-193"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="a2834-193">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="a2834-194">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a2834-194">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="a2834-195">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="a2834-195">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="a2834-196">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="a2834-196">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="a2834-197">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="a2834-197">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="a2834-198">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="a2834-198">Attribute</span></span> | <span data-ttu-id="a2834-199">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a2834-199">Description</span></span> | <span data-ttu-id="a2834-200">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a2834-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a2834-201">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-201">Optional string attribute.</span></span></p><p><span data-ttu-id="a2834-202">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a2834-202">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="a2834-203">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-204">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a2834-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="a2834-205">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-206">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a2834-207">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="a2834-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="a2834-208">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-208">Optional string attribute.</span></span></p><p><span data-ttu-id="a2834-209">Barındırma modeli işlemdeki belirtir (`InProcess`) veya işlem dışı (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="a2834-209">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="a2834-210">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-211">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="a2834-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="a2834-212">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="a2834-212">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="a2834-213">Ayar `processesPerApplication` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-213">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="a2834-214">Bu öznitelik, gelecekteki bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="a2834-214">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="a2834-215">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="a2834-215">Default: `1`</span></span><br><span data-ttu-id="a2834-216">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="a2834-216">Min: `1`</span></span><br><span data-ttu-id="a2834-217">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="a2834-217">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="a2834-218">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-218">Required string attribute.</span></span></p><p><span data-ttu-id="a2834-219">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="a2834-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a2834-220">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a2834-220">Relative paths are supported.</span></span> <span data-ttu-id="a2834-221">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a2834-222">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-223">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a2834-224">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="a2834-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="a2834-225">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-225">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="a2834-226">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="a2834-226">Default: `10`</span></span><br><span data-ttu-id="a2834-227">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-227">Min: `0`</span></span><br><span data-ttu-id="a2834-228">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="a2834-228">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="a2834-229">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-229">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a2834-230">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2834-230">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a2834-231">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-231">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="a2834-232">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="a2834-232">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="a2834-233">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="a2834-233">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="a2834-234">Dizenin dakika ve saniye parçaları için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="a2834-234">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="a2834-235">Kullanım **60** sonuçlarında, dakika ve saniye değeri olarak bir *500 - İç sunucu hatası*.</span><span class="sxs-lookup"><span data-stu-id="a2834-235">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="a2834-236">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-236">Default: `00:02:00`</span></span><br><span data-ttu-id="a2834-237">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-237">Min: `00:00:00`</span></span><br><span data-ttu-id="a2834-238">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-238">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="a2834-239">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-240">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="a2834-240">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="a2834-241">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="a2834-241">Default: `10`</span></span><br><span data-ttu-id="a2834-242">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-242">Min: `0`</span></span><br><span data-ttu-id="a2834-243">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="a2834-243">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="a2834-244">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-244">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-245">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="a2834-245">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a2834-246">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a2834-246">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a2834-247">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="a2834-247">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="a2834-248">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="a2834-248">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="a2834-249">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="a2834-249">Default: `120`</span></span><br><span data-ttu-id="a2834-250">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-250">Min: `0`</span></span><br><span data-ttu-id="a2834-251">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="a2834-251">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="a2834-252">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-252">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-253">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a2834-253">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a2834-254">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-254">Optional string attribute.</span></span></p><p><span data-ttu-id="a2834-255">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-255">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a2834-256">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="a2834-256">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a2834-257">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-257">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a2834-258">Günlük dosyası oluşturulduğunda yolunda sağlanan herhangi bir klasörde modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2834-258">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="a2834-259">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="a2834-259">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a2834-260">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="a2834-260">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="a2834-261">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="a2834-261">Attribute</span></span> | <span data-ttu-id="a2834-262">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a2834-262">Description</span></span> | <span data-ttu-id="a2834-263">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a2834-263">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a2834-264">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-264">Optional string attribute.</span></span></p><p><span data-ttu-id="a2834-265">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a2834-265">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="a2834-266">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-266">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-267">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a2834-267">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="a2834-268">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-268">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-269">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-269">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a2834-270">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="a2834-270">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="a2834-271">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-271">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-272">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="a2834-272">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="a2834-273">Ayar `processesPerApplication` önerilmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-273">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="a2834-274">Bu öznitelik, gelecekteki bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="a2834-274">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="a2834-275">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="a2834-275">Default: `1`</span></span><br><span data-ttu-id="a2834-276">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="a2834-276">Min: `1`</span></span><br><span data-ttu-id="a2834-277">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="a2834-277">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="a2834-278">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-278">Required string attribute.</span></span></p><p><span data-ttu-id="a2834-279">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="a2834-279">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a2834-280">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a2834-280">Relative paths are supported.</span></span> <span data-ttu-id="a2834-281">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-281">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a2834-282">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-283">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="a2834-283">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a2834-284">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="a2834-284">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="a2834-285">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="a2834-285">Default: `10`</span></span><br><span data-ttu-id="a2834-286">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-286">Min: `0`</span></span><br><span data-ttu-id="a2834-287">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="a2834-287">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="a2834-288">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-288">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a2834-289">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="a2834-289">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a2834-290">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-290">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="a2834-291">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-291">Default: `00:02:00`</span></span><br><span data-ttu-id="a2834-292">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-292">Min: `00:00:00`</span></span><br><span data-ttu-id="a2834-293">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="a2834-293">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="a2834-294">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-294">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-295">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="a2834-295">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="a2834-296">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="a2834-296">Default: `10`</span></span><br><span data-ttu-id="a2834-297">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-297">Min: `0`</span></span><br><span data-ttu-id="a2834-298">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="a2834-298">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="a2834-299">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-299">Optional integer attribute.</span></span></p><p><span data-ttu-id="a2834-300">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="a2834-300">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a2834-301">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a2834-301">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a2834-302">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="a2834-302">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="a2834-303">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="a2834-303">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="a2834-304">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="a2834-304">Default: `120`</span></span><br><span data-ttu-id="a2834-305">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="a2834-305">Min: `0`</span></span><br><span data-ttu-id="a2834-306">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="a2834-306">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="a2834-307">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-307">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a2834-308">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a2834-308">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a2834-309">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-309">Optional string attribute.</span></span></p><p><span data-ttu-id="a2834-310">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-310">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a2834-311">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="a2834-311">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a2834-312">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-312">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a2834-313">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a2834-313">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="a2834-314">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="a2834-314">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a2834-315">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="a2834-315">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="a2834-316">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="a2834-316">Setting environment variables</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2834-317">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-317">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a2834-318">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="a2834-318">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="a2834-319">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a2834-319">Environment variables set in this section take precedence over system environment variables.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a2834-320">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-320">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a2834-321">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="a2834-321">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="a2834-322">Aynı ada sahip bu bölümde çakışma sistem ortam değişkenlerini ayarlamak ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a2834-322">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="a2834-323">Hem de bir ortam değişkenine ayarlanmışsa *web.config* dosya ve sistem düzeyi, Windows, değerinden *web.config* dosya sistemi ortam değişken değerini (eklenmiş olur Örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`), önleyen uygulamanın başlatılmasını.</span><span class="sxs-lookup"><span data-stu-id="a2834-323">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

::: moniker-end

<span data-ttu-id="a2834-324">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a2834-324">The following example sets two environment variables.</span></span> <span data-ttu-id="a2834-325">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="a2834-325">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="a2834-326">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="a2834-326">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="a2834-327">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="a2834-327">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="a2834-328">Doğrudan bir ortam ayarını bir alternatif *web.config* eklemektir `<EnvironmentName>` yayımlama profilini özelliğinde ( *.pubxml*) ya da proje dosyası.</span><span class="sxs-lookup"><span data-stu-id="a2834-328">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="a2834-329">Bu yaklaşım ortamı ayarlar *web.config* proje yayımlandığında ne zaman:</span><span class="sxs-lookup"><span data-stu-id="a2834-329">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="a2834-330">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="a2834-330">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="a2834-331">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="a2834-331">app_offline.htm</span></span>

<span data-ttu-id="a2834-332">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2834-332">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="a2834-333">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a2834-333">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="a2834-334">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2834-334">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="a2834-335">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="a2834-335">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-336">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="a2834-336">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="a2834-337">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-337">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="a2834-338">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="a2834-338">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-339">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="a2834-339">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="a2834-340">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="a2834-340">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="a2834-341">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="a2834-341">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="a2834-342">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="a2834-342">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="a2834-343">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-343">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a2834-344">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a2834-344">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a2834-345">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="a2834-345">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="a2834-346">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a2834-346">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a2834-347">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a2834-347">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="a2834-349">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a2834-349">Log creation and redirection</span></span>

<span data-ttu-id="a2834-350">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a2834-350">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="a2834-351">Herhangi bir klasörde `stdoutLogFile` günlük dosyası oluşturulduğunda, yol modülü tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2834-351">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="a2834-352">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="a2834-352">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="a2834-353">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="a2834-353">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="a2834-354">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="a2834-354">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="a2834-355">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-355">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="a2834-356">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a2834-356">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="a2834-357">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2834-357">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a2834-358">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a2834-358">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="a2834-359">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="a2834-359">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="a2834-360">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur ( *.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="a2834-360">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="a2834-361">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="a2834-361">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-362">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="a2834-362">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="a2834-363">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="a2834-363">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="a2834-364">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a2834-364">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="a2834-365">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-365">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="a2834-366">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a2834-366">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="a2834-367">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="a2834-367">Enhanced diagnostic logs</span></span>

<span data-ttu-id="a2834-368">ASP.NET Core modülü Gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-368">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="a2834-369">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="a2834-369">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2834-370">Yolundaki herhangi bir klasörde (*günlükleri* önceki örnekte) günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2834-370">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="a2834-371">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="a2834-371">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a2834-372">Sağlanan yolda `<handlerSetting>` değeri (*günlükleri* önceki örnekte) modülü tarafından otomatik olarak oluşturulmaz ve dağıtımda önceden mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a2834-372">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="a2834-373">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="a2834-373">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-374">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="a2834-374">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="a2834-375">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="a2834-375">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="a2834-376">HATA</span><span class="sxs-lookup"><span data-stu-id="a2834-376">ERROR</span></span>
* <span data-ttu-id="a2834-377">UYARI</span><span class="sxs-lookup"><span data-stu-id="a2834-377">WARNING</span></span>
* <span data-ttu-id="a2834-378">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="a2834-378">INFO</span></span>
* <span data-ttu-id="a2834-379">TRACE</span><span class="sxs-lookup"><span data-stu-id="a2834-379">TRACE</span></span>

<span data-ttu-id="a2834-380">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="a2834-380">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="a2834-381">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="a2834-381">CONSOLE</span></span>
* <span data-ttu-id="a2834-382">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="a2834-382">EVENTLOG</span></span>
* <span data-ttu-id="a2834-383">DOSYA</span><span class="sxs-lookup"><span data-stu-id="a2834-383">FILE</span></span>

<span data-ttu-id="a2834-384">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="a2834-384">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="a2834-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="a2834-385">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="a2834-386">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="a2834-386">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="a2834-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="a2834-387">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="a2834-388">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="a2834-388">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="a2834-389">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a2834-389">The size of the log isn't limited.</span></span> <span data-ttu-id="a2834-390">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="a2834-390">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="a2834-391">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2834-391">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="modify-the-stack-size"></a><span data-ttu-id="a2834-392">Yığın boyutunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="a2834-392">Modify the stack size</span></span>

<span data-ttu-id="a2834-393">Yönetilen yığın boyutu kullanarak yapılandırma `stackSize` bayt olarak ayarlama.</span><span class="sxs-lookup"><span data-stu-id="a2834-393">Configure the managed stack size using the `stackSize` setting in bytes.</span></span> <span data-ttu-id="a2834-394">Varsayılan boyutu `1048576` bayt (1 MB).</span><span class="sxs-lookup"><span data-stu-id="a2834-394">The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

::: moniker-end

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="a2834-395">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2834-395">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-396">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="a2834-396">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="a2834-397">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2834-397">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="a2834-398">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="a2834-398">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="a2834-399">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="a2834-399">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="a2834-400">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a2834-400">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="a2834-401">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="a2834-401">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="a2834-402">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="a2834-402">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="a2834-403">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="a2834-403">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="a2834-404">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="a2834-404">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="a2834-405">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="a2834-405">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="a2834-406">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="a2834-406">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="a2834-407">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2834-407">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="a2834-408">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **TrustedInstaller** hesabı.</span><span class="sxs-lookup"><span data-stu-id="a2834-408">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="a2834-409">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken oluşturur *applicationHost.config*  Dosya paylaşımındaki.</span><span class="sxs-lookup"><span data-stu-id="a2834-409">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a2834-410">IIS paylaşılan yapılandırmaya, IIS yüklemesi ile aynı makinede kullanırken, ASP.NET Core barındırma Paket Yükleyici ile çalıştırma `OPT_NO_SHARED_CONFIG_CHECK` parametresini `1`:</span><span class="sxs-lookup"><span data-stu-id="a2834-410">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="a2834-411">Paylaşılan yapılandırma yolu IIS yüklemesi ile aynı makinede olmadığı durumlarda, aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="a2834-411">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="a2834-412">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="a2834-412">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a2834-413">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-413">Run the installer.</span></span>
1. <span data-ttu-id="a2834-414">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="a2834-414">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a2834-415">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a2834-415">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a2834-416">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="a2834-416">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="a2834-417">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="a2834-417">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a2834-418">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2834-418">Run the installer.</span></span>
1. <span data-ttu-id="a2834-419">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="a2834-419">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a2834-420">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a2834-420">Re-enable the IIS Shared Configuration.</span></span>

::: moniker-end

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="a2834-421">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="a2834-421">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="a2834-422">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="a2834-422">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="a2834-423">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="a2834-423">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="a2834-424">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2834-424">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="a2834-425">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="a2834-425">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="a2834-426">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a2834-426">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="a2834-427">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="a2834-427">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="a2834-428">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="a2834-428">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="a2834-429">Modül</span><span class="sxs-lookup"><span data-stu-id="a2834-429">Module</span></span>

<span data-ttu-id="a2834-430">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a2834-430">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="a2834-431">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-431">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="a2834-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-432">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a2834-433">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-433">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a2834-434">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-434">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="a2834-435">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a2834-435">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="a2834-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-436">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="a2834-437">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-437">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a2834-438">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-438">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="a2834-439">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="a2834-439">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="a2834-440">Şema</span><span class="sxs-lookup"><span data-stu-id="a2834-440">Schema</span></span>

<span data-ttu-id="a2834-441">**IIS**</span><span class="sxs-lookup"><span data-stu-id="a2834-441">**IIS**</span></span>

* <span data-ttu-id="a2834-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="a2834-442">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a2834-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="a2834-443">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

<span data-ttu-id="a2834-444">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a2834-444">**IIS Express**</span></span>

* <span data-ttu-id="a2834-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a2834-445">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a2834-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="a2834-446">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="a2834-447">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a2834-447">Configuration</span></span>

<span data-ttu-id="a2834-448">**IIS**</span><span class="sxs-lookup"><span data-stu-id="a2834-448">**IIS**</span></span>

* <span data-ttu-id="a2834-449">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a2834-449">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="a2834-450">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a2834-450">**IIS Express**</span></span>

* <span data-ttu-id="a2834-451">Visual Studio: {Uygulama KÖKÜ}\\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a2834-451">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="a2834-452">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="a2834-452">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="a2834-453">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a2834-453">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2834-454">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2834-454">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="a2834-455">ASP.NET Core modülü GitHub deposu (başvuru kaynağı)</span><span class="sxs-lookup"><span data-stu-id="a2834-455">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
