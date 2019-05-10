---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu olan ASP.NET Core Web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: b391b5e514e750f64f30d33cf4eb91e489242eba
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901580"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="a0f42-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="a0f42-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="a0f42-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a0f42-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a0f42-105">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="a0f42-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="a0f42-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="a0f42-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a0f42-107">En az bir sunucu ve istek işleme ardışık konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="a0f42-108">Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0f42-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a0f42-109">Bu konuda 1.1 sürümü için indirme [ASP.NET Core Web ana bilgisayarı (sürüm 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="a0f42-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="a0f42-110">Bu makalede, ASP.NET Core Web ana bilgisayarı yer almaktadır (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), web uygulamalarını barındırmak için olduğu.</span><span class="sxs-lookup"><span data-stu-id="a0f42-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="a0f42-111">.NET genel ana bilgisayar hakkında bilgi için ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), bkz: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="a0f42-112">Bu makalede, ASP.NET Core Web ana bilgisayarı yer almaktadır ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="a0f42-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="a0f42-113">ASP.NET Core 3.0 sürümünde, Web barındırma genel ana bilgisayar değiştirir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="a0f42-114">Daha fazla bilgi için [konak](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="a0f42-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="a0f42-115">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="a0f42-115">Set up a host</span></span>

<span data-ttu-id="a0f42-116">Bir konak örneği kullanarak oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a0f42-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="a0f42-117">Bu, genellikle uygulamanın Giriş noktasında gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a0f42-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="a0f42-118">Proje şablonlarındaki `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="a0f42-118">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="a0f42-119">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="a0f42-119">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="a0f42-120">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="a0f42-120">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="a0f42-121">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) uygulamayı kullanarak web sunucusu olarak yapılandırma sağlayıcıları barındıran.</span><span class="sxs-lookup"><span data-stu-id="a0f42-121">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="a0f42-122">Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-122">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="a0f42-123">İçerik kök tarafından döndürülen yol ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="a0f42-123">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="a0f42-124">Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:</span><span class="sxs-lookup"><span data-stu-id="a0f42-124">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="a0f42-125">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="a0f42-125">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="a0f42-126">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a0f42-126">Command-line arguments.</span></span>
* <span data-ttu-id="a0f42-127">Uygulama yapılandırması ile aşağıdaki sırada yükler:</span><span class="sxs-lookup"><span data-stu-id="a0f42-127">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="a0f42-128">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a0f42-128">*appsettings.json*.</span></span>
  * <span data-ttu-id="a0f42-129">*appSettings. {Ortamı} .json*.</span><span class="sxs-lookup"><span data-stu-id="a0f42-129">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="a0f42-130">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="a0f42-130">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a0f42-131">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a0f42-131">Environment variables.</span></span>
  * <span data-ttu-id="a0f42-132">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="a0f42-132">Command-line arguments.</span></span>
* <span data-ttu-id="a0f42-133">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktı.</span><span class="sxs-lookup"><span data-stu-id="a0f42-133">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="a0f42-134">Günlük kaydı içerir [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a0f42-134">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="a0f42-135">Arkasında IIS ile çalıştırırken [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` sağlayan [IIS tümleştirme](xref:host-and-deploy/iis/index), uygulamanın taban adresini ve bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-135">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="a0f42-136">IIS tümleştirme, ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="a0f42-136">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="a0f42-137">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-137">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="a0f42-138">Kümeleri [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="a0f42-138">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="a0f42-139">Daha fazla bilgi için [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="a0f42-139">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="a0f42-140">Tarafından tanımlanan yapılandırma `CreateDefaultBuilder` geçersiz kılındı ve tarafından Genişletilmiş [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), diğer yöntemler ve uzantı yöntemlerinin [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="a0f42-140">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="a0f42-141">Aşağıda birkaç örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="a0f42-141">A few examples follow:</span></span>

* <span data-ttu-id="a0f42-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulama için.</span><span class="sxs-lookup"><span data-stu-id="a0f42-142">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="a0f42-143">Aşağıdaki `ConfigureAppConfiguration` çağrıyı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya.</span><span class="sxs-lookup"><span data-stu-id="a0f42-143">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="a0f42-144">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-144">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="a0f42-145">Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) geçerli değil olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a0f42-145">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="a0f42-146">Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.</span><span class="sxs-lookup"><span data-stu-id="a0f42-146">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="a0f42-147">Aşağıdaki `ConfigureLogging` en düşük günlük düzeyi yapılandıracak bir temsilci çağrısı ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="a0f42-147">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="a0f42-148">Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-148">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a0f42-149">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-149">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="a0f42-150">Aşağıdaki çağrı `ConfigureKestrel` varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-150">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="a0f42-151">Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-151">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="a0f42-152">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-152">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="a0f42-153">Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-153">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="a0f42-154">Kullanılan varsayılan [Visual Studio](https://visualstudio.microsoft.com) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="a0f42-154">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="a0f42-155">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-155">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="a0f42-156">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` konaktan oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) desteklenen bir yaklaşım ile ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="a0f42-156">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="a0f42-157">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="a0f42-157">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="a0f42-158">Bir ana bilgisayar, ayarlarken [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-158">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="a0f42-159">Varsa bir `Startup` sınıf belirtilmişse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a0f42-159">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="a0f42-160">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-160">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="a0f42-161">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0f42-161">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="a0f42-162">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a0f42-162">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="a0f42-163">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="a0f42-163">Host configuration values</span></span>

<span data-ttu-id="a0f42-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="a0f42-164">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="a0f42-165">Ortam değişkenlerini biçiminde içerir konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-165">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="a0f42-166">Örneğin: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a0f42-166">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="a0f42-167">Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).</span><span class="sxs-lookup"><span data-stu-id="a0f42-167">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="a0f42-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-168">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="a0f42-169">Bir değerle ayarlarken `UseSetting`, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-169">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="a0f42-170">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-170">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="a0f42-171">Daha fazla bilgi için [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="a0f42-171">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="a0f42-172">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="a0f42-172">Application Key (Name)</span></span>

<span data-ttu-id="a0f42-173">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanmış olduğunda [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) konak yapım sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-173">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="a0f42-174">Değer, uygulamanın giriş noktasını içeren derleme adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-174">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="a0f42-175">Değeri açıkça ayarlamak için kullanın [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="a0f42-175">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="a0f42-176">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="a0f42-176">**Key**: applicationName</span></span>  
<span data-ttu-id="a0f42-177">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-177">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-178">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="a0f42-178">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="a0f42-179">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a0f42-179">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a0f42-180">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="a0f42-180">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="a0f42-181">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="a0f42-181">Capture Startup Errors</span></span>

<span data-ttu-id="a0f42-182">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-182">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="a0f42-183">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="a0f42-183">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="a0f42-184">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="a0f42-184">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a0f42-185">**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-185">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="a0f42-186">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="a0f42-186">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="a0f42-187">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="a0f42-187">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="a0f42-188">Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-188">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="a0f42-189">Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="a0f42-189">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="a0f42-190">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="a0f42-190">Content Root</span></span>

<span data-ttu-id="a0f42-191">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramasını başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-191">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="a0f42-192">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="a0f42-192">**Key**: contentRoot</span></span>  
<span data-ttu-id="a0f42-193">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-193">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-194">**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.</span><span class="sxs-lookup"><span data-stu-id="a0f42-194">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="a0f42-195">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="a0f42-195">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="a0f42-196">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="a0f42-196">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="a0f42-197">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="a0f42-197">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="a0f42-198">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a0f42-198">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="a0f42-199">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="a0f42-199">Detailed Errors</span></span>

<span data-ttu-id="a0f42-200">Ayrıntılı hatalar yakalanmasının gerekip belirler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-200">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="a0f42-201">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="a0f42-201">**Key**: detailedErrors</span></span>  
<span data-ttu-id="a0f42-202">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="a0f42-202">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a0f42-203">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="a0f42-203">**Default**: false</span></span>  
<span data-ttu-id="a0f42-204">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a0f42-204">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a0f42-205">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="a0f42-205">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="a0f42-206">Etkin olduğunda (veya <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumlar yakalar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-206">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="a0f42-207">Ortam</span><span class="sxs-lookup"><span data-stu-id="a0f42-207">Environment</span></span>

<span data-ttu-id="a0f42-208">Uygulamanın ortamı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-208">Sets the app's environment.</span></span>

<span data-ttu-id="a0f42-209">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="a0f42-209">**Key**: environment</span></span>  
<span data-ttu-id="a0f42-210">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-210">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-211">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="a0f42-211">**Default**: Production</span></span>  
<span data-ttu-id="a0f42-212">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="a0f42-212">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="a0f42-213">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="a0f42-213">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="a0f42-214">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-214">The environment can be set to any value.</span></span> <span data-ttu-id="a0f42-215">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-215">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="a0f42-216">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-216">Values aren't case sensitive.</span></span> <span data-ttu-id="a0f42-217">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="a0f42-217">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a0f42-218">Kullanırken [Visual Studio](https://visualstudio.microsoft.com), ortam değişkenleri ayarlanabilir *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a0f42-218">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="a0f42-219">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-219">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="a0f42-220">Başlangıç derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="a0f42-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="a0f42-221">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="a0f42-222">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="a0f42-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="a0f42-223">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-223">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-224">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="a0f42-224">**Default**: Empty string</span></span>  
<span data-ttu-id="a0f42-225">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a0f42-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a0f42-226">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="a0f42-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="a0f42-227">Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="a0f42-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="a0f42-228">Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a0f42-228">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="a0f42-229">Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-229">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="a0f42-230">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="a0f42-230">HTTPS Port</span></span>

<span data-ttu-id="a0f42-231">HTTPS, yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a0f42-231">Set the HTTPS redirect port.</span></span> <span data-ttu-id="a0f42-232">Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="a0f42-232">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="a0f42-233">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="a0f42-233">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="a0f42-234">**Kullanılarak ayarlanan**: `UseSetting`
**Ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="a0f42-234">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="a0f42-235">Başlangıç dışlama derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="a0f42-235">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="a0f42-236">Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="a0f42-236">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="a0f42-237">**Anahtar**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="a0f42-237">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="a0f42-238">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-238">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-239">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="a0f42-239">**Default**: Empty string</span></span>  
<span data-ttu-id="a0f42-240">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a0f42-240">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a0f42-241">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="a0f42-241">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="a0f42-242">URL'leri barındırma tercih et</span><span class="sxs-lookup"><span data-stu-id="a0f42-242">Prefer Hosting URLs</span></span>

<span data-ttu-id="a0f42-243">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="a0f42-243">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="a0f42-244">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="a0f42-244">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="a0f42-245">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="a0f42-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a0f42-246">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="a0f42-246">**Default**: true</span></span>  
<span data-ttu-id="a0f42-247">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="a0f42-247">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="a0f42-248">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="a0f42-248">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="a0f42-249">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="a0f42-249">Prevent Hosting Startup</span></span>

<span data-ttu-id="a0f42-250">Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-250">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="a0f42-251">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-251">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="a0f42-252">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="a0f42-252">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="a0f42-253">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="a0f42-253">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="a0f42-254">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="a0f42-254">**Default**: false</span></span>  
<span data-ttu-id="a0f42-255">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="a0f42-255">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="a0f42-256">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="a0f42-256">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="a0f42-257">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="a0f42-257">Server URLs</span></span>

<span data-ttu-id="a0f42-258">IP adresi veya konak bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri adresleriyle gösterir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="a0f42-259">**Anahtar**: URL'ler</span><span class="sxs-lookup"><span data-stu-id="a0f42-259">**Key**: urls</span></span>  
<span data-ttu-id="a0f42-260">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-260">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-261">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="a0f42-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="a0f42-262">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="a0f42-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="a0f42-263">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="a0f42-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="a0f42-264">Bir noktalı virgülle ayrılmış ayarlayın (;) listesini URL ön ekleri sunucunun yanıt vermelidir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="a0f42-265">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="a0f42-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="a0f42-266">Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="a0f42-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="a0f42-267">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="a0f42-268">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-268">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="a0f42-269">Kestrel'i kendi API uç nokta yapılandırması vardır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-269">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="a0f42-270">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-270">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="a0f42-271">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="a0f42-271">Shutdown Timeout</span></span>

<span data-ttu-id="a0f42-272">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-272">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="a0f42-273">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="a0f42-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="a0f42-274">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="a0f42-274">**Type**: *int*</span></span>  
<span data-ttu-id="a0f42-275">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="a0f42-275">**Default**: 5</span></span>  
<span data-ttu-id="a0f42-276">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="a0f42-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="a0f42-277">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="a0f42-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="a0f42-278">Anahtarı kabul eder ancak bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi alır bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="a0f42-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="a0f42-279">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="a0f42-279">During the timeout period, hosting:</span></span>

* <span data-ttu-id="a0f42-280">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="a0f42-280">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="a0f42-281">Barındırılan hizmetler, durdurmak için başarısız olan hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-281">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="a0f42-282">Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur.</span><span class="sxs-lookup"><span data-stu-id="a0f42-282">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="a0f42-283">İşlem tamamlandı olmasanız bile hizmetleri durdurun.</span><span class="sxs-lookup"><span data-stu-id="a0f42-283">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="a0f42-284">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="a0f42-284">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="a0f42-285">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="a0f42-285">Startup Assembly</span></span>

<span data-ttu-id="a0f42-286">Aramak için derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a0f42-286">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="a0f42-287">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="a0f42-287">**Key**: startupAssembly</span></span>  
<span data-ttu-id="a0f42-288">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-288">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-289">**Varsayılan**: Uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="a0f42-289">**Default**: The app's assembly</span></span>  
<span data-ttu-id="a0f42-290">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="a0f42-290">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="a0f42-291">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="a0f42-291">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="a0f42-292">Ada göre bütünleştirilmiş kod (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-292">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="a0f42-293">Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-293">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="a0f42-294">Web kökü</span><span class="sxs-lookup"><span data-stu-id="a0f42-294">Web Root</span></span>

<span data-ttu-id="a0f42-295">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-295">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="a0f42-296">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="a0f42-296">**Key**: webroot</span></span>  
<span data-ttu-id="a0f42-297">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="a0f42-297">**Type**: *string*</span></span>  
<span data-ttu-id="a0f42-298">**Varsayılan**: Belirtilmezse, varsayılan "(Content Root)/wwwroot olur.", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="a0f42-298">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="a0f42-299">Yol mevcut değilse, ardından İşlemsiz dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-299">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="a0f42-300">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="a0f42-300">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="a0f42-301">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="a0f42-301">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="a0f42-302">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="a0f42-302">Override configuration</span></span>

<span data-ttu-id="a0f42-303">Kullanım [yapılandırma](xref:fundamentals/configuration/index) Web ana bilgisayarı yapılandırılamadı.</span><span class="sxs-lookup"><span data-stu-id="a0f42-303">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="a0f42-304">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="a0f42-304">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="a0f42-305">Herhangi bir yapılandırma öğesinden yüklenen *hostsettings.json* dosya tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="a0f42-305">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="a0f42-306">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="a0f42-306">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="a0f42-307">`IWebHostBuilder` yapılandırma, uygulamanın yapılandırma için eklenir, ancak listesiyse true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="a0f42-307">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="a0f42-308">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="a0f42-308">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="a0f42-309">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="a0f42-309">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="a0f42-310">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-310">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="a0f42-311">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="a0f42-311">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="a0f42-312">`UseConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="a0f42-312">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="a0f42-313">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-313">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="a0f42-314">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-314">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="a0f42-315">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="a0f42-315">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="a0f42-316">`UseConfiguration` anahtarlar yalnızca sağlanan akış kopyalar `IConfiguration` konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="a0f42-316">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="a0f42-317">Bu nedenle, ayarı `reloadOnChange: true` JSON INI ve XML ayar dosyaları için hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="a0f42-317">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="a0f42-318">Ana belirli URL'yi belirtmek için istenen değeri bir komut istemi'nden yürütülürken geçirilebilir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="a0f42-318">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="a0f42-319">Komut satırı bağımsız değişkeni geçersiz kılmalar `urls` değerini *hostsettings.json* dosya ve sunucu, 8080 bağlantı noktasında dinler:</span><span class="sxs-lookup"><span data-stu-id="a0f42-319">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="a0f42-320">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="a0f42-320">Manage the host</span></span>

<span data-ttu-id="a0f42-321">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="a0f42-321">**Run**</span></span>

<span data-ttu-id="a0f42-322">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="a0f42-322">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="a0f42-323">**Start**</span><span class="sxs-lookup"><span data-stu-id="a0f42-323">**Start**</span></span>

<span data-ttu-id="a0f42-324">Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a0f42-324">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="a0f42-325">URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="a0f42-325">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="a0f42-326">Uygulamayı başlatın ve önceden yapılandırılmış varsayılan kullanarak yeni bir ana bilgisayarı başlatılamıyor `CreateDefaultBuilder` statik kolaylık yöntemi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="a0f42-326">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="a0f42-327">Bu yöntemler konsol çıktısı olmadan ve sunucuyu başlatın [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için mola bekleyin:</span><span class="sxs-lookup"><span data-stu-id="a0f42-327">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="a0f42-328">**Başlangıç (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-328">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="a0f42-329">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-329">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a0f42-330">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="a0f42-330">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a0f42-331">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-331">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a0f42-332">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-332">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a0f42-333">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-333">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="a0f42-334">Bir URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-334">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="a0f42-335">Aynı sonucu üretir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-335">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="a0f42-336">**Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-336">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a0f42-337">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="a0f42-337">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="a0f42-338">Aşağıdaki tarayıcı isteklerini ile örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="a0f42-338">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="a0f42-339">İstek</span><span class="sxs-lookup"><span data-stu-id="a0f42-339">Request</span></span>                                    | <span data-ttu-id="a0f42-340">Yanıt</span><span class="sxs-lookup"><span data-stu-id="a0f42-340">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="a0f42-341">Martin Merhaba!</span><span class="sxs-lookup"><span data-stu-id="a0f42-341">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="a0f42-342">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="a0f42-342">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="a0f42-343">"Ooops!" dizesi ile bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="a0f42-343">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="a0f42-344">Bir özel durum dizesi ile oluşturur "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="a0f42-344">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="a0f42-345">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="a0f42-345">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="a0f42-346">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="a0f42-346">Hello World!</span></span>                             |

<span data-ttu-id="a0f42-347">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a0f42-348">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a0f42-349">**Başlat (eylem url dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-349">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="a0f42-350">Bir URL örneği kullanıp `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-350">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="a0f42-351">Aynı sonucu üretir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-351">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="a0f42-352">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-352">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a0f42-353">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-353">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="a0f42-354">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="a0f42-354">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="a0f42-355">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="a0f42-356">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="a0f42-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="a0f42-357">**StartWith (url, eylem dizesi&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="a0f42-357">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="a0f42-358">Bir URL ve yapılandıracak bir temsilci sağlar bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-358">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="a0f42-359">Aynı sonucu üretir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a0f42-359">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="a0f42-360">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="a0f42-360">IHostingEnvironment interface</span></span>

<span data-ttu-id="a0f42-361">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-361">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="a0f42-362">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="a0f42-362">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="a0f42-363">A [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortama bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-363">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="a0f42-364">Alternatif olarak, ekleme `IHostingEnvironment` içine `Startup` Oluşturucusu kullanılmak üzere `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a0f42-364">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="a0f42-365">Ek olarak `IsDevelopment` genişletme yöntemi `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a0f42-365">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="a0f42-366">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a0f42-366">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="a0f42-367">`IHostingEnvironment` Hizmet de eklenen doğrudan `Configure` işleme işlem hattı ayarlamaya yönelik yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a0f42-367">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="a0f42-368">`IHostingEnvironment` içine yerleştirilebilir `Invoke` özel oluştururken yöntemi [ara yazılım](xref:fundamentals/middleware/write):</span><span class="sxs-lookup"><span data-stu-id="a0f42-368">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="a0f42-369">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="a0f42-369">IApplicationLifetime interface</span></span>

<span data-ttu-id="a0f42-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0f42-370">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="a0f42-371">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a0f42-371">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="a0f42-372">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="a0f42-372">Cancellation Token</span></span>    | <span data-ttu-id="a0f42-373">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="a0f42-373">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="a0f42-374">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="a0f42-374">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="a0f42-375">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="a0f42-375">The host has fully started.</span></span> |
| [<span data-ttu-id="a0f42-376">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="a0f42-376">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="a0f42-377">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="a0f42-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="a0f42-378">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="a0f42-378">All requests should be processed.</span></span> <span data-ttu-id="a0f42-379">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-379">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="a0f42-380">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="a0f42-380">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="a0f42-381">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="a0f42-381">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="a0f42-382">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="a0f42-382">Requests may still be processing.</span></span> <span data-ttu-id="a0f42-383">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="a0f42-383">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="a0f42-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="a0f42-384">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="a0f42-385">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a0f42-385">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="a0f42-386">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="a0f42-386">Scope validation</span></span>

<span data-ttu-id="a0f42-387">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="a0f42-387">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="a0f42-388">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı, doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="a0f42-388">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="a0f42-389">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-389">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="a0f42-390">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-390">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="a0f42-391">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-391">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="a0f42-392">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-392">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="a0f42-393">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="a0f42-393">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="a0f42-394">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="a0f42-394">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="a0f42-395">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="a0f42-395">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="a0f42-396">Kapsamlar üretim ortamında dahil olmak üzere, her zaman doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="a0f42-396">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="a0f42-397">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a0f42-397">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
