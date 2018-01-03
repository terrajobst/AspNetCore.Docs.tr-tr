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
ms.openlocfilehash: 0ee8827ad3d5464e1645a40d453054b9e23641cf
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="9fbe1-104">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="9fbe1-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="9fbe1-105">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9fbe1-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9fbe1-106">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="9fbe1-107">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9fbe1-108">En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="9fbe1-109">Bir konak ayarlıyor</span><span class="sxs-lookup"><span data-stu-id="9fbe1-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-110">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9fbe1-111">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="9fbe1-112">Bu genellikle uygulamanın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9fbe1-113">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="9fbe1-114">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="9fbe1-115">`CreateDefaultBuilder`Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="9fbe1-116">Yapılandırır [Kestrel](servers/kestrel.md) web sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="9fbe1-117">Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="9fbe1-118">İçerik kök tarafından döndürülen yola ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="9fbe1-119">Gelen isteğe bağlı yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="9fbe1-120">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="9fbe1-121">*appSettings. {Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="9fbe1-122">[Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="9fbe1-123">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-123">Environment variables.</span></span>
  * <span data-ttu-id="9fbe1-124">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-124">Command-line arguments.</span></span>
* <span data-ttu-id="9fbe1-125">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktısı için.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="9fbe1-126">Günlük kaydı içerir [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="9fbe1-127">IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-127">When running behind IIS, enables [IIS integration](xref:publishing/iis).</span></span> <span data-ttu-id="9fbe1-128">Taban yol ve bağlantı noktası sunucusunun dinlemesi gereken kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-128">Configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="9fbe1-129">Modül IIS ve Kestrel arasında ters proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-129">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="9fbe1-130">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="9fbe1-131">IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="9fbe1-132">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9fbe1-133">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9fbe1-134">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9fbe1-135">Uygulama yapılandırması hakkında daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="9fbe1-136">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="9fbe1-137">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-138">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-139">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="9fbe1-140">Bir ana bilgisayar oluşturma uygulamanın giriş noktası genellikle gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9fbe1-141">Proje şablonları içinde `Main` bulunan *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="9fbe1-142">`WebHostBuilder`gerektiren bir [IServer uygulayan sunucu](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="9fbe1-143">Yerleşik sunucularıdır [Kestrel](servers/kestrel.md) ve [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="9fbe1-144">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="9fbe1-145">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9fbe1-146">İçin varsayılan içerik kök elde `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="9fbe1-147">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9fbe1-148">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9fbe1-149">IIS ters proxy kullanmak için arama [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="9fbe1-150">`UseIISIntegration`yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="9fbe1-151">`UseIISIntegration`Taban yol ve bağlantı noktası sunucusunun dinlemesi gereken kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters proxy oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-151">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="9fbe1-152">IIS ASP.NET Core ile kullanmak için `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="9fbe1-153">`UseIISIntegration`yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="9fbe1-154">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="9fbe1-155">Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="9fbe1-156">Bir konak ayarlıyor zaman [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="9fbe1-157">Varsa bir `Startup` sınıfı belirtilmediyse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="9fbe1-158">Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](startup.md).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="9fbe1-159">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="9fbe1-160">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9fbe1-161">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="9fbe1-161">Host configuration values</span></span>

<span data-ttu-id="9fbe1-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="9fbe1-163">Ortam değişkenleri biçiminde içeren konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="9fbe1-164">Örneğin, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="9fbe1-165">Açık yöntemleri gibi `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="9fbe1-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="9fbe1-167">Bir değerle ayarlarken `UseSetting`, değer, dize türünden bağımsız olarak olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="9fbe1-168">Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="9fbe1-169">Daha fazla bilgi için bkz: [geçersiz kılınıyor yapılandırma](#overriding-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="9fbe1-170">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="9fbe1-170">Capture Startup Errors</span></span>

<span data-ttu-id="9fbe1-171">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="9fbe1-172">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="9fbe1-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="9fbe1-173">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="9fbe1-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9fbe1-174">**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="9fbe1-175">**Kullanılarak ayarlanan**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="9fbe1-176">**Ortam değişkeni**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="9fbe1-177">Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9fbe1-178">Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-179">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-180">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="9fbe1-181">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="9fbe1-181">Content Root</span></span>

<span data-ttu-id="9fbe1-182">Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="9fbe1-183">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9fbe1-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="9fbe1-184">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-184">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-185">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9fbe1-186">**Kullanılarak ayarlanan**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9fbe1-187">**Ortam değişkeni**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="9fbe1-188">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="9fbe1-189">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-190">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-191">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="9fbe1-192">Ayrıntılı hataları</span><span class="sxs-lookup"><span data-stu-id="9fbe1-192">Detailed Errors</span></span>

<span data-ttu-id="9fbe1-193">Ayrıntılı hataları yakalanan belirler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="9fbe1-194">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="9fbe1-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="9fbe1-195">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="9fbe1-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9fbe1-196">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="9fbe1-196">**Default**: false</span></span>  
<span data-ttu-id="9fbe1-197">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9fbe1-198">**Ortam değişkeni**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="9fbe1-199">Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-200">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-201">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="9fbe1-202">Ortam</span><span class="sxs-lookup"><span data-stu-id="9fbe1-202">Environment</span></span>

<span data-ttu-id="9fbe1-203">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-203">Sets the app's environment.</span></span>

<span data-ttu-id="9fbe1-204">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="9fbe1-204">**Key**: environment</span></span>  
<span data-ttu-id="9fbe1-205">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-205">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-206">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="9fbe1-206">**Default**: Production</span></span>  
<span data-ttu-id="9fbe1-207">**Kullanılarak ayarlanan**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9fbe1-208">**Ortam değişkeni**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="9fbe1-209">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-209">The environment can be set to any value.</span></span> <span data-ttu-id="9fbe1-210">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9fbe1-211">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-211">Values aren't case sensitive.</span></span> <span data-ttu-id="9fbe1-212">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9fbe1-213">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="9fbe1-214">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-215">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-216">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="9fbe1-217">Barındırma başlangıç derlemeler</span><span class="sxs-lookup"><span data-stu-id="9fbe1-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="9fbe1-218">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="9fbe1-219">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="9fbe1-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="9fbe1-220">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-220">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-221">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="9fbe1-221">**Default**: Empty string</span></span>  
<span data-ttu-id="9fbe1-222">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9fbe1-223">**Ortam değişkeni**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="9fbe1-224">Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="9fbe1-225">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="9fbe1-226">Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="9fbe1-227">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-228">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-229">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-230">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="9fbe1-231">URL'leri barındırma tercih</span><span class="sxs-lookup"><span data-stu-id="9fbe1-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="9fbe1-232">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="9fbe1-233">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="9fbe1-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="9fbe1-234">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="9fbe1-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9fbe1-235">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="9fbe1-235">**Default**: true</span></span>  
<span data-ttu-id="9fbe1-236">**Kullanılarak ayarlanan**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="9fbe1-237">**Ortam değişkeni**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="9fbe1-238">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-239">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-240">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-241">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="9fbe1-242">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="9fbe1-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="9fbe1-243">Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="9fbe1-244">Bkz: [IHostingStartup kullanarak bir dış derlemeden uygulama özelliklerini eklemek](xref:hosting/ihostingstartup) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-244">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="9fbe1-245">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="9fbe1-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="9fbe1-246">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="9fbe1-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9fbe1-247">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="9fbe1-247">**Default**: false</span></span>  
<span data-ttu-id="9fbe1-248">**Kullanılarak ayarlanan**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9fbe1-249">**Ortam değişkeni**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="9fbe1-250">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-251">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-252">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-253">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="9fbe1-254">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="9fbe1-254">Server URLs</span></span>

<span data-ttu-id="9fbe1-255">IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="9fbe1-256">**Anahtar**: URL'leri</span><span class="sxs-lookup"><span data-stu-id="9fbe1-256">**Key**: urls</span></span>  
<span data-ttu-id="9fbe1-257">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-257">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-258">**Varsayılan**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="9fbe1-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="9fbe1-259">**Kullanılarak ayarlanan**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="9fbe1-260">**Ortam değişkeni**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="9fbe1-261">Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="9fbe1-262">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9fbe1-263">Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="9fbe1-264">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9fbe1-265">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-266">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="9fbe1-267">Kestrel kendi uç nokta yapılandırması API vardır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="9fbe1-268">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-269">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="9fbe1-270">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="9fbe1-270">Shutdown Timeout</span></span>

<span data-ttu-id="9fbe1-271">Web ana bilgisayarı kapatmak için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="9fbe1-272">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="9fbe1-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="9fbe1-273">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-273">**Type**: *int*</span></span>  
<span data-ttu-id="9fbe1-274">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="9fbe1-274">**Default**: 5</span></span>  
<span data-ttu-id="9fbe1-275">**Kullanılarak ayarlanan**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="9fbe1-276">**Ortam değişkeni**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="9fbe1-277">Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` genişletme yöntemi geçen bir `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="9fbe1-278">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-279">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-280">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-281">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="9fbe1-282">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="9fbe1-282">Startup Assembly</span></span>

<span data-ttu-id="9fbe1-283">Aranacak derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="9fbe1-284">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="9fbe1-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="9fbe1-285">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-285">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-286">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="9fbe1-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="9fbe1-287">**Kullanılarak ayarlanan**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="9fbe1-288">**Ortam değişkeni**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="9fbe1-289">Ada göre derleme (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="9fbe1-290">Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-291">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-292">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="9fbe1-293">Kök web</span><span class="sxs-lookup"><span data-stu-id="9fbe1-293">Web Root</span></span>

<span data-ttu-id="9fbe1-294">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="9fbe1-295">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="9fbe1-295">**Key**: webroot</span></span>  
<span data-ttu-id="9fbe1-296">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="9fbe1-296">**Type**: *string*</span></span>  
<span data-ttu-id="9fbe1-297">**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="9fbe1-298">Yol yoksa, yok dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="9fbe1-299">**Kullanılarak ayarlanan**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="9fbe1-300">**Ortam değişkeni**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="9fbe1-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-301">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-302">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="9fbe1-303">Yapılandırma geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="9fbe1-303">Overriding configuration</span></span>

<span data-ttu-id="9fbe1-304">Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="9fbe1-305">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="9fbe1-306">Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="9fbe1-307">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-308">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9fbe1-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="9fbe1-310">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-311">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="9fbe1-313">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="9fbe1-314">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9fbe1-315">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="9fbe1-316">`UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="9fbe1-317">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9fbe1-318">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9fbe1-319">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="9fbe1-320">Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istenen değeri bir komut isteminden yürütülürken geçirilebilir `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="9fbe1-321">Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="9fbe1-322">Ana bilgisayar başlatma</span><span class="sxs-lookup"><span data-stu-id="9fbe1-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9fbe1-323">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9fbe1-324">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-324">**Run**</span></span>

<span data-ttu-id="9fbe1-325">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9fbe1-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-326">**Start**</span></span>

<span data-ttu-id="9fbe1-327">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9fbe1-328">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="9fbe1-329">Uygulamayı başlatın ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="9fbe1-330">Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="9fbe1-331">**Başlangıç (RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="9fbe1-332">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9fbe1-333">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="9fbe1-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9fbe1-334">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9fbe1-335">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9fbe1-336">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="9fbe1-337">Bir URL ile başlatın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9fbe1-338">Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="9fbe1-339">**Başlangıç (Eylem<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="9fbe1-340">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="9fbe1-341">Aşağıdaki tarayıcı isteklerini örnekle kullanın:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="9fbe1-342">İstek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-342">Request</span></span>                                    | <span data-ttu-id="9fbe1-343">Yanıt</span><span class="sxs-lookup"><span data-stu-id="9fbe1-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="9fbe1-344">Merhaba, Martin!</span><span class="sxs-lookup"><span data-stu-id="9fbe1-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="9fbe1-345">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="9fbe1-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="9fbe1-346">"Ooops!" dizesini içeren bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="9fbe1-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="9fbe1-347">Aykırı dizesiyle "Uh!!"</span><span class="sxs-lookup"><span data-stu-id="9fbe1-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="9fbe1-348">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="9fbe1-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="9fbe1-349">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="9fbe1-349">Hello World!</span></span>                             |

<span data-ttu-id="9fbe1-350">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9fbe1-351">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9fbe1-352">**Başlangıç (url, eylem dize<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="9fbe1-353">Bir örneği ile bir URL kullanın `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="9fbe1-354">Aynı sonucu verir **Başlat (Eylem<IRouteBuilder> routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="9fbe1-355">**StartWith (Eylem<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="9fbe1-356">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9fbe1-357">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="9fbe1-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9fbe1-358">`WaitForShutdown`break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9fbe1-359">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9fbe1-360">**StartWith (url, eylem dize<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="9fbe1-361">Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9fbe1-362">Aynı sonucu verir **StartWith (Eylem<IApplicationBuilder> uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9fbe1-363">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="9fbe1-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9fbe1-364">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-364">**Run**</span></span>

<span data-ttu-id="9fbe1-365">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9fbe1-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-366">**Start**</span></span>

<span data-ttu-id="9fbe1-367">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9fbe1-368">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9fbe1-369">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="9fbe1-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="9fbe1-370">[IHostingEnvironment arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="9fbe1-371">Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="9fbe1-372">A [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="9fbe1-373">Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="9fbe1-374">Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="9fbe1-375">Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="9fbe1-376">`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işleme ardışık ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="9fbe1-377">`IHostingEnvironment`içine eklenen `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="9fbe1-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9fbe1-378">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="9fbe1-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="9fbe1-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="9fbe1-380">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="9fbe1-381">Ayrıca bir `StopApplication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="9fbe1-382">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="9fbe1-382">Cancellation Token</span></span>    | <span data-ttu-id="9fbe1-383">&#8230;tetiklenen;</span><span class="sxs-lookup"><span data-stu-id="9fbe1-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="9fbe1-384">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="9fbe1-385">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9fbe1-386">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-386">Requests may still be processing.</span></span> <span data-ttu-id="9fbe1-387">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="9fbe1-388">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9fbe1-389">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-389">All requests should be processed.</span></span> <span data-ttu-id="9fbe1-390">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="9fbe1-391">Yöntem</span><span class="sxs-lookup"><span data-stu-id="9fbe1-391">Method</span></span>            | <span data-ttu-id="9fbe1-392">Eylem</span><span class="sxs-lookup"><span data-stu-id="9fbe1-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="9fbe1-393">Geçerli uygulamanın sonlandırılması istekleri.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-393">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="9fbe1-394">Sorun giderme System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="9fbe1-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="9fbe1-395">**ASP.NET çekirdeği 2.0 yalnızca uygular**</span><span class="sxs-lookup"><span data-stu-id="9fbe1-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="9fbe1-396">Konak ekleyerek oluşturulursa `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`, aşağıdaki hata oluşabilir: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-396">If the host is built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, the following error may occur: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="9fbe1-397">Bu kaynaklanır [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="9fbe1-398">Uygulamayı el ile yerleştirir, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="9fbe1-399">Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="9fbe1-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="9fbe1-400">**Not**: uygulama çağırdığınızda değil yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca budur `UseStartup` veya `Configure`.</span><span class="sxs-lookup"><span data-stu-id="9fbe1-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="9fbe1-401">Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="9fbe1-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9fbe1-402">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9fbe1-402">Additional resources</span></span>

* [<span data-ttu-id="9fbe1-403">IIS kullanan Windows için yayımlama</span><span class="sxs-lookup"><span data-stu-id="9fbe1-403">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="9fbe1-404">Nginx kullanarak Linux yayımlama</span><span class="sxs-lookup"><span data-stu-id="9fbe1-404">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="9fbe1-405">Apache kullanarak Linux yayımlama</span><span class="sxs-lookup"><span data-stu-id="9fbe1-405">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="9fbe1-406">Bir Windows hizmeti ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="9fbe1-406">Host in a Windows Service</span></span>](xref:hosting/windows-service)
