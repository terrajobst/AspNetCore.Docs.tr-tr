---
title: .NET genel ana bilgisayar
author: tdykstra
description: Uygulama başlatma ve ömür yönetiminden sorumlu .NET Core genel ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9f5ecc7840fc7ffd9432a3bb67d0418efb7e8fd6
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975625"
---
# <a name="net-generic-host"></a><span data-ttu-id="b74f7-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="b74f7-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b74f7-104">Bu makalede .NET Core genel Konağı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) tanıtılmakta ve nasıl kullanılacağına ilişkin yönergeler sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="b74f7-105">Ana bilgisayar nedir?</span><span class="sxs-lookup"><span data-stu-id="b74f7-105">What's a host?</span></span>

<span data-ttu-id="b74f7-106">*Ana bilgisayar* , bir uygulamanın kaynaklarını kapsülleyen bir nesnedir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b74f7-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="b74f7-107">Bağımlılık ekleme (dı)</span><span class="sxs-lookup"><span data-stu-id="b74f7-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="b74f7-108">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="b74f7-108">Logging</span></span>
* <span data-ttu-id="b74f7-109">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b74f7-109">Configuration</span></span>
* <span data-ttu-id="b74f7-110">`IHostedService`uygulamalarını</span><span class="sxs-lookup"><span data-stu-id="b74f7-110">`IHostedService` implementations</span></span>

<span data-ttu-id="b74f7-111">Bir konak başlatıldığında, bu uygulamanın, `IHostedService.StartAsync` dı kapsayıcısında bulduğu her <xref:Microsoft.Extensions.Hosting.IHostedService> bir uygulamasına çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="b74f7-112">Bir Web uygulamasında, `IHostedService` uygulamalardan biri [http sunucu uygulaması](xref:fundamentals/index#servers)Başlatan bir Web hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="b74f7-113">Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="b74f7-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="b74f7-114">3,0 ' den önceki ASP.NET Core sürümlerinde, [Web ana BILGISAYARı](xref:fundamentals/host/web-host) http iş yükleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="b74f7-115">Web ana bilgisayarı artık Web uygulamaları için önerilmez ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="b74f7-116">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="b74f7-116">Set up a host</span></span>

<span data-ttu-id="b74f7-117">Ana bilgisayar genellikle `Program` sınıfında kodla yapılandırılır, oluşturulur ve çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="b74f7-118">`Main` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b74f7-118">The `Main` method:</span></span>

* <span data-ttu-id="b74f7-119">Bir Oluşturucu `CreateHostBuilder` nesnesi oluşturmak ve yapılandırmak için bir yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="b74f7-120">Oluşturucu `Build` nesnesindeki `Run` çağrılar ve Yöntemler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="b74f7-121">Bu, dı kapsayıcısına tek `IHostedService` bir uygulama eklenerek http olmayan bir iş yükü için program.cs kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="b74f7-122">Bir http iş yükü `Main` için yöntem aynıdır `ConfigureWebHostDefaults`ancak `CreateHostBuilder` çağrılır:</span><span class="sxs-lookup"><span data-stu-id="b74f7-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="b74f7-123">Uygulama Entity Framework Core kullanıyorsa, `CreateHostBuilder` yöntemin adını veya imzasını değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="b74f7-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="b74f7-124">[Entity Framework Core araçları](/ef/core/miscellaneous/cli/) , uygulamayı çalıştırmadan Konağı yapılandıran `CreateHostBuilder` bir yöntem bulmayı bekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="b74f7-125">Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="b74f7-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="b74f7-126">Varsayılan Oluşturucu ayarları</span><span class="sxs-lookup"><span data-stu-id="b74f7-126">Default builder settings</span></span> 

<span data-ttu-id="b74f7-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b74f7-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="b74f7-128">İçerik kökünü tarafından <xref:System.IO.Directory.GetCurrentDirectory*>döndürülen yola ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-128">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="b74f7-129">Ana bilgisayar yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="b74f7-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="b74f7-130">"DOTNET_" önekli ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="b74f7-131">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-131">Command-line arguments.</span></span>
* <span data-ttu-id="b74f7-132">Uygulama yapılandırmasını şuradan yükler:</span><span class="sxs-lookup"><span data-stu-id="b74f7-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="b74f7-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b74f7-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="b74f7-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b74f7-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="b74f7-135">[](xref:security/app-secrets) Uygulama `Development` ortamda çalıştığında gizli dizi Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="b74f7-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="b74f7-136">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-136">Environment variables.</span></span>
  * <span data-ttu-id="b74f7-137">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-137">Command-line arguments.</span></span>
* <span data-ttu-id="b74f7-138">Aşağıdaki [günlük](xref:fundamentals/logging/index) sağlayıcılarını ekler:</span><span class="sxs-lookup"><span data-stu-id="b74f7-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="b74f7-139">Konsol</span><span class="sxs-lookup"><span data-stu-id="b74f7-139">Console</span></span>
  * <span data-ttu-id="b74f7-140">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="b74f7-140">Debug</span></span>
  * <span data-ttu-id="b74f7-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="b74f7-141">EventSource</span></span>
  * <span data-ttu-id="b74f7-142">Olay günlüğü (yalnızca Windows üzerinde çalışırken)</span><span class="sxs-lookup"><span data-stu-id="b74f7-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="b74f7-143">Ortam geliştirme sırasında [kapsam doğrulaması](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="b74f7-144">`ConfigureWebHostDefaults` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b74f7-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="b74f7-145">"ASPNETCORE_" önekli ortam değişkenlerinden ana bilgisayar yapılandırmasını yükler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="b74f7-146">[Kestrel](xref:fundamentals/servers/kestrel) sunucusunu Web sunucusu olarak ayarlar ve uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="b74f7-147">Kestrel sunucusunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="b74f7-148">[Ana bilgisayar filtreleme ara yazılımı](xref:fundamentals/servers/kestrel#host-filtering)ekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="b74f7-149">ASPNETCORE_FORWARDEDHEADERS_ENABLED = true ise [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) ekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="b74f7-150">IIS tümleştirmesini etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-150">Enables IIS integration.</span></span> <span data-ttu-id="b74f7-151">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="b74f7-152">Bu makalenin ilerleyen kısımlarında [Web Apps bölümlerine yönelik](#settings-for-web-apps) [tüm uygulama türleri](#settings-for-all-app-types) ve ayarlarının ayarları, varsayılan Oluşturucu ayarlarının nasıl geçersiz kılınacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="b74f7-153">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="b74f7-153">Framework-provided services</span></span>

<span data-ttu-id="b74f7-154">Kayıtlı hizmetler otomatik olarak şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="b74f7-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="b74f7-155">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="b74f7-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="b74f7-156">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="b74f7-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="b74f7-157">Ihostenvironment/ıwebhostenvironment</span><span class="sxs-lookup"><span data-stu-id="b74f7-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="b74f7-158">Framework tarafından sunulan tüm hizmetlerin bir listesi için bkz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-158">For a list of all framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="b74f7-159">Ihostapplicationlifetime</span><span class="sxs-lookup"><span data-stu-id="b74f7-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="b74f7-160">Başlatma sonrası ve düzgün `IApplicationLifetime`kapanma görevlerini işlemek için herhangi bir sınıfa (eskiadıyla)hizmetiniekleyin.<xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime></span><span class="sxs-lookup"><span data-stu-id="b74f7-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="b74f7-161">Arabirimdeki üç özellik, uygulama başlatma ve uygulama durdurma olay işleyicisi yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="b74f7-162">Arabirim Ayrıca bir `StopApplication` yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="b74f7-163">Aşağıdaki örnek, `IApplicationLifetime` olayları kaydeden `IHostedService` bir uygulamasıdır:</span><span class="sxs-lookup"><span data-stu-id="b74f7-163">The following example is an `IHostedService` implementation that registers the `IApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="b74f7-164">Ihostlifetime</span><span class="sxs-lookup"><span data-stu-id="b74f7-164">IHostLifetime</span></span>

<span data-ttu-id="b74f7-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> Uygulama, ana bilgisayar başladığında ve durdurulduğunda denetler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="b74f7-166">Kaydedilen son uygulama kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-166">The last implementation registered is used.</span></span>

<span data-ttu-id="b74f7-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>Varsayılan `IHostLifetime` uygulama.</span><span class="sxs-lookup"><span data-stu-id="b74f7-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="b74f7-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="b74f7-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="b74f7-169">CTRL + C/sigint veya sigdönem için dinler ve kapalı <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="b74f7-170">[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="b74f7-171">Ihostenvironment</span><span class="sxs-lookup"><span data-stu-id="b74f7-171">IHostEnvironment</span></span>

<span data-ttu-id="b74f7-172">Aşağıdaki bilgileri almak için hizmetibirsınıfaekleyin:<xref:Microsoft.Extensions.Hosting.IHostEnvironment></span><span class="sxs-lookup"><span data-stu-id="b74f7-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="b74f7-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="b74f7-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="b74f7-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="b74f7-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="b74f7-175">Contentrootyolu</span><span class="sxs-lookup"><span data-stu-id="b74f7-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="b74f7-176">Web Apps, devralan `IWebHostEnvironment` `IHostEnvironment` ve ekleyen arabirimini uygular:</span><span class="sxs-lookup"><span data-stu-id="b74f7-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="b74f7-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="b74f7-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="b74f7-178">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b74f7-178">Host configuration</span></span>

<span data-ttu-id="b74f7-179">Ana bilgisayar yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulamanın özellikleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="b74f7-180">Konak yapılandırması, içinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> [hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="b74f7-181">Sonra `ConfigureAppConfiguration` ,`HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="b74f7-182">Konak yapılandırması eklemek için üzerinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> `IHostBuilder`öğesini çağırın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="b74f7-183">`ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="b74f7-184">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="b74f7-185">Ön eke `DOTNET_` ve komut satırı bağımsız değişkenlerine sahip ortam değişkeni sağlayıcısı, createdefaultbuilder tarafından eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="b74f7-186">Web Apps için, ön ekine `ASPNETCORE_` sahip ortam değişkeni sağlayıcısı eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="b74f7-187">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="b74f7-188">Örneğin, için `ASPNETCORE_ENVIRONMENT` ortam değişkeni değeri, `environment` anahtar için ana bilgisayar yapılandırma değeri olur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="b74f7-189">Aşağıdaki örnek ana bilgisayar yapılandırması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b74f7-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="b74f7-190">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b74f7-190">App configuration</span></span>

<span data-ttu-id="b74f7-191">Uygulama yapılandırması, üzerinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> `IHostBuilder`çağırarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="b74f7-192">`ConfigureAppConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="b74f7-193">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="b74f7-194">Tarafından `ConfigureAppConfiguration` oluşturulan yapılandırma, sonraki işlemler ve dı hizmeti olarak, [hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) konumunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="b74f7-195">Konak yapılandırması, uygulama yapılandırmasına de eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="b74f7-196">Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index#configureappconfiguration)konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="b74f7-197">Tüm uygulama türleri için ayarlar</span><span class="sxs-lookup"><span data-stu-id="b74f7-197">Settings for all app types</span></span>

<span data-ttu-id="b74f7-198">Bu bölüm, hem HTTP hem de HTTP olmayan iş yükleri için uygulanan konak ayarlarını listeler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="b74f7-199">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

### <a name="applicationname"></a><span data-ttu-id="b74f7-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="b74f7-200">ApplicationName</span></span>

<span data-ttu-id="b74f7-201">[Ihostenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="b74f7-202">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="b74f7-202">**Key**: applicationName</span></span>  
<span data-ttu-id="b74f7-203">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-203">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-204">**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="b74f7-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="b74f7-205">**Ortam değişkeni**:`<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="b74f7-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="b74f7-206">Bu değeri ayarlamak için ortam değişkenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="b74f7-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="b74f7-207">ContentRootPath</span></span>

<span data-ttu-id="b74f7-208">[Ihostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özelliği, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="b74f7-209">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="b74f7-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="b74f7-210">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="b74f7-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="b74f7-211">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-211">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-212">**Varsayılan**: Uygulama derlemesinin bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="b74f7-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="b74f7-213">**Ortam değişkeni**:`<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="b74f7-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="b74f7-214">Bu değeri ayarlamak için, ortam değişkenini kullanın veya üzerinde `UseContentRoot` `IHostBuilder`arama yapın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a><span data-ttu-id="b74f7-215">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="b74f7-215">EnvironmentName</span></span>

<span data-ttu-id="b74f7-216">[Ihostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-216">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="b74f7-217">Çerçeve tanımlı değerler, `Development` `Staging`, ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b74f7-218">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-218">Values aren't case sensitive.</span></span>

<span data-ttu-id="b74f7-219">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="b74f7-219">**Key**: environment</span></span>  
<span data-ttu-id="b74f7-220">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-220">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-221">**Varsayılan**: Üretiminden</span><span class="sxs-lookup"><span data-stu-id="b74f7-221">**Default**: Production</span></span>  
<span data-ttu-id="b74f7-222">**Ortam değişkeni**:`<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="b74f7-222">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="b74f7-223">Bu değeri ayarlamak için, ortam değişkenini kullanın veya üzerinde `UseEnvironment` `IHostBuilder`arama yapın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-223">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="b74f7-224">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="b74f7-224">ShutdownTimeout</span></span>

<span data-ttu-id="b74f7-225">[Hostoptions. ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) , için <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>zaman aşımını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="b74f7-226">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-226">The default value is five seconds.</span></span>  <span data-ttu-id="b74f7-227">Zaman aşımı süresi boyunca ana bilgisayar:</span><span class="sxs-lookup"><span data-stu-id="b74f7-227">During the timeout period, the host:</span></span>

* <span data-ttu-id="b74f7-228">[Ihostapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)tetikler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-228">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="b74f7-229">Üzerinde durmayacak hizmetler için barındırılan Hizmetleri durdurma ve hataları günlüğe kaydetme girişimleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-229">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="b74f7-230">Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-230">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="b74f7-231">Hizmetler, işlemeyi tamamlamadıklarında bile durur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-231">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="b74f7-232">Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-232">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="b74f7-233">**Anahtar**: shutdowntimeoutseconds</span><span class="sxs-lookup"><span data-stu-id="b74f7-233">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="b74f7-234">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="b74f7-234">**Type**: *int*</span></span>  
<span data-ttu-id="b74f7-235">**Varsayılan**: 5 saniye **ortam değişkeni**:`<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="b74f7-235">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="b74f7-236">Bu değeri ayarlamak için, ortam değişkenini kullanın veya yapılandırın `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="b74f7-236">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="b74f7-237">Aşağıdaki örnek, zaman aşımını 20 saniye olarak ayarlar:</span><span class="sxs-lookup"><span data-stu-id="b74f7-237">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="b74f7-238">Web Apps ayarları</span><span class="sxs-lookup"><span data-stu-id="b74f7-238">Settings for web apps</span></span>

<span data-ttu-id="b74f7-239">Bazı konak ayarları yalnızca HTTP iş yükleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-239">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="b74f7-240">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin bir `DOTNET_` veya `ASPNETCORE_` öneki olabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-240">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="b74f7-241">Üzerinde `IWebHostBuilder` uzantı yöntemleri bu ayarlar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-241">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="b74f7-242">Aşağıdaki örnekte olduğu gibi, ' nin `webBuilder` `IWebHostBuilder`bir örneği olduğunu belirten uzantı yöntemlerinin nasıl çağrılacağını gösteren kod örnekleri:</span><span class="sxs-lookup"><span data-stu-id="b74f7-242">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="b74f7-243">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="b74f7-243">CaptureStartupErrors</span></span>

<span data-ttu-id="b74f7-244">Ne `false`zaman, başlatma sırasında oluşan hata, ana bilgisayardan çıkılıyor.</span><span class="sxs-lookup"><span data-stu-id="b74f7-244">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="b74f7-245">Ne `true`zaman, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-245">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="b74f7-246">**Anahtar**: capturestartuperrors</span><span class="sxs-lookup"><span data-stu-id="b74f7-246">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="b74f7-247">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b74f7-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b74f7-248">**Varsayılan**: Uygulamanın IIS 'nin arkasında Kestrel, varsayılan olarak olduğu `true` durumlardışındaçalışır.`false`</span><span class="sxs-lookup"><span data-stu-id="b74f7-248">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="b74f7-249">**Ortam değişkeni**:`<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="b74f7-249">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="b74f7-250">Bu değeri ayarlamak için yapılandırma veya çağırma `CaptureStartupErrors`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-250">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="b74f7-251">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="b74f7-251">DetailedErrors</span></span>

<span data-ttu-id="b74f7-252">Etkinleştirildiğinde veya ortam `Development`olduğunda, uygulama ayrıntılı hataları yakalar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-252">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="b74f7-253">**Anahtar**: detailederrors</span><span class="sxs-lookup"><span data-stu-id="b74f7-253">**Key**: detailedErrors</span></span>  
<span data-ttu-id="b74f7-254">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b74f7-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b74f7-255">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="b74f7-255">**Default**: false</span></span>  
<span data-ttu-id="b74f7-256">**Ortam değişkeni**:`<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="b74f7-256">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="b74f7-257">Bu değeri ayarlamak için yapılandırma veya çağırma `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-257">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="b74f7-258">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b74f7-258">HostingStartupAssemblies</span></span>

<span data-ttu-id="b74f7-259">Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="b74f7-259">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="b74f7-260">Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-260">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="b74f7-261">Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-261">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="b74f7-262">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="b74f7-262">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="b74f7-263">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-263">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-264">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="b74f7-264">**Default**: Empty string</span></span>  
<span data-ttu-id="b74f7-265">**Ortam değişkeni**:`<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b74f7-265">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="b74f7-266">Bu değeri ayarlamak için yapılandırma veya çağırma `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-266">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="b74f7-267">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="b74f7-267">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="b74f7-268">Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.</span><span class="sxs-lookup"><span data-stu-id="b74f7-268">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="b74f7-269">**Anahtar**: hostingstartupexcludeassemblies</span><span class="sxs-lookup"><span data-stu-id="b74f7-269">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="b74f7-270">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-270">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-271">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="b74f7-271">**Default**: Empty string</span></span>  
<span data-ttu-id="b74f7-272">**Ortam değişkeni**:`<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="b74f7-272">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="b74f7-273">Bu değeri ayarlamak için yapılandırma veya çağırma `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-273">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="b74f7-274">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="b74f7-274">HTTPS_Port</span></span>

<span data-ttu-id="b74f7-275">HTTPS yeniden yönlendirme bağlantı noktası.</span><span class="sxs-lookup"><span data-stu-id="b74f7-275">The HTTPS redirect port.</span></span> <span data-ttu-id="b74f7-276">[Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-276">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="b74f7-277">**Anahtar**: https_port **Type**: *dize*
**varsayılan**: Varsayılan değer ayarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-277">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="b74f7-278">**Ortam değişkeni**:`<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="b74f7-278">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="b74f7-279">Bu değeri ayarlamak için yapılandırma veya çağırma `UseSetting`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="b74f7-280">Tercih Hostingurl 'Leri</span><span class="sxs-lookup"><span data-stu-id="b74f7-280">PreferHostingUrls</span></span>

<span data-ttu-id="b74f7-281">Konağın `IWebHostBuilder` `IServer` uygulamayla yapılandırılanlar yerine ile yapılandırılan URL 'lerde dinleme yapıp kullanmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-281">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="b74f7-282">**Anahtar**: preferhostingurl 'leri</span><span class="sxs-lookup"><span data-stu-id="b74f7-282">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="b74f7-283">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b74f7-283">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b74f7-284">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="b74f7-284">**Default**: true</span></span>  
<span data-ttu-id="b74f7-285">**Ortam değişkeni**:`<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="b74f7-285">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="b74f7-286">Bu değeri ayarlamak için, ortam değişkenini veya çağrısını `PreferHostingUrls`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-286">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="b74f7-287">Koruyucu Thostınstartup</span><span class="sxs-lookup"><span data-stu-id="b74f7-287">PreventHostingStartup</span></span>

<span data-ttu-id="b74f7-288">Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="b74f7-288">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="b74f7-289">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-289">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="b74f7-290">**Anahtar**: koruyucu thostingstartup</span><span class="sxs-lookup"><span data-stu-id="b74f7-290">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="b74f7-291">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="b74f7-291">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="b74f7-292">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="b74f7-292">**Default**: false</span></span>  
<span data-ttu-id="b74f7-293">**Ortam değişkeni**:`<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="b74f7-293">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="b74f7-294">Bu değeri ayarlamak için, ortam değişkenini veya çağrısını `UseSetting` kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-294">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="b74f7-295">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="b74f7-295">StartupAssembly</span></span>

<span data-ttu-id="b74f7-296">`Startup` Sınıfı aramak için bütünleştirilmiş kod.</span><span class="sxs-lookup"><span data-stu-id="b74f7-296">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="b74f7-297">**Anahtar**: startupassembly **türü**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-297">**Key**: startupAssembly **Type**: *string*</span></span>  
<span data-ttu-id="b74f7-298">**Varsayılan**: Uygulamanın derlemesi</span><span class="sxs-lookup"><span data-stu-id="b74f7-298">**Default**: The app's assembly</span></span>  
<span data-ttu-id="b74f7-299">**Ortam değişkeni**:`<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="b74f7-299">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="b74f7-300">Bu değeri ayarlamak için, ortam değişkenini veya çağrısını `UseStartup`kullanın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-300">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="b74f7-301">`UseStartup`bir derleme adı (`string`) veya bir tür (`TStartup`) alabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-301">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="b74f7-302">Birden çok `UseStartup` yöntem çağrılırsa, son bir öncelik alır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-302">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="b74f7-303">URL'ler</span><span class="sxs-lookup"><span data-stu-id="b74f7-303">URLs</span></span>

<span data-ttu-id="b74f7-304">Sunucu istekleri için dinlemesi gereken bağlantı noktaları ve protokollerle, noktalı virgülle ayrılmış IP adresleri listesi veya ana bilgisayar adresleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-304">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="b74f7-305">Örneğin: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="b74f7-305">For example, `http://localhost:123`.</span></span> <span data-ttu-id="b74f7-306">Sunucunun belirtilen\*bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "" kullanın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-306">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="b74f7-307">Protokol (`http://` veya `https://`) her URL 'ye dahil edilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-307">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="b74f7-308">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-308">Supported formats vary among servers.</span></span>

<span data-ttu-id="b74f7-309">**Anahtar**: URL 'ler</span><span class="sxs-lookup"><span data-stu-id="b74f7-309">**Key**: urls</span></span>  
<span data-ttu-id="b74f7-310">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-310">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-311">**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001`ortam değişkeni: 
`<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="b74f7-311">**Default**: `http://localhost:5000` and `https://localhost:5001`
**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="b74f7-312">Bu değeri ayarlamak için, ortam değişkenini veya çağrısını `UseUrls`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-312">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="b74f7-313">Kestrel kendi uç nokta yapılandırması API 'sine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-313">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="b74f7-314">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-314">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="b74f7-315">WebRoot</span><span class="sxs-lookup"><span data-stu-id="b74f7-315">WebRoot</span></span>

<span data-ttu-id="b74f7-316">Uygulamanın statik varlıklarının göreli yolu.</span><span class="sxs-lookup"><span data-stu-id="b74f7-316">The relative path to the app's static assets.</span></span>

<span data-ttu-id="b74f7-317">**Anahtar**: Webroot</span><span class="sxs-lookup"><span data-stu-id="b74f7-317">**Key**: webroot</span></span>  
<span data-ttu-id="b74f7-318">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-318">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-319">**Varsayılan**: *(Içerik kökü)/Wwwroot*, yol varsa.</span><span class="sxs-lookup"><span data-stu-id="b74f7-319">**Default**: *(Content Root)/wwwroot*, if the path exists.</span></span> <span data-ttu-id="b74f7-320">Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-320">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="b74f7-321">**Ortam değişkeni**:`<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="b74f7-321">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="b74f7-322">Bu değeri ayarlamak için, ortam değişkenini veya çağrısını `UseWebRoot`kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-322">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="b74f7-323">Konak ömrünü yönetme</span><span class="sxs-lookup"><span data-stu-id="b74f7-323">Manage the host lifetime</span></span>

<span data-ttu-id="b74f7-324">Uygulamayı başlatmak ve durdurmak için <xref:Microsoft.Extensions.Hosting.IHost> oluşturulan uygulamadaki Yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-324">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="b74f7-325">Bu yöntemler, hizmet <xref:Microsoft.Extensions.Hosting.IHostedService> kapsayıcısına kayıtlı tüm uygulamaları etkiler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-325">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="b74f7-326">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="b74f7-326">Run</span></span>

<span data-ttu-id="b74f7-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>uygulamayı çalıştırır ve ana bilgisayarı kapatıncaya kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="b74f7-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="b74f7-328">RunAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-328">RunAsync</span></span>

<span data-ttu-id="b74f7-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*>uygulamayı çalıştırır ve iptal belirteci veya <xref:System.Threading.Tasks.Task> kapanışı tetiklendiğinde tamamlanmış bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="b74f7-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="b74f7-330">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-330">RunConsoleAsync</span></span>

<span data-ttu-id="b74f7-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>Konsol desteği sağlar, ana bilgisayarı oluşturur ve başlatır, sonra da CTRL + C/SIGINT veya SIGDÖNEM ' in kapatılmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="b74f7-332">Başlat</span><span class="sxs-lookup"><span data-stu-id="b74f7-332">Start</span></span>

<span data-ttu-id="b74f7-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*>Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="b74f7-334">StartAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-334">StartAsync</span></span>

<span data-ttu-id="b74f7-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>Konağı başlatır ve iptal belirteci ya <xref:System.Threading.Tasks.Task> da kapatması tetiklendiğinde tamamlanmış bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="b74f7-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="b74f7-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, ' nin `StartAsync`başlangıcında çağrılır ve devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="b74f7-337">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-337">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="b74f7-338">StopAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-338">StopAsync</span></span>

<span data-ttu-id="b74f7-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="b74f7-340">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="b74f7-340">WaitForShutdown</span></span>

<span data-ttu-id="b74f7-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>CTRL + C/SIGINT veya SIGTERM gibi bir ıhostlifetime tarafından tetiklenene kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="b74f7-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="b74f7-342">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-342">WaitForShutdownAsync</span></span>

<span data-ttu-id="b74f7-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>verilen belirteç <xref:System.Threading.Tasks.Task> ve çağrılar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>aracılığıyla kapatılma tetiklendiğinde tamamlanan bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="b74f7-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="b74f7-344">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="b74f7-344">External control</span></span>

<span data-ttu-id="b74f7-345">Ana bilgisayar ömrünün doğrudan denetimi dışarıdan çağrılabilen yöntemler kullanılarak sağlanabilir:</span><span class="sxs-lookup"><span data-stu-id="b74f7-345">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="b74f7-346">ASP.NET Core uygulamalar bir konağı yapılandırıp başlatır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-346">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="b74f7-347">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-347">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="b74f7-348">Bu makalede, http isteklerini işlemeyecek uygulamalar<xref:Microsoft.Extensions.Hosting.HostBuilder>için kullanılan genel ana bilgisayar () ASP.NET Core ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-348">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="b74f7-349">Genel konağın amacı, daha geniş bir konak senaryolarını etkinleştirmek üzere Web ana bilgisayar API 'sinden HTTP işlem hattını ayırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-349">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="b74f7-350">Yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliğinden genel ana bilgisayar avantajı temel alınarak mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="b74f7-350">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="b74f7-351">Genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir ve Web barındırma senaryolarında uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-351">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="b74f7-352">Web barındırma senaryolarında [Web konağını](xref:fundamentals/host/web-host)kullanın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-352">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="b74f7-353">Genel ana bilgisayar gelecek bir sürümdeki Web konağını değiştirecek ve hem HTTP hem de HTTP olmayan senaryolarda birincil ana bilgisayar API 'SI olarak görev yapacak.</span><span class="sxs-lookup"><span data-stu-id="b74f7-353">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="b74f7-354">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b74f7-354">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b74f7-355">Örnek uygulamayı [Visual Studio Code](https://code.visualstudio.com/)' de çalıştırırken, *dış veya tümleşik bir Terminal*kullanın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-355">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="b74f7-356">Örneği bir `internalConsole`içinde çalıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-356">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="b74f7-357">Konsolu Visual Studio Code ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="b74f7-357">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="b74f7-358">*. Vscode/Launch. JSON* dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-358">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="b74f7-359">**.NET Core başlatma (konsol)** yapılandırmasında **konsol** girişini bulun.</span><span class="sxs-lookup"><span data-stu-id="b74f7-359">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="b74f7-360">Değeri ya da `externalTerminal` `integratedTerminal`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-360">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="b74f7-361">Giriş</span><span class="sxs-lookup"><span data-stu-id="b74f7-361">Introduction</span></span>

<span data-ttu-id="b74f7-362">Genel konak Kitaplığı <xref:Microsoft.Extensions.Hosting> ad alanında kullanılabilir ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-362">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="b74f7-363">Paket, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app) 'e dahildir (ASP.NET Core 2,1 veya üzeri). `Microsoft.Extensions.Hosting`</span><span class="sxs-lookup"><span data-stu-id="b74f7-363">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="b74f7-364"><xref:Microsoft.Extensions.Hosting.IHostedService>, kod yürütmeye yönelik giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-364"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="b74f7-365">Her <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama, [ConfigureServices içinde hizmet kaydı](#configureservices)sırasında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="b74f7-365">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="b74f7-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>, konak başlatıldığında çağrılır <xref:Microsoft.Extensions.Hosting.IHostedService> ve <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> ana bilgisayar düzgün bir şekilde kapandığında ters kayıt sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="b74f7-367">Konak ayarlama</span><span class="sxs-lookup"><span data-stu-id="b74f7-367">Set up a host</span></span>

<span data-ttu-id="b74f7-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder>, kitaplıkların ve uygulamaların Konağı başlatmak, derlemek ve çalıştırmak için kullandığı ana bileşendir:</span><span class="sxs-lookup"><span data-stu-id="b74f7-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="b74f7-369">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="b74f7-369">Options</span></span>

<span data-ttu-id="b74f7-370"><xref:Microsoft.Extensions.Hosting.HostOptions>için seçenekleri yapılandırın <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-370"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="b74f7-371">Kapatılma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="b74f7-371">Shutdown timeout</span></span>

<span data-ttu-id="b74f7-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>zaman aşımını <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="b74f7-373">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-373">The default value is five seconds.</span></span>

<span data-ttu-id="b74f7-374">' Deki `Program.Main` aşağıdaki seçenek yapılandırması, varsayılan beş saniyelik kapatılma zaman aşımını 20 saniyeye yükseltir:</span><span class="sxs-lookup"><span data-stu-id="b74f7-374">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="b74f7-375">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="b74f7-375">Default services</span></span>

<span data-ttu-id="b74f7-376">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="b74f7-376">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="b74f7-377">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-377">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="b74f7-378">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-378">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="b74f7-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="b74f7-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="b74f7-381">[Seçenekler](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-381">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="b74f7-382">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="b74f7-382">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="b74f7-383">Konak yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b74f7-383">Host configuration</span></span>

<span data-ttu-id="b74f7-384">Ana bilgisayar yapılandırması şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b74f7-384">Host configuration is created by:</span></span>

* <span data-ttu-id="b74f7-385">[İçerik kökünü](#content-root) ve ortamı <xref:Microsoft.Extensions.Hosting.IHostBuilder> ayarlamak için uzantı yöntemleri çağrılıyor. [](#environment)</span><span class="sxs-lookup"><span data-stu-id="b74f7-385">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="b74f7-386">İçindeki <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>yapılandırma sağlayıcılarından yapılandırma okunuyor.</span><span class="sxs-lookup"><span data-stu-id="b74f7-386">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="b74f7-387">Uzantı yöntemleri</span><span class="sxs-lookup"><span data-stu-id="b74f7-387">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="b74f7-388">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="b74f7-388">Application key (name)</span></span>

<span data-ttu-id="b74f7-389">[Ihostingenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-389">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="b74f7-390">Değeri açıkça ayarlamak için, [Hostdefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-390">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="b74f7-391">**Anahtar**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="b74f7-391">**Key**: applicationName</span></span>  
<span data-ttu-id="b74f7-392">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-392">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-393">**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.</span><span class="sxs-lookup"><span data-stu-id="b74f7-393">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="b74f7-394">Şunu **kullanarak ayarla**:`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="b74f7-394">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="b74f7-395">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="b74f7-395">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="b74f7-396">İçerik kökü</span><span class="sxs-lookup"><span data-stu-id="b74f7-396">Content root</span></span>

<span data-ttu-id="b74f7-397">Bu ayar, konağın içerik dosyalarını aramaya başladığı yeri belirler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-397">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="b74f7-398">**Anahtar**: contentroot</span><span class="sxs-lookup"><span data-stu-id="b74f7-398">**Key**: contentRoot</span></span>  
<span data-ttu-id="b74f7-399">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-399">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-400">**Varsayılan**: Uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-400">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="b74f7-401">Şunu **kullanarak ayarla**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="b74f7-401">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="b74f7-402">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="b74f7-402">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="b74f7-403">Yol yoksa, ana bilgisayar başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="b74f7-403">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="b74f7-404">Ortam</span><span class="sxs-lookup"><span data-stu-id="b74f7-404">Environment</span></span>

<span data-ttu-id="b74f7-405">Uygulamanın [ortamını](xref:fundamentals/environments)ayarlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-405">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="b74f7-406">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="b74f7-406">**Key**: environment</span></span>  
<span data-ttu-id="b74f7-407">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="b74f7-407">**Type**: *string*</span></span>  
<span data-ttu-id="b74f7-408">**Varsayılan**: Üretiminden</span><span class="sxs-lookup"><span data-stu-id="b74f7-408">**Default**: Production</span></span>  
<span data-ttu-id="b74f7-409">Şunu **kullanarak ayarla**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="b74f7-409">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="b74f7-410">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [isteğe bağlı ve Kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="b74f7-410">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="b74f7-411">Ortam herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-411">The environment can be set to any value.</span></span> <span data-ttu-id="b74f7-412">Çerçeve tanımlı değerler, `Development` `Staging`, ve `Production`içerir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-412">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="b74f7-413">Değerler büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-413">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="b74f7-414">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="b74f7-414">ConfigureHostConfiguration</span></span>

<span data-ttu-id="b74f7-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>konak <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> için<xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="b74f7-416">Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uygulamanın derleme sürecinde kullanılmak üzere başlatmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-416">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="b74f7-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="b74f7-418">Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-418">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="b74f7-419">Varsayılan olarak hiçbir sağlayıcı dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-419">No providers are included by default.</span></span> <span data-ttu-id="b74f7-420">Uygulamanın gerektirdiği <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>her türlü yapılandırma sağlayıcısını açık bir şekilde belirtmeniz gerekir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="b74f7-420">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="b74f7-421">Dosya yapılandırması (örneğin, *HostSettings. JSON* dosyasından).</span><span class="sxs-lookup"><span data-stu-id="b74f7-421">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="b74f7-422">Ortam değişkeni yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="b74f7-422">Environment variable configuration.</span></span>
* <span data-ttu-id="b74f7-423">Komut satırı bağımsız değişken yapılandırması.</span><span class="sxs-lookup"><span data-stu-id="b74f7-423">Command-line argument configuration.</span></span>
* <span data-ttu-id="b74f7-424">Diğer tüm gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="b74f7-424">Any other required configuration providers.</span></span>

<span data-ttu-id="b74f7-425">Konağın dosya yapılandırması, uygulamanın temel yolu `SetBasePath` ve ardından [dosya yapılandırma sağlayıcılarından](xref:fundamentals/configuration/index#file-configuration-provider)birine yapılan bir çağrı tarafından belirtilerek etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-425">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="b74f7-426">Örnek uygulama, bir JSON dosyası, *HostSettings. JSON*ve dosyanın ana bilgisayar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> yapılandırma ayarlarını kullanmak için çağrılar kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-426">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="b74f7-427">Konağın [ortam değişkeni yapılandırmasını](xref:fundamentals/configuration/index#environment-variables-configuration-provider) eklemek için konak Oluşturucu ' ya <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> çağrı yapın.</span><span class="sxs-lookup"><span data-stu-id="b74f7-427">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="b74f7-428">`AddEnvironmentVariables`Kullanıcı tanımlı isteğe bağlı bir ön eki kabul eder.</span><span class="sxs-lookup"><span data-stu-id="b74f7-428">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="b74f7-429">Örnek uygulama, öneki `PREFIX_`kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-429">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="b74f7-430">Ortam değişkenleri okurken ön ek kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-430">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="b74f7-431">Örnek uygulamanın ana bilgisayarı yapılandırıldığında, için `PREFIX_ENVIRONMENT` ortam değişkeni değeri `environment` anahtar için ana bilgisayar yapılandırma değeri olur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-431">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="b74f7-432">[Visual Studio](https://visualstudio.microsoft.com) kullanırken veya ile `dotnet run`bir uygulama çalıştırırken geliştirme sırasında, *Özellikler/launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-432">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="b74f7-433">[Visual Studio Code](https://code.visualstudio.com/), geliştirme sırasında *. vscode/Launch. JSON* dosyasında ortam değişkenleri ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-433">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="b74f7-434">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-434">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="b74f7-435">[Komut satırı yapılandırması](xref:fundamentals/configuration/index#command-line-configuration-provider) çağırarak <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-435">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="b74f7-436">Komut satırı yapılandırması, önceki yapılandırma sağlayıcıları tarafından belirtilen yapılandırmayı geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek üzere en son eklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-436">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="b74f7-437">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b74f7-437">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="b74f7-438">Ek yapılandırma [ApplicationName](#application-key-name) ve [contentroot](#content-root) anahtarlarıyla birlikte etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-438">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="b74f7-439">`HostBuilder` Kullanarak<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>örnek yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="b74f7-439">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="b74f7-440">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b74f7-440">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b74f7-441">Uygulama yapılandırması, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulama çağrısı yaparak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b74f7-441">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="b74f7-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>uygulama <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> için<xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="b74f7-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="b74f7-444">Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-444">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="b74f7-445">Tarafından <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> oluşturulan yapılandırma, sonraki işlemler ve içinde <xref:Microsoft.Extensions.Hosting.IHost.Services*>, [hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) konumunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-445">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="b74f7-446">Uygulama yapılandırması, [Configurehostconfiguration](#configurehostconfiguration)tarafından belirtilen konak yapılandırmasını otomatik olarak alır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-446">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="b74f7-447">Kullanarak <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>örnek uygulama yapılandırması:</span><span class="sxs-lookup"><span data-stu-id="b74f7-447">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="b74f7-448">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b74f7-448">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="b74f7-449">*appSettings. Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b74f7-449">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="b74f7-450">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="b74f7-450">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="b74f7-451">Ayar dosyalarını çıkış dizinine taşımak için, ayarlar dosyalarını proje dosyasında [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) olarak belirtin.</span><span class="sxs-lookup"><span data-stu-id="b74f7-451">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="b74f7-452">Örnek uygulama, JSON uygulama ayarları dosyalarını ve *HostSettings. JSON* dosyasını şu `<Content>` öğe ile taşıdıkça:</span><span class="sxs-lookup"><span data-stu-id="b74f7-452">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="b74f7-453"><xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> Ve<xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> gibi yapılandırma uzantısı yöntemleri, örneğin [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables)gibi ek NuGet paketleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-453">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="b74f7-454">Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'i kullanmadığı takdirde, bu paketlerin temel [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paketine ek olarak projeye eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-454">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="b74f7-455">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-455">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="b74f7-456">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="b74f7-456">ConfigureServices</span></span>

<span data-ttu-id="b74f7-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>uygulamanın [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına hizmet ekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b74f7-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="b74f7-459">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-459">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="b74f7-460">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-460">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="b74f7-461">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , yaşam süresi `AddHostedService` olayları, `LifetimeEventsHostedService`ve zamanlanan bir arka plan görevi `TimedHostedService`için uygulamaya bir hizmet eklemek için genişletme yöntemini kullanır:</span><span class="sxs-lookup"><span data-stu-id="b74f7-461">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="b74f7-462">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="b74f7-462">ConfigureLogging</span></span>

<span data-ttu-id="b74f7-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>Belirtilen <xref:Microsoft.Extensions.Logging.ILoggingBuilder>yapılandırmayı yapılandırmak için bir temsilci ekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="b74f7-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="b74f7-465">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="b74f7-465">UseConsoleLifetime</span></span>

<span data-ttu-id="b74f7-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>CTRL + C/sigint veya sigdönem için dinler ve kapalı <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> işlemi başlatmak için çağırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="b74f7-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>[RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="b74f7-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>Varsayılan yaşam süresi uygulamasına önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="b74f7-469">Kaydedilen son yaşam süresi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-469">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="b74f7-470">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="b74f7-470">Container configuration</span></span>

<span data-ttu-id="b74f7-471">Diğer kapsayıcıları takmayı desteklemek için ana bilgisayar bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-471">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="b74f7-472">Fabrika sağlamak, dı kapsayıcı kaydının bir parçası değildir, ancak bunun yerine somut dı kapsayıcısını oluşturmak için kullanılan bir konak iç sıdır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-472">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="b74f7-473">[Useserviceproviderfactory (ıvıceproviderfactory&lt;tcontainerbuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) , uygulamanın hizmet sağlayıcısını oluşturmak için kullanılan varsayılan fabrikası geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-473">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="b74f7-474">Özel kapsayıcı yapılandırması <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemiyle yönetilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-474">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="b74f7-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>temel ana bilgisayar API 'sinin üstünde kapsayıcıyı yapılandırmak için kesin olarak belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="b74f7-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="b74f7-477">Uygulama için bir hizmet kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="b74f7-477">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="b74f7-478">Hizmet kapsayıcısı fabrikası sağlama:</span><span class="sxs-lookup"><span data-stu-id="b74f7-478">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="b74f7-479">Fabrika 'yi kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-479">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="b74f7-480">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="b74f7-480">Extensibility</span></span>

<span data-ttu-id="b74f7-481">Konak genişletilebilirliği, üzerindeki <xref:Microsoft.Extensions.Hosting.IHostBuilder>genişletme yöntemleriyle gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-481">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="b74f7-482">Aşağıdaki örnek, bir genişletme yönteminin ' de <xref:Microsoft.Extensions.Hosting.IHostBuilder> <xref:fundamentals/host/hosted-services>gösterilen [timedhostedservice](xref:fundamentals/host/hosted-services#timed-background-tasks) örneği ile bir uygulamayı nasıl genişlettiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-482">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="b74f7-483">Uygulama, `UseHostedService` `T`geçirilen barındırılan hizmeti kaydetmek için uzantı yöntemi oluşturur:</span><span class="sxs-lookup"><span data-stu-id="b74f7-483">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="b74f7-484">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="b74f7-484">Manage the host</span></span>

<span data-ttu-id="b74f7-485">Uygulama, hizmet kapsayıcısında kayıtlı olan <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamaları başlatıp durdurmaktan sorumludur. <xref:Microsoft.Extensions.Hosting.IHost></span><span class="sxs-lookup"><span data-stu-id="b74f7-485">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="b74f7-486">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="b74f7-486">Run</span></span>

<span data-ttu-id="b74f7-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="b74f7-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="b74f7-488">RunAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-488">RunAsync</span></span>

<span data-ttu-id="b74f7-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*>uygulamayı çalıştırır ve iptal belirteci veya <xref:System.Threading.Tasks.Task> kapanışı tetiklendiğinde tamamlanmış bir döndürür:</span><span class="sxs-lookup"><span data-stu-id="b74f7-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="b74f7-490">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-490">RunConsoleAsync</span></span>

<span data-ttu-id="b74f7-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>Konsol desteği sağlar, ana bilgisayarı oluşturur ve başlatır, sonra da CTRL + C/SIGINT veya SIGDÖNEM ' in kapatılmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="b74f7-492">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-492">Start and StopAsync</span></span>

<span data-ttu-id="b74f7-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*>Konağı zaman uyumlu olarak başlatır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="b74f7-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="b74f7-495">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-495">StartAsync and StopAsync</span></span>

<span data-ttu-id="b74f7-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="b74f7-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>uygulamayı sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="b74f7-498">Waitforkapatması</span><span class="sxs-lookup"><span data-stu-id="b74f7-498">WaitForShutdown</span></span>

<span data-ttu-id="b74f7-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (örneğin, <xref:Microsoft.Extensions.Hosting.IHostLifetime>CTRL + C/sigint veya sigterim dinler) ile tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="b74f7-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>çağırır <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="b74f7-501">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="b74f7-501">WaitForShutdownAsync</span></span>

<span data-ttu-id="b74f7-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>verilen belirteç <xref:System.Threading.Tasks.Task> ve çağrılar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>aracılığıyla kapatılma tetiklendiğinde tamamlanan bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="b74f7-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="b74f7-503">Dış denetim</span><span class="sxs-lookup"><span data-stu-id="b74f7-503">External control</span></span>

<span data-ttu-id="b74f7-504">Ana bilgisayarın dış denetimine, dışarıdan çağrılabilen yöntemler kullanılarak ulaşılabilecek:</span><span class="sxs-lookup"><span data-stu-id="b74f7-504">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="b74f7-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>, ' nin <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>başlangıcında çağrılır ve devam etmeden önce tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="b74f7-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="b74f7-506">Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-506">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="b74f7-507">Ihostingenvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="b74f7-507">IHostingEnvironment interface</span></span>

<span data-ttu-id="b74f7-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>uygulamanın barındırma ortamı hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="b74f7-509">Özelliklerini ve uzantı yöntemlerini kullanmak üzere <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> sağlamak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="b74f7-509">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="b74f7-510">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b74f7-510">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="b74f7-511">Iapplicationlifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="b74f7-511">IApplicationLifetime interface</span></span>

<span data-ttu-id="b74f7-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime>düzgün kapanma istekleri de dahil olmak üzere başlatma sonrası ve kapanma etkinliklerine izin verir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="b74f7-513">Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan yöntemleri kaydetmek <xref:System.Action> için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-513">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="b74f7-514">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="b74f7-514">Cancellation Token</span></span> | <span data-ttu-id="b74f7-515">Tetiklendiği zaman&#8230;</span><span class="sxs-lookup"><span data-stu-id="b74f7-515">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="b74f7-516">Konak tam olarak başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="b74f7-516">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="b74f7-517">Ana bilgisayar düzgün kapanma işlemini tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="b74f7-517">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="b74f7-518">Tüm isteklerin işlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-518">All requests should be processed.</span></span> <span data-ttu-id="b74f7-519">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-519">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="b74f7-520">Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="b74f7-520">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="b74f7-521">İstekler hala işliyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="b74f7-521">Requests may still be processing.</span></span> <span data-ttu-id="b74f7-522">Bu olay tamamlanana kadar kapalı bloklar.</span><span class="sxs-lookup"><span data-stu-id="b74f7-522">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="b74f7-523">Constructor- <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> hizmeti herhangi bir sınıfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b74f7-523">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="b74f7-524">[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , olayları kaydetmek için bir `LifetimeEventsHostedService` sınıfa (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) ekleme oluşturucusunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="b74f7-524">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="b74f7-525">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="b74f7-525">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="b74f7-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>uygulamanın sonlandırılmasını ister.</span><span class="sxs-lookup"><span data-stu-id="b74f7-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="b74f7-527">Aşağıdaki sınıf, `Shutdown` sınıfın <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için kullanır:</span><span class="sxs-lookup"><span data-stu-id="b74f7-527">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="b74f7-528">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b74f7-528">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
