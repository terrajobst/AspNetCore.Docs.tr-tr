---
title: ASP.NET Core durum denetimleri
author: guardrex
description: Uygulamalar ve veritabanları gibi ASP.NET Core altyapısı için sistem durumu denetimlerini ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 11/03/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: c7cf1c432d2186f0e2f9f5082e8a2229d8a5ef8f
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463025"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="13d41-103">ASP.NET Core durum denetimleri</span><span class="sxs-lookup"><span data-stu-id="13d41-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="13d41-104">[Luke Latham](https://github.com/guardrex) ve [Glenn CONDRON](https://github.com/glennc) tarafından</span><span class="sxs-lookup"><span data-stu-id="13d41-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="13d41-105">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="13d41-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="13d41-106">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="13d41-107">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="13d41-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="13d41-108">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="13d41-109">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="13d41-110">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="13d41-111">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="13d41-112">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="13d41-113">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13d41-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="13d41-114">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="13d41-115">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="13d41-116">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="13d41-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13d41-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="13d41-117">Prerequisites</span></span>

<span data-ttu-id="13d41-118">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="13d41-119">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="13d41-120">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="13d41-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="13d41-121">[Microsoft. AspNetCore. Diagnostics. Healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine ASP.NET Core uygulamaları için örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="13d41-122">Entity Framework Core kullanarak sistem durumu denetimleri gerçekleştirmek için, [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="13d41-123">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="13d41-124">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="13d41-125">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="13d41-126">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="13d41-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="13d41-127">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="13d41-128">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="13d41-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="13d41-129">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="13d41-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="13d41-130">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="13d41-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="13d41-131">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="13d41-132">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="13d41-133">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="13d41-134">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="13d41-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="13d41-135">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="13d41-135">Basic health probe</span></span>

<span data-ttu-id="13d41-136">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="13d41-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="13d41-137">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="13d41-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="13d41-138">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="13d41-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="13d41-139">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="13d41-140">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="13d41-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="13d41-141">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-142">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="13d41-143">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

<span data-ttu-id="13d41-144">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="13d41-145">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="13d41-145">Docker example</span></span>

<span data-ttu-id="13d41-146">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="13d41-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="13d41-147">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="13d41-147">Create health checks</span></span>

<span data-ttu-id="13d41-148">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="13d41-149"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="13d41-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="13d41-150">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="13d41-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="13d41-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="13d41-152">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="13d41-153">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="13d41-154">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="13d41-155">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="13d41-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a><span data-ttu-id="13d41-156">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="13d41-156">Register health check services</span></span>

<span data-ttu-id="13d41-157">`ExampleHealthCheck` türü, `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="13d41-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="13d41-158">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="13d41-159">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="13d41-160">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="13d41-161">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="13d41-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="13d41-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="13d41-163">Aşağıdaki örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="13d41-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="13d41-164">Sistem durumu denetimleri yönlendirmeyi kullanma</span><span class="sxs-lookup"><span data-stu-id="13d41-164">Use Health Checks Routing</span></span>

<span data-ttu-id="13d41-165">`Startup.Configure`, uç nokta URL 'SI veya göreli yol ile Endpoint Builder üzerinde `MapHealthChecks` çağırın:</span><span class="sxs-lookup"><span data-stu-id="13d41-165">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="13d41-166">Konak gerektir</span><span class="sxs-lookup"><span data-stu-id="13d41-166">Require host</span></span>

<span data-ttu-id="13d41-167">Sistem durumu denetimi uç noktası için bir veya daha fazla izin verilen Konakları belirtmek üzere `RequireHost` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="13d41-167">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="13d41-168">Konaklar, puni kodu yerine Unicode olmalıdır ve bir bağlantı noktası içerebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-168">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="13d41-169">Bir koleksiyon sağlanmazsa, herhangi bir konak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-169">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="13d41-170">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="13d41-170">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="13d41-171">Yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="13d41-171">Require authorization</span></span>

<span data-ttu-id="13d41-172">Sistem durumu denetimi istek uç noktasında yetkilendirme ara yazılımını çalıştırmak için `RequireAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="13d41-172">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="13d41-173">`RequireAuthorization` aşırı yükleme bir veya daha fazla yetkilendirme ilkesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="13d41-173">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="13d41-174">Bir ilke sağlanmazsa, varsayılan yetkilendirme ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-174">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="13d41-175">Kaynaklar Arası İstekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-175">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="13d41-176">Bir tarayıcıdan el ile sistem durumu denetimleri gerçekleştirmek yaygın kullanım senaryosu olmamasına karşın, CORS ara yazılımı `RequireCors` sistem durumu denetimleri uç noktalarında çağırarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-176">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="13d41-177">`RequireCors` aşırı yüklemesi CORS ilke Oluşturucu temsilcisini (`CorsPolicyBuilder`) veya bir ilke adını kabul eder.</span><span class="sxs-lookup"><span data-stu-id="13d41-177">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="13d41-178">Bir ilke sağlanmazsa varsayılan CORS ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-178">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="13d41-179">Daha fazla bilgi için bkz. <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="13d41-179">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="13d41-180">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="13d41-180">Health check options</span></span>

<span data-ttu-id="13d41-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="13d41-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="13d41-182">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-182">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="13d41-183">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-183">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="13d41-184">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="13d41-184">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="13d41-185">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-185">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="13d41-186">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-186">Filter health checks</span></span>

<span data-ttu-id="13d41-187">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-187">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="13d41-188">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-188">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="13d41-189">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür. :</span><span class="sxs-lookup"><span data-stu-id="13d41-189">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="13d41-190">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13d41-190">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="13d41-191">`Startup.Configure`, `Predicate` ' Bar ' sistem durumu denetimini filtreler.</span><span class="sxs-lookup"><span data-stu-id="13d41-191">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="13d41-192">Yalnızca Foo ve baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="13d41-192">Only Foo and Baz execute.:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="13d41-193">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-193">Customize the HTTP status code</span></span>

<span data-ttu-id="13d41-194">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-194">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="13d41-195">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="13d41-195">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="13d41-196">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="13d41-196">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="13d41-197">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-197">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="13d41-198">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="13d41-198">Suppress cache headers</span></span>

<span data-ttu-id="13d41-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="13d41-200">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="13d41-200">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="13d41-201">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-201">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="13d41-202">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-202">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="13d41-203">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-203">Customize output</span></span>

<span data-ttu-id="13d41-204"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-204">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="13d41-205">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="13d41-206">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="13d41-206">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="13d41-207">Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-207">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

<span data-ttu-id="13d41-208">Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür.</span><span class="sxs-lookup"><span data-stu-id="13d41-208">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="13d41-209">Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-209">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="13d41-210">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="13d41-210">Database probe</span></span>

<span data-ttu-id="13d41-211">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-211">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="13d41-212">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-212">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="13d41-213">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="13d41-213">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="13d41-214">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="13d41-214">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="13d41-215">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-215">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="13d41-216">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="13d41-216">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="13d41-217">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="13d41-217">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="13d41-218">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="13d41-218">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="13d41-219">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-219">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="13d41-220">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-220">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="13d41-221">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-221">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="13d41-222">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-222">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-223">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-223">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-224">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="13d41-224">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="13d41-225">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-225">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="13d41-226">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="13d41-227">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="13d41-227">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="13d41-228">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="13d41-228">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="13d41-229">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="13d41-229">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="13d41-230">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-230">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="13d41-231">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-231">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="13d41-232">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="13d41-232">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="13d41-233">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-233">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="13d41-234">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-234">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="13d41-235">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="13d41-235">By default:</span></span>

* <span data-ttu-id="13d41-236">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-236">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="13d41-237">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-237">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="13d41-238">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-238">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="13d41-239">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-239">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-240">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="13d41-240">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="13d41-241">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="13d41-241">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="13d41-242">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="13d41-242">If the database exists, delete it.</span></span>

<span data-ttu-id="13d41-243">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-243">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="13d41-244">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-244">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="13d41-245">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="13d41-245">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="13d41-246">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-246">Trigger the sample app to create the database.</span></span> <span data-ttu-id="13d41-247">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-247">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="13d41-248">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-248">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="13d41-249">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-249">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="13d41-250">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-250">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="13d41-251">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-251">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="13d41-252">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-252">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="13d41-253">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-253">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="13d41-254">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-254">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="13d41-255">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="13d41-255">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="13d41-256">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="13d41-256">Separate readiness and liveness probes</span></span>

<span data-ttu-id="13d41-257">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="13d41-257">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="13d41-258">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="13d41-258">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="13d41-259">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="13d41-259">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="13d41-260">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="13d41-260">The app is functioning and responding to requests.</span></span> <span data-ttu-id="13d41-261">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-261">This state is the app's *liveness*.</span></span>

<span data-ttu-id="13d41-262">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-262">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="13d41-263">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-263">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="13d41-264">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-264">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="13d41-265">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-265">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="13d41-266">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-266">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="13d41-267">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-267">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="13d41-268">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-268">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="13d41-269">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-269">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="13d41-270">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-270">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-271">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-271">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="13d41-272">Örnek uygulamada, sistem durumu denetimi uç noktaları şu konumda oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="13d41-272">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="13d41-273">Hazırlık denetimi için `/health/ready`.</span><span class="sxs-lookup"><span data-stu-id="13d41-273">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="13d41-274">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-274">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="13d41-275">libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="13d41-275">`/health/live` for the liveness check.</span></span> <span data-ttu-id="13d41-276">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="13d41-276">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="13d41-277">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="13d41-277">In the following example code:</span></span>

* <span data-ttu-id="13d41-278">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-278">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="13d41-279">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="13d41-279">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

<span data-ttu-id="13d41-280">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-280">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="13d41-281">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="13d41-281">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="13d41-282">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="13d41-282">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="13d41-283">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-283">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="13d41-284">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13d41-284">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="13d41-285">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="13d41-285">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="13d41-286">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="13d41-286">Kubernetes example</span></span>

<span data-ttu-id="13d41-287">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-287">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="13d41-288">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-288">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="13d41-289">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-289">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="13d41-290">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="13d41-290">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="13d41-291">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="13d41-291">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="13d41-292">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="13d41-292">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="13d41-293">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-293">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="13d41-294">uygulama, belirli bir bellek eşiğine (örnek uygulamada 1 GB) daha fazlasını kullanıyorsa `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="13d41-294">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="13d41-295"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-295">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="13d41-296">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-296">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-297">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-297">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="13d41-298">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-298">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="13d41-299">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="13d41-299">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="13d41-300">Örnek uygulamada (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-300">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="13d41-301">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-301">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="13d41-302">Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-302">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="13d41-303">`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-303">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="13d41-304">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-304">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="13d41-305">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="13d41-306">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="13d41-307">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-307">Filter by port</span></span>

<span data-ttu-id="13d41-308">Belirtilen bağlantı noktasına sistem durumu denetim isteklerini kısıtlamak için bir bağlantı noktası belirten bir URL düzeniyle `MapHealthChecks` `RequireHost` çağırın.</span><span class="sxs-lookup"><span data-stu-id="13d41-308">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="13d41-309">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-309">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="13d41-310">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-310">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="13d41-311">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-311">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="13d41-312">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="13d41-312">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="13d41-313">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-313">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="13d41-314">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="13d41-314">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="13d41-315">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-315">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-316">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-316">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="13d41-317">Örnek uygulamada, `Startup.Configure` uç noktasındaki `RequireHost` çağrısı, yapılandırmadan yönetim bağlantı noktasını belirtir:</span><span class="sxs-lookup"><span data-stu-id="13d41-317">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="13d41-318">Uç noktalar `Startup.Configure`örnek uygulamada oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-318">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="13d41-319">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="13d41-319">In the following example code:</span></span>

* <span data-ttu-id="13d41-320">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-320">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="13d41-321">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="13d41-321">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> <span data-ttu-id="13d41-322">Yönetim bağlantı noktasını kodda açıkça ayarlayarak, örnek uygulamada *Launchsettings. JSON* dosyasını oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-322">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="13d41-323"><xref:Microsoft.Extensions.Hosting.HostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> için bir çağrı ekleyin ve uygulamanın yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-323">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="13d41-324">*ManagementPortStartup.cs*`Configure` `RequireHost`ile yönetim bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="13d41-324">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="13d41-325">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13d41-325">*Program.cs*:</span></span>
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> <span data-ttu-id="13d41-326">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="13d41-326">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="13d41-327">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-327">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="13d41-328">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="13d41-328">Distribute a health check library</span></span>

<span data-ttu-id="13d41-329">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="13d41-329">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="13d41-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="13d41-330">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="13d41-331">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-331">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="13d41-332">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="13d41-332">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="13d41-333">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-333">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="13d41-334">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="13d41-334">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="13d41-335">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="13d41-335">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="13d41-336">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="13d41-336">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="13d41-337">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="13d41-337">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="13d41-338">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-338">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="13d41-339">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-339">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="13d41-340">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="13d41-340">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="13d41-341">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="13d41-341">health check name (`name`).</span></span> <span data-ttu-id="13d41-342">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-342">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="13d41-343">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="13d41-343">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="13d41-344">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="13d41-344">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="13d41-345">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-345">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="13d41-346">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="13d41-346">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="13d41-347">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="13d41-347">The default is `null`.</span></span> <span data-ttu-id="13d41-348">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-348">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="13d41-349">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="13d41-349">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="13d41-350">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="13d41-350">Health Check Publisher</span></span>

<span data-ttu-id="13d41-351">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-351">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="13d41-352">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-352">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="13d41-353"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="13d41-353">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="13d41-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="13d41-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="13d41-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="13d41-356">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="13d41-356">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="13d41-357">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-357">The default value is five seconds.</span></span>
* <span data-ttu-id="13d41-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="13d41-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="13d41-359">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-359">The default value is 30 seconds.</span></span>
* <span data-ttu-id="13d41-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="13d41-361">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-361">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="13d41-362">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-362">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="13d41-363">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="13d41-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="13d41-364">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-364">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="13d41-365">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-365">The default value is 30 seconds.</span></span>

<span data-ttu-id="13d41-366">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-366">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="13d41-367">Sistem durumu denetimi durumu her denetim için günlük düzeyinde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="13d41-367">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="13d41-368">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="13d41-368">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="13d41-369">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="13d41-369">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="13d41-370">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-370">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="13d41-371"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="13d41-371">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="13d41-372">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-372">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="13d41-373">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-373">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="13d41-374">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="13d41-374">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="13d41-375">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-375">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="13d41-376">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="13d41-376">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

<span data-ttu-id="13d41-377">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="13d41-377">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="13d41-378">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="13d41-378">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="13d41-379">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-379">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="13d41-380">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="13d41-380">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="13d41-381">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-381">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="13d41-382">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-382">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="13d41-383">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-383">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="13d41-384">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-384">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="13d41-385">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-385">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="13d41-386">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13d41-386">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="13d41-387">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-387">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="13d41-388">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-388">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="13d41-389">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="13d41-389">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13d41-390">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="13d41-390">Prerequisites</span></span>

<span data-ttu-id="13d41-391">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-391">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="13d41-392">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-392">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="13d41-393">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="13d41-393">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="13d41-394">Microsoft. aspnetcore [. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Aspnetcore. Diagnostics. healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-394">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="13d41-395">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-395">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="13d41-396">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-396">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="13d41-397">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-397">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="13d41-398">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="13d41-398">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="13d41-399">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-399">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="13d41-400">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="13d41-400">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="13d41-401">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="13d41-401">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="13d41-402">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="13d41-402">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="13d41-403">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-403">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="13d41-404">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-404">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="13d41-405">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-405">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="13d41-406">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="13d41-406">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="13d41-407">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="13d41-407">Basic health probe</span></span>

<span data-ttu-id="13d41-408">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="13d41-408">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="13d41-409">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="13d41-409">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="13d41-410">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="13d41-410">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="13d41-411">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-411">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="13d41-412">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="13d41-412">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="13d41-413">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-413">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-414">`Startup.Configure`istek işleme ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> olan sistem durumu denetimleri ara yazılımı için bir uç nokta ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-414">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="13d41-415">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-415">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

<span data-ttu-id="13d41-416">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-416">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="13d41-417">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="13d41-417">Docker example</span></span>

<span data-ttu-id="13d41-418">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="13d41-418">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="13d41-419">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="13d41-419">Create health checks</span></span>

<span data-ttu-id="13d41-420">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13d41-420">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="13d41-421"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="13d41-421">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="13d41-422">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="13d41-422">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="13d41-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="13d41-424">Örnek sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="13d41-424">Example health check</span></span>

<span data-ttu-id="13d41-425">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-425">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="13d41-426">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-426">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="13d41-427">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-427">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="13d41-428">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="13d41-428">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="13d41-429">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="13d41-429">Register health check services</span></span>

<span data-ttu-id="13d41-430">`ExampleHealthCheck` türü <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>ile `Startup.ConfigureServices` sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="13d41-430">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="13d41-431">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-431">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="13d41-432">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-432">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="13d41-433">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-433">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="13d41-434">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="13d41-434">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="13d41-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="13d41-436">Aşağıdaki `Startup.ConfigureServices` örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="13d41-436">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="13d41-437">Sistem durumu denetimleri ara yazılımı kullan</span><span class="sxs-lookup"><span data-stu-id="13d41-437">Use Health Checks Middleware</span></span>

<span data-ttu-id="13d41-438">`Startup.Configure`, işlem hattının uç nokta URL 'SI veya göreli yolu ile <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="13d41-438">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="13d41-439">Durum denetimlerinin belirli bir bağlantı noktasını dinlemesi gerekiyorsa, bağlantı noktasını ayarlamak için <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> aşırı yüklemesini kullanın ( [bağlantı noktasına göre filtrele](#filter-by-port) bölümünde anlatılmıştır):</span><span class="sxs-lookup"><span data-stu-id="13d41-439">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="13d41-440">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="13d41-440">Health check options</span></span>

<span data-ttu-id="13d41-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="13d41-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="13d41-442">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-442">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="13d41-443">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-443">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="13d41-444">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="13d41-444">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="13d41-445">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-445">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="13d41-446">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-446">Filter health checks</span></span>

<span data-ttu-id="13d41-447">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-447">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="13d41-448">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-448">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="13d41-449">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür. :</span><span class="sxs-lookup"><span data-stu-id="13d41-449">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () =>
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () =>
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="13d41-450">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-450">Customize the HTTP status code</span></span>

<span data-ttu-id="13d41-451">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-451">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="13d41-452">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="13d41-452">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="13d41-453">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="13d41-453">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="13d41-454">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-454">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="13d41-455">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="13d41-455">Suppress cache headers</span></span>

<span data-ttu-id="13d41-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="13d41-457">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="13d41-457">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="13d41-458">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-458">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="13d41-459">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-459">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="13d41-460">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="13d41-460">Customize output</span></span>

<span data-ttu-id="13d41-461"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-461">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="13d41-462">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="13d41-462">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="13d41-463">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="13d41-463">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="13d41-464">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="13d41-464">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="13d41-465">Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-465">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

<span data-ttu-id="13d41-466">Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür.</span><span class="sxs-lookup"><span data-stu-id="13d41-466">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="13d41-467">Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-467">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="13d41-468">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="13d41-468">Database probe</span></span>

<span data-ttu-id="13d41-469">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-469">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="13d41-470">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-470">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="13d41-471">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="13d41-471">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="13d41-472">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="13d41-472">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="13d41-473">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-473">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="13d41-474">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="13d41-474">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="13d41-475">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="13d41-475">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="13d41-476">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="13d41-476">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="13d41-477">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-477">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="13d41-478">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-478">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="13d41-479">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-479">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="13d41-480">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-480">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-481">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-481">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-482">Uygulama işleme işlem hattının `Startup.Configure`içindeki sistem durumu denetimleri ara yazılımını çağır:</span><span class="sxs-lookup"><span data-stu-id="13d41-482">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="13d41-483">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-483">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="13d41-484">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-484">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="13d41-485">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="13d41-485">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="13d41-486">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="13d41-486">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="13d41-487">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="13d41-487">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="13d41-488">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-488">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="13d41-489">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-489">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="13d41-490">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="13d41-490">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="13d41-491">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-491">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="13d41-492">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-492">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="13d41-493">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="13d41-493">By default:</span></span>

* <span data-ttu-id="13d41-494">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-494">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="13d41-495">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-495">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="13d41-496">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-496">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="13d41-497">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-497">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-498">Örnek uygulamada, `UseHealthChecks` sistem durumu denetimleri ara yazılımını `Startup.Configure`ekler.</span><span class="sxs-lookup"><span data-stu-id="13d41-498">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="13d41-499">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="13d41-499">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="13d41-500">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="13d41-500">If the database exists, delete it.</span></span>

<span data-ttu-id="13d41-501">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-501">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="13d41-502">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-502">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="13d41-503">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="13d41-503">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="13d41-504">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-504">Trigger the sample app to create the database.</span></span> <span data-ttu-id="13d41-505">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-505">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="13d41-506">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-506">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="13d41-507">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-507">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="13d41-508">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-508">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="13d41-509">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="13d41-509">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="13d41-510">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-510">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="13d41-511">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="13d41-511">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="13d41-512">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-512">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="13d41-513">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="13d41-513">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="13d41-514">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="13d41-514">Separate readiness and liveness probes</span></span>

<span data-ttu-id="13d41-515">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="13d41-515">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="13d41-516">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="13d41-516">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="13d41-517">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="13d41-517">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="13d41-518">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="13d41-518">The app is functioning and responding to requests.</span></span> <span data-ttu-id="13d41-519">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-519">This state is the app's *liveness*.</span></span>

<span data-ttu-id="13d41-520">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-520">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="13d41-521">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-521">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="13d41-522">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="13d41-522">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="13d41-523">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-523">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="13d41-524">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-524">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="13d41-525">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-525">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="13d41-526">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-526">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="13d41-527">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-527">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="13d41-528">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-528">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13d41-529">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="13d41-529">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="13d41-530">Örnek uygulamada, sistem durumu denetimi uç noktaları, hazır olma denetimi için `/health/ready` oluşturulur ve libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="13d41-530">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="13d41-531">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="13d41-531">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="13d41-532">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="13d41-532">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

<span data-ttu-id="13d41-533">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-533">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="13d41-534">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="13d41-534">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="13d41-535">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="13d41-535">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="13d41-536">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-536">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="13d41-537">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13d41-537">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="13d41-538">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="13d41-538">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="13d41-539">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="13d41-539">Kubernetes example</span></span>

<span data-ttu-id="13d41-540">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-540">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="13d41-541">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-541">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="13d41-542">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-542">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="13d41-543">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="13d41-543">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="13d41-544">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="13d41-544">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="13d41-545">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="13d41-545">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="13d41-546">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-546">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="13d41-547">uygulama, belirli bir bellek eşiğini (örnek uygulamada 1 GB) kullanıyorsa, sağlıksız bir durumu raporlar `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="13d41-547">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="13d41-548"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-548">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="13d41-549">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-549">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-550">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-550">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="13d41-551">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-551">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="13d41-552">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="13d41-552">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="13d41-553">Örnek uygulamada (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-553">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="13d41-554">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="13d41-554">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="13d41-555">Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:</span><span class="sxs-lookup"><span data-stu-id="13d41-555">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="13d41-556">`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-556">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="13d41-557">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-557">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="13d41-558">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-558">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="13d41-559">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-559">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="13d41-560">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="13d41-560">Filter by port</span></span>

<span data-ttu-id="13d41-561"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> bir bağlantı noktası ile çağırmak, belirtilen bağlantı noktasına durum denetimi isteklerini kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-561">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="13d41-562">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-562">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="13d41-563">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-563">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="13d41-564">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-564">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="13d41-565">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="13d41-565">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="13d41-566">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13d41-566">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="13d41-567">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="13d41-567">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="13d41-568">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="13d41-568">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="13d41-569"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrısı, yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="13d41-569">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="13d41-570">Kod içinde açıkça URL 'Leri ve yönetim bağlantı noktasını ayarlayarak *Launchsettings. JSON* dosyasını örnek uygulamada oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="13d41-570">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="13d41-571"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> için bir çağrı ekleyin ve uygulamanın normal yanıt uç noktasını ve yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-571">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="13d41-572"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrıldığında, yönetim bağlantı noktasını açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="13d41-572">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="13d41-573">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="13d41-573">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="13d41-574">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="13d41-574">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="13d41-575">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="13d41-575">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="13d41-576">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="13d41-576">Distribute a health check library</span></span>

<span data-ttu-id="13d41-577">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="13d41-577">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="13d41-578"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="13d41-578">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="13d41-579">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-579">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="13d41-580">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="13d41-580">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="13d41-581">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-581">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="13d41-582">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="13d41-582">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="13d41-583">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="13d41-583">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public class ExampleHealthCheck : IHealthCheck
   {
       private readonly string _data1;
       private readonly int? _data2;

       public ExampleHealthCheck(string data1, int? data2)
       {
           _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
           _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
       }

       public async Task<HealthCheckResult> CheckHealthAsync(
           HealthCheckContext context, CancellationToken cancellationToken)
       {
           try
           {
               return HealthCheckResult.Healthy();
           }
           catch (AccessViolationException ex)
           {
               return new HealthCheckResult(
                   context.Registration.FailureStatus,
                   description: "An access violation occurred during the check.",
                   exception: ex,
                   data: null);
           }
       }
   }
   ```

1. <span data-ttu-id="13d41-584">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="13d41-584">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="13d41-585">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="13d41-585">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="13d41-586">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13d41-586">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="13d41-587">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="13d41-587">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="13d41-588">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="13d41-588">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="13d41-589">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="13d41-589">health check name (`name`).</span></span> <span data-ttu-id="13d41-590">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-590">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="13d41-591">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="13d41-591">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="13d41-592">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="13d41-592">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="13d41-593">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13d41-593">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="13d41-594">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="13d41-594">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="13d41-595">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="13d41-595">The default is `null`.</span></span> <span data-ttu-id="13d41-596">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-596">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="13d41-597">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="13d41-597">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="13d41-598">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="13d41-598">Health Check Publisher</span></span>

<span data-ttu-id="13d41-599">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-599">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="13d41-600">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-600">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="13d41-601"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="13d41-601">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="13d41-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="13d41-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="13d41-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="13d41-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="13d41-604">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="13d41-604">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="13d41-605">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-605">The default value is five seconds.</span></span>
* <span data-ttu-id="13d41-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="13d41-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="13d41-607">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-607">The default value is 30 seconds.</span></span>
* <span data-ttu-id="13d41-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="13d41-609">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="13d41-609">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="13d41-610">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13d41-610">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="13d41-611">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="13d41-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="13d41-612">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-612">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="13d41-613">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="13d41-613">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="13d41-614">ASP.NET Core 2,2 sürümünde, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> ayarı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulama tarafından kabul edilemez; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13d41-614">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="13d41-615">Bu sorun ASP.NET Core 3,0 ' de giderilmiştir.</span><span class="sxs-lookup"><span data-stu-id="13d41-615">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="13d41-616">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="13d41-616">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="13d41-617">Durum denetimi durumu her denetim için şu şekilde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="13d41-617">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="13d41-618">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="13d41-618">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="13d41-619">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="13d41-619">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="13d41-620">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="13d41-620">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="13d41-621"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="13d41-621">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="13d41-622">Aşağıdaki geçici çözüm, uygulamaya bir veya daha fazla barındırılan hizmet zaten eklendiyse, hizmet kapsayıcısına <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> bir örnek eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="13d41-622">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="13d41-623">Bu geçici çözüm ASP.NET Core 3,0 ' de gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="13d41-623">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly =
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService),
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

> [!NOTE]
> <span data-ttu-id="13d41-624">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="13d41-624">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="13d41-625">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="13d41-625">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="13d41-626">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="13d41-626">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="13d41-627">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="13d41-627">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="13d41-628">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="13d41-628">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="13d41-629">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="13d41-629">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
