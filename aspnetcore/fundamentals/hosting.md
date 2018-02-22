---
title: "ASP.NET çekirdek barındırma"
author: guardrex
description: "Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: f85efe029baa07d963587dcd02a2c07da40de103
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="62a59-103">ASP.NET çekirdek barındırma</span><span class="sxs-lookup"><span data-stu-id="62a59-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="62a59-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62a59-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="62a59-105">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="62a59-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="62a59-106">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="62a59-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="62a59-107">En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="62a59-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="62a59-108">Bir konak ayarlıyor</span><span class="sxs-lookup"><span data-stu-id="62a59-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62a59-110">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="62a59-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="62a59-111">Bu genellikle uygulamanın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a59-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="62a59-112">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="62a59-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="62a59-113">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="62a59-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="62a59-114">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="62a59-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="62a59-115">Yapılandırır [Kestrel](servers/kestrel.md) web sunucusu olarak.</span><span class="sxs-lookup"><span data-stu-id="62a59-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="62a59-116">Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="62a59-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="62a59-117">İçerik kök tarafından döndürülen yola ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="62a59-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="62a59-118">Gelen isteğe bağlı yapılandırma yükler:</span><span class="sxs-lookup"><span data-stu-id="62a59-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="62a59-119">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="62a59-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="62a59-120">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="62a59-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="62a59-121">[Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="62a59-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="62a59-122">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="62a59-122">Environment variables.</span></span>
  * <span data-ttu-id="62a59-123">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="62a59-123">Command-line arguments.</span></span>
* <span data-ttu-id="62a59-124">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktısı için.</span><span class="sxs-lookup"><span data-stu-id="62a59-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="62a59-125">Günlük kaydı içerir [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="62a59-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="62a59-126">IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="62a59-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="62a59-127">Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="62a59-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="62a59-128">Modül IIS ve Kestrel arasında ters bir proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="62a59-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="62a59-129">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="62a59-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="62a59-130">IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="62a59-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="62a59-131">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="62a59-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="62a59-132">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62a59-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="62a59-133">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="62a59-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="62a59-134">Uygulama yapılandırması hakkında daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="62a59-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="62a59-135">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="62a59-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="62a59-136">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="62a59-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-138">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="62a59-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="62a59-139">Bir ana bilgisayar oluşturma uygulamanın giriş noktası genellikle gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a59-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="62a59-140">Proje şablonları içinde `Main` bulunan *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="62a59-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="62a59-141">`WebHostBuilder` gerektiren bir [IServer uygulayan sunucu](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="62a59-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="62a59-142">Yerleşik sunucularıdır [Kestrel](servers/kestrel.md) ve [HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="62a59-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="62a59-143">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="62a59-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="62a59-144">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="62a59-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="62a59-145">İçin varsayılan içerik kök elde `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="62a59-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="62a59-146">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62a59-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="62a59-147">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="62a59-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="62a59-148">IIS ters proxy kullanmak için arama [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="62a59-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="62a59-149">`UseIISIntegration` yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="62a59-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="62a59-150">`UseIISIntegration` Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters Ara sunucu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="62a59-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="62a59-151">IIS ASP.NET Core ile kullanmak için `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="62a59-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="62a59-152">`UseIISIntegration` yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="62a59-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="62a59-153">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="62a59-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="62a59-154">Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:</span><span class="sxs-lookup"><span data-stu-id="62a59-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="62a59-155">Bir konak ayarlıyor zaman [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="62a59-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="62a59-156">Varsa bir `Startup` sınıfı belirtilmediyse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a59-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="62a59-157">Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](startup.md).</span><span class="sxs-lookup"><span data-stu-id="62a59-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="62a59-158">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="62a59-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="62a59-159">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="62a59-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="62a59-160">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="62a59-160">Host configuration values</span></span>

<span data-ttu-id="62a59-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="62a59-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="62a59-162">Ortam değişkenleri biçiminde içeren konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="62a59-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="62a59-163">Örneğin, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="62a59-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="62a59-164">Açık yöntemleri gibi `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="62a59-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="62a59-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="62a59-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="62a59-166">Bir değerle ayarlarken `UseSetting`, değer, dize türünden bağımsız olarak olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="62a59-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="62a59-167">Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="62a59-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="62a59-168">Daha fazla bilgi için bkz: [geçersiz kılınıyor yapılandırma](#overriding-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="62a59-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="62a59-169">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="62a59-169">Capture Startup Errors</span></span>

<span data-ttu-id="62a59-170">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="62a59-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="62a59-171">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="62a59-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="62a59-172">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="62a59-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62a59-173">**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="62a59-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="62a59-174">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="62a59-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="62a59-175">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="62a59-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="62a59-176">Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="62a59-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="62a59-177">Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="62a59-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="62a59-180">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="62a59-180">Content Root</span></span>

<span data-ttu-id="62a59-181">Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="62a59-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="62a59-182">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="62a59-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="62a59-183">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-183">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-184">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="62a59-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="62a59-185">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="62a59-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="62a59-186">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="62a59-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="62a59-187">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="62a59-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="62a59-188">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="62a59-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="62a59-191">Ayrıntılı hataları</span><span class="sxs-lookup"><span data-stu-id="62a59-191">Detailed Errors</span></span>

<span data-ttu-id="62a59-192">Ayrıntılı hataları yakalanan belirler.</span><span class="sxs-lookup"><span data-stu-id="62a59-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="62a59-193">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="62a59-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="62a59-194">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="62a59-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62a59-195">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="62a59-195">**Default**: false</span></span>  
<span data-ttu-id="62a59-196">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62a59-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="62a59-197">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="62a59-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="62a59-198">Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="62a59-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="62a59-201">Ortam</span><span class="sxs-lookup"><span data-stu-id="62a59-201">Environment</span></span>

<span data-ttu-id="62a59-202">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="62a59-202">Sets the app's environment.</span></span>

<span data-ttu-id="62a59-203">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="62a59-203">**Key**: environment</span></span>  
<span data-ttu-id="62a59-204">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-204">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-205">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="62a59-205">**Default**: Production</span></span>  
<span data-ttu-id="62a59-206">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="62a59-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="62a59-207">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="62a59-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="62a59-208">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="62a59-208">The environment can be set to any value.</span></span> <span data-ttu-id="62a59-209">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="62a59-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="62a59-210">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="62a59-210">Values aren't case sensitive.</span></span> <span data-ttu-id="62a59-211">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="62a59-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="62a59-212">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="62a59-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="62a59-213">Daha fazla bilgi için bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="62a59-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="62a59-216">Barındırma başlangıç derlemeler</span><span class="sxs-lookup"><span data-stu-id="62a59-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="62a59-217">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="62a59-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="62a59-218">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="62a59-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="62a59-219">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-219">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-220">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="62a59-220">**Default**: Empty string</span></span>  
<span data-ttu-id="62a59-221">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62a59-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="62a59-222">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="62a59-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="62a59-223">Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize.</span><span class="sxs-lookup"><span data-stu-id="62a59-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="62a59-224">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="62a59-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="62a59-225">Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="62a59-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="62a59-226">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="62a59-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-229">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="62a59-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="62a59-230">URL'leri barındırma tercih</span><span class="sxs-lookup"><span data-stu-id="62a59-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="62a59-231">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="62a59-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="62a59-232">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="62a59-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="62a59-233">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="62a59-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62a59-234">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="62a59-234">**Default**: true</span></span>  
<span data-ttu-id="62a59-235">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="62a59-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="62a59-236">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="62a59-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="62a59-237">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="62a59-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-240">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="62a59-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="62a59-241">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="62a59-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="62a59-242">Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="62a59-243">Bkz: [platforma özgü yapılandırma kullanarak uygulama özellik ekleme](xref:host-and-deploy/platform-specific-configuration) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="62a59-243">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="62a59-244">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="62a59-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="62a59-245">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="62a59-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="62a59-246">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="62a59-246">**Default**: false</span></span>  
<span data-ttu-id="62a59-247">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="62a59-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="62a59-248">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="62a59-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="62a59-249">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="62a59-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-252">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="62a59-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="62a59-253">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="62a59-253">Server URLs</span></span>

<span data-ttu-id="62a59-254">IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="62a59-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="62a59-255">**Anahtar**: URL'leri</span><span class="sxs-lookup"><span data-stu-id="62a59-255">**Key**: urls</span></span>  
<span data-ttu-id="62a59-256">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-256">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-257">**Default**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="62a59-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="62a59-258">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="62a59-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="62a59-259">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="62a59-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="62a59-260">Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt.</span><span class="sxs-lookup"><span data-stu-id="62a59-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="62a59-261">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="62a59-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="62a59-262">Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="62a59-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="62a59-263">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="62a59-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="62a59-264">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="62a59-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="62a59-266">Kestrel kendi uç nokta yapılandırması API vardır.</span><span class="sxs-lookup"><span data-stu-id="62a59-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="62a59-267">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="62a59-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="62a59-269">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="62a59-269">Shutdown Timeout</span></span>

<span data-ttu-id="62a59-270">Web ana bilgisayarı kapatmak için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="62a59-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="62a59-271">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="62a59-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="62a59-272">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="62a59-272">**Type**: *int*</span></span>  
<span data-ttu-id="62a59-273">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="62a59-273">**Default**: 5</span></span>  
<span data-ttu-id="62a59-274">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="62a59-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="62a59-275">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="62a59-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="62a59-276">Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` genişletme yöntemi geçen bir `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="62a59-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="62a59-277">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="62a59-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-280">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="62a59-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="62a59-281">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="62a59-281">Startup Assembly</span></span>

<span data-ttu-id="62a59-282">Aranacak derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="62a59-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="62a59-283">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="62a59-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="62a59-284">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-284">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-285">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="62a59-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="62a59-286">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="62a59-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="62a59-287">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="62a59-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="62a59-288">Ada göre derleme (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="62a59-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="62a59-289">Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="62a59-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="62a59-292">Kök web</span><span class="sxs-lookup"><span data-stu-id="62a59-292">Web Root</span></span>

<span data-ttu-id="62a59-293">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="62a59-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="62a59-294">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="62a59-294">**Key**: webroot</span></span>  
<span data-ttu-id="62a59-295">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="62a59-295">**Type**: *string*</span></span>  
<span data-ttu-id="62a59-296">**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="62a59-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="62a59-297">Yol yoksa, yok dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62a59-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="62a59-298">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="62a59-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="62a59-299">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="62a59-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="62a59-302">Yapılandırma geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="62a59-302">Overriding configuration</span></span>

<span data-ttu-id="62a59-303">Kullanım [yapılandırma](xref:fundamentals/configuration/index) ana bilgisayarı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="62a59-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="62a59-304">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hosting.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="62a59-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="62a59-305">Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hosting.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="62a59-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="62a59-306">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="62a59-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62a59-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="62a59-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="62a59-309">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="62a59-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="62a59-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="62a59-312">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hosting.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="62a59-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="62a59-313">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="62a59-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="62a59-314">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="62a59-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="62a59-315">`UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="62a59-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="62a59-316">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="62a59-317">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="62a59-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="62a59-318">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="62a59-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="62a59-319">Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istenen değeri bir komut isteminden yürütülürken geçirilebilir `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="62a59-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="62a59-320">Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hosting.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:</span><span class="sxs-lookup"><span data-stu-id="62a59-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="62a59-321">Ana bilgisayar başlatma</span><span class="sxs-lookup"><span data-stu-id="62a59-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="62a59-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="62a59-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="62a59-323">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="62a59-323">**Run**</span></span>

<span data-ttu-id="62a59-324">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatma kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="62a59-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="62a59-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="62a59-325">**Start**</span></span>

<span data-ttu-id="62a59-326">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="62a59-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="62a59-327">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="62a59-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="62a59-328">Uygulamayı başlatın ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="62a59-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="62a59-329">Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:</span><span class="sxs-lookup"><span data-stu-id="62a59-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="62a59-330">**Başlangıç (RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="62a59-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="62a59-331">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="62a59-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="62a59-332">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="62a59-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="62a59-333">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62a59-334">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="62a59-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62a59-335">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="62a59-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="62a59-336">Bir URL ile başlatın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="62a59-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="62a59-337">Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62a59-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="62a59-338">**Başlangıç (Eylem<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="62a59-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="62a59-339">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="62a59-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="62a59-340">Aşağıdaki tarayıcı isteklerini örnekle kullanın:</span><span class="sxs-lookup"><span data-stu-id="62a59-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="62a59-341">İstek</span><span class="sxs-lookup"><span data-stu-id="62a59-341">Request</span></span>                                    | <span data-ttu-id="62a59-342">Yanıt</span><span class="sxs-lookup"><span data-stu-id="62a59-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="62a59-343">Merhaba, Martin!</span><span class="sxs-lookup"><span data-stu-id="62a59-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="62a59-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="62a59-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="62a59-345">"Ooops!" dizesini içeren bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="62a59-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="62a59-346">Aykırı dizesiyle "Uh!!"</span><span class="sxs-lookup"><span data-stu-id="62a59-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="62a59-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="62a59-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="62a59-348">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="62a59-348">Hello World!</span></span>                             |

<span data-ttu-id="62a59-349">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62a59-350">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="62a59-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62a59-351">**Başlangıç (url, eylem dize<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="62a59-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="62a59-352">Bir örneği ile bir URL kullanın `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62a59-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="62a59-353">Aynı sonucu verir **Başlat (Eylem<IRouteBuilder> routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62a59-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="62a59-354">**StartWith (Eylem<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="62a59-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="62a59-355">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62a59-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="62a59-356">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="62a59-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="62a59-357">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="62a59-358">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="62a59-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="62a59-359">**StartWith (url, eylem dize<IApplicationBuilder> uygulama)**</span><span class="sxs-lookup"><span data-stu-id="62a59-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="62a59-360">Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="62a59-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="62a59-361">Aynı sonucu verir **StartWith (Eylem<IApplicationBuilder> uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="62a59-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="62a59-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="62a59-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="62a59-363">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="62a59-363">**Run**</span></span>

<span data-ttu-id="62a59-364">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="62a59-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="62a59-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="62a59-365">**Start**</span></span>

<span data-ttu-id="62a59-366">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="62a59-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="62a59-367">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="62a59-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="62a59-368">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="62a59-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="62a59-369">[IHostingEnvironment arabirimi](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a59-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="62a59-370">Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="62a59-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="62a59-371">A [kurala dayalı yaklaşım](xref:fundamentals/environments#startup-conventions) ortamına bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="62a59-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="62a59-372">Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="62a59-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="62a59-373">Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="62a59-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="62a59-374">Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="62a59-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="62a59-375">`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işleme ardışık ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="62a59-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="62a59-376">`IHostingEnvironment` içine eklenen `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="62a59-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="62a59-377">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="62a59-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="62a59-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a59-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="62a59-379">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="62a59-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="62a59-380">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="62a59-380">Cancellation Token</span></span>    | <span data-ttu-id="62a59-381">&#8230;tetiklenen;</span><span class="sxs-lookup"><span data-stu-id="62a59-381">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="62a59-382">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="62a59-382">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="62a59-383">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="62a59-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="62a59-384">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="62a59-384">Requests may still be processing.</span></span> <span data-ttu-id="62a59-385">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-385">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="62a59-386">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="62a59-386">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="62a59-387">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="62a59-387">All requests should be processed.</span></span> <span data-ttu-id="62a59-388">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="62a59-388">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="62a59-389">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister.</span><span class="sxs-lookup"><span data-stu-id="62a59-389">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="62a59-390">Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde kapatma bir uygulama için zaman sınıfının `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="62a59-390">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="62a59-391">Sorun giderme System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="62a59-391">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="62a59-392">**ASP.NET çekirdeği 2.0 yalnızca uygular**</span><span class="sxs-lookup"><span data-stu-id="62a59-392">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="62a59-393">Bir konak ekleyerek yerleşik `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`:</span><span class="sxs-lookup"><span data-stu-id="62a59-393">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="62a59-394">Konak bu şekilde oluşturulduysa, aşağıdaki hata oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="62a59-394">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="62a59-395">Bu kaynaklanır [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="62a59-395">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="62a59-396">Uygulamayı el ile yerleştirir, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="62a59-396">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="62a59-397">Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="62a59-397">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="62a59-398">**Not**: uygulama çağırdığınızda değil yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca budur `UseStartup` veya `Configure`.</span><span class="sxs-lookup"><span data-stu-id="62a59-398">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="62a59-399">Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="62a59-399">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62a59-400">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="62a59-400">Additional resources</span></span>

* [<span data-ttu-id="62a59-401">IIS ile Windows’da barındırma</span><span class="sxs-lookup"><span data-stu-id="62a59-401">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="62a59-402">Nginx ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="62a59-402">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="62a59-403">Apache ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="62a59-403">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="62a59-404">Bir Windows hizmeti ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="62a59-404">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
