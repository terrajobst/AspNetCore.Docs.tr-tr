---
title: ASP.NET Core Web ana bilgisayarı
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu olan ASP.NET Core Web ana bilgisayarı hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 977c1df67c2775870d630f3a1085d5e19cef58f5
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981906"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="c994b-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="c994b-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="c994b-104">ASP.NET Core uygulamalar bir *Konağı*yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="c994b-104">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="c994b-105">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="c994b-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c994b-106">Ana bilgisayar, en azından bir sunucu ve bir istek işleme işlem hattı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c994b-106">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="c994b-107">Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-107">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c994b-108">Bu makalede, yalnızca geriye dönük uyumluluk için kullanılabilen Web ana bilgisayarı ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c994b-108">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="c994b-109">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-109">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="c994b-110">Bu makale, Web uygulamalarını barındırmak için olan Web konağını ele alır.</span><span class="sxs-lookup"><span data-stu-id="c994b-110">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="c994b-111">Diğer uygulama türleri için [genel Konağı](xref:fundamentals/host/generic-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="c994b-111">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="c994b-112">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="c994b-112">Set up a host</span></span>

<span data-ttu-id="c994b-113">[Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın bir örneğini kullanarak bir konak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c994b-113">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="c994b-114">Bu, genellikle uygulamanın giriş noktasında, `Main` yönteminde gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-114">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="c994b-115">Proje şablonlarında, `Main` *program.cs*içinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="c994b-115">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="c994b-116">Tipik bir uygulama, bir konak ayarlamaya başlamak için [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağırır:</span><span class="sxs-lookup"><span data-stu-id="c994b-116">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="c994b-117">@No__t-0 ' ı çağıran kod, Oluşturucu nesnesinde `Run` ' ü çağıran `Main` ' deki koddan ayıran `CreateWebHostBuilder` adlı bir yöntemde bulunur.</span><span class="sxs-lookup"><span data-stu-id="c994b-117">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="c994b-118">[Entity Framework Core araçlarını](/ef/core/miscellaneous/cli/)kullanıyorsanız bu ayrım gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c994b-118">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="c994b-119">Araçlar, uygulamayı çalıştırmadan ana bilgisayarı yapılandırmak için tasarım zamanında çağrlayabilecekleri bir `CreateWebHostBuilder` yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-119">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="c994b-120">Diğer bir seçenek de `IDesignTimeDbContextFactory` ' a uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c994b-120">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="c994b-121">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="c994b-121">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="c994b-122">`CreateDefaultBuilder` aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="c994b-122">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="c994b-123">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu, uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak Web sunucusu olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c994b-123">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="c994b-124">Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="c994b-124">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="c994b-125">İçerik kökünü [Directory. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-125">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="c994b-126">[Ana bilgisayar yapılandırmasını](#host-configuration-values) şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="c994b-126">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="c994b-127">@No__t-0 (örneğin, `ASPNETCORE_ENVIRONMENT`) önekli ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c994b-127">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="c994b-128">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c994b-128">Command-line arguments.</span></span>
* <span data-ttu-id="c994b-129">Aşağıdaki sırayla uygulama yapılandırmasını yükler:</span><span class="sxs-lookup"><span data-stu-id="c994b-129">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="c994b-130">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="c994b-130">*appsettings.json*.</span></span>
  * <span data-ttu-id="c994b-131">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="c994b-131">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="c994b-132">Uygulama, giriş derlemesini kullanarak `Development` ortamında çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="c994b-132">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="c994b-133">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c994b-133">Environment variables.</span></span>
  * <span data-ttu-id="c994b-134">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="c994b-134">Command-line arguments.</span></span>
* <span data-ttu-id="c994b-135">Konsol ve hata ayıklama çıkışı için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c994b-135">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="c994b-136">Günlüğe kaydetme, bir *appSettings. JSON* veya appSettings 'in günlük yapılandırma bölümünde belirtilen [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) kurallarını içerir *. { Environment}. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="c994b-136">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="c994b-137">[ASP.NET Core modülle](xref:host-and-deploy/aspnet-core-module)IIS 'nin arkasında çalışırken, `CreateDefaultBuilder`, uygulamanın temel adresini ve bağlantı noktasını yapılandıran [IIS tümleştirmesine](xref:host-and-deploy/iis/index)izin vermez.</span><span class="sxs-lookup"><span data-stu-id="c994b-137">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="c994b-138">IIS tümleştirmesi, uygulamayı [başlatma hatalarını yakalamaya](#capture-startup-errors)de yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c994b-138">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="c994b-139">IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="c994b-139">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="c994b-140">Uygulamanın ortamı geliştirmelerde, [Serviceprovideroptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) `true` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-140">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="c994b-141">Daha fazla bilgi için bkz. [kapsam doğrulaması](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="c994b-141">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="c994b-142">@No__t-0 tarafından tanımlanan yapılandırma, [Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [configurelogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın diğer yöntemleri ve genişletme yöntemleri tarafından geçersiz kılınabilir ve genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-142">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="c994b-143">Birkaç örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c994b-143">A few examples follow:</span></span>

* <span data-ttu-id="c994b-144">[Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) , uygulama için ek `IConfiguration` belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="c994b-145">Aşağıdaki `ConfigureAppConfiguration` çağrısı, *appSettings. xml* dosyasına uygulama yapılandırmasını dahil etmek için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-145">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="c994b-146">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-146">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="c994b-147">Bu yapılandırmanın ana bilgisayar için (örneğin, sunucu URL 'Leri veya ortam) uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c994b-147">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="c994b-148">[Konak yapılandırma değerleri](#host-configuration-values) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c994b-148">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="c994b-149">Aşağıdaki `ConfigureLogging` çağrısı, minimum günlük düzeyi ([Setminimumlevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) değerini [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel)olarak yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-149">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="c994b-150">Bu ayar appSettings 'teki ayarları geçersiz kılar *. Development. JSON* (`LogLevel.Debug`) ve *appSettings. Production. JSON* (`LogLevel.Error`) `CreateDefaultBuilder` tarafından yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="c994b-150">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="c994b-151">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-151">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="c994b-152">@No__t-0 ' a yönelik aşağıdaki çağrı varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30.000.000 Byte, Kestrel `CreateDefaultBuilder` tarafından yapılandırıldığında belirlenir:</span><span class="sxs-lookup"><span data-stu-id="c994b-152">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="c994b-153">Aşağıdaki [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) çağrısı, varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel `CreateDefaultBuilder` tarafından yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:</span><span class="sxs-lookup"><span data-stu-id="c994b-153">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="c994b-154">*İçerik kökü* , konağın MVC görünüm dosyaları gibi içerik dosyalarını arayacağı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="c994b-154">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="c994b-155">Uygulama, projenin kök klasöründen başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-155">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="c994b-156">Bu, [Visual Studio](https://visualstudio.microsoft.com) 'da ve [DotNet yeni şablonlarda](/dotnet/core/tools/dotnet-new)kullanılan varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="c994b-156">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="c994b-157">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c994b-157">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="c994b-158">Statik `CreateDefaultBuilder` yönteminin kullanılmasına alternatif olarak, [Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 'dan bir konak oluşturmak, ASP.NET Core 2. x ile desteklenen bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="c994b-158">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="c994b-159">Bir ana bilgisayar ayarlanırken, [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="c994b-160">@No__t-0 sınıfı belirtilmişse, bir `Configure` yöntemi tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="c994b-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="c994b-161">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c994b-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="c994b-162">@No__t-0 ' a birden çok çağrı bir diğerine eklenir.</span><span class="sxs-lookup"><span data-stu-id="c994b-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="c994b-163">@No__t-2 ' de `Configure` veya `UseStartup` ' e birden çok çağrı önceki ayarları değiştirir.</span><span class="sxs-lookup"><span data-stu-id="c994b-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c994b-164">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="c994b-164">Host configuration values</span></span>

<span data-ttu-id="c994b-165">[Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) , ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımları kullanır:</span><span class="sxs-lookup"><span data-stu-id="c994b-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="c994b-166">@No__t-0 biçimindeki ortam değişkenlerini içeren konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="c994b-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="c994b-167">Örneğin, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="c994b-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="c994b-168">[Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) gibi uzantılar ( [geçersiz kılma yapılandırması](#override-configuration) bölümüne bakın).</span><span class="sxs-lookup"><span data-stu-id="c994b-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="c994b-169">[Usesetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="c994b-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="c994b-170">@No__t-0 ile bir değer ayarlarken, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c994b-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="c994b-171">Konak, bir değeri en son ayarlayan seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="c994b-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="c994b-172">Daha fazla bilgi için, sonraki bölümde [yapılandırmayı geçersiz kılma](#override-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="c994b-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="c994b-173">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="c994b-173">Application Key (Name)</span></span>

<span data-ttu-id="c994b-174">Konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında, [ıhostingenvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c994b-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="c994b-175">Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c994b-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="c994b-176">Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="c994b-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="c994b-177">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="c994b-177">**Key**: applicationName</span></span>  
<span data-ttu-id="c994b-178">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-178">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-179">**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="c994b-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="c994b-180">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c994b-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c994b-181">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="c994b-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="c994b-182">Yakalama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="c994b-182">Capture Startup Errors</span></span>

<span data-ttu-id="c994b-183">Bu ayar, başlatma hatalarının yakalanmasını denetler.</span><span class="sxs-lookup"><span data-stu-id="c994b-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="c994b-184">**Anahtar**: capturestartuperrors</span><span class="sxs-lookup"><span data-stu-id="c994b-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="c994b-185">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c994b-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c994b-186">**Varsayılan**: Uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` ' dır ve varsayılan olarak `true` ' dir.</span><span class="sxs-lookup"><span data-stu-id="c994b-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="c994b-187">Şunu **kullanarak ayarla**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="c994b-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="c994b-188">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="c994b-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="c994b-189">@No__t-0 olduğunda, başlangıçtaki ana bilgisayardaki hatalar çıkıyor.</span><span class="sxs-lookup"><span data-stu-id="c994b-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="c994b-190">@No__t-0 olduğunda, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="c994b-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="c994b-191">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="c994b-191">Content Root</span></span>

<span data-ttu-id="c994b-192">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="c994b-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="c994b-193">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="c994b-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="c994b-194">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-194">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-195">**Varsayılan**: Uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="c994b-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="c994b-196">Şunu **kullanarak ayarla**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="c994b-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="c994b-197">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="c994b-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="c994b-198">İçerik kökü, [Web kök ayarı](#web-root)için temel yol olarak da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="c994b-199">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="c994b-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="c994b-200">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="c994b-200">Detailed Errors</span></span>

<span data-ttu-id="c994b-201">Ayrıntılı hataların yakalanıp yakalanmayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="c994b-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="c994b-202">**Anahtar**: detailederrors</span><span class="sxs-lookup"><span data-stu-id="c994b-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="c994b-203">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c994b-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c994b-204">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="c994b-204">**Default**: false</span></span>  
<span data-ttu-id="c994b-205">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c994b-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c994b-206">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="c994b-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="c994b-207">Etkinleştirildiğinde (veya <a href="#environment">ortam</a> `Development` olarak ayarlandığında), uygulama ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="c994b-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="c994b-208">Ortam</span><span class="sxs-lookup"><span data-stu-id="c994b-208">Environment</span></span>

<span data-ttu-id="c994b-209">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-209">Sets the app's environment.</span></span>

<span data-ttu-id="c994b-210">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="c994b-210">**Key**: environment</span></span>  
<span data-ttu-id="c994b-211">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-211">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-212">**Varsayılan**: Üretiminden</span><span class="sxs-lookup"><span data-stu-id="c994b-212">**Default**: Production</span></span>  
<span data-ttu-id="c994b-213">Şunu **kullanarak ayarla**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="c994b-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="c994b-214">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="c994b-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="c994b-215">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-215">The environment can be set to any value.</span></span> <span data-ttu-id="c994b-216">Çerçeve tanımlı değerler `Development`, `Staging` ve `Production` içerir.</span><span class="sxs-lookup"><span data-stu-id="c994b-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="c994b-217">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c994b-217">Values aren't case sensitive.</span></span> <span data-ttu-id="c994b-218">Varsayılan olarak, *ortam* `ASPNETCORE_ENVIRONMENT` ortam değişkeninden okunurdur.</span><span class="sxs-lookup"><span data-stu-id="c994b-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="c994b-219">[Visual Studio](https://visualstudio.microsoft.com)kullanılırken, *launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="c994b-220">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c994b-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="c994b-221">Başlangıç derlemelerini barındırma</span><span class="sxs-lookup"><span data-stu-id="c994b-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="c994b-222">Uygulamanın barındırma başlangıç derlemelerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="c994b-223">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="c994b-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="c994b-224">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-224">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-225">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="c994b-225">**Default**: Empty string</span></span>  
<span data-ttu-id="c994b-226">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c994b-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c994b-227">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c994b-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="c994b-228">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="c994b-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="c994b-229">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="c994b-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="c994b-230">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="c994b-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="c994b-231">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="c994b-231">HTTPS Port</span></span>

<span data-ttu-id="c994b-232">HTTPS yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c994b-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="c994b-233">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="c994b-234">**Anahtar**: https_port **Type**: *dize*
**varsayılan**: Varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="c994b-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="c994b-235">Şunu **kullanarak ayarla**: `UseSetting` @ no__t-1**ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="c994b-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="c994b-236">Barındırma başlatma derlemeleri dışlama</span><span class="sxs-lookup"><span data-stu-id="c994b-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="c994b-237">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="c994b-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="c994b-238">**Anahtar**: hostingstartupexcludeassemblies</span><span class="sxs-lookup"><span data-stu-id="c994b-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="c994b-239">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-239">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-240">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="c994b-240">**Default**: Empty string</span></span>  
<span data-ttu-id="c994b-241">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c994b-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c994b-242">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="c994b-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="c994b-243">Barındırma URL 'Lerini tercih et</span><span class="sxs-lookup"><span data-stu-id="c994b-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="c994b-244">Konağın `IServer` uygulamasıyla yapılandırılanlar yerine `WebHostBuilder` ile yapılandırılan URL 'Leri dinlemesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c994b-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="c994b-245">**Anahtar**: preferhostingurl 'leri</span><span class="sxs-lookup"><span data-stu-id="c994b-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="c994b-246">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c994b-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c994b-247">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="c994b-247">**Default**: true</span></span>  
<span data-ttu-id="c994b-248">Şunu **kullanarak ayarla**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="c994b-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="c994b-249">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="c994b-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="c994b-250">Barındırma başlangıcını engelle</span><span class="sxs-lookup"><span data-stu-id="c994b-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="c994b-251">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="c994b-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="c994b-252">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c994b-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="c994b-253">**Anahtar**: koruyucu thostingstartup</span><span class="sxs-lookup"><span data-stu-id="c994b-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="c994b-254">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="c994b-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="c994b-255">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="c994b-255">**Default**: false</span></span>  
<span data-ttu-id="c994b-256">Şunu **kullanarak ayarla**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="c994b-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="c994b-257">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="c994b-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="c994b-258">Sunucu URL 'Leri</span><span class="sxs-lookup"><span data-stu-id="c994b-258">Server URLs</span></span>

<span data-ttu-id="c994b-259">Sunucunun istekler için dinlemesi gereken bağlantı noktaları ve protokoller içeren IP adreslerini veya ana bilgisayar adreslerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c994b-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="c994b-260">**Anahtar**: URL 'ler</span><span class="sxs-lookup"><span data-stu-id="c994b-260">**Key**: urls</span></span>  
<span data-ttu-id="c994b-261">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-261">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-262">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="c994b-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="c994b-263">Şunu **kullanarak ayarla**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="c994b-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="c994b-264">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="c994b-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="c994b-265">Noktalı virgülle ayrılmış olarak ayarlayın (;) sunucunun yanıtlaması gereken URL ön eklerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="c994b-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="c994b-266">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="c994b-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="c994b-267">Sunucunun belirtilen bağlantı noktasını ve protokolü kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="c994b-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="c994b-268">Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c994b-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="c994b-269">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="c994b-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="c994b-270">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c994b-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="c994b-271">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="c994b-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="c994b-272">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="c994b-272">Shutdown Timeout</span></span>

<span data-ttu-id="c994b-273">Web konağının kapanması için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="c994b-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="c994b-274">**Anahtar**: shutdowntimeoutseconds</span><span class="sxs-lookup"><span data-stu-id="c994b-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="c994b-275">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="c994b-275">**Type**: *int*</span></span>  
<span data-ttu-id="c994b-276">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="c994b-276">**Default**: 5</span></span>  
<span data-ttu-id="c994b-277">Şunu **kullanarak ayarla**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="c994b-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="c994b-278">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="c994b-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="c994b-279">Anahtar, `UseSetting` ile bir *int* kabul etse de (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [useshutdowntimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi bir [TimeSpan](/dotnet/api/system.timespan)alır.</span><span class="sxs-lookup"><span data-stu-id="c994b-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="c994b-280">Zaman aşımı süresi boyunca barındırma:</span><span class="sxs-lookup"><span data-stu-id="c994b-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="c994b-281">[Iapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)öğesini tetikler.</span><span class="sxs-lookup"><span data-stu-id="c994b-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="c994b-282">Üzerinde durmayacak hizmetlerin hatalarını günlüğe kaydetmek için barındırılan hizmetleri durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="c994b-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="c994b-283">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="c994b-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="c994b-284">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="c994b-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="c994b-285">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="c994b-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="c994b-286">Başlangıç derlemesi</span><span class="sxs-lookup"><span data-stu-id="c994b-286">Startup Assembly</span></span>

<span data-ttu-id="c994b-287">@No__t-0 sınıfı için arama yapılacak derlemeyi belirler.</span><span class="sxs-lookup"><span data-stu-id="c994b-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="c994b-288">**Anahtar**: startupassembly</span><span class="sxs-lookup"><span data-stu-id="c994b-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="c994b-289">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-289">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-290">**Varsayılan**: Uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="c994b-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="c994b-291">Şunu **kullanarak ayarla**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="c994b-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="c994b-292">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="c994b-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="c994b-293">Ada (`string`) veya türe (`TStartup`) göre derlemeye başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="c994b-294">Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="c994b-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="c994b-295">Web kökü</span><span class="sxs-lookup"><span data-stu-id="c994b-295">Web Root</span></span>

<span data-ttu-id="c994b-296">Uygulamanın statik varlıklarının göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="c994b-297">**Anahtar**: Webroot</span><span class="sxs-lookup"><span data-stu-id="c994b-297">**Key**: webroot</span></span>  
<span data-ttu-id="c994b-298">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="c994b-298">**Type**: *string*</span></span>  
<span data-ttu-id="c994b-299">**Varsayılan**: Belirtilmemişse, varsayılan olarak "(Içerik kökü)/Wwwroot" olur.</span><span class="sxs-lookup"><span data-stu-id="c994b-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="c994b-300">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="c994b-301">Şunu **kullanarak ayarla**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="c994b-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="c994b-302">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="c994b-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="c994b-303">Geçersiz kılma yapılandırması</span><span class="sxs-lookup"><span data-stu-id="c994b-303">Override configuration</span></span>

<span data-ttu-id="c994b-304">Web konağını yapılandırmak için [yapılandırma](xref:fundamentals/configuration/index) kullanın.</span><span class="sxs-lookup"><span data-stu-id="c994b-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="c994b-305">Aşağıdaki örnekte, konak yapılandırması isteğe bağlı olarak bir *HostSettings. JSON* dosyasında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="c994b-306">*HostSettings. JSON* dosyasından yüklenen herhangi bir yapılandırma komut satırı bağımsız değişkenleri tarafından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="c994b-307">Oluşturulan yapılandırma (`config`), Konağı [Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)ile yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c994b-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="c994b-308">`IWebHostBuilder` yapılandırması uygulamanın yapılandırmasına eklenir, ancak bu değer doğru değil @ no__t-1 @ no__t-2 `IWebHostBuilder` yapılandırmasını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="c994b-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="c994b-309">@No__t-0 tarafından belirtilen yapılandırmayı *HostSettings* ile geçersiz kılma. önce JSON config, komut satırı bağımsız değişkeni yapılandırma saniyesi:</span><span class="sxs-lookup"><span data-stu-id="c994b-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="c994b-310">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="c994b-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="c994b-311">[Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) uzantı yöntemi şu anda `GetSection` tarafından döndürülen bir yapılandırma bölümünü ayrıştırma özelliğine sahip değil (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="c994b-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="c994b-312">@No__t-0 yöntemi, yapılandırma anahtarlarını istenen bölüme süzer ancak bölüm adını anahtarlar üzerinde bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="c994b-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="c994b-313">@No__t-0 yöntemi, anahtarların `WebHostBuilder` anahtarlarıyla eşleşmesini bekler (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="c994b-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="c994b-314">Anahtarlarda bölüm adının varlığı, bölümün değerlerinin konak yapılandırmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="c994b-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="c994b-315">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="c994b-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="c994b-316">Daha fazla bilgi ve geçici çözüm için bkz [. yapılandırma bölümünü WebHostBuilder 'A geçirme. UseConfiguration tam anahtarları kullanır](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="c994b-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="c994b-317">`UseConfiguration` yalnızca belirtilen `IConfiguration` ' den konak Oluşturucu yapılandırmasına anahtar kopyalar.</span><span class="sxs-lookup"><span data-stu-id="c994b-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="c994b-318">Bu nedenle, JSON, INI ve XML ayarları dosyaları için `reloadOnChange: true` ayarlarının hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="c994b-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="c994b-319">Belirli bir URL 'de çalıştırılacak Konağı belirtmek için, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)yürütürken istenen değer bir komut isteminden geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="c994b-320">Komut satırı bağımsız değişkeni *HostSettings. JSON* dosyasından `urls` değerini geçersiz kılar ve sunucu 8080 numaralı bağlantı noktasını dinler:</span><span class="sxs-lookup"><span data-stu-id="c994b-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="c994b-321">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="c994b-321">Manage the host</span></span>

<span data-ttu-id="c994b-322">**Çalıştırma**</span><span class="sxs-lookup"><span data-stu-id="c994b-322">**Run**</span></span>

<span data-ttu-id="c994b-323">@No__t-0 yöntemi, Web uygulamasını başlatır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="c994b-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="c994b-324">**Start**</span><span class="sxs-lookup"><span data-stu-id="c994b-324">**Start**</span></span>

<span data-ttu-id="c994b-325">@No__t-0 yöntemini çağırarak Konağı engellenmeyen bir şekilde çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c994b-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="c994b-326">@No__t-0 yöntemine bir URL listesi geçirilirse, belirtilen URL 'lerde dinler:</span><span class="sxs-lookup"><span data-stu-id="c994b-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="c994b-327">Uygulama, statik bir kolaylık yöntemi kullanarak `CreateDefaultBuilder` ' ın önceden yapılandırılmış varsayılanlarını kullanarak yeni bir ana bilgisayar başlatabilir ve başlatabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="c994b-328">Bu yöntemler, konsol çıktısı olmadan sunucuyu başlatır ve [Waitforkapatmadan](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) bir kesme (CTRL-C/sigint veya sigterim) bekler:</span><span class="sxs-lookup"><span data-stu-id="c994b-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="c994b-329">**Başlat (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="c994b-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="c994b-330">@No__t ile başlayın-0:</span><span class="sxs-lookup"><span data-stu-id="c994b-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c994b-331">"Merhaba Dünya!" yanıtını almak için tarayıcıda `http://localhost:5000` ' a bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="c994b-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c994b-332">bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok.</span><span class="sxs-lookup"><span data-stu-id="c994b-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c994b-333">Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c994b-334">**Başlangıç (dize URL 'si, RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="c994b-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="c994b-335">URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="c994b-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="c994b-336">Uygulamanın `http://localhost:8080` ' de yanıt vermesi dışında, **Başlangıç (RequestDelegate uygulaması)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="c994b-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="c994b-337">**Başlat (eylem @ no__t-1IRouteBuilder @ no__t-2 routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c994b-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c994b-338">Yönlendirme ara yazılımını kullanmak için `IRouteBuilder` ([Microsoft. AspNetCore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) örneğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="c994b-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="c994b-339">Aşağıdaki tarayıcı isteklerini örnekle birlikte kullanın:</span><span class="sxs-lookup"><span data-stu-id="c994b-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="c994b-340">İstek</span><span class="sxs-lookup"><span data-stu-id="c994b-340">Request</span></span>                                    | <span data-ttu-id="c994b-341">Yanıt</span><span class="sxs-lookup"><span data-stu-id="c994b-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="c994b-342">Merhaba, martın!</span><span class="sxs-lookup"><span data-stu-id="c994b-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="c994b-343">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="c994b-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="c994b-344">"Ooops!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="c994b-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="c994b-345">"Uh oh!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="c994b-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="c994b-346">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="c994b-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="c994b-347">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="c994b-347">Hello World!</span></span>                             |

<span data-ttu-id="c994b-348">bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok.</span><span class="sxs-lookup"><span data-stu-id="c994b-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c994b-349">Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c994b-350">**Başlat (dize URL 'si, eylem @ no__t-1ıroutebuilder @ no__t-2 routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="c994b-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="c994b-351">Bir URL ve @no__t örneği kullanın-0:</span><span class="sxs-lookup"><span data-stu-id="c994b-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="c994b-352">Uygulamanın `http://localhost:8080` ' te yanıt vermesi dışında, **Başlangıç (Action @ no__t-1IRouteBuilder @ no__t-2 routebuilder)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="c994b-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="c994b-353">**StartWith (Action @ no__t-1IApplicationBuilder @ no__t-2 App)**</span><span class="sxs-lookup"><span data-stu-id="c994b-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c994b-354">@No__t yapılandırmak için bir temsilci sağlayın-0:</span><span class="sxs-lookup"><span data-stu-id="c994b-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c994b-355">"Merhaba Dünya!" yanıtını almak için tarayıcıda `http://localhost:5000` ' a bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="c994b-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="c994b-356">bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok.</span><span class="sxs-lookup"><span data-stu-id="c994b-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="c994b-357">Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.</span><span class="sxs-lookup"><span data-stu-id="c994b-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="c994b-358">**StartWith (dize URL 'si, Action @ no__t-1IApplicationBuilder @ no__t-2 App)**</span><span class="sxs-lookup"><span data-stu-id="c994b-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="c994b-359">@No__t yapılandırmak için bir URL ve temsilci sağlayın-0:</span><span class="sxs-lookup"><span data-stu-id="c994b-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="c994b-360">Uygulamanın `http://localhost:8080` ' te yanıt vermesi dışında, **StartWith ile aynı sonucu üretir (Action @ no__t-1IApplicationBuilder @ no__t-2 App)** .</span><span class="sxs-lookup"><span data-stu-id="c994b-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="c994b-361">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="c994b-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="c994b-362">[Ihostingenvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) , uygulamanın Web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="c994b-363">Özelliklerini ve uzantı yöntemlerini kullanmak için `IHostingEnvironment` ' i almak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:</span><span class="sxs-lookup"><span data-stu-id="c994b-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="c994b-364">Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="c994b-365">Alternatif olarak, `IHostingEnvironment` ' ı `ConfigureServices` ' de kullanılmak üzere `Startup` oluşturucusuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c994b-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="c994b-366">@No__t-0 uzantısı yöntemine ek olarak, `IHostingEnvironment` `IsStaging`, `IsProduction` ve `IsEnvironment(string environmentName)` yöntemleri sunar.</span><span class="sxs-lookup"><span data-stu-id="c994b-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="c994b-367">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c994b-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="c994b-368">@No__t-0 hizmeti ayrıca işlem ardışık düzenini ayarlamak için doğrudan `Configure` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="c994b-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="c994b-369">`IHostingEnvironment`, özel [Ara yazılım](xref:fundamentals/middleware/write)oluştururken `Invoke` yöntemine eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="c994b-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="c994b-370">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="c994b-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="c994b-371">[Iapplicationlifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) , başlatma sonrası ve kapalı etkinlikler için izin verir.</span><span class="sxs-lookup"><span data-stu-id="c994b-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="c994b-372">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan `Action` yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="c994b-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="c994b-373">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="c994b-373">Cancellation Token</span></span>    | <span data-ttu-id="c994b-374">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="c994b-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="c994b-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="c994b-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="c994b-376">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="c994b-376">The host has fully started.</span></span> |
| [<span data-ttu-id="c994b-377">Applicationdurdurulan</span><span class="sxs-lookup"><span data-stu-id="c994b-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="c994b-378">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="c994b-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="c994b-379">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c994b-379">All requests should be processed.</span></span> <span data-ttu-id="c994b-380">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="c994b-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="c994b-381">Applicationdurduruluyor</span><span class="sxs-lookup"><span data-stu-id="c994b-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="c994b-382">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="c994b-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="c994b-383">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="c994b-383">Requests may still be processing.</span></span> <span data-ttu-id="c994b-384">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="c994b-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="c994b-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) , uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="c994b-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="c994b-386">Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için `StopApplication` kullanır:</span><span class="sxs-lookup"><span data-stu-id="c994b-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="c994b-387">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="c994b-387">Scope validation</span></span>

<span data-ttu-id="c994b-388">Uygulamanın ortamı geliştirmede [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) , [Serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini `true` olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c994b-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="c994b-389">@No__t-0 `true` olarak ayarlandığında, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="c994b-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c994b-390">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="c994b-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c994b-391">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="c994b-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c994b-392">[Buildserviceprovider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c994b-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="c994b-393">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c994b-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c994b-394">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="c994b-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c994b-395">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="c994b-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c994b-396">@No__t-0 çağrıldığında, hizmet kapsamlarını doğrulamak bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="c994b-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c994b-397">Üretim ortamında da dahil olmak üzere kapsamları her zaman doğrulamak için, ana bilgisayar Oluşturucu 'da [Serviceprovideroptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) 'ı [usedefaultserviceprovider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) ile yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="c994b-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="c994b-398">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c994b-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
