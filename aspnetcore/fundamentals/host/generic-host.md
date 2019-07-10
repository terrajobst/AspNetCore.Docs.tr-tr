---
title: .NET genel ana bilgisayar
author: tdykstra
description: Uygulama başlatma ve ömür yönetimi için sorumlu olduğu .NET Core genel konak hakkında öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d787559eaecd6d4d6cfe01e37baf28774a90c5c3
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724420"
---
# <a name="net-generic-host"></a><span data-ttu-id="64cab-103">.NET genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="64cab-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64cab-104">Bu makalede .NET Core genel ana bilgisayar sunar (<xref:Microsoft.Extensions.Hosting.HostBuilder>) nasıl kullanılacağını sağlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="64cab-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="64cab-105">Bir konak nedir?</span><span class="sxs-lookup"><span data-stu-id="64cab-105">What's a host?</span></span>

<span data-ttu-id="64cab-106">A *konak* gibi bir uygulamanın kaynakları sarmalayan bir nesnedir:</span><span class="sxs-lookup"><span data-stu-id="64cab-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="64cab-107">Bağımlılık ekleme (dı)</span><span class="sxs-lookup"><span data-stu-id="64cab-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="64cab-108">Günlüğe Kaydetme</span><span class="sxs-lookup"><span data-stu-id="64cab-108">Logging</span></span>
* <span data-ttu-id="64cab-109">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="64cab-109">Configuration</span></span>
* <span data-ttu-id="64cab-110">`IHostedService` Uygulamaları</span><span class="sxs-lookup"><span data-stu-id="64cab-110">`IHostedService` implementations</span></span>

<span data-ttu-id="64cab-111">Bir ana bilgisayar başlatıldığında çağırdığı `IHostedService.StartAsync` her uygulaması üzerinde <xref:Microsoft.Extensions.Hosting.IHostedService> DI kapsayıcısında bulduğu.</span><span class="sxs-lookup"><span data-stu-id="64cab-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="64cab-112">Bir web uygulaması, aşağıdakilerden birini `IHostedService` uygulamaları başlatan bir web hizmeti olan bir [HTTP sunucusu uygulamasını](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="64cab-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="64cab-113">Tüm bağımlı kaynakları uygulamanın bir nesnesinde de dahil olmak üzere ana nedeni ömrü yönetimi,: uygulama başlatma ve normal şekilde kapatılmasını üzerinde denetim.</span><span class="sxs-lookup"><span data-stu-id="64cab-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="64cab-114">ASP.NET Core 3.0,'dan önceki sürümlerinde [Web ana bilgisayarı](xref:fundamentals/host/web-host) HTTP iş yükleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="64cab-115">Web ana bilgisayarı artık web apps için önerilir ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.</span><span class="sxs-lookup"><span data-stu-id="64cab-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="64cab-116">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="64cab-116">Set up a host</span></span>

<span data-ttu-id="64cab-117">Ana bilgisayar genellikle, yerleşik ve çalışan kod tarafından yapılandırılan `Program` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="64cab-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="64cab-118">`Main` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="64cab-118">The `Main` method:</span></span>

* <span data-ttu-id="64cab-119">Çağrıları bir `CreateHostBuilder` oluşturmak ve bir oluşturucu nesnesini yapılandırmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="64cab-120">Çağrıları `Build` ve `Run` Oluşturucu nesnesini yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="64cab-121">İşte *Program.cs* tek bir HTTP olmayan bir iş yükü için kod `IHostedService` uygulama DI kapsayıcıya eklendi.</span><span class="sxs-lookup"><span data-stu-id="64cab-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="64cab-122">Bir HTTP iş yükü için `Main` yöntemdir aynı ancak `CreateHostBuilder` çağrıları `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="64cab-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="64cab-123">Uygulama, Entity Framework Core kullanıyorsa, ad veya imzası değişmez `CreateHostBuilder` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="64cab-124">[Entity Framework Core Araçları](/ef/core/miscellaneous/cli/) bulmayı beklediğinize bir `CreateHostBuilder` uygulamayı başlatmadan konak yapılandırır yöntemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="64cab-125">Daha fazla bilgi için [tasarım zamanında DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="64cab-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="64cab-126">Varsayılan Oluşturucu ayarları</span><span class="sxs-lookup"><span data-stu-id="64cab-126">Default builder settings</span></span> 

<span data-ttu-id="64cab-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="64cab-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="64cab-128">İçerik kök tarafından döndürülen yol ayarlar <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-128">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="64cab-129">Yükleri yapılandırmasından barındırın:</span><span class="sxs-lookup"><span data-stu-id="64cab-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="64cab-130">Ortam değişkenleri "İle DOTNET_" öneki.</span><span class="sxs-lookup"><span data-stu-id="64cab-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="64cab-131">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-131">Command-line arguments.</span></span>
* <span data-ttu-id="64cab-132">Yükleri uygulama yapılandırmasından:</span><span class="sxs-lookup"><span data-stu-id="64cab-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="64cab-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="64cab-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="64cab-134">*appSettings. {Ortamı} .json*.</span><span class="sxs-lookup"><span data-stu-id="64cab-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="64cab-135">[Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` ortam.</span><span class="sxs-lookup"><span data-stu-id="64cab-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="64cab-136">Ortam değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-136">Environment variables.</span></span>
  * <span data-ttu-id="64cab-137">Komut satırı bağımsız değişkenleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-137">Command-line arguments.</span></span>
* <span data-ttu-id="64cab-138">Aşağıdaki ekler [günlüğü](xref:fundamentals/logging/index) sağlayıcıları:</span><span class="sxs-lookup"><span data-stu-id="64cab-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="64cab-139">Konsol</span><span class="sxs-lookup"><span data-stu-id="64cab-139">Console</span></span>
  * <span data-ttu-id="64cab-140">Hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="64cab-140">Debug</span></span>
  * <span data-ttu-id="64cab-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="64cab-141">EventSource</span></span>
  * <span data-ttu-id="64cab-142">(Yalnızca Windows üzerinde çalışırken) olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="64cab-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="64cab-143">Sağlar [kapsam doğrulama](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) geliştirme ortamı olduğunda.</span><span class="sxs-lookup"><span data-stu-id="64cab-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="64cab-144">`ConfigureWebHostDefaults` Yöntemi:</span><span class="sxs-lookup"><span data-stu-id="64cab-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="64cab-145">Yükler, ortam değişkenleri "İle ASPNETCORE_" önekli yapılandırmasından barındırın.</span><span class="sxs-lookup"><span data-stu-id="64cab-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="64cab-146">Kümeleri [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve uygulamanın barındırma yapılandırma sağlayıcıları kullanarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="64cab-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="64cab-147">Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="64cab-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="64cab-148">Ekler [konak filtreleme ara yazılım](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="64cab-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="64cab-149">Ekler [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) varsa ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="64cab-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="64cab-150">IIS tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-150">Enables IIS integration.</span></span> <span data-ttu-id="64cab-151">IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="64cab-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="64cab-152">[Tüm uygulama türleri için ayarları](#settings-for-all-app-types) ve [web apps için ayarlar](#settings-for-web-apps) bu makalenin sonraki bölümlerine varsayılan oluşturucu ayarlarının nasıl geçersiz kılınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="64cab-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="64cab-153">Framework tarafından sağlanan hizmetleri</span><span class="sxs-lookup"><span data-stu-id="64cab-153">Framework-provided services</span></span>

<span data-ttu-id="64cab-154">Otomatik olarak kaydedilen hizmetleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="64cab-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="64cab-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="64cab-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="64cab-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="64cab-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="64cab-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="64cab-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="64cab-158">Framework tarafından sağlanan tüm hizmetlerin listesi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="64cab-158">For a list of all framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="64cab-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="64cab-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="64cab-160">Ekleme <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmet oturum kapatma sonrası başlatma ve normal görevlerini işlemek için herhangi bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="64cab-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="64cab-161">Arabirim üç özellikleri, uygulama başlangıç ve uygulama durdurma olay işleyicisi yöntemleri kaydetmek için kullanılan iptal belirteçleridir.</span><span class="sxs-lookup"><span data-stu-id="64cab-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="64cab-162">Ayrıca arabirimin içerdiği bir `StopApplication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="64cab-163">Aşağıdaki örnek, bir `IHostedService` kaydeder uygulama `IApplicationLifetime` olayları:</span><span class="sxs-lookup"><span data-stu-id="64cab-163">The following example is an `IHostedService` implementation that registers the `IApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="64cab-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="64cab-164">IHostLifetime</span></span>

<span data-ttu-id="64cab-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> Uygulama, konak başlatıldığında ve ne zaman durdurur denetler.</span><span class="sxs-lookup"><span data-stu-id="64cab-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="64cab-166">Kaydedilen son uygulaması kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-166">The last implementation registered is used.</span></span>

<span data-ttu-id="64cab-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan değer `IHostLifetime` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="64cab-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="64cab-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="64cab-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="64cab-169">CTRL + C/SIGINT veya SIGTERM ve aramalar için dinler <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="64cab-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="64cab-170">Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="64cab-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="64cab-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="64cab-171">IHostEnvironment</span></span>

<span data-ttu-id="64cab-172">Ekleme <xref:Microsoft.Extensions.Hosting.IHostEnvironment> hizmet aşağıdakiler hakkında bilgi almak için bir sınıf içinde:</span><span class="sxs-lookup"><span data-stu-id="64cab-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="64cab-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="64cab-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="64cab-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="64cab-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="64cab-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="64cab-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="64cab-176">Web apps uygulama `IWebHostEnvironment` devralan bir arayüzü `IHostEnvironment` ve ekler:</span><span class="sxs-lookup"><span data-stu-id="64cab-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="64cab-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="64cab-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="64cab-178">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="64cab-178">Host configuration</span></span>

<span data-ttu-id="64cab-179">Ana bilgisayar yapılandırma özellikleri için kullanılan <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulaması.</span><span class="sxs-lookup"><span data-stu-id="64cab-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="64cab-180">Ana bilgisayar yapılandırması kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="64cab-181">Sonra `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="64cab-182">Ana bilgisayar yapılandırması eklemek için arama <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> üzerinde `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64cab-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="64cab-183">`ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="64cab-184">Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="64cab-185">Ortam değişkeni sağlayıcı önekiyle `DOTNET_` ve komut satırı değişkenleri tarafından CreateDefaultBuilder dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="64cab-186">Web uygulamaları, ortam değişkeni sağlayıcı önekiyle `ASPNETCORE_` eklenir.</span><span class="sxs-lookup"><span data-stu-id="64cab-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="64cab-187">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="64cab-188">Örneğin, ortam değişken değeri `ASPNETCORE_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="64cab-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="64cab-189">Aşağıdaki örnek, ana bilgisayar yapılandırması oluşturur:</span><span class="sxs-lookup"><span data-stu-id="64cab-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="64cab-190">Uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="64cab-190">App configuration</span></span>

<span data-ttu-id="64cab-191">Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64cab-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="64cab-192">`ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="64cab-193">Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="64cab-194">Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve hizmet olarak sunduğu DI.</span><span class="sxs-lookup"><span data-stu-id="64cab-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="64cab-195">Ana bilgisayar yapılandırması için uygulama yapılandırma de eklenir.</span><span class="sxs-lookup"><span data-stu-id="64cab-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="64cab-196">Daha fazla bilgi için [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="64cab-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="64cab-197">Tüm uygulama türleri için ayarları</span><span class="sxs-lookup"><span data-stu-id="64cab-197">Settings for all app types</span></span>

<span data-ttu-id="64cab-198">Bu bölümde, hem HTTP hem de HTTP olmayan iş yükleri için geçerli bir ana bilgisayar ayarları listelenir.</span><span class="sxs-lookup"><span data-stu-id="64cab-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="64cab-199">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerini olabilir bir `DOTNET_` veya `ASPNETCORE_` önek.</span><span class="sxs-lookup"><span data-stu-id="64cab-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

### <a name="applicationname"></a><span data-ttu-id="64cab-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="64cab-200">ApplicationName</span></span>

<span data-ttu-id="64cab-201">[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="64cab-202">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="64cab-202">**Key**: applicationName</span></span>  
<span data-ttu-id="64cab-203">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-203">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-204">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="64cab-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="64cab-205">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="64cab-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="64cab-206">Bu değeri ayarlamak için ortam değişkenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="64cab-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="64cab-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="64cab-207">ContentRootPath</span></span>

<span data-ttu-id="64cab-208">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özellik, ana içerik dosyalarını aramaya başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="64cab-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="64cab-209">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="64cab-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="64cab-210">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="64cab-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="64cab-211">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-211">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-212">**Varsayılan**: Uygulama derleme bulunduğu klasör.</span><span class="sxs-lookup"><span data-stu-id="64cab-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="64cab-213">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="64cab-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="64cab-214">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseContentRoot` üzerinde `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="64cab-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a><span data-ttu-id="64cab-215">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="64cab-215">EnvironmentName</span></span>

<span data-ttu-id="64cab-216">[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-216">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="64cab-217">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="64cab-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="64cab-218">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="64cab-218">Values aren't case sensitive.</span></span>

<span data-ttu-id="64cab-219">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="64cab-219">**Key**: environment</span></span>  
<span data-ttu-id="64cab-220">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-220">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-221">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="64cab-221">**Default**: Production</span></span>  
<span data-ttu-id="64cab-222">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="64cab-222">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="64cab-223">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseEnvironment` üzerinde `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="64cab-223">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="64cab-224">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="64cab-224">ShutdownTimeout</span></span>

<span data-ttu-id="64cab-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) zaman aşımı'için ayarlar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="64cab-226">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="64cab-226">The default value is five seconds.</span></span>  <span data-ttu-id="64cab-227">Zaman aşımı dönemi sırasında konak:</span><span class="sxs-lookup"><span data-stu-id="64cab-227">During the timeout period, the host:</span></span>

* <span data-ttu-id="64cab-228">Tetikleyiciler [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="64cab-228">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="64cab-229">Barındırılan hizmetler, durdurmak için başarısız olan hizmetlerin hatalarını günlüğe kaydetme durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="64cab-229">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="64cab-230">Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur.</span><span class="sxs-lookup"><span data-stu-id="64cab-230">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="64cab-231">İşlem tamamlandı olmasanız bile hizmetleri durdurun.</span><span class="sxs-lookup"><span data-stu-id="64cab-231">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="64cab-232">Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.</span><span class="sxs-lookup"><span data-stu-id="64cab-232">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="64cab-233">**Anahtar**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="64cab-233">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="64cab-234">**Tür**: *int*</span><span class="sxs-lookup"><span data-stu-id="64cab-234">**Type**: *int*</span></span>  
<span data-ttu-id="64cab-235">**Varsayılan**: 5 saniye **ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="64cab-235">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="64cab-236">Bu değeri ayarlamak için ortam değişkenini kullanmak veya yapılandırma `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="64cab-236">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="64cab-237">Aşağıdaki örnekte, 20 saniye olarak zaman aşımı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="64cab-237">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="64cab-238">Web apps için ayarlar</span><span class="sxs-lookup"><span data-stu-id="64cab-238">Settings for web apps</span></span>

<span data-ttu-id="64cab-239">Bazı konak ayarları, yalnızca HTTP iş yükleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="64cab-239">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="64cab-240">Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerini olabilir bir `DOTNET_` veya `ASPNETCORE_` önek.</span><span class="sxs-lookup"><span data-stu-id="64cab-240">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="64cab-241">Uzantı yöntemleri `IWebHostBuilder` bu ayarlar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-241">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="64cab-242">Genişletme yöntemleri çağırmak nasıl gösteren kod örnekleri varsayar `webBuilder` örneğidir `IWebHostBuilder`, aşağıdaki örnekte olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="64cab-242">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="64cab-243">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="64cab-243">CaptureStartupErrors</span></span>

<span data-ttu-id="64cab-244">Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar.</span><span class="sxs-lookup"><span data-stu-id="64cab-244">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="64cab-245">Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.</span><span class="sxs-lookup"><span data-stu-id="64cab-245">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="64cab-246">**Anahtar**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="64cab-246">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="64cab-247">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="64cab-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="64cab-248">**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.</span><span class="sxs-lookup"><span data-stu-id="64cab-248">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="64cab-249">**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="64cab-249">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="64cab-250">Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="64cab-250">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="64cab-251">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="64cab-251">DetailedErrors</span></span>

<span data-ttu-id="64cab-252">Etkin olduğunda ya da ortam `Development`, uygulamanın ayrıntılı hataları yakalar.</span><span class="sxs-lookup"><span data-stu-id="64cab-252">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="64cab-253">**Anahtar**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="64cab-253">**Key**: detailedErrors</span></span>  
<span data-ttu-id="64cab-254">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="64cab-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="64cab-255">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="64cab-255">**Default**: false</span></span>  
<span data-ttu-id="64cab-256">**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="64cab-256">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="64cab-257">Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="64cab-257">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="64cab-258">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="64cab-258">HostingStartupAssemblies</span></span>

<span data-ttu-id="64cab-259">Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="64cab-259">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="64cab-260">Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="64cab-260">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="64cab-261">Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="64cab-261">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="64cab-262">**Anahtar**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="64cab-262">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="64cab-263">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-263">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-264">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="64cab-264">**Default**: Empty string</span></span>  
<span data-ttu-id="64cab-265">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="64cab-265">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="64cab-266">Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="64cab-266">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="64cab-267">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="64cab-267">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="64cab-268">Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.</span><span class="sxs-lookup"><span data-stu-id="64cab-268">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="64cab-269">**Anahtar**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="64cab-269">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="64cab-270">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-270">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-271">**Varsayılan**: Boş dize</span><span class="sxs-lookup"><span data-stu-id="64cab-271">**Default**: Empty string</span></span>  
<span data-ttu-id="64cab-272">**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="64cab-272">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="64cab-273">Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="64cab-273">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="httpsport"></a><span data-ttu-id="64cab-274">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="64cab-274">HTTPS_Port</span></span>

<span data-ttu-id="64cab-275">HTTPS bağlantı noktasına yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="64cab-275">The HTTPS redirect port.</span></span> <span data-ttu-id="64cab-276">Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="64cab-276">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="64cab-277">**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="64cab-277">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="64cab-278">**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="64cab-278">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="64cab-279">Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="64cab-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="64cab-280">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="64cab-280">PreferHostingUrls</span></span>

<span data-ttu-id="64cab-281">Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `IWebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="64cab-281">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="64cab-282">**Anahtar**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="64cab-282">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="64cab-283">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="64cab-283">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="64cab-284">**Varsayılan**: true</span><span class="sxs-lookup"><span data-stu-id="64cab-284">**Default**: true</span></span>  
<span data-ttu-id="64cab-285">**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="64cab-285">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="64cab-286">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="64cab-286">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="64cab-287">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="64cab-287">PreventHostingStartup</span></span>

<span data-ttu-id="64cab-288">Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="64cab-288">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="64cab-289">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="64cab-289">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="64cab-290">**Anahtar**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="64cab-290">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="64cab-291">**Tür**: *bool* (`true` veya `1`)</span><span class="sxs-lookup"><span data-stu-id="64cab-291">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="64cab-292">**Varsayılan**: false</span><span class="sxs-lookup"><span data-stu-id="64cab-292">**Default**: false</span></span>  
<span data-ttu-id="64cab-293">**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="64cab-293">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="64cab-294">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="64cab-294">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="64cab-295">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="64cab-295">StartupAssembly</span></span>

<span data-ttu-id="64cab-296">Aramak için derleme `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="64cab-296">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="64cab-297">**Anahtar**: startupAssembly **türü**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-297">**Key**: startupAssembly **Type**: *string*</span></span>  
<span data-ttu-id="64cab-298">**Varsayılan**: Uygulamanın derleme</span><span class="sxs-lookup"><span data-stu-id="64cab-298">**Default**: The app's assembly</span></span>  
<span data-ttu-id="64cab-299">**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="64cab-299">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="64cab-300">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="64cab-300">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="64cab-301">`UseStartup` bir derleme adı alabilir (`string`) veya bir tür (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="64cab-301">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="64cab-302">Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="64cab-302">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="64cab-303">URL'ler</span><span class="sxs-lookup"><span data-stu-id="64cab-303">URLs</span></span>

<span data-ttu-id="64cab-304">IP adresleri veya bağlantı noktası ve sunucu üzerinde istekleri dinlemesi gereken protokolleri konak adresleri noktalı virgülle ayrılmış listesi.</span><span class="sxs-lookup"><span data-stu-id="64cab-304">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="64cab-305">Örneğin: `http://localhost:123`</span><span class="sxs-lookup"><span data-stu-id="64cab-305">For example, `http://localhost:123`.</span></span> <span data-ttu-id="64cab-306">Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="64cab-306">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="64cab-307">Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="64cab-307">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="64cab-308">Desteklenen biçimler sunucular arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="64cab-308">Supported formats vary among servers.</span></span>

<span data-ttu-id="64cab-309">**Anahtar**: URL'ler</span><span class="sxs-lookup"><span data-stu-id="64cab-309">**Key**: urls</span></span>  
<span data-ttu-id="64cab-310">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-310">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-311">**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001` 
 **ortam değişkeni**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="64cab-311">**Default**: `http://localhost:5000` and `https://localhost:5001`
**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="64cab-312">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="64cab-312">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="64cab-313">Kestrel'i kendi API uç nokta yapılandırması vardır.</span><span class="sxs-lookup"><span data-stu-id="64cab-313">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="64cab-314">Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="64cab-314">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="64cab-315">WebRoot</span><span class="sxs-lookup"><span data-stu-id="64cab-315">WebRoot</span></span>

<span data-ttu-id="64cab-316">Uygulamanın statik varlıklar göreli yolu.</span><span class="sxs-lookup"><span data-stu-id="64cab-316">The relative path to the app's static assets.</span></span>

<span data-ttu-id="64cab-317">**Anahtar**: webroot</span><span class="sxs-lookup"><span data-stu-id="64cab-317">**Key**: webroot</span></span>  
<span data-ttu-id="64cab-318">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-318">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-319">**Varsayılan**: *(Kök içerik) / wwwroot*, yol varsa.</span><span class="sxs-lookup"><span data-stu-id="64cab-319">**Default**: *(Content Root)/wwwroot*, if the path exists.</span></span> <span data-ttu-id="64cab-320">Yol mevcut değilse İşlemsiz dosya sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-320">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="64cab-321">**Ortam değişkeni**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="64cab-321">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="64cab-322">Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="64cab-322">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="64cab-323">Konak yaşam süresini yönetme</span><span class="sxs-lookup"><span data-stu-id="64cab-323">Manage the host lifetime</span></span>

<span data-ttu-id="64cab-324">Yerleşik üzerinde yöntemleri çağırmak <xref:Microsoft.Extensions.Hosting.IHost> uygulamasını başlatın ve uygulamayı durdurun.</span><span class="sxs-lookup"><span data-stu-id="64cab-324">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="64cab-325">Bu yöntemler tüm etkileyen <xref:Microsoft.Extensions.Hosting.IHostedService> service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="64cab-325">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="64cab-326">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="64cab-326">Run</span></span>

<span data-ttu-id="64cab-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="64cab-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="64cab-328">RunAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-328">RunAsync</span></span>

<span data-ttu-id="64cab-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="64cab-330">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-330">RunConsoleAsync</span></span>

<span data-ttu-id="64cab-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve kapatmak Ctrl + C/SIGINT veya SIGTERM bekler.</span><span class="sxs-lookup"><span data-stu-id="64cab-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="64cab-332">Başlat</span><span class="sxs-lookup"><span data-stu-id="64cab-332">Start</span></span>

<span data-ttu-id="64cab-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="64cab-334">StartAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-334">StartAsync</span></span>

<span data-ttu-id="64cab-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> konak başlatıldığında ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="64cab-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır `StartAsync`, devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="64cab-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="64cab-337">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-337">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="64cab-338">StopAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-338">StopAsync</span></span>

<span data-ttu-id="64cab-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="64cab-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="64cab-340">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="64cab-340">WaitForShutdown</span></span>

<span data-ttu-id="64cab-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> kapatma IHostLifetime tarafından gibi Ctrl + C/SIGINT veya SIGTERM aracılığıyla tetiklenen kadar çağıran iş parçacığını engeller.</span><span class="sxs-lookup"><span data-stu-id="64cab-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="64cab-342">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-342">WaitForShutdownAsync</span></span>

<span data-ttu-id="64cab-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir <xref:System.Threading.Tasks.Task> kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="64cab-344">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="64cab-344">External control</span></span>

<span data-ttu-id="64cab-345">Konak ömrü doğrudan denetimi harici olarak çağrılabilir yöntemleri kullanılarak gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="64cab-345">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="64cab-346">ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın.</span><span class="sxs-lookup"><span data-stu-id="64cab-346">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="64cab-347">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="64cab-347">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="64cab-348">Bu makalede, ASP.NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>), HTTP isteklerini mıdl'ye işleme uygulamaları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-348">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="64cab-349">Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır.</span><span class="sxs-lookup"><span data-stu-id="64cab-349">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="64cab-350">Mesajlaşma, arka plan görevleri ve diğer genel ana bilgisayar yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri avantajından temel HTTP olmayan iş yükleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-350">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="64cab-351">Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir.</span><span class="sxs-lookup"><span data-stu-id="64cab-351">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="64cab-352">Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="64cab-352">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="64cab-353">Genel konak Web ana bilgisayarı gelecekteki bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="64cab-353">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="64cab-354">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64cab-354">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="64cab-355">Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*.</span><span class="sxs-lookup"><span data-stu-id="64cab-355">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="64cab-356">Örneği çalıştırma bir `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="64cab-356">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="64cab-357">Visual Studio Code'da konsol ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="64cab-357">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="64cab-358">Açık *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="64cab-358">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="64cab-359">İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi.</span><span class="sxs-lookup"><span data-stu-id="64cab-359">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="64cab-360">Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="64cab-360">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="64cab-361">Giriş</span><span class="sxs-lookup"><span data-stu-id="64cab-361">Introduction</span></span>

<span data-ttu-id="64cab-362">Genel Host kitaplığı kullanılabilir <xref:Microsoft.Extensions.Hosting> ad alanı tarafından sağlanan ve [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket.</span><span class="sxs-lookup"><span data-stu-id="64cab-362">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="64cab-363">`Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="64cab-363">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="64cab-364"><xref:Microsoft.Extensions.Hosting.IHostedService> kod yürütme için giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="64cab-364"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="64cab-365">Her <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="64cab-365">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="64cab-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> Her çağrılır <xref:Microsoft.Extensions.Hosting.IHostedService> konak başlatıldığında ve <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="64cab-367">Bir ana bilgisayar kümesi</span><span class="sxs-lookup"><span data-stu-id="64cab-367">Set up a host</span></span>

<span data-ttu-id="64cab-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşen şöyledir:</span><span class="sxs-lookup"><span data-stu-id="64cab-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="64cab-369">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="64cab-369">Options</span></span>

<span data-ttu-id="64cab-370"><xref:Microsoft.Extensions.Hosting.HostOptions> seçeneklerini yapılandırmak <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="64cab-370"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="64cab-371">Kapatma zaman aşımı</span><span class="sxs-lookup"><span data-stu-id="64cab-371">Shutdown timeout</span></span>

<span data-ttu-id="64cab-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> zaman aşımı'için ayarlar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="64cab-373">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="64cab-373">The default value is five seconds.</span></span>

<span data-ttu-id="64cab-374">Aşağıdaki seçeneği yapılandırmada `Program.Main` varsayılan beş ikinci kapatma zaman aşımı süresi 20 saniye artırır:</span><span class="sxs-lookup"><span data-stu-id="64cab-374">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="64cab-375">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="64cab-375">Default services</span></span>

<span data-ttu-id="64cab-376">Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="64cab-376">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="64cab-377">[Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="64cab-377">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="64cab-378">[Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="64cab-378">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="64cab-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="64cab-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="64cab-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="64cab-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="64cab-381">[Seçenekleri](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="64cab-381">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="64cab-382">[Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="64cab-382">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="64cab-383">Ana Bilgisayar Yapılandırması</span><span class="sxs-lookup"><span data-stu-id="64cab-383">Host configuration</span></span>

<span data-ttu-id="64cab-384">Ana Bilgisayar Yapılandırması tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="64cab-384">Host configuration is created by:</span></span>

* <span data-ttu-id="64cab-385">Uzantı metotları çağırma <xref:Microsoft.Extensions.Hosting.IHostBuilder> ayarlanacak [içerik kök](#content-root) ve [ortam](#environment).</span><span class="sxs-lookup"><span data-stu-id="64cab-385">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="64cab-386">Yapılandırma sağlayıcıları okuma yapılandırmasından <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-386">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="64cab-387">Genişletme yöntemleri</span><span class="sxs-lookup"><span data-stu-id="64cab-387">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="64cab-388">Uygulama anahtarı (ad)</span><span class="sxs-lookup"><span data-stu-id="64cab-388">Application key (name)</span></span>

<span data-ttu-id="64cab-389">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-389">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="64cab-390">Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="64cab-390">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="64cab-391">**Anahtar**: applicationName</span><span class="sxs-lookup"><span data-stu-id="64cab-391">**Key**: applicationName</span></span>  
<span data-ttu-id="64cab-392">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-392">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-393">**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="64cab-393">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="64cab-394">**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="64cab-394">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="64cab-395">**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="64cab-395">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="64cab-396">İçerik kök</span><span class="sxs-lookup"><span data-stu-id="64cab-396">Content root</span></span>

<span data-ttu-id="64cab-397">Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.</span><span class="sxs-lookup"><span data-stu-id="64cab-397">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="64cab-398">**Anahtar**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="64cab-398">**Key**: contentRoot</span></span>  
<span data-ttu-id="64cab-399">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-399">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-400">**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.</span><span class="sxs-lookup"><span data-stu-id="64cab-400">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="64cab-401">**Kullanılarak ayarlanan**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="64cab-401">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="64cab-402">**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="64cab-402">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="64cab-403">Yol mevcut değilse, konak başlatmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="64cab-403">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="64cab-404">Ortam</span><span class="sxs-lookup"><span data-stu-id="64cab-404">Environment</span></span>

<span data-ttu-id="64cab-405">Uygulamanın ayarlar [ortam](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="64cab-405">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="64cab-406">**Anahtar**: ortam</span><span class="sxs-lookup"><span data-stu-id="64cab-406">**Key**: environment</span></span>  
<span data-ttu-id="64cab-407">**Tür**: *dize*</span><span class="sxs-lookup"><span data-stu-id="64cab-407">**Type**: *string*</span></span>  
<span data-ttu-id="64cab-408">**Varsayılan**: Üretim</span><span class="sxs-lookup"><span data-stu-id="64cab-408">**Default**: Production</span></span>  
<span data-ttu-id="64cab-409">**Kullanılarak ayarlanan**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="64cab-409">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="64cab-410">**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="64cab-410">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="64cab-411">Ortamı, herhangi bir değere ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-411">The environment can be set to any value.</span></span> <span data-ttu-id="64cab-412">Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`.</span><span class="sxs-lookup"><span data-stu-id="64cab-412">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="64cab-413">Değerler büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="64cab-413">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="64cab-414">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="64cab-414">ConfigureHostConfiguration</span></span>

<span data-ttu-id="64cab-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> konak için.</span><span class="sxs-lookup"><span data-stu-id="64cab-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="64cab-416">Konak yapılandırmasının başlatmak için kullanılan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> kullanılmak üzere uygulamanın derleme işlemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-416">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="64cab-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="64cab-418">Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-418">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="64cab-419">Yok sağlayıcıları varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-419">No providers are included by default.</span></span> <span data-ttu-id="64cab-420">Uygulama gerekiyor, herhangi bir yapılandırma sağlayıcısı açıkça belirtmeniz gerekir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>de dahil olmak üzere:</span><span class="sxs-lookup"><span data-stu-id="64cab-420">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="64cab-421">Dosya yapılandırması (örneğin, bir *hostsettings.json* dosyası).</span><span class="sxs-lookup"><span data-stu-id="64cab-421">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="64cab-422">Ortam değişkeni yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="64cab-422">Environment variable configuration.</span></span>
* <span data-ttu-id="64cab-423">Komut satırı bağımsız yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="64cab-423">Command-line argument configuration.</span></span>
* <span data-ttu-id="64cab-424">Herhangi diğer gerekli yapılandırma sağlayıcıları.</span><span class="sxs-lookup"><span data-stu-id="64cab-424">Any other required configuration providers.</span></span>

<span data-ttu-id="64cab-425">Ana bilgisayar dosya yapılandırması ile uygulamanın taban yolu belirterek etkin `SetBasePath` birine bir çağrı tarafından izlenen [yapılandırma sağlayıcıları dosya](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="64cab-425">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="64cab-426">Bir JSON dosyası örnek uygulamanın kullandığı *hostsettings.json*ve aramalar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> dosyanın ana bilgisayar yapılandırma ayarlarını kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="64cab-426">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="64cab-427">Eklenecek [ortam değişkeni yapılandırma](xref:fundamentals/configuration/index#environment-variables-configuration-provider) ana bilgisayarının çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> konak oluşturucu üzerinde.</span><span class="sxs-lookup"><span data-stu-id="64cab-427">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="64cab-428">`AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder.</span><span class="sxs-lookup"><span data-stu-id="64cab-428">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="64cab-429">Bir önek örnek uygulamanın kullandığı `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="64cab-429">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="64cab-430">Ön ek ortam değişkenlerini okunduğunda kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-430">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="64cab-431">Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="64cab-431">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="64cab-432">Kullanırken, geliştirme sırasında [Visual Studio](https://visualstudio.microsoft.com) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="64cab-432">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="64cab-433">İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya.</span><span class="sxs-lookup"><span data-stu-id="64cab-433">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="64cab-434">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="64cab-434">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="64cab-435">[Komut satırı yapılandırma](xref:fundamentals/configuration/index#command-line-configuration-provider) çağrılarak eklenir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-435">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="64cab-436">Komut satırı yapılandırma önceki yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek için son eklenir.</span><span class="sxs-lookup"><span data-stu-id="64cab-436">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="64cab-437">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="64cab-437">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="64cab-438">Ek yapılandırma ile sağlanabilir [applicationName](#application-key-name) ve [contentRoot](#content-root) anahtarları.</span><span class="sxs-lookup"><span data-stu-id="64cab-438">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="64cab-439">Örnek `HostBuilder` yapılandırmayla <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="64cab-439">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="64cab-440">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="64cab-440">ConfigureAppConfiguration</span></span>

<span data-ttu-id="64cab-441">Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulaması.</span><span class="sxs-lookup"><span data-stu-id="64cab-441">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="64cab-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> uygulama için.</span><span class="sxs-lookup"><span data-stu-id="64cab-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="64cab-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="64cab-444">Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.</span><span class="sxs-lookup"><span data-stu-id="64cab-444">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="64cab-445">Tarafından oluşturulan yapılandırmayı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve buna <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-445">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="64cab-446">Uygulama Yapılandırması tarafından sağlanan ana bilgisayar yapılandırması otomatik olarak aldığı [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="64cab-446">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="64cab-447">Örnek uygulama yapılandırmasını kullanma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="64cab-447">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="64cab-448">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="64cab-448">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="64cab-449">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="64cab-449">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="64cab-450">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="64cab-450">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="64cab-451">Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki.</span><span class="sxs-lookup"><span data-stu-id="64cab-451">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="64cab-452">Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki `<Content>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="64cab-452">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="64cab-453">Yapılandırma genişletme yöntemleri gibi <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ek NuGet paketleri gibi gereken [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="64cab-453">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="64cab-454">Uygulamanın kullandığı sürece [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bu paketleri çekirdek ek olarak projeye eklenmelidir [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paket.</span><span class="sxs-lookup"><span data-stu-id="64cab-454">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="64cab-455">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="64cab-455">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="64cab-456">Createservicereplicalisteners()</span><span class="sxs-lookup"><span data-stu-id="64cab-456">ConfigureServices</span></span>

<span data-ttu-id="64cab-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> uygulamanın Hizmetleri ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="64cab-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="64cab-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="64cab-459">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="64cab-459">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="64cab-460">Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="64cab-460">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="64cab-461">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:</span><span class="sxs-lookup"><span data-stu-id="64cab-461">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="64cab-462">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="64cab-462">ConfigureLogging</span></span>

<span data-ttu-id="64cab-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> sağlanan yapılandırmak için bir temsilci ekler <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="64cab-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="64cab-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="64cab-465">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="64cab-465">UseConsoleLifetime</span></span>

<span data-ttu-id="64cab-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> CTRL + C/SIGINT veya SIGTERM ve aramalar için dinler <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="64cab-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="64cab-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="64cab-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="64cab-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan yaşam süresi uygulaması önceden kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="64cab-469">Kaydedilen son ömrü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64cab-469">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="64cab-470">Kapsayıcı yapılandırması</span><span class="sxs-lookup"><span data-stu-id="64cab-470">Container configuration</span></span>

<span data-ttu-id="64cab-471">Diğer kapsayıcılarda takma desteklemek için konak alabilen bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="64cab-471">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="64cab-472">Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir.</span><span class="sxs-lookup"><span data-stu-id="64cab-472">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="64cab-473">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="64cab-473">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="64cab-474">Özel kapsayıcı yapılandırması tarafından yönetilir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="64cab-474">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="64cab-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="64cab-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> Ek sonuçlar birden çok kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="64cab-477">App service kapsayıcısı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="64cab-477">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="64cab-478">Bir hizmet kapsayıcı üreteci sağlar:</span><span class="sxs-lookup"><span data-stu-id="64cab-478">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="64cab-479">Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="64cab-479">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="64cab-480">Genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="64cab-480">Extensibility</span></span>

<span data-ttu-id="64cab-481">Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="64cab-481">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="64cab-482">Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasıyla [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) gösterilen örnek içinde <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="64cab-482">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="64cab-483">Bir uygulama oluşturur `UseHostedService` genişletme yöntemi, barındırılan hizmeti kaydetmek için geçirilen `T`:</span><span class="sxs-lookup"><span data-stu-id="64cab-483">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="64cab-484">Konağı yönetme</span><span class="sxs-lookup"><span data-stu-id="64cab-484">Manage the host</span></span>

<span data-ttu-id="64cab-485"><xref:Microsoft.Extensions.Hosting.IHost> Uygulamasıdır ve durdurmaktan sorumludur <xref:Microsoft.Extensions.Hosting.IHostedService> service kapsayıcısında kayıtlı uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="64cab-485">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="64cab-486">Çalıştır</span><span class="sxs-lookup"><span data-stu-id="64cab-486">Run</span></span>

<span data-ttu-id="64cab-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:</span><span class="sxs-lookup"><span data-stu-id="64cab-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="64cab-488">RunAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-488">RunAsync</span></span>

<span data-ttu-id="64cab-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar:</span><span class="sxs-lookup"><span data-stu-id="64cab-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="64cab-490">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-490">RunConsoleAsync</span></span>

<span data-ttu-id="64cab-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve kapatmak Ctrl + C/SIGINT veya SIGTERM bekler.</span><span class="sxs-lookup"><span data-stu-id="64cab-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="64cab-492">Başlangıç ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-492">Start and StopAsync</span></span>

<span data-ttu-id="64cab-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="64cab-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="64cab-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="64cab-495">StartAsync ve StopAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-495">StartAsync and StopAsync</span></span>

<span data-ttu-id="64cab-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="64cab-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="64cab-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı durdurur.</span><span class="sxs-lookup"><span data-stu-id="64cab-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="64cab-498">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="64cab-498">WaitForShutdown</span></span>

<span data-ttu-id="64cab-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> aracılığıyla tetiklenen <xref:Microsoft.Extensions.Hosting.IHostLifetime>, gibi <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (Ctrl + C/SIGINT veya SIGTERM dinler).</span><span class="sxs-lookup"><span data-stu-id="64cab-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="64cab-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> çağrıları <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="64cab-501">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="64cab-501">WaitForShutdownAsync</span></span>

<span data-ttu-id="64cab-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir <xref:System.Threading.Tasks.Task> kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="64cab-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="64cab-503">Dış denetimi</span><span class="sxs-lookup"><span data-stu-id="64cab-503">External control</span></span>

<span data-ttu-id="64cab-504">Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:</span><span class="sxs-lookup"><span data-stu-id="64cab-504">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="64cab-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, devam etmeden önce işlemi tamamlanana kadar bekler.</span><span class="sxs-lookup"><span data-stu-id="64cab-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="64cab-506">Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64cab-506">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="64cab-507">IHostingEnvironment arabirimi</span><span class="sxs-lookup"><span data-stu-id="64cab-507">IHostingEnvironment interface</span></span>

<span data-ttu-id="64cab-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uygulama hakkındaki bilgileri barındırma ortamı sağlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="64cab-509">Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> genişletme yöntemleri ve özellikleri kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="64cab-509">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="64cab-510">Daha fazla bilgi için bkz. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="64cab-510">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="64cab-511">IApplicationLifetime arabirimi</span><span class="sxs-lookup"><span data-stu-id="64cab-511">IApplicationLifetime interface</span></span>

<span data-ttu-id="64cab-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> normal şekilde kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar.</span><span class="sxs-lookup"><span data-stu-id="64cab-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="64cab-513">Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini <xref:System.Action> başlatma ve kapatma olayları tanımlayan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="64cab-513">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="64cab-514">İptal belirteci</span><span class="sxs-lookup"><span data-stu-id="64cab-514">Cancellation Token</span></span> | <span data-ttu-id="64cab-515">Ne zaman tetiklenir&#8230;</span><span class="sxs-lookup"><span data-stu-id="64cab-515">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="64cab-516">Ana bilgisayarın tam olarak başlatılmış.</span><span class="sxs-lookup"><span data-stu-id="64cab-516">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="64cab-517">Konak bir şekilde kapatılmasını tamamlıyor.</span><span class="sxs-lookup"><span data-stu-id="64cab-517">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="64cab-518">Tüm isteklerin işlenmesi.</span><span class="sxs-lookup"><span data-stu-id="64cab-518">All requests should be processed.</span></span> <span data-ttu-id="64cab-519">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="64cab-519">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="64cab-520">Konak bir şekilde kapatılmasını gerçekleştiriyor.</span><span class="sxs-lookup"><span data-stu-id="64cab-520">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="64cab-521">İstekler hala işleniyor.</span><span class="sxs-lookup"><span data-stu-id="64cab-521">Requests may still be processing.</span></span> <span data-ttu-id="64cab-522">Bu olay tamamlanıncaya kadar kapatma engeller.</span><span class="sxs-lookup"><span data-stu-id="64cab-522">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="64cab-523">Oluşturucu Ekle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> herhangi bir sınıf içinde hizmet.</span><span class="sxs-lookup"><span data-stu-id="64cab-523">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="64cab-524">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) olaylarını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="64cab-524">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="64cab-525">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="64cab-525">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="64cab-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> uygulamanın sonlandırma ister.</span><span class="sxs-lookup"><span data-stu-id="64cab-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="64cab-527">Aşağıdaki sınıf kullandığı <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="64cab-527">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="64cab-528">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="64cab-528">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
