---
title: ASP.NET Core durum denetimleri
author: guardrex
description: Uygulamalar ve veritabanları gibi ASP.NET Core altyapısı için sistem durumu denetimlerini ayarlamayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 33e5e71983a55b4ee30436d8e9e1e04186259a5d
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829224"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="63084-103">ASP.NET Core durum denetimleri</span><span class="sxs-lookup"><span data-stu-id="63084-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="63084-104">[Luke Latham](https://github.com/guardrex) ve [Glenn CONDRON](https://github.com/glennc) tarafından</span><span class="sxs-lookup"><span data-stu-id="63084-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="63084-105">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="63084-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="63084-106">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63084-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="63084-107">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="63084-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="63084-108">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="63084-109">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="63084-110">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="63084-111">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="63084-112">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="63084-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63084-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="63084-114">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="63084-115">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="63084-116">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63084-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="63084-117">Prerequisites</span></span>

<span data-ttu-id="63084-118">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="63084-119">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="63084-120">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="63084-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="63084-121">[Microsoft. AspNetCore. Diagnostics. Healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine ASP.NET Core uygulamaları için örtük olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="63084-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="63084-122">Entity Framework Core kullanarak sistem durumu denetimleri gerçekleştirmek için, [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="63084-123">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="63084-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="63084-124">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="63084-125">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="63084-126">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="63084-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="63084-127">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="63084-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="63084-128">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="63084-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="63084-129">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="63084-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="63084-130">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="63084-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="63084-131">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="63084-132">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63084-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="63084-133">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63084-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="63084-134">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="63084-135">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="63084-135">Basic health probe</span></span>

<span data-ttu-id="63084-136">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="63084-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="63084-137">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="63084-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="63084-138">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="63084-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="63084-139">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="63084-140">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="63084-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="63084-141">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-142">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="63084-143">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="63084-144">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="63084-145">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="63084-145">Docker example</span></span>

<span data-ttu-id="63084-146">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="63084-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="63084-147">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="63084-147">Create health checks</span></span>

<span data-ttu-id="63084-148">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63084-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="63084-149"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="63084-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="63084-150">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="63084-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="63084-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="63084-152">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="63084-153">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="63084-154">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="63084-155">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63084-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="63084-156">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="63084-156">Register health check services</span></span>

<span data-ttu-id="63084-157">`ExampleHealthCheck` türü, `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="63084-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="63084-158">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="63084-159">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="63084-160">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="63084-161">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="63084-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="63084-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="63084-163">Aşağıdaki örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="63084-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="63084-164">Bağımsız değişkenleri bir sistem durumu denetimi uygulamasına geçirmek için <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="63084-164">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arguments to a health check implementation.</span></span> <span data-ttu-id="63084-165">Aşağıdaki örnekte, `TestHealthCheckWithArgs` bir tamsayıyı ve <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> çağrıldığında kullanılacak dizeyi kabul eder:</span><span class="sxs-lookup"><span data-stu-id="63084-165">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

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

<span data-ttu-id="63084-166">`TestHealthCheckWithArgs`, uygulamaya geçirilen tamsayı ve dize ile `AddTypeActivatedCheck` çağırarak kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="63084-166">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="63084-167">Sistem durumu denetimleri yönlendirmeyi kullanma</span><span class="sxs-lookup"><span data-stu-id="63084-167">Use Health Checks Routing</span></span>

<span data-ttu-id="63084-168">`Startup.Configure`, uç nokta URL 'SI veya göreli yol ile Endpoint Builder üzerinde `MapHealthChecks` çağırın:</span><span class="sxs-lookup"><span data-stu-id="63084-168">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="63084-169">Konak gerektir</span><span class="sxs-lookup"><span data-stu-id="63084-169">Require host</span></span>

<span data-ttu-id="63084-170">Sistem durumu denetimi uç noktası için bir veya daha fazla izin verilen Konakları belirtmek üzere `RequireHost` çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="63084-170">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="63084-171">Konaklar, puni kodu yerine Unicode olmalıdır ve bir bağlantı noktası içerebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-171">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="63084-172">Bir koleksiyon sağlanmazsa, herhangi bir konak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="63084-172">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="63084-173">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-173">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="63084-174">Yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="63084-174">Require authorization</span></span>

<span data-ttu-id="63084-175">Sistem durumu denetimi istek uç noktasında yetkilendirme ara yazılımını çalıştırmak için `RequireAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="63084-175">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="63084-176">`RequireAuthorization` aşırı yükleme bir veya daha fazla yetkilendirme ilkesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="63084-176">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="63084-177">Bir ilke sağlanmazsa, varsayılan yetkilendirme ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-177">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="63084-178">Kaynaklar Arası İstekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-178">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="63084-179">Bir tarayıcıdan el ile sistem durumu denetimleri gerçekleştirmek yaygın kullanım senaryosu olmamasına karşın, CORS ara yazılımı `RequireCors` sistem durumu denetimleri uç noktalarında çağırarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-179">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="63084-180">`RequireCors` aşırı yüklemesi CORS ilke Oluşturucu temsilcisini (`CorsPolicyBuilder`) veya bir ilke adını kabul eder.</span><span class="sxs-lookup"><span data-stu-id="63084-180">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="63084-181">Bir ilke sağlanmazsa varsayılan CORS ilkesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-181">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="63084-182">Daha fazla bilgi için bkz. <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="63084-182">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="63084-183">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="63084-183">Health check options</span></span>

<span data-ttu-id="63084-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="63084-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="63084-185">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-185">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="63084-186">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-186">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="63084-187">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="63084-187">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="63084-188">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-188">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="63084-189">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-189">Filter health checks</span></span>

<span data-ttu-id="63084-190">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-190">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="63084-191">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-191">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="63084-192">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür:</span><span class="sxs-lookup"><span data-stu-id="63084-192">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="63084-193">`Startup.ConfigureServices` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-193">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="63084-194">`Startup.Configure`, `Predicate` ' Bar ' sistem durumu denetimini filtreler.</span><span class="sxs-lookup"><span data-stu-id="63084-194">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="63084-195">Yalnızca Foo ve baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="63084-195">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="63084-196">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-196">Customize the HTTP status code</span></span>

<span data-ttu-id="63084-197">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="63084-198">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="63084-198">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="63084-199">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="63084-199">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="63084-200">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-200">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="63084-201">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="63084-201">Suppress cache headers</span></span>

<span data-ttu-id="63084-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="63084-203">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="63084-203">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="63084-204">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="63084-204">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="63084-205">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="63084-206">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-206">Customize output</span></span>

<span data-ttu-id="63084-207">`Startup.Configure`, [Healthcheckoptions. ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) seçeneğini yanıtı yazmak için bir temsilci olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="63084-207">In `Startup.Configure`, set the [HealthCheckOptions.ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) option to a delegate for writing the response:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="63084-208">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="63084-208">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="63084-209">Aşağıdaki özel temsilciler özel bir JSON yanıtını çıktı.</span><span class="sxs-lookup"><span data-stu-id="63084-209">The following custom delegates output a custom JSON response.</span></span>

<span data-ttu-id="63084-210">Örnek uygulamadaki ilk örnek <xref:System.Text.Json?displayProperty=fullName>nasıl kullanacağınızı gösterir:</span><span class="sxs-lookup"><span data-stu-id="63084-210">The first example from the sample app demonstrates how to use <xref:System.Text.Json?displayProperty=fullName>:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_SystemTextJson)]

<span data-ttu-id="63084-211">İkinci örnek [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/)' ın nasıl kullanılacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="63084-211">The second example demonstrates how to use [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_NewtonSoftJson)]

<span data-ttu-id="63084-212">Örnek uygulamada, `WriteResponse``Newtonsoft.Json` sürümünü etkinleştirmek için, *CustomWriterStartup.cs* ' de `SYSTEM_TEXT_JSON` [Önişlemci yönergesini](xref:index#preprocessor-directives-in-sample-code) not edin.</span><span class="sxs-lookup"><span data-stu-id="63084-212">In the sample app, comment out the `SYSTEM_TEXT_JSON` [preprocessor directive](xref:index#preprocessor-directives-in-sample-code) in *CustomWriterStartup.cs* to enable the `Newtonsoft.Json` version of `WriteResponse`.</span></span>

<span data-ttu-id="63084-213">Durum denetimleri API 'SI, biçim, izleme sistemine özgü olan karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="63084-213">The health checks API doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="63084-214">Önceki örneklerde gereken yanıtı gerektiği gibi özelleştirin.</span><span class="sxs-lookup"><span data-stu-id="63084-214">Customize the response in the preceding examples as needed.</span></span> <span data-ttu-id="63084-215">`System.Text.Json`ile JSON serileştirme hakkında daha fazla bilgi için bkz. [.net 'TE JSON serileştirilmesi ve serisini kaldırma](/dotnet/standard/serialization/system-text-json-how-to).</span><span class="sxs-lookup"><span data-stu-id="63084-215">For more information on JSON serialization with `System.Text.Json`, see [How to serialize and deserialize JSON in .NET](/dotnet/standard/serialization/system-text-json-how-to).</span></span>

## <a name="database-probe"></a><span data-ttu-id="63084-216">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="63084-216">Database probe</span></span>

<span data-ttu-id="63084-217">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-217">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="63084-218">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="63084-218">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="63084-219">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="63084-219">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="63084-220">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="63084-220">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="63084-221">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-221">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="63084-222">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63084-222">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="63084-223">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="63084-223">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="63084-224">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="63084-224">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="63084-225">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-225">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="63084-226">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-226">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="63084-227">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="63084-227">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="63084-228">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-228">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-229">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-229">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-230">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="63084-230">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="63084-231">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-231">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="63084-232">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-232">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="63084-233">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="63084-233">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="63084-234">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="63084-234">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="63084-235">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="63084-235">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="63084-236">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-236">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="63084-237">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-237">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="63084-238">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="63084-238">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="63084-239">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-239">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="63084-240">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-240">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="63084-241">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="63084-241">By default:</span></span>

* <span data-ttu-id="63084-242">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="63084-242">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="63084-243">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63084-243">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="63084-244">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-244">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="63084-245">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-245">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-246">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="63084-246">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="63084-247">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="63084-247">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="63084-248">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="63084-248">If the database exists, delete it.</span></span>

<span data-ttu-id="63084-249">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-249">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="63084-250">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-250">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="63084-251">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="63084-251">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="63084-252">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-252">Trigger the sample app to create the database.</span></span> <span data-ttu-id="63084-253">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-253">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="63084-254">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-254">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="63084-255">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-255">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="63084-256">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-256">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="63084-257">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-257">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="63084-258">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-258">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="63084-259">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-259">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="63084-260">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-260">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="63084-261">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="63084-261">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="63084-262">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="63084-262">Separate readiness and liveness probes</span></span>

<span data-ttu-id="63084-263">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="63084-263">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="63084-264">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="63084-264">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="63084-265">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="63084-265">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="63084-266">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="63084-266">The app is functioning and responding to requests.</span></span> <span data-ttu-id="63084-267">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="63084-267">This state is the app's *liveness*.</span></span>

<span data-ttu-id="63084-268">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="63084-268">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="63084-269">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="63084-269">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="63084-270">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63084-270">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="63084-271">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-271">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="63084-272">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-272">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="63084-273">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="63084-273">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="63084-274">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="63084-274">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="63084-275">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-275">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="63084-276">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-276">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-277">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63084-277">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="63084-278">Örnek uygulamada, sistem durumu denetimi uç noktaları şu konumda oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="63084-278">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="63084-279">Hazırlık denetimi için `/health/ready`.</span><span class="sxs-lookup"><span data-stu-id="63084-279">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="63084-280">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-280">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="63084-281">libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="63084-281">`/health/live` for the liveness check.</span></span> <span data-ttu-id="63084-282">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="63084-282">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="63084-283">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="63084-283">In the following example code:</span></span>

* <span data-ttu-id="63084-284">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="63084-284">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="63084-285">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="63084-285">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="63084-286">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-286">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="63084-287">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="63084-287">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="63084-288">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="63084-288">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="63084-289">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="63084-289">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="63084-290">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63084-290">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="63084-291">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="63084-291">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="63084-292">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="63084-292">Kubernetes example</span></span>

<span data-ttu-id="63084-293">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-293">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="63084-294">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-294">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="63084-295">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="63084-295">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="63084-296">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="63084-296">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="63084-297">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="63084-297">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="63084-298">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="63084-298">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="63084-299">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-299">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="63084-300">uygulama, belirli bir bellek eşiğine (örnek uygulamada 1 GB) daha fazlasını kullanıyorsa `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="63084-300">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="63084-301"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-301">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="63084-302">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-302">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-303">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-303">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="63084-304">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-304">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="63084-305">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="63084-305">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="63084-306">Örnek uygulamanın *CustomWriterStartup.cs* ' de:</span><span class="sxs-lookup"><span data-stu-id="63084-306">In *CustomWriterStartup.cs* of the sample app:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="63084-307">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63084-307">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="63084-308">Sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıktısını almak için Microsoft. AspNetCore. Diagnostics. Healthdenetimleri. HealthCheckOptions. ResponseWriter > özelliğine < `WriteResponse` bir temsilci sağlanır:</span><span class="sxs-lookup"><span data-stu-id="63084-308">A `WriteResponse` delegate is provided to the <Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="63084-309">`WriteResponse` temsilci, `CompositeHealthCheckResult` bir JSON nesnesine biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir.</span><span class="sxs-lookup"><span data-stu-id="63084-309">The `WriteResponse` delegate formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response.</span></span> <span data-ttu-id="63084-310">Daha fazla bilgi için [çıktıyı özelleştirme](#customize-output) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-310">For more information, see the [Customize output](#customize-output) section.</span></span>

<span data-ttu-id="63084-311">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-311">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="63084-312">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-312">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="63084-313">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-313">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="63084-314">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-314">Filter by port</span></span>

<span data-ttu-id="63084-315">Belirtilen bağlantı noktasına sistem durumu denetim isteklerini kısıtlamak için bir bağlantı noktası belirten bir URL düzeniyle `MapHealthChecks` `RequireHost` çağırın.</span><span class="sxs-lookup"><span data-stu-id="63084-315">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="63084-316">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-316">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="63084-317">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="63084-317">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="63084-318">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-318">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="63084-319">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63084-319">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="63084-320">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-320">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="63084-321">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="63084-321">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="63084-322">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-322">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-323">`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-323">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="63084-324">Örnek uygulamada, `Startup.Configure` uç noktasındaki `RequireHost` çağrısı, yapılandırmadan yönetim bağlantı noktasını belirtir:</span><span class="sxs-lookup"><span data-stu-id="63084-324">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="63084-325">Uç noktalar `Startup.Configure`örnek uygulamada oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63084-325">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="63084-326">Aşağıdaki örnek kodda:</span><span class="sxs-lookup"><span data-stu-id="63084-326">In the following example code:</span></span>

* <span data-ttu-id="63084-327">Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="63084-327">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="63084-328">`Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="63084-328">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="63084-329">Yönetim bağlantı noktasını kodda açıkça ayarlayarak, örnek uygulamada *Launchsettings. JSON* dosyasını oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63084-329">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="63084-330"><xref:Microsoft.Extensions.Hosting.HostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> için bir çağrı ekleyin ve uygulamanın yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-330">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="63084-331">*ManagementPortStartup.cs*`Configure` `RequireHost`ile yönetim bağlantı noktasını belirtin:</span><span class="sxs-lookup"><span data-stu-id="63084-331">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="63084-332">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="63084-332">*Program.cs*:</span></span>
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
> <span data-ttu-id="63084-333">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63084-333">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="63084-334">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-334">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="63084-335">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="63084-335">Distribute a health check library</span></span>

<span data-ttu-id="63084-336">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="63084-336">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="63084-337"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="63084-337">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="63084-338">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-338">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="63084-339">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="63084-339">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="63084-340">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-340">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="63084-341">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="63084-341">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="63084-342">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63084-342">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="63084-343">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="63084-343">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="63084-344">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="63084-344">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="63084-345">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-345">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="63084-346">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-346">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="63084-347">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="63084-347">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="63084-348">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="63084-348">health check name (`name`).</span></span> <span data-ttu-id="63084-349">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-349">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="63084-350">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="63084-350">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="63084-351">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="63084-351">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="63084-352">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-352">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="63084-353">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="63084-353">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="63084-354">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="63084-354">The default is `null`.</span></span> <span data-ttu-id="63084-355">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-355">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="63084-356">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="63084-356">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="63084-357">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="63084-357">Health Check Publisher</span></span>

<span data-ttu-id="63084-358">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="63084-358">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="63084-359">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-359">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="63084-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="63084-360">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="63084-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="63084-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="63084-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="63084-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="63084-363">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="63084-363">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="63084-364">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-364">The default value is five seconds.</span></span>
* <span data-ttu-id="63084-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="63084-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="63084-366">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-366">The default value is 30 seconds.</span></span>
* <span data-ttu-id="63084-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="63084-368">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-368">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="63084-369">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-369">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="63084-370">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="63084-370"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="63084-371">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-371">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="63084-372">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-372">The default value is 30 seconds.</span></span>

<span data-ttu-id="63084-373">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-373">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="63084-374">Sistem durumu denetimi durumu her denetim için günlük düzeyinde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="63084-374">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="63084-375">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="63084-375">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="63084-376">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="63084-376">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="63084-377">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-377">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="63084-378"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="63084-378">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="63084-379">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-379">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="63084-380">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-380">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="63084-381">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="63084-381">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="63084-382">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-382">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="63084-383">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="63084-383">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="63084-384">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="63084-384">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="63084-385">ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="63084-385">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="63084-386">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63084-386">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="63084-387">Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="63084-387">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="63084-388">Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-388">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="63084-389">Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-389">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="63084-390">Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-390">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="63084-391">Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-391">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="63084-392">Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-392">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="63084-393">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63084-393">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="63084-394">Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-394">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="63084-395">Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-395">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="63084-396">Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-396">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63084-397">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="63084-397">Prerequisites</span></span>

<span data-ttu-id="63084-398">Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-398">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="63084-399">Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-399">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="63084-400">İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.</span><span class="sxs-lookup"><span data-stu-id="63084-400">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="63084-401">Microsoft. aspnetcore [. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Aspnetcore. Diagnostics. healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-401">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="63084-402">Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="63084-402">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="63084-403">[Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-403">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="63084-404">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-404">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="63084-405">Örnek uygulama olan veritabanı senaryolarını araştırmak için:</span><span class="sxs-lookup"><span data-stu-id="63084-405">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="63084-406">Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="63084-406">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="63084-407">, Proje dosyasında aşağıdaki paket başvurularına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="63084-407">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="63084-408">AspNetCore. Healthdenetimleri. SqlServer</span><span class="sxs-lookup"><span data-stu-id="63084-408">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="63084-409">Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="63084-409">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="63084-410">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-410">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="63084-411">Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir.</span><span class="sxs-lookup"><span data-stu-id="63084-411">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="63084-412">Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63084-412">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="63084-413">Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="63084-413">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="63084-414">Temel sistem durumu araştırması</span><span class="sxs-lookup"><span data-stu-id="63084-414">Basic health probe</span></span>

<span data-ttu-id="63084-415">Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="63084-415">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="63084-416">Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="63084-416">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="63084-417">Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir.</span><span class="sxs-lookup"><span data-stu-id="63084-417">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="63084-418">Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-418">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="63084-419">Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.</span><span class="sxs-lookup"><span data-stu-id="63084-419">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="63084-420">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-420">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-421">`Startup.Configure`istek işleme ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> olan sistem durumu denetimleri ara yazılımı için bir uç nokta ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-421">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="63084-422">Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-422">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="63084-423">Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-423">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="63084-424">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="63084-424">Docker example</span></span>

<span data-ttu-id="63084-425">[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:</span><span class="sxs-lookup"><span data-stu-id="63084-425">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="63084-426">Durum denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="63084-426">Create health checks</span></span>

<span data-ttu-id="63084-427">Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="63084-427">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="63084-428"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür.</span><span class="sxs-lookup"><span data-stu-id="63084-428">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="63084-429">Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="63084-429">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="63084-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="63084-431">Örnek sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="63084-431">Example health check</span></span>

<span data-ttu-id="63084-432">Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-432">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="63084-433">Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-433">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="63084-434">Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-434">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="63084-435">`healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63084-435">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="63084-436">Sistem durumu denetimi hizmetlerini Kaydet</span><span class="sxs-lookup"><span data-stu-id="63084-436">Register health check services</span></span>

<span data-ttu-id="63084-437">`ExampleHealthCheck` türü <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>ile `Startup.ConfigureServices` sistem durumu denetimi hizmetlerine eklenir:</span><span class="sxs-lookup"><span data-stu-id="63084-437">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="63084-438">Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-438">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="63084-439">Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-439">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="63084-440">Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-440">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="63084-441">*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="63084-441">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="63084-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="63084-443">Aşağıdaki `Startup.ConfigureServices` örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:</span><span class="sxs-lookup"><span data-stu-id="63084-443">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="63084-444">Sistem durumu denetimleri ara yazılımı kullan</span><span class="sxs-lookup"><span data-stu-id="63084-444">Use Health Checks Middleware</span></span>

<span data-ttu-id="63084-445">`Startup.Configure`, işlem hattının uç nokta URL 'SI veya göreli yolu ile <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağırın:</span><span class="sxs-lookup"><span data-stu-id="63084-445">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="63084-446">Durum denetimlerinin belirli bir bağlantı noktasını dinlemesi gerekiyorsa, bağlantı noktasını ayarlamak için <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> aşırı yüklemesini kullanın ( [bağlantı noktasına göre filtrele](#filter-by-port) bölümünde anlatılmıştır):</span><span class="sxs-lookup"><span data-stu-id="63084-446">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="63084-447">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="63084-447">Health check options</span></span>

<span data-ttu-id="63084-448"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:</span><span class="sxs-lookup"><span data-stu-id="63084-448"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="63084-449">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-449">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="63084-450">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-450">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="63084-451">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="63084-451">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="63084-452">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-452">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="63084-453">Durum denetimlerini filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-453">Filter health checks</span></span>

<span data-ttu-id="63084-454">Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-454">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="63084-455">Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-455">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="63084-456">Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür:</span><span class="sxs-lookup"><span data-stu-id="63084-456">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="63084-457">HTTP durum kodunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-457">Customize the HTTP status code</span></span>

<span data-ttu-id="63084-458">Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-458">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="63084-459">Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="63084-459">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="63084-460">Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="63084-460">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="63084-461">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-461">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="63084-462">Önbellek üstbilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="63084-462">Suppress cache headers</span></span>

<span data-ttu-id="63084-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="63084-464">Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="63084-464">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="63084-465">Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="63084-465">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="63084-466">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-466">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="63084-467">Çıktıyı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="63084-467">Customize output</span></span>

<span data-ttu-id="63084-468"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-468">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="63084-469">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="63084-469">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="63084-470">`Startup.Configure` içinde:</span><span class="sxs-lookup"><span data-stu-id="63084-470">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="63084-471">Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.</span><span class="sxs-lookup"><span data-stu-id="63084-471">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="63084-472">Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:</span><span class="sxs-lookup"><span data-stu-id="63084-472">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="63084-473">Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür.</span><span class="sxs-lookup"><span data-stu-id="63084-473">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="63084-474">Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63084-474">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="63084-475">Veritabanı araştırması</span><span class="sxs-lookup"><span data-stu-id="63084-475">Database probe</span></span>

<span data-ttu-id="63084-476">Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-476">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="63084-477">Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır.</span><span class="sxs-lookup"><span data-stu-id="63084-477">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="63084-478">`AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.</span><span class="sxs-lookup"><span data-stu-id="63084-478">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="63084-479">Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="63084-479">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="63084-480">Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-480">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="63084-481">Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63084-481">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="63084-482">Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="63084-482">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="63084-483">Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.</span><span class="sxs-lookup"><span data-stu-id="63084-483">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="63084-484">[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-484">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="63084-485">Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-485">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="63084-486">Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:</span><span class="sxs-lookup"><span data-stu-id="63084-486">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="63084-487">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-487">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-488">Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-488">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-489">Uygulama işleme işlem hattının `Startup.Configure`içindeki sistem durumu denetimleri ara yazılımını çağır:</span><span class="sxs-lookup"><span data-stu-id="63084-489">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="63084-490">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-490">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="63084-491">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-491">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="63084-492">DbContext araştırması Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="63084-492">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="63084-493">`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="63084-493">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="63084-494">`DbContext` denetimi şu uygulamalar için desteklenir:</span><span class="sxs-lookup"><span data-stu-id="63084-494">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="63084-495">[Entity Framework (EF) Core](/ef/core/)kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-495">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="63084-496">[Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-496">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="63084-497">`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder.</span><span class="sxs-lookup"><span data-stu-id="63084-497">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="63084-498">`DbContext`, metoduna `TContext` olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-498">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="63084-499">Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-499">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="63084-500">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="63084-500">By default:</span></span>

* <span data-ttu-id="63084-501">`DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="63084-501">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="63084-502">`AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63084-502">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="63084-503">Sistem durumu denetiminin adı `TContext` türünün adıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-503">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="63084-504">Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-504">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-505">Örnek uygulamada, `UseHealthChecks` sistem durumu denetimleri ara yazılımını `Startup.Configure`ekler.</span><span class="sxs-lookup"><span data-stu-id="63084-505">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="63084-506">Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="63084-506">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="63084-507">Veritabanı varsa, silin.</span><span class="sxs-lookup"><span data-stu-id="63084-507">If the database exists, delete it.</span></span>

<span data-ttu-id="63084-508">Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-508">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="63084-509">Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-509">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="63084-510">Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:</span><span class="sxs-lookup"><span data-stu-id="63084-510">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="63084-511">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-511">Trigger the sample app to create the database.</span></span> <span data-ttu-id="63084-512">`/createdatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-512">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="63084-513">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-513">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="63084-514">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-514">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="63084-515">Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-515">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="63084-516">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="63084-516">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="63084-517">`/deletedatabase`için bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-517">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="63084-518">Uygulama yanıt veriyor:</span><span class="sxs-lookup"><span data-stu-id="63084-518">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="63084-519">`/health` uç noktasına bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-519">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="63084-520">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="63084-520">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="63084-521">Ayrı hazırlık ve lizlilik araştırmaları</span><span class="sxs-lookup"><span data-stu-id="63084-521">Separate readiness and liveness probes</span></span>

<span data-ttu-id="63084-522">Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:</span><span class="sxs-lookup"><span data-stu-id="63084-522">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="63084-523">Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma.</span><span class="sxs-lookup"><span data-stu-id="63084-523">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="63084-524">Bu durum, uygulamanın *hazır olma*durumu.</span><span class="sxs-lookup"><span data-stu-id="63084-524">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="63084-525">Uygulama çalışır ve isteklere yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="63084-525">The app is functioning and responding to requests.</span></span> <span data-ttu-id="63084-526">Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.</span><span class="sxs-lookup"><span data-stu-id="63084-526">This state is the app's *liveness*.</span></span>

<span data-ttu-id="63084-527">Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="63084-527">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="63084-528">Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="63084-528">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="63084-529">Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63084-529">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="63084-530">Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-530">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="63084-531">`StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-531">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="63084-532">Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır.</span><span class="sxs-lookup"><span data-stu-id="63084-532">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="63084-533">Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="63084-533">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="63084-534">Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-534">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="63084-535">Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-535">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="63084-536">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="63084-536">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="63084-537">Örnek uygulamada, sistem durumu denetimi uç noktaları, hazır olma denetimi için `/health/ready` oluşturulur ve libir denetim için `/health/live`.</span><span class="sxs-lookup"><span data-stu-id="63084-537">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="63084-538">Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.</span><span class="sxs-lookup"><span data-stu-id="63084-538">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="63084-539">Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="63084-539">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="63084-540">Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-540">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="63084-541">Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin.</span><span class="sxs-lookup"><span data-stu-id="63084-541">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="63084-542">Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="63084-542">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="63084-543">15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.</span><span class="sxs-lookup"><span data-stu-id="63084-543">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="63084-544">Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="63084-544">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="63084-545">Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.</span><span class="sxs-lookup"><span data-stu-id="63084-545">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="63084-546">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="63084-546">Kubernetes example</span></span>

<span data-ttu-id="63084-547">Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-547">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="63084-548">Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="63084-548">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="63084-549">Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="63084-549">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="63084-550">Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .</span><span class="sxs-lookup"><span data-stu-id="63084-550">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="63084-551">Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="63084-551">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="63084-552">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="63084-552">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="63084-553">Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-553">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="63084-554">uygulama, belirli bir bellek eşiğini (örnek uygulamada 1 GB) kullanıyorsa, sağlıksız bir durumu raporlar `MemoryHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="63084-554">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="63084-555"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-555">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="63084-556">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-556">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-557">Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="63084-557">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="63084-558">Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-558">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="63084-559">Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.</span><span class="sxs-lookup"><span data-stu-id="63084-559">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="63084-560">Örnek uygulamada (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-560">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="63084-561">`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı.</span><span class="sxs-lookup"><span data-stu-id="63084-561">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="63084-562">Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:</span><span class="sxs-lookup"><span data-stu-id="63084-562">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="63084-563">`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:</span><span class="sxs-lookup"><span data-stu-id="63084-563">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="63084-564">Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-564">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="63084-565">[Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-565">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="63084-566">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-566">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="63084-567">Bağlantı noktasına göre filtrele</span><span class="sxs-lookup"><span data-stu-id="63084-567">Filter by port</span></span>

<span data-ttu-id="63084-568"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> bir bağlantı noktası ile çağırmak, belirtilen bağlantı noktasına durum denetimi isteklerini kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="63084-568">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="63084-569">Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-569">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="63084-570">Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="63084-570">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="63084-571">Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-571">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="63084-572">Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63084-572">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="63084-573">Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.</span><span class="sxs-lookup"><span data-stu-id="63084-573">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="63084-574">Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:</span><span class="sxs-lookup"><span data-stu-id="63084-574">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="63084-575">Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin.</span><span class="sxs-lookup"><span data-stu-id="63084-575">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="63084-576"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrısı, yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="63084-576">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="63084-577">Kod içinde açıkça URL 'Leri ve yönetim bağlantı noktasını ayarlayarak *Launchsettings. JSON* dosyasını örnek uygulamada oluşturmaktan kaçınabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="63084-577">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="63084-578"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> için bir çağrı ekleyin ve uygulamanın normal yanıt uç noktasını ve yönetim bağlantı noktası uç noktasını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-578">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="63084-579"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrıldığında, yönetim bağlantı noktasını açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="63084-579">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="63084-580">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="63084-580">*Program.cs*:</span></span>
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
> <span data-ttu-id="63084-581">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="63084-581">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="63084-582">Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:</span><span class="sxs-lookup"><span data-stu-id="63084-582">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="63084-583">Bir sistem durumu denetim kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="63084-583">Distribute a health check library</span></span>

<span data-ttu-id="63084-584">Bir sistem durumu denetimini kitaplık olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="63084-584">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="63084-585"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın.</span><span class="sxs-lookup"><span data-stu-id="63084-585">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="63084-586">Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="63084-586">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="63084-587">`CheckHealthAsync`sistem durumu denetim mantığındaki:</span><span class="sxs-lookup"><span data-stu-id="63084-587">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="63084-588">`data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-588">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="63084-589">`AccessViolationException` işlenir.</span><span class="sxs-lookup"><span data-stu-id="63084-589">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="63084-590">Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.</span><span class="sxs-lookup"><span data-stu-id="63084-590">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="63084-591">Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın.</span><span class="sxs-lookup"><span data-stu-id="63084-591">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="63084-592">Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="63084-592">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="63084-593">Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="63084-593">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="63084-594">Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır.</span><span class="sxs-lookup"><span data-stu-id="63084-594">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="63084-595">Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:</span><span class="sxs-lookup"><span data-stu-id="63084-595">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="63084-596">sistem durumu denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="63084-596">health check name (`name`).</span></span> <span data-ttu-id="63084-597">`null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-597">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="63084-598">sistem durumu denetimi için dize veri noktası (`data1`).</span><span class="sxs-lookup"><span data-stu-id="63084-598">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="63084-599">sistem durumu denetimi için tamsayı veri noktası (`data2`).</span><span class="sxs-lookup"><span data-stu-id="63084-599">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="63084-600">`null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63084-600">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="63084-601">hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="63084-601">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="63084-602">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="63084-602">The default is `null`.</span></span> <span data-ttu-id="63084-603">`null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-603">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="63084-604">Etiketler (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="63084-604">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="63084-605">Sistem durumu denetimi yayımcısı</span><span class="sxs-lookup"><span data-stu-id="63084-605">Health Check Publisher</span></span>

<span data-ttu-id="63084-606">Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır.</span><span class="sxs-lookup"><span data-stu-id="63084-606">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="63084-607">Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-607">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="63084-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:</span><span class="sxs-lookup"><span data-stu-id="63084-608">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="63084-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:</span><span class="sxs-lookup"><span data-stu-id="63084-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="63084-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;.</span><span class="sxs-lookup"><span data-stu-id="63084-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="63084-611">Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="63084-611">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="63084-612">Varsayılan değer beş saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-612">The default value is five seconds.</span></span>
* <span data-ttu-id="63084-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>.</span><span class="sxs-lookup"><span data-stu-id="63084-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="63084-614">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-614">The default value is 30 seconds.</span></span>
* <span data-ttu-id="63084-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="63084-616">Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın.</span><span class="sxs-lookup"><span data-stu-id="63084-616">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="63084-617">Koşul her dönem değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="63084-617">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="63084-618">Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>.</span><span class="sxs-lookup"><span data-stu-id="63084-618"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="63084-619">Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-619">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="63084-620">Varsayılan değer 30 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="63084-620">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="63084-621">ASP.NET Core 2,2 sürümünde, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> ayarı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulama tarafından kabul edilemez; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="63084-621">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="63084-622">Bu sorun ASP.NET Core 3,0 ' de giderilmiştir.</span><span class="sxs-lookup"><span data-stu-id="63084-622">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="63084-623">Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="63084-623">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="63084-624">Durum denetimi durumu her denetim için şu şekilde günlüğe kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="63084-624">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="63084-625">Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).</span><span class="sxs-lookup"><span data-stu-id="63084-625">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="63084-626">Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).</span><span class="sxs-lookup"><span data-stu-id="63084-626">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="63084-627">Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="63084-627">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="63084-628"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:</span><span class="sxs-lookup"><span data-stu-id="63084-628">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="63084-629">Aşağıdaki geçici çözüm, uygulamaya bir veya daha fazla barındırılan hizmet zaten eklendiyse, hizmet kapsayıcısına <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> bir örnek eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="63084-629">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="63084-630">Bu geçici çözüm ASP.NET Core 3,0 ' de gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63084-630">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="63084-631">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.</span><span class="sxs-lookup"><span data-stu-id="63084-631">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="63084-632">[Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="63084-632">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="63084-633">Durum denetimlerini Mapperne zaman kısıtla</span><span class="sxs-lookup"><span data-stu-id="63084-633">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="63084-634">Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.</span><span class="sxs-lookup"><span data-stu-id="63084-634">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="63084-635">Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:</span><span class="sxs-lookup"><span data-stu-id="63084-635">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="63084-636">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="63084-636">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
