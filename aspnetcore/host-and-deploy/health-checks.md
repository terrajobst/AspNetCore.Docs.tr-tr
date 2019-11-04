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
# <a name="health-checks-in-aspnet-core"></a>ASP.NET Core durum denetimleri

[Luke Latham](https://github.com/guardrex) ve [Glenn CONDRON](https://github.com/glennc) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.

Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir. Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:

* Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir. Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir. Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.
* Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.
* Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir. Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın. Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.

## <a name="prerequisites"></a>Prerequisites

Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır. Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin. İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.

[Microsoft. AspNetCore. Diagnostics. Healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine ASP.NET Core uygulamaları için örtük olarak başvurulur. Entity Framework Core kullanarak sistem durumu denetimleri gerçekleştirmek için, [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) paketine bir paket başvurusu ekleyin.

Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar. [Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler. [DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler. Örnek uygulama olan veritabanı senaryolarını araştırmak için:

* Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.
* , Proje dosyasında aşağıdaki paket başvurularına sahiptir:
  * [AspNetCore. Healthdenetimleri. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir. Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir. Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.

## <a name="basic-health-probe"></a>Temel sistem durumu araştırması

Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.

Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir. Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir. Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir. Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. `Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.

Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):

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

Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker örneği

[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>Durum denetimleri oluşturma

Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür. Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır). <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.

Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir. Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir. Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar. `healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.

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

## <a name="register-health-check-services"></a>Sistem durumu denetimi hizmetlerini Kaydet

`ExampleHealthCheck` türü, `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> sistem durumu denetimi hizmetlerine eklenir:

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar. Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir. Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.

*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir. Aşağıdaki örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a>Sistem durumu denetimleri yönlendirmeyi kullanma

`Startup.Configure`, uç nokta URL 'SI veya göreli yol ile Endpoint Builder üzerinde `MapHealthChecks` çağırın:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a>Konak gerektir

Sistem durumu denetimi uç noktası için bir veya daha fazla izin verilen Konakları belirtmek üzere `RequireHost` çağrısı yapın. Konaklar, puni kodu yerine Unicode olmalıdır ve bir bağlantı noktası içerebilir. Bir koleksiyon sağlanmazsa, herhangi bir konak kabul edilir.

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.

### <a name="require-authorization"></a>Yetkilendirme gerektir

Sistem durumu denetimi istek uç noktasında yetkilendirme ara yazılımını çalıştırmak için `RequireAuthorization` çağırın. `RequireAuthorization` aşırı yükleme bir veya daha fazla yetkilendirme ilkesini kabul eder. Bir ilke sağlanmazsa, varsayılan yetkilendirme ilkesi kullanılır.

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a>Kaynaklar Arası İstekleri (CORS) etkinleştirme

Bir tarayıcıdan el ile sistem durumu denetimleri gerçekleştirmek yaygın kullanım senaryosu olmamasına karşın, CORS ara yazılımı `RequireCors` sistem durumu denetimleri uç noktalarında çağırarak etkinleştirilebilir. `RequireCors` aşırı yüklemesi CORS ilke Oluşturucu temsilcisini (`CorsPolicyBuilder`) veya bir ilke adını kabul eder. Bir ilke sağlanmazsa varsayılan CORS ilkesi kullanılır. Daha fazla bilgi için bkz. <xref:security/cors>.

## <a name="health-check-options"></a>Sistem durumu denetimi seçenekleri

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:

* [Durum denetimlerini filtrele](#filter-health-checks)
* [HTTP durum kodunu özelleştirme](#customize-the-http-status-code)
* [Önbellek üstbilgilerini gösterme](#suppress-cache-headers)
* [Çıktıyı özelleştirme](#customize-output)

### <a name="filter-health-checks"></a>Durum denetimlerini filtrele

Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır. Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın. Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür. :

`Startup.ConfigureServices`:

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

`Startup.Configure`, `Predicate` ' Bar ' sistem durumu denetimini filtreler. Yalnızca Foo ve baz Execute.:

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

### <a name="customize-the-http-status-code"></a>HTTP durum kodunu özelleştirme

Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın. Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir. Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.

`Startup.Configure`:

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

### <a name="suppress-cache-headers"></a>Önbellek üstbilgilerini gösterme

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler. Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar. Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.

`Startup.Configure`:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a>Çıktıyı özelleştirme

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar.

`Startup.Configure`:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar. Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:

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

Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür. Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.

## <a name="database-probe"></a>Veritabanı araştırması

Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.

Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır. `AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.

> [!WARNING]
> Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin. Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır. Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir. Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir. Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.

[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.

Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın. Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="entity-framework-core-dbcontext-probe"></a>DbContext araştırması Entity Framework Core

`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar. `DbContext` denetimi şu uygulamalar için desteklenir:

* [Entity Framework (EF) Core](/ef/core/)kullanın.
* [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.

`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder. `DbContext`, metoduna `TContext` olarak sağlanır. Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.

Varsayılan olarak:

* `DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır. `AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.
* Sistem durumu denetiminin adı `TContext` türünün adıdır.

Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun. Veritabanı varsa, silin.

Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario dbcontext
```

Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin. Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:

```
Unhealthy
```

Veritabanını oluşturmak için örnek uygulamayı tetikleyin. `/createdatabase`için bir istek oluşturun. Uygulama yanıt veriyor:

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

`/health` uç noktasına bir istek oluşturun. Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:

```
Healthy
```

Veritabanını silmek için örnek uygulamayı tetikleyin. `/deletedatabase`için bir istek oluşturun. Uygulama yanıt veriyor:

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

`/health` uç noktasına bir istek oluşturun. Uygulama sağlıksız bir yanıt sağlar:

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>Ayrı hazırlık ve lizlilik araştırmaları

Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:

* Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma. Bu durum, uygulamanın *hazır olma*durumu.
* Uygulama çalışır ve isteklere yanıt verir. Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.

Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir. Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir. Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.

Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir. `StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır. Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir. Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur. Örnek uygulamada, sistem durumu denetimi uç noktaları şu konumda oluşturulur:

* Hazırlık denetimi için `/health/ready`. Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler.
* libir denetim için `/health/live`. Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks))

Aşağıdaki örnek kodda:

* Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.
* `Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.

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

Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:

```dotnetcli
dotnet run --scenario liveness
```

Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin. Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor. 15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.

Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur. Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.

### <a name="kubernetes-example"></a>Kubernetes örneği

Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır. Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir. Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır. Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .

Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:

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

Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.

uygulama, belirli bir bellek eşiğine (örnek uygulamada 1 GB) daha fazlasını kullanıyorsa `MemoryHealthCheck`. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir. Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir. Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.

Örnek uygulamada (*CustomWriterStartup.cs*):

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

`Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetim uç noktası oluşturulur. Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.
>
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="filter-by-port"></a>Bağlantı noktasına göre filtrele

Belirtilen bağlantı noktasına sistem durumu denetim isteklerini kısıtlamak için bir bağlantı noktası belirten bir URL düzeniyle `MapHealthChecks` `RequireHost` çağırın. Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.

Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır. Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir. Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.

Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.

Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:

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

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. `Startup.Configure``MapHealthChecks` çağırarak bir sistem durumu denetimi uç noktası oluşturun.

Örnek uygulamada, `Startup.Configure` uç noktasındaki `RequireHost` çağrısı, yapılandırmadan yönetim bağlantı noktasını belirtir:

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

Uç noktalar `Startup.Configure`örnek uygulamada oluşturulur. Aşağıdaki örnek kodda:

* Hazır olma denetimi ' hazır ' etiketiyle tüm kayıtlı denetimleri kullanır.
* `Predicate` tüm denetimleri dışladığı ve 200-Tamam döndürüyor.

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
> Yönetim bağlantı noktasını kodda açıkça ayarlayarak, örnek uygulamada *Launchsettings. JSON* dosyasını oluşturmaktan kaçınabilirsiniz. <xref:Microsoft.Extensions.Hosting.HostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> için bir çağrı ekleyin ve uygulamanın yönetim bağlantı noktası uç noktasını sağlayın. *ManagementPortStartup.cs*`Configure` `RequireHost`ile yönetim bağlantı noktasını belirtin:
>
> *Program.cs*:
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
> *ManagementPortStartup.cs*:
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>Bir sistem durumu denetim kitaplığı dağıtma

Bir sistem durumu denetimini kitaplık olarak dağıtmak için:

1. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın. Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.

   `CheckHealthAsync`sistem durumu denetim mantığındaki:

   * `data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.
   * `AccessViolationException` işlenir.

   Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.

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

1. Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın. Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir. Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır. Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:

   * sistem durumu denetim adı (`name`). `null`, `example_health_check` kullanılır.
   * sistem durumu denetimi için dize veri noktası (`data1`).
   * sistem durumu denetimi için tamsayı veri noktası (`data2`). `null`, `1` kullanılır.
   * hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>). Varsayılan, `null` değeridir. `null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.
   * Etiketler (`IEnumerable<string>`).

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

## <a name="health-check-publisher"></a>Sistem durumu denetimi yayımcısı

Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır. Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;. Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz. Varsayılan değer beş saniyedir.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>. Varsayılan değer 30 saniyedir.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır. Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın. Koşul her dönem değerlendirilir.
* Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>. Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın. Varsayılan değer 30 saniyedir.

Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır. Sistem durumu denetimi durumu her denetim için günlük düzeyinde günlüğe kaydedilir:

* Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).
* Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.
>
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="restrict-health-checks-with-mapwhen"></a>Durum denetimlerini Mapperne zaman kısıtla

Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.

Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:

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

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core, uygulama altyapısı bileşenlerinin sistem durumunu raporlamak için sistem durumu denetimleri ve kitaplıkları sunar.

Sistem durumu denetimleri, bir uygulama tarafından HTTP uç noktaları olarak gösterilir. Durum denetimi uç noktaları, çeşitli gerçek zamanlı izleme senaryoları için yapılandırılabilir:

* Sistem durumu araştırmaları, kapsayıcı yöneticileri ve yük dengeleyiciler tarafından bir uygulamanın durumunu denetlemek için kullanılabilir. Örneğin, bir kapsayıcı Orchestrator, sıralı bir dağıtımı kaldırarak veya kapsayıcıyı yeniden başlatarak başarısız olan bir sistem durumu denetimine yanıt verebilir. Yük dengeleyici, trafiği başarısız olan örnekten sağlıklı bir örneğe yönlendirerek sağlıklı olmayan bir uygulamaya tepki verebilir.
* Bellek, disk ve diğer fiziksel sunucu kaynaklarının kullanımı sağlıklı bir durum için izlenebilir.
* Sistem durumu denetimleri, kullanılabilirliği ve normal çalışmayı onaylamak için bir uygulamanın veritabanları ve dış hizmet uç noktaları gibi bağımlılıklarını test edebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama, bu konuda açıklanan senaryoların örneklerini içerir. Örnek uygulamayı belirli bir senaryo için çalıştırmak için, bir komut kabuğunda projenin klasöründen [DotNet Run](/dotnet/core/tools/dotnet-run) komutunu kullanın. Örnek uygulamayı kullanma hakkında ayrıntılı bilgi için bu konudaki örnek uygulamanın *README.MD* dosyasına ve senaryo açıklamalarına bakın.

## <a name="prerequisites"></a>Prerequisites

Durum denetimleri, genellikle bir uygulamanın durumunu denetlemek için bir dış izleme hizmeti veya kapsayıcı Orchestrator ile birlikte kullanılır. Bir uygulamaya sistem durumu denetimleri eklemeden önce, hangi izleme sisteminin kullanılacağını belirleyin. İzleme sistemi ne tür bir sistem durumu denetimi oluşturulacağını ve bunların uç noktalarını nasıl yapılandıracağınızı belirler.

Microsoft. aspnetcore [. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Aspnetcore. Diagnostics. healthdenetimlerin](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) paketine bir paket başvurusu ekleyin.

Örnek uygulama, çeşitli senaryolar için sistem durumu denetimlerini göstermek üzere başlangıç kodu sağlar. [Veritabanı araştırma](#database-probe) senaryosu, [Aspnetcore. Diagnostics. healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanarak bir veritabanı bağlantısının sistem durumunu denetler. [DbContext araştırma](#entity-framework-core-dbcontext-probe) senaryosu, bir EF Core `DbContext`kullanarak bir veritabanını denetler. Örnek uygulama olan veritabanı senaryolarını araştırmak için:

* Bir veritabanı oluşturur ve bunun bağlantı dizesini *appSettings. JSON* dosyasında sağlar.
* , Proje dosyasında aşağıdaki paket başvurularına sahiptir:
  * [AspNetCore. Healthdenetimleri. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

Başka bir sistem durumu denetimi senaryosunda, sistem durumu denetimlerinin bir yönetim bağlantı noktasına nasıl filtreleneceği gösterilir. Örnek uygulama, Yönetim URL 'sini ve yönetim bağlantı noktasını içeren bir *Özellikler/launchSettings. JSON* dosyası oluşturmanızı gerektirir. Daha fazla bilgi için, [bağlantı noktasına göre filtrele](#filter-by-port) bölümüne bakın.

## <a name="basic-health-probe"></a>Temel sistem durumu araştırması

Birçok uygulama için, uygulamanın istekleri işleme için kullanılabilirliğini raporlayan temel bir durum araştırma yapılandırması, uygulamanın durumunu öğrenmekiçin yeterlidir.

Temel yapılandırma, sistem durumu denetimi hizmetlerini kaydeder ve sistem durumu denetimleri ara yazılımını çağırarak bir URL uç noktasında bir sistem durumu yanıtı ile yanıt verir. Varsayılan olarak, belirli bir bağımlılığı veya alt sistemi test etmek için belirli bir sistem durumu denetimi kayıtlı değildir. Sistem durumu uç nokta URL 'sinde yanıt veriyorsa, uygulama sağlıklı olarak değerlendirilir. Varsayılan yanıt yazıcı, durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) istemciye geri düz metin yanıtı olarak yazar [. sağlıklı](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)durum, sistem sağlığı durumu [. düşürülmüş](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) veya [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) durum.

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. `Startup.Configure`istek işleme ardışık düzeninde <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> olan sistem durumu denetimleri ara yazılımı için bir uç nokta ekleyin.

Örnek uygulamada, durum denetimi uç noktası `/health` oluşturulur (*BasicStartup.cs*):

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

Örnek uygulamayı kullanarak temel yapılandırma senaryosunu çalıştırmak için, komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker örneği

[Docker](xref:host-and-deploy/docker/index) , temel sistem durumu denetimi yapılandırmasını kullanan bir uygulamanın durumunu denetlemek için kullanılabilen yerleşik bir `HEALTHCHECK` yönergesi sunar:

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>Durum denetimleri oluşturma

Durum denetimleri <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimi uygulayarak oluşturulur. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> yöntemi, sistem durumunu `Healthy`, `Degraded`veya `Unhealthy`olarak belirten bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> döndürür. Sonuç yapılandırılabilir bir durum kodu ile düz metin yanıtı olarak yazılır (yapılandırma, [sistem durumu denetimi seçenekleri](#health-check-options) bölümünde açıklanmıştır). <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, isteğe bağlı anahtar-değer çiftleri de döndürebilir.

### <a name="example-health-check"></a>Örnek sistem durumu denetimi

Aşağıdaki `ExampleHealthCheck` sınıfı, bir sistem durumu denetiminin yerleşimini gösterir. Durum denetimleri mantığı `CheckHealthAsync` yöntemine yerleştirilir. Aşağıdaki örnek, `true`için `healthCheckResultHealthy`bir kukla değişken ayarlar. `healthCheckResultHealthy` değeri `false`olarak ayarlanırsa [Healthcheckresult. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) durum döndürülür.

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

### <a name="register-health-check-services"></a>Sistem durumu denetimi hizmetlerini Kaydet

`ExampleHealthCheck` türü <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>ile `Startup.ConfigureServices` sistem durumu denetimi hizmetlerine eklenir:

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

Aşağıdaki örnekte gösterilen <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> aşırı yüklemesi, sistem durumu denetimi bir hata bildirdiğinde hata durumunu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) ayarlar. Hata durumu `null` (varsayılan) olarak ayarlandıysa, [HealthStatus. sağlıksız](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) olarak bildirilir. Bu aşırı yükleme, kitaplık yazarları için yararlı bir senaryodur. burada, sistem durumu denetimi uygulaması ayarı varsa, bir sistem durumu denetimi hatası oluştuğunda, kitaplık tarafından belirtilen başarısızlık durumu uygulama tarafından zorlanır.

*Etiketler* , sistem durumu denetimlerini filtrelemek için kullanılabilir ( [durum denetimleri filtreleme](#filter-health-checks) bölümünde daha ayrıntılı olarak açıklanmıştır).

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> bir Lambda işlevi de yürütebilir. Aşağıdaki `Startup.ConfigureServices` örnekte, sistem durumu denetim adı `Example` olarak belirtilir ve denetim her zaman sağlıklı bir durum döndürür:

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a>Sistem durumu denetimleri ara yazılımı kullan

`Startup.Configure`, işlem hattının uç nokta URL 'SI veya göreli yolu ile <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağırın:

```csharp
app.UseHealthChecks("/health");
```

Durum denetimlerinin belirli bir bağlantı noktasını dinlemesi gerekiyorsa, bağlantı noktasını ayarlamak için <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> aşırı yüklemesini kullanın ( [bağlantı noktasına göre filtrele](#filter-by-port) bölümünde anlatılmıştır):

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a>Sistem durumu denetimi seçenekleri

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> sistem durumu denetimi davranışını özelleştirmek için bir fırsat sağlayın:

* [Durum denetimlerini filtrele](#filter-health-checks)
* [HTTP durum kodunu özelleştirme](#customize-the-http-status-code)
* [Önbellek üstbilgilerini gösterme](#suppress-cache-headers)
* [Çıktıyı özelleştirme](#customize-output)

### <a name="filter-health-checks"></a>Durum denetimlerini filtrele

Varsayılan olarak, sistem durumu denetimleri ara yazılımı tüm kayıtlı sistem durumu denetimlerini çalıştırır. Bir sistem durumu denetimleri alt kümesini çalıştırmak için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> seçeneğine Boole değeri döndüren bir işlev sağlayın. Aşağıdaki örnekte, `Bar` sistem durumu denetimi, işlevin koşullu deyimindeki etiketiyle (`bar_tag`) filtrelenerek `true` yalnızca sistem durumu denetiminin <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> özelliği `foo_tag` veya `baz_tag`eşleşiyorsa döndürülür. :

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

### <a name="customize-the-http-status-code"></a>HTTP durum kodunu özelleştirme

Sistem durumunun HTTP durum kodlarına eşlenmesini özelleştirmek için <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> kullanın. Aşağıdaki <xref:Microsoft.AspNetCore.Http.StatusCodes> atamaları, ara yazılım tarafından kullanılan varsayılan değerlerdir. Durum kodu değerlerini gereksinimlerinize uyacak şekilde değiştirin.

`Startup.Configure`:

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

### <a name="suppress-cache-headers"></a>Önbellek üstbilgilerini gösterme

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>, yanıt önbelleğini engellemek için sistem durumu denetimlerinin ara yazılım tarafından araştırma yanıtına HTTP üstbilgileri ekleyip eklemediğini denetler. Değer `false` (varsayılan) ise, ara yazılım, yanıt önbelleğe almayı engellemek için `Cache-Control`, `Expires`ve `Pragma` üst bilgilerini ayarlar veya geçersiz kılar. Değer `true`ise, ara yazılım yanıtın önbellek üstbilgilerini değiştirmez.

`Startup.Configure`:

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a>Çıktıyı özelleştirme

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> seçeneği, yanıtı yazmak için kullanılan bir temsilciyi alır veya ayarlar. Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar.

`Startup.Configure`:

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

Varsayılan temsilci, [HealthReport. Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)dize değerine sahip en az bir düz metin yanıtı yazar. Aşağıdaki özel temsilci, `WriteResponse`özel bir JSON yanıtı verir:

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

Durum denetimleri sistemi, karmaşık JSON dönüş biçimleri için yerleşik destek sağlamaz çünkü biçim, sizin tercih ettiğiniz izleme sistemine özgüdür. Gereksinimlerinizi karşılamak için gereken önceki örnekteki `JObject` özelleştirebilirsiniz.

## <a name="database-probe"></a>Veritabanı araştırması

Bir sistem durumu denetimi, veritabanının normal olarak yanıt verip vermediğini göstermek üzere Boole testi olarak çalışacak bir veritabanı sorgusu belirtebilir.

Örnek uygulama, SQL Server veritabanında bir sistem durumu denetimi gerçekleştirmek için ASP.NET Core uygulamalar için bir sistem durumu denetim kitaplığı olan [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)kullanır. `AspNetCore.Diagnostics.HealthChecks` veritabanı bağlantısının sağlıklı olduğundan emin olmak için veritabanında `SELECT 1` bir sorgu yürütür.

> [!WARNING]
> Bir sorgu ile bir veritabanı bağlantısı denetlerken, hızlı bir şekilde dönen bir sorgu seçin. Sorgu yaklaşımı, veritabanını aşırı yükleme ve performansını düşürmeye yönelik riski çalıştırır. Çoğu durumda, test sorgusunun çalıştırılması gerekli değildir. Yalnızca veritabanına başarılı bir bağlantı oluşturmak yeterlidir. Bir sorgu çalıştırmak için gerekli olduğunu fark ederseniz, `SELECT 1`gibi basit bir seçme sorgusu seçin.

[Aspnetcore. Healthdenetimlerin. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)öğesine bir paket başvurusu ekleyin.

Örnek uygulamanın *appSettings. JSON* dosyasında geçerli bir veritabanı bağlantı dizesi sağlayın. Uygulama, `HealthCheckSample`adında bir SQL Server veritabanı kullanır:

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. Örnek uygulama, veritabanının bağlantı dizesiyle `AddSqlServer` yöntemini çağırır (*DbHealthStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

Uygulama işleme işlem hattının `Startup.Configure`içindeki sistem durumu denetimleri ara yazılımını çağır:

```csharp
app.UseHealthChecks("/health");
```

Örnek uygulamayı kullanarak veritabanı araştırma senaryosunu çalıştırmak için, bir komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="entity-framework-core-dbcontext-probe"></a>DbContext araştırması Entity Framework Core

`DbContext` denetimi, uygulamanın bir EF Core `DbContext`için yapılandırılmış veritabanıyla iletişim kurabildiğini onaylar. `DbContext` denetimi şu uygulamalar için desteklenir:

* [Entity Framework (EF) Core](/ef/core/)kullanın.
* [Microsoft. Extensions. Diagnostics. Healthdenetimleri. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)'a bir paket başvurusu ekleyin.

`AddDbContextCheck<TContext>` bir `DbContext`sistem durumu denetimi kaydeder. `DbContext`, metoduna `TContext` olarak sağlanır. Hata durumu, Etiketler ve özel bir test sorgusunu yapılandırmak için aşırı yükleme kullanılabilir.

Varsayılan olarak:

* `DbContextHealthCheck` EF Core `CanConnectAsync` yöntemini çağırır. `AddDbContextCheck` yöntemi aşırı yüklerini kullanarak sistem durumunu denetlerken hangi işlemin çalıştırılacağını özelleştirebilirsiniz.
* Sistem durumu denetiminin adı `TContext` türünün adıdır.

Örnek uygulamada, `AppDbContext` `AddDbContextCheck` ve hizmet olarak `Startup.ConfigureServices` (*DbContextHealthStartup.cs*) olarak kaydedilir.

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

Örnek uygulamada, `UseHealthChecks` sistem durumu denetimleri ara yazılımını `Startup.Configure`ekler.

```csharp
app.UseHealthChecks("/health");
```

Örnek uygulamayı kullanarak `DbContext` araştırma senaryosunu çalıştırmak için, bağlantı dizesi tarafından belirtilen veritabanının SQL Server örneğinde mevcut olmadığından emin olun. Veritabanı varsa, silin.

Komut kabuğunda projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario dbcontext
```

Uygulama çalıştırıldıktan sonra, bir tarayıcıda `/health` uç noktasına bir istek yaparak sistem durumunu denetleyin. Veritabanı ve `AppDbContext` olmadığından, uygulama aşağıdaki yanıtı sağlar:

```
Unhealthy
```

Veritabanını oluşturmak için örnek uygulamayı tetikleyin. `/createdatabase`için bir istek oluşturun. Uygulama yanıt veriyor:

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

`/health` uç noktasına bir istek oluşturun. Veritabanı ve bağlam var, bu nedenle uygulama yanıt veriyor:

```
Healthy
```

Veritabanını silmek için örnek uygulamayı tetikleyin. `/deletedatabase`için bir istek oluşturun. Uygulama yanıt veriyor:

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

`/health` uç noktasına bir istek oluşturun. Uygulama sağlıksız bir yanıt sağlar:

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>Ayrı hazırlık ve lizlilik araştırmaları

Bazı barındırma senaryolarında iki uygulama durumunu ayırt eden bir çift sistem durumu denetimi kullanılır:

* Uygulama çalışıyor ancak henüz istekleri almaya hazırlanma. Bu durum, uygulamanın *hazır olma*durumu.
* Uygulama çalışır ve isteklere yanıt verir. Bu durum, uygulamanın kullanım *koşullarına*göre yapılır.

Hazırlık denetimi genellikle uygulamanın tüm alt sistemlerinin ve kaynaklarının kullanılabilir olup olmadığını belirlemede daha kapsamlı ve zaman alıcı denetim kümesi gerçekleştirir. Bir onay işareti yalnızca uygulamanın istekleri işlemek için kullanılabilir olup olmadığını belirlemede hızlı bir denetim gerçekleştirir. Uygulama, hazırlık denetimini geçirdikten sonra, yüksek hazırlık denetimleri kümesi ile uygulamayı daha fazla yüklemeye gerek yoktur&mdash;daha fazla denetim için yalnızca lileme denetimi gerektirir.

Örnek uygulama, [barındırılan bir hizmette](xref:fundamentals/host/hosted-services)uzun süre çalışan başlatma görevinin tamamlandığını raporlamak için bir sistem durumu denetimi içerir. `StartupHostedServiceHealthCheck`, barındırılan hizmetin uzun süre çalışan görevi bittiğinde `true` olarak ayarlayabilmesini `StartupTaskCompleted`bir özelliği kullanıma sunar (*StartupHostedServiceHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

Uzun süre çalışan arka plan görevi bir [barındırılan hizmet](xref:fundamentals/host/hosted-services) (*Hizmetler/startuphostedservice*) tarafından başlatılır. Görevin sonunda, `StartupHostedServiceHealthCheck.StartupTaskCompleted` `true`olarak ayarlanır:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

Sistem durumu denetimi, barındırılan hizmetle birlikte `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> kaydedilir. Barındırılan hizmetin, sistem durumu denetiminde özelliği ayarlaması gerektiğinden, sistem durumu denetimi de hizmet kapsayıcısına kaydedilir (*LivenessProbeStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı. Örnek uygulamada, sistem durumu denetimi uç noktaları, hazır olma denetimi için `/health/ready` oluşturulur ve libir denetim için `/health/live`. Hazırlık denetimi, durum denetimini `ready` etiketiyle durum denetimi olarak denetler. Libu denetim, [Healthcheckoptions. koşula](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) `false` döndürerek `StartupHostedServiceHealthCheck` filtreler (daha fazla bilgi için bkz. [durum denetimlerini filtrele](#filter-health-checks)):

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

Örnek uygulamayı kullanarak hazırlık/lizlilik yapılandırma senaryosunu çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:

```dotnetcli
dotnet run --scenario liveness
```

Bir tarayıcıda, 15 saniye geçtikten sonra `/health/ready` birkaç kez ziyaret edin. Sistem durumu denetimi ilk 15 saniye boyunca *sağlıksız* durum bildiriyor. 15 saniye sonra, uç nokta, barındırılan hizmet tarafından uzun süre çalışan görevin tamamlandığını yansıtan *sağlıklı*şekilde raporlar.

Bu örnek ayrıca, iki saniyelik bir gecikmeyle ilk hazırlık denetimini çalıştıran bir sistem durumu denetimi yayımcısı (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulaması) oluşturur. Daha fazla bilgi için bkz. [sistem durumu denetimi yayımcısı](#health-check-publisher) bölümü.

### <a name="kubernetes-example"></a>Kubernetes örneği

Ayrı hazır olma ve [Ezlilik denetimleri kullanmak, Kubernetes](https://kubernetes.io/)gibi bir ortamda yararlıdır. Kubernetes 'te, temel veritabanı kullanılabilirliğinin bir testi gibi istekleri kabul etmeden önce, zaman alan Başlangıç işlerini gerçekleştirmek için bir uygulamanın olması gerekebilir. Ayrı denetimlerin kullanılması, Orchestrator 'ın uygulamanın çalışıp çalışmadığını, ancak henüz hazırlanmadığını ya da uygulamanın başlayamadığını ayırt etmesine olanak tanır. Kubernetes 'te hazır olma ve Elanma araştırmaları hakkında daha fazla bilgi için bkz. Kubernetes belgelerindeki [limize ve hazırlık araştırmalarını yapılandırma](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) .

Aşağıdaki örnekte bir Kubernetes Readiness araştırma yapılandırması gösterilmektedir:

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

Örnek uygulama, özel bir yanıt yazıcısı ile bir bellek durumu denetimini gösterir.

uygulama, belirli bir bellek eşiğini (örnek uygulamada 1 GB) kullanıyorsa, sağlıksız bir durumu raporlar `MemoryHealthCheck`. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, uygulama için çöp toplayıcı (GC) bilgilerini içerir (*MemoryHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. Durum denetimini <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>geçirerek etkinleştirmek yerine, `MemoryHealthCheck` bir hizmet olarak kaydedilir. Tüm kayıtlı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> Hizmetleri sistem durumu denetimi Hizmetleri ve ara yazılım tarafından kullanılabilir. Sistem durumu denetimi hizmetlerini tek hizmet olarak kaydetmeyi öneririz.

Örnek uygulamada (*CustomWriterStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

`Startup.Configure`'de uygulama işleme ardışık düzeninde sistem durumu denetimleri ara yazılımı. Bir `WriteResponse` temsilcisi, sistem durumu denetimi yürütüldüğünde özel bir JSON yanıtının çıkışı için `ResponseWriter` özelliğine sağlanır:

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

`WriteResponse` yöntemi bir JSON nesnesine `CompositeHealthCheckResult` biçimlendirir ve sistem durumu denetimi yanıtı için JSON çıktısı verir:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

Örnek uygulamayı kullanarak özel yanıt yazıcı çıkışıyla ölçüm tabanlı araştırmayı çalıştırmak için, komut kabuğu 'ndaki projenin klasöründen aşağıdaki komutu yürütün:

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> [Aspnetcore. Diagnostics. healthcheck](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , disk depolama ve maksimum değer düzeyi denetimleri dahil ölçüm tabanlı sistem durumu denetimi senaryolarını içerir.
>
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="filter-by-port"></a>Bağlantı noktasına göre filtrele

<xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> bir bağlantı noktası ile çağırmak, belirtilen bağlantı noktasına durum denetimi isteklerini kısıtlar. Bu genellikle bir kapsayıcı ortamında, izleme hizmetleri için bir bağlantı noktası oluşturmak için kullanılır.

Örnek uygulama, [ortam değişkeni yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#environment-variables-configuration-provider)kullanarak bağlantı noktasını yapılandırır. Bağlantı noktası, *Launchsettings. JSON* dosyasında ayarlanır ve bir ortam değişkeni aracılığıyla yapılandırma sağlayıcısına geçirilir. Ayrıca, sunucuyu Yönetim bağlantı noktasındaki istekleri dinleyecek şekilde yapılandırmanız gerekir.

Yönetim bağlantı noktası yapılandırmasını göstermek üzere örnek uygulamayı kullanmak için, bir *Özellikler* klasöründe *launchsettings. JSON* dosyasını oluşturun.

Örnek uygulamadaki aşağıdaki *Özellikler/launchSettings. JSON* dosyası, örnek uygulamanın proje dosyalarına dahil değildir ve el ile oluşturulması gerekir:

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

Sistem durumu denetimi hizmetlerini `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> ile kaydedin. <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrısı, yönetim bağlantı noktasını belirtir (*ManagementPortStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> Kod içinde açıkça URL 'Leri ve yönetim bağlantı noktasını ayarlayarak *Launchsettings. JSON* dosyasını örnek uygulamada oluşturmaktan kaçınabilirsiniz. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> oluşturulduğu *program.cs* içinde, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> için bir çağrı ekleyin ve uygulamanın normal yanıt uç noktasını ve yönetim bağlantı noktası uç noktasını sağlayın. <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> çağrıldığında, yönetim bağlantı noktasını açıkça belirtin.
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

Yönetim bağlantı noktası yapılandırma senaryosunu örnek uygulamayı kullanarak çalıştırmak için, aşağıdaki komutu projenin klasöründen bir komut kabuğu 'ndan yürütün:

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>Bir sistem durumu denetim kitaplığı dağıtma

Bir sistem durumu denetimini kitaplık olarak dağıtmak için:

1. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> arabirimini tek başına bir sınıf olarak uygulayan bir sistem durumu denetimi yazın. Sınıf, yapılandırma verilerine erişmek için [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection), tür etkinleştirme ve [adlandırılmış seçenekleri](xref:fundamentals/configuration/options) kullanabilir.

   `CheckHealthAsync`sistem durumu denetim mantığındaki:

   * `data1` ve `data2`, araştırmanın sistem durumu denetimi mantığını çalıştırmak için yönteminde kullanılır.
   * `AccessViolationException` işlenir.

   Bir <xref:System.AccessViolationException> gerçekleştiğinde, kullanıcıların sistem durumu denetimleri hata durumunu yapılandırmasına izin veren <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> ile <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> döndürülür.

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

1. Kullanan uygulamanın `Startup.Configure` yönteminde çağırdığı parametrelere sahip bir genişletme yöntemi yazın. Aşağıdaki örnekte, aşağıdaki sistem durumu denetim yöntemi imzasını varsayın:

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   Yukarıdaki imza, `ExampleHealthCheck` sistem durumu denetimi araştırma mantığını işlemek için ek veri gerektirdiğini gösterir. Veriler, sistem durumu denetimi bir genişletme yöntemiyle kaydedildiğinde sistem durumu denetim örneğini oluşturmak için kullanılan temsilciye sağlanır. Aşağıdaki örnekte, çağıran isteğe bağlı olarak şunları belirtir:

   * sistem durumu denetim adı (`name`). `null`, `example_health_check` kullanılır.
   * sistem durumu denetimi için dize veri noktası (`data1`).
   * sistem durumu denetimi için tamsayı veri noktası (`data2`). `null`, `1` kullanılır.
   * hata durumu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>). Varsayılan, `null` değeridir. `null`, bir hata durumu için [HealthStatus. sağlıksız durum](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) bildirilir.
   * Etiketler (`IEnumerable<string>`).

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

## <a name="health-check-publisher"></a>Sistem durumu denetimi yayımcısı

Hizmet kapsayıcısına bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> eklendiğinde, sistem durumu denetimi sistemi düzenli olarak sistem durumu kontrollerinizi yürütür ve sonuçla `PublishAsync` çağırır. Bu, her bir işlemin sistem durumunu belirlemede düzenli aralıklarla izleme sistemini çağırmasını bekleyen, gönderim tabanlı bir sistem durumu izleme sistemi senaryosunda yararlıdır.

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> arabirimi tek bir yönteme sahiptir:

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> şunları ayarlamanıza izin verir:

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri yürütülmeden önce uygulama başladıktan sonra uygulanan ilk gecikmeyi &ndash;. Gecikme başlangıçta bir kez uygulanır ve sonraki yinelemelere uygulanmaz. Varsayılan değer beş saniyedir.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> yürütme dönemi &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>. Varsayılan değer 30 saniyedir.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> `null` (varsayılan), sistem durumu denetimi yayımcı hizmeti tüm kayıtlı sistem durumu denetimlerini çalıştırır. Bir sistem durumu denetimleri alt kümesini çalıştırmak için denetim kümesini filtreleyen bir işlev sağlayın. Koşul her dönem değerlendirilir.
* Tüm <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> örnekleri için sistem durumu denetimlerinin yürütülmesi için zaman aşımını &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>. Zaman aşımı olmadan yürütmek için <xref:System.Threading.Timeout.InfiniteTimeSpan> kullanın. Varsayılan değer 30 saniyedir.

> [!WARNING]
> ASP.NET Core 2,2 sürümünde, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> ayarı <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulama tarafından kabul edilemez; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>değerini ayarlar. Bu sorun ASP.NET Core 3,0 ' de giderilmiştir.

Örnek uygulamada, `ReadinessPublisher` bir <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasıdır. Durum denetimi durumu her denetim için şu şekilde günlüğe kaydedilir:

* Durum denetim durumu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>ise bilgi (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>).
* Durum <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> veya <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>ise hata (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>).

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

Örnek uygulamanın `LivenessProbeStartup` örneğinde, `StartupHostedService` hazırlık denetimi iki saniyelik bir başlangıç gecikmesine sahiptir ve denetimi her 30 saniyede bir çalıştırır. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> uygulamasını etkinleştirmek için örnek, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında tek bir hizmet olarak `ReadinessPublisher` kaydeder:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> Aşağıdaki geçici çözüm, uygulamaya bir veya daha fazla barındırılan hizmet zaten eklendiyse, hizmet kapsayıcısına <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> bir örnek eklenmesine izin verir. Bu geçici çözüm ASP.NET Core 3,0 ' de gerekli değildir.
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
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) , [Application Insights](/azure/application-insights/app-insights-overview)dahil olmak üzere çeşitli sistemler için yayımcılar içerir.
>
> [Aspnetcore. Diagnostics. Healthdenetimleri](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) Microsoft tarafından korunmaz veya desteklenmez.

## <a name="restrict-health-checks-with-mapwhen"></a>Durum denetimlerini Mapperne zaman kısıtla

Durum denetimi uç noktaları için istek ardışık düzenini koşullu olarak dallandırmak için <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> kullanın.

Aşağıdaki örnekte, `api/HealthCheck` uç noktası için bir GET isteği alındığında durum denetimleri ara yazılımını etkinleştirmek üzere istek ardışık düzenini `MapWhen` dallandırır:

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index#use-run-and-map>.

::: moniker-end
