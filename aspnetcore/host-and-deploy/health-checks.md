---
title: ASP.NET Core durum denetimleri
author: guardrex
description: Uygulamalar ve veritabanları gibi ASP.NET Core altyapısı için sistem durumu denetimlerini ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 11/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 4a4606a58178018f0d71d467d4c8b6c9982c09dc
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116000"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="1b022-103">ASP.NET Core durum denetimleri</span><span class="sxs-lookup"><span data-stu-id="1b022-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="1b022-104">[Luke Latham](https://github.com/guardrex) ve [Glenn CONDRON](https://github.com/glennc) tarafından</span><span class="sxs-lookup"><span data-stu-id="1b022-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1b022-105">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="1b022-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="1b022-106">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="1b022-107">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1b022-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="1b022-108">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="1b022-109">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="1b022-110">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="1b022-111">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="1b022-112">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="1b022-113">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1b022-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1b022-114">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="1b022-115">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="1b022-116">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="1b022-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b022-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="1b022-117">Prerequisites</span></span>

<span data-ttu-id="1b022-118">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="1b022-119">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="1b022-120">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="1b022-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="1b022-121">[Microsoft. AspNetCore. Diagnostics. Healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine ASP.NET Core uygulamaları için örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="1b022-122">Entity Framework Core kullanarak sistem durumu denetimleri gerçekleştirmek için, [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="1b022-123">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="1b022-124">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="1b022-125">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="1b022-126">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="1b022-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="1b022-127">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="1b022-128">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1b022-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="1b022-129">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1b022-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="1b022-130">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1b022-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="1b022-131">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="1b022-132">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="1b022-133">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="1b022-134">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="1b022-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="1b022-135">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="1b022-135">Basic health probe</span></span>

<span data-ttu-id="1b022-136">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="1b022-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="1b022-137">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="1b022-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="1b022-138">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="1b022-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="1b022-139">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="1b022-140">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="1b022-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="1b022-141">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-142">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="1b022-143">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="1b022-144">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="1b022-145">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="1b022-145">Docker example</span></span>

<span data-ttu-id="1b022-146">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="1b022-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="1b022-147">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="1b022-147">Create health checks</span></span>

<span data-ttu-id="1b022-148">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="1b022-149"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="1b022-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="1b022-150">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="1b022-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="1b022-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="1b022-152">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="1b022-153">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="1b022-154">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="1b022-155">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1b022-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="1b022-156">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="1b022-156">Register health check services</span></span>

<span data-ttu-id="1b022-157">`ExampleHealthCheck` türü, `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="1b022-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="1b022-158">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="1b022-159">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="1b022-160">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="1b022-161">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="1b022-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="1b022-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="1b022-163">Aşağıdaki örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="1b022-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="1b022-164">Bir sistem durumu denetimi uygulamasına daha fazla geçiş yapmak için <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="1b022-164">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arugments to a health check implementation.</span></span> <span data-ttu-id="1b022-165">Aşağıdaki örnekte, `TestHealthCheckWithArgs` bir tamsayıyı ve <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> çağrıldığında kullanılacak dizeyi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="1b022-165">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

```csharp
private class TestHealthCheckWithArgs : IHealthCheck
{
    public TestHealthCheckWithArgs(int i, string s)
    {
        I = i;
        S = s;
    }

    public int I { get; set; }

    public string S { get; set; }

    public Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, 
        CancellationToken cancellationToken = default)
    {
        ...
    }
}
```

<span data-ttu-id="1b022-166">`TestHealthCheckWithArgs`, uygulamaya geçirilen tamsayı ve dize ile `AddTypeActivatedCheck` çağırarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="1b022-166">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="1b022-167">Sistem durumu denetimleri yönlendirmeyi kullanma</span><span class="sxs-lookup"><span data-stu-id="1b022-167">Use Health Checks Routing</span></span>

<span data-ttu-id="1b022-168">`Startup.Configure`, uç nokta URL 'SI veya göreli yol ile Endpoint Builder üzerinde `MapHealthChecks` çağırın:</span><span class="sxs-lookup"><span data-stu-id="1b022-168">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="1b022-169">Konak gerektir</span><span class="sxs-lookup"><span data-stu-id="1b022-169">Require host</span></span>

<span data-ttu-id="1b022-170">Sistem durumu denetimi uç noktası için bir veya daha fazla izin verilen Konakları belirtmek üzere `RequireHost` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="1b022-170">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="1b022-171">Konaklar, puni kodu yerine Unicode olmalıdır ve bir bağlantı noktası içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-171">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="1b022-172">Bir koleksiyon sağlanmazsa, herhangi bir konak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-172">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="1b022-173">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="1b022-173">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="1b022-174">Yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="1b022-174">Require authorization</span></span>

<span data-ttu-id="1b022-175">Sistem durumu denetimi istek uç noktasında yetkilendirme ara yazılımını çalıştırmak için `RequireAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1b022-175">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="1b022-176">`RequireAuthorization` aşırı yükleme bir veya daha fazla yetkilendirme ilkesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="1b022-176">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="1b022-177">Bir ilke sağlanmazsa, varsayılan yetkilendirme ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-177">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="1b022-178">Kaynaklar Arası İstekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-178">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="1b022-179">Bir tarayıcıdan el ile sistem durumu denetimleri gerçekleştirmek yaygın kullanım senaryosu olmamasına karşın, CORS ara yazılımı `RequireCors` sistem durumu denetimleri uç noktalarında çağırarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-179">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="1b022-180">`RequireCors` aşırı yüklemesi CORS ilke Oluşturucu temsilcisini (`CorsPolicyBuilder`) veya bir ilke adını kabul eder.</span><span class="sxs-lookup"><span data-stu-id="1b022-180">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="1b022-181">Bir ilke sağlanmazsa varsayılan CORS ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-181">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="1b022-182">Daha fazla bilgi için bkz. <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="1b022-182">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="1b022-183">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="1b022-183">Health check options</span></span>

<span data-ttu-id="1b022-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="1b022-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="1b022-185">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-185">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="1b022-186">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-186">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="1b022-187">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="1b022-187">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="1b022-188">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-188">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="1b022-189">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-189">Filter health checks</span></span>

<span data-ttu-id="1b022-190">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-190">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="1b022-191">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-191">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="1b022-192">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür:</span><span class="sxs-lookup"><span data-stu-id="1b022-192">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="1b022-193">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1b022-193">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="1b022-194">`Startup.Configure`, `Predicate` ' Bar ' sistem durumu denetimini filtreler.</span><span class="sxs-lookup"><span data-stu-id="1b022-194">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="1b022-195">Yalnızca Foo ve baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="1b022-195">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="1b022-196">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-196">Customize the HTTP status code</span></span>

<span data-ttu-id="1b022-197">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="1b022-198">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="1b022-198">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="1b022-199">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1b022-199">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="1b022-200">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-200">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="1b022-201">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="1b022-201">Suppress cache headers</span></span>

<span data-ttu-id="1b022-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="1b022-203">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1b022-203">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="1b022-204">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-204">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="1b022-205">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="1b022-206">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-206">Customize output</span></span>

<span data-ttu-id="1b022-207"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-207">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="1b022-208">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-208">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="1b022-209">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="1b022-209">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="1b022-210">Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-210">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="1b022-211">Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür.</span><span class="sxs-lookup"><span data-stu-id="1b022-211">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="1b022-212">Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-212">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="1b022-213">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="1b022-213">Database probe</span></span>

<span data-ttu-id="1b022-214">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-214">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="1b022-215">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-215">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="1b022-216">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="1b022-216">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="1b022-217">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="1b022-217">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="1b022-218">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-218">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="1b022-219">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1b022-219">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="1b022-220">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="1b022-220">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="1b022-221">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="1b022-221">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="1b022-222">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-222">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="1b022-223">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-223">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="1b022-224">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-224">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="1b022-225">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-225">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-226">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-226">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-227">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1b022-227">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="1b022-228">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-228">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="1b022-229">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-229">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="1b022-230">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1b022-230">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="1b022-231">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="1b022-231">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="1b022-232">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="1b022-232">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="1b022-233">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-233">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="1b022-234">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-234">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="1b022-235">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1b022-235">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="1b022-236">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-236">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="1b022-237">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-237">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="1b022-238">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="1b022-238">By default:</span></span>

* <span data-ttu-id="1b022-239">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-239">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="1b022-240">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-240">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="1b022-241">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-241">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="1b022-242">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-242">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-243">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1b022-243">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="1b022-244">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="1b022-244">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="1b022-245">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="1b022-245">If the database exists, delete it.</span></span>

<span data-ttu-id="1b022-246">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-246">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="1b022-247">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-247">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="1b022-248">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b022-248">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="1b022-249">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-249">Trigger the sample app to create the database.</span></span> <span data-ttu-id="1b022-250">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-250">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="1b022-251">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-251">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1b022-252">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-252">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1b022-253">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-253">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="1b022-254">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-254">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="1b022-255">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-255">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="1b022-256">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-256">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1b022-257">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-257">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1b022-258">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b022-258">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="1b022-259">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="1b022-259">Separate readiness and liveness probes</span></span>

<span data-ttu-id="1b022-260">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="1b022-260">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="1b022-261">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="1b022-261">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="1b022-262">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="1b022-262">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="1b022-263">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="1b022-263">The app is functioning and responding to requests.</span></span> <span data-ttu-id="1b022-264">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-264">This state is the app's *liveness*.</span></span>

<span data-ttu-id="1b022-265">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-265">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="1b022-266">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-266">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="1b022-267">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-267">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="1b022-268">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-268">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="1b022-269">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-269">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="1b022-270">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-270">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="1b022-271">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-271">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="1b022-272">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-272">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="1b022-273">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-273">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-274">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-274">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="1b022-275">Örnek uygulamada, sistem durumu denetimi uç noktaları şu konumda oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="1b022-275">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="1b022-276">Hazırlık denetimi için `/health/ready`.</span><span class="sxs-lookup"><span data-stu-id="1b022-276">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="1b022-277">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-277">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="1b022-278">libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="1b022-278">`/health/live` for the liveness check.</span></span> <span data-ttu-id="1b022-279">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="1b022-279">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="1b022-280">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="1b022-280">In the following example code:</span></span>

* <span data-ttu-id="1b022-281">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-281">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="1b022-282">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="1b022-282">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="1b022-283">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-283">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="1b022-284">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="1b022-284">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="1b022-285">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="1b022-285">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="1b022-286">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-286">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="1b022-287">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1b022-287">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="1b022-288">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1b022-288">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="1b022-289">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="1b022-289">Kubernetes example</span></span>

<span data-ttu-id="1b022-290">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-290">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="1b022-291">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-291">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="1b022-292">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-292">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="1b022-293">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="1b022-293">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="1b022-294">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1b022-294">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="1b022-295">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="1b022-295">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="1b022-296">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-296">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="1b022-297">uygulama, belirli bir bellek eşiğine (örnek uygulamada 1 GB) daha fazlasını kullanıyorsa `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="1b022-297">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="1b022-298"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-298">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="1b022-299">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-299">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-300">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-300">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="1b022-301">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-301">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="1b022-302">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="1b022-302">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="1b022-303">Örnek uygulamada (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-303">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="1b022-304">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-304">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="1b022-305">Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-305">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="1b022-306">`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-306">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="1b022-307">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-307">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="1b022-308">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-308">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="1b022-309">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-309">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="1b022-310">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-310">Filter by port</span></span>

<span data-ttu-id="1b022-311">Belirtilen bağlantı noktasına sistem durumu denetim isteklerini kısıtlamak için bir bağlantı noktası belirten bir URL düzeniyle `MapHealthChecks` `RequireHost` çağırın.</span><span class="sxs-lookup"><span data-stu-id="1b022-311">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="1b022-312">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-312">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="1b022-313">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-313">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1b022-314">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-314">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="1b022-315">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1b022-315">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="1b022-316">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-316">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="1b022-317">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="1b022-317">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="1b022-318">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-318">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-319">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-319">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="1b022-320">Örnek uygulamada, `Startup.Configure` uç noktasındaki `RequireHost` çağrısı, yapılandırmadan yönetim bağlantı noktasını belirtir:</span><span class="sxs-lookup"><span data-stu-id="1b022-320">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="1b022-321">Uç noktalar `Startup.Configure`örnek uygulamada oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-321">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="1b022-322">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="1b022-322">In the following example code:</span></span>

* <span data-ttu-id="1b022-323">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-323">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="1b022-324">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="1b022-324">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="1b022-325">Yönetim bağlantı noktasını kodda açıkça ayarlayarak, örnek uygulamada *Launchsettings. JSON* dosyasını oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-325">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="1b022-326"><xref:Microsoft.Extensions.Hosting.HostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> için bir çağrı ekleyin ve uygulamanın yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-326">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="1b022-327">*ManagementPortStartup.cs*`Configure` `RequireHost`ile yönetim bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="1b022-327">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="1b022-328">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b022-328">*Program.cs*:</span></span>
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
> <span data-ttu-id="1b022-329">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b022-329">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="1b022-330">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-330">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="1b022-331">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="1b022-331">Distribute a health check library</span></span>

<span data-ttu-id="1b022-332">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="1b022-332">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="1b022-333"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="1b022-333">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="1b022-334">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-334">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="1b022-335">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="1b022-335">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="1b022-336">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-336">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="1b022-337">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="1b022-337">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="1b022-338">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1b022-338">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="1b022-339">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="1b022-339">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="1b022-340">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="1b022-340">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="1b022-341">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-341">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="1b022-342">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-342">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="1b022-343">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="1b022-343">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="1b022-344">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="1b022-344">health check name (`name`).</span></span> <span data-ttu-id="1b022-345">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-345">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="1b022-346">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="1b022-346">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="1b022-347">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="1b022-347">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="1b022-348">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-348">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="1b022-349">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="1b022-349">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="1b022-350">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1b022-350">The default is `null`.</span></span> <span data-ttu-id="1b022-351">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-351">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="1b022-352">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="1b022-352">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="1b022-353">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="1b022-353">Health Check Publisher</span></span>

<span data-ttu-id="1b022-354">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-354">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="1b022-355">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-355">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="1b022-356"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1b022-356">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="1b022-357"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-357"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="1b022-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="1b022-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1b022-359">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="1b022-359">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="1b022-360">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-360">The default value is five seconds.</span></span>
* <span data-ttu-id="1b022-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="1b022-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="1b022-362">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-362">The default value is 30 seconds.</span></span>
* <span data-ttu-id="1b022-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="1b022-364">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-364">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="1b022-365">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-365">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="1b022-366">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="1b022-366"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1b022-367">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-367">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="1b022-368">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-368">The default value is 30 seconds.</span></span>

<span data-ttu-id="1b022-369">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-369">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="1b022-370">Sistem durumu denetimi durumu her denetim için günlük düzeyinde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="1b022-370">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="1b022-371">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="1b022-371">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="1b022-372">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="1b022-372">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="1b022-373">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-373">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="1b022-374"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="1b022-374">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="1b022-375">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-375">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="1b022-376">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-376">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="1b022-377">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="1b022-377">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="1b022-378">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-378">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="1b022-379">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="1b022-379">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="1b022-380">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="1b022-380">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1b022-381">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="1b022-381">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="1b022-382">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-382">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="1b022-383">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="1b022-383">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="1b022-384">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-384">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="1b022-385">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-385">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="1b022-386">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-386">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="1b022-387">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-387">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="1b022-388">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-388">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="1b022-389">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1b022-389">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1b022-390">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-390">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="1b022-391">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-391">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="1b022-392">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="1b022-392">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b022-393">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="1b022-393">Prerequisites</span></span>

<span data-ttu-id="1b022-394">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-394">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="1b022-395">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-395">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="1b022-396">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="1b022-396">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="1b022-397">Microsoft. aspnetcore [. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Aspnetcore. Diagnostics. healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-397">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="1b022-398">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-398">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="1b022-399">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-399">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="1b022-400">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-400">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="1b022-401">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="1b022-401">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="1b022-402">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-402">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="1b022-403">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1b022-403">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="1b022-404">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1b022-404">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="1b022-405">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1b022-405">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="1b022-406">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-406">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="1b022-407">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-407">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="1b022-408">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-408">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="1b022-409">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="1b022-409">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="1b022-410">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="1b022-410">Basic health probe</span></span>

<span data-ttu-id="1b022-411">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="1b022-411">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="1b022-412">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="1b022-412">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="1b022-413">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="1b022-413">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="1b022-414">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-414">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="1b022-415">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="1b022-415">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="1b022-416">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-416">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-417">`Startup.Configure`istek işleme ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> olan sistem durumu denetimleri ara yazılımı için bir uç nokta ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-417">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="1b022-418">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-418">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="1b022-419">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-419">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="1b022-420">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="1b022-420">Docker example</span></span>

<span data-ttu-id="1b022-421">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="1b022-421">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="1b022-422">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="1b022-422">Create health checks</span></span>

<span data-ttu-id="1b022-423">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1b022-423">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="1b022-424"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="1b022-424">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="1b022-425">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="1b022-425">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="1b022-426"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-426"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="1b022-427">Örnek sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="1b022-427">Example health check</span></span>

<span data-ttu-id="1b022-428">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-428">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="1b022-429">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-429">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="1b022-430">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-430">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="1b022-431">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1b022-431">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="1b022-432">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="1b022-432">Register health check services</span></span>

<span data-ttu-id="1b022-433">`ExampleHealthCheck` türü <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>ile `Startup.ConfigureServices` sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="1b022-433">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="1b022-434">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-434">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="1b022-435">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-435">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="1b022-436">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-436">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="1b022-437">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="1b022-437">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="1b022-438"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-438"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="1b022-439">Aşağıdaki `Startup.ConfigureServices` örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="1b022-439">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="1b022-440">Sistem durumu denetimleri ara yazılımı kullan</span><span class="sxs-lookup"><span data-stu-id="1b022-440">Use Health Checks Middleware</span></span>

<span data-ttu-id="1b022-441">`Startup.Configure`, işlem hattının uç nokta URL 'SI veya göreli yolu ile <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="1b022-441">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1b022-442">Durum denetimlerinin belirli bir bağlantı noktasını dinlemesi gerekiyorsa, bağlantı noktasını ayarlamak için <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> aşırı yüklemesini kullanın ( [bağlantı noktasına göre filtrele](#filter-by-port) bölümünde anlatılmıştır):</span><span class="sxs-lookup"><span data-stu-id="1b022-442">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="1b022-443">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="1b022-443">Health check options</span></span>

<span data-ttu-id="1b022-444"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="1b022-444"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="1b022-445">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-445">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="1b022-446">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-446">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="1b022-447">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="1b022-447">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="1b022-448">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-448">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="1b022-449">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-449">Filter health checks</span></span>

<span data-ttu-id="1b022-450">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-450">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="1b022-451">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-451">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="1b022-452">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür:</span><span class="sxs-lookup"><span data-stu-id="1b022-452">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="1b022-453">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-453">Customize the HTTP status code</span></span>

<span data-ttu-id="1b022-454">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-454">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="1b022-455">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="1b022-455">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="1b022-456">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1b022-456">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="1b022-457">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-457">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="1b022-458">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="1b022-458">Suppress cache headers</span></span>

<span data-ttu-id="1b022-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="1b022-460">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="1b022-460">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="1b022-461">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-461">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="1b022-462">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-462">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="1b022-463">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1b022-463">Customize output</span></span>

<span data-ttu-id="1b022-464"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-464">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="1b022-465">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="1b022-465">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="1b022-466">`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1b022-466">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="1b022-467">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="1b022-467">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="1b022-468">Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-468">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="1b022-469">Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür.</span><span class="sxs-lookup"><span data-stu-id="1b022-469">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="1b022-470">Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-470">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="1b022-471">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="1b022-471">Database probe</span></span>

<span data-ttu-id="1b022-472">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-472">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="1b022-473">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-473">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="1b022-474">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="1b022-474">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="1b022-475">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="1b022-475">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="1b022-476">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-476">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="1b022-477">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1b022-477">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="1b022-478">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="1b022-478">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="1b022-479">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="1b022-479">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="1b022-480">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-480">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="1b022-481">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-481">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="1b022-482">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-482">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="1b022-483">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-483">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-484">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-484">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-485">Uygulama işleme işlem hattının `Startup.Configure`içindeki sistem durumu denetimleri ara yazılımını çağır:</span><span class="sxs-lookup"><span data-stu-id="1b022-485">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1b022-486">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-486">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="1b022-487">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-487">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="1b022-488">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1b022-488">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="1b022-489">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="1b022-489">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="1b022-490">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="1b022-490">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="1b022-491">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-491">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="1b022-492">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-492">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="1b022-493">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="1b022-493">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="1b022-494">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-494">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="1b022-495">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-495">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="1b022-496">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="1b022-496">By default:</span></span>

* <span data-ttu-id="1b022-497">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-497">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="1b022-498">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-498">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="1b022-499">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-499">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="1b022-500">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-500">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-501">Örnek uygulamada, `UseHealthChecks` sistem durumu denetimleri ara yazılımını `Startup.Configure`ekler.</span><span class="sxs-lookup"><span data-stu-id="1b022-501">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1b022-502">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="1b022-502">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="1b022-503">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="1b022-503">If the database exists, delete it.</span></span>

<span data-ttu-id="1b022-504">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-504">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="1b022-505">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-505">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="1b022-506">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b022-506">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="1b022-507">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-507">Trigger the sample app to create the database.</span></span> <span data-ttu-id="1b022-508">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-508">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="1b022-509">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-509">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1b022-510">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-510">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1b022-511">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-511">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="1b022-512">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="1b022-512">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="1b022-513">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-513">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="1b022-514">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="1b022-514">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1b022-515">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-515">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1b022-516">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b022-516">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="1b022-517">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="1b022-517">Separate readiness and liveness probes</span></span>

<span data-ttu-id="1b022-518">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="1b022-518">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="1b022-519">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="1b022-519">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="1b022-520">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="1b022-520">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="1b022-521">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="1b022-521">The app is functioning and responding to requests.</span></span> <span data-ttu-id="1b022-522">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-522">This state is the app's *liveness*.</span></span>

<span data-ttu-id="1b022-523">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-523">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="1b022-524">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-524">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="1b022-525">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="1b022-525">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="1b022-526">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-526">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="1b022-527">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-527">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="1b022-528">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-528">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="1b022-529">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-529">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="1b022-530">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-530">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="1b022-531">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-531">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1b022-532">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="1b022-532">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="1b022-533">Örnek uygulamada, sistem durumu denetimi uç noktaları, hazır olma denetimi için `/health/ready` oluşturulur ve libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="1b022-533">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="1b022-534">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="1b022-534">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="1b022-535">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="1b022-535">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="1b022-536">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-536">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="1b022-537">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="1b022-537">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="1b022-538">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="1b022-538">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="1b022-539">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-539">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="1b022-540">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1b022-540">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="1b022-541">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="1b022-541">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="1b022-542">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="1b022-542">Kubernetes example</span></span>

<span data-ttu-id="1b022-543">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-543">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="1b022-544">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-544">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="1b022-545">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-545">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="1b022-546">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="1b022-546">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="1b022-547">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="1b022-547">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="1b022-548">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="1b022-548">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="1b022-549">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-549">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="1b022-550">uygulama, belirli bir bellek eşiğini (örnek uygulamada 1 GB) kullanıyorsa, sağlıksız bir durumu raporlar `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="1b022-550">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="1b022-551"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-551">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="1b022-552">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-552">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-553">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-553">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="1b022-554">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-554">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="1b022-555">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="1b022-555">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="1b022-556">Örnek uygulamada (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-556">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="1b022-557">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="1b022-557">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="1b022-558">Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:</span><span class="sxs-lookup"><span data-stu-id="1b022-558">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="1b022-559">`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-559">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="1b022-560">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-560">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="1b022-561">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-561">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="1b022-562">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-562">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="1b022-563">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="1b022-563">Filter by port</span></span>

<span data-ttu-id="1b022-564"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> bir bağlantı noktası ile çağırmak, belirtilen bağlantı noktasına durum denetimi isteklerini kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-564">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="1b022-565">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-565">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="1b022-566">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-566">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1b022-567">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-567">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="1b022-568">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="1b022-568">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="1b022-569">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1b022-569">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="1b022-570">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="1b022-570">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="1b022-571">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="1b022-571">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1b022-572"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrısı, yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="1b022-572">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="1b022-573">Kod içinde açıkça URL 'Leri ve yönetim bağlantı noktasını ayarlayarak *Launchsettings. JSON* dosyasını örnek uygulamada oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1b022-573">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="1b022-574"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> için bir çağrı ekleyin ve uygulamanın normal yanıt uç noktasını ve yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-574">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="1b022-575"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrıldığında, yönetim bağlantı noktasını açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="1b022-575">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="1b022-576">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b022-576">*Program.cs*:</span></span>
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
> <span data-ttu-id="1b022-577">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1b022-577">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="1b022-578">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="1b022-578">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="1b022-579">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="1b022-579">Distribute a health check library</span></span>

<span data-ttu-id="1b022-580">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="1b022-580">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="1b022-581"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="1b022-581">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="1b022-582">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-582">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="1b022-583">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="1b022-583">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="1b022-584">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-584">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="1b022-585">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="1b022-585">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="1b022-586">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="1b022-586">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="1b022-587">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="1b022-587">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="1b022-588">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="1b022-588">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="1b022-589">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="1b022-589">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="1b022-590">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="1b022-590">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="1b022-591">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="1b022-591">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="1b022-592">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="1b022-592">health check name (`name`).</span></span> <span data-ttu-id="1b022-593">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-593">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="1b022-594">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="1b022-594">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="1b022-595">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="1b022-595">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="1b022-596">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b022-596">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="1b022-597">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="1b022-597">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="1b022-598">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1b022-598">The default is `null`.</span></span> <span data-ttu-id="1b022-599">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-599">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="1b022-600">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="1b022-600">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="1b022-601">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="1b022-601">Health Check Publisher</span></span>

<span data-ttu-id="1b022-602">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-602">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="1b022-603">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-603">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="1b022-604"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="1b022-604">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="1b022-605"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="1b022-605"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="1b022-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="1b022-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1b022-607">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="1b022-607">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="1b022-608">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-608">The default value is five seconds.</span></span>
* <span data-ttu-id="1b022-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="1b022-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="1b022-610">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-610">The default value is 30 seconds.</span></span>
* <span data-ttu-id="1b022-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="1b022-612">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="1b022-612">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="1b022-613">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="1b022-613">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="1b022-614">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="1b022-614"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1b022-615">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-615">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="1b022-616">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="1b022-616">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="1b022-617">ASP.NET Core 2,2 sürümünde, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> ayarı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulama tarafından kabul edilemez; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1b022-617">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="1b022-618">Bu sorun ASP.NET Core 3,0 ' de giderilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1b022-618">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="1b022-619">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="1b022-619">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="1b022-620">Durum denetimi durumu her denetim için şu şekilde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="1b022-620">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="1b022-621">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="1b022-621">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="1b022-622">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="1b022-622">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="1b022-623">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="1b022-623">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="1b022-624"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="1b022-624">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="1b022-625">Aşağıdaki geçici çözüm, uygulamaya bir veya daha fazla barındırılan hizmet zaten eklendiyse, hizmet kapsayıcısına <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> bir örnek eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="1b022-625">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="1b022-626">Bu geçici çözüm ASP.NET Core 3,0 ' de gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1b022-626">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="1b022-627">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="1b022-627">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="1b022-628">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="1b022-628">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="1b022-629">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="1b022-629">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="1b022-630">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b022-630">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="1b022-631">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="1b022-631">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="1b022-632">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="1b022-632">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
