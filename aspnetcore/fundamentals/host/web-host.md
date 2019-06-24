---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu olan ASP.NET Core Web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: c5d5b723b31a5c211a47e378e50be858fda0b2bd
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313795"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="c0b50-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="c0b50-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="c0b50-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c0b50-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c0b50-105">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="c0b50-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="c0b50-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="c0b50-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c0b50-107">En az bir sunucu ve istek işleme ardışık konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="c0b50-108">Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c0b50-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c0b50-109">Bu makale yalnızca geriye dönük uyumluluk için kullanılabilir olmaya devam ettiğinden Web ana kapsar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-109">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="c0b50-110">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-110">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c0b50-111">Bu makale, web uygulamalarını barındırmak için olan Web ana kapsar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-111">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="c0b50-112">Diğer uygulama türleri için kullanın [genel ana bilgisayar](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="c0b50-112">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="c0b50-113">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="c0b50-113">Set up a host</span></span>

<span data-ttu-id="c0b50-114">Bir konak örneği kullanarak oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c0b50-114">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="c0b50-115">Bu, genellikle uygulamanın Giriş noktasında gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-115">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="c0b50-116">Proje şablonlarındaki `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c0b50-116">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="c0b50-117">Normal bir uygulama çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="c0b50-117">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="c0b50-118">Çağıran kodu `CreateDefaultBuilder` adlı bir yöntemde olduğu `CreateWebHostBuilder`, hangi ayırır, koddan `Main` çağrılarının `Run` Oluşturucu nesne üzerinde.</span><span class="sxs-lookup"><span data-stu-id="c0b50-118">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="c0b50-119">Bu ayrımı kullanırsanız gereklidir [Entity Framework Core Araçları](/ef/core/miscellaneous/cli/).</span><span class="sxs-lookup"><span data-stu-id="c0b50-119">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="c0b50-120">Araçlar bulmayı beklediğinize bir `CreateWebHostBuilder` uygulamayı başlatmadan ana bilgisayarı yapılandırmak için tasarım zamanında çağırabilirsiniz yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-120">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="c0b50-121">Alternatif uygulamaktır `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-121">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="c0b50-122">Daha fazla bilgi için [tasarım zamanında DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="c0b50-122">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="c0b50-123">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="c0b50-123">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="c0b50-124">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) uygulamayı kullanarak web sunucusu olarak yapılandırma sağlayıcıları barındıran.</span><span class="sxs-lookup"><span data-stu-id="c0b50-124">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="c0b50-125">Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-125">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="c0b50-126">İçerik kök tarafından döndürülen yol ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="c0b50-126">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="c0b50-127">Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:</span><span class="sxs-lookup"><span data-stu-id="c0b50-127">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="c0b50-128">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="c0b50-128">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="c0b50-129">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c0b50-129">Command-line arguments.</span></span>
* <span data-ttu-id="c0b50-130">Uygulama yapılandırması ile aşağıdaki sırada yükler:</span><span class="sxs-lookup"><span data-stu-id="c0b50-130">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="c0b50-131">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c0b50-131">*appsettings.json*.</span></span>
  * <span data-ttu-id="c0b50-132">*appSettings. {Ortamı} .json*.</span><span class="sxs-lookup"><span data-stu-id="c0b50-132">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="c0b50-133">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="c0b50-133">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c0b50-134">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c0b50-134">Environment variables.</span></span>
  * <span data-ttu-id="c0b50-135">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c0b50-135">Command-line arguments.</span></span>
* <span data-ttu-id="c0b50-136">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktı.</span><span class="sxs-lookup"><span data-stu-id="c0b50-136">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="c0b50-137">Günlük kaydı içerir [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="c0b50-137">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="c0b50-138">Arkasında IIS ile çalıştırırken [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` sağlayan [IIS tümleştirme](xref:host-and-deploy/iis/index), uygulamanın taban adresini ve bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-138">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="c0b50-139">IIS tümleştirme, ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="c0b50-139">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="c0b50-140">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-140">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="c0b50-141">Kümeleri [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="c0b50-141">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="c0b50-142">Daha fazla bilgi için [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="c0b50-142">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="c0b50-143">Tarafından tanımlanan yapılandırma `CreateDefaultBuilder` geçersiz kılındı ve tarafından Genişletilmiş [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), diğer yöntemler ve uzantı yöntemlerinin [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="c0b50-143">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="c0b50-144">Aşağıda birkaç örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c0b50-144">A few examples follow:</span></span>

* <span data-ttu-id="c0b50-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulama için.</span><span class="sxs-lookup"><span data-stu-id="c0b50-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="c0b50-146">Aşağıdaki `ConfigureAppConfiguration` çağrıyı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya.</span><span class="sxs-lookup"><span data-stu-id="c0b50-146">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="c0b50-147">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-147">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="c0b50-148">Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) geçerli değil olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c0b50-148">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="c0b50-149">Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.</span><span class="sxs-lookup"><span data-stu-id="c0b50-149">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="c0b50-150">Aşağıdaki `ConfigureLogging` en düşük günlük düzeyi yapılandıracak bir temsilci çağrısı ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="c0b50-150">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="c0b50-151">Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-151">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c0b50-152">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-152">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c0b50-153">Aşağıdaki çağrı `ConfigureKestrel` varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-153">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="c0b50-154">Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-154">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="c0b50-155">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-155">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c0b50-156">Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-156">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c0b50-157">Kullanılan varsayılan [Visual Studio](https://visualstudio.microsoft.com) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="c0b50-157">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c0b50-158">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-158">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="c0b50-159">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` konaktan oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) desteklenen bir yaklaşım ile ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="c0b50-159">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="c0b50-160">Bir ana bilgisayar, ayarlarken [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-160">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="c0b50-161">Varsa bir `Startup` sınıf belirtilmişse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-161">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="c0b50-162">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-162">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="c0b50-163">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c0b50-163">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="c0b50-164">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="c0b50-164">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c0b50-165">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="c0b50-165">Host configuration values</span></span>

<span data-ttu-id="c0b50-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="c0b50-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="c0b50-167">Ortam değişkenlerini biçiminde içerir konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-167">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="c0b50-168">Örneğin: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="c0b50-168">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="c0b50-169">Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).</span><span class="sxs-lookup"><span data-stu-id="c0b50-169">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="c0b50-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="c0b50-171">Bir değerle ayarlarken `UseSetting`, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-171">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="c0b50-172">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-172">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="c0b50-173">Daha fazla bilgi için [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="c0b50-173">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="c0b50-174">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="c0b50-174">Application Key (Name)</span></span>

<span data-ttu-id="c0b50-175">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanmış olduğunda [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) konak yapım sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-175">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="c0b50-176">Değer, uygulamanın giriş noktasını içeren derleme adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-176">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="c0b50-177">Değeri açıkça ayarlamak için kullanın [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="c0b50-177">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="c0b50-178">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="c0b50-178">**Key**: applicationName</span></span>  
<span data-ttu-id="c0b50-179">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-179">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-180">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="c0b50-180">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="c0b50-181">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0b50-181">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0b50-182">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="c0b50-182">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="c0b50-183">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="c0b50-183">Capture Startup Errors</span></span>

<span data-ttu-id="c0b50-184">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-184">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="c0b50-185">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="c0b50-185">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="c0b50-186">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c0b50-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0b50-187">**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-187">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="c0b50-188">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="c0b50-188">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="c0b50-189">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="c0b50-189">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="c0b50-190">Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-190">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="c0b50-191">Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="c0b50-191">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="c0b50-192">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="c0b50-192">Content Root</span></span>

<span data-ttu-id="c0b50-193">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramasını başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-193">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="c0b50-194">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="c0b50-194">**Key**: contentRoot</span></span>  
<span data-ttu-id="c0b50-195">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-195">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-196">**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.</span><span class="sxs-lookup"><span data-stu-id="c0b50-196">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="c0b50-197">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="c0b50-197">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="c0b50-198">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="c0b50-198">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="c0b50-199">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="c0b50-199">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="c0b50-200">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c0b50-200">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="c0b50-201">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="c0b50-201">Detailed Errors</span></span>

<span data-ttu-id="c0b50-202">Ayrıntılı hatalar yakalanmasının gerekip belirler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-202">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="c0b50-203">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="c0b50-203">**Key**: detailedErrors</span></span>  
<span data-ttu-id="c0b50-204">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c0b50-204">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0b50-205">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="c0b50-205">**Default**: false</span></span>  
<span data-ttu-id="c0b50-206">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0b50-206">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0b50-207">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="c0b50-207">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="c0b50-208">Etkin olduğunda (veya <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumlar yakalar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-208">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="c0b50-209">Ortam</span><span class="sxs-lookup"><span data-stu-id="c0b50-209">Environment</span></span>

<span data-ttu-id="c0b50-210">Uygulamanın ortamı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-210">Sets the app's environment.</span></span>

<span data-ttu-id="c0b50-211">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="c0b50-211">**Key**: environment</span></span>  
<span data-ttu-id="c0b50-212">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-212">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-213">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="c0b50-213">**Default**: Production</span></span>  
<span data-ttu-id="c0b50-214">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="c0b50-214">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="c0b50-215">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="c0b50-215">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="c0b50-216">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-216">The environment can be set to any value.</span></span> <span data-ttu-id="c0b50-217">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="c0b50-218">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-218">Values aren't case sensitive.</span></span> <span data-ttu-id="c0b50-219">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="c0b50-219">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="c0b50-220">Kullanırken [Visual Studio](https://visualstudio.microsoft.com), ortam değişkenleri ayarlanabilir *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="c0b50-220">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="c0b50-221">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-221">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="c0b50-222">Başlangıç derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="c0b50-222">Hosting Startup Assemblies</span></span>

<span data-ttu-id="c0b50-223">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-223">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="c0b50-224">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="c0b50-224">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="c0b50-225">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-225">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-226">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="c0b50-226">**Default**: Empty string</span></span>  
<span data-ttu-id="c0b50-227">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0b50-227">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0b50-228">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c0b50-228">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="c0b50-229">Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-229">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="c0b50-230">Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c0b50-230">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="c0b50-231">Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-231">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="c0b50-232">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="c0b50-232">HTTPS Port</span></span>

<span data-ttu-id="c0b50-233">HTTPS, yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c0b50-233">Set the HTTPS redirect port.</span></span> <span data-ttu-id="c0b50-234">Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c0b50-234">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="c0b50-235">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="c0b50-235">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="c0b50-236">**Kullanılarak ayarlanan**: `UseSetting`
**Ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="c0b50-236">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="c0b50-237">Başlangıç dışlama derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="c0b50-237">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="c0b50-238">Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-238">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="c0b50-239">**Anahtar**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="c0b50-239">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="c0b50-240">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-240">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-241">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="c0b50-241">**Default**: Empty string</span></span>  
<span data-ttu-id="c0b50-242">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0b50-242">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0b50-243">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c0b50-243">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="c0b50-244">URL'leri barındırma tercih et</span><span class="sxs-lookup"><span data-stu-id="c0b50-244">Prefer Hosting URLs</span></span>

<span data-ttu-id="c0b50-245">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="c0b50-245">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="c0b50-246">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="c0b50-246">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="c0b50-247">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c0b50-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0b50-248">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="c0b50-248">**Default**: true</span></span>  
<span data-ttu-id="c0b50-249">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="c0b50-249">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="c0b50-250">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="c0b50-250">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="c0b50-251">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="c0b50-251">Prevent Hosting Startup</span></span>

<span data-ttu-id="c0b50-252">Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-252">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="c0b50-253">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-253">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="c0b50-254">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="c0b50-254">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="c0b50-255">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c0b50-255">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c0b50-256">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="c0b50-256">**Default**: false</span></span>  
<span data-ttu-id="c0b50-257">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c0b50-257">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c0b50-258">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="c0b50-258">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="c0b50-259">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="c0b50-259">Server URLs</span></span>

<span data-ttu-id="c0b50-260">IP adresi veya konak bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri adresleriyle gösterir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-260">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="c0b50-261">**Anahtar**: URL'ler</span><span class="sxs-lookup"><span data-stu-id="c0b50-261">**Key**: urls</span></span>  
<span data-ttu-id="c0b50-262">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-262">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-263">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="c0b50-263">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="c0b50-264">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="c0b50-264">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="c0b50-265">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c0b50-265">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="c0b50-266">Bir noktalı virgülle ayrılmış ayarlayın (;) listesini URL ön ekleri sunucunun yanıt vermelidir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-266">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="c0b50-267">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="c0b50-267">For example, `http://localhost:123`.</span></span> <span data-ttu-id="c0b50-268">Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="c0b50-268">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="c0b50-269">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-269">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="c0b50-270">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-270">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="c0b50-271">Kestrel'i kendi API uç nokta yapılandırması vardır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-271">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="c0b50-272">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-272">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="c0b50-273">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="c0b50-273">Shutdown Timeout</span></span>

<span data-ttu-id="c0b50-274">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-274">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="c0b50-275">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="c0b50-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="c0b50-276">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="c0b50-276">**Type**: *int*</span></span>  
<span data-ttu-id="c0b50-277">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="c0b50-277">**Default**: 5</span></span>  
<span data-ttu-id="c0b50-278">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="c0b50-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="c0b50-279">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="c0b50-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="c0b50-280">Anahtarı kabul eder ancak bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi alır bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="c0b50-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="c0b50-281">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="c0b50-281">During the timeout period, hosting:</span></span>

* <span data-ttu-id="c0b50-282">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="c0b50-282">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="c0b50-283">Barındırılan hizmetler, durdurmak için başarısız olan hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-283">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="c0b50-284">Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur.</span><span class="sxs-lookup"><span data-stu-id="c0b50-284">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="c0b50-285">İşlem tamamlandı olmasanız bile hizmetleri durdurun.</span><span class="sxs-lookup"><span data-stu-id="c0b50-285">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="c0b50-286">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="c0b50-286">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="c0b50-287">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="c0b50-287">Startup Assembly</span></span>

<span data-ttu-id="c0b50-288">Aramak için derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c0b50-288">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="c0b50-289">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="c0b50-289">**Key**: startupAssembly</span></span>  
<span data-ttu-id="c0b50-290">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-290">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-291">**Varsayılan**: Uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="c0b50-291">**Default**: The app's assembly</span></span>  
<span data-ttu-id="c0b50-292">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="c0b50-292">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="c0b50-293">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="c0b50-293">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="c0b50-294">Ada göre bütünleştirilmiş kod (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-294">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="c0b50-295">Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-295">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="c0b50-296">Web kökü</span><span class="sxs-lookup"><span data-stu-id="c0b50-296">Web Root</span></span>

<span data-ttu-id="c0b50-297">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-297">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="c0b50-298">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="c0b50-298">**Key**: webroot</span></span>  
<span data-ttu-id="c0b50-299">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c0b50-299">**Type**: *string*</span></span>  
<span data-ttu-id="c0b50-300">**Varsayılan**: Belirtilmezse, varsayılan "(Content Root)/wwwroot olur.", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="c0b50-300">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="c0b50-301">Yol mevcut değilse, ardından İşlemsiz dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-301">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="c0b50-302">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="c0b50-302">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="c0b50-303">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="c0b50-303">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="c0b50-304">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="c0b50-304">Override configuration</span></span>

<span data-ttu-id="c0b50-305">Kullanım [yapılandırma](xref:fundamentals/configuration/index) Web ana bilgisayarı yapılandırılamadı.</span><span class="sxs-lookup"><span data-stu-id="c0b50-305">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="c0b50-306">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="c0b50-306">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="c0b50-307">Herhangi bir yapılandırma öğesinden yüklenen *hostsettings.json* dosya tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="c0b50-307">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="c0b50-308">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="c0b50-308">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="c0b50-309">`IWebHostBuilder` yapılandırma, uygulamanın yapılandırma için eklenir, ancak listesiyse true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="c0b50-309">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="c0b50-310">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="c0b50-310">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="c0b50-311">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="c0b50-311">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="c0b50-312">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-312">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="c0b50-313">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="c0b50-313">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="c0b50-314">`UseConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="c0b50-314">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="c0b50-315">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-315">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="c0b50-316">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-316">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="c0b50-317">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="c0b50-317">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="c0b50-318">`UseConfiguration` anahtarlar yalnızca sağlanan akış kopyalar `IConfiguration` konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="c0b50-318">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="c0b50-319">Bu nedenle, ayarı `reloadOnChange: true` JSON INI ve XML ayar dosyaları için hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="c0b50-319">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="c0b50-320">Ana belirli URL'yi belirtmek için istenen değeri bir komut istemi'nden yürütülürken geçirilebilir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="c0b50-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="c0b50-321">Komut satırı bağımsız değişkeni geçersiz kılmalar `urls` değerini *hostsettings.json* dosya ve sunucu, 8080 bağlantı noktasında dinler:</span><span class="sxs-lookup"><span data-stu-id="c0b50-321">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="c0b50-322">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="c0b50-322">Manage the host</span></span>

<span data-ttu-id="c0b50-323">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="c0b50-323">**Run**</span></span>

<span data-ttu-id="c0b50-324">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="c0b50-324">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c0b50-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="c0b50-325">**Start**</span></span>

<span data-ttu-id="c0b50-326">Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c0b50-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c0b50-327">URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="c0b50-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="c0b50-328">Uygulamayı başlatın ve önceden yapılandırılmış varsayılan kullanarak yeni bir ana bilgisayarı başlatılamıyor `CreateDefaultBuilder` statik kolaylık yöntemi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="c0b50-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="c0b50-329">Bu yöntemler konsol çıktısı olmadan ve sunucuyu başlatın [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için mola bekleyin:</span><span class="sxs-lookup"><span data-stu-id="c0b50-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="c0b50-330">**Başlangıç (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="c0b50-331">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c0b50-332">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="c0b50-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c0b50-333">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0b50-334">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0b50-335">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="c0b50-336">Bir URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c0b50-337">Aynı sonucu üretir **başlangıç (RequestDelegate uygulama)** , uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="c0b50-338">**Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c0b50-339">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="c0b50-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="c0b50-340">Aşağıdaki tarayıcı isteklerini ile örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="c0b50-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="c0b50-341">İstek</span><span class="sxs-lookup"><span data-stu-id="c0b50-341">Request</span></span>                                    | <span data-ttu-id="c0b50-342">Yanıt</span><span class="sxs-lookup"><span data-stu-id="c0b50-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="c0b50-343">Martin Merhaba!</span><span class="sxs-lookup"><span data-stu-id="c0b50-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="c0b50-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="c0b50-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="c0b50-345">"Ooops!" dizesi ile bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="c0b50-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="c0b50-346">Bir özel durum dizesi ile oluşturur "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="c0b50-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="c0b50-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="c0b50-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="c0b50-348">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="c0b50-348">Hello World!</span></span>                             |

<span data-ttu-id="c0b50-349">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0b50-350">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0b50-351">**Başlat (eylem url dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c0b50-352">Bir URL örneği kullanıp `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="c0b50-353">Aynı sonucu üretir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)** , uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-353">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="c0b50-354">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c0b50-355">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c0b50-356">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="c0b50-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c0b50-357">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c0b50-358">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="c0b50-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c0b50-359">**StartWith (url, eylem dizesi&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="c0b50-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c0b50-360">Bir URL ve yapılandıracak bir temsilci sağlar bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c0b50-361">Aynı sonucu üretir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)** , uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="c0b50-361">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="c0b50-362">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="c0b50-362">IHostingEnvironment interface</span></span>

<span data-ttu-id="c0b50-363">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-363">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="c0b50-364">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="c0b50-364">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="c0b50-365">A [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortama bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-365">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="c0b50-366">Alternatif olarak, ekleme `IHostingEnvironment` içine `Startup` Oluşturucusu kullanılmak üzere `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c0b50-366">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="c0b50-367">Ek olarak `IsDevelopment` genişletme yöntemi `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="c0b50-367">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="c0b50-368">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c0b50-368">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="c0b50-369">`IHostingEnvironment` Hizmet de eklenen doğrudan `Configure` işleme işlem hattı ayarlamaya yönelik yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c0b50-369">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="c0b50-370">`IHostingEnvironment` içine yerleştirilebilir `Invoke` özel oluştururken yöntemi [ara yazılım](xref:fundamentals/middleware/write):</span><span class="sxs-lookup"><span data-stu-id="c0b50-370">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="c0b50-371">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="c0b50-371">IApplicationLifetime interface</span></span>

<span data-ttu-id="c0b50-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="c0b50-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="c0b50-373">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="c0b50-373">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="c0b50-374">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="c0b50-374">Cancellation Token</span></span>    | <span data-ttu-id="c0b50-375">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="c0b50-375">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="c0b50-376">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="c0b50-376">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="c0b50-377">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="c0b50-377">The host has fully started.</span></span> |
| [<span data-ttu-id="c0b50-378">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="c0b50-378">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="c0b50-379">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="c0b50-379">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="c0b50-380">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="c0b50-380">All requests should be processed.</span></span> <span data-ttu-id="c0b50-381">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-381">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="c0b50-382">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="c0b50-382">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="c0b50-383">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="c0b50-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="c0b50-384">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="c0b50-384">Requests may still be processing.</span></span> <span data-ttu-id="c0b50-385">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="c0b50-385">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="c0b50-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="c0b50-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="c0b50-387">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="c0b50-387">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="c0b50-388">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="c0b50-388">Scope validation</span></span>

<span data-ttu-id="c0b50-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="c0b50-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="c0b50-390">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı, doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="c0b50-390">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c0b50-391">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-391">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c0b50-392">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-392">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c0b50-393">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-393">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="c0b50-394">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-394">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c0b50-395">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="c0b50-395">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c0b50-396">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="c0b50-396">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c0b50-397">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c0b50-397">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c0b50-398">Kapsamlar üretim ortamında dahil olmak üzere, her zaman doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="c0b50-398">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="c0b50-399">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c0b50-399">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
