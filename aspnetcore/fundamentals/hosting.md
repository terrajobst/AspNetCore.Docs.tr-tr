---
title: "ASP.NET çekirdek barındırma"
author: guardrex
description: "Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin."
keywords: "ASP.NET Core web ana bilgisayarı, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: dfec2a67112d40b528b97c847da3dda8ef1e63bd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="1ae97-104">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="1ae97-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="1ae97-105">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ae97-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1ae97-106">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*, uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu.</span><span class="sxs-lookup"><span data-stu-id="1ae97-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1ae97-107">En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="1ae97-108">Bir konak ayarlıyor</span><span class="sxs-lookup"><span data-stu-id="1ae97-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-109">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1ae97-110">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1ae97-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1ae97-111">Bu genellikle, uygulamanızın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1ae97-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="1ae97-112">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ae97-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1ae97-113">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="1ae97-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="1ae97-114">`CreateDefaultBuilder`Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="1ae97-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="1ae97-115">Yapılandırır [Kestrel](servers/kestrel.md) web sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="1ae97-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="1ae97-116">Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="1ae97-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="1ae97-117">İçerik kök ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1ae97-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="1ae97-118">Gelen isteğe bağlı yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="1ae97-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="1ae97-119">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="1ae97-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="1ae97-120">*appSettings. {Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="1ae97-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1ae97-121">[Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="1ae97-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="1ae97-122">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-122">Environment variables.</span></span>
  * <span data-ttu-id="1ae97-123">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-123">Command-line arguments.</span></span>
* <span data-ttu-id="1ae97-124">Yapılandırır [günlüğü](xref:fundamentals/logging/index) ile konsol ve hata ayıklama çıktısı için [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. {Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1ae97-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="1ae97-125">IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:publishing/iis) temel yolunu ve bağlantı noktası yapılandırarak sunucu, kullanırken dinleyecek [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1ae97-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1ae97-126">Modül IIS ve Kestrel arasında ters proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1ae97-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="1ae97-127">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="1ae97-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="1ae97-128">IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="1ae97-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="1ae97-129">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1ae97-130">Varsayılan içerik kök [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="1ae97-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="1ae97-131">Bu uygulama kök klasörden başlatıldığında web projenin kök klasörü içerik kök olarak kullanarak sonuçları (örneğin, arama [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) proje klasöründen).</span><span class="sxs-lookup"><span data-stu-id="1ae97-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="1ae97-132">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1ae97-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1ae97-133">Bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index) uygulama yapılandırması hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="1ae97-134">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="1ae97-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="1ae97-135">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="1ae97-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-136">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-137">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="1ae97-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="1ae97-138">Bu genellikle, uygulamanızın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1ae97-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="1ae97-139">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ae97-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="1ae97-140">Aşağıdaki *Program.cs* nasıl kullanılacağı ortaya `WebHostBuilder` konak oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="1ae97-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="1ae97-141">`WebHostBuilder`gerektiren bir [IServer uygulayan sunucu](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1ae97-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="1ae97-142">Yerleşik sunucularıdır [Kestrel](servers/kestrel.md) ve [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="1ae97-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="1ae97-143">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="1ae97-144">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="1ae97-145">Varsayılan içerik kök tarafından sağlanan `UseContentRoot` olan [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="1ae97-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="1ae97-146">Bu uygulama kök klasörden başlatıldığında web projenin kök klasörü içerik kök olarak kullanarak sonuçları (örneğin, arama [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) proje klasöründen).</span><span class="sxs-lookup"><span data-stu-id="1ae97-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="1ae97-147">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="1ae97-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="1ae97-148">IIS ters proxy kullanmak için arama [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="1ae97-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="1ae97-149">`UseIISIntegration`yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="1ae97-150">`UseIISIntegration`Taban yol ve bağlantı noktası sunucusunun dinlemesi gereken kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters proxy oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="1ae97-151">IIS ile ASP.NET Core kullanmak için her ikisini de belirtmelisiniz `UseKestrel` ve `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="1ae97-152">`UseIISIntegration`yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="1ae97-153">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1ae97-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="1ae97-154">Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:</span><span class="sxs-lookup"><span data-stu-id="1ae97-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="1ae97-155">Bir konak ayarlıyor durumlarda sağlayabilir [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="1ae97-156">Belirtirseniz bir `Startup` sınıfı gerekir tanımlamanız bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1ae97-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="1ae97-157">Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](startup.md).</span><span class="sxs-lookup"><span data-stu-id="1ae97-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="1ae97-158">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ae97-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="1ae97-159">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1ae97-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1ae97-160">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="1ae97-160">Host configuration values</span></span>

<span data-ttu-id="1ae97-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar için hangi ile doğrudan da ayarlanabilir çoğu kullanılabilir yapılandırma değerlerini ayarlamak için yöntemler sağlar [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="1ae97-162">Bir değerle ayarlarken `UseSetting`, değer bir dize (olarak tırnak işaretleri) türünden bağımsız olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="1ae97-163">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="1ae97-163">Capture Startup Errors</span></span>

<span data-ttu-id="1ae97-164">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="1ae97-165">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1ae97-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1ae97-166">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="1ae97-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1ae97-167">**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1ae97-168">**Kullanılarak ayarlanan**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="1ae97-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="1ae97-169">Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1ae97-170">Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-171">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-172">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="1ae97-173">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="1ae97-173">Content Root</span></span>

<span data-ttu-id="1ae97-174">Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="1ae97-175">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1ae97-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="1ae97-176">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-176">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-177">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="1ae97-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1ae97-178">**Kullanılarak ayarlanan**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1ae97-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="1ae97-179">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="1ae97-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="1ae97-180">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1ae97-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-181">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-182">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="1ae97-183">Ayrıntılı hataları</span><span class="sxs-lookup"><span data-stu-id="1ae97-183">Detailed Errors</span></span>

<span data-ttu-id="1ae97-184">Ayrıntılı hataları yakalanan belirler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="1ae97-185">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="1ae97-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1ae97-186">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="1ae97-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1ae97-187">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="1ae97-187">**Default**: false</span></span>  
<span data-ttu-id="1ae97-188">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1ae97-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1ae97-189">Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-190">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-191">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="1ae97-192">Ortam</span><span class="sxs-lookup"><span data-stu-id="1ae97-192">Environment</span></span>

<span data-ttu-id="1ae97-193">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-193">Sets the app's environment.</span></span>

<span data-ttu-id="1ae97-194">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="1ae97-194">**Key**: environment</span></span>  
<span data-ttu-id="1ae97-195">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-195">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-196">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="1ae97-196">**Default**: Production</span></span>  
<span data-ttu-id="1ae97-197">**Kullanılarak ayarlanan**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1ae97-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="1ae97-198">Ayarlayabileceğiniz *ortamı* için herhangi bir değer.</span><span class="sxs-lookup"><span data-stu-id="1ae97-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="1ae97-199">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1ae97-200">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-200">Values aren't case sensitive.</span></span> <span data-ttu-id="1ae97-201">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="1ae97-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="1ae97-202">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1ae97-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="1ae97-203">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="1ae97-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-204">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-205">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="1ae97-206">Barındırma başlangıç derlemeler</span><span class="sxs-lookup"><span data-stu-id="1ae97-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="1ae97-207">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="1ae97-208">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1ae97-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1ae97-209">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-209">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-210">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="1ae97-210">**Default**: Empty string</span></span>  
<span data-ttu-id="1ae97-211">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1ae97-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1ae97-212">Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize.</span><span class="sxs-lookup"><span data-stu-id="1ae97-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="1ae97-213">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="1ae97-214">Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1ae97-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1ae97-215">Barındırma başlangıç derlemeleri sağladığınızda, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-216">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-217">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-218">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="1ae97-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="1ae97-219">URL'leri barındırma tercih</span><span class="sxs-lookup"><span data-stu-id="1ae97-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="1ae97-220">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="1ae97-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1ae97-221">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1ae97-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1ae97-222">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="1ae97-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1ae97-223">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="1ae97-223">**Default**: true</span></span>  
<span data-ttu-id="1ae97-224">**Kullanılarak ayarlanan**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="1ae97-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="1ae97-225">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-226">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-227">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-228">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="1ae97-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="1ae97-229">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="1ae97-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="1ae97-230">Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-230">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="1ae97-231">Bkz: [IHostingStartup kullanarak bir dış derlemeden uygulama özelliklerini eklemek](xref:hosting/ihostingstartup) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-231">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="1ae97-232">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1ae97-232">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1ae97-233">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="1ae97-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1ae97-234">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="1ae97-234">**Default**: false</span></span>  
<span data-ttu-id="1ae97-235">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="1ae97-235">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="1ae97-236">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-236">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-237">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-237">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-238">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-238">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-239">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="1ae97-239">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="1ae97-240">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="1ae97-240">Server URLs</span></span>

<span data-ttu-id="1ae97-241">IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-241">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="1ae97-242">**Anahtar**: URL'leri</span><span class="sxs-lookup"><span data-stu-id="1ae97-242">**Key**: urls</span></span>  
<span data-ttu-id="1ae97-243">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-243">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-244">**Varsayılan**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="1ae97-244">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="1ae97-245">**Kullanılarak ayarlanan**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="1ae97-245">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="1ae97-246">Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt.</span><span class="sxs-lookup"><span data-stu-id="1ae97-246">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="1ae97-247">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-247">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1ae97-248">Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="1ae97-248">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1ae97-249">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-249">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1ae97-250">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-250">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-251">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="1ae97-252">Kestrel kendi uç nokta yapılandırması API vardır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-252">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1ae97-253">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="1ae97-253">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-254">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-254">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="1ae97-255">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="1ae97-255">Shutdown Timeout</span></span>

<span data-ttu-id="1ae97-256">Web ana bilgisayarı kapatmak için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-256">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="1ae97-257">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1ae97-257">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1ae97-258">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="1ae97-258">**Type**: *int*</span></span>  
<span data-ttu-id="1ae97-259">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="1ae97-259">**Default**: 5</span></span>  
<span data-ttu-id="1ae97-260">**Kullanılarak ayarlanan**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="1ae97-260">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="1ae97-261">Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` genişletme yöntemi geçen bir `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-261">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="1ae97-262">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-262">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-263">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-263">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-264">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-264">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-265">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="1ae97-265">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="1ae97-266">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="1ae97-266">Startup Assembly</span></span>

<span data-ttu-id="1ae97-267">Aranacak derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1ae97-267">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1ae97-268">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="1ae97-268">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1ae97-269">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-269">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-270">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="1ae97-270">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1ae97-271">**Kullanılarak ayarlanan**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="1ae97-271">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="1ae97-272">Ada göre derleme başvurusu yapabilir (`string`) veya türü (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="1ae97-272">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="1ae97-273">Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-273">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-274">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-274">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-275">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-275">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="1ae97-276">Kök web</span><span class="sxs-lookup"><span data-stu-id="1ae97-276">Web Root</span></span>

<span data-ttu-id="1ae97-277">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-277">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="1ae97-278">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="1ae97-278">**Key**: webroot</span></span>  
<span data-ttu-id="1ae97-279">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="1ae97-279">**Type**: *string*</span></span>  
<span data-ttu-id="1ae97-280">**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="1ae97-280">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="1ae97-281">Yol yoksa, yok dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-281">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="1ae97-282">**Kullanılarak ayarlanan**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="1ae97-282">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-283">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-284">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-284">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="1ae97-285">Yapılandırma geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="1ae97-285">Overriding configuration</span></span>

<span data-ttu-id="1ae97-286">Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-286">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="1ae97-287">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="1ae97-287">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="1ae97-288">Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="1ae97-288">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="1ae97-289">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-289">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-290">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1ae97-291">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="1ae97-291">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1ae97-292">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="1ae97-292">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-293">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-293">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-294">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="1ae97-294">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="1ae97-295">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="1ae97-295">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="1ae97-296">`UseConfiguration` Genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-296">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="1ae97-297">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="1ae97-297">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="1ae97-298">`UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="1ae97-298">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="1ae97-299">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-299">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="1ae97-300">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-300">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="1ae97-301">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="1ae97-301">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="1ae97-302">Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istediğiniz değeri bir komut isteminden yürütülürken geçirebilirdiniz `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-302">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="1ae97-303">Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:</span><span class="sxs-lookup"><span data-stu-id="1ae97-303">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="1ae97-304">Önem derecesi sıralama</span><span class="sxs-lookup"><span data-stu-id="1ae97-304">Ordering importance</span></span>

<span data-ttu-id="1ae97-305">Bazı `WebHostBuilder` varsa ayarları ortam değişkenlerinden önce okunur ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1ae97-305">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="1ae97-306">Bu ortam değişkenleri biçimini kullanın `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-306">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="1ae97-307">Varsayılan olarak sunucunun dinlediği URL'leri ayarlamak için ayarladığınız `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-307">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="1ae97-308">Bu ortam değişken değerleri yapılandırma belirterek geçersiz (kullanarak `UseConfiguration`) veya değeri açıkça ayarlayarak (kullanarak `UseSetting` veya açık genişletme yöntemleri gibi `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="1ae97-308">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="1ae97-309">Ana bilgisayar değeri en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-309">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="1ae97-310">Program aracılığıyla varsayılan bir değere ayarlayın ancak yapılandırması ile geçersiz kılınabilir izin vermek istiyorsanız, URL'yi ayarlandıktan sonra komut satırı yapılandırmayı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ae97-310">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="1ae97-311">Bkz: [geçersiz kılınıyor yapılandırma](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="1ae97-311">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="1ae97-312">Ana bilgisayar başlatma</span><span class="sxs-lookup"><span data-stu-id="1ae97-312">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ae97-313">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-313">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1ae97-314">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="1ae97-314">**Run**</span></span>

<span data-ttu-id="1ae97-315">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="1ae97-315">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1ae97-316">**Start**</span><span class="sxs-lookup"><span data-stu-id="1ae97-316">**Start**</span></span>

<span data-ttu-id="1ae97-317">Engelleyici olmayan bir biçimde konak çağırarak çalıştırabilirsiniz kendi `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1ae97-317">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1ae97-318">İçin URL'lerin bir listesini geçirirseniz `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="1ae97-318">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="1ae97-319">Başlatma ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="1ae97-319">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="1ae97-320">Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:</span><span class="sxs-lookup"><span data-stu-id="1ae97-320">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="1ae97-321">**Başlangıç (RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-321">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="1ae97-322">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-322">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-323">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="1ae97-323">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1ae97-324">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-324">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1ae97-325">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-325">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1ae97-326">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-326">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="1ae97-327">Bir URL ile başlatın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-327">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-328">Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-328">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="1ae97-329">**Başlangıç (Eylem<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-329">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="1ae97-330">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="1ae97-330">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-331">Aşağıdaki tarayıcı isteklerini örnekle kullanın:</span><span class="sxs-lookup"><span data-stu-id="1ae97-331">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="1ae97-332">İstek</span><span class="sxs-lookup"><span data-stu-id="1ae97-332">Request</span></span>                                    | <span data-ttu-id="1ae97-333">Yanıt</span><span class="sxs-lookup"><span data-stu-id="1ae97-333">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="1ae97-334">Merhaba, Martin!</span><span class="sxs-lookup"><span data-stu-id="1ae97-334">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="1ae97-335">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="1ae97-335">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="1ae97-336">"Ooops!" dizesini içeren bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="1ae97-336">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="1ae97-337">Aykırı dizesiyle "Uh!!"</span><span class="sxs-lookup"><span data-stu-id="1ae97-337">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="1ae97-338">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="1ae97-338">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="1ae97-339">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="1ae97-339">Hello World!</span></span>                             |

<span data-ttu-id="1ae97-340">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-340">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1ae97-341">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-341">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1ae97-342">**Başlangıç (url, eylem dize<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-342">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="1ae97-343">Bir örneği ile bir URL kullanın `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-343">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-344">Aynı sonucu verir **Başlat (Eylem<IRouteBuilder> routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-344">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="1ae97-345">**StartWith (Eylem<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-345">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="1ae97-346">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-346">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-347">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="1ae97-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="1ae97-348">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="1ae97-349">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="1ae97-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="1ae97-350">**StartWith (url, eylem dize<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="1ae97-350">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="1ae97-351">Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-351">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="1ae97-352">Aynı sonucu verir **StartWith (Eylem<IApplicationBuilder> uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-352">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ae97-353">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="1ae97-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1ae97-354">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="1ae97-354">**Run**</span></span>

<span data-ttu-id="1ae97-355">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="1ae97-355">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="1ae97-356">**Start**</span><span class="sxs-lookup"><span data-stu-id="1ae97-356">**Start**</span></span>

<span data-ttu-id="1ae97-357">Engelleyici olmayan bir biçimde konak çağırarak çalıştırabilirsiniz kendi `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1ae97-357">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="1ae97-358">İçin URL'lerin bir listesini geçirirseniz `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="1ae97-358">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1ae97-359">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="1ae97-359">IHostingEnvironment interface</span></span>

<span data-ttu-id="1ae97-360">[IHostingEnvironment arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="1ae97-360">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="1ae97-361">Kullanabileceğiniz [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="1ae97-361">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="1ae97-362">Kullanabileceğiniz bir [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta Uygulamanızı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-362">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="1ae97-363">Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1ae97-363">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="1ae97-364">Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-364">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="1ae97-365">Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="1ae97-365">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="1ae97-366">`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işlem hattınızı ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="1ae97-366">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="1ae97-367">Ekleme `IHostingEnvironment` içine `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="1ae97-367">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1ae97-368">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="1ae97-368">IApplicationLifetime interface</span></span>

<span data-ttu-id="1ae97-369">[IApplicationLifetime arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) etkinlikleri sonrası başlatma ve kapatma işlemleri yapmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1ae97-369">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="1ae97-370">Üç arabirimde özelliklerdir ile kaydedebilirsiniz iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlamak için yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-370">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="1ae97-371">Ayrıca bir `StopApplication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1ae97-371">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="1ae97-372">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="1ae97-372">Cancellation Token</span></span>    | <span data-ttu-id="1ae97-373">&#8230;tetiklenen;</span><span class="sxs-lookup"><span data-stu-id="1ae97-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="1ae97-374">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="1ae97-374">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="1ae97-375">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="1ae97-375">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1ae97-376">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-376">Requests may still be processing.</span></span> <span data-ttu-id="1ae97-377">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-377">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="1ae97-378">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="1ae97-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1ae97-379">Tüm istekleri tamamen işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="1ae97-379">All requests should be completely processed.</span></span> <span data-ttu-id="1ae97-380">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="1ae97-380">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="1ae97-381">Yöntem</span><span class="sxs-lookup"><span data-stu-id="1ae97-381">Method</span></span>            | <span data-ttu-id="1ae97-382">Eylem</span><span class="sxs-lookup"><span data-stu-id="1ae97-382">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="1ae97-383">Geçerli uygulamanın sonlandırılması istekleri.</span><span class="sxs-lookup"><span data-stu-id="1ae97-383">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="1ae97-384">Sorun giderme System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="1ae97-384">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="1ae97-385">**ASP.NET çekirdeği 2.0 yalnızca uygular**</span><span class="sxs-lookup"><span data-stu-id="1ae97-385">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="1ae97-386">Konak ekleyerek yapı `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`, aşağıdaki hatalardan biriyle karşılaşabilirsiniz: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-386">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="1ae97-387">Bu kaynaklanır [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-387">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="1ae97-388">El ile ekleme, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin, `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="1ae97-388">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="1ae97-389">Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="1ae97-389">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="1ae97-390">**Not**: yok çağırdığınızda bu yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca `UseStartup` veya `Configure`.</span><span class="sxs-lookup"><span data-stu-id="1ae97-390">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="1ae97-391">Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="1ae97-391">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ae97-392">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1ae97-392">Additional resources</span></span>

* [<span data-ttu-id="1ae97-393">IIS kullanan Windows için yayımlama</span><span class="sxs-lookup"><span data-stu-id="1ae97-393">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="1ae97-394">Nginx kullanarak Linux yayımlama</span><span class="sxs-lookup"><span data-stu-id="1ae97-394">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="1ae97-395">Apache kullanarak Linux yayımlama</span><span class="sxs-lookup"><span data-stu-id="1ae97-395">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="1ae97-396">Bir Windows hizmeti ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="1ae97-396">Host in a Windows Service</span></span>](xref:hosting/windows-service)
