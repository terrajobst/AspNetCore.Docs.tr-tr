---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamalarını barındırmak için ASP.NET Core modülünü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 917ee462a8f9592120685b53d059a661cb4a7452
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333888"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="4cdda-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="4cdda-103">ASP.NET Core Module</span></span>

<span data-ttu-id="4cdda-104">, [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris](https://github.com/Tratcher)No, [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [adatın kotalik](https://github.com/jkotalik)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="4cdda-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4cdda-105">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="4cdda-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="4cdda-106">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="4cdda-107">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="4cdda-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="4cdda-108">Supported Windows versions:</span></span>

* <span data-ttu-id="4cdda-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-109">Windows 7 or later</span></span>
* <span data-ttu-id="4cdda-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4cdda-111">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4cdda-112">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="4cdda-113">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="4cdda-114">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="4cdda-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="4cdda-115">In-process hosting model</span></span>

<span data-ttu-id="4cdda-116">Uygulamalar, işlem içi barındırma modelinde varsayılan olarak ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cdda-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="4cdda-117">İşlem içi barındırma sırasında aşağıdaki özellikler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="4cdda-118">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="4cdda-119">İşlem içi, [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="4cdda-120">@No__t kaydedin-0.</span><span class="sxs-lookup"><span data-stu-id="4cdda-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="4cdda-121">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="4cdda-122">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="4cdda-123">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="4cdda-124">Uygulama havuzunu uygulamalar arasında paylaşma desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="4cdda-125">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-125">Use one app pool per app.</span></span>

* <span data-ttu-id="4cdda-126">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4cdda-127">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="4cdda-128">Uygulamanın mimarisi (bit genişliği) ve yüklü çalışma zamanının (x64 veya x86) uygulama havuzunun mimarisiyle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="4cdda-129">İstemci bağlantısı kesiliyor algılandı.</span><span class="sxs-lookup"><span data-stu-id="4cdda-129">Client disconnects are detected.</span></span> <span data-ttu-id="4cdda-130">İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="4cdda-131">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*>, uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="4cdda-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="4cdda-132">Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="4cdda-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="4cdda-133">@No__t-0 yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="4cdda-134">@No__t-0 ' a yönelik sonraki çağrılar, uygulamanın dizinini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="4cdda-135">İşlem sırasında barındırırken, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> iç olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="4cdda-136">Bu nedenle, her kimlik doğrulamasından sonra talepleri dönüştürmek için kullanılan <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="4cdda-137">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamasıyla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="4cdda-138">[Web paketi (tek dosya) dağıtımları](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="4cdda-139">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="4cdda-139">Out-of-process hosting model</span></span>

<span data-ttu-id="4cdda-140">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, `<AspNetCoreHostingModel>` özelliğinin değerini proje dosyasında ( *. csproj*) `OutOfProcess` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4cdda-141">İşlem içi barındırma, varsayılan değer olan `InProcess` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="4cdda-142">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4cdda-142">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4cdda-143">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-143">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="4cdda-144">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-144">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="4cdda-145">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-145">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="4cdda-146">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-146">Hosting model changes</span></span>

<span data-ttu-id="4cdda-147">@No__t-0 ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4cdda-147">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="4cdda-148">IIS Express için modül çalışan işlemini geri dönüştürmez, bunun yerine geçerli IIS Express işleminin düzgün bir şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-148">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="4cdda-149">Uygulamaya yönelik bir sonraki istek, yeni bir IIS Express işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-149">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="4cdda-150">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="4cdda-150">Process name</span></span>

<span data-ttu-id="4cdda-151">`Process.GetCurrentProcess().ProcessName` rapor `w3wp` @ no__t-2 @ no__t-3 (işlem içi) veya `dotnet` (işlem dışı).</span><span class="sxs-lookup"><span data-stu-id="4cdda-151">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="4cdda-152">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-152">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4cdda-153">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-153">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4cdda-154">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-154">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4cdda-155">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-155">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4cdda-156">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-156">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4cdda-157">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="4cdda-157">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4cdda-158">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="4cdda-158">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4cdda-159">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4cdda-159">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4cdda-160">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-160">Configuration with web.config</span></span>

<span data-ttu-id="4cdda-161">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-161">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4cdda-162">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-162">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4cdda-163">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-163">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4cdda-164">@No__t-0 özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-164">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="4cdda-165">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-165">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4cdda-166">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-166">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4cdda-167">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-167">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4cdda-168">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-168">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4cdda-169">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="4cdda-169">Attribute</span></span> | <span data-ttu-id="4cdda-170">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4cdda-170">Description</span></span> | <span data-ttu-id="4cdda-171">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="4cdda-171">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4cdda-172">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-172">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-173">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-173">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4cdda-174">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-174">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-175">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-175">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4cdda-176">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-176">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-177">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-177">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4cdda-178">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-178">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="4cdda-179">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-179">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-180">Barındırma modelini işlem içi (`InProcess`) veya işlem dışı (`OutOfProcess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-180">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `InProcess` |
| `processesPerApplication` | <p><span data-ttu-id="4cdda-181">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-181">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-182">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-182">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4cdda-183">&dagger;Işlem içi barındırma Için, değer `1` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-183">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="4cdda-184">@No__t-0 ayarı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-184">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4cdda-185">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="4cdda-185">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4cdda-186">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-186">Default: `1`</span></span><br><span data-ttu-id="4cdda-187">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-187">Min: `1`</span></span><br><span data-ttu-id="4cdda-188">En fazla: `100` @ no__t-1</span><span class="sxs-lookup"><span data-stu-id="4cdda-188">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="4cdda-189">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-189">Required string attribute.</span></span></p><p><span data-ttu-id="4cdda-190">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="4cdda-190">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4cdda-191">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-191">Relative paths are supported.</span></span> <span data-ttu-id="4cdda-192">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-192">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4cdda-193">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-193">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-194">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-194">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4cdda-195">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-195">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="4cdda-196">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-196">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="4cdda-197">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-197">Default: `10`</span></span><br><span data-ttu-id="4cdda-198">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-198">Min: `0`</span></span><br><span data-ttu-id="4cdda-199">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="4cdda-199">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4cdda-200">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-200">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4cdda-201">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-201">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4cdda-202">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-202">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="4cdda-203">İşlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-203">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="4cdda-204">İşlem içi barındırma için modül, uygulamanın isteği işlemesini bekler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-204">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="4cdda-205">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-205">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="4cdda-206">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-206">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="4cdda-207">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-207">Default: `00:02:00`</span></span><br><span data-ttu-id="4cdda-208">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-208">Min: `00:00:00`</span></span><br><span data-ttu-id="4cdda-209">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-209">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4cdda-210">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-210">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-211">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-211">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4cdda-212">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-212">Default: `10`</span></span><br><span data-ttu-id="4cdda-213">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-213">Min: `0`</span></span><br><span data-ttu-id="4cdda-214">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-214">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4cdda-215">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-215">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-216">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-216">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4cdda-217">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="4cdda-217">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4cdda-218">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="4cdda-218">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4cdda-219">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="4cdda-219">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4cdda-220">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="4cdda-220">Default: `120`</span></span><br><span data-ttu-id="4cdda-221">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-221">Min: `0`</span></span><br><span data-ttu-id="4cdda-222">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-222">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4cdda-223">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-223">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-224">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-224">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4cdda-225">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-225">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-226">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-226">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4cdda-227">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-227">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4cdda-228">@No__t-0 ' dan başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-228">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4cdda-229">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-229">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-230">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-230">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4cdda-231">@No__t-0 değeri bir değer olarak sağlanırsa, 2/5/2018 tarihinde işlem 1934 KIMLIĞI ile 19:41:32 ' de kaydedildiğinde, *Günlükler* klasöründe *stdout_20180205194132_1934. log* adlı bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-231">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="4cdda-232">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="4cdda-232">Set environment variables</span></span>

<span data-ttu-id="4cdda-233">@No__t-0 özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-233">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4cdda-234">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-234">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="4cdda-235">Bu bölümde ayarlanan ortam değişkenleri, sistem ortamı değişkenlerine göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-235">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="4cdda-236">Aşağıdaki örnek, *Web. config*dosyasında iki ortam değişkenini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development` ' ye yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-236">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4cdda-237">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-237">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4cdda-238">`CONFIG_DIR`, geliştiricinin uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-238">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="4cdda-239">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği yayımlama profili ( *. pubxml*) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-239">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="4cdda-240">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="4cdda-240">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="4cdda-241">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-241">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4cdda-242">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="4cdda-242">app_offline.htm</span></span>

<span data-ttu-id="4cdda-243">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-243">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4cdda-244">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-244">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4cdda-245">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-245">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4cdda-246">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-246">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="4cdda-247">İşlem dışı barındırma modeli kullanılırken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-247">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4cdda-248">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-248">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4cdda-249">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="4cdda-249">Start-up error page</span></span>

<span data-ttu-id="4cdda-250">Hem işlem içi hem de işlem dışı barındırma, uygulamayı başlatamadıklarında özel hata sayfaları üretir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-250">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="4cdda-251">ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-251">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-252">ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-252">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-253">ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-253">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-254">Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-254">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4cdda-255">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4cdda-255">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4cdda-256">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4cdda-256">Log creation and redirection</span></span>

<span data-ttu-id="4cdda-257">@No__t-2 öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-257">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4cdda-258">@No__t-0 yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-258">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-259">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-259">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-260">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-260">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4cdda-261">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-261">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4cdda-262">Stdout günlüğünün kullanılması yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-262">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4cdda-263">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-263">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4cdda-264">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-264">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4cdda-265">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4cdda-265">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4cdda-266">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-266">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4cdda-267">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-267">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4cdda-268">@No__t-0 yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan bir uygulama için günlük kaydı *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-268">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4cdda-269">@No__t-0 yanlış ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-269">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="4cdda-270">Başlangıçtan sonra tüm ek Günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-270">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="4cdda-271">Bir *Web. config* dosyasındaki aşağıdaki örnek `aspNetCore` öğesi, Azure App Service barındırılan bir uygulama için stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-271">The following sample `aspNetCore` element in a *web.config* file configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4cdda-272">Yerel günlük kaydı için bir yerel yol veya ağ paylaşımının yolu kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-272">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4cdda-273">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="4cdda-274">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-274">Enhanced diagnostic logs</span></span>

<span data-ttu-id="4cdda-275">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-275">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="4cdda-276">@No__t-0 öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. @No__t-3 ' ü `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="4cdda-276">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="4cdda-277">Yoldaki tüm klasörler (önceki örnekteki*Günlükler* ), günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-277">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-278">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-278">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-279">Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-279">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="4cdda-280">Düzeyler (en az ayrıntıdan en fazla ayrıntı sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="4cdda-280">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="4cdda-281">HATAYLA</span><span class="sxs-lookup"><span data-stu-id="4cdda-281">ERROR</span></span>
* <span data-ttu-id="4cdda-282">WARNING</span><span class="sxs-lookup"><span data-stu-id="4cdda-282">WARNING</span></span>
* <span data-ttu-id="4cdda-283">BILGISINE</span><span class="sxs-lookup"><span data-stu-id="4cdda-283">INFO</span></span>
* <span data-ttu-id="4cdda-284">TRACE</span><span class="sxs-lookup"><span data-stu-id="4cdda-284">TRACE</span></span>

<span data-ttu-id="4cdda-285">Konumlar (birden çok konuma izin verilir):</span><span class="sxs-lookup"><span data-stu-id="4cdda-285">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="4cdda-286">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="4cdda-286">CONSOLE</span></span>
* <span data-ttu-id="4cdda-287">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="4cdda-287">EVENTLOG</span></span>
* <span data-ttu-id="4cdda-288">DOSYASÝNÝ</span><span class="sxs-lookup"><span data-stu-id="4cdda-288">FILE</span></span>

<span data-ttu-id="4cdda-289">İşleyici ayarları, ortam değişkenleri aracılığıyla da kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-289">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="4cdda-290">`ASPNETCORE_MODULE_DEBUG_FILE` @no__t-hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="4cdda-290">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="4cdda-291">(Varsayılan: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="4cdda-291">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="4cdda-292">`ASPNETCORE_MODULE_DEBUG` &ndash; hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="4cdda-292">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="4cdda-293">Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.**</span><span class="sxs-lookup"><span data-stu-id="4cdda-293">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="4cdda-294">Günlüğün boyutu sınırlı değil.</span><span class="sxs-lookup"><span data-stu-id="4cdda-294">The size of the log isn't limited.</span></span> <span data-ttu-id="4cdda-295">Hata ayıklama günlüğünün etkin bırakılması, kullanılabilir disk alanını tüketebilir ve sunucu veya App Service 'i kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-295">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="4cdda-296">*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .</span><span class="sxs-lookup"><span data-stu-id="4cdda-296">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="4cdda-297">Yığın boyutunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="4cdda-297">Modify the stack size</span></span>

<span data-ttu-id="4cdda-298">*Yalnızca işlem içi barındırma modeli kullanılırken geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="4cdda-298">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="4cdda-299">Yönetilen yığın boyutunu, *Web. config*dosyasında bayt cinsinden `stackSize` ayarını kullanarak yapılandırın. Varsayılan boyut `1048576` bayttır (1 MB).</span><span class="sxs-lookup"><span data-stu-id="4cdda-299">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4cdda-300">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="4cdda-300">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4cdda-301">*Yalnızca işlem dışı barındırma için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="4cdda-301">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="4cdda-302">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-302">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4cdda-303">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-303">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4cdda-304">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-304">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4cdda-305">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-305">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4cdda-306">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-306">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4cdda-307">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-307">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4cdda-308">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-308">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4cdda-309">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-309">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4cdda-310">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-310">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4cdda-311">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="4cdda-311">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4cdda-312">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-312">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4cdda-313">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="4cdda-313">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4cdda-314">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, ASP.NET Core barındırma paketi yükleyicisini `1` olarak ayarlanmış `OPT_NO_SHARED_CONFIG_CHECK` parametresiyle çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-314">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="4cdda-315">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4cdda-315">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="4cdda-316">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-316">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4cdda-317">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-317">Run the installer.</span></span>
1. <span data-ttu-id="4cdda-318">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-318">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4cdda-319">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-319">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4cdda-320">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-320">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4cdda-321">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="4cdda-321">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4cdda-322">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-322">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4cdda-323">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="4cdda-323">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4cdda-324">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-324">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4cdda-325">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-325">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4cdda-326">Modülün barındırma paketi yükleyici günlükleri *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_Aspnetcoremodupa_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-326">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4cdda-327">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="4cdda-327">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4cdda-328">Modül</span><span class="sxs-lookup"><span data-stu-id="4cdda-328">Module</span></span>

<span data-ttu-id="4cdda-329">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-329">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-330">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-330">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-331">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-332">%ProgramFiles%\IIS\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-332">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4cdda-333">% ProgramFiles (x86)% \ ııs\ ASP.NET Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-333">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="4cdda-334">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-334">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-335">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-336">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-336">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-337">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4cdda-338">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-338">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4cdda-339">Şema</span><span class="sxs-lookup"><span data-stu-id="4cdda-339">Schema</span></span>

<span data-ttu-id="4cdda-340">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-340">**IIS**</span></span>

* <span data-ttu-id="4cdda-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-341">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4cdda-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-342">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="4cdda-343">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-343">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-344">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4cdda-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-345">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4cdda-346">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-346">Configuration</span></span>

<span data-ttu-id="4cdda-347">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-347">**IIS**</span></span>

* <span data-ttu-id="4cdda-348">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-348">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4cdda-349">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-349">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-350">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-350">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4cdda-351">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4cdda-351">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4cdda-352">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-352">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="4cdda-353">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="4cdda-353">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="4cdda-354">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-354">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="4cdda-355">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-355">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="4cdda-356">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="4cdda-356">Supported Windows versions:</span></span>

* <span data-ttu-id="4cdda-357">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-357">Windows 7 or later</span></span>
* <span data-ttu-id="4cdda-358">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-358">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4cdda-359">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-359">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4cdda-360">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-360">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="4cdda-361">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-361">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="4cdda-362">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-362">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="4cdda-363">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="4cdda-363">In-process hosting model</span></span>

<span data-ttu-id="4cdda-364">Bir uygulamayı işlem içi barındırma için yapılandırmak için, `<AspNetCoreHostingModel>` özelliğini uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma `OutOfProcess` ile ayarlanır) ile birlikte ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4cdda-364">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4cdda-365">İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-365">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="4cdda-366">Dosyada `<AspNetCoreHostingModel>` özelliği yoksa, varsayılan değer `OutOfProcess` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-366">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="4cdda-367">İşlem içi barındırma sırasında aşağıdaki özellikler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-367">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="4cdda-368">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-368">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="4cdda-369">İşlem içi, [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-369">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="4cdda-370">@No__t kaydedin-0.</span><span class="sxs-lookup"><span data-stu-id="4cdda-370">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="4cdda-371">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-371">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="4cdda-372">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-372">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="4cdda-373">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-373">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="4cdda-374">Uygulama havuzunu uygulamalar arasında paylaşma desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-374">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="4cdda-375">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-375">Use one app pool per app.</span></span>

* <span data-ttu-id="4cdda-376">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-376">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4cdda-377">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-377">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="4cdda-378">Uygulamanın mimarisi (bit genişliği) ve yüklü çalışma zamanının (x64 veya x86) uygulama havuzunun mimarisiyle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-378">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="4cdda-379">İstemci bağlantısı kesiliyor algılandı.</span><span class="sxs-lookup"><span data-stu-id="4cdda-379">Client disconnects are detected.</span></span> <span data-ttu-id="4cdda-380">İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-380">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="4cdda-381">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*>, uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="4cdda-381">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="4cdda-382">Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="4cdda-382">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="4cdda-383">@No__t-0 yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-383">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="4cdda-384">@No__t-0 ' a yönelik sonraki çağrılar, uygulamanın dizinini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-384">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="4cdda-385">İşlem sırasında barındırırken, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> iç olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-385">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="4cdda-386">Bu nedenle, her kimlik doğrulamasından sonra talepleri dönüştürmek için kullanılan <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-386">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="4cdda-387">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamasıyla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-387">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="4cdda-388">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="4cdda-388">Out-of-process hosting model</span></span>

<span data-ttu-id="4cdda-389">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-389">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="4cdda-390">@No__t-0 özelliğini belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-390">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="4cdda-391">Dosyada `<AspNetCoreHostingModel>` özelliği yoksa, varsayılan değer `OutOfProcess` ' dir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-391">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="4cdda-392">@No__t-0 özelliğinin değerini `OutOfProcess` olarak ayarlayın (işlem içi barındırma `InProcess` ile ayarlanır):</span><span class="sxs-lookup"><span data-stu-id="4cdda-392">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="4cdda-393">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="4cdda-393">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="4cdda-394">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-394">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="4cdda-395">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-395">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="4cdda-396">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-396">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="4cdda-397">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-397">Hosting model changes</span></span>

<span data-ttu-id="4cdda-398">@No__t-0 ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4cdda-398">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="4cdda-399">IIS Express için modül çalışan işlemini geri dönüştürmez, bunun yerine geçerli IIS Express işleminin düzgün bir şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-399">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="4cdda-400">Uygulamaya yönelik bir sonraki istek, yeni bir IIS Express işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-400">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="4cdda-401">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="4cdda-401">Process name</span></span>

<span data-ttu-id="4cdda-402">`Process.GetCurrentProcess().ProcessName` rapor `w3wp` @ no__t-2 @ no__t-3 (işlem içi) veya `dotnet` (işlem dışı).</span><span class="sxs-lookup"><span data-stu-id="4cdda-402">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="4cdda-403">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-403">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4cdda-404">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-404">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4cdda-405">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-405">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4cdda-406">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-406">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4cdda-407">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-407">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4cdda-408">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="4cdda-408">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4cdda-409">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="4cdda-409">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4cdda-410">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4cdda-410">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4cdda-411">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-411">Configuration with web.config</span></span>

<span data-ttu-id="4cdda-412">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-412">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4cdda-413">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-413">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4cdda-414">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-414">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4cdda-415">@No__t-0 özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-415">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="4cdda-416">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-416">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4cdda-417">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-417">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4cdda-418">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-418">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4cdda-419">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-419">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4cdda-420">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="4cdda-420">Attribute</span></span> | <span data-ttu-id="4cdda-421">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4cdda-421">Description</span></span> | <span data-ttu-id="4cdda-422">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="4cdda-422">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4cdda-423">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-423">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-424">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-424">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4cdda-425">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-425">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-426">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-426">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4cdda-427">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-427">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-428">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-428">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4cdda-429">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-429">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="4cdda-430">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-430">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-431">Barındırma modelini işlem içi (`InProcess`) veya işlem dışı (`OutOfProcess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-431">Specifies the hosting model as in-process (`InProcess`) or out-of-process (`OutOfProcess`).</span></span></p> | `OutOfProcess` |
| `processesPerApplication` | <p><span data-ttu-id="4cdda-432">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-432">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-433">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-433">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4cdda-434">&dagger;Işlem içi barındırma Için, değer `1` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-434">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="4cdda-435">@No__t-0 ayarı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-435">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4cdda-436">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="4cdda-436">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4cdda-437">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-437">Default: `1`</span></span><br><span data-ttu-id="4cdda-438">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-438">Min: `1`</span></span><br><span data-ttu-id="4cdda-439">En fazla: `100` @ no__t-1</span><span class="sxs-lookup"><span data-stu-id="4cdda-439">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="4cdda-440">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-440">Required string attribute.</span></span></p><p><span data-ttu-id="4cdda-441">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="4cdda-441">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4cdda-442">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-442">Relative paths are supported.</span></span> <span data-ttu-id="4cdda-443">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-443">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4cdda-444">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-444">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-445">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-445">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4cdda-446">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-446">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="4cdda-447">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-447">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="4cdda-448">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-448">Default: `10`</span></span><br><span data-ttu-id="4cdda-449">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-449">Min: `0`</span></span><br><span data-ttu-id="4cdda-450">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="4cdda-450">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4cdda-451">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-451">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4cdda-452">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-452">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4cdda-453">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-453">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="4cdda-454">İşlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="4cdda-454">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="4cdda-455">İşlem içi barındırma için modül, uygulamanın isteği işlemesini bekler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-455">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="4cdda-456">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-456">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="4cdda-457">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-457">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="4cdda-458">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-458">Default: `00:02:00`</span></span><br><span data-ttu-id="4cdda-459">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-459">Min: `00:00:00`</span></span><br><span data-ttu-id="4cdda-460">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-460">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4cdda-461">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-461">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-462">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-462">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4cdda-463">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-463">Default: `10`</span></span><br><span data-ttu-id="4cdda-464">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-464">Min: `0`</span></span><br><span data-ttu-id="4cdda-465">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-465">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4cdda-466">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-466">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-467">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-467">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4cdda-468">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="4cdda-468">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4cdda-469">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="4cdda-469">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4cdda-470">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="4cdda-470">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4cdda-471">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="4cdda-471">Default: `120`</span></span><br><span data-ttu-id="4cdda-472">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-472">Min: `0`</span></span><br><span data-ttu-id="4cdda-473">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-473">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4cdda-474">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-474">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-475">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-475">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4cdda-476">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-476">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-477">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-477">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4cdda-478">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-478">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4cdda-479">@No__t-0 ' dan başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-479">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4cdda-480">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-480">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-481">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-481">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4cdda-482">@No__t-0 değeri bir değer olarak sağlanırsa, 2/5/2018 tarihinde işlem 1934 KIMLIĞI ile 19:41:32 ' de kaydedildiğinde, *Günlükler* klasöründe *stdout_20180205194132_1934. log* adlı bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-482">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="4cdda-483">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="4cdda-483">Setting environment variables</span></span>

<span data-ttu-id="4cdda-484">@No__t-0 özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-484">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4cdda-485">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-485">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="4cdda-486">Bu bölümde ayarlanan ortam değişkenleri, sistem ortamı değişkenlerine göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-486">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="4cdda-487">Aşağıdaki örnek iki ortam değişkenini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-487">The following example sets two environment variables.</span></span> <span data-ttu-id="4cdda-488">`ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development` olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-488">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4cdda-489">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-489">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4cdda-490">`CONFIG_DIR`, geliştiricinin uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-490">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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

> [!NOTE]
> <span data-ttu-id="4cdda-491">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği yayımlama profili ( *. pubxml*) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-491">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file.</span></span> <span data-ttu-id="4cdda-492">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="4cdda-492">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="4cdda-493">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-493">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4cdda-494">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="4cdda-494">app_offline.htm</span></span>

<span data-ttu-id="4cdda-495">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-495">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4cdda-496">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-496">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4cdda-497">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-497">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4cdda-498">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-498">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="4cdda-499">İşlem dışı barındırma modeli kullanılırken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-499">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="4cdda-500">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-500">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4cdda-501">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="4cdda-501">Start-up error page</span></span>

<span data-ttu-id="4cdda-502">Hem işlem içi hem de işlem dışı barındırma, uygulamayı başlatamadıklarında özel hata sayfaları üretir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-502">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="4cdda-503">ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-503">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-504">ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-504">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-505">ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-505">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="4cdda-506">Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-506">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4cdda-507">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4cdda-507">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4cdda-508">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4cdda-508">Log creation and redirection</span></span>

<span data-ttu-id="4cdda-509">@No__t-2 öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-509">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4cdda-510">@No__t-0 yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-510">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-511">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-511">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-512">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-512">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4cdda-513">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-513">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4cdda-514">Stdout günlüğünün kullanılması yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-514">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4cdda-515">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-515">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4cdda-516">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-516">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4cdda-517">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4cdda-517">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4cdda-518">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-518">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4cdda-519">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-519">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4cdda-520">@No__t-0 yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan bir uygulama için günlük kaydı *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-520">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4cdda-521">@No__t-0 yanlış ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-521">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="4cdda-522">Başlangıçtan sonra tüm ek Günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-522">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="4cdda-523">Aşağıdaki örnek `aspNetCore` öğesi, Azure App Service barındırılan bir uygulama için stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-523">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4cdda-524">Yerel günlük kaydı için bir yerel yol veya ağ paylaşımının yolu kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-524">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4cdda-525">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-525">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="4cdda-526">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-526">Enhanced diagnostic logs</span></span>

<span data-ttu-id="4cdda-527">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-527">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="4cdda-528">@No__t-0 öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. @No__t-3 ' ü `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="4cdda-528">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="4cdda-529">@No__t-0 değerine (önceki örnekteki*Günlükler* ) belirtilen yoldaki klasörler, modül tarafından otomatik olarak oluşturulmaz ve dağıtımda önceden var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-529">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="4cdda-530">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-530">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-531">Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-531">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="4cdda-532">Düzeyler (en az ayrıntıdan en fazla ayrıntı sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="4cdda-532">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="4cdda-533">HATAYLA</span><span class="sxs-lookup"><span data-stu-id="4cdda-533">ERROR</span></span>
* <span data-ttu-id="4cdda-534">WARNING</span><span class="sxs-lookup"><span data-stu-id="4cdda-534">WARNING</span></span>
* <span data-ttu-id="4cdda-535">BILGISINE</span><span class="sxs-lookup"><span data-stu-id="4cdda-535">INFO</span></span>
* <span data-ttu-id="4cdda-536">TRACE</span><span class="sxs-lookup"><span data-stu-id="4cdda-536">TRACE</span></span>

<span data-ttu-id="4cdda-537">Konumlar (birden çok konuma izin verilir):</span><span class="sxs-lookup"><span data-stu-id="4cdda-537">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="4cdda-538">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="4cdda-538">CONSOLE</span></span>
* <span data-ttu-id="4cdda-539">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="4cdda-539">EVENTLOG</span></span>
* <span data-ttu-id="4cdda-540">DOSYASÝNÝ</span><span class="sxs-lookup"><span data-stu-id="4cdda-540">FILE</span></span>

<span data-ttu-id="4cdda-541">İşleyici ayarları, ortam değişkenleri aracılığıyla da kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-541">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="4cdda-542">`ASPNETCORE_MODULE_DEBUG_FILE` @no__t-hata ayıklama günlük dosyasının yolu.</span><span class="sxs-lookup"><span data-stu-id="4cdda-542">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="4cdda-543">(Varsayılan: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="4cdda-543">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="4cdda-544">`ASPNETCORE_MODULE_DEBUG` &ndash; hata ayıklama düzeyi ayarı.</span><span class="sxs-lookup"><span data-stu-id="4cdda-544">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="4cdda-545">Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.**</span><span class="sxs-lookup"><span data-stu-id="4cdda-545">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="4cdda-546">Günlüğün boyutu sınırlı değil.</span><span class="sxs-lookup"><span data-stu-id="4cdda-546">The size of the log isn't limited.</span></span> <span data-ttu-id="4cdda-547">Hata ayıklama günlüğünün etkin bırakılması, kullanılabilir disk alanını tüketebilir ve sunucu veya App Service 'i kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-547">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="4cdda-548">*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .</span><span class="sxs-lookup"><span data-stu-id="4cdda-548">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4cdda-549">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="4cdda-549">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4cdda-550">*Yalnızca işlem dışı barındırma için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="4cdda-550">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="4cdda-551">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-551">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4cdda-552">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-552">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4cdda-553">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-553">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4cdda-554">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-554">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4cdda-555">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-555">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4cdda-556">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-556">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4cdda-557">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-557">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4cdda-558">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-558">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4cdda-559">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-559">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4cdda-560">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="4cdda-560">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4cdda-561">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-561">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4cdda-562">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="4cdda-562">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4cdda-563">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, ASP.NET Core barındırma paketi yükleyicisini `1` olarak ayarlanmış `OPT_NO_SHARED_CONFIG_CHECK` parametresiyle çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4cdda-563">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="4cdda-564">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4cdda-564">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="4cdda-565">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-565">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4cdda-566">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-566">Run the installer.</span></span>
1. <span data-ttu-id="4cdda-567">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-567">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4cdda-568">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-568">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4cdda-569">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-569">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4cdda-570">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="4cdda-570">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4cdda-571">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-571">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4cdda-572">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="4cdda-572">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4cdda-573">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-573">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4cdda-574">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-574">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4cdda-575">Modülün barındırma paketi yükleyici günlükleri *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_Aspnetcoremodupa_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-575">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4cdda-576">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="4cdda-576">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4cdda-577">Modül</span><span class="sxs-lookup"><span data-stu-id="4cdda-577">Module</span></span>

<span data-ttu-id="4cdda-578">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-578">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-579">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-579">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-580">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-581">%ProgramFiles%\IIS\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-581">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4cdda-582">% ProgramFiles (x86)% \ ııs\ ASP.NET Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-582">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="4cdda-583">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-583">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-584">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-585">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-585">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-586">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="4cdda-587">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-587">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4cdda-588">Şema</span><span class="sxs-lookup"><span data-stu-id="4cdda-588">Schema</span></span>

<span data-ttu-id="4cdda-589">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-589">**IIS**</span></span>

* <span data-ttu-id="4cdda-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-590">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4cdda-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-591">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="4cdda-592">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-592">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-593">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="4cdda-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-594">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4cdda-595">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-595">Configuration</span></span>

<span data-ttu-id="4cdda-596">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-596">**IIS**</span></span>

* <span data-ttu-id="4cdda-597">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-597">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4cdda-598">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-598">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-599">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-599">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4cdda-600">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4cdda-600">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4cdda-601">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-601">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4cdda-602">ASP.NET Core modülü, Web isteklerini arka uca ASP.NET Core uygulamalarına iletmek için IIS ardışık düzenine takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="4cdda-602">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="4cdda-603">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="4cdda-603">Supported Windows versions:</span></span>

* <span data-ttu-id="4cdda-604">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-604">Windows 7 or later</span></span>
* <span data-ttu-id="4cdda-605">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="4cdda-605">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="4cdda-606">Modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-606">The module only works with Kestrel.</span></span> <span data-ttu-id="4cdda-607">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-607">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="4cdda-608">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini de işler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-608">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="4cdda-609">Modül, ilk istek ulaştığında ASP.NET Core App işlemini başlatır ve kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-609">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="4cdda-610">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen IIS 'de işlem içinde çalışan ASP.NET 4. x uygulamaları ile görüldüğü aynı davranıştır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-610">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="4cdda-611">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-611">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="4cdda-613">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-613">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="4cdda-614">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="4cdda-614">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="4cdda-615">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-615">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="4cdda-616">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucusu, `http://localhost:{port}` ' i dinlemek için sunucuyu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-616">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="4cdda-617">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-617">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="4cdda-618">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-618">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="4cdda-619">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-619">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="4cdda-620">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-620">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="4cdda-621">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-621">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="4cdda-622">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-622">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="4cdda-623">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-623">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="4cdda-624">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-624">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="4cdda-625">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="4cdda-625">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="4cdda-626">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-626">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="4cdda-627">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-627">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="4cdda-628">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="4cdda-628">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="4cdda-629">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="4cdda-629">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="4cdda-630">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="4cdda-630">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="4cdda-631">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-631">Configuration with web.config</span></span>

<span data-ttu-id="4cdda-632">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-632">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="4cdda-633">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-633">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="4cdda-634">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="4cdda-634">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="4cdda-635">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-635">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="4cdda-636">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-636">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="4cdda-637">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="4cdda-637">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="4cdda-638">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-638">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="4cdda-639">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="4cdda-639">Attribute</span></span> | <span data-ttu-id="4cdda-640">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4cdda-640">Description</span></span> | <span data-ttu-id="4cdda-641">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="4cdda-641">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="4cdda-642">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-642">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-643">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-643">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="4cdda-644">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-644">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-645">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-645">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="4cdda-646">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-646">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-647">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-647">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="4cdda-648">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-648">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="4cdda-649">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-649">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-650">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-650">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="4cdda-651">@No__t-0 ayarı önerilmez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-651">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="4cdda-652">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="4cdda-652">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="4cdda-653">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-653">Default: `1`</span></span><br><span data-ttu-id="4cdda-654">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="4cdda-654">Min: `1`</span></span><br><span data-ttu-id="4cdda-655">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="4cdda-655">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="4cdda-656">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-656">Required string attribute.</span></span></p><p><span data-ttu-id="4cdda-657">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="4cdda-657">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="4cdda-658">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-658">Relative paths are supported.</span></span> <span data-ttu-id="4cdda-659">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-659">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="4cdda-660">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-660">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-661">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-661">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="4cdda-662">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-662">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="4cdda-663">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-663">Default: `10`</span></span><br><span data-ttu-id="4cdda-664">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-664">Min: `0`</span></span><br><span data-ttu-id="4cdda-665">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="4cdda-665">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="4cdda-666">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-666">Optional timespan attribute.</span></span></p><p><span data-ttu-id="4cdda-667">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-667">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="4cdda-668">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-668">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="4cdda-669">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-669">Default: `00:02:00`</span></span><br><span data-ttu-id="4cdda-670">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-670">Min: `00:00:00`</span></span><br><span data-ttu-id="4cdda-671">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="4cdda-671">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="4cdda-672">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-672">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-673">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-673">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="4cdda-674">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="4cdda-674">Default: `10`</span></span><br><span data-ttu-id="4cdda-675">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-675">Min: `0`</span></span><br><span data-ttu-id="4cdda-676">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-676">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="4cdda-677">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-677">Optional integer attribute.</span></span></p><p><span data-ttu-id="4cdda-678">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="4cdda-678">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="4cdda-679">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="4cdda-679">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="4cdda-680">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="4cdda-680">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="4cdda-681">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="4cdda-681">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="4cdda-682">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="4cdda-682">Default: `120`</span></span><br><span data-ttu-id="4cdda-683">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="4cdda-683">Min: `0`</span></span><br><span data-ttu-id="4cdda-684">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="4cdda-684">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="4cdda-685">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-685">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="4cdda-686">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-686">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="4cdda-687">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4cdda-687">Optional string attribute.</span></span></p><p><span data-ttu-id="4cdda-688">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-688">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="4cdda-689">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-689">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="4cdda-690">@No__t-0 ' dan başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-690">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="4cdda-691">Modülün günlük dosyasını oluşturması için yolda sunulan klasörlerin bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-691">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="4cdda-692">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-692">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="4cdda-693">@No__t-0 değeri bir değer olarak sağlanırsa, 2/5/2018 tarihinde işlem 1934 KIMLIĞI ile 19:41:32 ' de kaydedildiğinde, *Günlükler* klasöründe *stdout_20180205194132_1934. log* adlı bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-693">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="4cdda-694">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="4cdda-694">Setting environment variables</span></span>

<span data-ttu-id="4cdda-695">@No__t-0 özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-695">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="4cdda-696">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-696">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="4cdda-697">Bu bölümde ayarlanan ortam değişkenleri, aynı ada sahip sistem ortam değişkenleri ile çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="4cdda-697">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="4cdda-698">Bir ortam değişkeni hem *Web. config* dosyasında hem de Windows 'un sistem düzeyinde ayarlandıysa, *Web. config* dosyasındaki değer sistem ortam değişkeni değerine (örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`) eklenerek uygulamanın şunlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-698">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="4cdda-699">Aşağıdaki örnek iki ortam değişkenini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-699">The following example sets two environment variables.</span></span> <span data-ttu-id="4cdda-700">`ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development` olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-700">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="4cdda-701">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-701">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="4cdda-702">`CONFIG_DIR`, geliştiricinin uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-702">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="4cdda-703">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-703">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="4cdda-704">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="4cdda-704">app_offline.htm</span></span>

<span data-ttu-id="4cdda-705">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-705">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="4cdda-706">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-706">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="4cdda-707">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="4cdda-707">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="4cdda-708">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-708">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="4cdda-709">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="4cdda-709">Start-up error page</span></span>

<span data-ttu-id="4cdda-710">ASP.NET Core modülü arka uç işlemini başlatamaz veya arka uç işlemi başlatılır, ancak yapılandırılmış bağlantı noktasında dinleme başarısız olursa, *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-710">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="4cdda-711">Bu sayfayı bastırın ve varsayılan IIS 502 durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-711">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="4cdda-712">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="4cdda-712">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502,5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="4cdda-714">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="4cdda-714">Log creation and redirection</span></span>

<span data-ttu-id="4cdda-715">@No__t-2 öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-715">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="4cdda-716">@No__t-0 yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-716">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="4cdda-717">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-717">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-718">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-718">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="4cdda-719">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-719">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="4cdda-720">Stdout günlüğünün kullanılması yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-720">Using the stdout log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="4cdda-721">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-721">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="4cdda-722">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-722">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4cdda-723">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4cdda-723">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="4cdda-724">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-724">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="4cdda-725">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-725">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="4cdda-726">@No__t-0 yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan bir uygulama için günlük kaydı *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-726">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="4cdda-727">Aşağıdaki örnek `aspNetCore` öğesi, Azure App Service barındırılan bir uygulama için stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-727">The following sample `aspNetCore` element configures stdout logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="4cdda-728">Yerel günlük kaydı için bir yerel yol veya ağ paylaşımının yolu kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-728">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="4cdda-729">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-729">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="4cdda-730">@No__t-0 değerine (önceki örnekteki*Günlükler* ) belirtilen yoldaki klasörler, modül tarafından otomatik olarak oluşturulmaz ve dağıtımda önceden var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-730">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="4cdda-731">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="4cdda-731">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="4cdda-732">*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .</span><span class="sxs-lookup"><span data-stu-id="4cdda-732">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="4cdda-733">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="4cdda-733">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="4cdda-734">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-734">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="4cdda-735">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="4cdda-735">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="4cdda-736">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-736">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="4cdda-737">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-737">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="4cdda-738">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-738">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="4cdda-739">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="4cdda-739">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="4cdda-740">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-740">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="4cdda-741">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-741">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="4cdda-742">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="4cdda-742">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="4cdda-743">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="4cdda-743">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="4cdda-744">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-744">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="4cdda-745">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="4cdda-745">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="4cdda-746">IIS paylaşılan yapılandırması kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4cdda-746">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="4cdda-747">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-747">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="4cdda-748">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-748">Run the installer.</span></span>
1. <span data-ttu-id="4cdda-749">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="4cdda-749">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="4cdda-750">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-750">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="4cdda-751">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="4cdda-751">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="4cdda-752">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="4cdda-752">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="4cdda-753">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-753">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="4cdda-754">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="4cdda-754">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="4cdda-755">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="4cdda-755">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="4cdda-756">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4cdda-756">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="4cdda-757">Modülün barındırma paketi yükleyici günlükleri *C: \\Users @ no__t-2% username% \\AppData @ no__t-4Local @ no__t-5Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__ @ no__t-7timestamp > _000_Aspnetcoremodupa_x64. log*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4cdda-757">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="4cdda-758">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="4cdda-758">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="4cdda-759">Modül</span><span class="sxs-lookup"><span data-stu-id="4cdda-759">Module</span></span>

<span data-ttu-id="4cdda-760">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-760">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-761">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-761">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-762">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="4cdda-763">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="4cdda-763">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="4cdda-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-764">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="4cdda-765">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="4cdda-765">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="4cdda-766">Şema</span><span class="sxs-lookup"><span data-stu-id="4cdda-766">Schema</span></span>

<span data-ttu-id="4cdda-767">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-767">**IIS**</span></span>

* <span data-ttu-id="4cdda-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-768">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="4cdda-769">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-769">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="4cdda-770">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="4cdda-771">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4cdda-771">Configuration</span></span>

<span data-ttu-id="4cdda-772">**ISS**</span><span class="sxs-lookup"><span data-stu-id="4cdda-772">**IIS**</span></span>

* <span data-ttu-id="4cdda-773">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-773">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="4cdda-774">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="4cdda-774">**IIS Express**</span></span>

* <span data-ttu-id="4cdda-775">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="4cdda-775">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="4cdda-776">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="4cdda-776">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="4cdda-777">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4cdda-777">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4cdda-778">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4cdda-778">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="4cdda-779">ASP.NET Core Module GitHub deposu (başvuru kaynağı)</span><span class="sxs-lookup"><span data-stu-id="4cdda-779">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
