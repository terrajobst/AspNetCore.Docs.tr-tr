---
title: ASP.NET Core Module yapılandırma başvurusu
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: b6fdca7e5d59fbfb80428f77b5c772cc04daf4b0
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216891"
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="0f8b0-103">ASP.NET Core Module yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="0f8b0-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="0f8b0-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), ve [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="0f8b0-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="0f8b0-105">Bu belge, ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırma hakkında yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="0f8b0-106">Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz [ASP.NET Core modülü genel bakış](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a><span data-ttu-id="0f8b0-107">Barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="0f8b0-107">Hosting model</span></span>

<span data-ttu-id="0f8b0-108">.NET Core 2.2 veya sonraki sürümlerde çalışan uygulamalarda, modül ters proxy (giden işlem) barındırmak için karşılaştırıldığında geliştirilmiş performans için bir işlem içi barındırma modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-108">For apps running on .NET Core 2.2 or later, the module supports an in-process hosting model for improved performance compared to reverse-proxy (out-of-process) hosting.</span></span> <span data-ttu-id="0f8b0-109">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-109">For more information, see <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.</span></span>

<span data-ttu-id="0f8b0-110">Procsess barındırma katılımı olan mevcut uygulamalar için ancak [yeni dotnet](/dotnet/core/tools/dotnet-new) işlemdeki tüm IIS ve IIS Express senaryoları için barındırma modelini varsayılan şablonları.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-110">In-procsess hosting is opt-in for existing apps, but [dotnet new](/dotnet/core/tools/dotnet-new) templates default to the in-process hosting model for all IIS and IIS Express scenarios.</span></span>

<span data-ttu-id="0f8b0-111">İşlem içi barındırmak için bir uygulamayı yapılandırmak için Ekle `<AspNetCoreHostingModel>` özelliği uygulamanın proje dosyasına (örneğin, *MyApp.csproj*) değerini `InProcess` (işlem dışı barındırma ile ayarlanır `outofprocess`):</span><span class="sxs-lookup"><span data-stu-id="0f8b0-111">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file (for example, *MyApp.csproj*) with a value of `InProcess` (out-of-process hosting is set with `outofprocess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="0f8b0-112">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-112">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="0f8b0-113">IIS HTTP sunucusu (`IISHttpServer`) yerine kullanılan [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-113">IIS HTTP Server (`IISHttpServer`) is used instead of the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="0f8b0-114">IIS HTTP sunucusu (`IISHttpServer`) başka bir özelliktir <xref:Microsoft.AspNetCore.Hosting.Server.IServer> yerel istekler IIS içinde ASP.NET Core dönüştüren uygulama isteklerini işlemek için uygulama tarafından yönetilen.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-114">IIS HTTP Server (`IISHttpServer`) is another <xref:Microsoft.AspNetCore.Hosting.Server.IServer> implementation that converts IIS native requests into ASP.NET Core managed requests for processing by the app.</span></span>

* <span data-ttu-id="0f8b0-115">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-115">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="0f8b0-116">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-116">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="0f8b0-117">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-117">Use one app pool per app.</span></span>

* <span data-ttu-id="0f8b0-118">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-118">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="0f8b0-119">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-119">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="0f8b0-120">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-120">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="0f8b0-121">El ile uygulamanın ana bilgisayar ayarlıyorsanız `WebHostBuilder` (kullanmayan [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) ve uygulama çağrı hiç olmadığı kadar (şirket içinde barındırılan) doğrudan Kestrel sunucuda çalıştırıldığında `UseKestrel` çağırmadan önce `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-121">If setting up the app's host manually with `WebHostBuilder` (not using [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) and the app is ever run directly on the Kestrel server (self-hosted), call `UseKestrel` before calling `UseIISIntegration`.</span></span> <span data-ttu-id="0f8b0-122">Sıra ters çevrilir ana bilgisayarı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-122">If the order is reversed, the host fails to start.</span></span>

* <span data-ttu-id="0f8b0-123">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-123">Client disconnects are detected.</span></span> <span data-ttu-id="0f8b0-124">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-124">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="0f8b0-125"><xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine yerine IIS tarafından başlatılan işlem alt dizinini döndürür (örneğin, *C:\Windows\System32\inetsrv* için *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-125"><xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="0f8b0-126">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-126">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="0f8b0-127">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-127">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="0f8b0-128">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-128">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="0f8b0-129">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="0f8b0-129">Hosting model changes</span></span>

<span data-ttu-id="0f8b0-130">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-130">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="0f8b0-131">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-131">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="0f8b0-132">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-132">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="0f8b0-133">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="0f8b0-133">Process name</span></span>

<span data-ttu-id="0f8b0-134">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-134">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

::: moniker-end

## <a name="configuration-with-webconfig"></a><span data-ttu-id="0f8b0-135">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0f8b0-135">Configuration with web.config</span></span>

<span data-ttu-id="0f8b0-136">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-136">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="0f8b0-137">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-137">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="0f8b0-138">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="0f8b0-138">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="0f8b0-139"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-139">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

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

<span data-ttu-id="0f8b0-140">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-140">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="0f8b0-141">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-141">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="0f8b0-142">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-142">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="0f8b0-143">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="0f8b0-143">Attributes of the aspNetCore element</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="0f8b0-144">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="0f8b0-144">Attribute</span></span> | <span data-ttu-id="0f8b0-145">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0f8b0-145">Description</span></span> | <span data-ttu-id="0f8b0-146">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0f8b0-146">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="0f8b0-147">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-147">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-148">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-148">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="0f8b0-149">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-149">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-150">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-150">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="0f8b0-151">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-151">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-152">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-152">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="0f8b0-153">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-153">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="0f8b0-154">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-154">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-155">Barındırma modeli işlemdeki belirtir (`InProcess`) veya işlem dışı (`OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-155">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="0f8b0-156">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-156">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-157">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-157">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="0f8b0-158">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-158">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p> | <span data-ttu-id="0f8b0-159">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-159">Default: `1`</span></span><br><span data-ttu-id="0f8b0-160">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-160">Min: `1`</span></span><br><span data-ttu-id="0f8b0-161">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="0f8b0-161">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="0f8b0-162">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-162">Required string attribute.</span></span></p><p><span data-ttu-id="0f8b0-163">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-163">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="0f8b0-164">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-164">Relative paths are supported.</span></span> <span data-ttu-id="0f8b0-165">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-165">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="0f8b0-166">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-166">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-167">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-167">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="0f8b0-168">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-168">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="0f8b0-169">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-169">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="0f8b0-170">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-170">Default: `10`</span></span><br><span data-ttu-id="0f8b0-171">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-171">Min: `0`</span></span><br><span data-ttu-id="0f8b0-172">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-172">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="0f8b0-173">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-173">Optional timespan attribute.</span></span></p><p><span data-ttu-id="0f8b0-174">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-174">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="0f8b0-175">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-175">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="0f8b0-176">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-176">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="0f8b0-177">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-177">For in-process hosting, the module waits for the app to process the request.</span></span></p> | <span data-ttu-id="0f8b0-178">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-178">Default: `00:02:00`</span></span><br><span data-ttu-id="0f8b0-179">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-179">Min: `00:00:00`</span></span><br><span data-ttu-id="0f8b0-180">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-180">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="0f8b0-181">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-182">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-182">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="0f8b0-183">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-183">Default: `10`</span></span><br><span data-ttu-id="0f8b0-184">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-184">Min: `0`</span></span><br><span data-ttu-id="0f8b0-185">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-185">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="0f8b0-186">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-186">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-187">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-187">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="0f8b0-188">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-188">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="0f8b0-189">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-189">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="0f8b0-190">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-190">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="0f8b0-191">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-191">Default: `120`</span></span><br><span data-ttu-id="0f8b0-192">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-192">Min: `0`</span></span><br><span data-ttu-id="0f8b0-193">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-193">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="0f8b0-194">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-194">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-195">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-195">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="0f8b0-196">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-196">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-197">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-197">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="0f8b0-198">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-198">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="0f8b0-199">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-199">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="0f8b0-200">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-200">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0f8b0-201">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-201">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="0f8b0-202">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-202">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="0f8b0-203">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="0f8b0-203">Attribute</span></span> | <span data-ttu-id="0f8b0-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0f8b0-204">Description</span></span> | <span data-ttu-id="0f8b0-205">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0f8b0-205">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="0f8b0-206">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-206">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-207">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-207">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="0f8b0-208">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-208">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-209">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-209">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="0f8b0-210">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-210">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-211">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-211">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="0f8b0-212">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-212">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="0f8b0-213">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-213">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-214">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-214">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="0f8b0-215">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-215">Default: `1`</span></span><br><span data-ttu-id="0f8b0-216">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-216">Min: `1`</span></span><br><span data-ttu-id="0f8b0-217">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-217">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="0f8b0-218">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-218">Required string attribute.</span></span></p><p><span data-ttu-id="0f8b0-219">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-219">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="0f8b0-220">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-220">Relative paths are supported.</span></span> <span data-ttu-id="0f8b0-221">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-221">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="0f8b0-222">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-223">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-223">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="0f8b0-224">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-224">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="0f8b0-225">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-225">Default: `10`</span></span><br><span data-ttu-id="0f8b0-226">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-226">Min: `0`</span></span><br><span data-ttu-id="0f8b0-227">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-227">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="0f8b0-228">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-228">Optional timespan attribute.</span></span></p><p><span data-ttu-id="0f8b0-229">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-229">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="0f8b0-230">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-230">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="0f8b0-231">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-231">Default: `00:02:00`</span></span><br><span data-ttu-id="0f8b0-232">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-232">Min: `00:00:00`</span></span><br><span data-ttu-id="0f8b0-233">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-233">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="0f8b0-234">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-234">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-235">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-235">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="0f8b0-236">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-236">Default: `10`</span></span><br><span data-ttu-id="0f8b0-237">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-237">Min: `0`</span></span><br><span data-ttu-id="0f8b0-238">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-238">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="0f8b0-239">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-239">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-240">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-240">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="0f8b0-241">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-241">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="0f8b0-242">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-242">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="0f8b0-243">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-243">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="0f8b0-244">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-244">Default: `120`</span></span><br><span data-ttu-id="0f8b0-245">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-245">Min: `0`</span></span><br><span data-ttu-id="0f8b0-246">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-246">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="0f8b0-247">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-247">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-248">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-248">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="0f8b0-249">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-249">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-250">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-250">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="0f8b0-251">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-251">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="0f8b0-252">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-252">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="0f8b0-253">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-253">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0f8b0-254">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-254">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="0f8b0-255">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-255">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| <span data-ttu-id="0f8b0-256">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="0f8b0-256">Attribute</span></span> | <span data-ttu-id="0f8b0-257">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0f8b0-257">Description</span></span> | <span data-ttu-id="0f8b0-258">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="0f8b0-258">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="0f8b0-259">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-259">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-260">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-260">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="0f8b0-261">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-261">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-262">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-262">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="0f8b0-263">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-263">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-264">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-264">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="0f8b0-265">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-265">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="0f8b0-266">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-266">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-267">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-267">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p> | <span data-ttu-id="0f8b0-268">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-268">Default: `1`</span></span><br><span data-ttu-id="0f8b0-269">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-269">Min: `1`</span></span><br><span data-ttu-id="0f8b0-270">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-270">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="0f8b0-271">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-271">Required string attribute.</span></span></p><p><span data-ttu-id="0f8b0-272">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-272">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="0f8b0-273">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-273">Relative paths are supported.</span></span> <span data-ttu-id="0f8b0-274">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-274">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="0f8b0-275">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-275">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-276">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-276">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="0f8b0-277">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-277">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="0f8b0-278">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-278">Default: `10`</span></span><br><span data-ttu-id="0f8b0-279">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-279">Min: `0`</span></span><br><span data-ttu-id="0f8b0-280">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-280">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="0f8b0-281">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-281">Optional timespan attribute.</span></span></p><p><span data-ttu-id="0f8b0-282">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-282">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="0f8b0-283">ASP.NET Core 2.0 veya daha önceki sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` yalnızca tam dakikalar içinde aksi 2 dakika için varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-283">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.0 or earlier, the `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | <span data-ttu-id="0f8b0-284">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-284">Default: `00:02:00`</span></span><br><span data-ttu-id="0f8b0-285">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-285">Min: `00:00:00`</span></span><br><span data-ttu-id="0f8b0-286">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-286">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="0f8b0-287">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-287">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-288">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-288">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="0f8b0-289">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-289">Default: `10`</span></span><br><span data-ttu-id="0f8b0-290">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-290">Min: `0`</span></span><br><span data-ttu-id="0f8b0-291">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-291">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="0f8b0-292">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-292">Optional integer attribute.</span></span></p><p><span data-ttu-id="0f8b0-293">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-293">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="0f8b0-294">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-294">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="0f8b0-295">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-295">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | <span data-ttu-id="0f8b0-296">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-296">Default: `120`</span></span><br><span data-ttu-id="0f8b0-297">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-297">Min: `0`</span></span><br><span data-ttu-id="0f8b0-298">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="0f8b0-298">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="0f8b0-299">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-299">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="0f8b0-300">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-300">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="0f8b0-301">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-301">Optional string attribute.</span></span></p><p><span data-ttu-id="0f8b0-302">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-302">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="0f8b0-303">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-303">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="0f8b0-304">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-304">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="0f8b0-305">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-305">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0f8b0-306">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak (*.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-306">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="0f8b0-307">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-307">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a><span data-ttu-id="0f8b0-308">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="0f8b0-308">Setting environment variables</span></span>

<span data-ttu-id="0f8b0-309">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-309">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="0f8b0-310">Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi bir `environmentVariables` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-310">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="0f8b0-311">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-311">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="0f8b0-312">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-312">The following example sets two environment variables.</span></span> <span data-ttu-id="0f8b0-313">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-313">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="0f8b0-314">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-314">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="0f8b0-315">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-315">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="0f8b0-316">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-316">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="0f8b0-317">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="0f8b0-317">app_offline.htm</span></span>

<span data-ttu-id="0f8b0-318">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-318">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="0f8b0-319">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-319">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="0f8b0-320">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-320">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="0f8b0-321">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-321">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0f8b0-322">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-322">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="0f8b0-323">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-323">For example, a websocket connection may delay app shut down.</span></span>

::: moniker-end

## <a name="start-up-error-page"></a><span data-ttu-id="0f8b0-324">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="0f8b0-324">Start-up error page</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0f8b0-325">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-325">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="0f8b0-326">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-326">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="0f8b0-327">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-327">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="0f8b0-328">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-328">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="0f8b0-329">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-329">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="0f8b0-330">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-330">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="0f8b0-331">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-331">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="0f8b0-332">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-332">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="0f8b0-333">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-333">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a><span data-ttu-id="0f8b0-335">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="0f8b0-335">Log creation and redirection</span></span>

<span data-ttu-id="0f8b0-336">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-336">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="0f8b0-337">Herhangi bir klasörde `stdoutLogFile` yolu, modül günlük dosyası oluşturmak için sırayla bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-337">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="0f8b0-338">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-338">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="0f8b0-339">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-339">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="0f8b0-340">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-340">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="0f8b0-341">Stdout günlüğü kullanarak uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-341">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="0f8b0-342">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-342">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="0f8b0-343">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-343">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="0f8b0-344">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="0f8b0-344">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="0f8b0-345">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-345">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="0f8b0-346">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-346">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="0f8b0-347">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-347">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0f8b0-348">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-348">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="0f8b0-349">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-349">After startup, all additional logs are discarded.</span></span>

::: moniker-end

<span data-ttu-id="0f8b0-350">Aşağıdaki örnek `aspNetCore` öğesi stdout günlük kaydı için Azure App Service'te barındırılan bir uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-350">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="0f8b0-351">Bir yerel yol ya da ağ paylaşımı yolu yerel günlüğe kaydetme için kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-351">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="0f8b0-352">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-352">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

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

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="0f8b0-353">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="0f8b0-353">Enhanced diagnostic logs</span></span>

<span data-ttu-id="0f8b0-354">ASP.NET Core modülü sağlayan gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-354">The ASP.NET Core Module provides is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="0f8b0-355">Ekleme `<handlerSettings>` öğesine `<aspNetCore>` öğesinde *web.config*. Ayarı `debugLevel` için `TRACE` tanılama bilgileri daha yüksek bir aslına uygunluk sunar:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-355">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="0f8b0-356">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-356">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="0f8b0-357">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="0f8b0-357">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="0f8b0-358">HATA</span><span class="sxs-lookup"><span data-stu-id="0f8b0-358">ERROR</span></span>
* <span data-ttu-id="0f8b0-359">UYARI</span><span class="sxs-lookup"><span data-stu-id="0f8b0-359">WARNING</span></span>
* <span data-ttu-id="0f8b0-360">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="0f8b0-360">INFO</span></span>
* <span data-ttu-id="0f8b0-361">TRACE</span><span class="sxs-lookup"><span data-stu-id="0f8b0-361">TRACE</span></span>

<span data-ttu-id="0f8b0-362">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-362">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="0f8b0-363">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="0f8b0-363">CONSOLE</span></span>
* <span data-ttu-id="0f8b0-364">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="0f8b0-364">EVENTLOG</span></span>
* <span data-ttu-id="0f8b0-365">DOSYA</span><span class="sxs-lookup"><span data-stu-id="0f8b0-365">FILE</span></span>

<span data-ttu-id="0f8b0-366">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-366">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="0f8b0-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-367">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="0f8b0-368">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="0f8b0-368">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="0f8b0-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-369">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="0f8b0-370">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-370">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="0f8b0-371">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-371">The size of the log isn't limited.</span></span> <span data-ttu-id="0f8b0-372">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-372">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

::: moniker-end

<span data-ttu-id="0f8b0-373">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-373">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="0f8b0-374">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-374">Proxy configuration uses HTTP protocol and a pairing token</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="0f8b0-375">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="0f8b0-375">*Only applies to out-of-process hosting.*</span></span>

::: moniker-end

<span data-ttu-id="0f8b0-376">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-376">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="0f8b0-377">HTTP bir performans iyileştirme Kestrel ve modül arasındaki trafik bir geri döngü adresine ağ arabiriminin dışına gerçekleştiği kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-377">Using HTTP is a performance optimization, where the traffic between the module and Kestrel takes place on a loopback address off of the network interface.</span></span> <span data-ttu-id="0f8b0-378">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-378">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="0f8b0-379">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-379">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="0f8b0-380">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-380">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="0f8b0-381">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-381">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="0f8b0-382">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-382">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="0f8b0-383">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-383">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="0f8b0-384">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-384">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="0f8b0-385">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-385">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="0f8b0-386">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0f8b0-386">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="0f8b0-387">ASP.NET Core modülü yükleyiciyi ayrıcalıklarıyla çalıştırır **sistem** hesabı.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-387">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="0f8b0-388">IIS paylaşılan yapılandırması tarafından kullanılan paylaşımı yol için izne sahip yerel sistem hesabı olmayan değiştirme çünkü Yükleyici bir erişim reddedildi hatası modül ayarlarını yapılandırılmaya çalışılırken İsabetleri *applicationHost.config* paylaşımda.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-388">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="0f8b0-389">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-389">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="0f8b0-390">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-390">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="0f8b0-391">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-391">Run the installer.</span></span>
1. <span data-ttu-id="0f8b0-392">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-392">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="0f8b0-393">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-393">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="0f8b0-394">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="0f8b0-394">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="0f8b0-395">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="0f8b0-395">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="0f8b0-396">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-396">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="0f8b0-397">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-397">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="0f8b0-398">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-398">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="0f8b0-399">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-399">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="0f8b0-400">Barındırma Paket Yükleyici günlükleri modülü için adresten *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosyanın nasıl adlandırıldığı *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-400">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="0f8b0-401">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="0f8b0-401">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="0f8b0-402">Modül</span><span class="sxs-lookup"><span data-stu-id="0f8b0-402">Module</span></span>

<span data-ttu-id="0f8b0-403">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-403">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="0f8b0-404">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-404">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="0f8b0-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-405">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="0f8b0-406">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-406">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="0f8b0-407">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-407">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

<span data-ttu-id="0f8b0-408">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-408">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="0f8b0-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-409">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="0f8b0-410">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-410">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="0f8b0-411">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-411">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

   * <span data-ttu-id="0f8b0-412">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="0f8b0-412">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

::: moniker-end

### <a name="schema"></a><span data-ttu-id="0f8b0-413">Şema</span><span class="sxs-lookup"><span data-stu-id="0f8b0-413">Schema</span></span>

<span data-ttu-id="0f8b0-414">**IIS**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-414">**IIS**</span></span>

   * <span data-ttu-id="0f8b0-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="0f8b0-415">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="0f8b0-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="0f8b0-416">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end
<span data-ttu-id="0f8b0-417">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-417">**IIS Express**</span></span>

   * <span data-ttu-id="0f8b0-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="0f8b0-418">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

::: moniker range=">= aspnetcore-2.2"

   * <span data-ttu-id="0f8b0-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="0f8b0-419">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="0f8b0-420">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0f8b0-420">Configuration</span></span>

<span data-ttu-id="0f8b0-421">**IIS**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-421">**IIS**</span></span>

   * <span data-ttu-id="0f8b0-422">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="0f8b0-422">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="0f8b0-423">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="0f8b0-423">**IIS Express**</span></span>

   * <span data-ttu-id="0f8b0-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="0f8b0-424">%ProgramFiles%\IIS Express\config\templates\PersonalWebServer\applicationHost.config</span></span>

<span data-ttu-id="0f8b0-425">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f8b0-425">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>
