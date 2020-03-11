---
title: .NET genel ana bilgisayar
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu .NET Core genel ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 6a0ef02db883db3bc91722786cd042ccec092735
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659924"
---
# <a name="net-generic-host"></a><span data-ttu-id="034a5-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="034a5-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="034a5-104">Bu makalede .NET Core genel ana bilgisayarı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) tanıtılmakta ve nasıl kullanılacağına ilişkin yönergeler sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="034a5-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="034a5-105">Ana bilgisayar nedir?</span><span class="sxs-lookup"><span data-stu-id="034a5-105">What's a host?</span></span>

<span data-ttu-id="034a5-106">*Ana bilgisayar* , bir uygulamanın kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="034a5-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="034a5-107">Bağımlılık ekleme (dı)</span><span class="sxs-lookup"><span data-stu-id="034a5-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="034a5-108">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="034a5-108">Logging</span></span>
* <span data-ttu-id="034a5-109">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="034a5-109">Configuration</span></span>
* <span data-ttu-id="034a5-110">`IHostedService` uygulamalar</span><span class="sxs-lookup"><span data-stu-id="034a5-110">`IHostedService` implementations</span></span>

<span data-ttu-id="034a5-111">Bir konak başlatıldığında, DI kapsayıcısında bulduğu <xref:Microsoft.Extensions.Hosting.IHostedService> her bir uygulamada `IHostedService.StartAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="034a5-112">Bir Web uygulamasında, `IHostedService` uygulamalarından biri, [http sunucu uygulaması](xref:fundamentals/index#servers)Başlatan bir Web hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="034a5-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="034a5-113">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="034a5-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="034a5-114">3,0 ' den önceki ASP.NET Core sürümlerinde, [Web ana BILGISAYARı](xref:fundamentals/host/web-host) http iş yükleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="034a5-115">Web ana bilgisayarı artık Web uygulamaları için önerilmez ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="034a5-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="034a5-116">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="034a5-116">Set up a host</span></span>

<span data-ttu-id="034a5-117">Konak genellikle `Program` sınıfındaki kodla yapılandırılır, oluşturulur ve çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="034a5-118">`Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="034a5-118">The `Main` method:</span></span>

* <span data-ttu-id="034a5-119">Bir Oluşturucu nesnesi oluşturmak ve yapılandırmak için bir `CreateHostBuilder` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="034a5-120">Oluşturucu nesnesinde `Build` ve `Run` yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="034a5-121">İşte, tek bir `IHostedService` uygulama olarak dı kapsayıcısına eklenen HTTP olmayan bir iş yükü için *program.cs* kodu.</span><span class="sxs-lookup"><span data-stu-id="034a5-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="034a5-122">Bir HTTP iş yükü için `Main` yöntemi aynıdır ancak `CreateHostBuilder` `ConfigureWebHostDefaults`çağırır:</span><span class="sxs-lookup"><span data-stu-id="034a5-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="034a5-123">Uygulama Entity Framework Core kullanıyorsa `CreateHostBuilder` yönteminin adını veya imzasını değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="034a5-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="034a5-124">[Entity Framework Core araçları](/ef/core/miscellaneous/cli/) , uygulamayı çalıştırmadan Konağı yapılandıran bir `CreateHostBuilder` yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="034a5-125">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="034a5-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="034a5-126">Varsayılan Oluşturucu ayarları</span><span class="sxs-lookup"><span data-stu-id="034a5-126">Default builder settings</span></span>

<span data-ttu-id="034a5-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="034a5-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="034a5-128">[İçerik kökünü](xref:fundamentals/index#content-root) <xref:System.IO.Directory.GetCurrentDirectory*>tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="034a5-129">Ana bilgisayar yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="034a5-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="034a5-130">"DOTNET_" önekli ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="034a5-131">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-131">Command-line arguments.</span></span>
* <span data-ttu-id="034a5-132">Uygulama yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="034a5-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="034a5-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="034a5-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="034a5-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="034a5-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="034a5-135">Uygulama `Development` ortamda çalıştığında [gizli dizi Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="034a5-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="034a5-136">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-136">Environment variables.</span></span>
  * <span data-ttu-id="034a5-137">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-137">Command-line arguments.</span></span>
* <span data-ttu-id="034a5-138">Aşağıdaki [günlük](xref:fundamentals/logging/index) sağlayıcılarını ekler:</span><span class="sxs-lookup"><span data-stu-id="034a5-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="034a5-139">Konsol</span><span class="sxs-lookup"><span data-stu-id="034a5-139">Console</span></span>
  * <span data-ttu-id="034a5-140">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="034a5-140">Debug</span></span>
  * <span data-ttu-id="034a5-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="034a5-141">EventSource</span></span>
  * <span data-ttu-id="034a5-142">Olay günlüğü (yalnızca Windows üzerinde çalışırken)</span><span class="sxs-lookup"><span data-stu-id="034a5-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="034a5-143">Ortam geliştirme sırasında [kapsam doğrulaması](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="034a5-144">`ConfigureWebHostDefaults` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="034a5-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="034a5-145">"ASPNETCORE_" önekli ortam değişkenlerinden ana bilgisayar yapılandırmasını yükler.</span><span class="sxs-lookup"><span data-stu-id="034a5-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="034a5-146">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu Web sunucusu olarak ayarlar ve uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="034a5-147">Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="034a5-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="034a5-148">[Ana bilgisayar filtreleme ara yazılımı](xref:fundamentals/servers/kestrel#host-filtering)ekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="034a5-149">ASPNETCORE_FORWARDEDHEADERS_ENABLED = true ise [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) ekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="034a5-150">IIS tümleştirmesini etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-150">Enables IIS integration.</span></span> <span data-ttu-id="034a5-151">IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="034a5-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="034a5-152">Bu makalenin ilerleyen kısımlarında [Web Apps bölümlerine yönelik](#settings-for-web-apps) [tüm uygulama türleri](#settings-for-all-app-types) ve ayarlarının ayarları, varsayılan Oluşturucu ayarlarının nasıl geçersiz kılınacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="034a5-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="034a5-153">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="034a5-153">Framework-provided services</span></span>

<span data-ttu-id="034a5-154">Kayıtlı hizmetler otomatik olarak şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="034a5-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="034a5-155">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="034a5-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="034a5-156">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="034a5-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="034a5-157">Ihostenvironment/ıwebhostenvironment</span><span class="sxs-lookup"><span data-stu-id="034a5-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="034a5-158">Framework tarafından sunulan hizmetler hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="034a5-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="034a5-159">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="034a5-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="034a5-160">Başlatma sonrası ve düzgün kapanma görevlerini işlemek için <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmeti herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="034a5-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="034a5-161">Arabirimdeki üç özellik, uygulama başlatma ve uygulama durdurma olay işleyicisi yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="034a5-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="034a5-162">Arabirim Ayrıca bir `StopApplication` yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="034a5-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="034a5-163">Aşağıdaki örnek, `IHostApplicationLifetime` olaylarını kaydeden bir `IHostedService` uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="034a5-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="034a5-164">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="034a5-164">IHostLifetime</span></span>

<span data-ttu-id="034a5-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> uygulama, ana bilgisayar başladığında ve durdurulduğunda kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="034a5-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="034a5-166">Kaydedilen son uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-166">The last implementation registered is used.</span></span>

<span data-ttu-id="034a5-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` varsayılan `IHostLifetime` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="034a5-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="034a5-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="034a5-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="034a5-169">CTRL + C/SIGINT veya SIGDÖNEM için dinler ve <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, başlatma işlemini başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-169">Listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="034a5-170">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="034a5-171">Ihostenvironment</span><span class="sxs-lookup"><span data-stu-id="034a5-171">IHostEnvironment</span></span>

<span data-ttu-id="034a5-172"><xref:Microsoft.Extensions.Hosting.IHostEnvironment> hizmetini bir sınıfa ekleyin ve aşağıdakiler hakkında bilgi alın:</span><span class="sxs-lookup"><span data-stu-id="034a5-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="034a5-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="034a5-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="034a5-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="034a5-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="034a5-175">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="034a5-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="034a5-176">Web uygulamaları, `IHostEnvironment` devralan ve [WebRootPath](#webroot)ekleyen `IWebHostEnvironment` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="034a5-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="034a5-177">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="034a5-177">Host configuration</span></span>

<span data-ttu-id="034a5-178">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulamasının özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="034a5-179">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>içinde [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="034a5-180">`ConfigureAppConfiguration`sonra, `HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="034a5-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="034a5-181">Konak yapılandırması eklemek için `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="034a5-182">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="034a5-183">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="034a5-184">Ön ek `DOTNET_` ve komut satırı bağımsız değişkenlerine sahip ortam değişkeni sağlayıcısı, CreateDefaultBuilder tarafından eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-184">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="034a5-185">Web Apps için `ASPNETCORE_` ön ekine sahip ortam değişkeni sağlayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="034a5-186">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="034a5-187">Örneğin, `ASPNETCORE_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri haline gelir.</span><span class="sxs-lookup"><span data-stu-id="034a5-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="034a5-188">Aşağıdaki örnek ana bilgisayar yapılandırması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="034a5-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="034a5-189">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="034a5-189">App configuration</span></span>

<span data-ttu-id="034a5-190">Uygulama yapılandırması, `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="034a5-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="034a5-191">`ConfigureAppConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="034a5-192">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="034a5-193">`ConfigureAppConfiguration` tarafından oluşturulan yapılandırma, sonraki işlemler ve DI hizmeti olarak, [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) konumunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="034a5-194">Konak yapılandırması, uygulama yapılandırmasına de eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="034a5-195">Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index#configureappconfiguration)konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="034a5-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="034a5-196">Tüm uygulama türleri için ayarlar</span><span class="sxs-lookup"><span data-stu-id="034a5-196">Settings for all app types</span></span>

<span data-ttu-id="034a5-197">Bu bölüm, hem HTTP hem de HTTP olmayan iş yükleri için uygulanan konak ayarlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="034a5-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="034a5-198">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="034a5-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="034a5-199">ApplicationName</span></span>

<span data-ttu-id="034a5-200">[Ihostenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="034a5-201">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="034a5-201">**Key**: applicationName</span></span>  
<span data-ttu-id="034a5-202">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-202">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-203">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="034a5-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="034a5-204">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="034a5-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="034a5-205">Bu değeri ayarlamak için ortam değişkenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="034a5-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="034a5-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="034a5-206">ContentRootPath</span></span>

<span data-ttu-id="034a5-207">[Ihostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özelliği, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="034a5-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="034a5-208">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="034a5-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="034a5-209">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="034a5-209">**Key**: contentRoot</span></span>  
<span data-ttu-id="034a5-210">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-210">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-211">**Varsayılan**: uygulama derlemesinin bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="034a5-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="034a5-212">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="034a5-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="034a5-213">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseContentRoot` çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="034a5-214">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="034a5-214">For more information, see:</span></span>

* [<span data-ttu-id="034a5-215">Temel bilgiler: Içerik kökü</span><span class="sxs-lookup"><span data-stu-id="034a5-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="034a5-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="034a5-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="034a5-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="034a5-217">EnvironmentName</span></span>

<span data-ttu-id="034a5-218">[Ihostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="034a5-219">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="034a5-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="034a5-220">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="034a5-220">Values aren't case sensitive.</span></span>

<span data-ttu-id="034a5-221">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="034a5-221">**Key**: environment</span></span>  
<span data-ttu-id="034a5-222">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-222">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-223">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="034a5-223">**Default**: Production</span></span>  
<span data-ttu-id="034a5-224">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="034a5-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="034a5-225">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseEnvironment` çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="034a5-226">shutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="034a5-226">ShutdownTimeout</span></span>

<span data-ttu-id="034a5-227">[Hostoptions. shutdowntimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>için zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="034a5-228">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="034a5-228">The default value is five seconds.</span></span>  <span data-ttu-id="034a5-229">Zaman aşımı süresi boyunca ana bilgisayar:</span><span class="sxs-lookup"><span data-stu-id="034a5-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="034a5-230">[Ihostapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping)tetikler.</span><span class="sxs-lookup"><span data-stu-id="034a5-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="034a5-231">Üzerinde durmayacak hizmetler için barındırılan Hizmetleri durdurma ve hataları günlüğe kaydetme girişimleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="034a5-232">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="034a5-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="034a5-233">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="034a5-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="034a5-234">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="034a5-235">**Anahtar**: shutdowntimeoutseconds</span><span class="sxs-lookup"><span data-stu-id="034a5-235">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="034a5-236">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="034a5-236">**Type**: *int*</span></span>  
<span data-ttu-id="034a5-237">**Varsayılan**: 5 saniye **ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="034a5-237">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="034a5-238">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `HostOptions`yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-238">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="034a5-239">Aşağıdaki örnek, zaman aşımını 20 saniye olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="034a5-239">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="034a5-240">Web Apps ayarları</span><span class="sxs-lookup"><span data-stu-id="034a5-240">Settings for web apps</span></span>

<span data-ttu-id="034a5-241">Bazı konak ayarları yalnızca HTTP iş yükleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="034a5-241">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="034a5-242">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-242">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="034a5-243">`IWebHostBuilder` genişletme yöntemleri bu ayarlar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-243">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="034a5-244">Uzantı yöntemlerinin nasıl çağrılacağını gösteren kod örnekleri, aşağıdaki örnekte olduğu gibi `webBuilder` bir `IWebHostBuilder`örneği olduğunu varsayar:</span><span class="sxs-lookup"><span data-stu-id="034a5-244">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="034a5-245">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="034a5-245">CaptureStartupErrors</span></span>

<span data-ttu-id="034a5-246">`false`, başlangıç sırasında hata durumunda çıkış sırasında hatalar oluştu.</span><span class="sxs-lookup"><span data-stu-id="034a5-246">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="034a5-247">`true`, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="034a5-247">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="034a5-248">**Anahtar**: capturestartuperrors</span><span class="sxs-lookup"><span data-stu-id="034a5-248">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="034a5-249">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="034a5-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="034a5-250">**Varsayılan**: uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="034a5-250">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="034a5-251">**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="034a5-251">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="034a5-252">Bu değeri ayarlamak için yapılandırma veya çağrı `CaptureStartupErrors`kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-252">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="034a5-253">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="034a5-253">DetailedErrors</span></span>

<span data-ttu-id="034a5-254">Etkinleştirildiğinde veya ortam `Development`olduğunda, uygulama ayrıntılı hataları yakalar.</span><span class="sxs-lookup"><span data-stu-id="034a5-254">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="034a5-255">**Anahtar**: detailederrors</span><span class="sxs-lookup"><span data-stu-id="034a5-255">**Key**: detailedErrors</span></span>  
<span data-ttu-id="034a5-256">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="034a5-256">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="034a5-257">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="034a5-257">**Default**: false</span></span>  
<span data-ttu-id="034a5-258">**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="034a5-258">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="034a5-259">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-259">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="034a5-260">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="034a5-260">HostingStartupAssemblies</span></span>

<span data-ttu-id="034a5-261">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="034a5-261">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="034a5-262">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="034a5-262">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="034a5-263">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-263">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="034a5-264">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="034a5-264">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="034a5-265">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-265">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-266">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="034a5-266">**Default**: Empty string</span></span>  
<span data-ttu-id="034a5-267">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="034a5-267">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="034a5-268">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-268">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="034a5-269">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="034a5-269">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="034a5-270">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="034a5-270">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="034a5-271">**Anahtar**: hostingstartupexcludeassemblies</span><span class="sxs-lookup"><span data-stu-id="034a5-271">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="034a5-272">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-272">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-273">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="034a5-273">**Default**: Empty string</span></span>  
<span data-ttu-id="034a5-274">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="034a5-274">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="034a5-275">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-275">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="034a5-276">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="034a5-276">HTTPS_Port</span></span>

<span data-ttu-id="034a5-277">HTTPS yeniden yönlendirme bağlantı noktası.</span><span class="sxs-lookup"><span data-stu-id="034a5-277">The HTTPS redirect port.</span></span> <span data-ttu-id="034a5-278">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-278">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="034a5-279">**Anahtar**: https_port</span><span class="sxs-lookup"><span data-stu-id="034a5-279">**Key**: https_port</span></span>  
<span data-ttu-id="034a5-280">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-280">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-281">**Varsayılan**: varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="034a5-281">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="034a5-282">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="034a5-282">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="034a5-283">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-283">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="034a5-284">Tercih Hostingurl 'Leri</span><span class="sxs-lookup"><span data-stu-id="034a5-284">PreferHostingUrls</span></span>

<span data-ttu-id="034a5-285">Konağın `IServer` uygulamayla yapılandırılanlar yerine `IWebHostBuilder` ile yapılandırılan URL 'lerde dinleme yapıp kullanmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="034a5-285">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="034a5-286">**Anahtar**: preferhostingurl 'leri</span><span class="sxs-lookup"><span data-stu-id="034a5-286">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="034a5-287">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="034a5-287">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="034a5-288">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="034a5-288">**Default**: true</span></span>  
<span data-ttu-id="034a5-289">**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="034a5-289">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="034a5-290">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `PreferHostingUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-290">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="034a5-291">Koruyucu Thostınstartup</span><span class="sxs-lookup"><span data-stu-id="034a5-291">PreventHostingStartup</span></span>

<span data-ttu-id="034a5-292">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="034a5-292">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="034a5-293">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="034a5-293">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="034a5-294">**Anahtar**: koruyucu thostingstartup</span><span class="sxs-lookup"><span data-stu-id="034a5-294">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="034a5-295">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="034a5-295">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="034a5-296">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="034a5-296">**Default**: false</span></span>  
<span data-ttu-id="034a5-297">**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="034a5-297">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="034a5-298">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseSetting` çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-298">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="034a5-299">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="034a5-299">StartupAssembly</span></span>

<span data-ttu-id="034a5-300">`Startup` sınıfını aramak için bütünleştirilmiş kod.</span><span class="sxs-lookup"><span data-stu-id="034a5-300">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="034a5-301">**Anahtar**: startupassembly</span><span class="sxs-lookup"><span data-stu-id="034a5-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="034a5-302">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-302">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-303">**Varsayılan**: uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="034a5-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="034a5-304">**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="034a5-304">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="034a5-305">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseStartup`çağırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-305">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="034a5-306">`UseStartup`, bir derleme adı (`string`) veya bir tür (`TStartup`) alabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-306">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="034a5-307">Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="034a5-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="034a5-308">URL’ler</span><span class="sxs-lookup"><span data-stu-id="034a5-308">URLs</span></span>

<span data-ttu-id="034a5-309">Sunucu istekleri için dinlemesi gereken bağlantı noktaları ve protokollerle, noktalı virgülle ayrılmış IP adresleri listesi veya ana bilgisayar adresleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-309">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="034a5-310">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="034a5-310">For example, `http://localhost:123`.</span></span> <span data-ttu-id="034a5-311">Sunucunun belirtilen bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın.</span><span class="sxs-lookup"><span data-stu-id="034a5-311">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="034a5-312">Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="034a5-312">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="034a5-313">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="034a5-313">Supported formats vary among servers.</span></span>

<span data-ttu-id="034a5-314">**Anahtar**: URL 'ler</span><span class="sxs-lookup"><span data-stu-id="034a5-314">**Key**: urls</span></span>  
<span data-ttu-id="034a5-315">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-315">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-316">**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="034a5-316">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="034a5-317">**Ortam değişkeni**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="034a5-317">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="034a5-318">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-318">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="034a5-319">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="034a5-319">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="034a5-320">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="034a5-320">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="034a5-321">WebRoot</span><span class="sxs-lookup"><span data-stu-id="034a5-321">WebRoot</span></span>

<span data-ttu-id="034a5-322">Uygulamanın statik varlıklarının göreli yolu.</span><span class="sxs-lookup"><span data-stu-id="034a5-322">The relative path to the app's static assets.</span></span>

<span data-ttu-id="034a5-323">**Anahtar**: Webroot</span><span class="sxs-lookup"><span data-stu-id="034a5-323">**Key**: webroot</span></span>  
<span data-ttu-id="034a5-324">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-324">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-325">**Varsayılan**: varsayılan `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="034a5-325">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="034a5-326">*{Content root}/Wwwroot* yolu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="034a5-326">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="034a5-327">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-327">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="034a5-328">**Ortam değişkeni**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="034a5-328">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="034a5-329">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseWebRoot`çağırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-329">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="034a5-330">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="034a5-330">For more information, see:</span></span>

* [<span data-ttu-id="034a5-331">Temel bilgiler: Web kökü</span><span class="sxs-lookup"><span data-stu-id="034a5-331">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="034a5-332">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="034a5-332">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="034a5-333">Konak ömrünü yönetme</span><span class="sxs-lookup"><span data-stu-id="034a5-333">Manage the host lifetime</span></span>

<span data-ttu-id="034a5-334">Uygulamayı başlatmak ve durdurmak için oluşturulan <xref:Microsoft.Extensions.Hosting.IHost> uygulamasındaki Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-334">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="034a5-335">Bu yöntemler, hizmet kapsayıcısında kayıtlı olan tüm <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını etkiler.</span><span class="sxs-lookup"><span data-stu-id="034a5-335">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="034a5-336">Çalıştırın</span><span class="sxs-lookup"><span data-stu-id="034a5-336">Run</span></span>

<span data-ttu-id="034a5-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="034a5-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="034a5-338">RunAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-338">RunAsync</span></span>

<span data-ttu-id="034a5-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlayan bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="034a5-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="034a5-340">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-340">RunConsoleAsync</span></span>

<span data-ttu-id="034a5-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>, konsol desteği sağlar, Konağı oluşturur ve başlatır ve CTRL + C/SIGINT ya da SIGDÖNEM 'in kapatılmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="034a5-342">Başlat</span><span class="sxs-lookup"><span data-stu-id="034a5-342">Start</span></span>

<span data-ttu-id="034a5-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="034a5-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="034a5-344">StartAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-344">StartAsync</span></span>

<span data-ttu-id="034a5-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, Konağı başlatır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="034a5-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="034a5-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, `StartAsync`başlangıcında çağrılır ve bu, devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="034a5-347">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-347">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="034a5-348">StopAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-348">StopAsync</span></span>

<span data-ttu-id="034a5-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="034a5-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="034a5-350">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="034a5-350">WaitForShutdown</span></span>

<span data-ttu-id="034a5-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, CTRL + C/SIGINT veya SIGTERM gibi ıhostlifetime tarafından kapanmadan, çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="034a5-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="034a5-352">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-352">WaitForShutdownAsync</span></span>

<span data-ttu-id="034a5-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, verilen belirteç aracılığıyla kapalı tetiklendiğinde ve <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağıran bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="034a5-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="034a5-354">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="034a5-354">External control</span></span>

<span data-ttu-id="034a5-355">Ana bilgisayar ömrünün doğrudan denetimi dışarıdan çağrılabilen yöntemler kullanılarak sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="034a5-355">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="034a5-356">ASP.NET Core uygulamalar bir konağı yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="034a5-356">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="034a5-357">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="034a5-357">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="034a5-358">Bu makalede, HTTP isteklerini işlemeyin uygulamalar için kullanılan ASP.NET Core genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.HostBuilder>) ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="034a5-358">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="034a5-359">Genel konağın amacı, daha geniş bir konak senaryolarını etkinleştirmek üzere Web ana bilgisayar API 'sinden HTTP işlem hattını ayırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-359">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="034a5-360">Yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliğinden genel ana bilgisayar avantajı temel alınarak mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="034a5-360">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="034a5-361">Genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir ve Web barındırma senaryolarında uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="034a5-361">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="034a5-362">Web barındırma senaryolarında [Web konağını](xref:fundamentals/host/web-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="034a5-362">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="034a5-363">Genel ana bilgisayar gelecek bir sürümdeki Web konağını değiştirecek ve hem HTTP hem de HTTP olmayan senaryolarda birincil ana bilgisayar API 'SI olarak görev yapacak.</span><span class="sxs-lookup"><span data-stu-id="034a5-363">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="034a5-364">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="034a5-364">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="034a5-365">Örnek uygulamayı [Visual Studio Code](https://code.visualstudio.com/)' de çalıştırırken, *dış veya tümleşik bir Terminal*kullanın.</span><span class="sxs-lookup"><span data-stu-id="034a5-365">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="034a5-366">Örneği bir `internalConsole`çalıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="034a5-366">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="034a5-367">Konsolu Visual Studio Code ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="034a5-367">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="034a5-368">*. Vscode/Launch. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="034a5-368">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="034a5-369">**.NET Core başlatma (konsol)** yapılandırmasında **konsol** girişini bulun.</span><span class="sxs-lookup"><span data-stu-id="034a5-369">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="034a5-370">Değeri `externalTerminal` ya da `integratedTerminal`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="034a5-370">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="034a5-371">Giriş</span><span class="sxs-lookup"><span data-stu-id="034a5-371">Introduction</span></span>

<span data-ttu-id="034a5-372">Genel ana bilgisayar kitaplığı <xref:Microsoft.Extensions.Hosting> ad alanında kullanılabilir ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-372">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="034a5-373">`Microsoft.Extensions.Hosting` paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e dahildir (ASP.NET Core 2,1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="034a5-373">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="034a5-374"><xref:Microsoft.Extensions.Hosting.IHostedService>, kod yürütmeye yönelik giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="034a5-374"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="034a5-375">Her bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama, [ConfigureServices 'de hizmet kaydı](#configureservices)sırasında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="034a5-375">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="034a5-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>, konak başlatıldığında her <xref:Microsoft.Extensions.Hosting.IHostedService> çağrılır ve konak düzgün şekilde kapandığında <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> ters kayıt sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="034a5-377">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="034a5-377">Set up a host</span></span>

<span data-ttu-id="034a5-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder>, kitaplıkların ve uygulamaların Konağı başlatmak, derlemek ve çalıştırmak için kullandığı ana bileşendir:</span><span class="sxs-lookup"><span data-stu-id="034a5-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="034a5-379">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="034a5-379">Options</span></span>

<span data-ttu-id="034a5-380"><xref:Microsoft.Extensions.Hosting.HostOptions> <xref:Microsoft.Extensions.Hosting.IHost>seçeneklerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-380"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="034a5-381">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="034a5-381">Shutdown timeout</span></span>

<span data-ttu-id="034a5-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>, <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="034a5-383">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="034a5-383">The default value is five seconds.</span></span>

<span data-ttu-id="034a5-384">`Program.Main` ' deki aşağıdaki seçenek yapılandırması, varsayılan beş saniyelik kapatılma zaman aşımını 20 saniyeye yükseltir:</span><span class="sxs-lookup"><span data-stu-id="034a5-384">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="034a5-385">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="034a5-385">Default services</span></span>

<span data-ttu-id="034a5-386">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="034a5-386">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="034a5-387">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="034a5-387">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="034a5-388">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="034a5-388">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="034a5-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="034a5-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="034a5-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="034a5-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="034a5-391">[Seçenekler](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="034a5-391">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="034a5-392">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="034a5-392">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="034a5-393">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="034a5-393">Host configuration</span></span>

<span data-ttu-id="034a5-394">Ana bilgisayar yapılandırması şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="034a5-394">Host configuration is created by:</span></span>

* <span data-ttu-id="034a5-395">[İçerik kökünü](#content-root) ve [ortamı](#environment)ayarlamak için <xref:Microsoft.Extensions.Hosting.IHostBuilder> uzantı yöntemleri çağrılıyor.</span><span class="sxs-lookup"><span data-stu-id="034a5-395">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="034a5-396"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>içindeki yapılandırma sağlayıcılarından yapılandırma okunuyor.</span><span class="sxs-lookup"><span data-stu-id="034a5-396">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="034a5-397">Uzantı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="034a5-397">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="034a5-398">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="034a5-398">Application key (name)</span></span>

<span data-ttu-id="034a5-399">[Ihostingenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-399">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="034a5-400">Değeri açıkça ayarlamak için, [Hostdefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-400">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="034a5-401">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="034a5-401">**Key**: applicationName</span></span>  
<span data-ttu-id="034a5-402">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-402">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-403">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="034a5-403">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="034a5-404">Şunu **kullanarak ayarla**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="034a5-404">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="034a5-405">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="034a5-405">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="034a5-406">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="034a5-406">Content root</span></span>

<span data-ttu-id="034a5-407">Bu ayar, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="034a5-407">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="034a5-408">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="034a5-408">**Key**: contentRoot</span></span>  
<span data-ttu-id="034a5-409">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-409">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-410">**Varsayılan**: uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="034a5-410">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="034a5-411">Şunu **kullanarak ayarla**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="034a5-411">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="034a5-412">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="034a5-412">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="034a5-413">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="034a5-413">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="034a5-414">Daha fazla bilgi için bkz. [temel bilgiler: içerik kökü](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="034a5-414">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="034a5-415">Ortam</span><span class="sxs-lookup"><span data-stu-id="034a5-415">Environment</span></span>

<span data-ttu-id="034a5-416">Uygulamanın [ortamını](xref:fundamentals/environments)ayarlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-416">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="034a5-417">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="034a5-417">**Key**: environment</span></span>  
<span data-ttu-id="034a5-418">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="034a5-418">**Type**: *string*</span></span>  
<span data-ttu-id="034a5-419">**Varsayılan**: üretim</span><span class="sxs-lookup"><span data-stu-id="034a5-419">**Default**: Production</span></span>  
<span data-ttu-id="034a5-420">Şunu **kullanarak ayarla**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="034a5-420">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="034a5-421">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="034a5-421">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="034a5-422">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-422">The environment can be set to any value.</span></span> <span data-ttu-id="034a5-423">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="034a5-423">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="034a5-424">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="034a5-424">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="034a5-425">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="034a5-425">ConfigureHostConfiguration</span></span>

<span data-ttu-id="034a5-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, ana bilgisayar için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="034a5-427">Konak yapılandırması, uygulamanın derleme sürecinde kullanılmak üzere <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-427">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="034a5-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="034a5-429">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-429">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="034a5-430">Varsayılan olarak hiçbir sağlayıcı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="034a5-430">No providers are included by default.</span></span> <span data-ttu-id="034a5-431">Uygulamanın <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>gereken her türlü yapılandırma sağlayıcısını açıkça belirtmeniz gerekir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="034a5-431">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="034a5-432">Dosya yapılandırması (örneğin, *HostSettings. JSON* dosyasından).</span><span class="sxs-lookup"><span data-stu-id="034a5-432">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="034a5-433">Ortam değişkeni yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="034a5-433">Environment variable configuration.</span></span>
* <span data-ttu-id="034a5-434">Komut satırı bağımsız değişken yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="034a5-434">Command-line argument configuration.</span></span>
* <span data-ttu-id="034a5-435">Diğer tüm gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="034a5-435">Any other required configuration providers.</span></span>

<span data-ttu-id="034a5-436">Konağın dosya yapılandırması, uygulamanın temel yolu `SetBasePath` ve ardından [dosya yapılandırma sağlayıcılarından](xref:fundamentals/configuration/index#file-configuration-provider)birine yapılan bir çağrı tarafından belirtilerek etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-436">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="034a5-437">Örnek uygulama bir JSON dosyası, *HostSettings. JSON*kullanır ve dosyanın konak yapılandırma ayarlarını kullanmak için <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-437">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="034a5-438">Konağın [ortam değişkeni yapılandırmasını](xref:fundamentals/configuration/index#environment-variables-configuration-provider) eklemek için konak oluşturucu üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="034a5-438">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="034a5-439">`AddEnvironmentVariables`, Kullanıcı tanımlı isteğe bağlı bir ön eki kabul eder.</span><span class="sxs-lookup"><span data-stu-id="034a5-439">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="034a5-440">Örnek uygulama `PREFIX_`önekini kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-440">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="034a5-441">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-441">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="034a5-442">Örnek uygulamanın ana bilgisayarı yapılandırıldığında, `PREFIX_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri olur.</span><span class="sxs-lookup"><span data-stu-id="034a5-442">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="034a5-443">[Visual Studio](https://visualstudio.microsoft.com) kullanırken veya `dotnet run`bir uygulama çalıştırırken geliştirme sırasında, *Özellikler/launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-443">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="034a5-444">[Visual Studio Code](https://code.visualstudio.com/), geliştirme sırasında *. vscode/Launch. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-444">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="034a5-445">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="034a5-445">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="034a5-446">[Komut satırı yapılandırması](xref:fundamentals/configuration/index#command-line-configuration-provider) , <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>çağırarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-446">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="034a5-447">Komut satırı yapılandırması, önceki yapılandırma sağlayıcıları tarafından belirtilen yapılandırmayı geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek üzere en son eklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-447">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="034a5-448">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="034a5-448">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="034a5-449">Ek yapılandırma [ApplicationName](#application-key-name) ve [contentroot](#content-root) anahtarlarıyla birlikte etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-449">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="034a5-450"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>kullanarak `HostBuilder` yapılandırma örneği:</span><span class="sxs-lookup"><span data-stu-id="034a5-450">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="034a5-451">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="034a5-451">ConfigureAppConfiguration</span></span>

<span data-ttu-id="034a5-452">Uygulama yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasındaki <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="034a5-452">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="034a5-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, uygulama için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="034a5-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="034a5-455">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-455">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="034a5-456"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> tarafından oluşturulan yapılandırma, sonraki işlemler ve <xref:Microsoft.Extensions.Hosting.IHost.Services*>için [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) ' da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-456">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="034a5-457">Uygulama yapılandırması, [Configurehostconfiguration](#configurehostconfiguration)tarafından belirtilen konak yapılandırmasını otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="034a5-457">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="034a5-458"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>kullanarak örnek uygulama yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="034a5-458">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="034a5-459">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="034a5-459">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="034a5-460">*appSettings. Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="034a5-460">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="034a5-461">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="034a5-461">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="034a5-462">Ayar dosyalarını çıkış dizinine taşımak için, ayarlar dosyalarını proje dosyasında [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="034a5-462">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="034a5-463">Örnek uygulama, JSON uygulama ayarları dosyalarını ve *HostSettings. JSON* dosyasını şu `<Content>` öğesiyle taşıdıkça:</span><span class="sxs-lookup"><span data-stu-id="034a5-463">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="034a5-464"><xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> gibi yapılandırma uzantısı yöntemleri, [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables)gibi ek NuGet paketleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="034a5-464">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="034a5-465">Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'i kullanmadığı takdirde, bu paketlerin temel [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paketine ek olarak projeye eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="034a5-465">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="034a5-466">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="034a5-466">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="034a5-467">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="034a5-467">ConfigureServices</span></span>

<span data-ttu-id="034a5-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, uygulamanın [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="034a5-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="034a5-470">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="034a5-470">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="034a5-471">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="034a5-471">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="034a5-472">[Örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , yaşam süresi olayları, `LifetimeEventsHostedService`ve zaman aşımına uğramış bir arka plan görevi olan `TimedHostedService`uygulamaya bir hizmet eklemek için `AddHostedService` uzantısı yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="034a5-472">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="034a5-473">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="034a5-473">ConfigureLogging</span></span>

<span data-ttu-id="034a5-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, belirtilen <xref:Microsoft.Extensions.Logging.ILoggingBuilder>yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="034a5-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="034a5-476">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="034a5-476">UseConsoleLifetime</span></span>

<span data-ttu-id="034a5-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>, CTRL + C/SIGINT veya SIGTERIM dinler ve <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> çağırarak, bu işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="034a5-478">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*></span><span class="sxs-lookup"><span data-stu-id="034a5-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="034a5-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` varsayılan ömür uygulamasıyla önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="034a5-480">Kaydedilen son yaşam süresi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="034a5-480">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="034a5-481">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="034a5-481">Container configuration</span></span>

<span data-ttu-id="034a5-482">Diğer kapsayıcıları takmayı desteklemek için, ana bilgisayar bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-482">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="034a5-483">Fabrika sağlamak, dı kapsayıcı kaydının bir parçası değildir, ancak bunun yerine somut dı kapsayıcısını oluşturmak için kullanılan bir konak iç sıdır.</span><span class="sxs-lookup"><span data-stu-id="034a5-483">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="034a5-484">[Useserviceproviderfactory (ıvıceproviderfactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) , uygulamanın hizmet sağlayıcısını oluşturmak için kullanılan varsayılan fabrikası geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="034a5-484">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="034a5-485">Özel kapsayıcı yapılandırması <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi tarafından yönetilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-485">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="034a5-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, temel ana bilgisayar API 'sinin üstünde kapsayıcıyı yapılandırmak için kesin olarak belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="034a5-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="034a5-488">Uygulama için bir hizmet kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="034a5-488">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="034a5-489">Hizmet kapsayıcısı fabrikası sağlama:</span><span class="sxs-lookup"><span data-stu-id="034a5-489">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="034a5-490">Fabrika 'yi kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="034a5-490">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="034a5-491">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="034a5-491">Extensibility</span></span>

<span data-ttu-id="034a5-492">Konak genişletilebilirliği <xref:Microsoft.Extensions.Hosting.IHostBuilder>uzantı yöntemleriyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-492">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="034a5-493">Aşağıdaki örnek, bir genişletme yönteminin <xref:fundamentals/host/hosted-services>gösterilen [Timedhostedservice](xref:fundamentals/host/hosted-services#timed-background-tasks) örneği ile bir <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasını nasıl genişlettiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="034a5-493">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="034a5-494">Uygulama, `T`geçirilen barındırılan hizmeti kaydetmek için `UseHostedService` uzantısı yöntemini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="034a5-494">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="034a5-495">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="034a5-495">Manage the host</span></span>

<span data-ttu-id="034a5-496"><xref:Microsoft.Extensions.Hosting.IHost> uygulaması, hizmet kapsayıcısında kayıtlı <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını başlatma ve durdurma sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="034a5-496">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="034a5-497">Çalıştırın</span><span class="sxs-lookup"><span data-stu-id="034a5-497">Run</span></span>

<span data-ttu-id="034a5-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="034a5-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="034a5-499">RunAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-499">RunAsync</span></span>

<span data-ttu-id="034a5-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlayan bir <xref:System.Threading.Tasks.Task> döndürür:</span><span class="sxs-lookup"><span data-stu-id="034a5-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="034a5-501">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-501">RunConsoleAsync</span></span>

<span data-ttu-id="034a5-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>, konsol desteği sağlar, Konağı oluşturur ve başlatır ve CTRL + C/SIGINT ya da SIGDÖNEM 'in kapatılmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="034a5-503">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-503">Start and StopAsync</span></span>

<span data-ttu-id="034a5-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="034a5-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="034a5-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="034a5-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="034a5-506">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-506">StartAsync and StopAsync</span></span>

<span data-ttu-id="034a5-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="034a5-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="034a5-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="034a5-509">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="034a5-509">WaitForShutdown</span></span>

<span data-ttu-id="034a5-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (CTRL + C/SIGINT veya SIGTERIM dinler) gibi <xref:Microsoft.Extensions.Hosting.IHostLifetime>aracılığıyla tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="034a5-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="034a5-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="034a5-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="034a5-512">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="034a5-512">WaitForShutdownAsync</span></span>

<span data-ttu-id="034a5-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, verilen belirteç aracılığıyla kapalı tetiklendiğinde ve <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağıran bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="034a5-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="034a5-514">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="034a5-514">External control</span></span>

<span data-ttu-id="034a5-515">Ana bilgisayarın dış denetimine, dışarıdan çağrılabilen yöntemler kullanılarak ulaşılabilecek:</span><span class="sxs-lookup"><span data-stu-id="034a5-515">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="034a5-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>başlangıcında çağrılır ve bu, devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="034a5-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="034a5-517">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-517">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="034a5-518">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="034a5-518">IHostingEnvironment interface</span></span>

<span data-ttu-id="034a5-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="034a5-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="034a5-520">Özelliklerini ve uzantı yöntemlerini kullanmak için <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> almak üzere [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:</span><span class="sxs-lookup"><span data-stu-id="034a5-520">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="034a5-521">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="034a5-521">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="034a5-522">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="034a5-522">IApplicationLifetime interface</span></span>

<span data-ttu-id="034a5-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime>, düzgün kapanma istekleri dahil olmak üzere başlatma sonrası ve kapanma etkinliklerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="034a5-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="034a5-524">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan <xref:System.Action> yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="034a5-524">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="034a5-525">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="034a5-525">Cancellation Token</span></span> | <span data-ttu-id="034a5-526">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="034a5-526">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="034a5-527">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="034a5-527">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="034a5-528">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="034a5-528">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="034a5-529">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="034a5-529">All requests should be processed.</span></span> <span data-ttu-id="034a5-530">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="034a5-530">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="034a5-531">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="034a5-531">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="034a5-532">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="034a5-532">Requests may still be processing.</span></span> <span data-ttu-id="034a5-533">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="034a5-533">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="034a5-534">Constructor-<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> hizmetini herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="034a5-534">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="034a5-535">[Örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , olayları kaydetmek için bir `LifetimeEventsHostedService` sınıfına (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulaması) Oluşturucu ekleme işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="034a5-535">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="034a5-536">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="034a5-536">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="034a5-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="034a5-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="034a5-538">Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="034a5-538">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="034a5-539">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="034a5-539">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
