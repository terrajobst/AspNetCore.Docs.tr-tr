---
title: ASP.NET Core Modülü
author: guardrex
description: ASP.NET Core uygulamalarını barındırmak için ASP.NET Core modülünü yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c9bbd36b8a55b837f6d78abf99215c5496895a39
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799407"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="8b80a-103">ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="8b80a-103">ASP.NET Core Module</span></span>

<span data-ttu-id="8b80a-104">, [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris](https://github.com/Tratcher)No, [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [adatın kotalik](https://github.com/jkotalik)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="8b80a-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), [Justin Kotalik](https://github.com/jkotalik), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8b80a-105">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="8b80a-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="8b80a-106">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-106">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="8b80a-107">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-107">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="8b80a-108">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="8b80a-108">Supported Windows versions:</span></span>

* <span data-ttu-id="8b80a-109">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-109">Windows 7 or later</span></span>
* <span data-ttu-id="8b80a-110">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-110">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="8b80a-111">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-111">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="8b80a-112">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-112">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="8b80a-113">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-113">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="8b80a-114">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-114">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="8b80a-115">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="8b80a-115">In-process hosting model</span></span>

<span data-ttu-id="8b80a-116">Uygulamalar, işlem içi barındırma modelinde varsayılan olarak ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b80a-116">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="8b80a-117">İşlem içi barındırma sırasında aşağıdaki özellikler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-117">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="8b80a-118">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-118">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="8b80a-119">İşlem içi, [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-119">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="8b80a-120">`IISHttpServer`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-120">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="8b80a-121">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-121">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="8b80a-122">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-122">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="8b80a-123">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-123">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="8b80a-124">Uygulama havuzunu uygulamalar arasında paylaşma desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-124">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="8b80a-125">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-125">Use one app pool per app.</span></span>

* <span data-ttu-id="8b80a-126">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-126">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="8b80a-127">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-127">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="8b80a-128">Uygulamanın mimarisi (bit genişliği) ve yüklü çalışma zamanının (x64 veya x86) uygulama havuzunun mimarisiyle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-128">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="8b80a-129">İstemci bağlantısı kesiliyor algılandı.</span><span class="sxs-lookup"><span data-stu-id="8b80a-129">Client disconnects are detected.</span></span> <span data-ttu-id="8b80a-130">İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-130">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="8b80a-131">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*>, uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="8b80a-131">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="8b80a-132">Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="8b80a-132">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="8b80a-133">`SetCurrentDirectory` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-133">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="8b80a-134"><xref:System.IO.Directory.GetCurrentDirectory*> sonraki çağrılar, uygulamanın dizinini sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-134">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="8b80a-135">İşlem sırasında barındırırken, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> iç olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-135">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="8b80a-136">Bu nedenle, her kimlik doğrulamasından sonra talepleri dönüştürmek için kullanılan <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-136">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="8b80a-137">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamasıyla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-137">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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
  
  * <span data-ttu-id="8b80a-138">[Web paketi (tek dosya) dağıtımları](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-138">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="8b80a-139">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="8b80a-139">Out-of-process hosting model</span></span>

<span data-ttu-id="8b80a-140">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, `<AspNetCoreHostingModel>` özelliğinin değerini proje dosyasında ( *. csproj*) `OutOfProcess` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-140">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (*.csproj*):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="8b80a-141">İşlem içi barındırma, varsayılan değer olan `InProcess` ile ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-141">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="8b80a-142">`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-142">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="8b80a-143">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="8b80a-143">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="8b80a-144">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-144">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="8b80a-145">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-145">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="8b80a-146">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-146">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="8b80a-147">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-147">Hosting model changes</span></span>

<span data-ttu-id="8b80a-148">`hostingModel` ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="8b80a-148">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="8b80a-149">IIS Express için modül çalışan işlemini geri dönüştürmez, bunun yerine geçerli IIS Express işleminin düzgün bir şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-149">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="8b80a-150">Uygulamaya yönelik bir sonraki istek, yeni bir IIS Express işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-150">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="8b80a-151">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="8b80a-151">Process name</span></span>

<span data-ttu-id="8b80a-152">`Process.GetCurrentProcess().ProcessName` rapor `w3wp`/`iisexpress` (işlem içi) veya `dotnet` (işlem dışı).</span><span class="sxs-lookup"><span data-stu-id="8b80a-152">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="8b80a-153">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-153">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="8b80a-154">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-154">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="8b80a-155">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-155">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="8b80a-156">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-156">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="8b80a-157">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-157">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="8b80a-158">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="8b80a-158">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="8b80a-159">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="8b80a-159">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="8b80a-160">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="8b80a-160">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="8b80a-161">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-161">Configuration with web.config</span></span>

<span data-ttu-id="8b80a-162">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-162">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="8b80a-163">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-163">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="8b80a-164">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-164">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="8b80a-165"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-165">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="8b80a-166">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-166">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-167">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-167">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="8b80a-168">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-168">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="8b80a-169">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-169">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="8b80a-170">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="8b80a-170">Attribute</span></span> | <span data-ttu-id="8b80a-171">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8b80a-171">Description</span></span> | <span data-ttu-id="8b80a-172">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="8b80a-172">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="8b80a-173">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-173">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-174">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-174">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="8b80a-175">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-175">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-176">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-176">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="8b80a-177">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-177">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-178">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-178">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="8b80a-179">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-179">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="8b80a-180">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-180">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-181">Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-181">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p><span data-ttu-id="8b80a-182">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-182">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-183">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-183">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="8b80a-184">&dagger;Işlem içi barındırma Için, değer `1` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-184">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="8b80a-185">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-185">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="8b80a-186">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="8b80a-186">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="8b80a-187">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-187">Default: `1`</span></span><br><span data-ttu-id="8b80a-188">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-188">Min: `1`</span></span><br><span data-ttu-id="8b80a-189">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="8b80a-189">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="8b80a-190">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-190">Required string attribute.</span></span></p><p><span data-ttu-id="8b80a-191">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="8b80a-191">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="8b80a-192">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-192">Relative paths are supported.</span></span> <span data-ttu-id="8b80a-193">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-193">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="8b80a-194">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-194">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-195">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-195">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="8b80a-196">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-196">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="8b80a-197">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-197">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="8b80a-198">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-198">Default: `10`</span></span><br><span data-ttu-id="8b80a-199">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-199">Min: `0`</span></span><br><span data-ttu-id="8b80a-200">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="8b80a-200">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="8b80a-201">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-201">Optional timespan attribute.</span></span></p><p><span data-ttu-id="8b80a-202">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-202">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="8b80a-203">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-203">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="8b80a-204">İşlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-204">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="8b80a-205">İşlem içi barındırma için modül, uygulamanın isteği işlemesini bekler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-205">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="8b80a-206">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-206">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="8b80a-207">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-207">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="8b80a-208">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-208">Default: `00:02:00`</span></span><br><span data-ttu-id="8b80a-209">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-209">Min: `00:00:00`</span></span><br><span data-ttu-id="8b80a-210">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-210">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="8b80a-211">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-211">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-212">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-212">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="8b80a-213">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-213">Default: `10`</span></span><br><span data-ttu-id="8b80a-214">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-214">Min: `0`</span></span><br><span data-ttu-id="8b80a-215">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-215">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="8b80a-216">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-216">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-217">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-217">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="8b80a-218">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="8b80a-218">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="8b80a-219">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="8b80a-219">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="8b80a-220">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="8b80a-220">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="8b80a-221">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="8b80a-221">Default: `120`</span></span><br><span data-ttu-id="8b80a-222">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-222">Min: `0`</span></span><br><span data-ttu-id="8b80a-223">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-223">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="8b80a-224">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-224">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-225">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-225">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="8b80a-226">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-226">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-227">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-227">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="8b80a-228">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-228">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="8b80a-229">`.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-229">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="8b80a-230">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-230">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-231">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-231">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="8b80a-232">`.\logs\stdout` bir değer olarak sağlandıysa, 2/5/2018 işlem 1934 KIMLIĞI ile 19:41:32 ' de tarihinde kaydedildiğinde *Günlükler* klasöründe *stdout_20180205194132_1934. log dosyasına* bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-232">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="8b80a-233">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="8b80a-233">Set environment variables</span></span>

<span data-ttu-id="8b80a-234">`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-234">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="8b80a-235">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-235">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="8b80a-236">Bu bölümde ayarlanan ortam değişkenleri, sistem ortamı değişkenlerine göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-236">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="8b80a-237">Aşağıdaki örnek, *Web. config*dosyasında iki ortam değişkenini ayarlar. `ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-237">The following example sets two environment variables in *web.config*. `ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="8b80a-238">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-238">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="8b80a-239">`CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-239">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="8b80a-240">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-240">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="8b80a-241">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8b80a-241">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="8b80a-242">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-242">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="8b80a-243">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="8b80a-243">app_offline.htm</span></span>

<span data-ttu-id="8b80a-244">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-244">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="8b80a-245">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-245">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="8b80a-246">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-246">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="8b80a-247">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-247">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="8b80a-248">İşlem dışı barındırma modeli kullanılırken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-248">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="8b80a-249">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-249">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="8b80a-250">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="8b80a-250">Start-up error page</span></span>

<span data-ttu-id="8b80a-251">Hem işlem içi hem de işlem dışı barındırma, uygulamayı başlatamadıklarında özel hata sayfaları üretir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-251">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="8b80a-252">ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-252">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-253">ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-253">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-254">ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-254">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-255">Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-255">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="8b80a-256">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="8b80a-256">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="8b80a-257">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="8b80a-257">Log creation and redirection</span></span>

<span data-ttu-id="8b80a-258">`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-258">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="8b80a-259">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-259">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-260">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="8b80a-260">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="8b80a-261">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-261">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="8b80a-262">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-262">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="8b80a-263">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-263">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="8b80a-264">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-264">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="8b80a-265">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-265">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="8b80a-266">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="8b80a-266">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="8b80a-267">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-267">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="8b80a-268">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-268">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="8b80a-269">`stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosyası *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-269">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="8b80a-270">`stdoutLogEnabled` false ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-270">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="8b80a-271">Başlangıçtan sonra tüm ek Günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-271">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="8b80a-272">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-272">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="8b80a-273">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-273">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="8b80a-274">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-274">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-275">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-275">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="8b80a-276">Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-276">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="8b80a-277">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="8b80a-277">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="8b80a-278">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-278">Enhanced diagnostic logs</span></span>

<span data-ttu-id="8b80a-279">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-279">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="8b80a-280">`<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="8b80a-280">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="8b80a-281">Yoldaki tüm klasörler (önceki örnekteki*Günlükler* ), günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-281">Any folders in the path (*logs* in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-282">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="8b80a-282">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="8b80a-283">Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-283">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="8b80a-284">Düzeyler (en az ayrıntıdan en fazla ayrıntı sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="8b80a-284">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="8b80a-285">HATAYLA</span><span class="sxs-lookup"><span data-stu-id="8b80a-285">ERROR</span></span>
* <span data-ttu-id="8b80a-286">WARNING</span><span class="sxs-lookup"><span data-stu-id="8b80a-286">WARNING</span></span>
* <span data-ttu-id="8b80a-287">BILGISINE</span><span class="sxs-lookup"><span data-stu-id="8b80a-287">INFO</span></span>
* <span data-ttu-id="8b80a-288">TRACE</span><span class="sxs-lookup"><span data-stu-id="8b80a-288">TRACE</span></span>

<span data-ttu-id="8b80a-289">Konumlar (birden çok konuma izin verilir):</span><span class="sxs-lookup"><span data-stu-id="8b80a-289">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="8b80a-290">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="8b80a-290">CONSOLE</span></span>
* <span data-ttu-id="8b80a-291">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="8b80a-291">EVENTLOG</span></span>
* <span data-ttu-id="8b80a-292">DOSYASÝNÝ</span><span class="sxs-lookup"><span data-stu-id="8b80a-292">FILE</span></span>

<span data-ttu-id="8b80a-293">İşleyici ayarları, ortam değişkenleri aracılığıyla da kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-293">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="8b80a-294">hata ayıklama günlük dosyasının &ndash; yolunu `ASPNETCORE_MODULE_DEBUG_FILE`.</span><span class="sxs-lookup"><span data-stu-id="8b80a-294">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="8b80a-295">(Varsayılan: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="8b80a-295">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="8b80a-296">Hata ayıklama düzeyi ayarını &ndash; `ASPNETCORE_MODULE_DEBUG`.</span><span class="sxs-lookup"><span data-stu-id="8b80a-296">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="8b80a-297">Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.**</span><span class="sxs-lookup"><span data-stu-id="8b80a-297">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="8b80a-298">Günlüğün boyutu sınırlı değil.</span><span class="sxs-lookup"><span data-stu-id="8b80a-298">The size of the log isn't limited.</span></span> <span data-ttu-id="8b80a-299">Hata ayıklama günlüğünün etkin bırakılması, kullanılabilir disk alanını tüketebilir ve sunucu veya App Service 'i kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-299">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="8b80a-300">*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .</span><span class="sxs-lookup"><span data-stu-id="8b80a-300">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="8b80a-301">Yığın boyutunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="8b80a-301">Modify the stack size</span></span>

<span data-ttu-id="8b80a-302">*Yalnızca işlem içi barındırma modeli kullanılırken geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="8b80a-302">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="8b80a-303">Yönetilen yığın boyutunu, *Web. config*dosyasında bayt cinsinden `stackSize` ayarını kullanarak yapılandırın. Varsayılan boyut `1048576` bayttır (1 MB).</span><span class="sxs-lookup"><span data-stu-id="8b80a-303">Configure the managed stack size using the `stackSize` setting in bytes in *web.config*. The default size is `1048576` bytes (1 MB).</span></span>

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

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="8b80a-304">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="8b80a-304">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="8b80a-305">*Yalnızca işlem dışı barındırma için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="8b80a-305">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="8b80a-306">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-306">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="8b80a-307">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-307">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="8b80a-308">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-308">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="8b80a-309">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-309">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="8b80a-310">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-310">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="8b80a-311">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-311">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="8b80a-312">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-312">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="8b80a-313">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-313">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="8b80a-314">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-314">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="8b80a-315">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="8b80a-315">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="8b80a-316">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-316">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="8b80a-317">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="8b80a-317">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="8b80a-318">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-318">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="8b80a-319">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="8b80a-319">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="8b80a-320">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-320">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="8b80a-321">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-321">Run the installer.</span></span>
1. <span data-ttu-id="8b80a-322">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-322">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="8b80a-323">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-323">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="8b80a-324">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-324">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="8b80a-325">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="8b80a-325">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="8b80a-326">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-326">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="8b80a-327">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="8b80a-327">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="8b80a-328">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-328">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="8b80a-329">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-329">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="8b80a-330">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_Aspnetcoremodupa_x64. log*olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-330">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="8b80a-331">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="8b80a-331">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="8b80a-332">Modül</span><span class="sxs-lookup"><span data-stu-id="8b80a-332">Module</span></span>

<span data-ttu-id="8b80a-333">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-333">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-334">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-334">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-335">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-336">%ProgramFiles%\IIS\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-336">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="8b80a-337">% ProgramFiles (x86)% \ ııs\ ASP.NET Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-337">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="8b80a-338">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-338">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-339">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-340">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-340">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-341">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="8b80a-342">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-342">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="8b80a-343">Şema</span><span class="sxs-lookup"><span data-stu-id="8b80a-343">Schema</span></span>

<span data-ttu-id="8b80a-344">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-344">**IIS**</span></span>

* <span data-ttu-id="8b80a-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-345">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="8b80a-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-346">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="8b80a-347">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-347">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-348">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="8b80a-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-349">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="8b80a-350">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-350">Configuration</span></span>

<span data-ttu-id="8b80a-351">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-351">**IIS**</span></span>

* <span data-ttu-id="8b80a-352">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-352">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="8b80a-353">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-353">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-354">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-354">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="8b80a-355">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="8b80a-355">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="8b80a-356">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-356">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="8b80a-357">ASP.NET Core modülü, IIS ardışık düzenine şu şekilde takılan yerel bir IIS modülüdür:</span><span class="sxs-lookup"><span data-stu-id="8b80a-357">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="8b80a-358">[İşlem içi barındırma modeli](#in-process-hosting-model)olarak adlandırılan IIS çalışan işleminin (`w3wp.exe`) içinde bir ASP.NET Core uygulaması barındırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-358">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="8b80a-359">Web isteklerini, [işlem dışı barındırma modeli](#out-of-process-hosting-model)olarak adlandırılan [Kestrel sunucusunu](xref:fundamentals/servers/kestrel)çalıştıran bir arka uç ASP.NET Core uygulamasına iletin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-359">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="8b80a-360">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="8b80a-360">Supported Windows versions:</span></span>

* <span data-ttu-id="8b80a-361">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-361">Windows 7 or later</span></span>
* <span data-ttu-id="8b80a-362">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-362">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="8b80a-363">İşlem içi barındırma sırasında, modül IIS HTTP sunucusu (`IISHttpServer`) adlı IIS için işlem içi sunucu uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-363">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="8b80a-364">İşlem dışı barındırma sırasında modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-364">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="8b80a-365">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-365">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="8b80a-366">Barındırma modelleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-366">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="8b80a-367">İşlem içi barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="8b80a-367">In-process hosting model</span></span>

<span data-ttu-id="8b80a-368">Bir uygulamayı işlem içi barındırma için yapılandırmak için, `<AspNetCoreHostingModel>` özelliğini uygulamanın proje dosyasına `InProcess` (işlem dışı barındırma `OutOfProcess` ile ayarlanır) ile birlikte ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8b80a-368">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="8b80a-369">İşlem içi barındırma modeli, .NET Framework hedef ASP.NET Core uygulamalar için desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-369">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="8b80a-370">`<AspNetCoreHostingModel>` değeri büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-370">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="8b80a-371">Dosyada `<AspNetCoreHostingModel>` özelliği yoksa, varsayılan değer `OutOfProcess` ' dir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-371">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="8b80a-372">İşlem içi barındırma sırasında aşağıdaki özellikler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-372">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="8b80a-373">[Kestrel](xref:fundamentals/servers/kestrel) Server yerıne IIS HTTP sunucusu (`IISHttpServer`) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-373">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="8b80a-374">İşlem içi, [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-374">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="8b80a-375">`IISHttpServer`kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-375">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="8b80a-376">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-376">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="8b80a-377">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-377">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="8b80a-378">[RequestTimeout özniteliği](#attributes-of-the-aspnetcore-element) işlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-378">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="8b80a-379">Uygulama havuzunu uygulamalar arasında paylaşma desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-379">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="8b80a-380">Uygulama başına bir uygulama havuzu kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-380">Use one app pool per app.</span></span>

* <span data-ttu-id="8b80a-381">[Web dağıtımı](/iis/publish/using-web-deploy/introduction-to-web-deploy) kullanırken veya dağıtımda el ile bir [app_offline. htm dosyası](xref:host-and-deploy/iis/index#locked-deployment-files)yerleştirilirken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-381">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="8b80a-382">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-382">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="8b80a-383">Uygulamanın mimarisi (bit genişliği) ve yüklü çalışma zamanının (x64 veya x86) uygulama havuzunun mimarisiyle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-383">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="8b80a-384">İstemci bağlantısı kesiliyor algılandı.</span><span class="sxs-lookup"><span data-stu-id="8b80a-384">Client disconnects are detected.</span></span> <span data-ttu-id="8b80a-385">İstemci bağlantısı kesildiğinde [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) iptal belirteci iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-385">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="8b80a-386">ASP.NET Core 2.2.1 veya önceki sürümlerde, <xref:System.IO.Directory.GetCurrentDirectory*>, uygulamanın dizini yerine IIS tarafından başlatılan işlemin çalışan dizinini döndürür (örneğin, *W3wp. exe*için *c:\Windows\System32\inetsrv* ).</span><span class="sxs-lookup"><span data-stu-id="8b80a-386">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="8b80a-387">Uygulamanın geçerli dizinini ayarlayan örnek kod için bkz. [Currentdirectoryyardımcıları sınıfı](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="8b80a-387">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="8b80a-388">`SetCurrentDirectory` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-388">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="8b80a-389"><xref:System.IO.Directory.GetCurrentDirectory*> sonraki çağrılar, uygulamanın dizinini sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-389">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="8b80a-390">İşlem sırasında barındırırken, bir kullanıcıyı başlatmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> iç olarak çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-390">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="8b80a-391">Bu nedenle, her kimlik doğrulamasından sonra talepleri dönüştürmek için kullanılan <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> bir uygulama varsayılan olarak etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-391">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="8b80a-392">Talepler <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> uygulamasıyla dönüştürülürken, kimlik doğrulama hizmetleri eklemek için <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-392">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

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

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="8b80a-393">İşlem dışı barındırma modeli</span><span class="sxs-lookup"><span data-stu-id="8b80a-393">Out-of-process hosting model</span></span>

<span data-ttu-id="8b80a-394">Bir uygulamayı işlem dışı barındırmak üzere yapılandırmak için, proje dosyasında aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-394">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="8b80a-395">`<AspNetCoreHostingModel>` özelliğini belirtmeyin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-395">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="8b80a-396">Dosyada `<AspNetCoreHostingModel>` özelliği yoksa, varsayılan değer `OutOfProcess` ' dir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-396">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="8b80a-397">`<AspNetCoreHostingModel>` özelliğinin değerini `OutOfProcess` olarak ayarlayın (işlem içi barındırma, `InProcess`ile ayarlanır):</span><span class="sxs-lookup"><span data-stu-id="8b80a-397">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="8b80a-398">Değer büyük/küçük harfe duyarlıdır, bu nedenle `inprocess` ve `outofprocess` geçerli değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-398">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="8b80a-399">[Kestrel](xref:fundamentals/servers/kestrel) sunucusu IIS HTTP sunucusu yerine kullanılır (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="8b80a-399">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="8b80a-400">İşlem dışı için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-400">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="8b80a-401">ASP.NET Core modülünün arkasında çalışırken sunucunun dinlemesi gereken bağlantı noktasını ve temel yolu yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-401">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="8b80a-402">Konağı, başlatma hatalarını yakalamak üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-402">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="8b80a-403">Barındırma modeli değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-403">Hosting model changes</span></span>

<span data-ttu-id="8b80a-404">`hostingModel` ayarı *Web. config* dosyasında değiştirilirse ( [Web. config ile yapılandırma](#configuration-with-webconfig) bölümünde AÇıKLANMıŞTıR), modül IIS için çalışan işlemini geri dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="8b80a-404">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="8b80a-405">IIS Express için modül çalışan işlemini geri dönüştürmez, bunun yerine geçerli IIS Express işleminin düzgün bir şekilde kapatılmasını tetikler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-405">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="8b80a-406">Uygulamaya yönelik bir sonraki istek, yeni bir IIS Express işlem olarak çoğaltılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-406">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="8b80a-407">İşlem adı</span><span class="sxs-lookup"><span data-stu-id="8b80a-407">Process name</span></span>

<span data-ttu-id="8b80a-408">`Process.GetCurrentProcess().ProcessName` rapor `w3wp`/`iisexpress` (işlem içi) veya `dotnet` (işlem dışı).</span><span class="sxs-lookup"><span data-stu-id="8b80a-408">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="8b80a-409">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-409">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="8b80a-410">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-410">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="8b80a-411">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-411">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="8b80a-412">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-412">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="8b80a-413">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-413">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="8b80a-414">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="8b80a-414">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="8b80a-415">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="8b80a-415">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="8b80a-416">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="8b80a-416">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="8b80a-417">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-417">Configuration with web.config</span></span>

<span data-ttu-id="8b80a-418">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-418">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="8b80a-419">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-419">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="8b80a-420">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-420">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="8b80a-421"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> özelliği, [\<location >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) öğesi içinde belirtilen ayarların uygulamanın bir alt dizininde bulunan uygulamalar tarafından devralınmadığını göstermek için `false` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-421">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="8b80a-422">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-422">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-423">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-423">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="8b80a-424">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-424">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="8b80a-425">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-425">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="8b80a-426">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="8b80a-426">Attribute</span></span> | <span data-ttu-id="8b80a-427">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8b80a-427">Description</span></span> | <span data-ttu-id="8b80a-428">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="8b80a-428">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="8b80a-429">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-429">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-430">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-430">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="8b80a-431">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-431">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-432">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-432">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="8b80a-433">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-433">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-434">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-434">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="8b80a-435">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-435">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="8b80a-436">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-436">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-437">Barındırma modelini işlem içi (`InProcess`/`inprocess`) veya işlem dışı (`OutOfProcess`/`outofprocess`) olarak belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-437">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="8b80a-438">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-438">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-439">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-439">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="8b80a-440">&dagger;Işlem içi barındırma Için, değer `1` ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-440">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="8b80a-441">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-441">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="8b80a-442">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="8b80a-442">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="8b80a-443">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-443">Default: `1`</span></span><br><span data-ttu-id="8b80a-444">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-444">Min: `1`</span></span><br><span data-ttu-id="8b80a-445">En fazla: `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="8b80a-445">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="8b80a-446">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-446">Required string attribute.</span></span></p><p><span data-ttu-id="8b80a-447">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="8b80a-447">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="8b80a-448">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-448">Relative paths are supported.</span></span> <span data-ttu-id="8b80a-449">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-449">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="8b80a-450">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-450">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-451">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-451">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="8b80a-452">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-452">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="8b80a-453">İşlem içi barındırma ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-453">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="8b80a-454">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-454">Default: `10`</span></span><br><span data-ttu-id="8b80a-455">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-455">Min: `0`</span></span><br><span data-ttu-id="8b80a-456">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="8b80a-456">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="8b80a-457">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-457">Optional timespan attribute.</span></span></p><p><span data-ttu-id="8b80a-458">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-458">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="8b80a-459">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-459">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="8b80a-460">İşlem içi barındırma için uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="8b80a-460">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="8b80a-461">İşlem içi barındırma için modül, uygulamanın isteği işlemesini bekler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-461">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="8b80a-462">Dizenin dakika ve saniye kesimleri için geçerli değerler 0-59 aralığındadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-462">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="8b80a-463">Dakika veya saniye değerindeki **60** kullanımı, *500-iç sunucu hatasına*neden olur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-463">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="8b80a-464">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-464">Default: `00:02:00`</span></span><br><span data-ttu-id="8b80a-465">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-465">Min: `00:00:00`</span></span><br><span data-ttu-id="8b80a-466">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-466">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="8b80a-467">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-467">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-468">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-468">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="8b80a-469">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-469">Default: `10`</span></span><br><span data-ttu-id="8b80a-470">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-470">Min: `0`</span></span><br><span data-ttu-id="8b80a-471">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-471">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="8b80a-472">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-472">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-473">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-473">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="8b80a-474">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="8b80a-474">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="8b80a-475">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="8b80a-475">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="8b80a-476">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="8b80a-476">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="8b80a-477">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="8b80a-477">Default: `120`</span></span><br><span data-ttu-id="8b80a-478">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-478">Min: `0`</span></span><br><span data-ttu-id="8b80a-479">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-479">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="8b80a-480">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-480">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-481">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-481">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="8b80a-482">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-482">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-483">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-483">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="8b80a-484">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-484">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="8b80a-485">`.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-485">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="8b80a-486">Yolda sunulan klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-486">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-487">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-487">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="8b80a-488">`.\logs\stdout` bir değer olarak sağlandıysa, 2/5/2018 işlem 1934 KIMLIĞI ile 19:41:32 ' de tarihinde kaydedildiğinde *Günlükler* klasöründe *stdout_20180205194132_1934. log dosyasına* bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-488">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="8b80a-489">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="8b80a-489">Setting environment variables</span></span>

<span data-ttu-id="8b80a-490">`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-490">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="8b80a-491">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-491">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="8b80a-492">Bu bölümde ayarlanan ortam değişkenleri, sistem ortamı değişkenlerine göre önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-492">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="8b80a-493">Aşağıdaki örnek iki ortam değişkenini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-493">The following example sets two environment variables.</span></span> <span data-ttu-id="8b80a-494">`ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-494">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="8b80a-495">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-495">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="8b80a-496">`CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-496">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="8b80a-497">Ortamı doğrudan *Web. config* içinde ayarlamaya alternatif olarak, `<EnvironmentName>` özelliği [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-497">An alternative to setting the environment directly in *web.config* is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="8b80a-498">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="8b80a-498">This approach sets the environment in *web.config* when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="8b80a-499">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-499">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="8b80a-500">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="8b80a-500">app_offline.htm</span></span>

<span data-ttu-id="8b80a-501">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-501">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="8b80a-502">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-502">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="8b80a-503">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-503">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="8b80a-504">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-504">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

<span data-ttu-id="8b80a-505">İşlem dışı barındırma modeli kullanılırken, açık bir bağlantı varsa uygulama hemen kapanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-505">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="8b80a-506">Örneğin, bir WebSocket bağlantısı, uygulamanın kapatılmasını erteleyebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-506">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="8b80a-507">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="8b80a-507">Start-up error page</span></span>

<span data-ttu-id="8b80a-508">Hem işlem içi hem de işlem dışı barındırma, uygulamayı başlatamadıklarında özel hata sayfaları üretir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-508">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="8b80a-509">ASP.NET Core modülü işlem içi veya işlem dışı istek işleyicisini bulamazsa, *500,0-işlem içi/işlem dışı Işleyici yükleme hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-509">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-510">ASP.NET Core modülü uygulamayı başlatamadığında işlem içi barındırma için, *500,30-başlatma hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-510">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-511">ASP.NET Core modülü arka uç işlemini başlatamadığında veya arka uç işlemi başlatılırsa ancak yapılandırılmış bağlantı noktasında dinleyemediğinde, işlem dışı barındırma için *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-511">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="8b80a-512">Bu sayfayı bastırın ve varsayılan IIS 5xx durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-512">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="8b80a-513">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="8b80a-513">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="8b80a-514">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="8b80a-514">Log creation and redirection</span></span>

<span data-ttu-id="8b80a-515">`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-515">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="8b80a-516">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-516">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-517">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="8b80a-517">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="8b80a-518">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-518">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="8b80a-519">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-519">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="8b80a-520">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-520">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="8b80a-521">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-521">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="8b80a-522">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-522">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="8b80a-523">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="8b80a-523">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="8b80a-524">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-524">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="8b80a-525">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-525">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="8b80a-526">`stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosyası *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-526">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="8b80a-527">`stdoutLogEnabled` false ise, uygulama başlangıcında oluşan hatalar yakalanır ve 30 KB 'a kadar olay günlüğüne yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-527">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="8b80a-528">Başlangıçtan sonra tüm ek Günlükler atılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-528">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="8b80a-529">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-529">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="8b80a-530">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-530">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="8b80a-531">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-531">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-532">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-532">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="8b80a-533">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="8b80a-533">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="8b80a-534">Gelişmiş tanılama günlükleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-534">Enhanced diagnostic logs</span></span>

<span data-ttu-id="8b80a-535">ASP.NET Core modülü, gelişmiş tanılama günlükleri sağlamak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-535">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="8b80a-536">`<handlerSettings>` öğesini *Web. config*içindeki `<aspNetCore>` öğesine ekleyin. `debugLevel` `TRACE` olarak ayarlamak, tanılama bilgilerini daha yüksek bir şekilde kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="8b80a-536">Add the `<handlerSettings>` element to the `<aspNetCore>` element in *web.config*. Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

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

<span data-ttu-id="8b80a-537">`<handlerSetting>` değerine (önceki örnekteki*Günlükler* ) belirtilen yoldaki klasörler, otomatik olarak modül tarafından oluşturulmaz ve dağıtımda önceden var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-537">Folders in the path provided to the `<handlerSetting>` value (*logs* in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="8b80a-538">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="8b80a-538">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="8b80a-539">Hata ayıklama düzeyi (`debugLevel`) değerleri hem düzeyi hem de konumu içerebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-539">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="8b80a-540">Düzeyler (en az ayrıntıdan en fazla ayrıntı sırasına göre):</span><span class="sxs-lookup"><span data-stu-id="8b80a-540">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="8b80a-541">HATAYLA</span><span class="sxs-lookup"><span data-stu-id="8b80a-541">ERROR</span></span>
* <span data-ttu-id="8b80a-542">WARNING</span><span class="sxs-lookup"><span data-stu-id="8b80a-542">WARNING</span></span>
* <span data-ttu-id="8b80a-543">BILGISINE</span><span class="sxs-lookup"><span data-stu-id="8b80a-543">INFO</span></span>
* <span data-ttu-id="8b80a-544">TRACE</span><span class="sxs-lookup"><span data-stu-id="8b80a-544">TRACE</span></span>

<span data-ttu-id="8b80a-545">Konumlar (birden çok konuma izin verilir):</span><span class="sxs-lookup"><span data-stu-id="8b80a-545">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="8b80a-546">KONSOLA</span><span class="sxs-lookup"><span data-stu-id="8b80a-546">CONSOLE</span></span>
* <span data-ttu-id="8b80a-547">EVENTLOG</span><span class="sxs-lookup"><span data-stu-id="8b80a-547">EVENTLOG</span></span>
* <span data-ttu-id="8b80a-548">DOSYASÝNÝ</span><span class="sxs-lookup"><span data-stu-id="8b80a-548">FILE</span></span>

<span data-ttu-id="8b80a-549">İşleyici ayarları, ortam değişkenleri aracılığıyla da kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-549">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="8b80a-550">hata ayıklama günlük dosyasının &ndash; yolunu `ASPNETCORE_MODULE_DEBUG_FILE`.</span><span class="sxs-lookup"><span data-stu-id="8b80a-550">`ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Path to the debug log file.</span></span> <span data-ttu-id="8b80a-551">(Varsayılan: *aspnetcore-Debug. log*)</span><span class="sxs-lookup"><span data-stu-id="8b80a-551">(Default: *aspnetcore-debug.log*)</span></span>
* <span data-ttu-id="8b80a-552">Hata ayıklama düzeyi ayarını &ndash; `ASPNETCORE_MODULE_DEBUG`.</span><span class="sxs-lookup"><span data-stu-id="8b80a-552">`ASPNETCORE_MODULE_DEBUG` &ndash; Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="8b80a-553">Bir sorunu gidermek için dağıtımda hata ayıklama günlüğü 'nün gerekenden uzun süre **etkin bırakmayın.**</span><span class="sxs-lookup"><span data-stu-id="8b80a-553">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="8b80a-554">Günlüğün boyutu sınırlı değil.</span><span class="sxs-lookup"><span data-stu-id="8b80a-554">The size of the log isn't limited.</span></span> <span data-ttu-id="8b80a-555">Hata ayıklama günlüğünün etkin bırakılması, kullanılabilir disk alanını tüketebilir ve sunucu veya App Service 'i kilitlemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-555">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="8b80a-556">*Web. config* dosyasındaki `aspNetCore` öğesinin bir örneği için bkz. [Web. config ile yapılandırma](#configuration-with-webconfig) .</span><span class="sxs-lookup"><span data-stu-id="8b80a-556">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="8b80a-557">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="8b80a-557">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="8b80a-558">*Yalnızca işlem dışı barındırma için geçerlidir.*</span><span class="sxs-lookup"><span data-stu-id="8b80a-558">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="8b80a-559">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-559">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="8b80a-560">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-560">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="8b80a-561">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-561">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="8b80a-562">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-562">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="8b80a-563">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-563">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="8b80a-564">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-564">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="8b80a-565">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-565">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="8b80a-566">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-566">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="8b80a-567">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-567">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="8b80a-568">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="8b80a-568">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="8b80a-569">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-569">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="8b80a-570">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="8b80a-570">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="8b80a-571">IIS yüklemesiyle aynı makinede bir IIS paylaşılan yapılandırması kullanırken, `OPT_NO_SHARED_CONFIG_CHECK` parametresi `1`olarak ayarlanan ASP.NET Core barındırma paketi yükleyicisini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="8b80a-571">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="8b80a-572">Paylaşılan yapılandırmanın yolu IIS yüklemesiyle aynı makinede olmadığında, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="8b80a-572">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="8b80a-573">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-573">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="8b80a-574">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-574">Run the installer.</span></span>
1. <span data-ttu-id="8b80a-575">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-575">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="8b80a-576">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-576">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="8b80a-577">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-577">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="8b80a-578">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="8b80a-578">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="8b80a-579">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-579">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="8b80a-580">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="8b80a-580">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="8b80a-581">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-581">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="8b80a-582">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-582">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="8b80a-583">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_Aspnetcoremodupa_x64. log*olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-583">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="8b80a-584">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="8b80a-584">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="8b80a-585">Modül</span><span class="sxs-lookup"><span data-stu-id="8b80a-585">Module</span></span>

<span data-ttu-id="8b80a-586">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-586">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-587">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-587">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-588">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-589">%ProgramFiles%\IIS\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-589">%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="8b80a-590">% ProgramFiles (x86)% \ ııs\ ASP.NET Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-590">%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

<span data-ttu-id="8b80a-591">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-591">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-592">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-593">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-593">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-594">%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

* <span data-ttu-id="8b80a-595">% ProgramFiles (x86)% \ IIS Express\Asp.Net Core Module\v2\aspnetcorev2,dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-595">%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll</span></span>

### <a name="schema"></a><span data-ttu-id="8b80a-596">Şema</span><span class="sxs-lookup"><span data-stu-id="8b80a-596">Schema</span></span>

<span data-ttu-id="8b80a-597">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-597">**IIS**</span></span>

* <span data-ttu-id="8b80a-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-598">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="8b80a-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-599">%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml</span></span>

<span data-ttu-id="8b80a-600">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-600">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-601">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

* <span data-ttu-id="8b80a-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-602">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="8b80a-603">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-603">Configuration</span></span>

<span data-ttu-id="8b80a-604">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-604">**IIS**</span></span>

* <span data-ttu-id="8b80a-605">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-605">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="8b80a-606">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-606">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-607">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-607">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="8b80a-608">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="8b80a-608">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="8b80a-609">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-609">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8b80a-610">ASP.NET Core modülü, Web isteklerini arka uca ASP.NET Core uygulamalarına iletmek için IIS ardışık düzenine takılan yerel bir IIS modülüdür.</span><span class="sxs-lookup"><span data-stu-id="8b80a-610">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="8b80a-611">Desteklenen Windows sürümleri:</span><span class="sxs-lookup"><span data-stu-id="8b80a-611">Supported Windows versions:</span></span>

* <span data-ttu-id="8b80a-612">Windows 7 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-612">Windows 7 or later</span></span>
* <span data-ttu-id="8b80a-613">Windows Server 2008 R2 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="8b80a-613">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="8b80a-614">Modül yalnızca Kestrel ile birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-614">The module only works with Kestrel.</span></span> <span data-ttu-id="8b80a-615">Modül, [http. sys](xref:fundamentals/servers/httpsys)ile uyumsuzdur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-615">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="8b80a-616">ASP.NET Core uygulamalar IIS çalışan işleminden ayrı bir işlemde çalıştığından, modül işlem yönetimini de işler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-616">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="8b80a-617">Modül, ilk istek ulaştığında ASP.NET Core App işlemini başlatır ve kilitlenirse uygulamayı yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-617">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="8b80a-618">Bu aslında, [Windows Işlem etkinleştirme hizmeti (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was)tarafından yönetilen IIS 'de işlem içinde çalışan ASP.NET 4. x uygulamaları ile görüldüğü aynı davranıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-618">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="8b80a-619">Aşağıdaki diyagramda IIS, ASP.NET Core modülü ve bir uygulama arasındaki ilişki gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-619">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core Modülü](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="8b80a-621">İstekler Web 'den çekirdek modu HTTP. sys sürücüsüne ulaşır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-621">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="8b80a-622">Sürücü, istekleri Web sitesinin yapılandırılmış bağlantı noktasında IIS 'ye yönlendirir, genellikle 80 (HTTP) veya 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="8b80a-622">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="8b80a-623">Modül, 80 veya 443 numaralı bağlantı noktası olmayan uygulama için rastgele bir bağlantı noktasında istekleri Kestrel 'e iletir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-623">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="8b80a-624">Modül, başlangıç sırasında bir ortam değişkeni aracılığıyla bağlantı noktasını belirtir ve [IIS tümleştirme ara](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) sunucusu, `http://localhost:{port}` ' i dinlemek için sunucuyu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-624">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="8b80a-625">Ek denetimler gerçekleştirilir ve modülünden kaynaklanmayan istekler reddedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-625">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="8b80a-626">Modül HTTPS iletmeyi desteklemez, bu nedenle istekler HTTPS üzerinden IIS tarafından alınsa bile HTTP üzerinden iletilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-626">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="8b80a-627">Kestrel, isteği modülden başlattıktan sonra, istek ASP.NET Core ara yazılım ardışık düzenine gönderilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-627">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="8b80a-628">Ara yazılım ardışık düzeni isteği işler ve uygulamanın mantığına `HttpContext` örneği olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-628">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="8b80a-629">IIS tümleştirmesi tarafından eklenen ara yazılım, isteği Kestrel iletmek için düzen, uzak IP ve pathbase 'i hesaba göre güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-629">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="8b80a-630">Uygulamanın yanıtı IIS 'e geri geçirilir ve bu, isteği başlatan HTTP istemcisine geri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-630">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="8b80a-631">Windows kimlik doğrulaması gibi birçok yerel modül etkin kalır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-631">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="8b80a-632">ASP.NET Core modülüyle etkin IIS modülleri hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-632">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="8b80a-633">ASP.NET Core modülü de şunları yapabilir:</span><span class="sxs-lookup"><span data-stu-id="8b80a-633">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="8b80a-634">Çalışan işlem için ortam değişkenlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-634">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="8b80a-635">Başlatma sorunlarını gidermek için stdout çıkışını dosya depolama alanına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-635">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="8b80a-636">Windows kimlik doğrulama belirteçlerini ilet.</span><span class="sxs-lookup"><span data-stu-id="8b80a-636">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="8b80a-637">ASP.NET Core modülünü yüklemek ve kullanmak</span><span class="sxs-lookup"><span data-stu-id="8b80a-637">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="8b80a-638">ASP.NET Core modülünün nasıl yükleneceğine ilişkin yönergeler için bkz. [.NET Core barındırma paketi 'Ni yüklemek](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="8b80a-638">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="8b80a-639">Web. config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-639">Configuration with web.config</span></span>

<span data-ttu-id="8b80a-640">ASP.NET Core modülü, sitenin *Web. config* dosyasındaki `system.webServer` düğümünün `aspNetCore` bölümü ile yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-640">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="8b80a-641">Aşağıdaki *Web. config* dosyası, [çerçeveye bağlı bir dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) Için yayımlanır ve ASP.NET Core modülünü site isteklerini işleyecek şekilde yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-641">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

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

<span data-ttu-id="8b80a-642">Aşağıdaki *Web. config* , [kendinden bağımsız bir dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd)için yayımlanır:</span><span class="sxs-lookup"><span data-stu-id="8b80a-642">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

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

<span data-ttu-id="8b80a-643">Bir uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/)dağıtıldığında `stdoutLogFile` yolu `\\?\%home%\LogFiles\stdout`olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-643">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-644">Yol, stdout günlüklerini hizmet tarafından otomatik olarak oluşturulan bir konum olan *LogFiles* klasörüne kaydeder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-644">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="8b80a-645">IIS alt uygulama yapılandırması hakkında bilgi için bkz. <xref:host-and-deploy/iis/index#sub-applications>.</span><span class="sxs-lookup"><span data-stu-id="8b80a-645">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="8b80a-646">AspNetCore öğesinin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-646">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="8b80a-647">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="8b80a-647">Attribute</span></span> | <span data-ttu-id="8b80a-648">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8b80a-648">Description</span></span> | <span data-ttu-id="8b80a-649">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="8b80a-649">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="8b80a-650">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-650">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-651">**ProcessPath**içinde belirtilen yürütülebilir dosya için bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-651">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="8b80a-652">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-652">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-653">Doğru ise, **502,5-Işlem hatası** sayfası bastırılır ve *Web. config* dosyasında yapılandırılan 502 durum kodu sayfası önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-653">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="8b80a-654">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-654">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-655">True ise belirteç, istek başına ' MS-ASPNETCORE-WıNAUTHTOKEN ' üst bilgisi olarak% ASPNETCORE_PORT% üzerinde dinleme yapan alt işleme iletilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-655">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="8b80a-656">Bu, istek başına bu belirteçte CloseHandle çağırma işleminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-656">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="8b80a-657">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-657">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-658">**ProcessPath** ayarında belirtilen işlemin örnek sayısını, uygulama başına bir şekilde işleyecek şekilde belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-658">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="8b80a-659">`processesPerApplication` ayarlama önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-659">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="8b80a-660">Bu öznitelik gelecek bir sürümde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="8b80a-660">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="8b80a-661">Varsayılan: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-661">Default: `1`</span></span><br><span data-ttu-id="8b80a-662">Min: `1`</span><span class="sxs-lookup"><span data-stu-id="8b80a-662">Min: `1`</span></span><br><span data-ttu-id="8b80a-663">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="8b80a-663">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="8b80a-664">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-664">Required string attribute.</span></span></p><p><span data-ttu-id="8b80a-665">HTTP isteklerini dinleyen bir işlemi başlatan yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="8b80a-665">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="8b80a-666">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-666">Relative paths are supported.</span></span> <span data-ttu-id="8b80a-667">Yol `.` ile başlıyorsa, yol site köküne göreli olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-667">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="8b80a-668">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-668">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-669">**ProcessPath** içinde belirtilen işleme dakika başına kilitlenme için izin verilen sayıyı belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-669">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="8b80a-670">Bu sınır aşılırsa modül, dakika geri kalanı için işlemi başlatmayı durduruyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-670">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="8b80a-671">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-671">Default: `10`</span></span><br><span data-ttu-id="8b80a-672">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-672">Min: `0`</span></span><br><span data-ttu-id="8b80a-673">En fazla: `100`</span><span class="sxs-lookup"><span data-stu-id="8b80a-673">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="8b80a-674">İsteğe bağlı TimeSpan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-674">Optional timespan attribute.</span></span></p><p><span data-ttu-id="8b80a-675">ASP.NET Core modülünün,% ASPNETCORE_PORT% üzerinde dinleme işleminden yanıt beklediği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-675">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="8b80a-676">ASP.NET Core 2,1 veya üzeri sürümü ile birlikte gelen ASP.NET Core modülünün sürümlerinde, `requestTimeout` saat, dakika ve saniye olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-676">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="8b80a-677">Varsayılan: `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-677">Default: `00:02:00`</span></span><br><span data-ttu-id="8b80a-678">Min: `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-678">Min: `00:00:00`</span></span><br><span data-ttu-id="8b80a-679">En fazla: `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="8b80a-679">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="8b80a-680">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-680">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-681">*App_offline. htm* dosyası algılandığında, modülün yürütülebilir dosyanın düzgün şekilde kapatılmasını beklediği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-681">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="8b80a-682">Varsayılan: `10`</span><span class="sxs-lookup"><span data-stu-id="8b80a-682">Default: `10`</span></span><br><span data-ttu-id="8b80a-683">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-683">Min: `0`</span></span><br><span data-ttu-id="8b80a-684">En fazla: `600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-684">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="8b80a-685">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-685">Optional integer attribute.</span></span></p><p><span data-ttu-id="8b80a-686">Modülün, bağlantı noktasında dinleme yapan bir işlemin başlamasını bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="8b80a-686">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="8b80a-687">Bu süre sınırı aşılırsa, modül işlemi bu işlemden sonra da bir kez gider.</span><span class="sxs-lookup"><span data-stu-id="8b80a-687">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="8b80a-688">Modül, yeni bir istek aldığında işlemi yeniden başlatmayı dener ve uygulamanın son geçen dakikada **rapidFailsPerMinute** kez başlayamadığı sürece sonraki gelen isteklerde işlemi yeniden başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="8b80a-688">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="8b80a-689">0 (sıfır) değeri sonsuz bir zaman aşımı olarak kabul **edilmez** .</span><span class="sxs-lookup"><span data-stu-id="8b80a-689">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="8b80a-690">Varsayılan: `120`</span><span class="sxs-lookup"><span data-stu-id="8b80a-690">Default: `120`</span></span><br><span data-ttu-id="8b80a-691">Min: `0`</span><span class="sxs-lookup"><span data-stu-id="8b80a-691">Min: `0`</span></span><br><span data-ttu-id="8b80a-692">En fazla: `3600`</span><span class="sxs-lookup"><span data-stu-id="8b80a-692">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="8b80a-693">İsteğe bağlı Boolean özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-693">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="8b80a-694">True ise, **processPath** içinde belirtilen işlem için **stdout** ve **stderr** , **stdoutLogFile**içinde belirtilen dosyaya yeniden yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-694">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="8b80a-695">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8b80a-695">Optional string attribute.</span></span></p><p><span data-ttu-id="8b80a-696">**ProcessPath** içinde belirtilen işlemden **stdout** ve **stderr** 'in günlüğe kaydedildiği göreli veya mutlak dosya yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-696">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="8b80a-697">Göreli yollar, sitenin köküne göredir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-697">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="8b80a-698">`.` başlayan tüm yollar site köküne göredir ve diğer tüm yollar mutlak yollar olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-698">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="8b80a-699">Modülün günlük dosyasını oluşturması için yolda sunulan klasörlerin bulunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-699">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="8b80a-700">Alt çizgi sınırlayıcılarını kullanma, bir zaman damgası, işlem KIMLIĞI ve dosya uzantısı ( *. log*) **stdoutLogFile** yolunun son kesimine eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-700">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="8b80a-701">`.\logs\stdout` bir değer olarak sağlandıysa, 2/5/2018 işlem 1934 KIMLIĞI ile 19:41:32 ' de tarihinde kaydedildiğinde *Günlükler* klasöründe *stdout_20180205194132_1934. log dosyasına* bir örnek stdout günlüğü kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-701">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="8b80a-702">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="8b80a-702">Setting environment variables</span></span>

<span data-ttu-id="8b80a-703">`processPath` özniteliğinde işlem için ortam değişkenleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-703">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="8b80a-704">Bir `<environmentVariables>` koleksiyon öğesinin `<environmentVariable>` alt öğesiyle bir ortam değişkeni belirtin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-704">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="8b80a-705">Bu bölümde ayarlanan ortam değişkenleri, aynı ada sahip sistem ortam değişkenleri ile çakışıyor.</span><span class="sxs-lookup"><span data-stu-id="8b80a-705">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="8b80a-706">Bir ortam değişkeni hem *Web. config* dosyasında hem de Windows 'un sistem düzeyinde ayarlandıysa, *Web. config* dosyasındaki değer sistem ortam değişkeni değerine (örneğin, `ASPNETCORE_ENVIRONMENT: Development;Development`) eklenerek uygulamanın şunlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-706">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="8b80a-707">Aşağıdaki örnek iki ortam değişkenini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-707">The following example sets two environment variables.</span></span> <span data-ttu-id="8b80a-708">`ASPNETCORE_ENVIRONMENT`, uygulamanın ortamını `Development`olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-708">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="8b80a-709">Bir geliştirici, uygulama özel durumunda hata ayıklarken [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling) yüklemeye zorlamak için bu değeri geçici olarak *Web. config* dosyasında ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-709">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="8b80a-710">`CONFIG_DIR`, geliştiricinin, uygulamanın yapılandırma dosyasını yüklemek için bir yol oluşturmak üzere başlangıçta değeri okuyan kodu yazdığı Kullanıcı tanımlı ortam değişkenine bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-710">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

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
> <span data-ttu-id="8b80a-711">Yalnızca `ASPNETCORE_ENVIRONMENT` ortam değişkenini, Internet gibi güvenilmeyen ağlarla erişilemeyen hazırlama ve test sunucularında `Development` olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-711">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="8b80a-712">app_offline. htm</span><span class="sxs-lookup"><span data-stu-id="8b80a-712">app_offline.htm</span></span>

<span data-ttu-id="8b80a-713">Bir uygulamanın kök dizininde *app_offline. htm* adlı bir dosya algılanırsa, ASP.NET Core modülü uygulamayı düzgün bir şekilde kapatmaya ve gelen istekleri işlemeyi durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-713">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="8b80a-714">Uygulama, `shutdownTimeLimit` ' da tanımlanan saniye sayısından sonra hala çalışıyorsa, ASP.NET Core modülü çalışan işlemi de yok eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-714">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="8b80a-715">*App_offline. htm* dosyası mevcut olsa da ASP.NET Core modülü, istekleri, *app_offline. htm* dosyasının içeriğini geri göndererek yanıtlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-715">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="8b80a-716">*App_offline. htm* dosyası kaldırıldığında, sonraki istek uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-716">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="8b80a-717">Başlatma hatası sayfası</span><span class="sxs-lookup"><span data-stu-id="8b80a-717">Start-up error page</span></span>

<span data-ttu-id="8b80a-718">ASP.NET Core modülü arka uç işlemini başlatamaz veya arka uç işlemi başlatılır, ancak yapılandırılmış bağlantı noktasında dinleme başarısız olursa, *502,5-Işlem hatası* durum kodu sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-718">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="8b80a-719">Bu sayfayı bastırın ve varsayılan IIS 502 durum kodu sayfasına dönmek için `disableStartUpErrorPage` özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-719">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="8b80a-720">Özel hata iletilerini yapılandırma hakkında daha fazla bilgi için bkz. [http hataları \<httpErrors >](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="8b80a-720">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502,5 işlem hatası durum kodu sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="8b80a-722">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="8b80a-722">Log creation and redirection</span></span>

<span data-ttu-id="8b80a-723">`aspNetCore` öğesinin `stdoutLogEnabled` ve `stdoutLogFile` öznitelikleri ayarlandıysa ASP.NET Core modülü stdout ve stderr konsol çıkışını diske yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-723">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="8b80a-724">`stdoutLogFile` yolundaki klasörler, günlük dosyası oluşturulduğunda modül tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-724">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="8b80a-725">Uygulama havuzunun, günlüklerin yazıldığı konuma yazma erişimi olması gerekir (yazma izni sağlamak için `IIS AppPool\<app_pool_name>` kullanın).</span><span class="sxs-lookup"><span data-stu-id="8b80a-725">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="8b80a-726">İşlem geri dönüştürme/yeniden başlatma gerçekleşmediği sürece Günlükler döndürülemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-726">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="8b80a-727">Bu, günlüklerin tükettiği disk alanını sınırlamak için barındırıcının sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-727">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="8b80a-728">Stdout günlüğünün kullanılması yalnızca IIS 'de barındırırken veya [Visual Studio Ile IIS için geliştirme zamanı desteği](xref:host-and-deploy/iis/development-time-iis-support)kullanılırken değil, yerel olarak hata ayıklarken ve uygulamayı IIS Express ile çalıştırırken yalnızca uygulama başlatma sorunlarını gidermek için önerilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-728">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="8b80a-729">Genel uygulama günlüğü amaçları için stdout günlüğünü kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-729">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="8b80a-730">ASP.NET Core uygulamasında rutin günlük kaydı için, günlük dosyası boyutunu sınırlayan ve günlükleri döndüren bir günlüğe kaydetme kitaplığı kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-730">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="8b80a-731">Daha fazla bilgi için bkz. [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="8b80a-731">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="8b80a-732">Günlük dosyası oluşturulduğunda zaman damgası ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-732">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="8b80a-733">Günlük dosyası adı, alt çizgi ile ayrılmış `stdoutLogFile` yolunun (genellikle *stdout*) son kesimine zaman damgası, işlem kimliği ve dosya uzantısı ( *. log*) eklenerek oluşur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-733">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="8b80a-734">`stdoutLogFile` yolu *stdout*ile sonlanıyorsa, 1934 ' de 19:42:32 2/5/2018 ' de oluşturulan PID 'sine sahip bir uygulama için bir günlük dosyası *stdout_20180205194132_1934. log*dosya adına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-734">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="8b80a-735">Aşağıdaki örnek `aspNetCore` öğesi, bir göreli yol `.\log\`stdout günlüğünü yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-735">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="8b80a-736">AppPool Kullanıcı kimliğinin, belirtilen yola yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-736">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="8b80a-737">Azure App Service dağıtım için bir uygulama yayımlarken, Web SDK `stdoutLogFile` değerini `\\?\%home%\LogFiles\stdout`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8b80a-737">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="8b80a-738">`%home` ortam değişkeni, Azure App Service tarafından barındırılan uygulamalar için önceden tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-738">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="8b80a-739">Günlüğe kaydetme filtresi kuralları oluşturmak için ASP.NET Core günlük belgelerinin [yapılandırma](xref:fundamentals/logging/index#log-filtering) ve [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) bölümlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-739">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="8b80a-740">Yol biçimleri hakkında daha fazla bilgi için bkz. [Windows sistemlerinde dosya yolu biçimleri](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="8b80a-740">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="8b80a-741">Proxy yapılandırması HTTP protokolünü ve eşleştirme belirtecini kullanır</span><span class="sxs-lookup"><span data-stu-id="8b80a-741">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="8b80a-742">ASP.NET Core modülü ve Kestrel arasında oluşturulan ara sunucu HTTP protokolünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-742">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="8b80a-743">Modül ve Kestrel arasındaki trafiği sunucu dışı bir konumdan bırakırken gizlice dinleme riski yoktur.</span><span class="sxs-lookup"><span data-stu-id="8b80a-743">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="8b80a-744">Eşleştirme belirteci, Kestrel tarafından alınan isteklerin IIS tarafından proxy aldığından ve başka bir kaynaktan gelmediğinden emin olmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-744">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="8b80a-745">Eşleştirme belirteci oluşturulur ve modül tarafından bir ortam değişkenine (`ASPNETCORE_TOKEN`) ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-745">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="8b80a-746">Eşleştirme belirteci, her proxy isteğinde de bir üst bilgi (`MS-ASPNETCORE-TOKEN`) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-746">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="8b80a-747">IIS ara yazılımı, eşleştirme belirteci üstbilgi değerinin ortam değişkeni değeriyle eşleşip eşleşmediğini doğrulamak için aldığı her isteği denetler.</span><span class="sxs-lookup"><span data-stu-id="8b80a-747">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="8b80a-748">Belirteç değerleri uyuşmadıysa, istek günlüğe kaydedilir ve reddedilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-748">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="8b80a-749">Eşleştirme belirteci ortam değişkeni ve modülle Kestrel arasındaki trafik, sunucu dışında bir konumdan erişilebilir değildir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-749">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="8b80a-750">Eşleştirme belirteç değerini bilmeden, bir saldırgan IIS ara yazılımı 'ndaki denetimi atlayan istekleri gönderemez.</span><span class="sxs-lookup"><span data-stu-id="8b80a-750">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="8b80a-751">IIS paylaşılan yapılandırmasıyla ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="8b80a-751">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="8b80a-752">ASP.NET Core modülü yükleyicisi, **TrustedInstaller** hesabının ayrıcalıklarıyla çalışır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-752">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="8b80a-753">Yerel sistem hesabı, IIS paylaşılan Yapılandırması tarafından kullanılan paylaşım yolu için değiştirme iznine sahip olmadığından, yükleyici, ' deki *ApplicationHost. config* dosyasında modül ayarlarını yapılandırmaya çalışırken bir erişim reddedildi hatası atar. paylaşma.</span><span class="sxs-lookup"><span data-stu-id="8b80a-753">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="8b80a-754">IIS paylaşılan yapılandırması kullanırken, şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="8b80a-754">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="8b80a-755">IIS paylaşılan yapılandırmasını devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-755">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="8b80a-756">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-756">Run the installer.</span></span>
1. <span data-ttu-id="8b80a-757">Güncelleştirilmiş *ApplicationHost. config* dosyasını paylaşıma dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="8b80a-757">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="8b80a-758">IIS paylaşılan yapılandırmasını yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-758">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="8b80a-759">Modül sürümü ve barındırma paketi yükleyici günlükleri</span><span class="sxs-lookup"><span data-stu-id="8b80a-759">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="8b80a-760">Yüklü ASP.NET Core modülünün sürümünü öğrenmek için:</span><span class="sxs-lookup"><span data-stu-id="8b80a-760">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="8b80a-761">Barındırma sisteminde *%windir%\system32\inetsrv dizinine*gidin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-761">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="8b80a-762">*Aspnetcore. dll* dosyasını bulun.</span><span class="sxs-lookup"><span data-stu-id="8b80a-762">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="8b80a-763">Dosyaya sağ tıklayın ve bağlam menüsünden **Özellikler** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="8b80a-763">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="8b80a-764">**Ayrıntılar** sekmesini seçin. **Dosya sürümü** ve **ürün sürümü** , modülün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8b80a-764">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="8b80a-765">Modülün barındırma paketi yükleyici günlükleri, *C:\\kullanıcılar\\% username%\\AppData\\yerel\\Temp*konumunda bulunur. Dosya, *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_Aspnetcoremodupa_x64. log*olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="8b80a-765">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="8b80a-766">Modül, şema ve yapılandırma dosyası konumları</span><span class="sxs-lookup"><span data-stu-id="8b80a-766">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="8b80a-767">Modül</span><span class="sxs-lookup"><span data-stu-id="8b80a-767">Module</span></span>

<span data-ttu-id="8b80a-768">**IIS (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-768">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-769">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-769">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-770">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="8b80a-771">**IIS Express (X86/AMD64):**</span><span class="sxs-lookup"><span data-stu-id="8b80a-771">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="8b80a-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-772">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="8b80a-773">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="8b80a-773">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="8b80a-774">Şema</span><span class="sxs-lookup"><span data-stu-id="8b80a-774">Schema</span></span>

<span data-ttu-id="8b80a-775">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-775">**IIS**</span></span>

* <span data-ttu-id="8b80a-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-776">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="8b80a-777">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-777">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="8b80a-778">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="8b80a-779">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8b80a-779">Configuration</span></span>

<span data-ttu-id="8b80a-780">**ISS**</span><span class="sxs-lookup"><span data-stu-id="8b80a-780">**IIS**</span></span>

* <span data-ttu-id="8b80a-781">%Windir%\System32\inetsrv\config\applicationHost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-781">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="8b80a-782">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="8b80a-782">**IIS Express**</span></span>

* <span data-ttu-id="8b80a-783">Visual Studio: {APPLICATION ROOT} \\. Vs\config\applicationhost,config</span><span class="sxs-lookup"><span data-stu-id="8b80a-783">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="8b80a-784">*ıısexpress. exe* CLI:%USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="8b80a-784">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="8b80a-785">Dosyalar, *ApplicationHost. config* dosyasında *aspnetcore* ' u arayarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="8b80a-785">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8b80a-786">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8b80a-786">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* [<span data-ttu-id="8b80a-787">ASP.NET Core Module GitHub deposu (başvuru kaynağı)</span><span class="sxs-lookup"><span data-stu-id="8b80a-787">ASP.NET Core Module GitHub repository (reference source)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
