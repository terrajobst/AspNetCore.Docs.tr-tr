---
title: ASP.NET Core Module yapılandırma başvurusu
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0d167f779f9dcae6b0d946dce5e341793daf43bf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091021"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="f2d4b-103">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="f2d4b-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="f2d4b-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f2d4b-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f2d4b-105">Bu belge, ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırma hakkında yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="f2d4b-106">Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz [ASP.NET Core modülü genel bakış](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="f2d4b-107">Barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="f2d4b-107">Hosting model</span></span>

<span data-ttu-id="f2d4b-108">.NET Core 2.2 veya sonraki sürümlerde çalışan uygulamalarda, modül ters proxy (giden işlem) barındırmak için karşılaştırıldığında geliştirilmiş performans için bir işlem içi barındırma modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="f2d4b-109">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="f2d4b-110">Procsess barındırma katılımı olan mevcut uygulamalar için ancak [yeni dotnet](/dotnet/core/tools/dotnet-new) işlemdeki tüm IIS ve IIS Express senaryoları için barındırma modelini varsayılan şablonları.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="f2d4b-111">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreModuleHostingModel>` özellik değerini içeren uygulamanın proje dosyasına `inprocess` (işlem dışı barındırma ile ayarlanır `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="f2d4b-111">To configure an app for in-process hosting, add the `<AspNetCoreModuleHostingModel>` property to the app's project file with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

<span data-ttu-id="f2d4b-112">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="f2d4b-113">[Kestrel sunucu](xref:fundamentals/servers/kestrel) kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="f2d4b-114">Özel bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> uygulaması `IISHttpServer` uygulamanın sunucusu işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="f2d4b-115">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="f2d4b-116">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="f2d4b-117">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-117">Use one app pool per app.</span></span>

* <span data-ttu-id="f2d4b-118">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f2d4b-119">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="f2d4b-120">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="f2d4b-121">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="f2d4b-122">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="f2d4b-123">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-123">Client disconnects are detected.</span></span> <span data-ttu-id="f2d4b-124">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="f2d4b-125">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="f2d4b-125">Hosting model changes</span></span>

<span data-ttu-id="f2d4b-126">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-126">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="f2d4b-127">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-127">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="f2d4b-128">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-128">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="f2d4b-129">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="f2d4b-129">Process name</span></span>

<span data-ttu-id="f2d4b-130">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-130">`Process.GetCurrentProcess().ProcessName` reports either `w3wp` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="f2d4b-131">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f2d4b-131">Configuration with web.config</span></span>

<span data-ttu-id="f2d4b-132">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-132">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="f2d4b-133">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-133">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
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

<span data-ttu-id="f2d4b-134">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="f2d4b-134">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
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
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="f2d4b-135">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-135">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="f2d4b-136">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-136">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="f2d4b-137">Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmanız için ilgili önemli bir not için *web.config* alt uygulamalarında dosyaları.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-137">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="f2d4b-138">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="f2d4b-138">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="f2d4b-139">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="f2d4b-139">Attribute</span></span> | <span data-ttu-id="f2d4b-140">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f2d4b-140">Description</span></span> | <span data-ttu-id="f2d4b-141">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="f2d4b-141">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="f2d4b-142">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-142">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-143">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-143">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="f2d4b-144">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-144">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-145">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-145">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="f2d4b-146">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-147">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-147">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f2d4b-148">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-148">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="f2d4b-149">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-149">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-150">Barındırma modeli işlemdeki belirtir (`inprocess`) veya işlem dışı (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-150">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="f2d4b-151">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-151">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-152">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-152">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="f2d4b-153">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-153">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="f2d4b-154">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-154">Default: `1`</span></span><br><span data-ttu-id="f2d4b-155">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-155">Min: `1`</span></span><br><span data-ttu-id="f2d4b-156">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="f2d4b-156">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="f2d4b-157">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-157">Required string attribute.</span></span></p><p><span data-ttu-id="f2d4b-158">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-158">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f2d4b-159">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-159">Relative paths are supported.</span></span> <span data-ttu-id="f2d4b-160">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-160">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="f2d4b-161">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-161">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-162">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-162">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f2d4b-163">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-163">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="f2d4b-164">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-164">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="f2d4b-165">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-165">Default: `10`</span></span><br><span data-ttu-id="f2d4b-166">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-166">Min: `0`</span></span><br><span data-ttu-id="f2d4b-167">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-167">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="f2d4b-168">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-168">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f2d4b-169">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-169">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f2d4b-170">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-170">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="f2d4b-171">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-171">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="f2d4b-172">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-172">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="f2d4b-173">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-173">Default: `00:02:00`</span></span><br><span data-ttu-id="f2d4b-174">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-174">Min: `00:00:00`</span></span><br><span data-ttu-id="f2d4b-175">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-175">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="f2d4b-176">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-176">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-177">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-177">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="f2d4b-178">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-178">Default: `10`</span></span><br><span data-ttu-id="f2d4b-179">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-179">Min: `0`</span></span><br><span data-ttu-id="f2d4b-180">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-180">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="f2d4b-181">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-182">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-182">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f2d4b-183">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-183">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f2d4b-184">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-184">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="f2d4b-185">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-185">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="f2d4b-186">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-186">Default: `120`</span></span><br><span data-ttu-id="f2d4b-187">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-187">Min: `0`</span></span><br><span data-ttu-id="f2d4b-188">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-188">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="f2d4b-189">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-189">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-190">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-190">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="f2d4b-191">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-191">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-192">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-192">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f2d4b-193">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-193">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f2d4b-194">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-194">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f2d4b-195">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-195">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="f2d4b-196">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-196">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f2d4b-197">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-197">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="f2d4b-198">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="f2d4b-198">Attribute</span></span> | <span data-ttu-id="f2d4b-199">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f2d4b-199">Description</span></span> | <span data-ttu-id="f2d4b-200">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="f2d4b-200">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="f2d4b-201">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-201">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-202">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-202">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="f2d4b-203">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-203">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-204">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-204">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="f2d4b-205">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-206">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-206">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f2d4b-207">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-207">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="f2d4b-208">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-208">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-209">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-209">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="f2d4b-210">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-210">Default: `1`</span></span><br><span data-ttu-id="f2d4b-211">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-211">Min: `1`</span></span><br><span data-ttu-id="f2d4b-212">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-212">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="f2d4b-213">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-213">Required string attribute.</span></span></p><p><span data-ttu-id="f2d4b-214">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-214">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f2d4b-215">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-215">Relative paths are supported.</span></span> <span data-ttu-id="f2d4b-216">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-216">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="f2d4b-217">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-217">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-218">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-218">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f2d4b-219">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-219">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="f2d4b-220">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-220">Default: `10`</span></span><br><span data-ttu-id="f2d4b-221">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-221">Min: `0`</span></span><br><span data-ttu-id="f2d4b-222">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-222">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="f2d4b-223">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-223">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f2d4b-224">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-224">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f2d4b-225">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-225">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="f2d4b-226">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-226">Default: `00:02:00`</span></span><br><span data-ttu-id="f2d4b-227">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-227">Min: `00:00:00`</span></span><br><span data-ttu-id="f2d4b-228">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-228">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="f2d4b-229">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-229">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-230">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-230">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="f2d4b-231">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-231">Default: `10`</span></span><br><span data-ttu-id="f2d4b-232">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-232">Min: `0`</span></span><br><span data-ttu-id="f2d4b-233">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-233">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="f2d4b-234">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-235">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-235">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f2d4b-236">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-236">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f2d4b-237">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-237">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="f2d4b-238">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-238">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="f2d4b-239">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-239">Default: `120`</span></span><br><span data-ttu-id="f2d4b-240">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-240">Min: `0`</span></span><br><span data-ttu-id="f2d4b-241">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-241">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="f2d4b-242">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-242">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-243">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-243">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="f2d4b-244">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-244">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-245">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-245">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f2d4b-246">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-246">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f2d4b-247">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-247">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f2d4b-248">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-248">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="f2d4b-249">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-249">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f2d4b-250">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-250">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="f2d4b-251">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="f2d4b-251">Attribute</span></span> | <span data-ttu-id="f2d4b-252">Açıklama</span><span class="sxs-lookup"><span data-stu-id="f2d4b-252">Description</span></span> | <span data-ttu-id="f2d4b-253">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="f2d4b-253">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="f2d4b-254">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-254">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-255">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-255">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="f2d4b-256">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-256">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-257">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-257">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="f2d4b-258">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-259">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-259">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="f2d4b-260">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-260">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="f2d4b-261">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-261">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-262">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-262">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="f2d4b-263">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-263">Default: `1`</span></span><br><span data-ttu-id="f2d4b-264">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-264">Min: `1`</span></span><br><span data-ttu-id="f2d4b-265">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-265">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="f2d4b-266">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-266">Required string attribute.</span></span></p><p><span data-ttu-id="f2d4b-267">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-267">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="f2d4b-268">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-268">Relative paths are supported.</span></span> <span data-ttu-id="f2d4b-269">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-269">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="f2d4b-270">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-270">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-271">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-271">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="f2d4b-272">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-272">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="f2d4b-273">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-273">Default: `10`</span></span><br><span data-ttu-id="f2d4b-274">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-274">Min: `0`</span></span><br><span data-ttu-id="f2d4b-275">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-275">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="f2d4b-276">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-276">Optional timespan attribute.</span></span></p><p><span data-ttu-id="f2d4b-277">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-277">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="f2d4b-278">ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-278">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="f2d4b-279">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-279">Default: `00:02:00`</span></span><br><span data-ttu-id="f2d4b-280">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-280">Min: `00:00:00`</span></span><br><span data-ttu-id="f2d4b-281">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-281">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="f2d4b-282">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-282">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-283">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-283">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="f2d4b-284">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-284">Default: `10`</span></span><br><span data-ttu-id="f2d4b-285">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-285">Min: `0`</span></span><br><span data-ttu-id="f2d4b-286">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-286">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="f2d4b-287">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="f2d4b-288">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-288">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="f2d4b-289">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-289">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="f2d4b-290">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-290">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="f2d4b-291">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-291">Default: `120`</span></span><br><span data-ttu-id="f2d4b-292">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-292">Min: `0`</span></span><br><span data-ttu-id="f2d4b-293">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="f2d4b-293">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="f2d4b-294">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-294">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="f2d4b-295">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-295">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="f2d4b-296">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-296">Optional string attribute.</span></span></p><p><span data-ttu-id="f2d4b-297">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-297">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="f2d4b-298">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-298">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="f2d4b-299">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-299">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="f2d4b-300">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-300">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="f2d4b-301">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-301">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="f2d4b-302">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-302">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="f2d4b-303">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="f2d4b-303">Setting environment variables</span></span>

<span data-ttu-id="f2d4b-304">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-304">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="f2d4b-305">Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi bir `environmentVariables` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-305">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="f2d4b-306">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-306">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="f2d4b-307">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-307">The following example sets two environment variables.</span></span> <span data-ttu-id="f2d4b-308">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-308">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="f2d4b-309">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-309">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="f2d4b-310">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-310">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
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
> <span data-ttu-id="f2d4b-311">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-311">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="f2d4b-312">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="f2d4b-312">app_offline.htm</span></span>

<span data-ttu-id="f2d4b-313">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-313">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="f2d4b-314">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-314">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="f2d4b-315">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-315">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="f2d4b-316">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-316">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f2d4b-317">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-317">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="f2d4b-318">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-318">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="f2d4b-319">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="f2d4b-319">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f2d4b-320">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="f2d4b-320">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="f2d4b-321">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-321">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="f2d4b-322">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-322">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="f2d4b-323">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-323">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="f2d4b-325">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="f2d4b-325">Log creation and redirection</span></span>

<span data-ttu-id="f2d4b-326">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-326">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="f2d4b-327">Herhangi bir klasörde `stdoutLogFile` yolu, modül günlük dosyası oluşturmak için sırayla bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-327">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="f2d4b-328">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-328">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="f2d4b-329">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-329">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="f2d4b-330">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-330">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="f2d4b-331">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-331">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="f2d4b-332">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-332">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="f2d4b-333">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-333">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="f2d4b-334">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="f2d4b-334">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="f2d4b-335">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-335">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="f2d4b-336">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-336">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="f2d4b-337">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-337">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="f2d4b-338">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-338">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="f2d4b-339">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-339">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="f2d4b-340">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-340">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="f2d4b-341">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="f2d4b-341">Enhanced diagnostic logs</span></span>

<span data-ttu-id="f2d4b-342">ASP.NET Core modülü sağlayan gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-342">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="f2d4b-343">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-343">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="f2d4b-344">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-344">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="f2d4b-345">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="f2d4b-345">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="f2d4b-346">HATA</span><span class="sxs-lookup"><span data-stu-id="f2d4b-346">ERROR</span></span>
* <span data-ttu-id="f2d4b-347">UYARI</span><span class="sxs-lookup"><span data-stu-id="f2d4b-347">WARNING</span></span>
* <span data-ttu-id="f2d4b-348">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="f2d4b-348">INFO</span></span>
* <span data-ttu-id="f2d4b-349">TRACE</span><span class="sxs-lookup"><span data-stu-id="f2d4b-349">TRACE</span></span>

<span data-ttu-id="f2d4b-350">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-350">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="f2d4b-351">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="f2d4b-351">CONSOLE</span></span>
* <span data-ttu-id="f2d4b-352">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="f2d4b-352">EVENTLOG</span></span>
* <span data-ttu-id="f2d4b-353">DOSYA</span><span class="sxs-lookup"><span data-stu-id="f2d4b-353">FILE</span></span>

<span data-ttu-id="f2d4b-354">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-354">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="f2d4b-355">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-355">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="f2d4b-356">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="f2d4b-356">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="f2d4b-357">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-357">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="f2d4b-358">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-358">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="f2d4b-359">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-359">The size of the log isn't limited.</span></span> <span data-ttu-id="f2d4b-360">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-360">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="f2d4b-361">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-361">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="f2d4b-362">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-362">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f2d4b-363">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="f2d4b-363">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="f2d4b-364">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-364">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="f2d4b-365">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-365">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="f2d4b-366">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-366">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="f2d4b-367">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-367">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="f2d4b-368">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-368">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="f2d4b-369">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MSAspNetCoreToken`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-369">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="f2d4b-370">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-370">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="f2d4b-371">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-371">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="f2d4b-372">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-372">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="f2d4b-373">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-373">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="f2d4b-374">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f2d4b-374">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="f2d4b-375">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **sistem** hesabı.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-375">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="f2d4b-376">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken İsabetleri *applicationHost.config* paylaşımda.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-376">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="f2d4b-377">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-377">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="f2d4b-378">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-378">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="f2d4b-379">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-379">Run the installer.</span></span>
1. <span data-ttu-id="f2d4b-380">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-380">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="f2d4b-381">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-381">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="f2d4b-382">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="f2d4b-382">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="f2d4b-383">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="f2d4b-383">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="f2d4b-384">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-384">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="f2d4b-385">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-385">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="f2d4b-386">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-386">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="f2d4b-387">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-387">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="f2d4b-388">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-388">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="f2d4b-389">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="f2d4b-389">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="f2d4b-390">Modül</span><span class="sxs-lookup"><span data-stu-id="f2d4b-390">Module</span></span>

<span data-ttu-id="f2d4b-391">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-391">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="f2d4b-392">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f2d4b-392">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="f2d4b-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f2d4b-393">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="f2d4b-394">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-394">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="f2d4b-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f2d4b-395">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="f2d4b-396">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="f2d4b-396">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="f2d4b-397">Şema</span><span class="sxs-lookup"><span data-stu-id="f2d4b-397">Schema</span></span>

<span data-ttu-id="f2d4b-398">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-398">**IIS**</span></span>

   * <span data-ttu-id="f2d4b-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="f2d4b-399">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="f2d4b-400">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-400">**IIS Express**</span></span>

   * <span data-ttu-id="f2d4b-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="f2d4b-401">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="f2d4b-402">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f2d4b-402">Configuration</span></span>

<span data-ttu-id="f2d4b-403">**IIS**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-403">**IIS**</span></span>

   * <span data-ttu-id="f2d4b-404">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f2d4b-404">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="f2d4b-405">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="f2d4b-405">**IIS Express**</span></span>

   * <span data-ttu-id="f2d4b-406">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="f2d4b-406">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="f2d4b-407">Dosyalar için arama yaparak bulunabilir *aspnetcore.dll* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-407">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="f2d4b-408">IIS Express için *applicationHost.config* dosya varsayılan olarak mevcut olmaz.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-408">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="f2d4b-409">Dosyanın oluşturulma  *\<application_root >\\.vs\\config* Visual Studio çözümü içinde herhangi bir web uygulaması projesine başlatırken.</span><span class="sxs-lookup"><span data-stu-id="f2d4b-409">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
