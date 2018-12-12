---
title: ASP.NET Core durum denetimleri
author: guardrex
description: Uygulamaları ve veritabanları gibi ASP.NET Core altyapısı için sistem durumu denetimleri ayarlama konusunda bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: 6c1c644b2cd44cd00c68a8fd7d1e7d496ec91a59
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284688"
---
# <a name="health-checks-in-aspnet-core"></a>ASP.NET Core durum denetimleri

Tarafından [Luke Latham](https://github.com/guardrex) ve [Glenn Condron](https://github.com/glennc)

ASP.NET Core, sistem durumu denetleme ara yazılım ve uygulama altyapı bileşenlerini durumunu raporlama kitaplıkları sunar.

Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak kullanıma sunulur. Sistem durumu denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:

* Sistem durumu araştırmaları ve yük Dengeleyiciler, bir uygulamanın durumunu denetlemek için kapsayıcı düzenleyiciler tarafından kullanılabilir. Örneğin, bir kapsayıcı Düzenleyicisi başarısız durum yanıt dağıtım çalışırken veya kapsayıcı yeniden durdurma tarafından kontrol edin. Bir yük dengeleyici sağlıksız bir uygulamaya yönlendirme trafiği sağlıklı bir örneği başarısız örneğine uzağa tepki.
* Bellek, disk ve diğer fiziksel sunucu kaynakları için sistem durumunu izlenebilir.
* Sistem durumu denetimleri, veritabanları ve dış hizmet uç noktaları, kullanılabilirlik ve normal çalışmasına onaylamak için gibi bir uygulamanın bağımlılıklarını test edebilirsiniz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama, bu konuda açıklanan senaryo örnekleri içerir. Belirli bir senaryo için örnek uygulama çalıştırmak için kullandığınız [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) proje klasöründeki bir komut kabuğu komut. Örnek uygulamanın bkz *README.md* dosya ve örnek uygulama kullanma hakkında ayrıntılı bilgi için bu konudaki senaryo açıklamaları.

## <a name="prerequisites"></a>Önkoşullar

Sistem durumu denetimleri, genellikle bir uygulama durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı orchestrator ile kullanılır. Bir uygulama için sistem durumu denetimleri eklemeden önce kullanmak için hangi izleme sistemi karar verin. İzleme sistemi, hangi tür kaynakların oluşturmak için sistem durumu denetimleri ve uç noktalarını yapılandırma belirler.

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paket.

Örnek uygulama için birkaç senaryo durum denetimleri göstermek için başlangıç kodunu sağlar. [Veritabanı araştırma](#database-probe) senaryosu kullanarak bir veritabanı bağlantısı, sistem durumu araştırmaları [BeatPulse](https://github.com/Xabaril/BeatPulse). [DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryo araştırmaları EF Core kullanarak veritabanı `DbContext`. Örnek uygulamayı kullanarak veritabanı senaryolarını keşfetmek için:

* Bir veritabanı oluşturun ve kendi bağlantı dizesinde sağlamak *appsettings.json* uygulamanın dosya.
* Paket başvurusu ekleme [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).

> [!NOTE]
> [BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.

Başka bir sistem durumu denetimi senaryo, bir yönetim noktasına sistem durumu denetimlerini filtrelemek gösterilmektedir. Örnek uygulama oluşturmanızı gerektiren bir *Properties/launchSettings.json* yönetim bağlantı noktası ve yönetim URL'si içeren dosya. Daha fazla bilgi için [bağlantı noktasına göre filtre](#filter-by-port) bölümü.

## <a name="basic-health-probe"></a>Temel durum araştırması

Birçok uygulama için istekleri işlemek için uygulamanın kullanılabilirliğini raporlar bir temel sistem durumu araştırma yapılandırması (*canlılık*) uygulama durumunu bulmak yeterlidir.

Temel yapılandırma sistem durumu denetimi Hizmetleri kaydeder ve bir URL uç noktası ile bir sistem durumu yanıtı, yanıt durumu denetleme Ara çağırır. Varsayılan olarak, hiçbir özel durum denetimleri, herhangi bir belirli bir bağımlılık veya alt sistem test etmek için kaydedilir. Uygulama sistem durumu uç nokta URL'sini yanıtlayabileceği ise sağlıklı olarak kabul edilir. Varsayılan yanıt yazıcı durumu yazar (`HealthStatus`) ya da bir düz metin istemcisine geri yanıt, belirten bir `HealthStatus.Healthy`, `HealthStatus.Degraded` veya `HealthStatus.Unhealthy` durumu.

Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`. Sistem durumu denetleme Ara yazılımla ekleme `UseHealthChecks` istek işleme ardışık düzeninde `Startup.Configure`.

Örnek uygulamada, sistem durumu onay uç noktası oluşturulan `/health` (*BasicStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

Örnek uygulamayı kullanarak temel yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker örneği

[Docker](xref:host-and-deploy/docker/index) yerleşik sunar `HEALTHCHECK` temel sistem durumu Denetim yapılandırması kullanan bir uygulamayı durumunu denetlemek için kullanılan yönergesi:

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>Sistem durumu denetimleri oluşturma

Durum denetimleri oluşturulur uygulayarak `IHealthCheck` arabirimi. `IHealthCheck.CheckHealthAsync` Yöntemi döndürür bir `Task<HealthCheckResult>` durumu bildiren `Healthy`, `Degraded`, veya `Unhealthy`. Sonuç yapılandırılabilir durum koduyla yanıt düz metin olarak yazılır (yapılandırma açıklanan [sistem durumu denetimi seçenekleri](#health-check-options) bölümü). `HealthCheckResult` Ayrıca isteğe bağlı bir anahtar-değer çiftleri döndürebilir.

### <a name="example-health-check"></a>Örnek sistem durumu denetimi

Aşağıdaki `ExampleHealthCheck` sınıfı bir sistem durumu denetimi düzenini gösterir:

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

### <a name="register-health-check-services"></a>Sistem durumu denetimi Hizmetleri Kaydet

`ExampleHealthCheck` Türü, sistem durumu denetimi hizmetleriyle eklenir `AddCheck`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

`AddCheck` Aşırı yüklemesi aşağıdaki örnekte gösterilen hata durumu ayarlar (`HealthStatus`) sistem durumu denetimi hata bildirdiğinde rapor. Hata durumu ayarlanırsa `null` (varsayılan), `HealthStatus.Unhealthy` bildirilir. Bu aşırı yükleme, kitaplık yazar, burada sistem durumu denetimi uygulama ayarı geliştirir, sistem durumu denetimi hatası oluştuğunda kitaplığı tarafından belirtilen hata durumu uygulama tarafından zorlanır için yararlı bir senaryodur.

*Etiketleri* sistem durumu denetimlerini filtrelemek için kullanılabilir (içinde açıklandığı gibi daha fazla [sistem durumu denetimlerini filtrelemek](#filter-health-checks) bölümü).

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

`AddCheck` Ayrıca, bir lambda işlevi yürütebilir. Aşağıdaki örnekte, sistem durumu denetimi adı olarak belirtilen `Example` ve denetimi her zaman iyi durum bildirir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a>Sistem durumu denetimleri ara yazılım kullanın

İçinde `Startup.Configure`, çağrı `UseHealthChecks` işleme ardışık düzeninde uç nokta URL'si veya göreli yol:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

Sistem durumu denetimleri, belirli bir bağlantı noktasında dinleyecek, bir aşırı yüklemesini kullanın. `UseHealthChecks` bağlantı noktası ayarlamak için (içinde açıklandığı gibi daha fazla [bağlantı noktasına göre filtre](#filter-by-port) bölümü):

```csharp
app.UseHealthChecks("/health", port: 8000);
```

Sistem durumu denetimleri ara yazılım bir *terminal ara yazılım* uygulamanın istek işleme ardışık düzeninde. İstek URL'si için bir tam eşleşme karşılaşılan ilk sistem durumu onay uç noktası yürütür ve kalan ara yazılım ardışık düzenini short-circuits. Kısa devre ortaya çıktığında, eşleşen bir sistem durumu denetimini izleyen hiçbir ara yazılım yürütür.

## <a name="health-check-options"></a>Sistem durumu denetimi seçenekleri

`HealthCheckOptions` Sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlar:

* [Sistem durumu denetimlerini filtrelemek](#filter-health-checks)
* [HTTP durum kodu özelleştirme](#customize-the-http-status-code)
* [Önbellek üst bilgilerini gösterme](#suppress-cache-headers)
* [Çıkış özelleştirme](#customize-output)

### <a name="filter-health-checks"></a>Sistem durumu denetimlerini filtrelemek

Varsayılan olarak, sistem durumu denetleme ara yazılım, tüm kayıtlı sistem durumu denetimleri çalıştırır. Sistem durumu denetimleri kümesini çalıştırmak için bir Boole değeri döndüren bir işlev sağlayan `Predicate` seçeneği. Aşağıdaki örnekte, `Bar` sistem durumu denetimi filtrelendi etiketine göre (`bar_tag`) işlevin koşullu deyiminde burada `true` yalnızca, döndürülen sistem durumu denetimini 's `Tag` özellik eşleşme `foo_tag` veya `baz_tag`:

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

### <a name="customize-the-http-status-code"></a>HTTP durum kodu özelleştirme

Kullanım `ResultStatusCodes` HTTP durum kodları için sistem durumunu eşleme özelleştirmek için. Aşağıdaki `StatusCode` atamaları ara yazılım tarafından kullanılan varsayılan değerlerdir. Durum kodu değerlerin gereksinimlerinizi karşılayacak şekilde değiştirin.

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

### <a name="suppress-cache-headers"></a>Önbellek üst bilgilerini gösterme

`AllowCachingResponses` Sistem durumu denetleme ara yazılımın yanıt önbelleğe alınmasını engellemek amacıyla bir araştırma yanıt HTTP üstbilgileri ekler olup olmadığını denetler. Değer ise `false` (varsayılan), ara yazılım ayarlar veya geçersiz kılmaları `Cache-Control`, `Expires`, ve `Pragma` yanıt önbelleğe alınmasını engellemek amacıyla üstbilgileri. Değer ise `true`, ara yazılımın yanıt önbelleği başlıklarını değiştirmez.

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

### <a name="customize-output"></a>Çıkış özelleştirme

`ResponseWriter` Seçeneği alır veya ayarlar yanıt yazmak için kullanılan bir temsilci. Varsayılan temsilci bir dize değeri en az bir düz metin yanıtıyla Yazar `HealthReport.Status`.

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

## <a name="database-probe"></a>Veritabanı yoklama

Sistem durumu denetimi, veritabanı normalde yanıt verip vermediğini belirten bir boolean test olarak çalıştırmak için bir veritabanı sorgusu belirtebilirsiniz.

Örnek uygulama kullandığı [BeatPulse](https://github.com/Xabaril/BeatPulse), SQL Server veritabanında sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamaları için bir sistem durumu denetimi kitaplığı. BeatPulse yürüten bir `SELECT 1` veritabanı bağlantısını doğrulamak için veritabanında sorgu iyi durumda.

> [!WARNING]
> Bir sorgu ile bir veritabanı bağlantısı kontrol ediliyor hızla döndüren bir sorgu seçin. Sorgu yaklaşım veritabanı aşırı yükleme ve performansının düşmesinde riskini çalıştırır. Çoğu durumda, bir test sorgusu çalıştırarak gerekli değildir. Yalnızca veritabanı başarılı bir bağlantı yapmadan yeterli olur. Bir sorgu çalıştırmak gerekli fark ederseniz, basit bir SELECT sorgusunu gibi seçin `SELECT 1`.

BeatPulse kitaplığı kullanmak için bir paket başvuru içeren [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).

Geçerli veritabanı bağlantı dizesini sağlamanız *appsettings.json* örnek uygulamanın dosya. Adlı bir SQL Server veritabanı uygulamanın kullandığı `HealthCheckSample`:

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`. Örnek uygulamayı çağırır BeatPulse'nın `AddSqlServer` veritabanının bağlantı dizesiyle yöntemi (*DbHealthStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario db
```

> [!NOTE]
> [BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext araştırma

`DbContext` Onay kullanan uygulamalarda desteklenen [Entity Framework (EF) çekirdek](/ef/core/). Bu denetimi uygulamanın bir EF Core için yapılandırılmış veritabanı ile iletişim kurabildiğini doğrular `DbContext`. Varsayılan olarak, `DbContextHealthCheck` EF Core'nın çağıran `CanConnectAsync` yöntemi. Kullanarak sistem durumu denetimi, aşırı yüklemeleri hangi işlemin çalıştırıldığı özelleştirebilirsiniz `AddDbContextCheck` yöntemi.

`AddDbContextCheck<TContext>` Sistem durumu denetimi için kayıtları bir `DbContext` (`TContext`). Varsayılan olarak, sistem durumu denetimini adını adıdır `TContext` türü. Aşırı hata durumu, etiketler ve özel test sorgusu yapılandırmak kullanılabilir.

Örnek uygulamada `AppDbContext` için sağlanan `AddDbContextCheck` ve bir hizmet olarak kayıtlı `Startup.ConfigureServices`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

Örnek uygulamada `UseHealthChecks` sistem durumu denetleme Ara yazılımında ekler `Startup.Configure`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

Çalıştırılacak `DbContext` araştırma senaryo örnek uygulaması kullanarak, veritabanı tarafından belirtilen onaylayın bağlantı dizesi SQL Server örneğinde yok. Veritabanı zaten varsa silin.

Proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario dbcontext
```

Bir isteği yaparak uygulama çalışmaya başladıktan sonra durumunu denetleyen `/health` uç noktası tarayıcıda. Veritabanı ve `AppDbContext` yoksa, uygulama şu yanıtı sağlanır:

```
Unhealthy
```

Veritabanını oluşturmak için örnek uygulamayı tetikleyin. İsteğin yapılacağı `/createdatabase`. Uygulama yanıt verir:

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

İsteğin yapılacağı `/health` uç noktası. Uygulama yanıt vermesi veritabanı ve bağlam vardır:

```
Healthy
```

Veritabanını silmek için örnek uygulamayı tetikleyin. İsteğin yapılacağı `/deletedatabase`. Uygulama yanıt verir:

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

İsteğin yapılacağı `/health` uç noktası. Uygulama sağlıksız bir yanıt sağlar:

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>Ayrı hazırlık ve canlılık araştırmaları

Barındırma bazı senaryolarda, sistem durumu denetimleri çifti kullanılır ayırt edilebilen iki uygulama durumu:

* Uygulama düzgün ancak henüz isteklerini almak hazır. Bu durumda uygulamanın *hazırlık*.
* Uygulamanın çalışmasını ve isteklere yanıt. Bu durumda uygulamanın *canlılık*.

Hazır olma denetimi genellikle tüm uygulama alt sistemlerinin ve kaynaklarınıza uygun olup olmadığını belirlemek için denetimler daha kapsamlı ve zaman alıcı bir kümesini gerçekleştirir. Canlılık onay yalnızca uygulama isteklerini işlemek için kullanılabilir olup olmadığını belirlemek için hızlı bir denetim gerçekleştirir. Uygulama, hazır olma denetimi geçtikten sonra uygulamayı daha pahalı hazırlık denetimleri kümesiyle yük gerek yoktur&mdash;başka denetimler yalnızca gereksinim için canlılık denetleniyor.

Örnek uygulamayı tamamlanması uzun süre çalışan başlangıç görevinde bildirmek için sistem durumu denetimi içeren bir [barındırılan hizmet](xref:fundamentals/host/hosted-services). `StartupHostedServiceHealthCheck` Bir özellik sunan `StartupTaskCompleted`, barındırılan hizmet ayarlayabileceğiniz `true` , uzun süre çalışan görev tamamlandığında (*StartupHostedServiceHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

Uzun süre çalışan arka plan görevi tarafından başlatılan bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetleri/StartupHostedService*). Sonunda görev `StartupHostedServiceHealthCheck.StartupTaskCompleted` ayarlanır `true`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

Sistem durumu denetimini kayıtlı `AddCheck` içinde `Startup.ConfigureServices` barındırılan hizmet ile birlikte. Barındırılan hizmet üzerinde sistem denetimi özelliği ayarlamanız gerekir çünkü sistem durumu denetimi aynı zamanda service kapsayıcısında kayıtlı (*LivenessProbeStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`. Örnek uygulamada, sistem durumu denetimi uç noktaları, oluşturulan `/health/ready` için hazır olma denetimi ve `/health/live` canlılık olup olmadığını kontrol edin. Durum denetimleri için Sistem Hazırlık denetimi filtrelerini denetleyin ile `ready` etiketi. Canlılık onay filtreler `StartupHostedServiceHealthCheck` döndürerek `false` içinde `HealthCheckOptions.Predicate` (daha fazla bilgi için [sistem durumu denetimlerini filtrelemek](#filter-health-checks)):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

Örnek uygulama kullanma hazırlığı/canlılık yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario liveness
```

Bir tarayıcıda ziyaret `/health/ready` birkaç kez 15 saniye geçtikten kadar. Sistem durumunu denetleme raporları `Unhealthy` ilk 15 saniye. Uç nokta 15 saniye sonra raporları `Healthy`, barındırılan hizmeti tarafından tamamlanması uzun süre çalışan görev yansıtır.

### <a name="kubernetes-example"></a>Kubernetes örneği

Ayrı hazırlık ve canlılık denetimleri kullanarak, bir ortamda kullanışlıdır gibi [Kubernetes](https://kubernetes.io/). Kubernetes, bir uygulama gibi bir temel alınan veritabanı kullanılabilirlik testi, istekleri kabul önce zaman başlatma işi gerçekleştirmek için gerekli olabilir. Ayrı denetimleri kullanarak orchestrator ayırt etmek için uygulamayı çalışan ancak henüz hazır olup olmadığını veya uygulamayı başlatmak çalıştıramadığını sağlar. Hazır olma ve kubernetes kapsayıcısında eşdeğerlik araştırmaları hakkında daha fazla bilgi için bkz. [canlılık yapılandırmak ve hazırlık araştırmaları](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) Kubernetes belgelerinde.

Aşağıdaki örnek, bir Kubernetes hazır olma durumu araştırması yapılandırması gösterilmektedir:

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a>Özel bir yanıt yazıcı ile ölçüm tabanlı araştırma

Örnek uygulama, özel bir yanıt yazıcı ile bellek sistem durumu denetimi gösterir.

`MemoryHealthCheck` uygulama birden çok belirli bir eşiği (örnek uygulamada 1 GB) bellek kullanıyorsa, düzeyi düşürülmüş durum bildirir. `HealthCheckResult` Uygulamanın atık toplayıcı (GC) bilgiler içerir (*MemoryHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`. Sistem durumu etkinleştirmek yerine geçirerek denetleyin `AddCheck`, `MemoryHealthCheck` hizmet olarak kaydedilir. Tüm `IHealthCheck` kaydedilen Hizmetleri ara yazılım ve sistem durumu denetimi Hizmetleri için kullanılabilir. Sistem durumu denetimi Hizmetleri tek hizmet olarak kaydettirme öneririz.

*CustomWriterStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

Uygulama işleme işlem hattı, sistem durumu denetleme ara yazılım çağırın `Startup.Configure`. A `WriteResponse` temsilci sağlanan `ResponseWriter` özelliği sistem durumu denetimini yürütüldüğünde, özel bir JSON yanıtı çıktısını almak için:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

`WriteResponse` Yöntemi biçimleri `CompositeHealthCheckResult` bir JSON nesnesi ve sistem durumu denetiminin yanıtı için JSON çıktısını verir:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

Ölçüm tabanlı araştırma örnek uygulaması kullanarak özel bir yanıt yazıcı çıkış çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario writer
```

> [!NOTE]
> [BeatPulse](https://github.com/Xabaril/BeatPulse) disk depolama ve en yüksek değer canlılık denetimleri de dahil olmak üzere, ölçüm tabanlı bir sistem durumu denetimi senaryolar içerir.
>
> [BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.

## <a name="filter-by-port"></a>Bağlantı noktası göre filtrele

Çağırma `UseHealthChecks` bir bağlantı noktası ile belirtilen bağlantı noktası için sistem durumu onay istekleri kısıtlar. Bu genellikle, bir kapsayıcı ortamında hizmetleri izlemek için bir bağlantı noktasını kullanıma sunma için kullanılır.

Örnek uygulamayı kullanarak bağlantı noktasını yapılandırır [ortam değişkeni yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#environment-variables-configuration-provider). Bağlantı noktası olarak *launchSettings.json* dosyasını ve yapılandırma sağlayıcısı için bir ortam değişkeni aracılığıyla geçirildi. Ayrıca, sunucunun yönetim noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.

Yönetim bağlantı noktası yapılandırması göstermek için örnek uygulamayı kullanmak için oluşturma *launchSettings.json* dosyası bir *özellikleri* klasör.

Aşağıdaki *launchSettings.json* dosya örnek uygulamanın proje dosyalarında bulunmaz ve el ile oluşturulması gerekir.

*Properties/launchSettings.json*:

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

Sistem durumu denetimi hizmetleriyle kaydetme `AddHealthChecks` içinde `Startup.ConfigureServices`. Çağrı `UseHealthChecks` yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> Oluşturmaktan kaçının *launchSettings.json* yönetim bağlantı noktası ve URL'leri kodda açıkça ayarlayarak örnek uygulama dosyasında. İçinde *Program.cs* burada `WebHostBuilder` olan oluşturulmuş bir çağrı ekleyin `UseUrls` uygulamanın normal yanıt uç noktası ve yönetim bağlantı noktası uç noktasını girin. İçinde *ManagementPortStartup.cs* burada `UseHealthChecks` olan yönetim bağlantı noktası olarak adlandırılan, açıkça belirtin.
>
> *Program.cs*:
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
> *ManagementPortStartup.cs*:
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

Örnek uygulamayı kullanarak yönetim bağlantı noktası yapılandırma senaryosu çalıştırmak için proje klasöründeki bir komut kabuğu'nda aşağıdaki komutu yürütün:

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>Bir sistem durumu denetimi kitaplığı dağıtma

Sistem durumu denetimi kitaplığı olarak dağıtmak için:

1. Uygulayan bir sistem durumu denetimi yazma `IHealthCheck` arabirimi olarak tek başına bir sınıf. Sınıf üzerinde güvenebilirsiniz [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), etkinleştirme, yazın ve [seçenekleri adlı](xref:fundamentals/configuration/options) yapılandırma verilerine erişmek için.

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

1. Tüketim uygulama çağrıları parametrelere sahip bir uzantı metodu yazma kendi `Startup.Configure` yöntemi. Aşağıdaki örnekte, aşağıdaki sistem durumu denetimi yöntem imzasını varsayın:

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   İmza belirten `ExampleHealthCheck` ek veri işleme durumu araştırma mantığını denetleyin gerektirir. Verileri, sistem durumu denetimi bir uzantı yönteminiz olarak kaydedildiğinde sistem durumu denetimi örneği oluşturmak için kullanılan temsilci için sağlanır. Aşağıdaki örnekte, arayanın isteğe bağlı belirtir:

   * Sistem durumu Denetim adı (`name`). Varsa `null`, `example_health_check` kullanılır.
   * Dize veri noktası için sistem durumu denetimi (`data1`).
   * tamsayı veri noktası için sistem durumu denetimi (`data2`). Varsa `null`, `1` kullanılır.
   * hata durumu (`HealthStatus`). Varsayılan, `null` değeridir. Varsa `null`, `HealthStatus.Unhealthy` için bir hata durumu bildirilir.
   * etiketleri (`IEnumerable<string>`).

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

## <a name="health-check-publisher"></a>Sistem durumu denetimi yayımcı

Olduğunda bir `IHealthCheckPublisher` eklenen hizmet kapsayıcısı için sistem durumu denetimi sistemi düzenli olarak yürüten sistem durumu denetimleri ve çağrıları `PublishAsync` sonuç. İzleme sistemi, durumu belirlemek için düzenli olarak çağırmak için her bir işlemi bekliyor sistem senaryosu gönderim tabanlı sistem durumu izleme kullanışlıdır.

`IHealthCheckPublisher` Arabirimi tek bir yöntemi vardır:

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> [BeatPulse](https://github.com/Xabaril/BeatPulse) dahil olmak üzere çeşitli sistemler için yayımcılar içerir [Application Insights](/azure/application-insights/app-insights-overview).
>
> [BeatPulse](https://github.com/Xabaril/BeatPulse) tutulan veya Microsoft tarafından desteklenmiyor.
