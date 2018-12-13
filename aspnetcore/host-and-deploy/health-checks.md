---
title: ASP.NET Core durum denetimleri
author: guardrex
description: Uygulamaları ve veritabanları gibi ASP.NET Core altyapısı için sistem durumu denetimleri ayarlama konusunda bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/12/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: cf2aea91221887dad5646604214f810493d4b175
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329152"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="06e08-103">ASP.NET Core durum denetimleri</span><span class="sxs-lookup"><span data-stu-id="06e08-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="06e08-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="06e08-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="06e08-105">ASP.NET Core, sistem durumu denetleme ara yazılım ve uygulama altyapı bileşenlerini durumunu raporlama kitaplıkları sunar.</span><span class="sxs-lookup"><span data-stu-id="06e08-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="06e08-106">Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="06e08-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="06e08-107">Sistem durumu denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="06e08-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="06e08-108">Sistem durumu araştırmaları ve yük Dengeleyiciler, bir uygulamanın durumunu denetlemek için kapsayıcı düzenleyiciler tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="06e08-109">Örneğin, bir kapsayıcı Düzenleyicisi başarısız durum yanıt dağıtım çalışırken veya kapsayıcı yeniden durdurma tarafından kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="06e08-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="06e08-110">Bir yük dengeleyici sağlıksız bir uygulamaya yönlendirme trafiği sağlıklı bir örneği başarısız örneğine uzağa tepki.</span><span class="sxs-lookup"><span data-stu-id="06e08-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="06e08-111">Bellek, disk ve diğer fiziksel sunucu kaynakları için sistem durumunu izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="06e08-112">Sistem durumu denetimleri, veritabanları ve dış hizmet uç noktaları, kullanılabilirlik ve normal çalışmasına onaylamak için gibi bir uygulamanın bağımlılıklarını test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06e08-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="06e08-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="06e08-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="06e08-114">Örnek uygulama, bu konuda açıklanan senaryo örnekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="06e08-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="06e08-115">Belirli bir senaryo için örnek uygulama çalıştırmak için kullandığınız [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) proje klasöründeki bir komut kabuğu komut.</span><span class="sxs-lookup"><span data-stu-id="06e08-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="06e08-116">Örnek uygulamanın bkz *README.md* dosya ve örnek uygulama kullanma hakkında ayrıntılı bilgi için bu konudaki senaryo açıklamaları.</span><span class="sxs-lookup"><span data-stu-id="06e08-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06e08-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="06e08-117">Prerequisites</span></span>

<span data-ttu-id="06e08-118">Sistem durumu denetimleri, genellikle bir uygulama durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı orchestrator ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06e08-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="06e08-119">Bir uygulama için sistem durumu denetimleri eklemeden önce kullanmak için hangi izleme sistemi karar verin.</span><span class="sxs-lookup"><span data-stu-id="06e08-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="06e08-120">İzleme sistemi, hangi tür kaynakların oluşturmak için sistem durumu denetimleri ve uç noktalarını yapılandırma belirler.</span><span class="sxs-lookup"><span data-stu-id="06e08-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="06e08-121">Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paket.</span><span class="sxs-lookup"><span data-stu-id="06e08-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="06e08-122">Başlangıç kodu için birkaç senaryo durum denetimleri göstermek için örnek uygulamayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="06e08-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="06e08-123">[Veritabanı araştırma](#database-probe) senaryo denetimleri kullanarak bir veritabanı bağlantı durumu [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="06e08-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="06e08-124">[DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryo denetler EF Core kullanarak veritabanı `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="06e08-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="06e08-125">Örnek uygulamayı veritabanına senaryolarını keşfetmek için:</span><span class="sxs-lookup"><span data-stu-id="06e08-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="06e08-126">Bir veritabanı oluşturur ve kendi bağlantı dizesinde sağlar *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="06e08-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="06e08-127">Aşağıdaki paket başvuruları, proje dosyasında sahiptir:</span><span class="sxs-lookup"><span data-stu-id="06e08-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="06e08-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="06e08-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="06e08-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="06e08-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="06e08-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="06e08-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="06e08-131">Başka bir sistem durumu denetimi senaryo, bir yönetim noktasına sistem durumu denetimlerini filtrelemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="06e08-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="06e08-132">Örnek uygulama oluşturmanızı gerektiren bir *Properties/launchSettings.json* yönetim bağlantı noktası ve yönetim URL'si içeren dosya.</span><span class="sxs-lookup"><span data-stu-id="06e08-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="06e08-133">Daha fazla bilgi için [bağlantı noktasına göre filtre](#filter-by-port) bölümü.</span><span class="sxs-lookup"><span data-stu-id="06e08-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="06e08-134">Temel durum araştırması</span><span class="sxs-lookup"><span data-stu-id="06e08-134">Basic health probe</span></span>

<span data-ttu-id="06e08-135">Birçok uygulama için istekleri işlemek için uygulamanın kullanılabilirliğini raporlar bir temel sistem durumu araştırma yapılandırması (*canlılık*) uygulama durumunu bulmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="06e08-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="06e08-136">Temel yapılandırma sistem durumu denetimi Hizmetleri kaydeder ve bir URL uç noktası ile bir sistem durumu yanıtı, yanıt durumu denetleme Ara çağırır.</span><span class="sxs-lookup"><span data-stu-id="06e08-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="06e08-137">Varsayılan olarak, hiçbir özel durum denetimleri, herhangi bir belirli bir bağımlılık veya alt sistem test etmek için kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="06e08-138">Uygulama sistem durumu uç nokta URL'sini yanıtlayabileceği ise sağlıklı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="06e08-139">Varsayılan yanıt yazıcı durumu yazar (`HealthStatus`) ya da bir düz metin istemcisine geri yanıt, belirten bir `HealthStatus.Healthy`, `HealthStatus.Degraded` veya `HealthStatus.Unhealthy` durumu.</span><span class="sxs-lookup"><span data-stu-id="06e08-139">The default response writer writes the status (`HealthStatus`) as a plaintext response back to the client, indicating either a `HealthStatus.Healthy`, `HealthStatus.Degraded` or `HealthStatus.Unhealthy` status.</span></span>

<span data-ttu-id="06e08-140">Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06e08-140">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="06e08-141">Sistem durumu denetleme Ara yazılımla ekleme `UseHealthChecks` istek işleme ardışık düzeninde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="06e08-141">Add Health Check Middleware with `UseHealthChecks` in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="06e08-142">Örnek uygulamada, sistem durumu onay uç noktası oluşturulan `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="06e08-143">Örnek uygulamayı kullanarak temel yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="06e08-144">Docker örneği</span><span class="sxs-lookup"><span data-stu-id="06e08-144">Docker example</span></span>

<span data-ttu-id="06e08-145">[Docker](xref:host-and-deploy/docker/index) yerleşik sunar `HEALTHCHECK` temel sistem durumu Denetim yapılandırması kullanan bir uygulamayı durumunu denetlemek için kullanılan yönergesi:</span><span class="sxs-lookup"><span data-stu-id="06e08-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="06e08-146">Sistem durumu denetimleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="06e08-146">Create health checks</span></span>

<span data-ttu-id="06e08-147">Durum denetimleri oluşturulur uygulayarak `IHealthCheck` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="06e08-147">Health checks are created by implementing the `IHealthCheck` interface.</span></span> <span data-ttu-id="06e08-148">`IHealthCheck.CheckHealthAsync` Yöntemi döndürür bir `Task<HealthCheckResult>` durumu bildiren `Healthy`, `Degraded`, veya `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="06e08-148">The `IHealthCheck.CheckHealthAsync` method returns a `Task<HealthCheckResult>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="06e08-149">Sonuç yapılandırılabilir durum koduyla yanıt düz metin olarak yazılır (yapılandırma açıklanan [sistem durumu denetimi seçenekleri](#health-check-options) bölümü).</span><span class="sxs-lookup"><span data-stu-id="06e08-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="06e08-150">`HealthCheckResult` Ayrıca isteğe bağlı bir anahtar-değer çiftleri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-150">`HealthCheckResult` can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="06e08-151">Örnek sistem durumu denetimi</span><span class="sxs-lookup"><span data-stu-id="06e08-151">Example health check</span></span>

<span data-ttu-id="06e08-152">Aşağıdaki `ExampleHealthCheck` sınıfı bir sistem durumu denetimi düzenini gösterir:</span><span class="sxs-lookup"><span data-stu-id="06e08-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
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

### <a name="register-health-check-services"></a><span data-ttu-id="06e08-153">Sistem durumu denetimi Hizmetleri Kaydet</span><span class="sxs-lookup"><span data-stu-id="06e08-153">Register health check services</span></span>

<span data-ttu-id="06e08-154">`ExampleHealthCheck` Türü, sistem durumu denetimi hizmetleriyle eklenir `AddCheck`:</span><span class="sxs-lookup"><span data-stu-id="06e08-154">The `ExampleHealthCheck` type is added to health check services with `AddCheck`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="06e08-155">`AddCheck` Aşırı yüklemesi aşağıdaki örnekte gösterilen hata durumu ayarlar (`HealthStatus`) sistem durumu denetimi hata bildirdiğinde rapor.</span><span class="sxs-lookup"><span data-stu-id="06e08-155">The `AddCheck` overload shown in the following example sets the failure status (`HealthStatus`) to report when the health check reports a failure.</span></span> <span data-ttu-id="06e08-156">Hata durumu ayarlanırsa `null` (varsayılan), `HealthStatus.Unhealthy` bildirilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-156">If the failure status is set to `null` (default), `HealthStatus.Unhealthy` is reported.</span></span> <span data-ttu-id="06e08-157">Bu aşırı yükleme, kitaplık yazar, burada sistem durumu denetimi uygulama ayarı geliştirir, sistem durumu denetimi hatası oluştuğunda kitaplığı tarafından belirtilen hata durumu uygulama tarafından zorlanır için yararlı bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="06e08-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="06e08-158">*Etiketleri* sistem durumu denetimlerini filtrelemek için kullanılabilir (içinde açıklandığı gibi daha fazla [sistem durumu denetimlerini filtrelemek](#filter-health-checks) bölümü).</span><span class="sxs-lookup"><span data-stu-id="06e08-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="06e08-159">`AddCheck` Ayrıca, bir lambda işlevi yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-159">`AddCheck` can also execute a lambda function.</span></span> <span data-ttu-id="06e08-160">Aşağıdaki örnekte, sistem durumu denetimi adı olarak belirtilen `Example` ve denetimi her zaman iyi durum bildirir:</span><span class="sxs-lookup"><span data-stu-id="06e08-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="06e08-161">Sistem durumu denetimleri ara yazılım kullanın</span><span class="sxs-lookup"><span data-stu-id="06e08-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="06e08-162">İçinde `Startup.Configure`, çağrı `UseHealthChecks` işleme ardışık düzeninde uç nokta URL'si veya göreli yol:</span><span class="sxs-lookup"><span data-stu-id="06e08-162">In `Startup.Configure`, call `UseHealthChecks` in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="06e08-163">Sistem durumu denetimleri, belirli bir bağlantı noktasında dinleyecek, bir aşırı yüklemesini kullanın. `UseHealthChecks` bağlantı noktası ayarlamak için (içinde açıklandığı gibi daha fazla [bağlantı noktasına göre filtre](#filter-by-port) bölümü):</span><span class="sxs-lookup"><span data-stu-id="06e08-163">If the health checks should listen on a specific port, use an overload of `UseHealthChecks` to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="06e08-164">Sistem durumu denetimleri ara yazılım bir *terminal ara yazılım* uygulamanın istek işleme ardışık düzeninde.</span><span class="sxs-lookup"><span data-stu-id="06e08-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="06e08-165">İstek URL'si için bir tam eşleşme karşılaşılan ilk sistem durumu onay uç noktası yürütür ve kalan ara yazılım ardışık düzenini short-circuits.</span><span class="sxs-lookup"><span data-stu-id="06e08-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="06e08-166">Kısa devre ortaya çıktığında, eşleşen bir sistem durumu denetimini izleyen hiçbir ara yazılım yürütür.</span><span class="sxs-lookup"><span data-stu-id="06e08-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="06e08-167">Sistem durumu denetimi seçenekleri</span><span class="sxs-lookup"><span data-stu-id="06e08-167">Health check options</span></span>

<span data-ttu-id="06e08-168">`HealthCheckOptions` Sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlar:</span><span class="sxs-lookup"><span data-stu-id="06e08-168">`HealthCheckOptions` provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="06e08-169">Sistem durumu denetimlerini filtrelemek</span><span class="sxs-lookup"><span data-stu-id="06e08-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="06e08-170">HTTP durum kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="06e08-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="06e08-171">Önbellek üst bilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="06e08-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="06e08-172">Çıkış özelleştirme</span><span class="sxs-lookup"><span data-stu-id="06e08-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="06e08-173">Sistem durumu denetimlerini filtrelemek</span><span class="sxs-lookup"><span data-stu-id="06e08-173">Filter health checks</span></span>

<span data-ttu-id="06e08-174">Varsayılan olarak, sistem durumu denetleme ara yazılım, tüm kayıtlı sistem durumu denetimleri çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="06e08-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="06e08-175">Sistem durumu denetimleri kümesini çalıştırmak için bir Boole değeri döndüren bir işlev sağlayan `Predicate` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="06e08-175">To run a subset of health checks, provide a function that returns a boolean to the `Predicate` option.</span></span> <span data-ttu-id="06e08-176">Aşağıdaki örnekte, `Bar` sistem durumu denetimi filtrelendi etiketine göre (`bar_tag`) işlevin koşullu deyiminde burada `true` yalnızca, döndürülen sistem durumu denetimini 's `Tag` özellik eşleşme `foo_tag` veya `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="06e08-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's `Tag` property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="06e08-177">HTTP durum kodu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="06e08-177">Customize the HTTP status code</span></span>

<span data-ttu-id="06e08-178">Kullanım `ResultStatusCodes` HTTP durum kodları için sistem durumunu eşleme özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="06e08-178">Use `ResultStatusCodes` to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="06e08-179">Aşağıdaki `StatusCode` atamaları ara yazılım tarafından kullanılan varsayılan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="06e08-179">The following `StatusCode` assignments are the default values used by the middleware.</span></span> <span data-ttu-id="06e08-180">Durum kodu değerlerin gereksinimlerinizi karşılayacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="06e08-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="06e08-181">Önbellek üst bilgilerini gösterme</span><span class="sxs-lookup"><span data-stu-id="06e08-181">Suppress cache headers</span></span>

<span data-ttu-id="06e08-182">`AllowCachingResponses` Sistem durumu denetleme ara yazılımın yanıt önbelleğe alınmasını engellemek amacıyla bir araştırma yanıt HTTP üstbilgileri ekler olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="06e08-182">`AllowCachingResponses` controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="06e08-183">Değer ise `false` (varsayılan), ara yazılım ayarlar veya geçersiz kılmaları `Cache-Control`, `Expires`, ve `Pragma` yanıt önbelleğe alınmasını engellemek amacıyla üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="06e08-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="06e08-184">Değer ise `true`, ara yazılımın yanıt önbelleği başlıklarını değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="06e08-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="06e08-185">Çıkış özelleştirme</span><span class="sxs-lookup"><span data-stu-id="06e08-185">Customize output</span></span>

<span data-ttu-id="06e08-186">`ResponseWriter` Seçeneği alır veya ayarlar yanıt yazmak için kullanılan bir temsilci.</span><span class="sxs-lookup"><span data-stu-id="06e08-186">The `ResponseWriter` option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="06e08-187">Varsayılan temsilci bir dize değeri en az bir düz metin yanıtıyla Yazar `HealthReport.Status`.</span><span class="sxs-lookup"><span data-stu-id="06e08-187">The default delegate writes a minimal plaintext response with the string value of `HealthReport.Status`.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
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

## <a name="database-probe"></a><span data-ttu-id="06e08-188">Veritabanı yoklama</span><span class="sxs-lookup"><span data-stu-id="06e08-188">Database probe</span></span>

<span data-ttu-id="06e08-189">Sistem durumu denetimi, veritabanı normalde yanıt verip vermediğini belirten bir boolean test olarak çalıştırmak için bir veritabanı sorgusu belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="06e08-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="06e08-190">Örnek uygulama kullandığı [BeatPulse](https://github.com/Xabaril/BeatPulse), SQL Server veritabanında sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamaları için bir sistem durumu denetimi kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="06e08-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="06e08-191">BeatPulse yürüten bir `SELECT 1` veritabanı bağlantısını doğrulamak için veritabanında sorgu iyi durumda.</span><span class="sxs-lookup"><span data-stu-id="06e08-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="06e08-192">Bir sorgu ile bir veritabanı bağlantısı kontrol ediliyor hızla döndüren bir sorgu seçin.</span><span class="sxs-lookup"><span data-stu-id="06e08-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="06e08-193">Sorgu yaklaşım veritabanı aşırı yükleme ve performansının düşmesinde riskini çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="06e08-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="06e08-194">Çoğu durumda, bir test sorgusu çalıştırarak gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="06e08-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="06e08-195">Yalnızca veritabanı başarılı bir bağlantı yapmadan yeterli olur.</span><span class="sxs-lookup"><span data-stu-id="06e08-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="06e08-196">Bir sorgu çalıştırmak gerekli fark ederseniz, basit bir SELECT sorgusunu gibi seçin `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="06e08-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="06e08-197">BeatPulse kitaplığı kullanmak için bir paket başvuru içeren [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="06e08-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="06e08-198">Geçerli veritabanı bağlantı dizesini sağlamanız *appsettings.json* örnek uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="06e08-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="06e08-199">Adlı bir SQL Server veritabanı uygulamanın kullandığı `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="06e08-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="06e08-200">Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06e08-200">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="06e08-201">Örnek uygulamayı çağırır BeatPulse'nın `AddSqlServer` veritabanının bağlantı dizesiyle yöntemi (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="06e08-202">Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="06e08-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="06e08-203">Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="06e08-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="06e08-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="06e08-205">Entity Framework Core DbContext araştırma</span><span class="sxs-lookup"><span data-stu-id="06e08-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="06e08-206">`DbContext` Onay onaylar uygulamayı bir EF Core için yapılandırılmış veritabanı ile iletişim kurabildiğini `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="06e08-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="06e08-207">`DbContext` Onay uygulamalarında desteklenen:</span><span class="sxs-lookup"><span data-stu-id="06e08-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="06e08-208">Kullanım [Entity Framework (EF) çekirdek](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="06e08-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="06e08-209">Bir paket başvuru eklemek [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="06e08-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="06e08-210">`AddDbContextCheck<TContext>` Sistem durumu denetimi için kayıtları bir `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="06e08-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="06e08-211">`DbContext` Olarak sağlanan `TContext` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="06e08-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="06e08-212">Aşırı hata durumu, etiketler ve özel test sorgusu yapılandırmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="06e08-213">Varsayılan olarak:</span><span class="sxs-lookup"><span data-stu-id="06e08-213">By default:</span></span>

* <span data-ttu-id="06e08-214">`DbContextHealthCheck` EF Core'nın çağıran `CanConnectAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="06e08-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="06e08-215">Hangi işlemin kullanarak sistem durumu denetlenirken çalıştırıldığı özelleştirebilirsiniz `AddDbContextCheck` yöntemi aşırı yüklemeleri.</span><span class="sxs-lookup"><span data-stu-id="06e08-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="06e08-216">Sistem durumu denetimini adını adıdır `TContext` türü.</span><span class="sxs-lookup"><span data-stu-id="06e08-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="06e08-217">Örnek uygulamada `AppDbContext` için sağlanan `AddDbContextCheck` ve bir hizmet olarak kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06e08-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="06e08-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="06e08-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="06e08-219">Örnek uygulamada `UseHealthChecks` sistem durumu denetleme Ara yazılımında ekler `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="06e08-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="06e08-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="06e08-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="06e08-221">Çalıştırılacak `DbContext` araştırma senaryo örnek uygulaması kullanarak, veritabanı tarafından belirtilen onaylayın bağlantı dizesi SQL Server örneğinde yok.</span><span class="sxs-lookup"><span data-stu-id="06e08-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="06e08-222">Veritabanı zaten varsa silin.</span><span class="sxs-lookup"><span data-stu-id="06e08-222">If the database exists, delete it.</span></span>

<span data-ttu-id="06e08-223">Proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="06e08-224">Bir isteği yaparak uygulama çalışmaya başladıktan sonra durumunu denetleyen `/health` uç noktası tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="06e08-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="06e08-225">Veritabanı ve `AppDbContext` yoksa, uygulama şu yanıtı sağlanır:</span><span class="sxs-lookup"><span data-stu-id="06e08-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="06e08-226">Veritabanını oluşturmak için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="06e08-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="06e08-227">İsteğin yapılacağı `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="06e08-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="06e08-228">Uygulama yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="06e08-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="06e08-229">İsteğin yapılacağı `/health` uç noktası.</span><span class="sxs-lookup"><span data-stu-id="06e08-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="06e08-230">Uygulama yanıt vermesi veritabanı ve bağlam vardır:</span><span class="sxs-lookup"><span data-stu-id="06e08-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="06e08-231">Veritabanını silmek için örnek uygulamayı tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="06e08-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="06e08-232">İsteğin yapılacağı `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="06e08-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="06e08-233">Uygulama yanıt verir:</span><span class="sxs-lookup"><span data-stu-id="06e08-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="06e08-234">İsteğin yapılacağı `/health` uç noktası.</span><span class="sxs-lookup"><span data-stu-id="06e08-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="06e08-235">Uygulama sağlıksız bir yanıt sağlar:</span><span class="sxs-lookup"><span data-stu-id="06e08-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="06e08-236">Ayrı hazırlık ve canlılık araştırmaları</span><span class="sxs-lookup"><span data-stu-id="06e08-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="06e08-237">Barındırma bazı senaryolarda, sistem durumu denetimleri çifti kullanılır ayırt edilebilen iki uygulama durumu:</span><span class="sxs-lookup"><span data-stu-id="06e08-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="06e08-238">Uygulama düzgün ancak henüz isteklerini almak hazır.</span><span class="sxs-lookup"><span data-stu-id="06e08-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="06e08-239">Bu durumda uygulamanın *hazırlık*.</span><span class="sxs-lookup"><span data-stu-id="06e08-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="06e08-240">Uygulamanın çalışmasını ve isteklere yanıt.</span><span class="sxs-lookup"><span data-stu-id="06e08-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="06e08-241">Bu durumda uygulamanın *canlılık*.</span><span class="sxs-lookup"><span data-stu-id="06e08-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="06e08-242">Hazır olma denetimi genellikle tüm uygulama alt sistemlerinin ve kaynaklarınıza uygun olup olmadığını belirlemek için denetimler daha kapsamlı ve zaman alıcı bir kümesini gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="06e08-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="06e08-243">Canlılık onay yalnızca uygulama isteklerini işlemek için kullanılabilir olup olmadığını belirlemek için hızlı bir denetim gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="06e08-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="06e08-244">Uygulama, hazır olma denetimi geçtikten sonra uygulamayı daha pahalı hazırlık denetimleri kümesiyle yük gerek yoktur&mdash;başka denetimler yalnızca gereksinim için canlılık denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="06e08-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="06e08-245">Örnek uygulamayı tamamlanması uzun süre çalışan başlangıç görevinde bildirmek için sistem durumu denetimi içeren bir [barındırılan hizmet](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="06e08-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="06e08-246">`StartupHostedServiceHealthCheck` Bir özellik sunan `StartupTaskCompleted`, barındırılan hizmet ayarlayabileceğiniz `true` , uzun süre çalışan görev tamamlandığında (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="06e08-247">Uzun süre çalışan arka plan görevi tarafından başlatılan bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetleri/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="06e08-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="06e08-248">Sonunda görev `StartupHostedServiceHealthCheck.StartupTaskCompleted` ayarlanır `true`:</span><span class="sxs-lookup"><span data-stu-id="06e08-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="06e08-249">Sistem durumu denetimini kayıtlı `AddCheck` içinde `Startup.ConfigureServices` barındırılan hizmet ile birlikte.</span><span class="sxs-lookup"><span data-stu-id="06e08-249">The health check is registered with `AddCheck` in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="06e08-250">Barındırılan hizmet üzerinde sistem denetimi özelliği ayarlamanız gerekir çünkü sistem durumu denetimi aynı zamanda service kapsayıcısında kayıtlı (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="06e08-251">Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="06e08-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="06e08-252">Örnek uygulamada, sistem durumu denetimi uç noktaları, oluşturulan `/health/ready` için hazır olma denetimi ve `/health/live` canlılık olup olmadığını kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="06e08-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="06e08-253">Durum denetimleri için Sistem Hazırlık denetimi filtrelerini denetleyin ile `ready` etiketi.</span><span class="sxs-lookup"><span data-stu-id="06e08-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="06e08-254">Canlılık onay filtreler `StartupHostedServiceHealthCheck` döndürerek `false` içinde `HealthCheckOptions.Predicate` (daha fazla bilgi için [sistem durumu denetimlerini filtrelemek](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="06e08-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the `HealthCheckOptions.Predicate` (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="06e08-255">Örnek uygulama kullanma hazırlığı/canlılık yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="06e08-256">Bir tarayıcıda ziyaret `/health/ready` birkaç kez 15 saniye geçtikten kadar.</span><span class="sxs-lookup"><span data-stu-id="06e08-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="06e08-257">Sistem durumunu denetleme raporları `Unhealthy` ilk 15 saniye.</span><span class="sxs-lookup"><span data-stu-id="06e08-257">The health check reports `Unhealthy` for the first 15 seconds.</span></span> <span data-ttu-id="06e08-258">Uç nokta 15 saniye sonra raporları `Healthy`, barındırılan hizmeti tarafından tamamlanması uzun süre çalışan görev yansıtır.</span><span class="sxs-lookup"><span data-stu-id="06e08-258">After 15 seconds, the endpoint reports `Healthy`, which reflects the completion of the long-running task by the hosted service.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="06e08-259">Kubernetes örneği</span><span class="sxs-lookup"><span data-stu-id="06e08-259">Kubernetes example</span></span>

<span data-ttu-id="06e08-260">Ayrı hazırlık ve canlılık denetimleri kullanarak, bir ortamda kullanışlıdır gibi [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="06e08-260">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="06e08-261">Kubernetes, bir uygulama gibi bir temel alınan veritabanı kullanılabilirlik testi, istekleri kabul önce zaman başlatma işi gerçekleştirmek için gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-261">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="06e08-262">Ayrı denetimleri kullanarak orchestrator ayırt etmek için uygulamayı çalışan ancak henüz hazır olup olmadığını veya uygulamayı başlatmak çalıştıramadığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="06e08-262">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="06e08-263">Hazır olma ve kubernetes kapsayıcısında eşdeğerlik araştırmaları hakkında daha fazla bilgi için bkz. [canlılık yapılandırmak ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) Kubernetes belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="06e08-263">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="06e08-264">Aşağıdaki örnek, bir Kubernetes hazır olma durumu araştırması yapılandırması gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="06e08-264">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="06e08-265">Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma</span><span class="sxs-lookup"><span data-stu-id="06e08-265">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="06e08-266">Örnek uygulama, özel bir yanıt yazıcı ile bellek sistem durumu denetimi gösterir.</span><span class="sxs-lookup"><span data-stu-id="06e08-266">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="06e08-267">`MemoryHealthCheck` uygulama birden çok belirli bir eşiği (örnek uygulamada 1 GB) bellek kullanıyorsa, düzeyi düşürülmüş durum bildirir.</span><span class="sxs-lookup"><span data-stu-id="06e08-267">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="06e08-268">`HealthCheckResult` Uygulamanın atık toplayıcı (GC) bilgiler içerir (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-268">The `HealthCheckResult` includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="06e08-269">Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06e08-269">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="06e08-270">Sistem durumu etkinleştirmek yerine geçirerek denetleyin `AddCheck`, `MemoryHealthCheck` hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-270">Instead of enabling the health check by passing it to `AddCheck`, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="06e08-271">Tüm `IHealthCheck` kaydedilen Hizmetleri ara yazılım ve sistem durumu denetimi Hizmetleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-271">All `IHealthCheck` registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="06e08-272">Sistem durumu denetimi Hizmetleri tek hizmet olarak kaydettirme öneririz.</span><span class="sxs-lookup"><span data-stu-id="06e08-272">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="06e08-273">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="06e08-273">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="06e08-274">Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="06e08-274">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="06e08-275">A `WriteResponse` temsilci sağlanan `ResponseWriter` özelliği sistem durumu denetimini yürütüldüğünde, özel bir JSON yanıtı çıktısını almak için:</span><span class="sxs-lookup"><span data-stu-id="06e08-275">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="06e08-276">`WriteResponse` Yöntemi biçimleri `CompositeHealthCheckResult` bir JSON nesnesi ve sistem durumu denetiminin yanıtı için JSON çıktısını verir:</span><span class="sxs-lookup"><span data-stu-id="06e08-276">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="06e08-277">Ölçüm tabanlı araştırma örnek uygulaması kullanarak özel bir yanıt yazıcı çıkış çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-277">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="06e08-278">[BeatPulse](https://github.com/Xabaril/BeatPulse) disk depolama ve en yüksek değer canlılık denetimleri de dahil olmak üzere, ölçüm tabanlı bir sistem durumu denetimi senaryolar içerir.</span><span class="sxs-lookup"><span data-stu-id="06e08-278">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="06e08-279">[BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="06e08-279">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="06e08-280">Bağlantı noktası göre filtrele</span><span class="sxs-lookup"><span data-stu-id="06e08-280">Filter by port</span></span>

<span data-ttu-id="06e08-281">Çağırma `UseHealthChecks` bir bağlantı noktası ile belirtilen bağlantı noktası için sistem durumu onay istekleri kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="06e08-281">Calling `UseHealthChecks` with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="06e08-282">Bu genellikle, bir kapsayıcı ortamında hizmetleri izlemek için bir bağlantı noktasını kullanıma sunma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06e08-282">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="06e08-283">Örnek uygulamayı kullanarak bağlantı noktasını yapılandırır [ortam değişkeni yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="06e08-283">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="06e08-284">Bağlantı noktası olarak *launchSettings.json* dosyasını ve yapılandırma sağlayıcısı için bir ortam değişkeni aracılığıyla geçirildi.</span><span class="sxs-lookup"><span data-stu-id="06e08-284">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="06e08-285">Ayrıca, sunucunun yönetim noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="06e08-285">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="06e08-286">Yönetim bağlantı noktası yapılandırması göstermek için örnek uygulamayı kullanmak için oluşturma *launchSettings.json* dosyası bir *özellikleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="06e08-286">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="06e08-287">Aşağıdaki *launchSettings.json* dosya örnek uygulamanın proje dosyalarında bulunmaz ve el ile oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="06e08-287">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="06e08-288">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="06e08-288">*Properties/launchSettings.json*:</span></span>

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

<span data-ttu-id="06e08-289">Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="06e08-289">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="06e08-290">Çağrı `UseHealthChecks` yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="06e08-290">The call to `UseHealthChecks` specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="06e08-291">Oluşturmaktan kaçının *launchSettings.json* yönetim bağlantı noktası ve URL'leri kodda açıkça ayarlayarak örnek uygulama dosyasında.</span><span class="sxs-lookup"><span data-stu-id="06e08-291">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="06e08-292">İçinde *Program.cs* burada `WebHostBuilder` olan oluşturulmuş bir çağrı ekleyin `UseUrls` uygulamanın normal yanıt uç noktası ve yönetim bağlantı noktası uç noktasını girin.</span><span class="sxs-lookup"><span data-stu-id="06e08-292">In *Program.cs* where the `WebHostBuilder` is created, add a call to `UseUrls` and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="06e08-293">İçinde *ManagementPortStartup.cs* burada `UseHealthChecks` olan yönetim bağlantı noktası olarak adlandırılan, açıkça belirtin.</span><span class="sxs-lookup"><span data-stu-id="06e08-293">In *ManagementPortStartup.cs* where `UseHealthChecks` is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="06e08-294">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="06e08-294">*Program.cs*:</span></span>
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
> <span data-ttu-id="06e08-295">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="06e08-295">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="06e08-296">Örnek uygulamayı kullanarak yönetim bağlantı noktası yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="06e08-296">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="06e08-297">Bir sistem durumu denetimi kitaplığı dağıtma</span><span class="sxs-lookup"><span data-stu-id="06e08-297">Distribute a health check library</span></span>

<span data-ttu-id="06e08-298">Sistem durumu denetimi kitaplığı olarak dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="06e08-298">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="06e08-299">Uygulayan bir sistem durumu denetimi yazma `IHealthCheck` arabirimi olarak tek başına bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="06e08-299">Write a health check that implements the `IHealthCheck` interface as a standalone class.</span></span> <span data-ttu-id="06e08-300">Sınıf üzerinde güvenebilirsiniz [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), etkinleştirme, yazın ve [seçenekleri adlı](xref:fundamentals/configuration/options) yapılandırma verilerine erişmek için.</span><span class="sxs-lookup"><span data-stu-id="06e08-300">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

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
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

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

1. <span data-ttu-id="06e08-301">Tüketim uygulama çağrıları parametrelere sahip bir uzantı metodu yazma kendi `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="06e08-301">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="06e08-302">Aşağıdaki örnekte, aşağıdaki sistem durumu denetimi yöntem imzasını varsayın:</span><span class="sxs-lookup"><span data-stu-id="06e08-302">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="06e08-303">İmza belirten `ExampleHealthCheck` ek veri işleme durumu araştırma mantığını denetleyin gerektirir.</span><span class="sxs-lookup"><span data-stu-id="06e08-303">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="06e08-304">Verileri, sistem durumu denetimi bir uzantı yönteminiz olarak kaydedildiğinde sistem durumu denetimi örneği oluşturmak için kullanılan temsilci için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="06e08-304">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="06e08-305">Aşağıdaki örnekte, arayanın isteğe bağlı belirtir:</span><span class="sxs-lookup"><span data-stu-id="06e08-305">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="06e08-306">Sistem durumu Denetim adı (`name`).</span><span class="sxs-lookup"><span data-stu-id="06e08-306">health check name (`name`).</span></span> <span data-ttu-id="06e08-307">Varsa `null`, `example_health_check` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06e08-307">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="06e08-308">Dize veri noktası için sistem durumu denetimi (`data1`).</span><span class="sxs-lookup"><span data-stu-id="06e08-308">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="06e08-309">tamsayı veri noktası için sistem durumu denetimi (`data2`).</span><span class="sxs-lookup"><span data-stu-id="06e08-309">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="06e08-310">Varsa `null`, `1` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="06e08-310">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="06e08-311">hata durumu (`HealthStatus`).</span><span class="sxs-lookup"><span data-stu-id="06e08-311">failure status (`HealthStatus`).</span></span> <span data-ttu-id="06e08-312">Varsayılan, `null` değeridir.</span><span class="sxs-lookup"><span data-stu-id="06e08-312">The default is `null`.</span></span> <span data-ttu-id="06e08-313">Varsa `null`, `HealthStatus.Unhealthy` için bir hata durumu bildirilir.</span><span class="sxs-lookup"><span data-stu-id="06e08-313">If `null`, `HealthStatus.Unhealthy` is reported for a failure status.</span></span>
   * <span data-ttu-id="06e08-314">etiketleri (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="06e08-314">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="06e08-315">Sistem durumu denetimi yayımcı</span><span class="sxs-lookup"><span data-stu-id="06e08-315">Health Check Publisher</span></span>

<span data-ttu-id="06e08-316">Olduğunda bir `IHealthCheckPublisher` eklenen hizmet kapsayıcısı için sistem durumu denetimi sistemi düzenli olarak yürüten sistem durumu denetimleri ve çağrıları `PublishAsync` sonuç.</span><span class="sxs-lookup"><span data-stu-id="06e08-316">When an `IHealthCheckPublisher` is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="06e08-317">İzleme sistemi, durumu belirlemek için düzenli olarak çağırmak için her bir işlemi bekliyor sistem senaryosu gönderim tabanlı sistem durumu izleme kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="06e08-317">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="06e08-318">`IHealthCheckPublisher` Arabirimi tek bir yöntemi vardır:</span><span class="sxs-lookup"><span data-stu-id="06e08-318">The `IHealthCheckPublisher` interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> <span data-ttu-id="06e08-319">[BeatPulse](https://github.com/Xabaril/BeatPulse) dahil olmak üzere çeşitli sistemler için yayımcılar içerir [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="06e08-319">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="06e08-320">[BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="06e08-320">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
