---
title: ASP.NET Core Web ana bilgisayarı
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu olan ASP.NET Core Web ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: bc18b5490d232758b796d33a62cd8d1a7dd7289f
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007101"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d75a4-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="d75a4-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d75a4-104">ASP.NET Core uygulamalar bir *Konağı*yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-104">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d75a4-105">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d75a4-106">Ana bilgisayar, en azından bir sunucu ve bir istek işleme işlem hattı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-106">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d75a4-107">Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-107">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d75a4-108">Bu makalede, yalnızca geriye dönük uyumluluk için kullanılabilen Web ana bilgisayarı ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-108">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="d75a4-109">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-109">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d75a4-110">Bu makale, Web uygulamalarını barındırmak için olan Web konağını ele alır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-110">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="d75a4-111">Diğer uygulama türleri için [genel Konağı](xref:fundamentals/host/generic-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-111">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="d75a4-112">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="d75a4-112">Set up a host</span></span>

<span data-ttu-id="d75a4-113">[Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın bir örneğini kullanarak bir konak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d75a4-113">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d75a4-114">Bu genellikle uygulamanın giriş noktasında `Main` yöntemi olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-114">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="d75a4-115">Proje şablonlarında, `Main` *program.cs*' de bulunur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-115">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d75a4-116">Tipik bir uygulama, bir konak ayarlamaya başlamak için [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağırır:</span><span class="sxs-lookup"><span data-stu-id="d75a4-116">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="d75a4-117">`CreateDefaultBuilder` çağıran kod, Oluşturucu nesnesinde `Run` çağıran `Main` koddan ayıran `CreateWebHostBuilder`adlı bir yöntemde bulunur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-117">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="d75a4-118">[Entity Framework Core araçlarını](/ef/core/miscellaneous/cli/)kullanıyorsanız bu ayrım gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-118">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="d75a4-119">Araçlar, uygulamayı çalıştırmadan ana bilgisayarı yapılandırmak için tasarım zamanında çağırabilecekleri bir `CreateWebHostBuilder` yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-119">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="d75a4-120">Diğer bir seçenek de `IDesignTimeDbContextFactory`uygulamaktır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-120">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="d75a4-121">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="d75a4-121">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="d75a4-122">`CreateDefaultBuilder` aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-122">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d75a4-123">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu, uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak Web sunucusu olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-123">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d75a4-124">Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-124">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d75a4-125">[İçerik kökünü](xref:fundamentals/index#content-root) [Directory. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-125">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d75a4-126">[Ana bilgisayar yapılandırmasını](#host-configuration-values) şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="d75a4-126">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d75a4-127">`ASPNETCORE_` ön eki olan ortam değişkenleri (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="d75a4-127">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d75a4-128">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d75a4-128">Command-line arguments.</span></span>
* <span data-ttu-id="d75a4-129">Aşağıdaki sırayla uygulama yapılandırmasını yükler:</span><span class="sxs-lookup"><span data-stu-id="d75a4-129">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="d75a4-130">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d75a4-130">*appsettings.json*.</span></span>
  * <span data-ttu-id="d75a4-131">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d75a4-131">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d75a4-132">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="d75a4-132">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d75a4-133">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d75a4-133">Environment variables.</span></span>
  * <span data-ttu-id="d75a4-134">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="d75a4-134">Command-line arguments.</span></span>
* <span data-ttu-id="d75a4-135">Konsol ve hata ayıklama çıkışı için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-135">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d75a4-136">Günlüğe kaydetme, bir *appSettings. JSON* veya appSettings 'in günlük yapılandırma bölümünde belirtilen [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) kurallarını içerir *. { Environment}. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="d75a4-136">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d75a4-137">[ASP.NET Core MODÜLÜYLE](xref:host-and-deploy/aspnet-core-module)IIS 'nin arkasında çalışırken, `CreateDefaultBuilder`, uygulamanın temel adresini ve bağlantı noktasını yapılandıran [IIS tümleştirmesini](xref:host-and-deploy/iis/index)mümkün bir şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-137">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="d75a4-138">IIS tümleştirmesi, uygulamayı [başlatma hatalarını yakalamaya](#capture-startup-errors)de yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-138">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d75a4-139">IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-139">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d75a4-140">Uygulamanın ortamı geliştirme ise, [Serviceprovideroptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini `true` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-140">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d75a4-141">Daha fazla bilgi için bkz. [kapsam doğrulaması](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="d75a4-141">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d75a4-142">`CreateDefaultBuilder` tarafından tanımlanan yapılandırma, [Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [configurelogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın diğer yöntemleri ve genişletme yöntemleri tarafından geçersiz kılınabilir ve genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-142">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d75a4-143">Birkaç örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-143">A few examples follow:</span></span>

* <span data-ttu-id="d75a4-144">[Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) , uygulama için ek `IConfiguration` belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d75a4-145">Aşağıdaki `ConfigureAppConfiguration` çağrısı, *appSettings. xml* dosyasına uygulama yapılandırmasını dahil etmek için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-145">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d75a4-146">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-146">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d75a4-147">Bu yapılandırmanın ana bilgisayar için (örneğin, sunucu URL 'Leri veya ortam) uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-147">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d75a4-148">[Konak yapılandırma değerleri](#host-configuration-values) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-148">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d75a4-149">Aşağıdaki `ConfigureLogging` çağrısı, en düşük günlük düzeyi ([Setminimumlevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) değerini [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel)olarak yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-149">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d75a4-150">Bu ayar appSettings 'teki ayarları geçersiz kılar *. Development. JSON* (`LogLevel.Debug`) ve *appSettings. Production. JSON* (`LogLevel.Error`) `CreateDefaultBuilder`tarafından yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="d75a4-150">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d75a4-151">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-151">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="d75a4-152">Aşağıdaki `ConfigureKestrel` çağrısı varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel `CreateDefaultBuilder`tarafından yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:</span><span class="sxs-lookup"><span data-stu-id="d75a4-152">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="d75a4-153">Aşağıdaki [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) çağrısı, varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel `CreateDefaultBuilder`tarafından yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:</span><span class="sxs-lookup"><span data-stu-id="d75a4-153">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="d75a4-154">[İçerik kökü](xref:fundamentals/index#content-root) , konağın MVC görünüm dosyaları gibi içerik dosyalarını arayacağı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-154">The [content root](xref:fundamentals/index#content-root) determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d75a4-155">Uygulama, projenin kök klasöründen başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-155">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d75a4-156">Bu, [Visual Studio](https://visualstudio.microsoft.com) 'da ve [DotNet yeni şablonlarda](/dotnet/core/tools/dotnet-new)kullanılan varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-156">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d75a4-157">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-157">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d75a4-158">Statik `CreateDefaultBuilder` yönteminin kullanılmasına alternatif olarak, [Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 'dan bir konak oluşturmak, ASP.NET Core 2. x ile desteklenen bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-158">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="d75a4-159">Bir ana bilgisayar ayarlanırken, [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="d75a4-160">Bir `Startup` sınıfı belirtilmişse, bir `Configure` yöntemi tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d75a4-161">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d75a4-162">`ConfigureServices` birden çok çağrısı birbirine eklenir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d75a4-163">`WebHostBuilder` `Configure` veya `UseStartup` birden çok çağrı önceki ayarların yerini alır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d75a4-164">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="d75a4-164">Host configuration values</span></span>

<span data-ttu-id="d75a4-165">[Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) , ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımları kullanır:</span><span class="sxs-lookup"><span data-stu-id="d75a4-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d75a4-166">`ASPNETCORE_{configurationKey}`biçimindeki ortam değişkenlerini içeren konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="d75a4-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d75a4-167">Örneğin: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d75a4-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d75a4-168">[Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) gibi uzantılar ( [geçersiz kılma yapılandırması](#override-configuration) bölümüne bakın).</span><span class="sxs-lookup"><span data-stu-id="d75a4-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d75a4-169">[Usesetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d75a4-170">`UseSetting`ile bir değer ayarlarken, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d75a4-171">Konak, bir değeri en son ayarlayan seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d75a4-172">Daha fazla bilgi için, sonraki bölümde [yapılandırmayı geçersiz kılma](#override-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d75a4-173">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="d75a4-173">Application Key (Name)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d75a4-174">`IWebHostEnvironment.ApplicationName` özelliği, konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-174">The `IWebHostEnvironment.ApplicationName` property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d75a4-175">Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d75a4-176">Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d75a4-177">Konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında, [ıhostingenvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-177">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d75a4-178">Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-178">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d75a4-179">Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-179">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

::: moniker-end

<span data-ttu-id="d75a4-180">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="d75a4-180">**Key**: applicationName</span></span>  
<span data-ttu-id="d75a4-181">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-181">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-182">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="d75a4-182">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d75a4-183">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d75a4-183">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d75a4-184">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="d75a4-184">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="d75a4-185">Yakalama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="d75a4-185">Capture Startup Errors</span></span>

<span data-ttu-id="d75a4-186">Bu ayar, başlatma hatalarının yakalanmasını denetler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-186">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d75a4-187">**Anahtar**: capturestartuperrors</span><span class="sxs-lookup"><span data-stu-id="d75a4-187">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d75a4-188">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d75a4-188">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d75a4-189">**Varsayılan**: uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-189">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d75a4-190">Şunu **kullanarak ayarla**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d75a4-190">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d75a4-191">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d75a4-191">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d75a4-192">`false`, başlangıç sırasında hata durumunda çıkış sırasında hatalar oluştu.</span><span class="sxs-lookup"><span data-stu-id="d75a4-192">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d75a4-193">`true`, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-193">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="d75a4-194">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-194">Content root</span></span>

<span data-ttu-id="d75a4-195">Bu ayar ASP.NET Core içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-195">This setting determines where ASP.NET Core begins searching for content files.</span></span>

<span data-ttu-id="d75a4-196">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="d75a4-196">**Key**: contentRoot</span></span>  
<span data-ttu-id="d75a4-197">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-197">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-198">**Varsayılan**: uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-198">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d75a4-199">Şunu **kullanarak ayarla**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d75a4-199">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d75a4-200">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d75a4-200">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d75a4-201">İçerik kökü, [Web kökünün](xref:fundamentals/index#web-root)temel yolu olarak da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-201">The content root is also used as the base path for the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="d75a4-202">İçerik kök yolu yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="d75a4-202">If the content root path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

<span data-ttu-id="d75a4-203">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="d75a4-203">For more information, see:</span></span>

* [<span data-ttu-id="d75a4-204">Temel bilgiler: Içerik kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-204">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="d75a4-205">Web kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-205">Web root</span></span>](#web-root)

### <a name="detailed-errors"></a><span data-ttu-id="d75a4-206">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="d75a4-206">Detailed Errors</span></span>

<span data-ttu-id="d75a4-207">Ayrıntılı hataların yakalanıp yakalanmayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-207">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d75a4-208">**Anahtar**: detailederrors</span><span class="sxs-lookup"><span data-stu-id="d75a4-208">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d75a4-209">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d75a4-209">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d75a4-210">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="d75a4-210">**Default**: false</span></span>  
<span data-ttu-id="d75a4-211">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d75a4-211">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d75a4-212">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d75a4-212">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d75a4-213">Etkinleştirildiğinde (veya <a href="#environment">ortam</a> `Development`olarak ayarlandığında), uygulama ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-213">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="d75a4-214">Ortam</span><span class="sxs-lookup"><span data-stu-id="d75a4-214">Environment</span></span>

<span data-ttu-id="d75a4-215">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-215">Sets the app's environment.</span></span>

<span data-ttu-id="d75a4-216">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="d75a4-216">**Key**: environment</span></span>  
<span data-ttu-id="d75a4-217">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-217">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-218">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="d75a4-218">**Default**: Production</span></span>  
<span data-ttu-id="d75a4-219">Şunu **kullanarak ayarla**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d75a4-219">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d75a4-220">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d75a4-220">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d75a4-221">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-221">The environment can be set to any value.</span></span> <span data-ttu-id="d75a4-222">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-222">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d75a4-223">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-223">Values aren't case sensitive.</span></span> <span data-ttu-id="d75a4-224">Varsayılan olarak, *ortam* `ASPNETCORE_ENVIRONMENT` ortam değişkeninden okunurdur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-224">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d75a4-225">[Visual Studio](https://visualstudio.microsoft.com)kullanılırken, *launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-225">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d75a4-226">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-226">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d75a4-227">Başlangıç derlemelerini barındırma</span><span class="sxs-lookup"><span data-stu-id="d75a4-227">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d75a4-228">Uygulamanın barındırma başlangıç derlemelerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-228">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d75a4-229">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d75a4-229">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d75a4-230">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-230">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-231">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="d75a4-231">**Default**: Empty string</span></span>  
<span data-ttu-id="d75a4-232">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d75a4-232">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d75a4-233">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d75a4-233">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d75a4-234">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="d75a4-234">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d75a4-235">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-235">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d75a4-236">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-236">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="d75a4-237">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="d75a4-237">HTTPS Port</span></span>

<span data-ttu-id="d75a4-238">HTTPS yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-238">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d75a4-239">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-239">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d75a4-240">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-240">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d75a4-241">Şunu **kullanarak ayarla**: `UseSetting`
**ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d75a4-241">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="d75a4-242">Barındırma başlatma derlemeleri dışlama</span><span class="sxs-lookup"><span data-stu-id="d75a4-242">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="d75a4-243">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="d75a4-243">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="d75a4-244">**Anahtar**: hostingstartupexcludeassemblies</span><span class="sxs-lookup"><span data-stu-id="d75a4-244">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="d75a4-245">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-245">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-246">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="d75a4-246">**Default**: Empty string</span></span>  
<span data-ttu-id="d75a4-247">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d75a4-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d75a4-248">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d75a4-248">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d75a4-249">Barındırma URL 'Lerini tercih et</span><span class="sxs-lookup"><span data-stu-id="d75a4-249">Prefer Hosting URLs</span></span>

<span data-ttu-id="d75a4-250">Konağın `IServer` uygulamayla yapılandırılanlar yerine `WebHostBuilder` ile yapılandırılan URL 'lerde dinleme yapıp kullanmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-250">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d75a4-251">**Anahtar**: preferhostingurl 'leri</span><span class="sxs-lookup"><span data-stu-id="d75a4-251">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d75a4-252">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d75a4-252">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d75a4-253">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="d75a4-253">**Default**: true</span></span>  
<span data-ttu-id="d75a4-254">Şunu **kullanarak ayarla**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d75a4-254">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d75a4-255">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d75a4-255">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d75a4-256">Barındırma başlangıcını engelle</span><span class="sxs-lookup"><span data-stu-id="d75a4-256">Prevent Hosting Startup</span></span>

<span data-ttu-id="d75a4-257">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="d75a4-257">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d75a4-258">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-258">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d75a4-259">**Anahtar**: koruyucu thostingstartup</span><span class="sxs-lookup"><span data-stu-id="d75a4-259">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d75a4-260">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="d75a4-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d75a4-261">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="d75a4-261">**Default**: false</span></span>  
<span data-ttu-id="d75a4-262">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d75a4-262">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d75a4-263">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d75a4-263">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="d75a4-264">Sunucu URL 'Leri</span><span class="sxs-lookup"><span data-stu-id="d75a4-264">Server URLs</span></span>

<span data-ttu-id="d75a4-265">Sunucunun istekler için dinlemesi gereken bağlantı noktaları ve protokoller içeren IP adreslerini veya ana bilgisayar adreslerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-265">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d75a4-266">**Anahtar**: URL 'ler</span><span class="sxs-lookup"><span data-stu-id="d75a4-266">**Key**: urls</span></span>  
<span data-ttu-id="d75a4-267">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-267">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-268">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d75a4-268">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d75a4-269">Şunu **kullanarak ayarla**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d75a4-269">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d75a4-270">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d75a4-270">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d75a4-271">Noktalı virgülle ayrılmış olarak ayarlayın (;) sunucunun yanıtlaması gereken URL ön eklerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="d75a4-271">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d75a4-272">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="d75a4-272">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d75a4-273">Sunucunun belirtilen bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-273">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d75a4-274">Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-274">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d75a4-275">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-275">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d75a4-276">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-276">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d75a4-277">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-277">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="d75a4-278">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="d75a4-278">Shutdown Timeout</span></span>

<span data-ttu-id="d75a4-279">Web konağının kapanması için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-279">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="d75a4-280">**Anahtar**: shutdowntimeoutseconds</span><span class="sxs-lookup"><span data-stu-id="d75a4-280">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d75a4-281">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="d75a4-281">**Type**: *int*</span></span>  
<span data-ttu-id="d75a4-282">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="d75a4-282">**Default**: 5</span></span>  
<span data-ttu-id="d75a4-283">Şunu **kullanarak ayarla**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d75a4-283">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d75a4-284">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d75a4-284">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d75a4-285">Anahtar, `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`) bir *int* kabul etse de, [useshutdowntimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi bir [TimeSpan](/dotnet/api/system.timespan)alır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-285">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d75a4-286">Zaman aşımı süresi boyunca barındırma:</span><span class="sxs-lookup"><span data-stu-id="d75a4-286">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d75a4-287">[Iapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)öğesini tetikler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-287">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d75a4-288">Üzerinde durmayacak hizmetlerin hatalarını günlüğe kaydetmek için barındırılan hizmetleri durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-288">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d75a4-289">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-289">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d75a4-290">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-290">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d75a4-291">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-291">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="d75a4-292">Başlangıç derlemesi</span><span class="sxs-lookup"><span data-stu-id="d75a4-292">Startup Assembly</span></span>

<span data-ttu-id="d75a4-293">`Startup` sınıfı için arama yapılacak derlemeyi belirler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-293">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d75a4-294">**Anahtar**: startupassembly</span><span class="sxs-lookup"><span data-stu-id="d75a4-294">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d75a4-295">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-295">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-296">**Varsayılan**: uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="d75a4-296">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d75a4-297">Şunu **kullanarak ayarla**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d75a4-297">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d75a4-298">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d75a4-298">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d75a4-299">Ada (`string`) veya türe (`TStartup`) göre derlemeye başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-299">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d75a4-300">Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-300">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="d75a4-301">Web kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-301">Web root</span></span>

<span data-ttu-id="d75a4-302">Uygulamanın statik varlıklarının göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-302">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d75a4-303">**Anahtar**: Webroot</span><span class="sxs-lookup"><span data-stu-id="d75a4-303">**Key**: webroot</span></span>  
<span data-ttu-id="d75a4-304">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="d75a4-304">**Type**: *string*</span></span>  
<span data-ttu-id="d75a4-305">**Varsayılan**: varsayılan `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-305">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="d75a4-306">*{Content root}/Wwwroot* yolu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-306">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="d75a4-307">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-307">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="d75a4-308">Şunu **kullanarak ayarla**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d75a4-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d75a4-309">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d75a4-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

<span data-ttu-id="d75a4-310">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="d75a4-310">For more information, see:</span></span>

* [<span data-ttu-id="d75a4-311">Temel bilgiler: Web kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-311">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="d75a4-312">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="d75a4-312">Content root</span></span>](#content-root)

## <a name="override-configuration"></a><span data-ttu-id="d75a4-313">Geçersiz kılma yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d75a4-313">Override configuration</span></span>

<span data-ttu-id="d75a4-314">Web konağını yapılandırmak için [yapılandırma](xref:fundamentals/configuration/index) kullanın.</span><span class="sxs-lookup"><span data-stu-id="d75a4-314">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="d75a4-315">Aşağıdaki örnekte, konak yapılandırması isteğe bağlı olarak bir *HostSettings. JSON* dosyasında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-315">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d75a4-316">*HostSettings. JSON* dosyasından yüklenen herhangi bir yapılandırma komut satırı bağımsız değişkenleri tarafından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-316">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d75a4-317">Oluşturulan yapılandırma (`config`), Konağı [Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)ile yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d75a4-317">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d75a4-318">`IWebHostBuilder` yapılandırması uygulamanın yapılandırmasına eklenir, ancak&mdash;`ConfigureAppConfiguration` `IWebHostBuilder` yapılandırmasını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="d75a4-318">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="d75a4-319">*HostSettings. JSON* config ile `UseUrls` tarafından belirtilen yapılandırmayı geçersiz kılma, komut satırı bağımsız değişkeni yapılandırma saniyesi:</span><span class="sxs-lookup"><span data-stu-id="d75a4-319">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d75a4-320">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="d75a4-320">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="d75a4-321">[Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) uzantı yöntemi şu anda `GetSection` tarafından döndürülen bir yapılandırma bölümünü ayrıştırma özelliğine sahip değil (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d75a4-322">`GetSection` yöntemi, yapılandırma anahtarlarını istenen bölüme süzer ancak bölüm adını anahtarlar üzerinde bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d75a4-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d75a4-323">`UseConfiguration` yöntemi, anahtarların `WebHostBuilder` anahtarlarla eşleşmesini bekler (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d75a4-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d75a4-324">Anahtarlarda bölüm adının varlığı, bölümün değerlerinin konak yapılandırmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="d75a4-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d75a4-325">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d75a4-326">Daha fazla bilgi ve geçici çözüm için bkz [. yapılandırma bölümünü WebHostBuilder 'A geçirme. UseConfiguration tam anahtarları kullanır](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d75a4-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="d75a4-327">`UseConfiguration`, anahtarları yalnızca belirtilen `IConfiguration` ana bilgisayar Oluşturucu yapılandırmasına kopyalar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-327">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d75a4-328">Bu nedenle, JSON, INI ve XML ayarları dosyaları için `reloadOnChange: true` ayarlamanın hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-328">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d75a4-329">Belirli bir URL 'de çalıştırılacak Konağı belirtmek için, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)yürütürken istenen değer bir komut isteminden geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d75a4-330">Komut satırı bağımsız değişkeni, *HostSettings. JSON* dosyasından `urls` değerini geçersiz kılar ve sunucu 8080 numaralı bağlantı noktasını dinler:</span><span class="sxs-lookup"><span data-stu-id="d75a4-330">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d75a4-331">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="d75a4-331">Manage the host</span></span>

<span data-ttu-id="d75a4-332">**Çalıştırma**</span><span class="sxs-lookup"><span data-stu-id="d75a4-332">**Run**</span></span>

<span data-ttu-id="d75a4-333">`Run` yöntemi, Web uygulamasını başlatır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="d75a4-333">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d75a4-334">**Start**</span><span class="sxs-lookup"><span data-stu-id="d75a4-334">**Start**</span></span>

<span data-ttu-id="d75a4-335">`Start` yöntemini çağırarak Konağı engellenmeyen bir şekilde çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-335">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d75a4-336">`Start` yöntemine bir URL listesi geçirilirse, belirtilen URL 'Leri dinler:</span><span class="sxs-lookup"><span data-stu-id="d75a4-336">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d75a4-337">Uygulama, statik bir kolaylık yöntemi kullanarak `CreateDefaultBuilder` önceden yapılandırılmış varsayılan değerlerini kullanarak yeni bir konak başlatabilir ve başlatabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-337">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d75a4-338">Bu yöntemler, konsol çıktısı olmadan sunucuyu başlatır ve [Waitforkapatmadan](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) bir kesme (CTRL-C/sigint veya sigterim) bekler:</span><span class="sxs-lookup"><span data-stu-id="d75a4-338">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d75a4-339">**Başlat (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-339">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d75a4-340">`RequestDelegate`ile başlayın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-340">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d75a4-341">"Merhaba Dünya!" yanıtını almak için `http://localhost:5000` tarayıcıda bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="d75a4-341">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d75a4-342">kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar blok `WaitForShutdown`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-342">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d75a4-343">Uygulama `Console.WriteLine` iletisini görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-343">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d75a4-344">**Başlangıç (dize URL 'si, RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-344">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d75a4-345">Bir URL ve `RequestDelegate`başlayın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-345">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d75a4-346">Uygulamanın `http://localhost:8080`yanıt vermesi dışında, **Başlangıç (RequestDelegate uygulaması)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-346">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d75a4-347">**Başlat (eylem\<ıroutebuilder > routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-347">**Start(Action\<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d75a4-348">Yönlendirme ara yazılımını kullanmak için bir `IRouteBuilder` örneği ([Microsoft. AspNetCore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-348">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d75a4-349">Aşağıdaki tarayıcı isteklerini örnekle birlikte kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-349">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d75a4-350">İstek</span><span class="sxs-lookup"><span data-stu-id="d75a4-350">Request</span></span>                                    | <span data-ttu-id="d75a4-351">Yanıt</span><span class="sxs-lookup"><span data-stu-id="d75a4-351">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d75a4-352">Merhaba, martın!</span><span class="sxs-lookup"><span data-stu-id="d75a4-352">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d75a4-353">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d75a4-353">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d75a4-354">"Ooops!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="d75a4-354">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d75a4-355">"Uh oh!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="d75a4-355">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d75a4-356">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d75a4-356">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d75a4-357">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="d75a4-357">Hello World!</span></span>                             |

<span data-ttu-id="d75a4-358">kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar blok `WaitForShutdown`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d75a4-359">Uygulama `Console.WriteLine` iletisini görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d75a4-360">**Başlangıç (dize URL 'si, eylem\<ıroutebuilder > routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-360">**Start(string url, Action\<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d75a4-361">Bir URL ve `IRouteBuilder`örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-361">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d75a4-362">Uygulamanın `http://localhost:8080`yanıt vermesi dışında, **Başlangıç (eylem\<ıroutebuilder > routebuilder)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-362">Produces the same result as **Start(Action\<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d75a4-363">**StartWith (Action\<IApplicationBuilder > App)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-363">**StartWith(Action\<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d75a4-364">`IApplicationBuilder`yapılandırmak için bir temsilci sağlayın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-364">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d75a4-365">"Merhaba Dünya!" yanıtını almak için `http://localhost:5000` tarayıcıda bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="d75a4-365">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d75a4-366">kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar blok `WaitForShutdown`.</span><span class="sxs-lookup"><span data-stu-id="d75a4-366">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d75a4-367">Uygulama `Console.WriteLine` iletisini görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="d75a4-367">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d75a4-368">**StartWith (dize URL 'si, Action\<IApplicationBuilder > App)**</span><span class="sxs-lookup"><span data-stu-id="d75a4-368">**StartWith(string url, Action\<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d75a4-369">`IApplicationBuilder`yapılandırmak için bir URL ve temsilci sağlayın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-369">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d75a4-370">Uygulamanın `http://localhost:8080`yanıt vermesi dışında, **StartWith ile aynı sonucu üretir (Action\<IApplicationBuilder > App)** .</span><span class="sxs-lookup"><span data-stu-id="d75a4-370">Produces the same result as **StartWith(Action\<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="iwebhostenvironment-interface"></a><span data-ttu-id="d75a4-371">Iwebhostenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="d75a4-371">IWebHostEnvironment interface</span></span>

<span data-ttu-id="d75a4-372">`IWebHostEnvironment` arabirimi, uygulamanın Web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-372">The `IWebHostEnvironment` interface provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d75a4-373">Özelliklerini ve uzantı yöntemlerini kullanmak için `IWebHostEnvironment` almak üzere [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-373">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IWebHostEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IWebHostEnvironment _env;

    public CustomFileReader(IWebHostEnvironment env)
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

<span data-ttu-id="d75a4-374">Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-374">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d75a4-375">Alternatif olarak, `ConfigureServices`kullanım için `Startup` oluşturucusuna `IWebHostEnvironment` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d75a4-375">Alternatively, inject the `IWebHostEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IWebHostEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IWebHostEnvironment HostingEnvironment { get; }

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
> <span data-ttu-id="d75a4-376">`IsDevelopment` uzantısı yöntemine ek olarak, `IWebHostEnvironment` `IsStaging`, `IsProduction`ve `IsEnvironment(string environmentName)` yöntemleri sunar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-376">In addition to the `IsDevelopment` extension method, `IWebHostEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d75a4-377">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-377">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d75a4-378">`IWebHostEnvironment` hizmeti ayrıca işlem ardışık düzenini ayarlamak için doğrudan `Configure` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-378">The `IWebHostEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
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

<span data-ttu-id="d75a4-379">`IWebHostEnvironment`, özel [Ara yazılım](xref:fundamentals/middleware/write)oluştururken `Invoke` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-379">`IWebHostEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

```csharp
public async Task Invoke(HttpContext context, IWebHostEnvironment env)
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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d75a4-380">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="d75a4-380">IHostingEnvironment interface</span></span>

<span data-ttu-id="d75a4-381">[Ihostingenvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) , uygulamanın Web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-381">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d75a4-382">Özelliklerini ve uzantı yöntemlerini kullanmak için `IHostingEnvironment` almak üzere [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-382">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d75a4-383">Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-383">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d75a4-384">Alternatif olarak, `ConfigureServices`kullanım için `Startup` oluşturucusuna `IHostingEnvironment` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d75a4-384">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d75a4-385">`IsDevelopment` uzantısı yöntemine ek olarak, `IHostingEnvironment` `IsStaging`, `IsProduction`ve `IsEnvironment(string environmentName)` yöntemleri sunar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-385">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d75a4-386">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d75a4-386">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d75a4-387">`IHostingEnvironment` hizmeti ayrıca işlem ardışık düzenini ayarlamak için doğrudan `Configure` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-387">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d75a4-388">`IHostingEnvironment`, özel [Ara yazılım](xref:fundamentals/middleware/write)oluştururken `Invoke` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-388">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="ihostapplicationlifetime-interface"></a><span data-ttu-id="d75a4-389">Ihostapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="d75a4-389">IHostApplicationLifetime interface</span></span>

<span data-ttu-id="d75a4-390">`IHostApplicationLifetime`, başlatma sonrası ve kapalı etkinlikler için izin verir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-390">`IHostApplicationLifetime` allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d75a4-391">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan `Action` yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-391">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d75a4-392">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="d75a4-392">Cancellation Token</span></span>    | <span data-ttu-id="d75a4-393">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="d75a4-393">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d75a4-394">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="d75a4-394">The host has fully started.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d75a4-395">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="d75a4-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d75a4-396">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-396">All requests should be processed.</span></span> <span data-ttu-id="d75a4-397">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-397">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d75a4-398">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="d75a4-398">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d75a4-399">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-399">Requests may still be processing.</span></span> <span data-ttu-id="d75a4-400">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-400">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IHostApplicationLifetime appLifetime)
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

<span data-ttu-id="d75a4-401">`StopApplication`, uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="d75a4-401">`StopApplication` requests termination of the app.</span></span> <span data-ttu-id="d75a4-402">Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için `StopApplication` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d75a4-402">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IHostApplicationLifetime _appLifetime;

    public MyClass(IHostApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d75a4-403">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="d75a4-403">IApplicationLifetime interface</span></span>

<span data-ttu-id="d75a4-404">[Iapplicationlifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) , başlatma sonrası ve kapalı etkinlikler için izin verir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-404">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d75a4-405">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan `Action` yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-405">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d75a4-406">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="d75a4-406">Cancellation Token</span></span>    | <span data-ttu-id="d75a4-407">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="d75a4-407">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d75a4-408">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d75a4-408">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d75a4-409">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="d75a4-409">The host has fully started.</span></span> |
| [<span data-ttu-id="d75a4-410">Applicationdurdurulan</span><span class="sxs-lookup"><span data-stu-id="d75a4-410">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d75a4-411">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="d75a4-411">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d75a4-412">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-412">All requests should be processed.</span></span> <span data-ttu-id="d75a4-413">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-413">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d75a4-414">Applicationdurduruluyor</span><span class="sxs-lookup"><span data-stu-id="d75a4-414">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d75a4-415">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="d75a4-415">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d75a4-416">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-416">Requests may still be processing.</span></span> <span data-ttu-id="d75a4-417">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-417">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d75a4-418">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) , uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="d75a4-418">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d75a4-419">Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için `StopApplication` kullanır:</span><span class="sxs-lookup"><span data-stu-id="d75a4-419">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

::: moniker-end

## <a name="scope-validation"></a><span data-ttu-id="d75a4-420">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="d75a4-420">Scope validation</span></span>

<span data-ttu-id="d75a4-421">[Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) , uygulamanın ortamı geliştirmede `true` Için [serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-421">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="d75a4-422">`ValidateScopes` `true`olarak ayarlandığında, varsayılan hizmet sağlayıcı aşağıdakileri doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="d75a4-422">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d75a4-423">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="d75a4-423">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d75a4-424">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-424">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d75a4-425">[Buildserviceprovider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-425">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d75a4-426">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="d75a4-426">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d75a4-427">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="d75a4-427">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d75a4-428">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="d75a4-428">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d75a4-429">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="d75a4-429">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d75a4-430">Üretim ortamında da dahil olmak üzere kapsamları her zaman doğrulamak için, ana bilgisayar Oluşturucu 'da [Serviceprovideroptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) 'ı [usedefaultserviceprovider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) ile yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="d75a4-430">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="d75a4-431">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d75a4-431">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
