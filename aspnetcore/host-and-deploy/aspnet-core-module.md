---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamaları barındırmak için gereken ASP.NET Core modülü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 75f4a158253dd3276ed37011d9aa73d82cad5b79
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75952015"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="dd7e4-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="dd7e4-103">ASP.NET Core Module</span></span>

<span data-ttu-id="dd7e4-104">, [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris](https://github.com/Tratcher)No, [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [adatın kotalik](https://github.com/jkotalik)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="dd7e4-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="dd7e4-105">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="dd7e4-106">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="dd7e4-107">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="dd7e4-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-108">Supported Windows versions:</span></span>

* <span data-ttu-id="dd7e4-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-109">Windows 7 or later</span></span>
* <span data-ttu-id="dd7e4-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="dd7e4-111">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="dd7e4-112">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="dd7e4-113">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="dd7e4-114">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="dd7e4-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="dd7e4-115">In-process hosting model</span></span>

<span data-ttu-id="dd7e4-116">Uygulamalar, işlem içi barındırma modelinde varsayılan olarak ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="dd7e4-117">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="dd7e4-118">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="dd7e4-119">İşlem içi için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="dd7e4-120">`IISHttpServer`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="dd7e4-121">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="dd7e4-122">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="dd7e4-123">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="dd7e4-124">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="dd7e4-125">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-125">Use one app pool per app.</span></span>

* <span data-ttu-id="dd7e4-126">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="dd7e4-127">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="dd7e4-128">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="dd7e4-129">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-129">Client disconnects are detected.</span></span> <span data-ttu-id="dd7e4-130">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="dd7e4-131">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="dd7e4-132">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="dd7e4-133">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="dd7e4-134">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="dd7e4-135">İşlem içi barındırma sırasında, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="dd7e4-136">Bu nedenle, her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="dd7e4-137">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulamayla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="dd7e4-138">[Web paketi (tek dosya) dağıtımları](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="dd7e4-139">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="dd7e4-139">Out-of-process hosting model</span></span>

<span data-ttu-id="dd7e4-140">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, `<AspNetCoreHostingModel>` özelliğinin değerini proje dosyasında ( *. csproj*) `OutOfProcess` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="dd7e4-141">İşlem içi barındırma, varsayılan değer olan `InProcess`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="dd7e4-142">`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-142">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="dd7e4-143">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-143">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="dd7e4-144">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-144">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="dd7e4-145">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-145">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="dd7e4-146">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-146">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="dd7e4-147">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-147">Hosting model changes</span></span>

<span data-ttu-id="dd7e4-148">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-148">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="dd7e4-149">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-149">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="dd7e4-150">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-150">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="dd7e4-151">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="dd7e4-151">Process name</span></span>

<span data-ttu-id="dd7e4-152">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-152">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="dd7e4-153">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-153">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="dd7e4-154">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-154">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="dd7e4-155">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-155">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="dd7e4-156">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-156">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="dd7e4-157">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-157">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="dd7e4-158">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-158">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="dd7e4-159">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="dd7e4-159">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="dd7e4-160">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-160">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="dd7e4-161">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-161">Configuration with web.config</span></span>

<span data-ttu-id="dd7e4-162">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-162">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="dd7e4-163">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-163">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="dd7e4-164">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-164">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="dd7e4-165"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-165">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="dd7e4-166">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-166">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-167">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-167">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="dd7e4-168">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-168">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="dd7e4-169">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-169">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="dd7e4-170">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="dd7e4-170">Attribute</span></span> | <span data-ttu-id="dd7e4-171">Açıklama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-171">Description</span></span> | <span data-ttu-id="dd7e4-172">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="dd7e4-172">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="dd7e4-173">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-173">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-174">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-174">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="dd7e4-175">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-176">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-176">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="dd7e4-177">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-177">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-178">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-178">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="dd7e4-179">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-179">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="dd7e4-180">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-180">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-181">Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-181">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p><span data-ttu-id="dd7e4-182">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-183">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-183">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="dd7e4-184">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-184">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="dd7e4-185">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-185">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="dd7e4-186">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-186">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="dd7e4-187">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-187">Default: `1`</span></span><br><span data-ttu-id="dd7e4-188">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-188">Min: `1`</span></span><br><span data-ttu-id="dd7e4-189">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="dd7e4-189">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="dd7e4-190">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-190">Required string attribute.</span></span></p><p><span data-ttu-id="dd7e4-191">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-191">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="dd7e4-192">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-192">Relative paths are supported.</span></span> <span data-ttu-id="dd7e4-193">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-193">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="dd7e4-194">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-194">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-195">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-195">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="dd7e4-196">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-196">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="dd7e4-197">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-197">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="dd7e4-198">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-198">Default: `10`</span></span><br><span data-ttu-id="dd7e4-199">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-199">Min: `0`</span></span><br><span data-ttu-id="dd7e4-200">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-200">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="dd7e4-201">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-201">Optional timespan attribute.</span></span></p><p><span data-ttu-id="dd7e4-202">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-202">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="dd7e4-203">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-203">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="dd7e4-204">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-204">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="dd7e4-205">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-205">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="dd7e4-206">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-206">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="dd7e4-207">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-207">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="dd7e4-208">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-208">Default: `00:02:00`</span></span><br><span data-ttu-id="dd7e4-209">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-209">Min: `00:00:00`</span></span><br><span data-ttu-id="dd7e4-210">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-210">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="dd7e4-211">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-211">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-212">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-212">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="dd7e4-213">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-213">Default: `10`</span></span><br><span data-ttu-id="dd7e4-214">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-214">Min: `0`</span></span><br><span data-ttu-id="dd7e4-215">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-215">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="dd7e4-216">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-216">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-217">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-217">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="dd7e4-218">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-218">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="dd7e4-219">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-219">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="dd7e4-220">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-220">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="dd7e4-221">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-221">Default: `120`</span></span><br><span data-ttu-id="dd7e4-222">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-222">Min: `0`</span></span><br><span data-ttu-id="dd7e4-223">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-223">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="dd7e4-224">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-224">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-225">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-225">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="dd7e4-226">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-226">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-227">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-227">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="dd7e4-228">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-228">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="dd7e4-229">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-229">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="dd7e4-230">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-230">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-231">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-231">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="dd7e4-232">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-232">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="dd7e4-233">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-233">Set environment variables</span></span>

<span data-ttu-id="dd7e4-234">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-234">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="dd7e4-235">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-235">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="dd7e4-236">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-236">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="dd7e4-237">Aşağıdaki örnek, *Web. config*dosyasında iki ortam değişkenini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-237">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="dd7e4-238">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-238">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="dd7e4-239">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-239">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="dd7e4-240">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-240">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="dd7e4-241">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-241">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="dd7e4-242">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-242">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="dd7e4-243">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="dd7e4-243">app_offline.htm</span></span>

<span data-ttu-id="dd7e4-244">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-244">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="dd7e4-245">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-245">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="dd7e4-246">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-246">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="dd7e4-247">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-247">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="dd7e4-248">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-248">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="dd7e4-249">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-249">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="dd7e4-250">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="dd7e4-250">Start-up error page</span></span>

<span data-ttu-id="dd7e4-251">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-251">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="dd7e4-252">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-252">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-253">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-253">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-254">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-254">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-255">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-255">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="dd7e4-256">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-256">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="dd7e4-257">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="dd7e4-257">Log creation and redirection</span></span>

<span data-ttu-id="dd7e4-258">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-258">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="dd7e4-259">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-259">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-260">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-260">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="dd7e4-261">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-261">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="dd7e4-262">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-262">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="dd7e4-263">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-263">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="dd7e4-264">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-264">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="dd7e4-265">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-265">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="dd7e4-266">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-266">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="dd7e4-267">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-267">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="dd7e4-268">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur ( *.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-268">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="dd7e4-269">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-269">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="dd7e4-270">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-270">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="dd7e4-271">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-271">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="dd7e4-272">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-272">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="dd7e4-273">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="dd7e4-274">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-274">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-275">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-275">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="dd7e4-276">Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-276">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="dd7e4-277">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-277">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="dd7e4-278">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-278">Enhanced diagnostic logs</span></span>

<span data-ttu-id="dd7e4-279">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-279">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="dd7e4-280">`<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-280">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="dd7e4-281">Yoldaki tüm klasörler (önceki örnekteki*Günlükler* ), günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-281">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-282">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-282">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="dd7e4-283">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-283">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="dd7e4-284">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-284">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="dd7e4-285">HATA</span><span class="sxs-lookup"><span data-stu-id="dd7e4-285">ERROR</span></span>
* <span data-ttu-id="dd7e4-286">UYARI</span><span class="sxs-lookup"><span data-stu-id="dd7e4-286">WARNING</span></span>
* <span data-ttu-id="dd7e4-287">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="dd7e4-287">INFO</span></span>
* <span data-ttu-id="dd7e4-288">TRACE</span><span class="sxs-lookup"><span data-stu-id="dd7e4-288">TRACE</span></span>

<span data-ttu-id="dd7e4-289">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-289">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="dd7e4-290">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="dd7e4-290">CONSOLE</span></span>
* <span data-ttu-id="dd7e4-291">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="dd7e4-291">EVENTLOG</span></span>
* <span data-ttu-id="dd7e4-292">DOSYA</span><span class="sxs-lookup"><span data-stu-id="dd7e4-292">FILE</span></span>

<span data-ttu-id="dd7e4-293">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-293">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="dd7e4-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="dd7e4-295">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="dd7e4-295">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="dd7e4-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="dd7e4-297">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-297">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="dd7e4-298">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-298">The size of the log isn't limited.</span></span> <span data-ttu-id="dd7e4-299">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-299">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="dd7e4-300">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-300">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="dd7e4-301">Yığın boyutunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="dd7e4-301">Modify the stack size</span></span>

<span data-ttu-id="dd7e4-302">*Yalnızca işlem içi barındırma modeli kullanılırken geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="dd7e4-302">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="dd7e4-303">Yönetilen yığın boyutunu, *Web. config*dosyasındaki bayt cinsinden `stackSize` ayarını kullanarak yapılandırın. Varsayılan boyut `1048576` bayttır (1 MB).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-303">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="dd7e4-304">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-304">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="dd7e4-305">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="dd7e4-305">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="dd7e4-306">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-306">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="dd7e4-307">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-307">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="dd7e4-308">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-308">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="dd7e4-309">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-309">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="dd7e4-310">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-310">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="dd7e4-311">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-311">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="dd7e4-312">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-312">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="dd7e4-313">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-313">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="dd7e4-314">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-314">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="dd7e4-315">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-315">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="dd7e4-316">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-316">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="dd7e4-317">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-317">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="dd7e4-318">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-318">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="dd7e4-319">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-319">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="dd7e4-320">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-320">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="dd7e4-321">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-321">Run the installer.</span></span>
1. <span data-ttu-id="dd7e4-322">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-322">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="dd7e4-323">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-323">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="dd7e4-324">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-324">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="dd7e4-325">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-325">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="dd7e4-326">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-326">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="dd7e4-327">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-327">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="dd7e4-328">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-328">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="dd7e4-329">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-329">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="dd7e4-330">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-330">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="dd7e4-331">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="dd7e4-331">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="dd7e4-332">Modülü</span><span class="sxs-lookup"><span data-stu-id="dd7e4-332">Module</span></span>

<span data-ttu-id="dd7e4-333">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-333">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-334">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-334">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-336">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-336">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="dd7e4-337">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-337">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="dd7e4-338">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-338">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-340">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-341">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="dd7e4-342">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-342">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="dd7e4-343">Şema</span><span class="sxs-lookup"><span data-stu-id="dd7e4-343">Schema</span></span>

<span data-ttu-id="dd7e4-344">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-344">**IIS**</span></span>

* <span data-ttu-id="dd7e4-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="dd7e4-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="dd7e4-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="dd7e4-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="dd7e4-347">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-347">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="dd7e4-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="dd7e4-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="dd7e4-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="dd7e4-350">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-350">Configuration</span></span>

<span data-ttu-id="dd7e4-351">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-351">**IIS**</span></span>

* <span data-ttu-id="dd7e4-352">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-352">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="dd7e4-353">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-353">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-354">Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="dd7e4-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="dd7e4-356">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-356">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="dd7e4-357">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-357">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="dd7e4-358">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-358">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="dd7e4-359">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-359">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="dd7e4-360">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-360">Supported Windows versions:</span></span>

* <span data-ttu-id="dd7e4-361">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-361">Windows 7 or later</span></span>
* <span data-ttu-id="dd7e4-362">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-362">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="dd7e4-363">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-363">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="dd7e4-364">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-364">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="dd7e4-365">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-365">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="dd7e4-366">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-366">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="dd7e4-367">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="dd7e4-367">In-process hosting model</span></span>

<span data-ttu-id="dd7e4-368">Bir uygulamayı işlem içi barındırma için yapılandırmak için, `<AspNetCoreHostingModel>` özelliğini uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma `OutOfProcess`olarak ayarlanır) olarak ekleyin:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-368">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="dd7e4-369">İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-369">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="dd7e4-370">`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-370">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="dd7e4-371">`<AspNetCoreHostingModel>` özelliği dosyasında yoksa, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-371">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="dd7e4-372">Aşağıdaki özellikler, işlem içi barındırırken geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-372">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="dd7e4-373">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-373">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="dd7e4-374">İşlem içi için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-374">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="dd7e4-375">`IISHttpServer`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-375">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="dd7e4-376">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-376">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="dd7e4-377">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-377">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="dd7e4-378">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-378">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="dd7e4-379">Uygulamalar arasında bir uygulama havuzu paylaşımı desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-379">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="dd7e4-380">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-380">Use one app pool per app.</span></span>

* <span data-ttu-id="dd7e4-381">Kullanırken [Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) veya el ile yerleştirme bir [app_offline.htm dosyasını dağıtımdaki](xref:host-and-deploy/iis/index#locked-deployment-files), açık bir bağlantı varsa uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-381">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="dd7e4-382">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-382">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="dd7e4-383">Yüklü çalışma zamanı (x64 veya x86) ve uygulama mimarisi (bit) uygulama havuzu mimarisiyle gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-383">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="dd7e4-384">İstemci bağlantısını keser algılanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-384">Client disconnects are detected.</span></span> <span data-ttu-id="dd7e4-385">[HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) istemci kestiğinde iptal belirteci iptal edildi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-385">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="dd7e4-386">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-386">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="dd7e4-387">Uygulamanın geçerli dizin ayarlar örnek kod için bkz: [CurrentDirectoryHelpers sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-387">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="dd7e4-388">Çağrı `SetCurrentDirectory` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-388">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="dd7e4-389">Yapılan sonraki çağrılar <xref:System.IO.Directory.GetCurrentDirectory*> uygulamanın dizinine sağlayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-389">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="dd7e4-390">İşlem içi barındırma sırasında, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> dahili olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-390">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="dd7e4-391">Bu nedenle, her kimlik doğrulaması sonrasında talepleri dönüştürmek için kullanılan bir <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-391">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="dd7e4-392">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulamayla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-392">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="dd7e4-393">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="dd7e4-393">Out-of-process hosting model</span></span>

<span data-ttu-id="dd7e4-394">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-394">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="dd7e4-395">`<AspNetCoreHostingModel>` özelliğini belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-395">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="dd7e4-396">`<AspNetCoreHostingModel>` özelliği dosyasında yoksa, varsayılan değer `OutOfProcess`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-396">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="dd7e4-397">`<AspNetCoreHostingModel>` özelliğinin değerini `OutOfProcess` olarak ayarlayın (işlem içi barındırma, `InProcess`ile ayarlanır):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-397">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="dd7e4-398">Değer büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-398">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="dd7e4-399">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-399">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="dd7e4-400">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) şu <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-400">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="dd7e4-401">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-401">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="dd7e4-402">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-402">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="dd7e4-403">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-403">Hosting model changes</span></span>

<span data-ttu-id="dd7e4-404">Varsa `hostingModel` ayar değiştirildiğinde *web.config* dosya (açıklandığı [web.config yapılandırmasıyla](#configuration-with-webconfig) bölümü), modülü için IIS çalışan işlemi geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-404">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="dd7e4-405">Modülü, IIS Express için çalışan işlemi geri dönüşüm değil ancak bunun yerine geçerli IIS Express işlemi normal şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-405">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="dd7e4-406">Uygulamanın sonraki isteği, IIS Express'in yeni bir işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-406">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="dd7e4-407">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="dd7e4-407">Process name</span></span>

<span data-ttu-id="dd7e4-408">`Process.GetCurrentProcess().ProcessName` raporları `w3wp` / `iisexpress` (işlem içi) veya `dotnet` (giden işlem).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-408">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="dd7e4-409">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-409">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="dd7e4-410">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-410">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="dd7e4-411">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-411">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="dd7e4-412">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-412">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="dd7e4-413">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-413">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="dd7e4-414">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-414">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="dd7e4-415">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="dd7e4-415">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="dd7e4-416">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-416">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="dd7e4-417">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-417">Configuration with web.config</span></span>

<span data-ttu-id="dd7e4-418">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-418">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="dd7e4-419">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-419">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="dd7e4-420">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-420">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="dd7e4-421"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Özelliği `false` ayarları içinde belirtilen belirtmek için [ \<konum >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi olmayan uygulamanın bir alt dizinde bulunan uygulamalar tarafından devralınan.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-421">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="dd7e4-422">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-422">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-423">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-423">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="dd7e4-424">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-424">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="dd7e4-425">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-425">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="dd7e4-426">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="dd7e4-426">Attribute</span></span> | <span data-ttu-id="dd7e4-427">Açıklama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-427">Description</span></span> | <span data-ttu-id="dd7e4-428">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="dd7e4-428">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="dd7e4-429">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-429">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-430">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-430">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="dd7e4-431">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-431">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-432">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-432">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="dd7e4-433">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-433">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-434">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-434">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="dd7e4-435">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-435">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="dd7e4-436">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-436">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-437">Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-437">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="dd7e4-438">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-438">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-439">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-439">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="dd7e4-440">&dagger;İşlem içi barındırmak için değer sınırlı olan `1`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-440">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="dd7e4-441">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-441">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="dd7e4-442">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-442">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="dd7e4-443">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-443">Default: `1`</span></span><br><span data-ttu-id="dd7e4-444">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-444">Min: `1`</span></span><br><span data-ttu-id="dd7e4-445">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="dd7e4-445">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="dd7e4-446">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-446">Required string attribute.</span></span></p><p><span data-ttu-id="dd7e4-447">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-447">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="dd7e4-448">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-448">Relative paths are supported.</span></span> <span data-ttu-id="dd7e4-449">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-449">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="dd7e4-450">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-450">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-451">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-451">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="dd7e4-452">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-452">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="dd7e4-453">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-453">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="dd7e4-454">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-454">Default: `10`</span></span><br><span data-ttu-id="dd7e4-455">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-455">Min: `0`</span></span><br><span data-ttu-id="dd7e4-456">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-456">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="dd7e4-457">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-457">Optional timespan attribute.</span></span></p><p><span data-ttu-id="dd7e4-458">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-458">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="dd7e4-459">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-459">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="dd7e4-460">İşlem içi barındırmak için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-460">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="dd7e4-461">İşlem içi barındırmak için modülü isteği işlemek için uygulamada bekler.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-461">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="dd7e4-462">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-462">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="dd7e4-463">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-463">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="dd7e4-464">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-464">Default: `00:02:00`</span></span><br><span data-ttu-id="dd7e4-465">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-465">Min: `00:00:00`</span></span><br><span data-ttu-id="dd7e4-466">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-466">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="dd7e4-467">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-467">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-468">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-468">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="dd7e4-469">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-469">Default: `10`</span></span><br><span data-ttu-id="dd7e4-470">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-470">Min: `0`</span></span><br><span data-ttu-id="dd7e4-471">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-471">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="dd7e4-472">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-472">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-473">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-473">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="dd7e4-474">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-474">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="dd7e4-475">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-475">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="dd7e4-476">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-476">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="dd7e4-477">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-477">Default: `120`</span></span><br><span data-ttu-id="dd7e4-478">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-478">Min: `0`</span></span><br><span data-ttu-id="dd7e4-479">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-479">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="dd7e4-480">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-480">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-481">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-481">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="dd7e4-482">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-482">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-483">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-483">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="dd7e4-484">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-484">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="dd7e4-485">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-485">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="dd7e4-486">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-486">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-487">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-487">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="dd7e4-488">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-488">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="dd7e4-489">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-489">Setting environment variables</span></span>

<span data-ttu-id="dd7e4-490">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-490">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="dd7e4-491">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-491">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="dd7e4-492">Ortam değişkenleri bu bölümünde ortam değişkenlerini sistem üzerinde önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-492">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="dd7e4-493">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-493">The following example sets two environment variables.</span></span> <span data-ttu-id="dd7e4-494">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-494">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="dd7e4-495">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-495">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="dd7e4-496">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-496">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="dd7e4-497">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-497">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="dd7e4-498">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-498">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="dd7e4-499">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-499">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="dd7e4-500">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="dd7e4-500">app_offline.htm</span></span>

<span data-ttu-id="dd7e4-501">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-501">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="dd7e4-502">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-502">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="dd7e4-503">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-503">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="dd7e4-504">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-504">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="dd7e4-505">Açık bir bağlantı varsa işlem dışı barındırma modeli kullanılırken, uygulamayı hemen kapatılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-505">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="dd7e4-506">Örneğin, bir websocket bağlantısı uygulama kapatma gecikmeye neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-506">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="dd7e4-507">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="dd7e4-507">Start-up error page</span></span>

<span data-ttu-id="dd7e4-508">İşlem içi ve dışı işlem uygulamayı başlatmak başarısız olduğunda ürettiği özel hata sayfaları barındırma.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-508">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="dd7e4-509">Her iki işlem veya işlem dışı istek işleyicisi, bulmak gereken ASP.NET Core modülü başarısız olursa bir *500.0 - içindeki işlem/Out-işlem işleyicisi yükleme hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-509">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-510">Uygulamayı başlatmak gereken ASP.NET Core modülü başarısız olursa, işlem içi barındırma için bir *500.30 - başlangıç hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-510">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-511">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa, barındırma işlemi çıkış için bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-511">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="dd7e4-512">Bu sayfayı gösterme ve varsayılan IIS 5xx durum kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-512">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="dd7e4-513">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-513">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="dd7e4-514">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="dd7e4-514">Log creation and redirection</span></span>

<span data-ttu-id="dd7e4-515">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-515">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="dd7e4-516">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-516">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-517">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-517">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="dd7e4-518">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-518">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="dd7e4-519">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-519">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="dd7e4-520">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-520">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="dd7e4-521">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-521">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="dd7e4-522">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-522">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="dd7e4-523">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-523">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="dd7e4-524">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-524">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="dd7e4-525">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur ( *.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-525">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="dd7e4-526">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-526">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="dd7e4-527">Varsa `stdoutLogEnabled` uygulama başlatma sırasında oluşan hatalar yakalanır ve olay günlüğüne yayılan false ise 30 KB'a kadar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-527">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="dd7e4-528">Başlatma işleminden sonra tüm ek günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-528">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="dd7e4-529">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-529">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="dd7e4-530">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-530">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="dd7e4-531">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-531">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-532">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-532">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="dd7e4-533">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-533">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="dd7e4-534">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-534">Enhanced diagnostic logs</span></span>

<span data-ttu-id="dd7e4-535">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-535">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="dd7e4-536">`<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-536">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="dd7e4-537">`<handlerSetting>` değerine (önceki örnekteki*Günlükler* ) belirtilen yoldaki klasörler, otomatik olarak modül tarafından oluşturulmaz ve dağıtımda önceden var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-537">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="dd7e4-538">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-538">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="dd7e4-539">Hata ayıklama düzeyini (`debugLevel`) hem düzeyine hem de konum değerleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-539">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="dd7e4-540">Düzeyleri (en az sırası için en ayrıntılı):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-540">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="dd7e4-541">HATA</span><span class="sxs-lookup"><span data-stu-id="dd7e4-541">ERROR</span></span>
* <span data-ttu-id="dd7e4-542">UYARI</span><span class="sxs-lookup"><span data-stu-id="dd7e4-542">WARNING</span></span>
* <span data-ttu-id="dd7e4-543">BİLGİLERİ</span><span class="sxs-lookup"><span data-stu-id="dd7e4-543">INFO</span></span>
* <span data-ttu-id="dd7e4-544">TRACE</span><span class="sxs-lookup"><span data-stu-id="dd7e4-544">TRACE</span></span>

<span data-ttu-id="dd7e4-545">(Birden fazla konumda izin verilir) konumları:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-545">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="dd7e4-546">KONSOLU</span><span class="sxs-lookup"><span data-stu-id="dd7e4-546">CONSOLE</span></span>
* <span data-ttu-id="dd7e4-547">OLAY GÜNLÜĞÜ</span><span class="sxs-lookup"><span data-stu-id="dd7e4-547">EVENTLOG</span></span>
* <span data-ttu-id="dd7e4-548">DOSYA</span><span class="sxs-lookup"><span data-stu-id="dd7e4-548">FILE</span></span>

<span data-ttu-id="dd7e4-549">İşleyici ayarları ortam değişkenlerini de sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-549">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="dd7e4-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="dd7e4-551">(Varsayılan: *aspnetcore debug.log*)</span><span class="sxs-lookup"><span data-stu-id="dd7e4-551">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="dd7e4-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="dd7e4-553">Yapmak **değil** hata ayıklama günlüğü dağıtımı için sorun gidermek için gereken süreden etkin bırakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-553">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="dd7e4-554">Günlüğünün boyutu sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-554">The size of the log isn't limited.</span></span> <span data-ttu-id="dd7e4-555">Etkin hata ayıklama günlüğünü bırakarak, kullanılabilir disk alanı tüketebilir ve sunucu veya app service kilitlenme.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-555">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="dd7e4-556">Bkz: [web.config yapılandırmasıyla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-556">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="dd7e4-557">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-557">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="dd7e4-558">*Yalnızca işlem dışı barındırmak için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="dd7e4-558">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="dd7e4-559">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-559">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="dd7e4-560">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-560">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="dd7e4-561">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-561">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="dd7e4-562">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-562">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="dd7e4-563">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-563">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="dd7e4-564">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-564">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="dd7e4-565">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-565">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="dd7e4-566">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-566">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="dd7e4-567">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-567">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="dd7e4-568">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-568">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="dd7e4-569">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-569">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="dd7e4-570">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-570">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="dd7e4-571">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-571">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="dd7e4-572">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-572">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="dd7e4-573">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-573">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="dd7e4-574">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-574">Run the installer.</span></span>
1. <span data-ttu-id="dd7e4-575">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-575">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="dd7e4-576">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-576">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="dd7e4-577">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-577">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="dd7e4-578">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-578">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="dd7e4-579">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-579">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="dd7e4-580">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-580">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="dd7e4-581">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-581">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="dd7e4-582">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-582">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="dd7e4-583">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-583">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="dd7e4-584">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="dd7e4-584">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="dd7e4-585">Modülü</span><span class="sxs-lookup"><span data-stu-id="dd7e4-585">Module</span></span>

<span data-ttu-id="dd7e4-586">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-586">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-587">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-587">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-589">%ProgramFiles%\IIS\Asp.NET çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-589">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="dd7e4-590">% ProgramFiles (x86) %\IIS\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-590">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="dd7e4-591">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-591">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-593">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-594">%ProgramFiles%\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="dd7e4-595">% ProgramFiles (x86) %\IIS Express\Asp.Net çekirdek Module\V2\aspnetcorev2.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-595">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="dd7e4-596">Şema</span><span class="sxs-lookup"><span data-stu-id="dd7e4-596">Schema</span></span>

<span data-ttu-id="dd7e4-597">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-597">**IIS**</span></span>

* <span data-ttu-id="dd7e4-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="dd7e4-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="dd7e4-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.XML</span><span class="sxs-lookup"><span data-stu-id="dd7e4-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="dd7e4-600">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-600">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="dd7e4-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="dd7e4-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="dd7e4-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="dd7e4-603">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-603">Configuration</span></span>

<span data-ttu-id="dd7e4-604">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-604">**IIS**</span></span>

* <span data-ttu-id="dd7e4-605">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-605">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="dd7e4-606">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-606">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-607">Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="dd7e4-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="dd7e4-609">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-609">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dd7e4-610">ASP.NET Core modülü, Web isteklerini arka uca ASP.NET Core uygulamalarına iletmek için IIS ardışık düzenine takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-610">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="dd7e4-611">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-611">Supported Windows versions:</span></span>

* <span data-ttu-id="dd7e4-612">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-612">Windows 7 or later</span></span>
* <span data-ttu-id="dd7e4-613">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-613">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="dd7e4-614">Modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-614">The module only works with Kestrel.</span></span> <span data-ttu-id="dd7e4-615">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-615">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="dd7e4-616">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini de işler.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-616">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="dd7e4-617">Modül, ilk istek ulaştığında ASP.NET Core App işlemini başlatır ve kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-617">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="dd7e4-618">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen IIS 'de işlem içinde çalışan ASP.NET 4. x uygulamaları ile görüldüğü aynı davranıştır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-618">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="dd7e4-619">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-619">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="dd7e4-621">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-621">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="dd7e4-622">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-622">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="dd7e4-623">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-623">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="dd7e4-624">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucuyu `http://localhost:{port}`dinlemek üzere yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-624">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="dd7e4-625">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-625">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="dd7e4-626">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-626">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="dd7e4-627">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-627">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="dd7e4-628">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına bir `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-628">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="dd7e4-629">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-629">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="dd7e4-630">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-630">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="dd7e4-631">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-631">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="dd7e4-632">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi edinmek için bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-632">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="dd7e4-633">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-633">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="dd7e4-634">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-634">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="dd7e4-635">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-635">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="dd7e4-636">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-636">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="dd7e4-637">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="dd7e4-637">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="dd7e4-638">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-638">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="dd7e4-639">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-639">Configuration with web.config</span></span>

<span data-ttu-id="dd7e4-640">ASP.NET Core modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-640">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="dd7e4-641">Aşağıdaki *web.config* dosya için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için gereken ASP.NET Core modülü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-641">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="dd7e4-642">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="dd7e4-642">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="dd7e4-643">Ne zaman bir uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolunu ayarlamak `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-643">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-644">Yol stdout günlüklerine kaydeder *LogFiles* klasörü, bir konumdur otomatik olarak oluşturulan hizmet tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-644">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="dd7e4-645">IIS alt uygulama yapılandırma hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-645">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="dd7e4-646">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-646">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="dd7e4-647">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="dd7e4-647">Attribute</span></span> | <span data-ttu-id="dd7e4-648">Açıklama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-648">Description</span></span> | <span data-ttu-id="dd7e4-649">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="dd7e4-649">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="dd7e4-650">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-650">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-651">Belirtilen yürütülebilir dosya için bağımsız değişkenler **processPath**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-651">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="dd7e4-652">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-652">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-653">TRUE ise **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durumu kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-653">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="dd7e4-654">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-654">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-655">TRUE ise, istek başına 'MS-ASPNETCORE-WINAUTHTOKEN' üst bilgi olarak ASPNETCORE_PORT % üzerinde dinleme alt işlem belirteci iletilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-655">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="dd7e4-656">İstek başına Bu belirteci CloseHandle çağırmak için işlemin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-656">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="dd7e4-657">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-657">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-658">Belirtilen işlem örneklerinin sayısını belirtir **processPath** ayarı hazırladık yedekleme uygulama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-658">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="dd7e4-659">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-659">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="dd7e4-660">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-660">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="dd7e4-661">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-661">Default: `1`</span></span><br><span data-ttu-id="dd7e4-662">En küçük: `1`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-662">Min: `1`</span></span><br><span data-ttu-id="dd7e4-663">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-663">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="dd7e4-664">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-664">Required string attribute.</span></span></p><p><span data-ttu-id="dd7e4-665">HTTP istekleri için dinleme işlemini başlatan yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-665">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="dd7e4-666">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-666">Relative paths are supported.</span></span> <span data-ttu-id="dd7e4-667">Yol ile başlıyorsa `.`, yolun site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-667">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="dd7e4-668">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-668">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-669">Belirtilen işlem sayısını belirtir **processPath** dakika başına kilitlenme izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-669">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="dd7e4-670">Bu sınır aşılırsa, dakikanın geri kalanı için işlem başlatılırken modülü durdurur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-670">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="dd7e4-671">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-671">Default: `10`</span></span><br><span data-ttu-id="dd7e4-672">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-672">Min: `0`</span></span><br><span data-ttu-id="dd7e4-673">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-673">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="dd7e4-674">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-674">Optional timespan attribute.</span></span></p><p><span data-ttu-id="dd7e4-675">ASP.NET Core modülü ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt almayı bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-675">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="dd7e4-676">ASP.NET Core 2.1 veya daha sonra sürüm ile birlikte gelen ASP.NET Core modülü sürümlerinde `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-676">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="dd7e4-677">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-677">Default: `00:02:00`</span></span><br><span data-ttu-id="dd7e4-678">En küçük: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-678">Min: `00:00:00`</span></span><br><span data-ttu-id="dd7e4-679">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-679">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="dd7e4-680">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-680">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-681">Modül düzgün biçimde kapatma çalıştırılabiliri için bekleyeceği saniye cinsinden zaman *app_offline.htm* dosyası algılandı.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-681">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="dd7e4-682">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-682">Default: `10`</span></span><br><span data-ttu-id="dd7e4-683">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-683">Min: `0`</span></span><br><span data-ttu-id="dd7e4-684">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-684">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="dd7e4-685">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-685">Optional integer attribute.</span></span></p><p><span data-ttu-id="dd7e4-686">Modül için yürütülebilir dosya bağlantı noktası üzerinde dinleme işlemini başlatmasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-686">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="dd7e4-687">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-687">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="dd7e4-688">Yeni bir istek alırsa ve uygulamayı başlatmak tarayamaz hale gelen sonraki istekleri işlemi yeniden denemek devam ediyor, işlemi yeniden başlatın dener modülün **rapidFailsPerMinute** son kez sayısı sıralı dakika.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-688">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="dd7e4-689">0 (sıfır) değeri **değil** sonsuz zaman aşımını kabul.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-689">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="dd7e4-690">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-690">Default: `120`</span></span><br><span data-ttu-id="dd7e4-691">En küçük: `0`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-691">Min: `0`</span></span><br><span data-ttu-id="dd7e4-692">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="dd7e4-692">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="dd7e4-693">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-693">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="dd7e4-694">TRUE ise **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyaya yeniden yönlendirilen **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-694">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="dd7e4-695">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-695">Optional string attribute.</span></span></p><p><span data-ttu-id="dd7e4-696">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işlemden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-696">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="dd7e4-697">Site köküne göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-697">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="dd7e4-698">İle başlayan herhangi bir yola `.` olan göreli site kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-698">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="dd7e4-699">Modül bir günlük dosyası oluşturmak için sırayla yolunda sağlanan herhangi bir klasörde bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-699">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="dd7e4-700">Alt çizgi sınırlayıcıları, bir zaman damgası, işlem kimliği ve dosya uzantısı kullanarak ( *.log*) son segmenti eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-700">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="dd7e4-701">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018'de, bir işlem kimliğine sahip 1934 19:41:32 kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-701">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="dd7e4-702">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="dd7e4-702">Setting environment variables</span></span>

<span data-ttu-id="dd7e4-703">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-703">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="dd7e4-704">Bir ortam değişkeni ile belirtin `<environmentVariable>` alt öğesi bir `<environmentVariables>` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-704">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="dd7e4-705">Bu bölümde ayarlanan ortam değişkenleri, aynı ada sahip sistem ortam değişkenleri ile çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-705">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="dd7e4-706">Bir ortam değişkeni hem *Web. config* dosyasında hem de Windows 'un sistem düzeyinde ayarlandıysa, *Web. config* dosyasındaki değer, uygulamanın başlamasını önleyen sistem ortam değişkeni değerine (örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`) eklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-706">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="dd7e4-707">Aşağıdaki örnek, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-707">The following example sets two environment variables.</span></span> <span data-ttu-id="dd7e4-708">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamı yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-708">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="dd7e4-709">Bir geliştirici geçici olarak bu değer ayarlayabilir *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-709">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="dd7e4-710">`CONFIG_DIR` bir kullanıcı tarafından tanımlanan ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kod burada yazmıştır örneğidir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-710">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!WARNING]
> <span data-ttu-id="dd7e4-711">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenine `Development` Internet gibi güvenilmeyen ağlara erişilemez sunucularını test etme ve hazırlama.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-711">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="dd7e4-712">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="dd7e4-712">app_offline.htm</span></span>

<span data-ttu-id="dd7e4-713">Bir dosya varsa adıyla *app_offline.htm* algılandığında, uygulama kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün bir şekilde kapatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-713">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="dd7e4-714">Uygulama içinde tanımlanan saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-714">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="dd7e4-715">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriği yeniden göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-715">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="dd7e4-716">Zaman *app_offline.htm* dosya kaldırılır, uygulamanın sonraki isteği başlatır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-716">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="dd7e4-717">Başlangıç hata sayfası</span><span class="sxs-lookup"><span data-stu-id="dd7e4-717">Start-up error page</span></span>

<span data-ttu-id="dd7e4-718">Arka uç işlemi veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek biçimde başarısız başlatmak gereken ASP.NET Core modülü başarısız olursa bir *502.5 - işlem hatası* durumu kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-718">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="dd7e4-719">Bu sayfayı gösterme ve varsayılan IIS 502 durumu kod sayfasına geri dönmek için kullandığınız `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-719">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="dd7e4-720">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz. [HTTP hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-720">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="dd7e4-722">Günlük oluşturma ve yönlendirme</span><span class="sxs-lookup"><span data-stu-id="dd7e4-722">Log creation and redirection</span></span>

<span data-ttu-id="dd7e4-723">ASP.NET Core modülü, stdout ve stderr konsol çıktısı diske yönlendirir `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-723">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="dd7e4-724">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-724">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="dd7e4-725">Uygulama havuzu günlükleri yazıldığı konumuna yazma erişimi olması gerekir (kullanın `IIS AppPool\<app_pool_name>` yazma izni sağlamak için).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-725">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="dd7e4-726">Günlükleri Döndürülmüş değildir, bu işlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-726">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="dd7e4-727">Barındırma sağlayıcının günlüklerini kullanma disk alanını sınırla sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-727">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="dd7e4-728">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-728">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="dd7e4-729">Stdout günlük genel uygulama günlüğe kaydetme amacıyla kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-729">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="dd7e4-730">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-730">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="dd7e4-731">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-731">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="dd7e4-732">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-732">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="dd7e4-733">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur ( *.log*) son segmenti için `stdoutLogFile` yolu (genellikle *stdout*) alt çizgi ile ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-733">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="dd7e4-734">Varsa `stdoutLogFile` yolu ile sona erer *stdout*, bir PID 19:42:32 2/5/2018'de oluşturulan 1934 ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-734">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="dd7e4-735">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-735">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="dd7e4-736">Uygulama havuzu kullanıcı kimliği sağlanan yol için yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-736">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="dd7e4-737">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-737">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="dd7e4-738">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-738">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="dd7e4-739">Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-739">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="dd7e4-740">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-740">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="dd7e4-741">HTTP protokolü ve eşleştirme simgesi Ara sunucu yapılandırmasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-741">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="dd7e4-742">Kestrel ve ASP.NET Core modülü arasında oluşturulan proxy, HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-742">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="dd7e4-743">Sunucu oturumunu bir konumdan Kestrel ve modül arasındaki trafik gizlice, riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-743">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="dd7e4-744">Eşleşme bir belirteç Kestrel tarafından alınan isteklerden IIS tarafından proxy ve başka bir kaynaktan gelmemiştir güvence altına almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-744">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="dd7e4-745">Eşleştirme belirteç oluşturulur ve bir ortam değişkeninde ayarlayın (`ASPNETCORE_TOKEN`) modülü tarafından.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-745">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="dd7e4-746">Eşleştirme belirteç de bir üstbilgisine ayarlanır (`MS-ASPNETCORE-TOKEN`) proxy kullanan her istekte.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-746">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="dd7e4-747">IIS ara yazılım denetimleri eşleştirme belirteci üstbilgi değeri ortam değişken değerini eşleştiğini doğrulamak için aldığı istek.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-747">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="dd7e4-748">Belirteç değerleri eşleşirse, isteği günlüğe ve reddedildi.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-748">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="dd7e4-749">Eşleştirme belirteci ortam değişkeni ve Kestrel ve modül arasındaki trafik bir konumdan sunucu dışına erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-749">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="dd7e4-750">Eşleştirme belirteç değeri farkında olmadan bir saldırgan IIS Ara yazılımında denetimini atla istekleri gönderemiyor.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-750">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="dd7e4-751">Bir IIS ile ASP.NET Core modülü paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-751">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="dd7e4-752">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-752">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="dd7e4-753">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici paylaşımdaki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-753">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="dd7e4-754">IIS paylaşılan yapılandırmaya kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-754">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="dd7e4-755">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-755">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="dd7e4-756">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-756">Run the installer.</span></span>
1. <span data-ttu-id="dd7e4-757">Güncelleştirilmiş dışarı *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-757">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="dd7e4-758">IIS paylaşılan yapılandırması yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-758">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="dd7e4-759">Modül sürümü ve paket barındırma yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="dd7e4-759">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="dd7e4-760">Yüklü ASP.NET Core modülü sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="dd7e4-760">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="dd7e4-761">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-761">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="dd7e4-762">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-762">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="dd7e4-763">Dosyaya sağ tıklayıp **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-763">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="dd7e4-764">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-764">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="dd7e4-765">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-765">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="dd7e4-766">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="dd7e4-766">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="dd7e4-767">Modülü</span><span class="sxs-lookup"><span data-stu-id="dd7e4-767">Module</span></span>

<span data-ttu-id="dd7e4-768">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-768">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-769">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-769">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="dd7e4-771">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-771">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="dd7e4-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="dd7e4-773">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="dd7e4-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="dd7e4-774">Şema</span><span class="sxs-lookup"><span data-stu-id="dd7e4-774">Schema</span></span>

<span data-ttu-id="dd7e4-775">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-775">**IIS**</span></span>

* <span data-ttu-id="dd7e4-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="dd7e4-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="dd7e4-777">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-777">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="dd7e4-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="dd7e4-779">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dd7e4-779">Configuration</span></span>

<span data-ttu-id="dd7e4-780">**IIS**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-780">**IIS**</span></span>

* <span data-ttu-id="dd7e4-781">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-781">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="dd7e4-782">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="dd7e4-782">**IIS Express**</span></span>

* <span data-ttu-id="dd7e4-783">Visual Studio: {APPLICATION ROOT}\\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="dd7e4-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="dd7e4-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="dd7e4-785">Dosyalar için arama yaparak bulunabilir *aspnetcore* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="dd7e4-785">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dd7e4-786">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dd7e4-786">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <span data-ttu-id="dd7e4-787">[ASP.NET Core modül başvuru kaynağı (ana dal)](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2) &ndash; belirli bir sürümü seçmek için **dal** açılan listesini kullanın (örneğin, `release/3.1`).</span><span class="sxs-lookup"><span data-stu-id="dd7e4-787">[ASP.NET Core Module reference source (master branch)](https://github.com/dotnet/aspnetcore/tree/master/src/Servers/IIS/AspNetCoreModuleV2) &ndash; Use the **Branch** drop down list to select a specific release (for example, `release/3.1`).</span></span>
* <xref:host-and-deploy/iis/modules>
