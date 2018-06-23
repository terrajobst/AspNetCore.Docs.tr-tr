---
title: ASP.NET Core Web barındırma
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET Core web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 98070f49c98919e7ebff41ecc69c953249977dcc
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314155"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="95205-103">ASP.NET Core Web barındırma</span><span class="sxs-lookup"><span data-stu-id="95205-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="95205-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="95205-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="95205-105">ASP.NET Core uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="95205-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="95205-106">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="95205-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="95205-107">En az bir sunucu ve bir istek işleme ardışık düzenine konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="95205-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="95205-108">Bu konu, ASP.NET çekirdek Web Host içerir ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), web uygulamaları barındırmak için yararlı olan.</span><span class="sxs-lookup"><span data-stu-id="95205-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="95205-109">Kapsamı .NET genel ana bilgisayar için ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), bkz: [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.</span><span class="sxs-lookup"><span data-stu-id="95205-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="95205-110">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="95205-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="95205-112">Bir örneği kullanılarak bir ana bilgisayar oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="95205-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="95205-113">Bu genellikle uygulamanın giriş noktası gerçekleştirilen `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="95205-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="95205-114">Proje şablonları içinde `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="95205-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="95205-115">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir ana bilgisayar ayarı başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="95205-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="95205-116">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="95205-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="95205-117">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve uygulamanın barındırma yapılandırma sağlayıcıları kullanarak sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="95205-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="95205-118">Kestrel varsayılan seçenekleri için bkz [Kestrel seçenekleri Kestrel web server ASP.NET Core uygulamasında bölümünü](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="95205-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="95205-119">İçerik kök tarafından döndürülen yola ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="95205-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="95205-120">Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:</span><span class="sxs-lookup"><span data-stu-id="95205-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="95205-121">Ortam değişkenleri önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="95205-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="95205-122">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="95205-122">Command-line arguments.</span></span>
* <span data-ttu-id="95205-123">Yükleri uygulama yapılandırmasından:</span><span class="sxs-lookup"><span data-stu-id="95205-123">Loads app configuration from:</span></span>
  * <span data-ttu-id="95205-124">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="95205-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="95205-125">*appSettings. {Ortam} .json*.</span><span class="sxs-lookup"><span data-stu-id="95205-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="95205-126">[Kullanıcı parolaları](xref:security/app-secrets) uygulama çalıştırıldığında `Development` girişi derleme kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="95205-126">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="95205-127">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="95205-127">Environment variables.</span></span>
  * <span data-ttu-id="95205-128">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="95205-128">Command-line arguments.</span></span>
* <span data-ttu-id="95205-129">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktısı için.</span><span class="sxs-lookup"><span data-stu-id="95205-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="95205-130">Günlük kaydı içerir [günlüğü filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="95205-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="95205-131">IIS çalıştırırken etkinleştirir [IIS tümleştirme](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="95205-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="95205-132">Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="95205-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="95205-133">Modül IIS ve Kestrel arasında ters bir proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="95205-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="95205-134">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="95205-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="95205-135">IIS, varsayılan seçenekleri için bkz: [IIS seçenekleri konak ASP.NET Core IIS ile Windows bölümünü](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="95205-135">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="95205-136">Ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="95205-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="95205-137">Daha fazla bilgi için bkz: [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="95205-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="95205-138">Tarafından tanımlanan yapılandırmasını `CreateDefaultBuilder` geçersiz kılındı ve tarafından engagement'ta [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve diğer yöntemleri ve uzantı yöntemleri [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="95205-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="95205-139">Aşağıda birkaç örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="95205-139">A few examples follow:</span></span>

* <span data-ttu-id="95205-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="95205-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="95205-141">Aşağıdaki `ConfigureAppConfiguration` çağrısı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya.</span><span class="sxs-lookup"><span data-stu-id="95205-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="95205-142">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="95205-143">Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) uygulanmaz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="95205-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="95205-144">Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.</span><span class="sxs-lookup"><span data-stu-id="95205-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="95205-145">Aşağıdaki `ConfigureLogging` çağrısı minimum günlüğe kaydetme düzeyi yapılandırmak için bir temsilci ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="95205-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="95205-146">Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="95205-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="95205-147">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="95205-148">Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt olarak belirlenen `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="95205-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="95205-149">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="95205-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="95205-150">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="95205-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="95205-151">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="95205-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="95205-152">Uygulama yapılandırması hakkında daha fazla bilgi için bkz: [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="95205-152">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="95205-153">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` yöntemi, bir ana bilgisayardan oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core ile desteklenen bir yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="95205-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="95205-154">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="95205-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="95205-156">Bir örneği kullanılarak bir ana bilgisayar oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="95205-156">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="95205-157">Bir ana bilgisayar oluşturma uygulamanın giriş noktası genellikle gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="95205-157">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="95205-158">Proje şablonları içinde `Main` bulunan *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="95205-158">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="95205-159">`WebHostBuilder` gerektiren bir [IServer uygulayan sunucu](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="95205-159">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="95205-160">Yerleşik sunucularıdır [Kestrel](xref:fundamentals/servers/kestrel) ve [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 sürümünden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="95205-160">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="95205-161">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="95205-161">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="95205-162">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyalarının nerede arayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="95205-162">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="95205-163">İçin varsayılan içerik kök elde `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="95205-163">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="95205-164">Uygulama projenin kök klasörden başlatıldığında, projenin kök klasörü içerik kök olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="95205-164">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="95205-165">Kullanılan varsayılan değer budur [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="95205-165">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="95205-166">IIS ters proxy kullanmak için arama [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="95205-166">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="95205-167">`UseIISIntegration` yapılandırmaz bir *server*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="95205-167">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="95205-168">`UseIISIntegration` Taban yol ve sunucunun dinlediği üzerinde kullanırken bağlantı noktasını yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters Ara sunucu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="95205-168">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="95205-169">IIS ASP.NET Core ile kullanmak için `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="95205-169">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="95205-170">`UseIISIntegration` yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="95205-170">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="95205-171">Daha fazla bilgi için bkz: [ASP.NET Core modülü için giriş](xref:fundamentals/servers/aspnet-core-module) ve [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="95205-171">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="95205-172">Bir ana bilgisayar (ve ASP.NET Core uygulama) yapılandırır en az bir uygulama sunucusu ve yapılandırma uygulamanın istek ardışık belirtme içerir:</span><span class="sxs-lookup"><span data-stu-id="95205-172">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="95205-173">Bir konak ayarlıyor zaman [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-173">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="95205-174">Varsa bir `Startup` sınıfı belirtilmediyse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="95205-174">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="95205-175">Daha fazla bilgi için bkz: [ASP.NET Core uygulama başlangıç](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="95205-175">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="95205-176">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95205-176">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="95205-177">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarlarını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="95205-177">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="95205-178">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="95205-178">Host configuration values</span></span>

<span data-ttu-id="95205-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="95205-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="95205-180">Ortam değişkenleri biçiminde içeren konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="95205-180">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="95205-181">Örneğin, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="95205-181">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="95205-182">Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).</span><span class="sxs-lookup"><span data-stu-id="95205-182">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="95205-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="95205-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="95205-184">Bir değerle ayarlarken `UseSetting`, değer, dize türünden bağımsız olarak olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="95205-184">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="95205-185">Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="95205-185">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="95205-186">Daha fazla bilgi için bkz: [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="95205-186">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="95205-187">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="95205-187">Capture Startup Errors</span></span>

<span data-ttu-id="95205-188">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="95205-188">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="95205-189">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="95205-189">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="95205-190">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="95205-190">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="95205-191">**Varsayılan**: varsayılan olarak `false` Kestrel IIS, varsayılan olduğu arkasında ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="95205-191">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="95205-192">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="95205-192">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="95205-193">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="95205-193">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="95205-194">Zaman `false`, çıkma konak başlangıç sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="95205-194">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="95205-195">Zaman `true`, ana bilgisayar başlatma sırasında özel durumları yakalar ve sunucu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="95205-195">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="95205-198">Kök içerik</span><span class="sxs-lookup"><span data-stu-id="95205-198">Content Root</span></span>

<span data-ttu-id="95205-199">Bu ayar, ASP.NET Core MVC görünümler gibi içerik dosyaları aranıyor başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="95205-199">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="95205-200">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="95205-200">**Key**: contentRoot</span></span>  
<span data-ttu-id="95205-201">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-201">**Type**: *string*</span></span>  
<span data-ttu-id="95205-202">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.</span><span class="sxs-lookup"><span data-stu-id="95205-202">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="95205-203">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="95205-203">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="95205-204">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="95205-204">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="95205-205">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="95205-205">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="95205-206">Yol yoksa, konağı başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="95205-206">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="95205-209">Ayrıntılı hataları</span><span class="sxs-lookup"><span data-stu-id="95205-209">Detailed Errors</span></span>

<span data-ttu-id="95205-210">Ayrıntılı hataları yakalanan belirler.</span><span class="sxs-lookup"><span data-stu-id="95205-210">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="95205-211">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="95205-211">**Key**: detailedErrors</span></span>  
<span data-ttu-id="95205-212">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="95205-212">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="95205-213">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="95205-213">**Default**: false</span></span>  
<span data-ttu-id="95205-214">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="95205-214">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="95205-215">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="95205-215">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="95205-216">Etkin olduğunda (veya ne zaman <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="95205-216">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="95205-219">Ortam</span><span class="sxs-lookup"><span data-stu-id="95205-219">Environment</span></span>

<span data-ttu-id="95205-220">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="95205-220">Sets the app's environment.</span></span>

<span data-ttu-id="95205-221">**Anahtar**: ortamı</span><span class="sxs-lookup"><span data-stu-id="95205-221">**Key**: environment</span></span>  
<span data-ttu-id="95205-222">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-222">**Type**: *string*</span></span>  
<span data-ttu-id="95205-223">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="95205-223">**Default**: Production</span></span>  
<span data-ttu-id="95205-224">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="95205-224">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="95205-225">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="95205-225">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="95205-226">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-226">The environment can be set to any value.</span></span> <span data-ttu-id="95205-227">Framework tanımlı değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="95205-227">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="95205-228">Değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="95205-228">Values aren't case sensitive.</span></span> <span data-ttu-id="95205-229">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="95205-229">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="95205-230">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri kümesinde *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="95205-230">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="95205-231">Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="95205-231">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-232">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-232">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="95205-234">Barındırma başlangıç derlemeler</span><span class="sxs-lookup"><span data-stu-id="95205-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="95205-235">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="95205-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="95205-236">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="95205-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="95205-237">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-237">**Type**: *string*</span></span>  
<span data-ttu-id="95205-238">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="95205-238">**Default**: Empty string</span></span>  
<span data-ttu-id="95205-239">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="95205-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="95205-240">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="95205-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="95205-241">Başlangıçta yüklemek için başlangıç derlemeleri barındırma noktalı virgülle ayrılmış dize.</span><span class="sxs-lookup"><span data-stu-id="95205-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="95205-242">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="95205-242">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="95205-243">Yapılandırma değeri için boş bir dize Varsayılanları rağmen barındırma başlangıç derlemeler her zaman uygulamanın derleme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="95205-243">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="95205-244">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlatma sırasında ortak hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="95205-244">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-245">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-245">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-246">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-246">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-247">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="95205-247">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="95205-248">URL'leri barındırma tercih</span><span class="sxs-lookup"><span data-stu-id="95205-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="95205-249">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="95205-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="95205-250">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="95205-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="95205-251">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="95205-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="95205-252">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="95205-252">**Default**: true</span></span>  
<span data-ttu-id="95205-253">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="95205-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="95205-254">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="95205-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="95205-255">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="95205-255">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-256">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-256">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-258">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="95205-258">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="95205-259">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="95205-259">Prevent Hosting Startup</span></span>

<span data-ttu-id="95205-260">Uygulamanın derlemesi tarafından yapılandırılan başlangıç derlemeleri barındırma dahil olmak üzere başlangıç derlemeleri barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-260">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="95205-261">Bkz: [IHostingStartup bir dış derleme uygulama geliştirmek](xref:fundamentals/configuration/platform-specific-configuration) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="95205-261">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="95205-262">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="95205-262">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="95205-263">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="95205-263">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="95205-264">**Varsayılan**: yanlış</span><span class="sxs-lookup"><span data-stu-id="95205-264">**Default**: false</span></span>  
<span data-ttu-id="95205-265">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="95205-265">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="95205-266">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="95205-266">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="95205-267">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="95205-267">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-268">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-268">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-270">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="95205-270">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="95205-271">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="95205-271">Server URLs</span></span>

<span data-ttu-id="95205-272">IP adresi veya ana bilgisayar adresleriyle bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="95205-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="95205-273">**Anahtar**: URL'leri</span><span class="sxs-lookup"><span data-stu-id="95205-273">**Key**: urls</span></span>  
<span data-ttu-id="95205-274">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-274">**Type**: *string*</span></span>  
<span data-ttu-id="95205-275">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="95205-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="95205-276">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="95205-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="95205-277">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="95205-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="95205-278">Bir noktalı virgülle ayrılmış ayarlayın (;) URL listesi önekleri sunucu yanıt.</span><span class="sxs-lookup"><span data-stu-id="95205-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="95205-279">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="95205-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="95205-280">Kullan "\*" sunucu IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokolü kullanarak isteklerini dinleme yapması gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="95205-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="95205-281">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="95205-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="95205-282">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="95205-282">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="95205-284">Kestrel kendi uç nokta yapılandırması API vardır.</span><span class="sxs-lookup"><span data-stu-id="95205-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="95205-285">Daha fazla bilgi için bkz: [Kestrel web server ASP.NET Core uygulamasında](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="95205-285">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-286">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-286">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="95205-287">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="95205-287">Shutdown Timeout</span></span>

<span data-ttu-id="95205-288">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="95205-288">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="95205-289">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="95205-289">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="95205-290">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="95205-290">**Type**: *int*</span></span>  
<span data-ttu-id="95205-291">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="95205-291">**Default**: 5</span></span>  
<span data-ttu-id="95205-292">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="95205-292">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="95205-293">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="95205-293">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="95205-294">Anahtar kabul rağmen bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi geçen bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="95205-294">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="95205-295">Bu özelliği, ASP.NET Core 2. 0 ' yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="95205-295">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="95205-296">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="95205-296">During the timeout period, hosting:</span></span>

* <span data-ttu-id="95205-297">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="95205-297">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="95205-298">Barındırılan hizmetler, durdurmak için başarısız hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="95205-298">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="95205-299">Tüm barındırılan Hizmetleri Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında kalan tüm etkin Hizmetleri durduruldu.</span><span class="sxs-lookup"><span data-stu-id="95205-299">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="95205-300">İşleme tamamlamadınız olsa bile hizmetlerini durdurun.</span><span class="sxs-lookup"><span data-stu-id="95205-300">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="95205-301">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımı süresini artırın.</span><span class="sxs-lookup"><span data-stu-id="95205-301">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-302">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-302">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-304">Bu özellik ASP.NET Core kullanılamıyor 1.x.</span><span class="sxs-lookup"><span data-stu-id="95205-304">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="95205-305">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="95205-305">Startup Assembly</span></span>

<span data-ttu-id="95205-306">Aranacak derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="95205-306">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="95205-307">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="95205-307">**Key**: startupAssembly</span></span>  
<span data-ttu-id="95205-308">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-308">**Type**: *string*</span></span>  
<span data-ttu-id="95205-309">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="95205-309">**Default**: The app's assembly</span></span>  
<span data-ttu-id="95205-310">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="95205-310">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="95205-311">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="95205-311">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="95205-312">Ada göre derleme (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-312">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="95205-313">Birden çok `UseStartup` yöntemleri çağrılmadan, son önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="95205-313">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="95205-316">Kök web</span><span class="sxs-lookup"><span data-stu-id="95205-316">Web Root</span></span>

<span data-ttu-id="95205-317">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="95205-317">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="95205-318">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="95205-318">**Key**: webroot</span></span>  
<span data-ttu-id="95205-319">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="95205-319">**Type**: *string*</span></span>  
<span data-ttu-id="95205-320">**Varsayılan**: belirtilmezse, varsayılan "(Content Root)/wwwroot olur", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="95205-320">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="95205-321">Yol yoksa, yok dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="95205-321">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="95205-322">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="95205-322">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="95205-323">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="95205-323">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-324">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-324">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="95205-326">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="95205-326">Override configuration</span></span>

<span data-ttu-id="95205-327">Kullanım [yapılandırma](xref:fundamentals/configuration/index) web ana bilgisayarı yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="95205-327">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="95205-328">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="95205-328">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="95205-329">Herhangi bir yapılandırma aşağıdaki konumdan yüklendi *hostsettings.json* dosyası tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="95205-329">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="95205-330">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="95205-330">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="95205-331">`IWebHostBuilder` yapılandırma, uygulamanın yapılandırma eklenir ancak ters true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="95205-331">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="95205-333">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="95205-333">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="95205-334">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="95205-334">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-335">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-335">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-336">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="95205-336">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="95205-337">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="95205-337">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

---

> [!NOTE]
> <span data-ttu-id="95205-338">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="95205-338">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="95205-339">`GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="95205-339">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="95205-340">`UseConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="95205-340">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="95205-341">Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-341">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="95205-342">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="95205-342">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="95205-343">Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="95205-343">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="95205-344">`UseConfiguration` yalnızca anahtarları sağlanan kopyalar `IConfiguration` konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="95205-344">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="95205-345">Bu nedenle, ayarı `reloadOnChange: true` JSON, INI ve XML ayarları dosyaları için herhangi bir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="95205-345">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="95205-346">Ana bilgisayar üzerinde belirli bir URL çalıştırmak belirtmek için istenen değeri bir komut isteminden yürütülürken geçirilebilir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="95205-346">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="95205-347">Komut satırı bağımsız değişkeni geçersiz kılmaları `urls` değeri *hostsettings.json* dosya ve sunucunun dinlediği bağlantı noktası 8080 üzerinde:</span><span class="sxs-lookup"><span data-stu-id="95205-347">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="95205-348">Ana bilgisayar yönetmek</span><span class="sxs-lookup"><span data-stu-id="95205-348">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="95205-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="95205-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="95205-350">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="95205-350">**Run**</span></span>

<span data-ttu-id="95205-351">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="95205-351">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="95205-352">**Start**</span><span class="sxs-lookup"><span data-stu-id="95205-352">**Start**</span></span>

<span data-ttu-id="95205-353">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="95205-353">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="95205-354">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="95205-354">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="95205-355">Uygulamayı başlatın ve önceden yapılandırılmış Varsayılanları birini kullanarak yeni bir ana bilgisayar Başlat `CreateDefaultBuilder` statik kolaylık yöntemini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="95205-355">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="95205-356">Bu yöntemler konsol çıktısı olmadan ve sunucuyu Başlat [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için bir sonu bekleyin:</span><span class="sxs-lookup"><span data-stu-id="95205-356">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="95205-357">**Başlangıç (RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="95205-357">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="95205-358">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="95205-358">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="95205-359">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="95205-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="95205-360">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="95205-361">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="95205-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="95205-362">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="95205-362">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="95205-363">Bir URL ile başlatın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="95205-363">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="95205-364">Aynı sonucu verir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="95205-364">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="95205-365">**Başlangıç (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="95205-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="95205-366">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="95205-366">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="95205-367">Aşağıdaki tarayıcı isteklerini örnekle kullanın:</span><span class="sxs-lookup"><span data-stu-id="95205-367">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="95205-368">İstek</span><span class="sxs-lookup"><span data-stu-id="95205-368">Request</span></span>                                    | <span data-ttu-id="95205-369">Yanıt</span><span class="sxs-lookup"><span data-stu-id="95205-369">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="95205-370">Merhaba, Martin!</span><span class="sxs-lookup"><span data-stu-id="95205-370">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="95205-371">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="95205-371">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="95205-372">"Ooops!" dizesini içeren bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="95205-372">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="95205-373">Aykırı dizesiyle "Uh!!"</span><span class="sxs-lookup"><span data-stu-id="95205-373">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="95205-374">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="95205-374">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="95205-375">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="95205-375">Hello World!</span></span>                             |

<span data-ttu-id="95205-376">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-376">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="95205-377">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="95205-377">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="95205-378">**Başlangıç (url, eylem dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="95205-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="95205-379">Bir örneği ile bir URL kullanın `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="95205-379">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="95205-380">Aynı sonucu verir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt adresindeki `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="95205-380">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="95205-381">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="95205-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="95205-382">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="95205-382">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="95205-383">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıt almak için</span><span class="sxs-lookup"><span data-stu-id="95205-383">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="95205-384">`WaitForShutdown` break (Ctrl-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-384">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="95205-385">Uygulama görüntüler `Console.WriteLine` ileti ve çıkmak keypress bekler.</span><span class="sxs-lookup"><span data-stu-id="95205-385">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="95205-386">**StartWith (url, eylem dize&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="95205-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="95205-387">Bir URL ve yapılandırmak için bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="95205-387">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="95205-388">Aynı sonucu verir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="95205-388">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="95205-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="95205-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="95205-390">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="95205-390">**Run**</span></span>

<span data-ttu-id="95205-391">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:</span><span class="sxs-lookup"><span data-stu-id="95205-391">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="95205-392">**Start**</span><span class="sxs-lookup"><span data-stu-id="95205-392">**Start**</span></span>

<span data-ttu-id="95205-393">Konak çağırarak engelleyici olmayan bir biçimde çalıştırmak, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="95205-393">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="95205-394">İçin URL'lerin bir listesini aktarılırsa `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="95205-394">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="95205-395">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="95205-395">IHostingEnvironment interface</span></span>

<span data-ttu-id="95205-396">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="95205-396">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="95205-397">Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="95205-397">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="95205-398">A [kurala dayalı yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortamına bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-398">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="95205-399">Alternatif olarak, Ekle `IHostingEnvironment` içine `Startup` kullanılmak üzere Oluşturucusu `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="95205-399">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="95205-400">Ek olarak `IsDevelopment` genişletme yöntemi, `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="95205-400">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="95205-401">Bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="95205-401">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="95205-402">`IHostingEnvironment` Hizmet aynı zamanda eklenen doğrudan `Configure` yöntemi işleme ardışık ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="95205-402">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="95205-403">`IHostingEnvironment` içine eklenen `Invoke` özel oluştururken yöntemi [Ara](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="95205-403">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="95205-404">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="95205-404">IApplicationLifetime interface</span></span>

<span data-ttu-id="95205-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="95205-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="95205-406">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="95205-406">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="95205-407">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="95205-407">Cancellation Token</span></span>    | <span data-ttu-id="95205-408">Ne zaman tetiklendi&#8230;</span><span class="sxs-lookup"><span data-stu-id="95205-408">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="95205-409">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="95205-409">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="95205-410">Ana bilgisayar tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="95205-410">The host has fully started.</span></span> |
| [<span data-ttu-id="95205-411">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="95205-411">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="95205-412">Konak bir kapama üzeredir.</span><span class="sxs-lookup"><span data-stu-id="95205-412">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="95205-413">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="95205-413">All requests should be processed.</span></span> <span data-ttu-id="95205-414">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-414">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="95205-415">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="95205-415">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="95205-416">Konak bir kapama gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="95205-416">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="95205-417">İstekleri hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="95205-417">Requests may still be processing.</span></span> <span data-ttu-id="95205-418">Bu olay tamamlanana kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="95205-418">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="95205-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister.</span><span class="sxs-lookup"><span data-stu-id="95205-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="95205-420">Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde bir uygulamasını kapatmak için zaman sınıfının `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="95205-420">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="95205-421">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="95205-421">Scope validation</span></span>

<span data-ttu-id="95205-422">ASP.NET Core 2.0 veya sonraki sürümlerde, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="95205-422">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="95205-423">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="95205-423">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="95205-424">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök servis sağlayıcısı'ndan çözülmüş değil.</span><span class="sxs-lookup"><span data-stu-id="95205-424">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="95205-425">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değil.</span><span class="sxs-lookup"><span data-stu-id="95205-425">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="95205-426">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="95205-426">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="95205-427">Kök hizmet sağlayıcısının ömrü zaman sağlayıcı uygulamayla başlatır ve uygulamayı kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="95205-427">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="95205-428">Kapsamlı Hizmetleri oluşturuldukları kapsayıcı tarafından elden.</span><span class="sxs-lookup"><span data-stu-id="95205-428">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="95205-429">Kapsamlı bir hizmet kök kapsayıcısında oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından atıldı çünkü hizmetin ömrü tekliye etkili bir şekilde yükseltildi.</span><span class="sxs-lookup"><span data-stu-id="95205-429">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="95205-430">Hizmet kapsamları doğrulama yakalar bu durumlarda, `BuildServiceProvider` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="95205-430">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="95205-431">Her zaman üretim ortamında da dahil olmak üzere kapsamları doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="95205-431">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="95205-432">Sorun giderme System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="95205-432">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="95205-433">**ASP.NET çekirdeği 2.0 yalnızca uygular**</span><span class="sxs-lookup"><span data-stu-id="95205-433">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="95205-434">Bir konak ekleyerek yerleşik `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`:</span><span class="sxs-lookup"><span data-stu-id="95205-434">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="95205-435">Konak bu şekilde oluşturulduysa, aşağıdaki hata oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="95205-435">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="95205-436">Bu kaynaklanır [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (geçerli derleme) taramak için gerekli `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="95205-436">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="95205-437">Uygulamayı el ile yerleştirir, `IStartup` bağımlılık ekleme kapsayıcısına aşağıdaki çağrısı ekleyin `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="95205-437">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="95205-438">Alternatif olarak, bir kukla eklemek `Configure` için `WebHostBuilder`, hangi kümeleri `applicationName`(`ApplicationKey`) otomatik olarak:</span><span class="sxs-lookup"><span data-stu-id="95205-438">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="95205-439">**Not**: uygulama çağırdığınızda değil yalnızca gerekli ASP.NET Core 2.0 sürümüyle ve yalnızca budur `UseStartup` veya `Configure`.</span><span class="sxs-lookup"><span data-stu-id="95205-439">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="95205-440">Daha fazla bilgi için bkz: [duyuruları: Microsoft.Extensions.PlatformAbstractions süredir (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="95205-440">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95205-441">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="95205-441">Additional resources</span></span>

* [<span data-ttu-id="95205-442">IIS ile Windows’da barındırma</span><span class="sxs-lookup"><span data-stu-id="95205-442">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="95205-443">Nginx ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="95205-443">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="95205-444">Apache ile Linux’ta barındırma</span><span class="sxs-lookup"><span data-stu-id="95205-444">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="95205-445">Bir Windows hizmeti ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="95205-445">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
