---
title: .NET genel ana bilgisayar
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu .NET Core genel ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0f8f03dabf65f2cbfe4c41d36b02a25d7902cefb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219226"
---
# <a name="net-generic-host"></a><span data-ttu-id="aaafe-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="aaafe-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="aaafe-104">Bu makalede .NET Core genel ana bilgisayarı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) tanıtılmakta ve nasıl kullanılacağına ilişkin yönergeler sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="aaafe-105">Ana bilgisayar nedir?</span><span class="sxs-lookup"><span data-stu-id="aaafe-105">What's a host?</span></span>

<span data-ttu-id="aaafe-106">*Ana bilgisayar* , bir uygulamanın kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="aaafe-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="aaafe-107">Bağımlılık ekleme (dı)</span><span class="sxs-lookup"><span data-stu-id="aaafe-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="aaafe-108">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="aaafe-108">Logging</span></span>
* <span data-ttu-id="aaafe-109">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="aaafe-109">Configuration</span></span>
* <span data-ttu-id="aaafe-110">`IHostedService` uygulamalar</span><span class="sxs-lookup"><span data-stu-id="aaafe-110">`IHostedService` implementations</span></span>

<span data-ttu-id="aaafe-111">Bir konak başlatıldığında, DI kapsayıcısında bulduğu <xref:Microsoft.Extensions.Hosting.IHostedService> her bir uygulamada `IHostedService.StartAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="aaafe-112">Bir Web uygulamasında, `IHostedService` uygulamalarından biri, [http sunucu uygulaması](xref:fundamentals/index#servers)Başlatan bir Web hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="aaafe-113">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="aaafe-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="aaafe-114">3,0 ' den önceki ASP.NET Core sürümlerinde, [Web ana BILGISAYARı](xref:fundamentals/host/web-host) http iş yükleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="aaafe-115">Web ana bilgisayarı artık Web uygulamaları için önerilmez ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="aaafe-116">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="aaafe-116">Set up a host</span></span>

<span data-ttu-id="aaafe-117">Konak genellikle `Program` sınıfındaki kodla yapılandırılır, oluşturulur ve çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="aaafe-118">`Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-118">The `Main` method:</span></span>

* <span data-ttu-id="aaafe-119">Bir Oluşturucu nesnesi oluşturmak ve yapılandırmak için bir `CreateHostBuilder` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="aaafe-120">Oluşturucu nesnesinde `Build` ve `Run` yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="aaafe-121">İşte, tek bir `IHostedService` uygulama olarak dı kapsayıcısına eklenen HTTP olmayan bir iş yükü için *program.cs* kodu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="aaafe-122">Bir HTTP iş yükü için `Main` yöntemi aynıdır ancak `CreateHostBuilder` `ConfigureWebHostDefaults`çağırır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="aaafe-123">Uygulama Entity Framework Core kullanıyorsa `CreateHostBuilder` yönteminin adını veya imzasını değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="aaafe-124">[Entity Framework Core araçları](/ef/core/miscellaneous/cli/) , uygulamayı çalıştırmadan Konağı yapılandıran bir `CreateHostBuilder` yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="aaafe-125">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="aaafe-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="aaafe-126">Varsayılan Oluşturucu ayarları</span><span class="sxs-lookup"><span data-stu-id="aaafe-126">Default builder settings</span></span>

<span data-ttu-id="aaafe-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="aaafe-128">[İçerik kökünü](xref:fundamentals/index#content-root) <xref:System.IO.Directory.GetCurrentDirectory*>tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="aaafe-129">Ana bilgisayar yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="aaafe-130">`DOTNET_`ön eki olan ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-130">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="aaafe-131">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-131">Command-line arguments.</span></span>
* <span data-ttu-id="aaafe-132">Uygulama yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="aaafe-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="aaafe-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="aaafe-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="aaafe-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="aaafe-135">Uygulama `Development` ortamda çalıştığında [gizli dizi Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="aaafe-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="aaafe-136">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-136">Environment variables.</span></span>
  * <span data-ttu-id="aaafe-137">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-137">Command-line arguments.</span></span>
* <span data-ttu-id="aaafe-138">Aşağıdaki [günlük](xref:fundamentals/logging/index) sağlayıcılarını ekler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="aaafe-139">Konsol</span><span class="sxs-lookup"><span data-stu-id="aaafe-139">Console</span></span>
  * <span data-ttu-id="aaafe-140">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="aaafe-140">Debug</span></span>
  * <span data-ttu-id="aaafe-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="aaafe-141">EventSource</span></span>
  * <span data-ttu-id="aaafe-142">Olay günlüğü (yalnızca Windows üzerinde çalışırken)</span><span class="sxs-lookup"><span data-stu-id="aaafe-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="aaafe-143">Ortam geliştirme sırasında [kapsam doğrulaması](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="aaafe-144">`ConfigureWebHostDefaults` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="aaafe-145">`ASPNETCORE_`önekli ortam değişkenlerinden ana bilgisayar yapılandırmasını yükler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-145">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="aaafe-146">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu Web sunucusu olarak ayarlar ve uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="aaafe-147">Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="aaafe-148">[Ana bilgisayar filtreleme ara yazılımı](xref:fundamentals/servers/kestrel#host-filtering)ekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="aaafe-149">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` eşitse, [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) ekler `true`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="aaafe-150">IIS tümleştirmesini etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-150">Enables IIS integration.</span></span> <span data-ttu-id="aaafe-151">IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="aaafe-152">Bu makalenin ilerleyen kısımlarında [Web Apps bölümlerine yönelik](#settings-for-web-apps) [tüm uygulama türleri](#settings-for-all-app-types) ve ayarlarının ayarları, varsayılan Oluşturucu ayarlarının nasıl geçersiz kılınacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="aaafe-153">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="aaafe-153">Framework-provided services</span></span>

<span data-ttu-id="aaafe-154">Aşağıdaki hizmetler otomatik olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-154">The following services are registered automatically:</span></span>

* [<span data-ttu-id="aaafe-155">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="aaafe-156">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="aaafe-157">Ihostenvironment/ıwebhostenvironment</span><span class="sxs-lookup"><span data-stu-id="aaafe-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="aaafe-158">Framework tarafından sunulan hizmetler hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="aaafe-159">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="aaafe-160">Başlatma sonrası ve düzgün kapanma görevlerini işlemek için <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmeti herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="aaafe-161">Arabirimdeki üç özellik, uygulama başlatma ve uygulama durdurma olay işleyicisi yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="aaafe-162">Arabirim Ayrıca bir `StopApplication` yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="aaafe-163">Aşağıdaki örnek, `IHostApplicationLifetime` olaylarını kaydeden bir `IHostedService` uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="aaafe-164">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-164">IHostLifetime</span></span>

<span data-ttu-id="aaafe-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> uygulama, ana bilgisayar başladığında ve durdurulduğunda kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="aaafe-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="aaafe-166">Kaydedilen son uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-166">The last implementation registered is used.</span></span>

<span data-ttu-id="aaafe-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` varsayılan `IHostLifetime` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="aaafe-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="aaafe-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="aaafe-169"><kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterim dinler ve <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> çağırarak, bu işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-169">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="aaafe-170">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="aaafe-171">Ihostenvironment</span><span class="sxs-lookup"><span data-stu-id="aaafe-171">IHostEnvironment</span></span>

<span data-ttu-id="aaafe-172">Aşağıdaki ayarlarla ilgili bilgi almak için <xref:Microsoft.Extensions.Hosting.IHostEnvironment> hizmetini bir sınıfa ekleyin:</span><span class="sxs-lookup"><span data-stu-id="aaafe-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="aaafe-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="aaafe-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="aaafe-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="aaafe-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="aaafe-175">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="aaafe-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="aaafe-176">Web uygulamaları, `IHostEnvironment` devralan ve [WebRootPath](#webroot)ekleyen `IWebHostEnvironment` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="aaafe-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="aaafe-177">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-177">Host configuration</span></span>

<span data-ttu-id="aaafe-178">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulamasının özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="aaafe-179">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>içinde [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="aaafe-180">`ConfigureAppConfiguration`sonra, `HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="aaafe-181">Konak yapılandırması eklemek için `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="aaafe-182">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-183">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="aaafe-184">Ön ek `DOTNET_` ve komut satırı bağımsız değişkenlerine sahip ortam değişkeni sağlayıcısı, `CreateDefaultBuilder`tarafından dahildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-184">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="aaafe-185">Web Apps için `ASPNETCORE_` ön ekine sahip ortam değişkeni sağlayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="aaafe-186">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="aaafe-187">Örneğin, `ASPNETCORE_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri haline gelir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="aaafe-188">Aşağıdaki örnek ana bilgisayar yapılandırması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="aaafe-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="aaafe-189">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-189">App configuration</span></span>

<span data-ttu-id="aaafe-190">Uygulama yapılandırması, `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="aaafe-191">`ConfigureAppConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-192">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="aaafe-193">`ConfigureAppConfiguration` tarafından oluşturulan yapılandırma, sonraki işlemler ve DI hizmeti olarak, [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) konumunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="aaafe-194">Konak yapılandırması, uygulama yapılandırmasına de eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="aaafe-195">Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index#configureappconfiguration)konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="aaafe-196">Tüm uygulama türleri için ayarlar</span><span class="sxs-lookup"><span data-stu-id="aaafe-196">Settings for all app types</span></span>

<span data-ttu-id="aaafe-197">Bu bölüm, hem HTTP hem de HTTP olmayan iş yükleri için uygulanan konak ayarlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="aaafe-198">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="aaafe-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="aaafe-199">ApplicationName</span></span>

<span data-ttu-id="aaafe-200">[Ihostenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="aaafe-201">**Anahtar**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="aaafe-201">**Key**: `applicationName`</span></span>  
<span data-ttu-id="aaafe-202">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-202">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-203">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="aaafe-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="aaafe-204">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="aaafe-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="aaafe-205">Bu değeri ayarlamak için ortam değişkenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="aaafe-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="aaafe-206">ContentRootPath</span></span>

<span data-ttu-id="aaafe-207">[Ihostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özelliği, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="aaafe-208">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="aaafe-209">**Anahtar**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-209">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="aaafe-210">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-210">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-211">**Varsayılan**: uygulama derlemesinin bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="aaafe-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="aaafe-212">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="aaafe-213">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseContentRoot` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="aaafe-214">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-214">For more information, see:</span></span>

* [<span data-ttu-id="aaafe-215">Temel bilgiler: Içerik kökü</span><span class="sxs-lookup"><span data-stu-id="aaafe-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="aaafe-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="aaafe-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="aaafe-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="aaafe-217">EnvironmentName</span></span>

<span data-ttu-id="aaafe-218">[Ihostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="aaafe-219">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="aaafe-220">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-220">Values aren't case-sensitive.</span></span>

<span data-ttu-id="aaafe-221">**Anahtar**: `environment`</span><span class="sxs-lookup"><span data-stu-id="aaafe-221">**Key**: `environment`</span></span>  
<span data-ttu-id="aaafe-222">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-222">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-223">**Varsayılan**: `Production`</span><span class="sxs-lookup"><span data-stu-id="aaafe-223">**Default**: `Production`</span></span>  
<span data-ttu-id="aaafe-224">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="aaafe-225">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseEnvironment` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="aaafe-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="aaafe-226">ShutdownTimeout</span></span>

<span data-ttu-id="aaafe-227">[Hostoptions. shutdowntimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>için zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="aaafe-228">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-228">The default value is five seconds.</span></span>  <span data-ttu-id="aaafe-229">Zaman aşımı süresi boyunca ana bilgisayar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="aaafe-230">[Ihostapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping)tetikler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="aaafe-231">Üzerinde durmayacak hizmetler için barındırılan Hizmetleri durdurma ve hataları günlüğe kaydetme girişimleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="aaafe-232">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="aaafe-233">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="aaafe-234">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="aaafe-235">**Anahtar**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="aaafe-235">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="aaafe-236">**Tür**: `int`</span><span class="sxs-lookup"><span data-stu-id="aaafe-236">**Type**: `int`</span></span>  
<span data-ttu-id="aaafe-237">**Varsayılan**: 5 saniye</span><span class="sxs-lookup"><span data-stu-id="aaafe-237">**Default**: 5 seconds</span></span>  
<span data-ttu-id="aaafe-238">**Ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-238">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="aaafe-239">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `HostOptions`yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="aaafe-240">Aşağıdaki örnek, zaman aşımını 20 saniye olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a><span data-ttu-id="aaafe-241">Değişiklik sırasında uygulama yapılandırması yeniden yüklemeyi devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="aaafe-241">Disable app configuration reload on change</span></span>

<span data-ttu-id="aaafe-242">[Varsayılan](xref:fundamentals/configuration/index#default)olarak, *appSettings. JSON* ve *appSettings. { Ortam}. JSON* , dosya değiştiğinde yeniden yüklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-242">By [default](xref:fundamentals/configuration/index#default), *appsettings.json* and *appsettings.{Environment}.json* are reloaded when the file changes.</span></span> <span data-ttu-id="aaafe-243">ASP.NET Core 5,0 Preview 3 veya sonraki bir sürümde bu yeniden yükleme davranışını devre dışı bırakmak için `hostBuilder:reloadConfigOnChange` anahtarını `false`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-243">To disable this reload behavior in ASP.NET Core 5.0 Preview 3 or later, set the `hostBuilder:reloadConfigOnChange` key to `false`.</span></span>

<span data-ttu-id="aaafe-244">**Anahtar**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="aaafe-244">**Key**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="aaafe-245">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-245">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-246">**Varsayılan**: `true`</span><span class="sxs-lookup"><span data-stu-id="aaafe-246">**Default**: `true`</span></span>  
<span data-ttu-id="aaafe-247">**Komut satırı bağımsız değişkeni**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="aaafe-247">**Command-line argument**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="aaafe-248">**Ortam değişkeni**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="aaafe-248">**Environment variable**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span></span>

> [!WARNING]
> <span data-ttu-id="aaafe-249">İki nokta üst üste (`:`) ayırıcı, tüm platformlarda ortam değişkeni hiyerarşik anahtarlarla birlikte çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-249">The colon (`:`) separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="aaafe-250">Daha fazla bilgi için bkz. [ortam değişkenleri](xref:fundamentals/configuration/index#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="aaafe-250">For more information, see [Environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span>

## <a name="settings-for-web-apps"></a><span data-ttu-id="aaafe-251">Web Apps ayarları</span><span class="sxs-lookup"><span data-stu-id="aaafe-251">Settings for web apps</span></span>

<span data-ttu-id="aaafe-252">Bazı konak ayarları yalnızca HTTP iş yükleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-252">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="aaafe-253">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-253">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="aaafe-254">`IWebHostBuilder` genişletme yöntemleri bu ayarlar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-254">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="aaafe-255">Uzantı yöntemlerinin nasıl çağrılacağını gösteren kod örnekleri, aşağıdaki örnekte olduğu gibi `webBuilder` bir `IWebHostBuilder`örneği olduğunu varsayar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-255">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="aaafe-256">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="aaafe-256">CaptureStartupErrors</span></span>

<span data-ttu-id="aaafe-257">`false`, başlangıç sırasında hata durumunda çıkış sırasında hatalar oluştu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-257">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="aaafe-258">`true`, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-258">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="aaafe-259">**Anahtar**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="aaafe-259">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="aaafe-260">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-260">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-261">**Varsayılan**: uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-261">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="aaafe-262">**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-262">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="aaafe-263">Bu değeri ayarlamak için yapılandırma veya çağrı `CaptureStartupErrors`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-263">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="aaafe-264">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="aaafe-264">DetailedErrors</span></span>

<span data-ttu-id="aaafe-265">Etkinleştirildiğinde veya ortam `Development`olduğunda, uygulama ayrıntılı hataları yakalar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-265">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="aaafe-266">**Anahtar**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="aaafe-266">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="aaafe-267">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-267">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-268">**Varsayılan**: `false`</span><span class="sxs-lookup"><span data-stu-id="aaafe-268">**Default**: `false`</span></span>  
<span data-ttu-id="aaafe-269">**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-269">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="aaafe-270">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-270">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="aaafe-271">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="aaafe-271">HostingStartupAssemblies</span></span>

<span data-ttu-id="aaafe-272">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="aaafe-272">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="aaafe-273">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-273">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="aaafe-274">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-274">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="aaafe-275">**Anahtar**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="aaafe-275">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="aaafe-276">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-276">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-277">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="aaafe-277">**Default**: Empty string</span></span>  
<span data-ttu-id="aaafe-278">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="aaafe-278">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="aaafe-279">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="aaafe-280">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="aaafe-280">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="aaafe-281">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="aaafe-281">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="aaafe-282">**Anahtar**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="aaafe-282">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="aaafe-283">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-283">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-284">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="aaafe-284">**Default**: Empty string</span></span>  
<span data-ttu-id="aaafe-285">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="aaafe-285">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="aaafe-286">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-286">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="aaafe-287">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="aaafe-287">HTTPS_Port</span></span>

<span data-ttu-id="aaafe-288">HTTPS yeniden yönlendirme bağlantı noktası.</span><span class="sxs-lookup"><span data-stu-id="aaafe-288">The HTTPS redirect port.</span></span> <span data-ttu-id="aaafe-289">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-289">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="aaafe-290">**Anahtar**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="aaafe-290">**Key**: `https_port`</span></span>  
<span data-ttu-id="aaafe-291">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-291">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-292">**Varsayılan**: varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-292">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="aaafe-293">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-293">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="aaafe-294">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-294">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="aaafe-295">Tercih Hostingurl 'Leri</span><span class="sxs-lookup"><span data-stu-id="aaafe-295">PreferHostingUrls</span></span>

<span data-ttu-id="aaafe-296">Konağın, `IServer` uygulamayla yapılandırılmış URL 'Ler yerine `IWebHostBuilder` ile yapılandırılan URL 'Leri dinlemesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-296">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="aaafe-297">**Anahtar**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="aaafe-297">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="aaafe-298">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-298">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-299">**Varsayılan**: `true`</span><span class="sxs-lookup"><span data-stu-id="aaafe-299">**Default**: `true`</span></span>  
<span data-ttu-id="aaafe-300">**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-300">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="aaafe-301">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `PreferHostingUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-301">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="aaafe-302">Koruyucu Thostınstartup</span><span class="sxs-lookup"><span data-stu-id="aaafe-302">PreventHostingStartup</span></span>

<span data-ttu-id="aaafe-303">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-303">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="aaafe-304">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-304">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="aaafe-305">**Anahtar**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="aaafe-305">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="aaafe-306">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-306">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-307">**Varsayılan**: `false`</span><span class="sxs-lookup"><span data-stu-id="aaafe-307">**Default**: `false`</span></span>  
<span data-ttu-id="aaafe-308">**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="aaafe-308">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="aaafe-309">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseSetting` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-309">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="aaafe-310">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="aaafe-310">StartupAssembly</span></span>

<span data-ttu-id="aaafe-311">`Startup` sınıfını aramak için bütünleştirilmiş kod.</span><span class="sxs-lookup"><span data-stu-id="aaafe-311">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="aaafe-312">**Anahtar**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="aaafe-312">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="aaafe-313">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-313">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-314">**Varsayılan**: uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="aaafe-314">**Default**: The app's assembly</span></span>  
<span data-ttu-id="aaafe-315">**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="aaafe-315">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="aaafe-316">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseStartup`çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-316">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="aaafe-317">`UseStartup`, bir derleme adı (`string`) veya bir tür (`TStartup`) alabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-317">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="aaafe-318">Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-318">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="aaafe-319">URL’ler</span><span class="sxs-lookup"><span data-stu-id="aaafe-319">URLs</span></span>

<span data-ttu-id="aaafe-320">Sunucu istekleri için dinlemesi gereken bağlantı noktaları ve protokollerle, noktalı virgülle ayrılmış IP adresleri listesi veya ana bilgisayar adresleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-320">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="aaafe-321">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-321">For example, `http://localhost:123`.</span></span> <span data-ttu-id="aaafe-322">Sunucunun belirtilen bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-322">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="aaafe-323">Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-323">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="aaafe-324">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-324">Supported formats vary among servers.</span></span>

<span data-ttu-id="aaafe-325">**Anahtar**: `urls`</span><span class="sxs-lookup"><span data-stu-id="aaafe-325">**Key**: `urls`</span></span>  
<span data-ttu-id="aaafe-326">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-326">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-327">**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="aaafe-327">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="aaafe-328">**Ortam değişkeni**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-328">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="aaafe-329">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-329">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="aaafe-330">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-330">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="aaafe-331">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-331">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="aaafe-332">WebRoot</span><span class="sxs-lookup"><span data-stu-id="aaafe-332">WebRoot</span></span>

<span data-ttu-id="aaafe-333">Uygulamanın statik varlıklarının göreli yolu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-333">The relative path to the app's static assets.</span></span>

<span data-ttu-id="aaafe-334">**Anahtar**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-334">**Key**: `webroot`</span></span>  
<span data-ttu-id="aaafe-335">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-335">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-336">**Varsayılan**: varsayılan `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-336">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="aaafe-337">*{Content root}/Wwwroot* yolu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-337">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="aaafe-338">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-338">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="aaafe-339">**Ortam değişkeni**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-339">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="aaafe-340">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseWebRoot`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-340">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="aaafe-341">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-341">For more information, see:</span></span>

* [<span data-ttu-id="aaafe-342">Temel bilgiler: Web kökü</span><span class="sxs-lookup"><span data-stu-id="aaafe-342">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="aaafe-343">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="aaafe-343">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="aaafe-344">Konak ömrünü yönetme</span><span class="sxs-lookup"><span data-stu-id="aaafe-344">Manage the host lifetime</span></span>

<span data-ttu-id="aaafe-345">Uygulamayı başlatmak ve durdurmak için oluşturulan <xref:Microsoft.Extensions.Hosting.IHost> uygulamasındaki Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-345">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="aaafe-346">Bu yöntemler, hizmet kapsayıcısında kayıtlı olan tüm <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını etkiler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-346">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="aaafe-347">Çalıştırın</span><span class="sxs-lookup"><span data-stu-id="aaafe-347">Run</span></span>

<span data-ttu-id="aaafe-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="aaafe-349">RunAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-349">RunAsync</span></span>

<span data-ttu-id="aaafe-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlayan bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="aaafe-351">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-351">RunConsoleAsync</span></span>

<span data-ttu-id="aaafe-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> konsol desteği sağlar, Konağı oluşturup başlatır ve kapatmak için <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterm bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="aaafe-353">Başlatma</span><span class="sxs-lookup"><span data-stu-id="aaafe-353">Start</span></span>

<span data-ttu-id="aaafe-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="aaafe-355">StartAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-355">StartAsync</span></span>

<span data-ttu-id="aaafe-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, Konağı başlatır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="aaafe-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, `StartAsync`başlangıcında çağrılır ve bu, devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="aaafe-358">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-358">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="aaafe-359">StopAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-359">StopAsync</span></span>

<span data-ttu-id="aaafe-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="aaafe-361">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="aaafe-361">WaitForShutdown</span></span>

<span data-ttu-id="aaafe-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, kapatmadan sonra, <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya Sigterm gibi bir ıhostlifetime tarafından tetiklenene kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="aaafe-363">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-363">WaitForShutdownAsync</span></span>

<span data-ttu-id="aaafe-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, verilen belirteç aracılığıyla kapalı tetiklendiğinde ve <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağıran bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="aaafe-365">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="aaafe-365">External control</span></span>

<span data-ttu-id="aaafe-366">Ana bilgisayar ömrünün doğrudan denetimi dışarıdan çağrılabilen yöntemler kullanılarak sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-366">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

<span data-ttu-id="aaafe-367">Bu makalede .NET Core genel ana bilgisayarı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) tanıtılmakta ve nasıl kullanılacağına ilişkin yönergeler sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-367">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="aaafe-368">Ana bilgisayar nedir?</span><span class="sxs-lookup"><span data-stu-id="aaafe-368">What's a host?</span></span>

<span data-ttu-id="aaafe-369">*Ana bilgisayar* , bir uygulamanın kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="aaafe-369">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="aaafe-370">Bağımlılık ekleme (dı)</span><span class="sxs-lookup"><span data-stu-id="aaafe-370">Dependency injection (DI)</span></span>
* <span data-ttu-id="aaafe-371">Günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="aaafe-371">Logging</span></span>
* <span data-ttu-id="aaafe-372">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="aaafe-372">Configuration</span></span>
* <span data-ttu-id="aaafe-373">`IHostedService` uygulamalar</span><span class="sxs-lookup"><span data-stu-id="aaafe-373">`IHostedService` implementations</span></span>

<span data-ttu-id="aaafe-374">Bir konak başlatıldığında, DI kapsayıcısında bulduğu <xref:Microsoft.Extensions.Hosting.IHostedService> her bir uygulamada `IHostedService.StartAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-374">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="aaafe-375">Bir Web uygulamasında, `IHostedService` uygulamalarından biri, [http sunucu uygulaması](xref:fundamentals/index#servers)Başlatan bir Web hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-375">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="aaafe-376">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="aaafe-376">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="aaafe-377">3,0 ' den önceki ASP.NET Core sürümlerinde, [Web ana BILGISAYARı](xref:fundamentals/host/web-host) http iş yükleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-377">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="aaafe-378">Web ana bilgisayarı artık Web uygulamaları için önerilmez ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-378">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="aaafe-379">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="aaafe-379">Set up a host</span></span>

<span data-ttu-id="aaafe-380">Konak genellikle `Program` sınıfındaki kodla yapılandırılır, oluşturulur ve çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-380">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="aaafe-381">`Main` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-381">The `Main` method:</span></span>

* <span data-ttu-id="aaafe-382">Bir Oluşturucu nesnesi oluşturmak ve yapılandırmak için bir `CreateHostBuilder` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-382">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="aaafe-383">Oluşturucu nesnesinde `Build` ve `Run` yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-383">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="aaafe-384">İşte, tek bir `IHostedService` uygulama olarak dı kapsayıcısına eklenen HTTP olmayan bir iş yükü için *program.cs* kodu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-384">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="aaafe-385">Bir HTTP iş yükü için `Main` yöntemi aynıdır ancak `CreateHostBuilder` `ConfigureWebHostDefaults`çağırır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-385">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="aaafe-386">Uygulama Entity Framework Core kullanıyorsa `CreateHostBuilder` yönteminin adını veya imzasını değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-386">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="aaafe-387">[Entity Framework Core araçları](/ef/core/miscellaneous/cli/) , uygulamayı çalıştırmadan Konağı yapılandıran bir `CreateHostBuilder` yöntemi bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-387">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="aaafe-388">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="aaafe-388">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="aaafe-389">Varsayılan Oluşturucu ayarları</span><span class="sxs-lookup"><span data-stu-id="aaafe-389">Default builder settings</span></span>

<span data-ttu-id="aaafe-390"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-390">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="aaafe-391">[İçerik kökünü](xref:fundamentals/index#content-root) <xref:System.IO.Directory.GetCurrentDirectory*>tarafından döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-391">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="aaafe-392">Ana bilgisayar yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-392">Loads host configuration from:</span></span>
  * <span data-ttu-id="aaafe-393">`DOTNET_`ön eki olan ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-393">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="aaafe-394">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-394">Command-line arguments.</span></span>
* <span data-ttu-id="aaafe-395">Uygulama yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-395">Loads app configuration from:</span></span>
  * <span data-ttu-id="aaafe-396">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="aaafe-396">*appsettings.json*.</span></span>
  * <span data-ttu-id="aaafe-397">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="aaafe-397">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="aaafe-398">Uygulama `Development` ortamda çalıştığında [gizli dizi Yöneticisi](xref:security/app-secrets) .</span><span class="sxs-lookup"><span data-stu-id="aaafe-398">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="aaafe-399">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-399">Environment variables.</span></span>
  * <span data-ttu-id="aaafe-400">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-400">Command-line arguments.</span></span>
* <span data-ttu-id="aaafe-401">Aşağıdaki [günlük](xref:fundamentals/logging/index) sağlayıcılarını ekler:</span><span class="sxs-lookup"><span data-stu-id="aaafe-401">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="aaafe-402">Konsol</span><span class="sxs-lookup"><span data-stu-id="aaafe-402">Console</span></span>
  * <span data-ttu-id="aaafe-403">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="aaafe-403">Debug</span></span>
  * <span data-ttu-id="aaafe-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="aaafe-404">EventSource</span></span>
  * <span data-ttu-id="aaafe-405">Olay günlüğü (yalnızca Windows üzerinde çalışırken)</span><span class="sxs-lookup"><span data-stu-id="aaafe-405">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="aaafe-406">Ortam geliştirme sırasında [kapsam doğrulaması](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-406">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="aaafe-407">`ConfigureWebHostDefaults` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="aaafe-407">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="aaafe-408">`ASPNETCORE_`önekli ortam değişkenlerinden ana bilgisayar yapılandırmasını yükler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-408">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="aaafe-409">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu Web sunucusu olarak ayarlar ve uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-409">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="aaafe-410">Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-410">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="aaafe-411">[Ana bilgisayar filtreleme ara yazılımı](xref:fundamentals/servers/kestrel#host-filtering)ekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-411">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="aaafe-412">`ASPNETCORE_FORWARDEDHEADERS_ENABLED` eşitse, [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) ekler `true`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-412">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="aaafe-413">IIS tümleştirmesini etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-413">Enables IIS integration.</span></span> <span data-ttu-id="aaafe-414">IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-414">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="aaafe-415">Bu makalenin ilerleyen kısımlarında [Web Apps bölümlerine yönelik](#settings-for-web-apps) [tüm uygulama türleri](#settings-for-all-app-types) ve ayarlarının ayarları, varsayılan Oluşturucu ayarlarının nasıl geçersiz kılınacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-415">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="aaafe-416">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="aaafe-416">Framework-provided services</span></span>

<span data-ttu-id="aaafe-417">Aşağıdaki hizmetler otomatik olarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-417">The following services are registered automatically:</span></span>

* [<span data-ttu-id="aaafe-418">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-418">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="aaafe-419">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-419">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="aaafe-420">Ihostenvironment/ıwebhostenvironment</span><span class="sxs-lookup"><span data-stu-id="aaafe-420">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="aaafe-421">Framework tarafından sunulan hizmetler hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-421">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="aaafe-422">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-422">IHostApplicationLifetime</span></span>

<span data-ttu-id="aaafe-423">Başlatma sonrası ve düzgün kapanma görevlerini işlemek için <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmeti herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-423">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="aaafe-424">Arabirimdeki üç özellik, uygulama başlatma ve uygulama durdurma olay işleyicisi yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-424">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="aaafe-425">Arabirim Ayrıca bir `StopApplication` yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-425">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="aaafe-426">Aşağıdaki örnek, `IHostApplicationLifetime` olaylarını kaydeden bir `IHostedService` uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-426">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="aaafe-427">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-427">IHostLifetime</span></span>

<span data-ttu-id="aaafe-428"><xref:Microsoft.Extensions.Hosting.IHostLifetime> uygulama, ana bilgisayar başladığında ve durdurulduğunda kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="aaafe-428">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="aaafe-429">Kaydedilen son uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-429">The last implementation registered is used.</span></span>

<span data-ttu-id="aaafe-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` varsayılan `IHostLifetime` uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="aaafe-431">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="aaafe-431">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="aaafe-432"><kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterim dinler ve <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> çağırarak, bu işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-432">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="aaafe-433">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-433">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="aaafe-434">Ihostenvironment</span><span class="sxs-lookup"><span data-stu-id="aaafe-434">IHostEnvironment</span></span>

<span data-ttu-id="aaafe-435">Aşağıdaki ayarlarla ilgili bilgi almak için <xref:Microsoft.Extensions.Hosting.IHostEnvironment> hizmetini bir sınıfa ekleyin:</span><span class="sxs-lookup"><span data-stu-id="aaafe-435">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="aaafe-436">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="aaafe-436">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="aaafe-437">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="aaafe-437">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="aaafe-438">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="aaafe-438">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="aaafe-439">Web uygulamaları, `IHostEnvironment` devralan ve [WebRootPath](#webroot)ekleyen `IWebHostEnvironment` arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="aaafe-439">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="aaafe-440">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-440">Host configuration</span></span>

<span data-ttu-id="aaafe-441">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulamasının özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-441">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="aaafe-442">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>içinde [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-442">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="aaafe-443">`ConfigureAppConfiguration`sonra, `HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-443">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="aaafe-444">Konak yapılandırması eklemek için `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-444">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="aaafe-445">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-445">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-446">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-446">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="aaafe-447">Ön ek `DOTNET_` ve komut satırı bağımsız değişkenlerine sahip ortam değişkeni sağlayıcısı, `CreateDefaultBuilder`tarafından dahildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-447">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="aaafe-448">Web Apps için `ASPNETCORE_` ön ekine sahip ortam değişkeni sağlayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-448">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="aaafe-449">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-449">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="aaafe-450">Örneğin, `ASPNETCORE_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri haline gelir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-450">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="aaafe-451">Aşağıdaki örnek ana bilgisayar yapılandırması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="aaafe-451">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="aaafe-452">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-452">App configuration</span></span>

<span data-ttu-id="aaafe-453">Uygulama yapılandırması, `IHostBuilder`<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="aaafe-454">`ConfigureAppConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-454">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-455">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-455">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="aaafe-456">`ConfigureAppConfiguration` tarafından oluşturulan yapılandırma, sonraki işlemler ve DI hizmeti olarak, [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) konumunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-456">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="aaafe-457">Konak yapılandırması, uygulama yapılandırmasına de eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-457">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="aaafe-458">Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index#configureappconfiguration)konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-458">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="aaafe-459">Tüm uygulama türleri için ayarlar</span><span class="sxs-lookup"><span data-stu-id="aaafe-459">Settings for all app types</span></span>

<span data-ttu-id="aaafe-460">Bu bölüm, hem HTTP hem de HTTP olmayan iş yükleri için uygulanan konak ayarlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-460">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="aaafe-461">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-461">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="aaafe-462">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="aaafe-462">ApplicationName</span></span>

<span data-ttu-id="aaafe-463">[Ihostenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-463">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="aaafe-464">**Anahtar**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="aaafe-464">**Key**: `applicationName`</span></span>  
<span data-ttu-id="aaafe-465">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-465">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-466">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="aaafe-466">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="aaafe-467">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="aaafe-467">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="aaafe-468">Bu değeri ayarlamak için ortam değişkenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-468">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="aaafe-469">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="aaafe-469">ContentRootPath</span></span>

<span data-ttu-id="aaafe-470">[Ihostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özelliği, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-470">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="aaafe-471">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-471">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="aaafe-472">**Anahtar**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-472">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="aaafe-473">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-473">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-474">**Varsayılan**: uygulama derlemesinin bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="aaafe-474">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="aaafe-475">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-475">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="aaafe-476">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseContentRoot` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-476">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="aaafe-477">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-477">For more information, see:</span></span>

* [<span data-ttu-id="aaafe-478">Temel bilgiler: Içerik kökü</span><span class="sxs-lookup"><span data-stu-id="aaafe-478">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="aaafe-479">WebRoot</span><span class="sxs-lookup"><span data-stu-id="aaafe-479">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="aaafe-480">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="aaafe-480">EnvironmentName</span></span>

<span data-ttu-id="aaafe-481">[Ihostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-481">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="aaafe-482">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-482">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="aaafe-483">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-483">Values aren't case-sensitive.</span></span>

<span data-ttu-id="aaafe-484">**Anahtar**: `environment`</span><span class="sxs-lookup"><span data-stu-id="aaafe-484">**Key**: `environment`</span></span>  
<span data-ttu-id="aaafe-485">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-485">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-486">**Varsayılan**: `Production`</span><span class="sxs-lookup"><span data-stu-id="aaafe-486">**Default**: `Production`</span></span>  
<span data-ttu-id="aaafe-487">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-487">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="aaafe-488">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder``UseEnvironment` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-488">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="aaafe-489">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="aaafe-489">ShutdownTimeout</span></span>

<span data-ttu-id="aaafe-490">[Hostoptions. shutdowntimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>için zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="aaafe-491">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-491">The default value is five seconds.</span></span>  <span data-ttu-id="aaafe-492">Zaman aşımı süresi boyunca ana bilgisayar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-492">During the timeout period, the host:</span></span>

* <span data-ttu-id="aaafe-493">[Ihostapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping)tetikler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-493">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="aaafe-494">Üzerinde durmayacak hizmetler için barındırılan Hizmetleri durdurma ve hataları günlüğe kaydetme girişimleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-494">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="aaafe-495">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-495">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="aaafe-496">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-496">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="aaafe-497">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-497">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="aaafe-498">**Anahtar**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="aaafe-498">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="aaafe-499">**Tür**: `int`</span><span class="sxs-lookup"><span data-stu-id="aaafe-499">**Type**: `int`</span></span>  
<span data-ttu-id="aaafe-500">**Varsayılan**: 5 saniye</span><span class="sxs-lookup"><span data-stu-id="aaafe-500">**Default**: 5 seconds</span></span>  
<span data-ttu-id="aaafe-501">**Ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-501">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="aaafe-502">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `HostOptions`yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-502">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="aaafe-503">Aşağıdaki örnek, zaman aşımını 20 saniye olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-503">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="aaafe-504">Web Apps ayarları</span><span class="sxs-lookup"><span data-stu-id="aaafe-504">Settings for web apps</span></span>

<span data-ttu-id="aaafe-505">Bazı konak ayarları yalnızca HTTP iş yükleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-505">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="aaafe-506">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-506">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="aaafe-507">`IWebHostBuilder` genişletme yöntemleri bu ayarlar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-507">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="aaafe-508">Uzantı yöntemlerinin nasıl çağrılacağını gösteren kod örnekleri, aşağıdaki örnekte olduğu gibi `webBuilder` bir `IWebHostBuilder`örneği olduğunu varsayar:</span><span class="sxs-lookup"><span data-stu-id="aaafe-508">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="aaafe-509">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="aaafe-509">CaptureStartupErrors</span></span>

<span data-ttu-id="aaafe-510">`false`, başlangıç sırasında hata durumunda çıkış sırasında hatalar oluştu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-510">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="aaafe-511">`true`, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-511">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="aaafe-512">**Anahtar**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="aaafe-512">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="aaafe-513">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-513">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-514">**Varsayılan**: uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-514">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="aaafe-515">**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-515">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="aaafe-516">Bu değeri ayarlamak için yapılandırma veya çağrı `CaptureStartupErrors`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-516">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="aaafe-517">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="aaafe-517">DetailedErrors</span></span>

<span data-ttu-id="aaafe-518">Etkinleştirildiğinde veya ortam `Development`olduğunda, uygulama ayrıntılı hataları yakalar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-518">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="aaafe-519">**Anahtar**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="aaafe-519">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="aaafe-520">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-520">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-521">**Varsayılan**: `false`</span><span class="sxs-lookup"><span data-stu-id="aaafe-521">**Default**: `false`</span></span>  
<span data-ttu-id="aaafe-522">**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-522">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="aaafe-523">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-523">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="aaafe-524">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="aaafe-524">HostingStartupAssemblies</span></span>

<span data-ttu-id="aaafe-525">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="aaafe-525">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="aaafe-526">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-526">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="aaafe-527">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-527">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="aaafe-528">**Anahtar**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="aaafe-528">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="aaafe-529">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-529">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-530">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="aaafe-530">**Default**: Empty string</span></span>  
<span data-ttu-id="aaafe-531">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="aaafe-531">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="aaafe-532">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-532">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="aaafe-533">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="aaafe-533">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="aaafe-534">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="aaafe-534">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="aaafe-535">**Anahtar**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="aaafe-535">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="aaafe-536">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-536">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-537">**Varsayılan**: boş dize</span><span class="sxs-lookup"><span data-stu-id="aaafe-537">**Default**: Empty string</span></span>  
<span data-ttu-id="aaafe-538">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="aaafe-538">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="aaafe-539">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-539">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="aaafe-540">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="aaafe-540">HTTPS_Port</span></span>

<span data-ttu-id="aaafe-541">HTTPS yeniden yönlendirme bağlantı noktası.</span><span class="sxs-lookup"><span data-stu-id="aaafe-541">The HTTPS redirect port.</span></span> <span data-ttu-id="aaafe-542">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-542">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="aaafe-543">**Anahtar**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="aaafe-543">**Key**: `https_port`</span></span>  
<span data-ttu-id="aaafe-544">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-544">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-545">**Varsayılan**: varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-545">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="aaafe-546">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-546">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="aaafe-547">Bu değeri ayarlamak için yapılandırma veya çağrı `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-547">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="aaafe-548">Tercih Hostingurl 'Leri</span><span class="sxs-lookup"><span data-stu-id="aaafe-548">PreferHostingUrls</span></span>

<span data-ttu-id="aaafe-549">Konağın, `IServer` uygulamayla yapılandırılmış URL 'Ler yerine `IWebHostBuilder` ile yapılandırılan URL 'Leri dinlemesi gerekip gerekmediğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-549">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="aaafe-550">**Anahtar**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="aaafe-550">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="aaafe-551">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-551">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-552">**Varsayılan**: `true`</span><span class="sxs-lookup"><span data-stu-id="aaafe-552">**Default**: `true`</span></span>  
<span data-ttu-id="aaafe-553">**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-553">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="aaafe-554">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `PreferHostingUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-554">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="aaafe-555">Koruyucu Thostınstartup</span><span class="sxs-lookup"><span data-stu-id="aaafe-555">PreventHostingStartup</span></span>

<span data-ttu-id="aaafe-556">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-556">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="aaafe-557">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-557">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="aaafe-558">**Anahtar**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="aaafe-558">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="aaafe-559">**Tür**: `bool` (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-559">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="aaafe-560">**Varsayılan**: `false`</span><span class="sxs-lookup"><span data-stu-id="aaafe-560">**Default**: `false`</span></span>  
<span data-ttu-id="aaafe-561">**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="aaafe-561">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="aaafe-562">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseSetting` çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-562">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="aaafe-563">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="aaafe-563">StartupAssembly</span></span>

<span data-ttu-id="aaafe-564">`Startup` sınıfını aramak için bütünleştirilmiş kod.</span><span class="sxs-lookup"><span data-stu-id="aaafe-564">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="aaafe-565">**Anahtar**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="aaafe-565">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="aaafe-566">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-566">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-567">**Varsayılan**: uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="aaafe-567">**Default**: The app's assembly</span></span>  
<span data-ttu-id="aaafe-568">**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="aaafe-568">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="aaafe-569">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseStartup`çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-569">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="aaafe-570">`UseStartup`, bir derleme adı (`string`) veya bir tür (`TStartup`) alabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-570">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="aaafe-571">Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-571">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="aaafe-572">URL’ler</span><span class="sxs-lookup"><span data-stu-id="aaafe-572">URLs</span></span>

<span data-ttu-id="aaafe-573">Sunucu istekleri için dinlemesi gereken bağlantı noktaları ve protokollerle, noktalı virgülle ayrılmış IP adresleri listesi veya ana bilgisayar adresleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-573">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="aaafe-574">Örneğin, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-574">For example, `http://localhost:123`.</span></span> <span data-ttu-id="aaafe-575">Sunucunun belirtilen bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-575">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="aaafe-576">Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-576">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="aaafe-577">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-577">Supported formats vary among servers.</span></span>

<span data-ttu-id="aaafe-578">**Anahtar**: `urls`</span><span class="sxs-lookup"><span data-stu-id="aaafe-578">**Key**: `urls`</span></span>  
<span data-ttu-id="aaafe-579">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-579">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-580">**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="aaafe-580">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="aaafe-581">**Ortam değişkeni**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="aaafe-581">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="aaafe-582">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseUrls`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-582">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="aaafe-583">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-583">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="aaafe-584">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-584">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="aaafe-585">WebRoot</span><span class="sxs-lookup"><span data-stu-id="aaafe-585">WebRoot</span></span>

<span data-ttu-id="aaafe-586">Uygulamanın statik varlıklarının göreli yolu.</span><span class="sxs-lookup"><span data-stu-id="aaafe-586">The relative path to the app's static assets.</span></span>

<span data-ttu-id="aaafe-587">**Anahtar**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-587">**Key**: `webroot`</span></span>  
<span data-ttu-id="aaafe-588">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-588">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-589">**Varsayılan**: varsayılan `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="aaafe-589">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="aaafe-590">*{Content root}/Wwwroot* yolu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-590">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="aaafe-591">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-591">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="aaafe-592">**Ortam değişkeni**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="aaafe-592">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="aaafe-593">Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseWebRoot`çağırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-593">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="aaafe-594">Daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-594">For more information, see:</span></span>

* [<span data-ttu-id="aaafe-595">Temel bilgiler: Web kökü</span><span class="sxs-lookup"><span data-stu-id="aaafe-595">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="aaafe-596">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="aaafe-596">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="aaafe-597">Konak ömrünü yönetme</span><span class="sxs-lookup"><span data-stu-id="aaafe-597">Manage the host lifetime</span></span>

<span data-ttu-id="aaafe-598">Uygulamayı başlatmak ve durdurmak için oluşturulan <xref:Microsoft.Extensions.Hosting.IHost> uygulamasındaki Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-598">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="aaafe-599">Bu yöntemler, hizmet kapsayıcısında kayıtlı olan tüm <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını etkiler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-599">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="aaafe-600">Çalıştırın</span><span class="sxs-lookup"><span data-stu-id="aaafe-600">Run</span></span>

<span data-ttu-id="aaafe-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="aaafe-602">RunAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-602">RunAsync</span></span>

<span data-ttu-id="aaafe-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlayan bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="aaafe-604">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-604">RunConsoleAsync</span></span>

<span data-ttu-id="aaafe-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> konsol desteği sağlar, Konağı oluşturup başlatır ve kapatmak için <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterm bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="aaafe-606">Başlatma</span><span class="sxs-lookup"><span data-stu-id="aaafe-606">Start</span></span>

<span data-ttu-id="aaafe-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="aaafe-608">StartAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-608">StartAsync</span></span>

<span data-ttu-id="aaafe-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, Konağı başlatır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="aaafe-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, `StartAsync`başlangıcında çağrılır ve bu, devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="aaafe-611">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-611">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="aaafe-612">StopAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-612">StopAsync</span></span>

<span data-ttu-id="aaafe-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="aaafe-614">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="aaafe-614">WaitForShutdown</span></span>

<span data-ttu-id="aaafe-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, kapatmadan sonra, <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya Sigterm gibi bir ıhostlifetime tarafından tetiklenene kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="aaafe-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="aaafe-616">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-616">WaitForShutdownAsync</span></span>

<span data-ttu-id="aaafe-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, verilen belirteç aracılığıyla kapalı tetiklendiğinde ve <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağıran bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="aaafe-618">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="aaafe-618">External control</span></span>

<span data-ttu-id="aaafe-619">Ana bilgisayar ömrünün doğrudan denetimi dışarıdan çağrılabilen yöntemler kullanılarak sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-619">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="aaafe-620">ASP.NET Core uygulamalar bir konağı yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-620">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="aaafe-621">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-621">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="aaafe-622">Bu makalede, HTTP isteklerini işlemeyin uygulamalar için kullanılan ASP.NET Core genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.HostBuilder>) ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-622">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="aaafe-623">Genel konağın amacı, daha geniş bir konak senaryolarını etkinleştirmek üzere Web ana bilgisayar API 'sinden HTTP işlem hattını ayırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-623">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="aaafe-624">Yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliğinden genel ana bilgisayar avantajı temel alınarak mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="aaafe-624">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="aaafe-625">Genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir ve Web barındırma senaryolarında uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-625">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="aaafe-626">Web barındırma senaryolarında [Web konağını](xref:fundamentals/host/web-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-626">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="aaafe-627">Genel ana bilgisayar gelecek bir sürümdeki Web konağını değiştirecek ve hem HTTP hem de HTTP olmayan senaryolarda birincil ana bilgisayar API 'SI olarak görev yapacak.</span><span class="sxs-lookup"><span data-stu-id="aaafe-627">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="aaafe-628">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aaafe-628">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="aaafe-629">Örnek uygulamayı [Visual Studio Code](https://code.visualstudio.com/)' de çalıştırırken, *dış veya tümleşik bir Terminal*kullanın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-629">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="aaafe-630">Örneği bir `internalConsole`çalıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-630">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="aaafe-631">Konsolu Visual Studio Code ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="aaafe-631">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="aaafe-632">*. Vscode/Launch. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-632">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="aaafe-633">**.NET Core başlatma (konsol)** yapılandırmasında **konsol** girişini bulun.</span><span class="sxs-lookup"><span data-stu-id="aaafe-633">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="aaafe-634">Değeri `externalTerminal` ya da `integratedTerminal`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-634">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="aaafe-635">Giriş</span><span class="sxs-lookup"><span data-stu-id="aaafe-635">Introduction</span></span>

<span data-ttu-id="aaafe-636">Genel ana bilgisayar kitaplığı <xref:Microsoft.Extensions.Hosting> ad alanında kullanılabilir ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-636">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="aaafe-637">`Microsoft.Extensions.Hosting` paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e dahildir (ASP.NET Core 2,1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="aaafe-637">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="aaafe-638"><xref:Microsoft.Extensions.Hosting.IHostedService>, kod yürütmeye yönelik giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-638"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="aaafe-639">Her bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama, [ConfigureServices 'de hizmet kaydı](#configureservices)sırasında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-639">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="aaafe-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>, konak başlatıldığında her <xref:Microsoft.Extensions.Hosting.IHostedService> çağrılır ve konak düzgün şekilde kapandığında <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> ters kayıt sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="aaafe-641">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="aaafe-641">Set up a host</span></span>

<span data-ttu-id="aaafe-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder>, kitaplıkların ve uygulamaların Konağı başlatmak, derlemek ve çalıştırmak için kullandığı ana bileşendir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="aaafe-643">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="aaafe-643">Options</span></span>

<span data-ttu-id="aaafe-644"><xref:Microsoft.Extensions.Hosting.HostOptions> <xref:Microsoft.Extensions.Hosting.IHost>seçeneklerini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-644"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="aaafe-645">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="aaafe-645">Shutdown timeout</span></span>

<span data-ttu-id="aaafe-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>, <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="aaafe-647">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-647">The default value is five seconds.</span></span>

<span data-ttu-id="aaafe-648">`Program.Main` ' deki aşağıdaki seçenek yapılandırması, varsayılan beş saniyelik kapatılma zaman aşımını 20 saniyeye yükseltir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-648">The following option configuration in `Program.Main` increases the default five-second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="aaafe-649">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="aaafe-649">Default services</span></span>

<span data-ttu-id="aaafe-650">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aaafe-650">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="aaafe-651">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="aaafe-651">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="aaafe-652">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="aaafe-652">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="aaafe-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="aaafe-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="aaafe-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="aaafe-655">[Seçenekler](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="aaafe-655">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="aaafe-656">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="aaafe-656">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="aaafe-657">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-657">Host configuration</span></span>

<span data-ttu-id="aaafe-658">Ana bilgisayar yapılandırması şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="aaafe-658">Host configuration is created by:</span></span>

* <span data-ttu-id="aaafe-659">[İçerik kökünü](#content-root) ve [ortamı](#environment)ayarlamak için <xref:Microsoft.Extensions.Hosting.IHostBuilder> uzantı yöntemleri çağrılıyor.</span><span class="sxs-lookup"><span data-stu-id="aaafe-659">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="aaafe-660"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>içindeki yapılandırma sağlayıcılarından yapılandırma okunuyor.</span><span class="sxs-lookup"><span data-stu-id="aaafe-660">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="aaafe-661">Uzantı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="aaafe-661">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="aaafe-662">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="aaafe-662">Application key (name)</span></span>

<span data-ttu-id="aaafe-663">[Ihostingenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-663">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="aaafe-664">Değeri açıkça ayarlamak için, [Hostdefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-664">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="aaafe-665">**Anahtar**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="aaafe-665">**Key**: `applicationName`</span></span>  
<span data-ttu-id="aaafe-666">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-666">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-667">**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="aaafe-667">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="aaafe-668">Şunu **kullanarak ayarla**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="aaafe-668">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="aaafe-669">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="aaafe-669">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="aaafe-670">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="aaafe-670">Content root</span></span>

<span data-ttu-id="aaafe-671">Bu ayar, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-671">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="aaafe-672">**Anahtar**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-672">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="aaafe-673">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-673">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-674">**Varsayılan**: uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-674">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="aaafe-675">Şunu **kullanarak ayarla**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="aaafe-675">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="aaafe-676">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="aaafe-676">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="aaafe-677">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="aaafe-677">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="aaafe-678">Daha fazla bilgi için bkz. [temel bilgiler: içerik kökü](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="aaafe-678">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="aaafe-679">Ortam</span><span class="sxs-lookup"><span data-stu-id="aaafe-679">Environment</span></span>

<span data-ttu-id="aaafe-680">Uygulamanın [ortamını](xref:fundamentals/environments)ayarlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-680">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="aaafe-681">**Anahtar**: `environment`</span><span class="sxs-lookup"><span data-stu-id="aaafe-681">**Key**: `environment`</span></span>  
<span data-ttu-id="aaafe-682">**Tür**: `string`</span><span class="sxs-lookup"><span data-stu-id="aaafe-682">**Type**: `string`</span></span>  
<span data-ttu-id="aaafe-683">**Varsayılan**: `Production`</span><span class="sxs-lookup"><span data-stu-id="aaafe-683">**Default**: `Production`</span></span>  
<span data-ttu-id="aaafe-684">Şunu **kullanarak ayarla**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="aaafe-684">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="aaafe-685">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="aaafe-685">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="aaafe-686">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-686">The environment can be set to any value.</span></span> <span data-ttu-id="aaafe-687">Çerçeve tanımlı değerler `Development`, `Staging`ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-687">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="aaafe-688">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-688">Values aren't case-sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="aaafe-689">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="aaafe-689">ConfigureHostConfiguration</span></span>

<span data-ttu-id="aaafe-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, ana bilgisayar için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="aaafe-691">Konak yapılandırması, uygulamanın derleme sürecinde kullanılmak üzere <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-691">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="aaafe-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-693">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-693">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="aaafe-694">Varsayılan olarak hiçbir sağlayıcı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-694">No providers are included by default.</span></span> <span data-ttu-id="aaafe-695">Uygulamanın <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>gereken her türlü yapılandırma sağlayıcısını açıkça belirtmeniz gerekir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="aaafe-695">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="aaafe-696">Dosya yapılandırması (örneğin, *HostSettings. JSON* dosyasından).</span><span class="sxs-lookup"><span data-stu-id="aaafe-696">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="aaafe-697">Ortam değişkeni yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="aaafe-697">Environment variable configuration.</span></span>
* <span data-ttu-id="aaafe-698">Komut satırı bağımsız değişken yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="aaafe-698">Command-line argument configuration.</span></span>
* <span data-ttu-id="aaafe-699">Diğer tüm gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="aaafe-699">Any other required configuration providers.</span></span>

<span data-ttu-id="aaafe-700">Konağın dosya yapılandırması, uygulamanın temel yolu `SetBasePath` ve ardından [dosya yapılandırma sağlayıcılarından](xref:fundamentals/configuration/index#file-configuration-provider)birine yapılan bir çağrı tarafından belirtilerek etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-700">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="aaafe-701">Örnek uygulama bir JSON dosyası, *HostSettings. JSON*kullanır ve dosyanın konak yapılandırma ayarlarını kullanmak için <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-701">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="aaafe-702">Konağın [ortam değişkeni yapılandırmasını](xref:fundamentals/configuration/index#environment-variables-configuration-provider) eklemek için konak oluşturucu üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="aaafe-702">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="aaafe-703">`AddEnvironmentVariables`, Kullanıcı tanımlı isteğe bağlı bir ön eki kabul eder.</span><span class="sxs-lookup"><span data-stu-id="aaafe-703">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="aaafe-704">Örnek uygulama `PREFIX_`önekini kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-704">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="aaafe-705">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-705">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="aaafe-706">Örnek uygulamanın ana bilgisayarı yapılandırıldığında, `PREFIX_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri olur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-706">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="aaafe-707">[Visual Studio](https://visualstudio.microsoft.com) kullanırken veya `dotnet run`bir uygulama çalıştırırken geliştirme sırasında, *Özellikler/launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-707">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="aaafe-708">[Visual Studio Code](https://code.visualstudio.com/), geliştirme sırasında *. vscode/Launch. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-708">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="aaafe-709">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-709">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="aaafe-710">[Komut satırı yapılandırması](xref:fundamentals/configuration/index#command-line-configuration-provider) , <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>çağırarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-710">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="aaafe-711">Komut satırı yapılandırması, önceki yapılandırma sağlayıcıları tarafından belirtilen yapılandırmayı geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek üzere en son eklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-711">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="aaafe-712">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="aaafe-712">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="aaafe-713">Ek yapılandırma [ApplicationName](#application-key-name) ve [contentroot](#content-root) anahtarlarıyla birlikte etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-713">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="aaafe-714"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>kullanarak `HostBuilder` yapılandırma örneği:</span><span class="sxs-lookup"><span data-stu-id="aaafe-714">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="aaafe-715">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="aaafe-715">ConfigureAppConfiguration</span></span>

<span data-ttu-id="aaafe-716">Uygulama yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasındaki <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aaafe-716">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="aaafe-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, uygulama için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="aaafe-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="aaafe-719">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-719">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="aaafe-720"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> tarafından oluşturulan yapılandırma, sonraki işlemler ve <xref:Microsoft.Extensions.Hosting.IHost.Services*>için [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) ' da kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-720">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="aaafe-721">Uygulama yapılandırması, [Configurehostconfiguration](#configurehostconfiguration)tarafından belirtilen konak yapılandırmasını otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-721">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="aaafe-722"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>kullanarak örnek uygulama yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="aaafe-722">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="aaafe-723">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="aaafe-723">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="aaafe-724">*appSettings. Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="aaafe-724">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="aaafe-725">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="aaafe-725">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="aaafe-726">Ayar dosyalarını çıkış dizinine taşımak için, ayarlar dosyalarını proje dosyasında [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-726">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="aaafe-727">Örnek uygulama, JSON uygulama ayarları dosyalarını ve *HostSettings. JSON* dosyasını şu `<Content>` öğesiyle taşıdıkça:</span><span class="sxs-lookup"><span data-stu-id="aaafe-727">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="aaafe-728"><xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> gibi yapılandırma uzantısı yöntemleri, [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables)gibi ek NuGet paketleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-728">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="aaafe-729">Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'i kullanmadığı takdirde, bu paketlerin temel [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paketine ek olarak projeye eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-729">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="aaafe-730">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-730">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="aaafe-731">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="aaafe-731">ConfigureServices</span></span>

<span data-ttu-id="aaafe-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, uygulamanın [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="aaafe-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="aaafe-734">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-734">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="aaafe-735">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-735">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="aaafe-736">[Örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , yaşam süresi olayları, `LifetimeEventsHostedService`ve zaman aşımına uğramış bir arka plan görevi olan `TimedHostedService`uygulamaya bir hizmet eklemek için `AddHostedService` uzantısı yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-736">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="aaafe-737">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="aaafe-737">ConfigureLogging</span></span>

<span data-ttu-id="aaafe-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, belirtilen <xref:Microsoft.Extensions.Logging.ILoggingBuilder>yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="aaafe-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="aaafe-740">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="aaafe-740">UseConsoleLifetime</span></span>

<span data-ttu-id="aaafe-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>, <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterm dinler ve <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatır ve bu işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="aaafe-742">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*></span><span class="sxs-lookup"><span data-stu-id="aaafe-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="aaafe-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` varsayılan ömür uygulamasıyla önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="aaafe-744">Kaydedilen son yaşam süresi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-744">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="aaafe-745">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="aaafe-745">Container configuration</span></span>

<span data-ttu-id="aaafe-746">Diğer kapsayıcıları takmayı desteklemek için, ana bilgisayar bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-746">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="aaafe-747">Fabrika sağlamak, dı kapsayıcı kaydının bir parçası değildir, ancak bunun yerine somut dı kapsayıcısını oluşturmak için kullanılan bir konak iç sıdır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-747">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="aaafe-748">[Useserviceproviderfactory (ıvıceproviderfactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) , uygulamanın hizmet sağlayıcısını oluşturmak için kullanılan varsayılan fabrikası geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-748">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="aaafe-749">Özel kapsayıcı yapılandırması <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi tarafından yönetilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-749">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="aaafe-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, temel ana bilgisayar API 'sinin üstünde kapsayıcıyı yapılandırmak için kesin olarak belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="aaafe-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="aaafe-752">Uygulama için bir hizmet kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="aaafe-752">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="aaafe-753">Hizmet kapsayıcısı fabrikası sağlama:</span><span class="sxs-lookup"><span data-stu-id="aaafe-753">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="aaafe-754">Fabrika 'yi kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-754">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="aaafe-755">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="aaafe-755">Extensibility</span></span>

<span data-ttu-id="aaafe-756">Konak genişletilebilirliği <xref:Microsoft.Extensions.Hosting.IHostBuilder>uzantı yöntemleriyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-756">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="aaafe-757">Aşağıdaki örnek, bir genişletme yönteminin <xref:fundamentals/host/hosted-services>gösterilen [Timedhostedservice](xref:fundamentals/host/hosted-services#timed-background-tasks) örneği ile bir <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasını nasıl genişlettiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-757">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="aaafe-758">Uygulama, `T`geçirilen barındırılan hizmeti kaydetmek için `UseHostedService` uzantısı yöntemini oluşturur:</span><span class="sxs-lookup"><span data-stu-id="aaafe-758">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="aaafe-759">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="aaafe-759">Manage the host</span></span>

<span data-ttu-id="aaafe-760"><xref:Microsoft.Extensions.Hosting.IHost> uygulaması, hizmet kapsayıcısında kayıtlı <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını başlatma ve durdurma sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-760">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="aaafe-761">Çalıştırın</span><span class="sxs-lookup"><span data-stu-id="aaafe-761">Run</span></span>

<span data-ttu-id="aaafe-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="aaafe-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="aaafe-763">RunAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-763">RunAsync</span></span>

<span data-ttu-id="aaafe-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlayan bir <xref:System.Threading.Tasks.Task> döndürür:</span><span class="sxs-lookup"><span data-stu-id="aaafe-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="aaafe-765">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-765">RunConsoleAsync</span></span>

<span data-ttu-id="aaafe-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> konsol desteği sağlar, Konağı oluşturup başlatır ve kapatmak için <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterm bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="aaafe-767">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-767">Start and StopAsync</span></span>

<span data-ttu-id="aaafe-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="aaafe-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="aaafe-770">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-770">StartAsync and StopAsync</span></span>

<span data-ttu-id="aaafe-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="aaafe-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="aaafe-773">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="aaafe-773">WaitForShutdown</span></span>

<span data-ttu-id="aaafe-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` ( <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT veya sigterim dinler) gibi <xref:Microsoft.Extensions.Hosting.IHostLifetime>aracılığıyla tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM).</span></span> <span data-ttu-id="aaafe-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağırır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="aaafe-776">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="aaafe-776">WaitForShutdownAsync</span></span>

<span data-ttu-id="aaafe-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, verilen belirteç aracılığıyla kapalı tetiklendiğinde ve <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>çağıran bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aaafe-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="aaafe-778">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="aaafe-778">External control</span></span>

<span data-ttu-id="aaafe-779">Ana bilgisayarın dış denetimine, dışarıdan çağrılabilen yöntemler kullanılarak ulaşılabilecek:</span><span class="sxs-lookup"><span data-stu-id="aaafe-779">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="aaafe-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>başlangıcında çağrılır ve bu, devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="aaafe-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="aaafe-781">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-781">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="aaafe-782">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="aaafe-782">IHostingEnvironment interface</span></span>

<span data-ttu-id="aaafe-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="aaafe-784">Özelliklerini ve uzantı yöntemlerini kullanmak için <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> almak üzere [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:</span><span class="sxs-lookup"><span data-stu-id="aaafe-784">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="aaafe-785">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="aaafe-785">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="aaafe-786">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="aaafe-786">IApplicationLifetime interface</span></span>

<span data-ttu-id="aaafe-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime>, düzgün kapanma istekleri dahil olmak üzere başlatma sonrası ve kapanma etkinliklerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="aaafe-788">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan <xref:System.Action> yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-788">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="aaafe-789">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="aaafe-789">Cancellation Token</span></span> | <span data-ttu-id="aaafe-790">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="aaafe-790">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="aaafe-791">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="aaafe-791">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="aaafe-792">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="aaafe-792">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="aaafe-793">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-793">All requests should be processed.</span></span> <span data-ttu-id="aaafe-794">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-794">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="aaafe-795">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="aaafe-795">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="aaafe-796">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="aaafe-796">Requests may still be processing.</span></span> <span data-ttu-id="aaafe-797">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="aaafe-797">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="aaafe-798">Constructor-<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> hizmetini herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aaafe-798">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="aaafe-799">[Örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , olayları kaydetmek için bir `LifetimeEventsHostedService` sınıfına (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulaması) Oluşturucu ekleme işlemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="aaafe-799">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="aaafe-800">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="aaafe-800">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="aaafe-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="aaafe-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="aaafe-802">Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="aaafe-802">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="aaafe-803">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="aaafe-803">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
