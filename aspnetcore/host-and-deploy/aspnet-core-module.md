---
title: ASP.NET Core Module yapılandırma başvurusu
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 32fbf2b19da2d088847279f447f9a72cedcf8085
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570184"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="96edb-103">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="96edb-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="96edb-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), ve [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="96edb-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="96edb-105">Bu belge, ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırma hakkında yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="96edb-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="96edb-106">Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz [ASP.NET Core modülü genel bakış](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="96edb-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="96edb-107">Barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="96edb-107">Hosting model</span></span>

<span data-ttu-id="96edb-108">.NET Core 2.2 veya sonraki sürümlerde çalışan uygulamalarda, modül ters proxy (giden işlem) barındırmak için karşılaştırıldığında geliştirilmiş performans için bir işlem içi barındırma modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="96edb-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="96edb-109">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="96edb-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="96edb-110">Procsess barındırma katılımı olan mevcut uygulamalar için ancak [yeni dotnet](/dotnet/core/tools/dotnet-new) işlemdeki tüm IIS ve IIS Express senaryoları için barındırma modelini varsayılan şablonları.</span><span class="sxs-lookup"><span data-stu-id="96edb-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="96edb-111">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreHostingModel>` özelliği uygulamanın proje dosyasına (örneğin, *MyApp.csproj*) değerini `inprocess` (işlem dışı barındırma ile ayarlanır `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="96edb-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `inprocess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>inprocess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="96edb-112">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="96edb-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="96edb-113">[Kestrel sunucu](xref:fundamentals/servers/kestrel) kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="96edb-113">The [Kestrel server](xref:fundamentals/servers/kestrel) isn't used.</span></span> <span data-ttu-id="96edb-114">Özel bir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> uygulaması `IISHttpServer` uygulamanın sunucusu işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="96edb-114">A custom <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation, `IISHttpServer` acts as the app's server.</span></span>

* <span data-ttu-id="96edb-115">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="96edb-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="96edb-116">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="96edb-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="96edb-117">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="96edb-117">Use one app pool per app.</span></span>

* <span data-ttu-id="96edb-118">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="96edb-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="96edb-119">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="96edb-120">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="96edb-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="96edb-121">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="96edb-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="96edb-122">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="96edb-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="96edb-123">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="96edb-123">Client disconnects are detected.</span></span> <span data-ttu-id="96edb-124">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="96edb-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="96edb-125">`Directory.GetCurrentDirectory()` uygulama dizini yerine IIS tarafından başlatılan işlem alt dizinini döndürür (örneğin, *C:\Windows\System32\inetsrv* için *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="96edb-125">`Directory.GetCurrentDirectory()` returns the worker directory of the process started by IIS rather than the application directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="96edb-126">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="96edb-126">Hosting model changes</span></span>

<span data-ttu-id="96edb-127">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="96edb-127">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="96edb-128">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="96edb-128">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="96edb-129">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="96edb-129">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="96edb-130">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="96edb-130">Process name</span></span>

<span data-ttu-id="96edb-131">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="96edb-131">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="96edb-132">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="96edb-132">Configuration with web.config</span></span>

<span data-ttu-id="96edb-133">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="96edb-133">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="96edb-134">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="96edb-134">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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
                  hostingModel="inprocess" />
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

<span data-ttu-id="96edb-135">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="96edb-135">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="96edb-136"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="96edb-136">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="96edb-137">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="96edb-137">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="96edb-138">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="96edb-138">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="96edb-139">Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmanız için ilgili önemli bir not için *web.config* alt uygulamalarında dosyaları.</span><span class="sxs-lookup"><span data-stu-id="96edb-139">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="96edb-140">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="96edb-140">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="96edb-141">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="96edb-141">Attribute</span></span> | <span data-ttu-id="96edb-142">Açıklama</span><span class="sxs-lookup"><span data-stu-id="96edb-142">Description</span></span> | <span data-ttu-id="96edb-143">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="96edb-143">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="96edb-144">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-144">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-145">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="96edb-145">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="96edb-146">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-146">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-147">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-147">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="96edb-148">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-148">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-149">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-149">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="96edb-150">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="96edb-150">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="96edb-151">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-151">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-152">Barındırma modeli işlemdeki belirtir (`inprocess`) veya işlem dışı (`outofprocess`).</span><span class="sxs-lookup"><span data-stu-id="96edb-152">Specifies the hosting model as in-process (`inprocess`) or out-of-process (`outofprocess`).</span></span></p> | `outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="96edb-153">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-153">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-154">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="96edb-154">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="96edb-155">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="96edb-155">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="96edb-156">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-156">Default: `1`</span></span><br><span data-ttu-id="96edb-157">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-157">Min: `1`</span></span><br><span data-ttu-id="96edb-158">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="96edb-158">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="96edb-159">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-159">Required string attribute.</span></span></p><p><span data-ttu-id="96edb-160">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-160">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="96edb-161">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="96edb-161">Relative paths are supported.</span></span> <span data-ttu-id="96edb-162">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-162">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="96edb-163">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-163">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-164">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="96edb-164">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="96edb-165">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="96edb-165">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="96edb-166">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="96edb-166">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="96edb-167">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-167">Default: `10`</span></span><br><span data-ttu-id="96edb-168">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-168">Min: `0`</span></span><br><span data-ttu-id="96edb-169">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="96edb-169">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="96edb-170">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-170">Optional timespan attribute.</span></span></p><p><span data-ttu-id="96edb-171">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="96edb-171">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="96edb-172">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-172">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="96edb-173">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="96edb-173">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="96edb-174">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="96edb-174">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="96edb-175">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-175">Default: `00:02:00`</span></span><br><span data-ttu-id="96edb-176">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-176">Min: `00:00:00`</span></span><br><span data-ttu-id="96edb-177">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-177">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="96edb-178">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-178">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-179">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="96edb-179">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="96edb-180">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-180">Default: `10`</span></span><br><span data-ttu-id="96edb-181">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-181">Min: `0`</span></span><br><span data-ttu-id="96edb-182">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="96edb-182">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="96edb-183">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-183">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-184">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="96edb-184">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="96edb-185">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="96edb-185">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="96edb-186">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="96edb-186">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="96edb-187">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="96edb-187">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="96edb-188">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="96edb-188">Default: `120`</span></span><br><span data-ttu-id="96edb-189">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-189">Min: `0`</span></span><br><span data-ttu-id="96edb-190">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="96edb-190">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="96edb-191">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-191">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-192">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="96edb-192">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="96edb-193">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-193">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-194">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-194">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="96edb-195">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="96edb-195">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="96edb-196">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-196">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="96edb-197">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96edb-197">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="96edb-198">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-198">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="96edb-199">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="96edb-199">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="96edb-200">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="96edb-200">Attribute</span></span> | <span data-ttu-id="96edb-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="96edb-201">Description</span></span> | <span data-ttu-id="96edb-202">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="96edb-202">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="96edb-203">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-203">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-204">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="96edb-204">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="96edb-205">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-205">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-206">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-206">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="96edb-207">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-207">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-208">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-208">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="96edb-209">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="96edb-209">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="96edb-210">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-211">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="96edb-211">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="96edb-212">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-212">Default: `1`</span></span><br><span data-ttu-id="96edb-213">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-213">Min: `1`</span></span><br><span data-ttu-id="96edb-214">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="96edb-214">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="96edb-215">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-215">Required string attribute.</span></span></p><p><span data-ttu-id="96edb-216">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-216">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="96edb-217">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="96edb-217">Relative paths are supported.</span></span> <span data-ttu-id="96edb-218">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-218">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="96edb-219">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-219">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-220">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="96edb-220">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="96edb-221">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="96edb-221">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="96edb-222">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-222">Default: `10`</span></span><br><span data-ttu-id="96edb-223">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-223">Min: `0`</span></span><br><span data-ttu-id="96edb-224">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="96edb-224">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="96edb-225">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-225">Optional timespan attribute.</span></span></p><p><span data-ttu-id="96edb-226">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="96edb-226">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="96edb-227">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-227">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="96edb-228">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-228">Default: `00:02:00`</span></span><br><span data-ttu-id="96edb-229">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-229">Min: `00:00:00`</span></span><br><span data-ttu-id="96edb-230">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-230">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="96edb-231">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-231">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-232">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="96edb-232">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="96edb-233">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-233">Default: `10`</span></span><br><span data-ttu-id="96edb-234">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-234">Min: `0`</span></span><br><span data-ttu-id="96edb-235">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="96edb-235">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="96edb-236">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-236">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-237">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="96edb-237">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="96edb-238">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="96edb-238">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="96edb-239">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="96edb-239">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="96edb-240">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="96edb-240">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="96edb-241">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="96edb-241">Default: `120`</span></span><br><span data-ttu-id="96edb-242">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-242">Min: `0`</span></span><br><span data-ttu-id="96edb-243">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="96edb-243">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="96edb-244">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-244">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-245">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="96edb-245">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="96edb-246">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-246">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-247">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-247">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="96edb-248">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="96edb-248">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="96edb-249">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-249">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="96edb-250">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96edb-250">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="96edb-251">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-251">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="96edb-252">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="96edb-252">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="96edb-253">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="96edb-253">Attribute</span></span> | <span data-ttu-id="96edb-254">Açıklama</span><span class="sxs-lookup"><span data-stu-id="96edb-254">Description</span></span> | <span data-ttu-id="96edb-255">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="96edb-255">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="96edb-256">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-256">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-257">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="96edb-257">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="96edb-258">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-258">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-259">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-259">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="96edb-260">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-260">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-261">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-261">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="96edb-262">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="96edb-262">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="96edb-263">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-263">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-264">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="96edb-264">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="96edb-265">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-265">Default: `1`</span></span><br><span data-ttu-id="96edb-266">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="96edb-266">Min: `1`</span></span><br><span data-ttu-id="96edb-267">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="96edb-267">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="96edb-268">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-268">Required string attribute.</span></span></p><p><span data-ttu-id="96edb-269">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-269">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="96edb-270">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="96edb-270">Relative paths are supported.</span></span> <span data-ttu-id="96edb-271">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-271">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="96edb-272">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-272">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-273">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="96edb-273">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="96edb-274">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="96edb-274">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="96edb-275">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-275">Default: `10`</span></span><br><span data-ttu-id="96edb-276">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-276">Min: `0`</span></span><br><span data-ttu-id="96edb-277">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="96edb-277">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="96edb-278">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-278">Optional timespan attribute.</span></span></p><p><span data-ttu-id="96edb-279">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="96edb-279">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="96edb-280">ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-280">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="96edb-281">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-281">Default: `00:02:00`</span></span><br><span data-ttu-id="96edb-282">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-282">Min: `00:00:00`</span></span><br><span data-ttu-id="96edb-283">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="96edb-283">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="96edb-284">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-284">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-285">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="96edb-285">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="96edb-286">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="96edb-286">Default: `10`</span></span><br><span data-ttu-id="96edb-287">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-287">Min: `0`</span></span><br><span data-ttu-id="96edb-288">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="96edb-288">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="96edb-289">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-289">Optional integer attribute.</span></span></p><p><span data-ttu-id="96edb-290">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="96edb-290">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="96edb-291">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="96edb-291">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="96edb-292">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="96edb-292">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="96edb-293">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="96edb-293">Default: `120`</span></span><br><span data-ttu-id="96edb-294">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="96edb-294">Min: `0`</span></span><br><span data-ttu-id="96edb-295">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="96edb-295">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="96edb-296">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-296">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="96edb-297">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="96edb-297">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="96edb-298">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-298">Optional string attribute.</span></span></p><p><span data-ttu-id="96edb-299">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-299">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="96edb-300">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="96edb-300">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="96edb-301">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-301">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="96edb-302">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96edb-302">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="96edb-303">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-303">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="96edb-304">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="96edb-304">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="96edb-305">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="96edb-305">Setting environment variables</span></span>

<span data-ttu-id="96edb-306">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-306">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="96edb-307">Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi bir `environmentVariables` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="96edb-307">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="96edb-308">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-308">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="96edb-309">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="96edb-309">The following example sets two environment variables.</span></span> <span data-ttu-id="96edb-310">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="96edb-310">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="96edb-311">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="96edb-311">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="96edb-312">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="96edb-312">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="96edb-313">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="96edb-313">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="96edb-314">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="96edb-314">app_offline.htm</span></span>

<span data-ttu-id="96edb-315">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="96edb-315">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="96edb-316">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="96edb-316">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="96edb-317">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="96edb-317">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="96edb-318">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="96edb-318">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96edb-319">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="96edb-319">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="96edb-320">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-320">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="96edb-321">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="96edb-321">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96edb-322">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="96edb-322">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="96edb-323">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="96edb-323">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="96edb-324">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="96edb-324">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="96edb-325">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="96edb-325">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="96edb-326">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-326">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="96edb-327">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="96edb-327">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="96edb-328">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="96edb-328">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="96edb-329">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="96edb-329">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="96edb-330">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="96edb-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="96edb-332">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="96edb-332">Log creation and redirection</span></span>

<span data-ttu-id="96edb-333">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="96edb-333">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="96edb-334">Herhangi bir klasörde `stdoutLogFile` yolu, modül günlük dosyası oluşturmak için sırayla bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96edb-334">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="96edb-335">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="96edb-335">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="96edb-336">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="96edb-336">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="96edb-337">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="96edb-337">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="96edb-338">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-338">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="96edb-339">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="96edb-339">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="96edb-340">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="96edb-340">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="96edb-341">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="96edb-341">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="96edb-342">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="96edb-342">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="96edb-343">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="96edb-343">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="96edb-344">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="96edb-344">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96edb-345">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="96edb-345">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="96edb-346">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="96edb-346">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="96edb-347">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="96edb-347">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="96edb-348">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-348">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="96edb-349">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="96edb-349">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="96edb-350">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="96edb-350">Enhanced diagnostic logs</span></span>

<span data-ttu-id="96edb-351">ASP.NET Core modülü sağlayan gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-351">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="96edb-352">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="96edb-352">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="96edb-353">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="96edb-353">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="96edb-354">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="96edb-354">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="96edb-355">HATA</span><span class="sxs-lookup"><span data-stu-id="96edb-355">ERROR</span></span>
* <span data-ttu-id="96edb-356">UYARI</span><span class="sxs-lookup"><span data-stu-id="96edb-356">WARNING</span></span>
* <span data-ttu-id="96edb-357">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="96edb-357">INFO</span></span>
* <span data-ttu-id="96edb-358">TRACE</span><span class="sxs-lookup"><span data-stu-id="96edb-358">TRACE</span></span>

<span data-ttu-id="96edb-359">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="96edb-359">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="96edb-360">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="96edb-360">CONSOLE</span></span>
* <span data-ttu-id="96edb-361">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="96edb-361">EVENTLOG</span></span>
* <span data-ttu-id="96edb-362">DOSYA</span><span class="sxs-lookup"><span data-stu-id="96edb-362">FILE</span></span>

<span data-ttu-id="96edb-363">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="96edb-363">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="96edb-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="96edb-364">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="96edb-365">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="96edb-365">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="96edb-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="96edb-366">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="96edb-367">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="96edb-367">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="96edb-368">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="96edb-368">The size of the log isn't limited.</span></span> <span data-ttu-id="96edb-369">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="96edb-369">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="96edb-370">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="96edb-370">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="96edb-371">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="96edb-371">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="96edb-372">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="96edb-372">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="96edb-373">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="96edb-373">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="96edb-374">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="96edb-374">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="96edb-375">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="96edb-375">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="96edb-376">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="96edb-376">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="96edb-377">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="96edb-377">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="96edb-378">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MSAspNetCoreToken`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="96edb-378">The pairing token is also set into a header (`MSAspNetCoreToken`) on every proxied request.</span></span> <span data-ttu-id="96edb-379">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="96edb-379">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="96edb-380">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="96edb-380">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="96edb-381">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="96edb-381">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="96edb-382">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="96edb-382">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="96edb-383">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="96edb-383">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="96edb-384">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **sistem** hesabı.</span><span class="sxs-lookup"><span data-stu-id="96edb-384">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="96edb-385">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken İsabetleri *applicationHost.config* paylaşımda.</span><span class="sxs-lookup"><span data-stu-id="96edb-385">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="96edb-386">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="96edb-386">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="96edb-387">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="96edb-387">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="96edb-388">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="96edb-388">Run the installer.</span></span>
1. <span data-ttu-id="96edb-389">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="96edb-389">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="96edb-390">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="96edb-390">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="96edb-391">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="96edb-391">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="96edb-392">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="96edb-392">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="96edb-393">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="96edb-393">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="96edb-394">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="96edb-394">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="96edb-395">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="96edb-395">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="96edb-396">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="96edb-396">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="96edb-397">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="96edb-397">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="96edb-398">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="96edb-398">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="96edb-399">Modül</span><span class="sxs-lookup"><span data-stu-id="96edb-399">Module</span></span>

<span data-ttu-id="96edb-400">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="96edb-400">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="96edb-401">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-401">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="96edb-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-402">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="96edb-403">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-403">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="96edb-404">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-404">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="96edb-405">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="96edb-405">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="96edb-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-406">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="96edb-407">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-407">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="96edb-408">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-408">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="96edb-409">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="96edb-409">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="96edb-410">Şema</span><span class="sxs-lookup"><span data-stu-id="96edb-410">Schema</span></span>

<span data-ttu-id="96edb-411">**IIS**</span><span class="sxs-lookup"><span data-stu-id="96edb-411">**IIS**</span></span>

   * <span data-ttu-id="96edb-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="96edb-412">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="96edb-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="96edb-413">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="96edb-414">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="96edb-414">**IIS Express**</span></span>

   * <span data-ttu-id="96edb-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="96edb-415">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="96edb-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="96edb-416">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="96edb-417">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="96edb-417">Configuration</span></span>

<span data-ttu-id="96edb-418">**IIS**</span><span class="sxs-lookup"><span data-stu-id="96edb-418">**IIS**</span></span>

   * <span data-ttu-id="96edb-419">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="96edb-419">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="96edb-420">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="96edb-420">**IIS Express**</span></span>

   * <span data-ttu-id="96edb-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="96edb-421">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="96edb-422">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="96edb-422">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
