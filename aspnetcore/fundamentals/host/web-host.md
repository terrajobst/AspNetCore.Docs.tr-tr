---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu olan ASP.NET Core web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: e19f12f69dfdd5653aea9c6be2b05f24009b875e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477455"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d9354-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="d9354-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d9354-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d9354-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d9354-105">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="d9354-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d9354-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="d9354-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d9354-107">En az bir sunucu ve istek işleme ardışık konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d9354-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d9354-108">Bu konu, ASP.NET Core Web ana bilgisayarı kapsar ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), web uygulamalarını barındırmak için kullanışlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="d9354-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="d9354-109">Kapsamı .NET genel ana bilgisayar için ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), bkz: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d9354-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d9354-110">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="d9354-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9354-111">Bir konak örneği kullanarak oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d9354-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d9354-112">Bu, genellikle uygulamanın Giriş noktasında gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d9354-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d9354-113">Proje şablonlarındaki `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d9354-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d9354-114">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="d9354-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="d9354-115">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="d9354-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d9354-116">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu ve uygulamanın barındırma yapılandırma sağlayıcıları kullanarak sunucusunu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d9354-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d9354-117">Kestrel'i varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="d9354-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d9354-118">İçerik kök tarafından döndürülen yol ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d9354-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d9354-119">Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:</span><span class="sxs-lookup"><span data-stu-id="d9354-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d9354-120">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="d9354-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d9354-121">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d9354-121">Command-line arguments.</span></span>
* <span data-ttu-id="d9354-122">Uygulama yapılandırması ile aşağıdaki sırada yükler:</span><span class="sxs-lookup"><span data-stu-id="d9354-122">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="d9354-123">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="d9354-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="d9354-124">*appSettings. {Ortamı} .json*.</span><span class="sxs-lookup"><span data-stu-id="d9354-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d9354-125">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="d9354-125">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d9354-126">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d9354-126">Environment variables.</span></span>
  * <span data-ttu-id="d9354-127">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d9354-127">Command-line arguments.</span></span>
* <span data-ttu-id="d9354-128">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktı.</span><span class="sxs-lookup"><span data-stu-id="d9354-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d9354-129">Günlük kaydı içerir [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d9354-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d9354-130">IIS çalıştırırken sağlar [IIS tümleştirme](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d9354-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d9354-131">Temel yol ve bağlantı noktası sunucunun dinlediği üzerinde kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d9354-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d9354-132">Modül IIS ile Kestrel arasında ters bir proxy oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d9354-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d9354-133">Ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d9354-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d9354-134">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="d9354-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d9354-135">Kümeleri [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="d9354-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d9354-136">Daha fazla bilgi için [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="d9354-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d9354-137">Tarafından tanımlanan yapılandırma `CreateDefaultBuilder` geçersiz kılındı ve tarafından Genişletilmiş [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), diğer yöntemler ve uzantı yöntemlerinin [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d9354-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d9354-138">Aşağıda birkaç örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d9354-138">A few examples follow:</span></span>

* <span data-ttu-id="d9354-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulama için.</span><span class="sxs-lookup"><span data-stu-id="d9354-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d9354-140">Aşağıdaki `ConfigureAppConfiguration` çağrıyı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya.</span><span class="sxs-lookup"><span data-stu-id="d9354-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d9354-141">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d9354-142">Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) geçerli değil olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d9354-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d9354-143">Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.</span><span class="sxs-lookup"><span data-stu-id="d9354-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d9354-144">Aşağıdaki `ConfigureLogging` en düşük günlük düzeyi yapılandıracak bir temsilci çağrısı ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="d9354-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d9354-145">Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d9354-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d9354-146">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d9354-147">Aşağıdaki çağrı `ConfigureKestrel` varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9354-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="d9354-148">Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9354-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9354-149">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler.</span><span class="sxs-lookup"><span data-stu-id="d9354-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d9354-150">Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d9354-151">Kullanılan varsayılan [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d9354-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d9354-152">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d9354-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d9354-153">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` konaktan oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) desteklenen bir yaklaşım ile ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d9354-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d9354-154">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="d9354-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d9354-155">Bir konak örneği kullanarak oluşturma [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d9354-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d9354-156">Bir ana bilgisayarı oluşturma uygulamanın Giriş noktasında, genellikle gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d9354-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d9354-157">Proje şablonlarındaki `Main` bulunan *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d9354-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="d9354-158">`WebHostBuilder` gerektiren bir [IServer uygulayan sunucu](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d9354-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d9354-159">Yerleşik sunucular [Kestrel](xref:fundamentals/servers/kestrel) ve [HTTP.sys](xref:fundamentals/servers/httpsys) (ASP.NET Core 2.0 sürümden önce HTTP.sys çağrıldı [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d9354-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d9354-160">Bu örnekte, [UseKestrel genişletme yöntemi](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Kestrel sunucuyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="d9354-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d9354-161">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler.</span><span class="sxs-lookup"><span data-stu-id="d9354-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d9354-162">Varsayılan içerik kök için elde edilen `UseContentRoot` tarafından [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d9354-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d9354-163">Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d9354-164">Kullanılan varsayılan [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d9354-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d9354-165">IIS ters bir proxy olarak kullanmak için çağrı [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) konak oluşturmanın bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="d9354-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d9354-166">`UseIISIntegration` yapılandırmaz bir *sunucu*gibi [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) yapar.</span><span class="sxs-lookup"><span data-stu-id="d9354-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d9354-167">`UseIISIntegration` temel yol ve bağlantı noktası sunucunun dinlediği üzerinde kullanırken yapılandırır [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) Kestrel ve IIS arasında ters Ara sunucu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="d9354-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d9354-168">IIS ile ASP.NET Core, kullanılacak `UseKestrel` ve `UseIISIntegration` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d9354-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d9354-169">`UseIISIntegration` yalnızca IIS veya IIS Express çalıştırırken etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="d9354-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d9354-170">Daha fazla bilgi için bkz. <xref:fundamentals/servers/aspnet-core-module> ve <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d9354-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d9354-171">Bir ana bilgisayar (ve ASP.NET Core uygulaması) yapılandırır. en az bir uygulama sunucusu ve uygulamanın istek ardışık düzeni yapılandırması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="d9354-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="d9354-172">Bir ana bilgisayar, ayarlarken [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d9354-173">Varsa bir `Startup` sınıf belirtilmişse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d9354-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d9354-174">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d9354-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d9354-175">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9354-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d9354-176">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d9354-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d9354-177">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="d9354-177">Host configuration values</span></span>

<span data-ttu-id="d9354-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="d9354-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d9354-179">Ortam değişkenlerini biçiminde içerir konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d9354-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d9354-180">Örneğin: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d9354-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d9354-181">Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).</span><span class="sxs-lookup"><span data-stu-id="d9354-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d9354-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="d9354-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d9354-183">Bir değerle ayarlarken `UseSetting`, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d9354-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d9354-184">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="d9354-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d9354-185">Daha fazla bilgi için [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="d9354-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d9354-186">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="d9354-186">Application Key (Name)</span></span>

<span data-ttu-id="d9354-187">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanmış olduğunda [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) konak yapım sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d9354-188">Değer, uygulamanın giriş noktasını içeren derleme adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d9354-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d9354-189">Değeri açıkça ayarlamak için kullanın [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="d9354-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="d9354-190">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="d9354-190">**Key**: applicationName</span></span>  
<span data-ttu-id="d9354-191">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-191">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-192">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="d9354-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d9354-193">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d9354-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d9354-194">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="d9354-194">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="d9354-195">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="d9354-195">Capture Startup Errors</span></span>

<span data-ttu-id="d9354-196">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="d9354-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d9354-197">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d9354-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d9354-198">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d9354-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d9354-199">**Varsayılan**: varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="d9354-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d9354-200">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d9354-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d9354-201">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d9354-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d9354-202">Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="d9354-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d9354-203">Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="d9354-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="d9354-204">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="d9354-204">Content Root</span></span>

<span data-ttu-id="d9354-205">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramasını başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="d9354-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d9354-206">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d9354-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="d9354-207">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-207">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-208">**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="d9354-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d9354-209">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d9354-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d9354-210">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d9354-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d9354-211">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d9354-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d9354-212">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="d9354-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="d9354-213">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="d9354-213">Detailed Errors</span></span>

<span data-ttu-id="d9354-214">Ayrıntılı hatalar yakalanmasının gerekip belirler.</span><span class="sxs-lookup"><span data-stu-id="d9354-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d9354-215">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d9354-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d9354-216">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d9354-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d9354-217">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="d9354-217">**Default**: false</span></span>  
<span data-ttu-id="d9354-218">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d9354-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d9354-219">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d9354-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d9354-220">Etkin olduğunda (veya <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumlar yakalar.</span><span class="sxs-lookup"><span data-stu-id="d9354-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="d9354-221">Ortam</span><span class="sxs-lookup"><span data-stu-id="d9354-221">Environment</span></span>

<span data-ttu-id="d9354-222">Uygulamanın ortamı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d9354-222">Sets the app's environment.</span></span>

<span data-ttu-id="d9354-223">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="d9354-223">**Key**: environment</span></span>  
<span data-ttu-id="d9354-224">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-224">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-225">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="d9354-225">**Default**: Production</span></span>  
<span data-ttu-id="d9354-226">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d9354-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d9354-227">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d9354-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d9354-228">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-228">The environment can be set to any value.</span></span> <span data-ttu-id="d9354-229">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="d9354-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d9354-230">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="d9354-230">Values aren't case sensitive.</span></span> <span data-ttu-id="d9354-231">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="d9354-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d9354-232">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri ayarlanabilir *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d9354-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d9354-233">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d9354-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d9354-234">Başlangıç derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="d9354-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d9354-235">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d9354-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d9354-236">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d9354-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d9354-237">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-237">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-238">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="d9354-238">**Default**: Empty string</span></span>  
<span data-ttu-id="d9354-239">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d9354-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d9354-240">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d9354-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d9354-241">Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="d9354-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d9354-242">Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d9354-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d9354-243">Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="d9354-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="d9354-244">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="d9354-244">HTTPS Port</span></span>

<span data-ttu-id="d9354-245">HTTPS, yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d9354-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d9354-246">Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="d9354-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d9354-247">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="d9354-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d9354-248">**Kullanılarak ayarlanan**: `UseSetting` 
 **ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d9354-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="d9354-249">Başlangıç dışlama derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="d9354-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="d9354-250">Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="d9354-250">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="d9354-251">**Anahtar**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="d9354-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="d9354-252">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-252">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-253">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="d9354-253">**Default**: Empty string</span></span>  
<span data-ttu-id="d9354-254">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d9354-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d9354-255">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d9354-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d9354-256">URL'leri barındırma tercih et</span><span class="sxs-lookup"><span data-stu-id="d9354-256">Prefer Hosting URLs</span></span>

<span data-ttu-id="d9354-257">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="d9354-257">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d9354-258">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d9354-258">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d9354-259">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d9354-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d9354-260">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="d9354-260">**Default**: true</span></span>  
<span data-ttu-id="d9354-261">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d9354-261">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d9354-262">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d9354-262">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d9354-263">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="d9354-263">Prevent Hosting Startup</span></span>

<span data-ttu-id="d9354-264">Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-264">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d9354-265">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d9354-265">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d9354-266">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d9354-266">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d9354-267">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d9354-267">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d9354-268">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="d9354-268">**Default**: false</span></span>  
<span data-ttu-id="d9354-269">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d9354-269">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d9354-270">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d9354-270">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="d9354-271">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="d9354-271">Server URLs</span></span>

<span data-ttu-id="d9354-272">IP adresi veya konak bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri adresleriyle gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9354-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d9354-273">**Anahtar**: URL'ler</span><span class="sxs-lookup"><span data-stu-id="d9354-273">**Key**: urls</span></span>  
<span data-ttu-id="d9354-274">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-274">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-275">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d9354-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d9354-276">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d9354-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d9354-277">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d9354-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d9354-278">Bir noktalı virgülle ayrılmış ayarlayın (;) listesini URL ön ekleri sunucunun yanıt vermelidir.</span><span class="sxs-lookup"><span data-stu-id="d9354-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d9354-279">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="d9354-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d9354-280">Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d9354-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d9354-281">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="d9354-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d9354-282">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="d9354-282">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d9354-283">Kestrel'i kendi API uç nokta yapılandırması vardır.</span><span class="sxs-lookup"><span data-stu-id="d9354-283">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d9354-284">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d9354-284">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="d9354-285">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="d9354-285">Shutdown Timeout</span></span>

<span data-ttu-id="d9354-286">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="d9354-286">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="d9354-287">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d9354-287">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d9354-288">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="d9354-288">**Type**: *int*</span></span>  
<span data-ttu-id="d9354-289">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="d9354-289">**Default**: 5</span></span>  
<span data-ttu-id="d9354-290">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d9354-290">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d9354-291">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d9354-291">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d9354-292">Anahtarı kabul eder ancak bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi alır bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="d9354-292">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d9354-293">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="d9354-293">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d9354-294">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="d9354-294">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d9354-295">Barındırılan hizmetler, durdurmak için başarısız olan hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="d9354-295">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d9354-296">Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur.</span><span class="sxs-lookup"><span data-stu-id="d9354-296">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d9354-297">İşlem tamamlandı olmasanız bile hizmetleri durdurun.</span><span class="sxs-lookup"><span data-stu-id="d9354-297">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d9354-298">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="d9354-298">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="d9354-299">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="d9354-299">Startup Assembly</span></span>

<span data-ttu-id="d9354-300">Aramak için derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d9354-300">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d9354-301">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d9354-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d9354-302">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-302">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-303">**Varsayılan**: uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="d9354-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d9354-304">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d9354-304">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d9354-305">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d9354-305">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d9354-306">Ada göre bütünleştirilmiş kod (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-306">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d9354-307">Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="d9354-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="d9354-308">Web kökü</span><span class="sxs-lookup"><span data-stu-id="d9354-308">Web Root</span></span>

<span data-ttu-id="d9354-309">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d9354-309">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d9354-310">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="d9354-310">**Key**: webroot</span></span>  
<span data-ttu-id="d9354-311">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d9354-311">**Type**: *string*</span></span>  
<span data-ttu-id="d9354-312">**Varsayılan**: belirtilmezse varsayılan "(Content Root)/wwwroot olan", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="d9354-312">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d9354-313">Yol mevcut değilse, ardından İşlemsiz dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-313">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d9354-314">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d9354-314">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d9354-315">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d9354-315">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="d9354-316">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="d9354-316">Override configuration</span></span>

<span data-ttu-id="d9354-317">Kullanım [yapılandırma](xref:fundamentals/configuration/index) web ana bilgisayarı yapılandırılamadı.</span><span class="sxs-lookup"><span data-stu-id="d9354-317">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="d9354-318">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="d9354-318">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d9354-319">Herhangi bir yapılandırma öğesinden yüklenen *hostsettings.json* dosya tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="d9354-319">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d9354-320">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="d9354-320">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d9354-321">`IWebHostBuilder` yapılandırma, uygulamanın yapılandırma için eklenir, ancak listesiyse true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="d9354-321">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9354-322">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="d9354-322">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d9354-323">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d9354-323">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d9354-324">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="d9354-324">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d9354-325">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d9354-325">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d9354-326">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d9354-326">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d9354-327">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d9354-327">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d9354-328">`UseConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d9354-328">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d9354-329">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-329">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d9354-330">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="d9354-330">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d9354-331">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d9354-331">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="d9354-332">`UseConfiguration` anahtarlar yalnızca sağlanan akış kopyalar `IConfiguration` konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="d9354-332">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d9354-333">Bu nedenle, ayarı `reloadOnChange: true` JSON INI ve XML ayar dosyaları için hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="d9354-333">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d9354-334">Ana belirli URL'yi belirtmek için istenen değeri bir komut istemi'nden yürütülürken geçirilebilir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="d9354-334">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d9354-335">Komut satırı bağımsız değişkeni geçersiz kılmalar `urls` değerini *hostsettings.json* dosya ve sunucu, 8080 bağlantı noktasında dinler:</span><span class="sxs-lookup"><span data-stu-id="d9354-335">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d9354-336">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="d9354-336">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9354-337">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="d9354-337">**Run**</span></span>

<span data-ttu-id="d9354-338">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="d9354-338">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d9354-339">**Start**</span><span class="sxs-lookup"><span data-stu-id="d9354-339">**Start**</span></span>

<span data-ttu-id="d9354-340">Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d9354-340">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d9354-341">URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="d9354-341">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d9354-342">Uygulamayı başlatın ve önceden yapılandırılmış varsayılan kullanarak yeni bir ana bilgisayarı başlatılamıyor `CreateDefaultBuilder` statik kolaylık yöntemi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="d9354-342">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d9354-343">Bu yöntemler konsol çıktısı olmadan ve sunucuyu başlatın [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için mola bekleyin:</span><span class="sxs-lookup"><span data-stu-id="d9354-343">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d9354-344">**Başlangıç (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="d9354-344">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d9354-345">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d9354-345">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d9354-346">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="d9354-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d9354-347">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d9354-348">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="d9354-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d9354-349">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="d9354-349">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d9354-350">Bir URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d9354-350">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d9354-351">Aynı sonucu üretir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d9354-351">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d9354-352">**Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d9354-352">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d9354-353">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="d9354-353">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d9354-354">Aşağıdaki tarayıcı isteklerini ile örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="d9354-354">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d9354-355">İstek</span><span class="sxs-lookup"><span data-stu-id="d9354-355">Request</span></span>                                    | <span data-ttu-id="d9354-356">Yanıt</span><span class="sxs-lookup"><span data-stu-id="d9354-356">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d9354-357">Martin Merhaba!</span><span class="sxs-lookup"><span data-stu-id="d9354-357">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d9354-358">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d9354-358">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d9354-359">"Ooops!" dizesi ile bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="d9354-359">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d9354-360">Bir özel durum dizesi ile oluşturur "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="d9354-360">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d9354-361">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d9354-361">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d9354-362">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="d9354-362">Hello World!</span></span>                             |

<span data-ttu-id="d9354-363">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d9354-364">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="d9354-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d9354-365">**Başlat (eylem url dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d9354-365">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d9354-366">Bir URL örneği kullanıp `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9354-366">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d9354-367">Aynı sonucu üretir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d9354-367">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d9354-368">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="d9354-368">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d9354-369">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9354-369">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d9354-370">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="d9354-370">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d9354-371">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-371">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d9354-372">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="d9354-372">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d9354-373">**StartWith (url, eylem dizesi&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="d9354-373">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d9354-374">Bir URL ve yapılandıracak bir temsilci sağlar bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9354-374">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d9354-375">Aynı sonucu üretir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d9354-375">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d9354-376">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="d9354-376">**Run**</span></span>

<span data-ttu-id="d9354-377">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="d9354-377">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d9354-378">**Start**</span><span class="sxs-lookup"><span data-stu-id="d9354-378">**Start**</span></span>

<span data-ttu-id="d9354-379">Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d9354-379">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d9354-380">URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="d9354-380">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d9354-381">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="d9354-381">IHostingEnvironment interface</span></span>

<span data-ttu-id="d9354-382">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d9354-382">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d9354-383">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="d9354-383">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d9354-384">A [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortama bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-384">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d9354-385">Alternatif olarak, ekleme `IHostingEnvironment` içine `Startup` Oluşturucusu kullanılmak üzere `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d9354-385">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d9354-386">Ek olarak `IsDevelopment` genişletme yöntemi `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="d9354-386">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d9354-387">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d9354-387">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d9354-388">`IHostingEnvironment` Hizmet de eklenen doğrudan `Configure` işleme işlem hattı ayarlamaya yönelik yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d9354-388">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d9354-389">`IHostingEnvironment` içine yerleştirilebilir `Invoke` özel oluştururken yöntemi [ara yazılım](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="d9354-389">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d9354-390">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="d9354-390">IApplicationLifetime interface</span></span>

<span data-ttu-id="d9354-391">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="d9354-391">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d9354-392">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="d9354-392">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d9354-393">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="d9354-393">Cancellation Token</span></span>    | <span data-ttu-id="d9354-394">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="d9354-394">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d9354-395">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d9354-395">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d9354-396">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="d9354-396">The host has fully started.</span></span> |
| [<span data-ttu-id="d9354-397">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d9354-397">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d9354-398">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="d9354-398">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d9354-399">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="d9354-399">All requests should be processed.</span></span> <span data-ttu-id="d9354-400">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-400">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d9354-401">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d9354-401">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d9354-402">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="d9354-402">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d9354-403">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="d9354-403">Requests may still be processing.</span></span> <span data-ttu-id="d9354-404">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="d9354-404">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d9354-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="d9354-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d9354-406">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d9354-406">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="d9354-407">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="d9354-407">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9354-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="d9354-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="d9354-409">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı, doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="d9354-409">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d9354-410">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="d9354-410">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d9354-411">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="d9354-411">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d9354-412">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-412">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d9354-413">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d9354-413">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d9354-414">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="d9354-414">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d9354-415">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="d9354-415">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d9354-416">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="d9354-416">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d9354-417">Kapsamlar üretim ortamında dahil olmak üzere, her zaman doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="d9354-417">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d9354-418">System.ArgumentException sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="d9354-418">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d9354-419">**Aşağıdaki uygulama çağırdığınızda değil, yalnızca ASP.NET Core 2.0 uygulamaları için geçerli `UseStartup` veya `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="d9354-419">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="d9354-420">Bir konak ekleyerek oluşturulabilir `IStartup` doğrudan çağırmak yerine bağımlılık ekleme kapsayıcısını `UseStartup` veya `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d9354-420">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d9354-421">Bu şekilde konak oluşturulursa, şu hata ortaya çıkabilir:</span><span class="sxs-lookup"><span data-stu-id="d9354-421">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d9354-422">Uygulama adı (geçerli derleme adı) taraması gereklidir çünkü böyle `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d9354-422">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d9354-423">Uygulamayı el ile ekler, `IStartup` bağımlılık ekleme kapsayıcısına eklemek için aşağıdaki çağrıyı `WebHostBuilder` ile belirtilen derleme adı:</span><span class="sxs-lookup"><span data-stu-id="d9354-423">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="d9354-424">Alternatif olarak, bir kukla eklenmesini `Configure` için `WebHostBuilder`, otomatik olarak uygulama adını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="d9354-424">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="d9354-425">Daha fazla bilgi için [duyuruları: Microsoft.Extensions.PlatformAbstractions geldi (Açıklama) kaldırıldı](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) ve [StartupInjection örnek](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d9354-425">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d9354-426">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d9354-426">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
