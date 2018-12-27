---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu olan ASP.NET Core web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7215027a083c0ed0bc3b15196e390a31c5dcfc14
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637852"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="57b83-103">ASP.NET Core Web ana bilgisayarı</span><span class="sxs-lookup"><span data-stu-id="57b83-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="57b83-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="57b83-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="57b83-105">Bu konuda 1.1 sürümü için indirme [ASP.NET Core Web ana bilgisayarı (sürüm 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="57b83-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="57b83-106">ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="57b83-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="57b83-107">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="57b83-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="57b83-108">En az bir sunucu ve istek işleme ardışık konak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="57b83-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="57b83-109">Bu konu, ASP.NET Core Web ana bilgisayarı kapsar ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), web uygulamalarını barındırmak için kullanışlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="57b83-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="57b83-110">Kapsamı .NET genel ana bilgisayar için ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), bkz: <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="57b83-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="57b83-111">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="57b83-111">Set up a host</span></span>

<span data-ttu-id="57b83-112">Bir konak örneği kullanarak oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="57b83-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="57b83-113">Bu, genellikle uygulamanın Giriş noktasında gerçekleştirilir `Main` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="57b83-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="57b83-114">Proje şablonlarındaki `Main` bulunan *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="57b83-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="57b83-115">Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir konak kurulumunu başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="57b83-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="57b83-116">`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="57b83-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="57b83-117">Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) uygulamayı kullanarak web sunucusu olarak yapılandırma sağlayıcıları barındıran.</span><span class="sxs-lookup"><span data-stu-id="57b83-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="57b83-118">Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="57b83-118">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="57b83-119">İçerik kök tarafından döndürülen yol ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="57b83-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="57b83-120">Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:</span><span class="sxs-lookup"><span data-stu-id="57b83-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="57b83-121">Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="57b83-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="57b83-122">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="57b83-122">Command-line arguments.</span></span>
* <span data-ttu-id="57b83-123">Uygulama yapılandırması ile aşağıdaki sırada yükler:</span><span class="sxs-lookup"><span data-stu-id="57b83-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="57b83-124">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="57b83-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="57b83-125">*appSettings. {Ortamı} .json*.</span><span class="sxs-lookup"><span data-stu-id="57b83-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="57b83-126">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.</span><span class="sxs-lookup"><span data-stu-id="57b83-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="57b83-127">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="57b83-127">Environment variables.</span></span>
  * <span data-ttu-id="57b83-128">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="57b83-128">Command-line arguments.</span></span>
* <span data-ttu-id="57b83-129">Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktı.</span><span class="sxs-lookup"><span data-stu-id="57b83-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="57b83-130">Günlük kaydı içerir [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.</span><span class="sxs-lookup"><span data-stu-id="57b83-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="57b83-131">Arkasında IIS ile çalıştırırken [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` sağlayan [IIS tümleştirme](xref:host-and-deploy/iis/index), uygulamanın taban adresini ve bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="57b83-131">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="57b83-132">IIS tümleştirme, ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="57b83-132">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="57b83-133">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="57b83-133">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="57b83-134">Kümeleri [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="57b83-134">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="57b83-135">Daha fazla bilgi için [kapsam doğrulama](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="57b83-135">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="57b83-136">Tarafından tanımlanan yapılandırma `CreateDefaultBuilder` geçersiz kılındı ve tarafından Genişletilmiş [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), diğer yöntemler ve uzantı yöntemlerinin [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="57b83-136">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="57b83-137">Aşağıda birkaç örnek verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="57b83-137">A few examples follow:</span></span>

* <span data-ttu-id="57b83-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulama için.</span><span class="sxs-lookup"><span data-stu-id="57b83-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="57b83-139">Aşağıdaki `ConfigureAppConfiguration` çağrıyı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya.</span><span class="sxs-lookup"><span data-stu-id="57b83-139">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="57b83-140">`ConfigureAppConfiguration` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-140">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="57b83-141">Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) geçerli değil olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="57b83-141">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="57b83-142">Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.</span><span class="sxs-lookup"><span data-stu-id="57b83-142">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="57b83-143">Aşağıdaki `ConfigureLogging` en düşük günlük düzeyi yapılandıracak bir temsilci çağrısı ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="57b83-143">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="57b83-144">Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="57b83-144">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="57b83-145">`ConfigureLogging` birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-145">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="57b83-146">Aşağıdaki çağrı `ConfigureKestrel` varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57b83-146">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="57b83-147">Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57b83-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="57b83-148">*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler.</span><span class="sxs-lookup"><span data-stu-id="57b83-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="57b83-149">Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57b83-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="57b83-150">Kullanılan varsayılan [Visual Studio](https://www.visualstudio.com/) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="57b83-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="57b83-151">Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="57b83-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="57b83-152">Statik kullanmaya alternatif olarak `CreateDefaultBuilder` konaktan oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) desteklenen bir yaklaşım ile ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="57b83-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="57b83-153">Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.</span><span class="sxs-lookup"><span data-stu-id="57b83-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="57b83-154">Bir ana bilgisayar, ayarlarken [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-154">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="57b83-155">Varsa bir `Startup` sınıf belirtilmişse, tanımlamanız gerekir bir `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="57b83-155">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="57b83-156">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="57b83-156">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="57b83-157">Birden çok çağrılar `ConfigureServices` birbirine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="57b83-157">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="57b83-158">Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarları değiştirin.</span><span class="sxs-lookup"><span data-stu-id="57b83-158">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="57b83-159">Ana bilgisayar yapılandırma değerleri</span><span class="sxs-lookup"><span data-stu-id="57b83-159">Host configuration values</span></span>

<span data-ttu-id="57b83-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:</span><span class="sxs-lookup"><span data-stu-id="57b83-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="57b83-161">Ortam değişkenlerini biçiminde içerir konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="57b83-161">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="57b83-162">Örneğin: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="57b83-162">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="57b83-163">Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).</span><span class="sxs-lookup"><span data-stu-id="57b83-163">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="57b83-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar.</span><span class="sxs-lookup"><span data-stu-id="57b83-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="57b83-165">Bir değerle ayarlarken `UseSetting`, değer türünden bağımsız olarak bir dize olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="57b83-165">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="57b83-166">Konak, bir değer, en son hangi seçeneği ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="57b83-166">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="57b83-167">Daha fazla bilgi için [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.</span><span class="sxs-lookup"><span data-stu-id="57b83-167">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="57b83-168">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="57b83-168">Application Key (Name)</span></span>

<span data-ttu-id="57b83-169">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanmış olduğunda [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) konak yapım sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="57b83-169">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="57b83-170">Değer, uygulamanın giriş noktasını içeren derleme adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="57b83-170">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="57b83-171">Değeri açıkça ayarlamak için kullanın [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="57b83-171">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="57b83-172">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="57b83-172">**Key**: applicationName</span></span>  
<span data-ttu-id="57b83-173">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-173">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-174">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="57b83-174">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="57b83-175">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57b83-175">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57b83-176">**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="57b83-176">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="57b83-177">Başlangıç hatalarını yakalama</span><span class="sxs-lookup"><span data-stu-id="57b83-177">Capture Startup Errors</span></span>

<span data-ttu-id="57b83-178">Bu ayar, başlangıç hatalarını yakalama denetler.</span><span class="sxs-lookup"><span data-stu-id="57b83-178">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="57b83-179">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="57b83-179">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="57b83-180">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="57b83-180">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57b83-181">**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="57b83-181">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="57b83-182">**Kullanılarak ayarlanan**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="57b83-182">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="57b83-183">**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="57b83-183">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="57b83-184">Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="57b83-184">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="57b83-185">Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="57b83-185">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="57b83-186">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="57b83-186">Content Root</span></span>

<span data-ttu-id="57b83-187">Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramasını başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="57b83-187">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="57b83-188">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="57b83-188">**Key**: contentRoot</span></span>  
<span data-ttu-id="57b83-189">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-189">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-190">**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.</span><span class="sxs-lookup"><span data-stu-id="57b83-190">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="57b83-191">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="57b83-191">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="57b83-192">**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="57b83-192">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="57b83-193">İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root).</span><span class="sxs-lookup"><span data-stu-id="57b83-193">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="57b83-194">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="57b83-194">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="57b83-195">Ayrıntılı hatalar</span><span class="sxs-lookup"><span data-stu-id="57b83-195">Detailed Errors</span></span>

<span data-ttu-id="57b83-196">Ayrıntılı hatalar yakalanmasının gerekip belirler.</span><span class="sxs-lookup"><span data-stu-id="57b83-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="57b83-197">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="57b83-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="57b83-198">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="57b83-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57b83-199">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="57b83-199">**Default**: false</span></span>  
<span data-ttu-id="57b83-200">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57b83-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57b83-201">**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="57b83-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="57b83-202">Etkin olduğunda (veya <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumlar yakalar.</span><span class="sxs-lookup"><span data-stu-id="57b83-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="57b83-203">Ortam</span><span class="sxs-lookup"><span data-stu-id="57b83-203">Environment</span></span>

<span data-ttu-id="57b83-204">Uygulamanın ortamı ayarlar.</span><span class="sxs-lookup"><span data-stu-id="57b83-204">Sets the app's environment.</span></span>

<span data-ttu-id="57b83-205">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="57b83-205">**Key**: environment</span></span>  
<span data-ttu-id="57b83-206">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-206">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-207">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="57b83-207">**Default**: Production</span></span>  
<span data-ttu-id="57b83-208">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="57b83-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="57b83-209">**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="57b83-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="57b83-210">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-210">The environment can be set to any value.</span></span> <span data-ttu-id="57b83-211">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="57b83-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="57b83-212">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="57b83-212">Values aren't case sensitive.</span></span> <span data-ttu-id="57b83-213">Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni.</span><span class="sxs-lookup"><span data-stu-id="57b83-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="57b83-214">Kullanırken [Visual Studio](https://www.visualstudio.com/), ortam değişkenleri ayarlanabilir *launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="57b83-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="57b83-215">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="57b83-215">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="57b83-216">Başlangıç derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="57b83-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="57b83-217">Uygulamanın barındırma başlangıç derlemeleri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="57b83-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="57b83-218">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="57b83-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="57b83-219">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-219">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-220">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="57b83-220">**Default**: Empty string</span></span>  
<span data-ttu-id="57b83-221">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57b83-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57b83-222">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="57b83-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="57b83-223">Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="57b83-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="57b83-224">Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="57b83-224">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="57b83-225">Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="57b83-225">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="57b83-226">HTTPS bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="57b83-226">HTTPS Port</span></span>

<span data-ttu-id="57b83-227">HTTPS, yeniden yönlendirme bağlantı noktasını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="57b83-227">Set the HTTPS redirect port.</span></span> <span data-ttu-id="57b83-228">Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="57b83-228">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="57b83-229">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="57b83-229">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="57b83-230">**Kullanılarak ayarlanan**: `UseSetting`
**Ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="57b83-230">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="57b83-231">Başlangıç dışlama derlemeleri barındırma</span><span class="sxs-lookup"><span data-stu-id="57b83-231">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="57b83-232">Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="57b83-232">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="57b83-233">**Anahtar**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="57b83-233">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="57b83-234">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-234">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-235">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="57b83-235">**Default**: Empty string</span></span>  
<span data-ttu-id="57b83-236">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57b83-236">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57b83-237">**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="57b83-237">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="57b83-238">URL'leri barındırma tercih et</span><span class="sxs-lookup"><span data-stu-id="57b83-238">Prefer Hosting URLs</span></span>

<span data-ttu-id="57b83-239">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="57b83-239">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="57b83-240">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="57b83-240">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="57b83-241">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="57b83-241">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57b83-242">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="57b83-242">**Default**: true</span></span>  
<span data-ttu-id="57b83-243">**Kullanılarak ayarlanan**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="57b83-243">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="57b83-244">**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="57b83-244">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="57b83-245">Başlangıç barındırma engelle</span><span class="sxs-lookup"><span data-stu-id="57b83-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="57b83-246">Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="57b83-247">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="57b83-247">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="57b83-248">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="57b83-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="57b83-249">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="57b83-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="57b83-250">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="57b83-250">**Default**: false</span></span>  
<span data-ttu-id="57b83-251">**Kullanılarak ayarlanan**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="57b83-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="57b83-252">**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="57b83-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="57b83-253">Sunucu URL'leri</span><span class="sxs-lookup"><span data-stu-id="57b83-253">Server URLs</span></span>

<span data-ttu-id="57b83-254">IP adresi veya konak bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri adresleriyle gösterir.</span><span class="sxs-lookup"><span data-stu-id="57b83-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="57b83-255">**Anahtar**: URL'ler</span><span class="sxs-lookup"><span data-stu-id="57b83-255">**Key**: urls</span></span>  
<span data-ttu-id="57b83-256">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-256">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-257">**Varsayılan**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="57b83-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="57b83-258">**Kullanılarak ayarlanan**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="57b83-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="57b83-259">**Ortam değişkeni**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="57b83-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="57b83-260">Bir noktalı virgülle ayrılmış ayarlayın (;) listesini URL ön ekleri sunucunun yanıt vermelidir.</span><span class="sxs-lookup"><span data-stu-id="57b83-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="57b83-261">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="57b83-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="57b83-262">Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="57b83-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="57b83-263">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="57b83-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="57b83-264">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="57b83-264">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="57b83-265">Kestrel'i kendi API uç nokta yapılandırması vardır.</span><span class="sxs-lookup"><span data-stu-id="57b83-265">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="57b83-266">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="57b83-266">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="57b83-267">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="57b83-267">Shutdown Timeout</span></span>

<span data-ttu-id="57b83-268">Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="57b83-268">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="57b83-269">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="57b83-269">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="57b83-270">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="57b83-270">**Type**: *int*</span></span>  
<span data-ttu-id="57b83-271">**Varsayılan**: 5</span><span class="sxs-lookup"><span data-stu-id="57b83-271">**Default**: 5</span></span>  
<span data-ttu-id="57b83-272">**Kullanılarak ayarlanan**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="57b83-272">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="57b83-273">**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="57b83-273">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="57b83-274">Anahtarı kabul eder ancak bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi alır bir [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="57b83-274">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="57b83-275">Sırasında zaman aşımı süresi, barındırma:</span><span class="sxs-lookup"><span data-stu-id="57b83-275">During the timeout period, hosting:</span></span>

* <span data-ttu-id="57b83-276">Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="57b83-276">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="57b83-277">Barındırılan hizmetler, durdurmak için başarısız olan hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="57b83-277">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="57b83-278">Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur.</span><span class="sxs-lookup"><span data-stu-id="57b83-278">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="57b83-279">İşlem tamamlandı olmasanız bile hizmetleri durdurun.</span><span class="sxs-lookup"><span data-stu-id="57b83-279">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="57b83-280">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="57b83-280">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="57b83-281">Başlangıç derleme</span><span class="sxs-lookup"><span data-stu-id="57b83-281">Startup Assembly</span></span>

<span data-ttu-id="57b83-282">Aramak için derleme belirler `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="57b83-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="57b83-283">**Anahtar**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="57b83-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="57b83-284">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-284">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-285">**Varsayılan**: Uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="57b83-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="57b83-286">**Kullanılarak ayarlanan**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="57b83-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="57b83-287">**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="57b83-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="57b83-288">Ada göre bütünleştirilmiş kod (`string`) veya türü (`TStartup`) başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="57b83-289">Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="57b83-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="57b83-290">Web kökü</span><span class="sxs-lookup"><span data-stu-id="57b83-290">Web Root</span></span>

<span data-ttu-id="57b83-291">Uygulamanın statik varlıklar için göreli yolunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="57b83-291">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="57b83-292">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="57b83-292">**Key**: webroot</span></span>  
<span data-ttu-id="57b83-293">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="57b83-293">**Type**: *string*</span></span>  
<span data-ttu-id="57b83-294">**Varsayılan**: Belirtilmezse, varsayılan "(Content Root)/wwwroot olur.", yol varsa.</span><span class="sxs-lookup"><span data-stu-id="57b83-294">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="57b83-295">Yol mevcut değilse, ardından İşlemsiz dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57b83-295">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="57b83-296">**Kullanılarak ayarlanan**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="57b83-296">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="57b83-297">**Ortam değişkeni**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="57b83-297">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="57b83-298">Yapılandırma geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="57b83-298">Override configuration</span></span>

<span data-ttu-id="57b83-299">Kullanım [yapılandırma](xref:fundamentals/configuration/index) web ana bilgisayarı yapılandırılamadı.</span><span class="sxs-lookup"><span data-stu-id="57b83-299">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="57b83-300">Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="57b83-300">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="57b83-301">Herhangi bir yapılandırma öğesinden yüklenen *hostsettings.json* dosya tarafından komut satırı bağımsız değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="57b83-301">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="57b83-302">Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="57b83-302">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="57b83-303">`IWebHostBuilder` yapılandırma, uygulamanın yapılandırma için eklenir, ancak listesiyse true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="57b83-303">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="57b83-304">Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:</span><span class="sxs-lookup"><span data-stu-id="57b83-304">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="57b83-305">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="57b83-305">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="57b83-306">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="57b83-306">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="57b83-307">`GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="57b83-307">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="57b83-308">`UseConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="57b83-308">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="57b83-309">Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-309">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="57b83-310">Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir.</span><span class="sxs-lookup"><span data-stu-id="57b83-310">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="57b83-311">Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="57b83-311">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="57b83-312">`UseConfiguration` anahtarlar yalnızca sağlanan akış kopyalar `IConfiguration` konak Oluşturucu yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="57b83-312">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="57b83-313">Bu nedenle, ayarı `reloadOnChange: true` JSON INI ve XML ayar dosyaları için hiçbir etkisi olmaz.</span><span class="sxs-lookup"><span data-stu-id="57b83-313">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="57b83-314">Ana belirli URL'yi belirtmek için istenen değeri bir komut istemi'nden yürütülürken geçirilebilir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="57b83-314">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="57b83-315">Komut satırı bağımsız değişkeni geçersiz kılmalar `urls` değerini *hostsettings.json* dosya ve sunucu, 8080 bağlantı noktasında dinler:</span><span class="sxs-lookup"><span data-stu-id="57b83-315">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="57b83-316">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="57b83-316">Manage the host</span></span>

<span data-ttu-id="57b83-317">**Çalıştır**</span><span class="sxs-lookup"><span data-stu-id="57b83-317">**Run**</span></span>

<span data-ttu-id="57b83-318">`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="57b83-318">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="57b83-319">**Start**</span><span class="sxs-lookup"><span data-stu-id="57b83-319">**Start**</span></span>

<span data-ttu-id="57b83-320">Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="57b83-320">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="57b83-321">URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:</span><span class="sxs-lookup"><span data-stu-id="57b83-321">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="57b83-322">Uygulamayı başlatın ve önceden yapılandırılmış varsayılan kullanarak yeni bir ana bilgisayarı başlatılamıyor `CreateDefaultBuilder` statik kolaylık yöntemi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="57b83-322">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="57b83-323">Bu yöntemler konsol çıktısı olmadan ve sunucuyu başlatın [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için mola bekleyin:</span><span class="sxs-lookup"><span data-stu-id="57b83-323">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="57b83-324">**Başlangıç (RequestDelegate uygulaması)**</span><span class="sxs-lookup"><span data-stu-id="57b83-324">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="57b83-325">İle başlayan bir `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="57b83-325">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57b83-326">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="57b83-326">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="57b83-327">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-327">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57b83-328">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="57b83-328">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57b83-329">**Başlangıç (dize url, RequestDelegate uygulama)**</span><span class="sxs-lookup"><span data-stu-id="57b83-329">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="57b83-330">Bir URL ile başlayın ve `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="57b83-330">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="57b83-331">Aynı sonucu üretir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57b83-331">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="57b83-332">**Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="57b83-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="57b83-333">Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="57b83-333">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="57b83-334">Aşağıdaki tarayıcı isteklerini ile örneği kullanın:</span><span class="sxs-lookup"><span data-stu-id="57b83-334">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="57b83-335">İstek</span><span class="sxs-lookup"><span data-stu-id="57b83-335">Request</span></span>                                    | <span data-ttu-id="57b83-336">Yanıt</span><span class="sxs-lookup"><span data-stu-id="57b83-336">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="57b83-337">Martin Merhaba!</span><span class="sxs-lookup"><span data-stu-id="57b83-337">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="57b83-338">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="57b83-338">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="57b83-339">"Ooops!" dizesi ile bir özel durum oluşturur</span><span class="sxs-lookup"><span data-stu-id="57b83-339">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="57b83-340">Bir özel durum dizesi ile oluşturur "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="57b83-340">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="57b83-341">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="57b83-341">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="57b83-342">Merhaba Dünya!</span><span class="sxs-lookup"><span data-stu-id="57b83-342">Hello World!</span></span>                             |

<span data-ttu-id="57b83-343">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57b83-344">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="57b83-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57b83-345">**Başlat (eylem url dize&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="57b83-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="57b83-346">Bir URL örneği kullanıp `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57b83-346">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="57b83-347">Aynı sonucu üretir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57b83-347">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="57b83-348">**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="57b83-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="57b83-349">Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57b83-349">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="57b83-350">Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için</span><span class="sxs-lookup"><span data-stu-id="57b83-350">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="57b83-351">`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-351">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="57b83-352">Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.</span><span class="sxs-lookup"><span data-stu-id="57b83-352">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="57b83-353">**StartWith (url, eylem dizesi&lt;IApplicationBuilder&gt; uygulama)**</span><span class="sxs-lookup"><span data-stu-id="57b83-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="57b83-354">Bir URL ve yapılandıracak bir temsilci sağlar bir `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="57b83-354">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="57b83-355">Aynı sonucu üretir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="57b83-355">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="57b83-356">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="57b83-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="57b83-357">[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="57b83-357">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="57b83-358">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="57b83-358">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="57b83-359">A [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortama bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-359">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="57b83-360">Alternatif olarak, ekleme `IHostingEnvironment` içine `Startup` Oluşturucusu kullanılmak üzere `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="57b83-360">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="57b83-361">Ek olarak `IsDevelopment` genişletme yöntemi `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="57b83-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="57b83-362">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="57b83-362">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="57b83-363">`IHostingEnvironment` Hizmet de eklenen doğrudan `Configure` işleme işlem hattı ayarlamaya yönelik yöntemi:</span><span class="sxs-lookup"><span data-stu-id="57b83-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="57b83-364">`IHostingEnvironment` içine yerleştirilebilir `Invoke` özel oluştururken yöntemi [ara yazılım](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="57b83-364">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="57b83-365">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="57b83-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="57b83-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="57b83-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="57b83-367">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="57b83-367">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="57b83-368">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="57b83-368">Cancellation Token</span></span>    | <span data-ttu-id="57b83-369">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="57b83-369">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="57b83-370">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="57b83-370">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="57b83-371">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="57b83-371">The host has fully started.</span></span> |
| [<span data-ttu-id="57b83-372">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="57b83-372">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="57b83-373">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="57b83-373">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="57b83-374">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="57b83-374">All requests should be processed.</span></span> <span data-ttu-id="57b83-375">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-375">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="57b83-376">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="57b83-376">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="57b83-377">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="57b83-377">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="57b83-378">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="57b83-378">Requests may still be processing.</span></span> <span data-ttu-id="57b83-379">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="57b83-379">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="57b83-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister.</span><span class="sxs-lookup"><span data-stu-id="57b83-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="57b83-381">Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="57b83-381">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="57b83-382">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="57b83-382">Scope validation</span></span>

<span data-ttu-id="57b83-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.</span><span class="sxs-lookup"><span data-stu-id="57b83-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="57b83-384">Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı, doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="57b83-384">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="57b83-385">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="57b83-385">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="57b83-386">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="57b83-386">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="57b83-387">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="57b83-387">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="57b83-388">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="57b83-388">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="57b83-389">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="57b83-389">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="57b83-390">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="57b83-390">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="57b83-391">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="57b83-391">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="57b83-392">Kapsamlar üretim ortamında dahil olmak üzere, her zaman doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:</span><span class="sxs-lookup"><span data-stu-id="57b83-392">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="57b83-393">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="57b83-393">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
