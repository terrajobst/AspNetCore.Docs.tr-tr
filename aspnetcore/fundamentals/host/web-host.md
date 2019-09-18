---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetiminden sorumlu olan ASP.NET Core Web ana bilgisayarı hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: d387098662cc832cc0e49b6a1636f0ebcc7308de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081685"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="b83fc-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="b83fc-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="b83fc-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b83fc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b83fc-105">ASP.NET Core uygulamalar bir *Konağı*yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="b83fc-106">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b83fc-107">Ana bilgisayar, en azından bir sunucu ve bir istek işleme işlem hattı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="b83fc-108">Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b83fc-109">Bu makalede, yalnızca geriye dönük uyumluluk için kullanılabilen Web ana bilgisayarı ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-109">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="b83fc-110">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-110">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="b83fc-111">Bu makale, Web uygulamalarını barındırmak için olan Web konağını ele alır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-111">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="b83fc-112">Diğer uygulama türleri için [genel Konağı](xref:fundamentals/host/generic-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-112">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="b83fc-113">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="b83fc-113">Set up a host</span></span>

<span data-ttu-id="b83fc-114">[Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın bir örneğini kullanarak bir konak oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b83fc-114">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="b83fc-115">Bu, genellikle uygulamanın giriş noktasında ve `Main` yöntemi olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-115">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="b83fc-116">Proje şablonlarında, `Main` *program.cs*konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-116">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="b83fc-117">Tipik bir uygulama, bir konak ayarlamaya başlamak için [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağırır:</span><span class="sxs-lookup"><span data-stu-id="b83fc-117">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="b83fc-118">Çağıran `CreateDefaultBuilder` kod, Oluşturucu nesnesindeki çağrılarındaki `Main` `Run` koddan ayıran `CreateWebHostBuilder`adlı bir yöntemde bulunur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-118">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="b83fc-119">[Entity Framework Core araçlarını](/ef/core/miscellaneous/cli/)kullanıyorsanız bu ayrım gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-119">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="b83fc-120">Araçlar, uygulamayı çalıştırmadan Konağı yapılandırmak `CreateWebHostBuilder` için tasarım zamanında çağırabilecekleri bir yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-120">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="b83fc-121">Diğer bir seçenek de uygulanır `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="b83fc-121">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="b83fc-122">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="b83fc-122">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="b83fc-123">`CreateDefaultBuilder`Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="b83fc-123">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="b83fc-124">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu, uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak Web sunucusu olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-124">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="b83fc-125">Kestrel sunucusunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-125">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="b83fc-126">İçerik kökünü [Directory. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-126">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="b83fc-127">[Ana bilgisayar yapılandırmasını](#host-configuration-values) şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="b83fc-127">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="b83fc-128">Ön eki olan `ASPNETCORE_` ortam değişkenleri (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="b83fc-128">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="b83fc-129">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b83fc-129">Command-line arguments.</span></span>
* <span data-ttu-id="b83fc-130">Aşağıdaki sırayla uygulama yapılandırmasını yükler:</span><span class="sxs-lookup"><span data-stu-id="b83fc-130">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="b83fc-131">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b83fc-131">*appsettings.json*.</span></span>
  * <span data-ttu-id="b83fc-132">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b83fc-132">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="b83fc-133">Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="b83fc-133">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b83fc-134">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b83fc-134">Environment variables.</span></span>
  * <span data-ttu-id="b83fc-135">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b83fc-135">Command-line arguments.</span></span>
* <span data-ttu-id="b83fc-136">Konsol ve hata ayıklama çıkışı için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-136">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="b83fc-137">Günlüğe kaydetme, bir *appSettings. JSON* veya appSettings 'in günlük yapılandırma bölümünde belirtilen [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) kurallarını içerir *. { Environment}. JSON* dosyası.</span><span class="sxs-lookup"><span data-stu-id="b83fc-137">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="b83fc-138">[ASP.NET Core modülle](xref:host-and-deploy/aspnet-core-module)IIS 'nin arkasında çalışırken, `CreateDefaultBuilder` uygulamanın temel adresini ve bağlantı noktasını yapılandıran [IIS tümleştirmesini](xref:host-and-deploy/iis/index)da sunar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-138">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="b83fc-139">IIS tümleştirmesi, uygulamayı [başlatma hatalarını yakalamaya](#capture-startup-errors)de yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-139">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="b83fc-140">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-140">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="b83fc-141">Uygulamanın ortamı geliştirme ortamındaysanız, [serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini olarak `true` ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-141">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="b83fc-142">Daha fazla bilgi için bkz. [kapsam doğrulaması](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="b83fc-142">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="b83fc-143">Tarafından `CreateDefaultBuilder` tanımlanan yapılandırma, [configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [configurelogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın diğer yöntemleri ve genişletme yöntemleri tarafından geçersiz kılınabilir ve genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-143">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="b83fc-144">Birkaç örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b83fc-144">A few examples follow:</span></span>

* <span data-ttu-id="b83fc-145">[Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) , uygulama için ek `IConfiguration` belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="b83fc-146">Aşağıdaki `ConfigureAppConfiguration` çağrı, *appSettings. xml* dosyasına uygulama yapılandırmasını dahil etmek için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-146">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="b83fc-147">`ConfigureAppConfiguration`birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-147">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="b83fc-148">Bu yapılandırmanın ana bilgisayar için (örneğin, sunucu URL 'Leri veya ortam) uygulanmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-148">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="b83fc-149">[Konak yapılandırma değerleri](#host-configuration-values) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-149">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="b83fc-150">Aşağıdaki `ConfigureLogging` çağrı, en düşük günlük düzeyi ([setminimumlevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) değerini [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel)olarak yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-150">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="b83fc-151">Bu ayar appSettings 'teki ayarları geçersiz kılar *. Development. JSON* (`LogLevel.Debug`) ve *appSettings. Production. JSON* (`LogLevel.Error`) tarafından `CreateDefaultBuilder`yapılandırıldı.</span><span class="sxs-lookup"><span data-stu-id="b83fc-151">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b83fc-152">`ConfigureLogging`birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-152">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="b83fc-153">Aşağıdaki çağrı `ConfigureKestrel` varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , `CreateDefaultBuilder`Kestrel yapılandırıldığında oluşturulan 30.000.000 bayt:</span><span class="sxs-lookup"><span data-stu-id="b83fc-153">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="b83fc-154">Aşağıdaki [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) çağrısı, varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel tarafından `CreateDefaultBuilder`yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:</span><span class="sxs-lookup"><span data-stu-id="b83fc-154">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="b83fc-155">*İçerik kökü* , konağın MVC görünüm dosyaları gibi içerik dosyalarını arayacağı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-155">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="b83fc-156">Uygulama, projenin kök klasöründen başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-156">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="b83fc-157">Bu, [Visual Studio](https://visualstudio.microsoft.com) 'da ve [DotNet yeni şablonlarda](/dotnet/core/tools/dotnet-new)kullanılan varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-157">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="b83fc-158">Uygulama yapılandırması hakkında daha fazla bilgi için bkz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-158">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="b83fc-159">Statik `CreateDefaultBuilder` yöntemin kullanılmasına alternatif olarak, [webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 'dan bir konak oluşturmak, ASP.NET Core 2. x ile desteklenen bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-159">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="b83fc-160">Bir ana bilgisayar ayarlanırken, [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-160">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="b83fc-161">Bir `Startup` sınıf belirtilmişse, bir `Configure` yöntemi tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-161">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="b83fc-162">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-162">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="b83fc-163">Birbirine eklenecek birden `ConfigureServices` çok çağrı.</span><span class="sxs-lookup"><span data-stu-id="b83fc-163">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="b83fc-164">Önceki ayarları `WebHostBuilder` Değiştir `Configure` üzerinde `UseStartup` veya üzerine birden çok çağrı.</span><span class="sxs-lookup"><span data-stu-id="b83fc-164">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="b83fc-165">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="b83fc-165">Host configuration values</span></span>

<span data-ttu-id="b83fc-166">[Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) , ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımları kullanır:</span><span class="sxs-lookup"><span data-stu-id="b83fc-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="b83fc-167">Bu biçimdeki `ASPNETCORE_{configurationKey}`ortam değişkenlerini içeren konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="b83fc-167">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="b83fc-168">Örneğin: `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="b83fc-168">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="b83fc-169">[Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) gibi uzantılar ( [geçersiz kılma yapılandırması](#override-configuration) bölümüne bakın).</span><span class="sxs-lookup"><span data-stu-id="b83fc-169">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="b83fc-170">[Usesetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="b83fc-171">İle `UseSetting`bir değer ayarlarken, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-171">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="b83fc-172">Konak, bir değeri en son ayarlayan seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-172">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="b83fc-173">Daha fazla bilgi için, sonraki bölümde [yapılandırmayı geçersiz kılma](#override-configuration) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-173">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="b83fc-174">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="b83fc-174">Application Key (Name)</span></span>

<span data-ttu-id="b83fc-175">Konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında, [ıhostingenvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-175">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="b83fc-176">Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-176">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="b83fc-177">Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-177">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="b83fc-178">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="b83fc-178">**Key**: applicationName</span></span>  
<span data-ttu-id="b83fc-179">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-179">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-180">**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="b83fc-180">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="b83fc-181">Şunu **kullanarak ayarla**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b83fc-181">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b83fc-182">**Ortam değişkeni**:`ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="b83fc-182">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="b83fc-183">Yakalama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="b83fc-183">Capture Startup Errors</span></span>

<span data-ttu-id="b83fc-184">Bu ayar, başlatma hatalarının yakalanmasını denetler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-184">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="b83fc-185">**Anahtar**: capturestartuperrors</span><span class="sxs-lookup"><span data-stu-id="b83fc-185">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="b83fc-186">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b83fc-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b83fc-187">**Varsayılan**: Uygulamanın IIS 'nin arkasında Kestrel, varsayılan olarak olduğu `true` durumlardışındaçalışır.`false`</span><span class="sxs-lookup"><span data-stu-id="b83fc-187">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="b83fc-188">Şunu **kullanarak ayarla**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="b83fc-188">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="b83fc-189">**Ortam değişkeni**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="b83fc-189">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="b83fc-190">Ne `false`zaman, başlatma sırasında oluşan hata, ana bilgisayardan çıkılıyor.</span><span class="sxs-lookup"><span data-stu-id="b83fc-190">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="b83fc-191">Ne `true`zaman, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-191">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="b83fc-192">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="b83fc-192">Content Root</span></span>

<span data-ttu-id="b83fc-193">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-193">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="b83fc-194">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="b83fc-194">**Key**: contentRoot</span></span>  
<span data-ttu-id="b83fc-195">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-195">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-196">**Varsayılan**: Uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-196">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="b83fc-197">Şunu **kullanarak ayarla**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="b83fc-197">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="b83fc-198">**Ortam değişkeni**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="b83fc-198">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="b83fc-199">İçerik kökü, [Web kök ayarı](#web-root)için temel yol olarak da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-199">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="b83fc-200">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="b83fc-200">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="b83fc-201">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="b83fc-201">Detailed Errors</span></span>

<span data-ttu-id="b83fc-202">Ayrıntılı hataların yakalanıp yakalanmayacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-202">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="b83fc-203">**Anahtar**: detailederrors</span><span class="sxs-lookup"><span data-stu-id="b83fc-203">**Key**: detailedErrors</span></span>  
<span data-ttu-id="b83fc-204">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b83fc-204">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b83fc-205">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="b83fc-205">**Default**: false</span></span>  
<span data-ttu-id="b83fc-206">Şunu **kullanarak ayarla**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b83fc-206">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b83fc-207">**Ortam değişkeni**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="b83fc-207">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="b83fc-208">Etkinleştirildiğinde (veya <a href="#environment">ortam</a> olarak `Development`ayarlandığında), uygulama ayrıntılı özel durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-208">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="b83fc-209">Ortam</span><span class="sxs-lookup"><span data-stu-id="b83fc-209">Environment</span></span>

<span data-ttu-id="b83fc-210">Uygulamanın ortamını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-210">Sets the app's environment.</span></span>

<span data-ttu-id="b83fc-211">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="b83fc-211">**Key**: environment</span></span>  
<span data-ttu-id="b83fc-212">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-212">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-213">**Varsayılan**: Üretiminden</span><span class="sxs-lookup"><span data-stu-id="b83fc-213">**Default**: Production</span></span>  
<span data-ttu-id="b83fc-214">Şunu **kullanarak ayarla**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b83fc-214">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="b83fc-215">**Ortam değişkeni**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="b83fc-215">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="b83fc-216">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-216">The environment can be set to any value.</span></span> <span data-ttu-id="b83fc-217">Çerçeve tanımlı değerler, `Development` `Staging`, ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b83fc-218">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-218">Values aren't case sensitive.</span></span> <span data-ttu-id="b83fc-219">Varsayılan olarak *ortam* , `ASPNETCORE_ENVIRONMENT` ortam değişkeninden okunurdur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-219">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="b83fc-220">[Visual Studio](https://visualstudio.microsoft.com)kullanılırken, *launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-220">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="b83fc-221">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-221">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="b83fc-222">Başlangıç derlemelerini barındırma</span><span class="sxs-lookup"><span data-stu-id="b83fc-222">Hosting Startup Assemblies</span></span>

<span data-ttu-id="b83fc-223">Uygulamanın barındırma başlangıç derlemelerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-223">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="b83fc-224">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b83fc-224">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="b83fc-225">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-225">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-226">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="b83fc-226">**Default**: Empty string</span></span>  
<span data-ttu-id="b83fc-227">Şunu **kullanarak ayarla**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b83fc-227">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b83fc-228">**Ortam değişkeni**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b83fc-228">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="b83fc-229">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="b83fc-229">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="b83fc-230">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-230">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="b83fc-231">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-231">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="b83fc-232">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="b83fc-232">HTTPS Port</span></span>

<span data-ttu-id="b83fc-233">HTTPS yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-233">Set the HTTPS redirect port.</span></span> <span data-ttu-id="b83fc-234">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-234">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="b83fc-235">**Anahtar**: https_port **Type**: *dize*
**varsayılan**: Varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-235">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="b83fc-236">Şunu **kullanarak ayarla**: `UseSetting`
**Ortam değişkeni**:`ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="b83fc-236">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="b83fc-237">Barındırma başlatma derlemeleri dışlama</span><span class="sxs-lookup"><span data-stu-id="b83fc-237">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="b83fc-238">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="b83fc-238">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="b83fc-239">**Anahtar**: hostingstartupexcludeassemblies</span><span class="sxs-lookup"><span data-stu-id="b83fc-239">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="b83fc-240">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-240">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-241">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="b83fc-241">**Default**: Empty string</span></span>  
<span data-ttu-id="b83fc-242">Şunu **kullanarak ayarla**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b83fc-242">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b83fc-243">**Ortam değişkeni**:`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b83fc-243">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="b83fc-244">Barındırma URL 'Lerini tercih et</span><span class="sxs-lookup"><span data-stu-id="b83fc-244">Prefer Hosting URLs</span></span>

<span data-ttu-id="b83fc-245">Konağın `WebHostBuilder` `IServer` uygulamayla yapılandırılanlar yerine ile yapılandırılan URL 'lerde dinleme yapıp kullanmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-245">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="b83fc-246">**Anahtar**: preferhostingurl 'leri</span><span class="sxs-lookup"><span data-stu-id="b83fc-246">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="b83fc-247">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b83fc-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b83fc-248">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="b83fc-248">**Default**: true</span></span>  
<span data-ttu-id="b83fc-249">Şunu **kullanarak ayarla**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="b83fc-249">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="b83fc-250">**Ortam değişkeni**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="b83fc-250">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="b83fc-251">Barındırma başlangıcını engelle</span><span class="sxs-lookup"><span data-stu-id="b83fc-251">Prevent Hosting Startup</span></span>

<span data-ttu-id="b83fc-252">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="b83fc-252">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="b83fc-253">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-253">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="b83fc-254">**Anahtar**: koruyucu thostingstartup</span><span class="sxs-lookup"><span data-stu-id="b83fc-254">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="b83fc-255">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b83fc-255">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b83fc-256">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="b83fc-256">**Default**: false</span></span>  
<span data-ttu-id="b83fc-257">Şunu **kullanarak ayarla**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="b83fc-257">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="b83fc-258">**Ortam değişkeni**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="b83fc-258">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="b83fc-259">Sunucu URL 'Leri</span><span class="sxs-lookup"><span data-stu-id="b83fc-259">Server URLs</span></span>

<span data-ttu-id="b83fc-260">Sunucunun istekler için dinlemesi gereken bağlantı noktaları ve protokoller içeren IP adreslerini veya ana bilgisayar adreslerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-260">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="b83fc-261">**Anahtar**: URL 'ler</span><span class="sxs-lookup"><span data-stu-id="b83fc-261">**Key**: urls</span></span>  
<span data-ttu-id="b83fc-262">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-262">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-263">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="b83fc-263">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="b83fc-264">Şunu **kullanarak ayarla**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="b83fc-264">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="b83fc-265">**Ortam değişkeni**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="b83fc-265">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="b83fc-266">Noktalı virgülle ayrılmış olarak ayarlayın (;) sunucunun yanıtlaması gereken URL ön eklerinin listesi.</span><span class="sxs-lookup"><span data-stu-id="b83fc-266">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="b83fc-267">Örneğin: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="b83fc-267">For example, `http://localhost:123`.</span></span> <span data-ttu-id="b83fc-268">Sunucunun belirtilen\*bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "" kullanın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-268">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="b83fc-269">Protokol (`http://` veya `https://`) her URL 'ye dahil edilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-269">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="b83fc-270">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-270">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="b83fc-271">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-271">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="b83fc-272">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-272">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="b83fc-273">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="b83fc-273">Shutdown Timeout</span></span>

<span data-ttu-id="b83fc-274">Web konağının kapanması için beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-274">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="b83fc-275">**Anahtar**: shutdowntimeoutseconds</span><span class="sxs-lookup"><span data-stu-id="b83fc-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="b83fc-276">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="b83fc-276">**Type**: *int*</span></span>  
<span data-ttu-id="b83fc-277">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="b83fc-277">**Default**: 5</span></span>  
<span data-ttu-id="b83fc-278">Şunu **kullanarak ayarla**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="b83fc-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="b83fc-279">**Ortam değişkeni**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="b83fc-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="b83fc-280">Anahtar, ile `UseSetting` bir *int* kabul etse de (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [useshutdowntimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi bir [TimeSpan](/dotnet/api/system.timespan)alır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="b83fc-281">Zaman aşımı süresi boyunca barındırma:</span><span class="sxs-lookup"><span data-stu-id="b83fc-281">During the timeout period, hosting:</span></span>

* <span data-ttu-id="b83fc-282">[Iapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)öğesini tetikler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-282">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="b83fc-283">Üzerinde durmayacak hizmetlerin hatalarını günlüğe kaydetmek için barındırılan hizmetleri durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-283">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="b83fc-284">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-284">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="b83fc-285">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-285">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="b83fc-286">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-286">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="b83fc-287">Başlangıç derlemesi</span><span class="sxs-lookup"><span data-stu-id="b83fc-287">Startup Assembly</span></span>

<span data-ttu-id="b83fc-288">`Startup` Sınıfın aranacağı derlemeyi belirler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-288">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="b83fc-289">**Anahtar**: startupassembly</span><span class="sxs-lookup"><span data-stu-id="b83fc-289">**Key**: startupAssembly</span></span>  
<span data-ttu-id="b83fc-290">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-290">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-291">**Varsayılan**: Uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="b83fc-291">**Default**: The app's assembly</span></span>  
<span data-ttu-id="b83fc-292">Şunu **kullanarak ayarla**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="b83fc-292">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="b83fc-293">**Ortam değişkeni**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="b83fc-293">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="b83fc-294">Ada (`string`) veya türe (`TStartup`) göre derlemeye başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-294">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="b83fc-295">Birden çok `UseStartup` yöntem çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-295">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="b83fc-296">Web kökü</span><span class="sxs-lookup"><span data-stu-id="b83fc-296">Web Root</span></span>

<span data-ttu-id="b83fc-297">Uygulamanın statik varlıklarının göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-297">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="b83fc-298">**Anahtar**: Webroot</span><span class="sxs-lookup"><span data-stu-id="b83fc-298">**Key**: webroot</span></span>  
<span data-ttu-id="b83fc-299">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b83fc-299">**Type**: *string*</span></span>  
<span data-ttu-id="b83fc-300">**Varsayılan**: Belirtilmemişse, varsayılan olarak "(Içerik kökü)/Wwwroot" olur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-300">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="b83fc-301">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-301">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="b83fc-302">Şunu **kullanarak ayarla**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="b83fc-302">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="b83fc-303">**Ortam değişkeni**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="b83fc-303">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="b83fc-304">Geçersiz kılma yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b83fc-304">Override configuration</span></span>

<span data-ttu-id="b83fc-305">Web konağını yapılandırmak için [yapılandırma](xref:fundamentals/configuration/index) kullanın.</span><span class="sxs-lookup"><span data-stu-id="b83fc-305">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="b83fc-306">Aşağıdaki örnekte, konak yapılandırması isteğe bağlı olarak bir *HostSettings. JSON* dosyasında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-306">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="b83fc-307">*HostSettings. JSON* dosyasından yüklenen herhangi bir yapılandırma komut satırı bağımsız değişkenleri tarafından geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-307">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="b83fc-308">Oluşturulan yapılandırma (içinde `config`), Konağı [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)ile yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b83fc-308">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="b83fc-309">`IWebHostBuilder`yapılandırma uygulamanın yapılandırmasına eklenir, ancak dönüştürme doğru&mdash; `ConfigureAppConfiguration` değildir, `IWebHostBuilder` yapılandırmayı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="b83fc-309">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="b83fc-310">*HostSettings. JSON* config `UseUrls` ile tarafından belirtilen yapılandırmayı geçersiz kılma, komut satırı bağımsız değişkeni yapılandırma saniyesi:</span><span class="sxs-lookup"><span data-stu-id="b83fc-310">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="b83fc-311">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b83fc-311">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="b83fc-312">[Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi şu anda tarafından `GetSection` döndürülen bir yapılandırma bölümünü ayrıştırma özelliğine sahip değil (örneğin,. `.UseConfiguration(Configuration.GetSection("section"))`</span><span class="sxs-lookup"><span data-stu-id="b83fc-312">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="b83fc-313">Yöntemi, yapılandırma anahtarlarını istenen bölüm ile filtreleyerek, ancak anahtarlar üzerinde bölüm adını bırakır (örneğin `section:environment`, `section:urls`,). `GetSection`</span><span class="sxs-lookup"><span data-stu-id="b83fc-313">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="b83fc-314">Yöntemi anahtarların `WebHostBuilder`anahtarlarlaeşleşmesini bekler (örneğin, `environment` ,).`urls` `UseConfiguration`</span><span class="sxs-lookup"><span data-stu-id="b83fc-314">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="b83fc-315">Anahtarlarda bölüm adının varlığı, bölümün değerlerinin konak yapılandırmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="b83fc-315">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="b83fc-316">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-316">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="b83fc-317">Daha fazla bilgi ve geçici çözüm için bkz [. yapılandırma bölümünü WebHostBuilder 'A geçirme. UseConfiguration tam anahtarları kullanır](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="b83fc-317">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="b83fc-318">`UseConfiguration`yalnızca belirtilen `IConfiguration` içindeki anahtarları ana bilgisayar Oluşturucu yapılandırmasına kopyalar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-318">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="b83fc-319">Bu nedenle, `reloadOnChange: true` JSON, ını ve xml ayarları dosyalarının ayarlanması etkisizdir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-319">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="b83fc-320">Belirli bir URL 'de çalıştırılacak Konağı belirtmek için, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)yürütürken istenen değer bir komut isteminden geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="b83fc-321">Komut satırı bağımsız değişkeni `urls` *HostSettings. JSON* dosyasından değeri geçersiz kılar ve sunucu 8080 numaralı bağlantı noktasını dinler:</span><span class="sxs-lookup"><span data-stu-id="b83fc-321">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="b83fc-322">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="b83fc-322">Manage the host</span></span>

<span data-ttu-id="b83fc-323">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="b83fc-323">**Run**</span></span>

<span data-ttu-id="b83fc-324">`Run` Yöntemi, Web uygulamasını başlatır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="b83fc-324">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="b83fc-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="b83fc-325">**Start**</span></span>

<span data-ttu-id="b83fc-326">`Start` Yöntemini çağırarak, Konağı engellenmeyen bir şekilde çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="b83fc-327">`Start` Yöntemine bir URL listesi geçirilirse, belirtilen URL 'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="b83fc-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="b83fc-328">Uygulama, statik bir kolaylık yöntemi `CreateDefaultBuilder` kullanarak önceden yapılandırılmış varsayılan ayarları kullanarak yeni bir ana bilgisayar başlatabilir ve başlatabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="b83fc-329">Bu yöntemler, konsol çıktısı olmadan sunucuyu başlatır ve [Waitforkapatmadan](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) bir kesme (CTRL-C/sigint veya sigterim) bekler:</span><span class="sxs-lookup"><span data-stu-id="b83fc-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="b83fc-330">**Başlat (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="b83fc-331">Kullanmaya `RequestDelegate`başlayın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b83fc-332">"Merhaba Dünya!" yanıtını almak `http://localhost:5000` için tarayıcıda bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="b83fc-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b83fc-333">`WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="b83fc-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b83fc-334">Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b83fc-335">**Başlangıç (dize URL 'si, RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="b83fc-336">URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="b83fc-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="b83fc-337">Uygulamanın yanıt `http://localhost:8080`vermesi dışında, **Başlangıç (requestdelegate uygulaması)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="b83fc-338">**Başlat (eylem&lt;ıroutebuilder&gt; routebuilder)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="b83fc-339">Yönlendirme ara yazılımını kullanmak `IRouteBuilder` için ([Microsoft. aspnetcore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) örneğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="b83fc-340">Aşağıdaki tarayıcı isteklerini örnekle birlikte kullanın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="b83fc-341">İstek</span><span class="sxs-lookup"><span data-stu-id="b83fc-341">Request</span></span>                                    | <span data-ttu-id="b83fc-342">Yanıt</span><span class="sxs-lookup"><span data-stu-id="b83fc-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="b83fc-343">Merhaba, martın!</span><span class="sxs-lookup"><span data-stu-id="b83fc-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="b83fc-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="b83fc-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="b83fc-345">"Ooops!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="b83fc-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="b83fc-346">"Uh oh!" dizesiyle bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="b83fc-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="b83fc-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="b83fc-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="b83fc-348">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="b83fc-348">Hello World!</span></span>                             |

<span data-ttu-id="b83fc-349">`WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="b83fc-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b83fc-350">Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b83fc-351">**Başlat (dize URL 'si,&lt;eylem ıroutebuilder&gt; routebuilder)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="b83fc-352">Bir URL ve örneği `IRouteBuilder`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="b83fc-353">Uygulamanın Yanıt`http://localhost:8080`vermesi dışında, **Başlangıç (eylem&lt;ıroutebuilder&gt; routebuilder)** ile aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-353">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="b83fc-354">**StartWith (eylem&lt;IApplicationBuilder&gt; uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="b83fc-355">Yapılandırmak için bir temsilci sağlayın `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b83fc-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b83fc-356">"Merhaba Dünya!" yanıtını almak `http://localhost:5000` için tarayıcıda bir istek yapın</span><span class="sxs-lookup"><span data-stu-id="b83fc-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="b83fc-357">`WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="b83fc-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="b83fc-358">Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.</span><span class="sxs-lookup"><span data-stu-id="b83fc-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="b83fc-359">**StartWith (dize URL 'si,&lt;eylem IApplicationBuilder&gt; uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="b83fc-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="b83fc-360">Yapılandırmak için bir URL ve temsilci sağlayın `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="b83fc-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="b83fc-361">Uygulamanın Yanıt`http://localhost:8080`vermesi dışında, **StartWith (Action&lt;iapplicationbuilder&gt; uygulaması) ile**aynı sonucu üretir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-361">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="b83fc-362">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="b83fc-362">IHostingEnvironment interface</span></span>

<span data-ttu-id="b83fc-363">[Ihostingenvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) , uygulamanın Web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-363">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="b83fc-364">Özelliklerini ve uzantı yöntemlerini kullanmak üzere `IHostingEnvironment` sağlamak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-364">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="b83fc-365">Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-365">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="b83fc-366">Alternatif olarak, `IHostingEnvironment` ' de `ConfigureServices`kullanım `Startup` için oluşturucuya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b83fc-366">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="b83fc-367">`IsDevelopment` Uzantı yöntemine`IsProduction` ekolarak`IsEnvironment(string environmentName)` ,, ve yöntemleri. `IHostingEnvironment` `IsStaging`</span><span class="sxs-lookup"><span data-stu-id="b83fc-367">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="b83fc-368">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b83fc-368">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="b83fc-369">Hizmet ayrıca işlem ardışık düzenini ayarlama `Configure` yöntemine doğrudan eklenebilir: `IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b83fc-369">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="b83fc-370">`IHostingEnvironment`özel [Ara yazılım](xref:fundamentals/middleware/write)oluşturulurken yöntemineeklenebilir:`Invoke`</span><span class="sxs-lookup"><span data-stu-id="b83fc-370">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="b83fc-371">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="b83fc-371">IApplicationLifetime interface</span></span>

<span data-ttu-id="b83fc-372">[Iapplicationlifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) , başlatma sonrası ve kapalı etkinlikler için izin verir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="b83fc-373">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan yöntemleri kaydetmek `Action` için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-373">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="b83fc-374">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="b83fc-374">Cancellation Token</span></span>    | <span data-ttu-id="b83fc-375">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="b83fc-375">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="b83fc-376">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="b83fc-376">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="b83fc-377">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="b83fc-377">The host has fully started.</span></span> |
| [<span data-ttu-id="b83fc-378">Applicationdurdurulan</span><span class="sxs-lookup"><span data-stu-id="b83fc-378">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="b83fc-379">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="b83fc-379">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="b83fc-380">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-380">All requests should be processed.</span></span> <span data-ttu-id="b83fc-381">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-381">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="b83fc-382">Applicationdurduruluyor</span><span class="sxs-lookup"><span data-stu-id="b83fc-382">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="b83fc-383">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="b83fc-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="b83fc-384">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-384">Requests may still be processing.</span></span> <span data-ttu-id="b83fc-385">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-385">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="b83fc-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) , uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="b83fc-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="b83fc-387">Aşağıdaki sınıf, `Shutdown` sınıfın `StopApplication` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için kullanır:</span><span class="sxs-lookup"><span data-stu-id="b83fc-387">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="b83fc-388">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="b83fc-388">Scope validation</span></span>

<span data-ttu-id="b83fc-389">[Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) , uygulamanın ortamı geliştirmede `true` [serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) ' i ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="b83fc-390">`ValidateScopes` , Olarak`true`ayarlandığında, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="b83fc-390">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="b83fc-391">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="b83fc-391">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="b83fc-392">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-392">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="b83fc-393">[Buildserviceprovider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-393">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="b83fc-394">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b83fc-394">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="b83fc-395">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="b83fc-395">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="b83fc-396">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="b83fc-396">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="b83fc-397">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` , çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="b83fc-397">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="b83fc-398">Üretim ortamında da dahil olmak üzere kapsamları her zaman doğrulamak için, ana bilgisayar Oluşturucu 'da [Serviceprovideroptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) 'ı [usedefaultserviceprovider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) ile yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="b83fc-398">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="b83fc-399">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b83fc-399">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
