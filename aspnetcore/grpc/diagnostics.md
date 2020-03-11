---
title: .NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama
author: jamesnk
description: .NET 'teki gRPC uygulamanızdan tanılamayı nasıl toplayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 17607b734e6d777de9516aa14e81c97f87b61023
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667344"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>.NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama

, [James bAyKiNg](https://twitter.com/jamesnk)

Bu makale, sorunları gidermeye yardımcı olmak için bir gRPC uygulamasından tanılamayı toplamaya yönelik rehberlik sağlar. Ele alınan konular:

* **Günlüğe kaydetme** - [.NET Core günlüğe](xref:fundamentals/logging/index)yazma ile yazılan yapılandırılmış Günlükler. <xref:Microsoft.Extensions.Logging.ILogger>, uygulama çerçeveleri tarafından Günlükler ve kullanıcılar tarafından bir uygulamada kendilerine ait günlüğe kaydetme için kullanılır.
* **İzleme** -`DiaganosticSource` ve `Activity`kullanılarak yazılmış bir işlemle ilgili olaylar. Tanılama kaynağından alınan izlemeler, [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) ve [opentelemetri](https://github.com/open-telemetry/opentelemetry-dotnet)gibi kitaplıklara göre uygulama telemetrisini toplamak için yaygın olarak kullanılır.
* **Ölçümler** -saniye başına isteklerin zaman aralıklarıyla veri ölçülerinin temsili. Ölçümler `EventCounter` kullanılarak yayınlanır ve [DotNet sayaçları](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) komut satırı aracı kullanılarak veya [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)ile gözlemlenebilir.

## <a name="logging"></a>Günlüğe kaydetme

gRPC Hizmetleri ve gRPC istemcisi [.NET Core günlüğü](xref:fundamentals/logging/index)kullanarak yazma günlükleri. Günlüklerde, uygulamalarınızda beklenmeyen davranışa hata ayıklamanız gerektiğinde başlamak için iyi bir yerdir.

### <a name="grpc-services-logging"></a>gRPC Hizmetleri günlüğü

> [!WARNING]
> Sunucu tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

GRPC Hizmetleri ASP.NET Core üzerinde barındırıldığından, bu, ASP.NET Core günlük sistemini kullanır. Varsayılan yapılandırmada, gRPC çok az bilgiyi günlüğe kaydeder, ancak bu yapılandırılabilir. ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#configuration) hakkındaki belgelere bakın.

gRPC `Grpc` kategorisi altına Günlükler ekler. GRPC 'den ayrıntılı günlükleri etkinleştirmek için, aşağıdaki öğeleri `Logging``LogLevel` alt bölümüne ekleyerek *appSettings. JSON* dosyanızdaki `Debug` düzeyine `Grpc` öneklerini yapılandırın:

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

Bunu, `ConfigureLogging`ile *Startup.cs* içinde de yapılandırabilirsiniz:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerini ayarlayın:

* `Logging:LogLevel:Grpc` = `Debug`

İç içe yapılandırma değerlerinin nasıl belirleneceğini belirlemek için yapılandırma sisteminizin belgelerini denetleyin. Örneğin, ortam değişkenlerini kullanırken, `:` yerine iki `_` karakter kullanılır (örneğin, `Logging__LogLevel__Grpc`).

Uygulamanız için daha ayrıntılı tanılama toplanırken `Debug` düzeyinin kullanılması önerilir. `Trace` düzeyi çok düşük düzey Tanılamalar üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.

#### <a name="sample-logging-output"></a>Örnek günlüğe kaydetme çıkışı

Aşağıda, bir gRPC hizmetinin `Debug` düzeyinde konsol çıkışının bir örneği verilmiştir:

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

### <a name="access-server-side-logs"></a>Sunucu tarafı günlüklerine erişin

Sunucu tarafı günlüklerine erişme, çalıştırdığınız ortama bağlıdır.

#### <a name="as-a-console-app"></a>Konsol uygulaması olarak

Konsol uygulamasında çalıştırıyorsanız, [konsol günlükçüsü](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmelidir. gRPC günlükleri konsolunda görünür.

#### <a name="other-environments"></a>Diğer ortamlar

Uygulama başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) dağıtılırsa, ortama uygun günlük sağlayıcılarının nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

### <a name="grpc-client-logging"></a>gRPC istemci günlüğü

> [!WARNING]
> İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

.NET istemcisinden günlükleri almak için, istemci kanalının oluşturulduğu zaman `GrpcChannelOptions.LoggerFactory` özelliğini ayarlayabilirsiniz. Bir ASP.NET Core uygulamasından gRPC hizmetini arıyorsanız, günlükçü fabrikası bağımlılık ekleme (DI) ' dan çözülebilir:

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

İstemci günlüğünü etkinleştirmenin alternatif bir yolu, istemci oluşturmak için [GRPC istemci fabrikası](xref:grpc/clientfactory) kullanmaktır. İstemci fabrikasına kayıtlı ve dı 'den çözümlenen bir gRPC istemcisi, otomatik olarak uygulamanın yapılandırılmış günlüğünü kullanacaktır.

Uygulamanız DI kullanıyorsa, [Loggerfactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)ile yeni bir `ILoggerFactory` örneği oluşturabilirsiniz. Bu yönteme erişmek için [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) paketini uygulamanıza ekleyin.

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>gRPC istemci günlüğü kapsamları

GRPC istemcisi, gRPC çağrısı sırasında yapılan günlüklere bir [günlüğe kaydetme kapsamı](https://docs.microsoft.com/aspnet/core/fundamentals/logginglog-scopes) ekler. Kapsamda gRPC çağrısıyla ilgili meta veriler vardır:

* **Grpcmethodtype** -GRPC Yöntem türü. Olası değerler `Grpc.Core.MethodType` numaralandırıcıdan adlardır, örneğin Birli
* **Grpcuri** -GRPC yönteminin göreli URI 'si; Örneğin,/bir. Greeter/Saymerhaba s

#### <a name="sample-logging-output"></a>Örnek günlüğe kaydetme çıkışı

Aşağıda, bir gRPC istemcisinin `Debug` düzeyinde konsol çıkışının bir örneği verilmiştir:

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="tracing"></a>İzleme

gRPC Hizmetleri ve gRPC istemcisi, [Diagnosticsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) ve [Activity](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity)kullanarak GRPC çağrıları hakkında bilgi sağlar.

* .NET gRPC, bir gRPC çağrısını temsil eden bir etkinlik kullanır.
* İzleme olayları, gRPC çağrısı etkinliğinin başlangıcında ve durdurulduğunda tanılama kaynağına yazılır.
* İzleme, gRPC akış çağrılarının kullanım ömrü boyunca iletiler gönderildiğinde hakkında bilgi yakalamaz.

### <a name="grpc-service-tracing"></a>gRPC hizmeti izleme

gRPC Hizmetleri, gelen HTTP istekleriyle ilgili olayları raporlayan ASP.NET Core barındırılır. gRPC 'ye özgü meta veriler, ASP.NET Core sağladığı mevcut HTTP istek tanımasına eklenir.

* Tanılama kaynağı adı `Microsoft.AspNetCore`.
* Etkinlik adı `Microsoft.AspNetCore.Hosting.HttpRequestIn`.
  * GRPC çağrısı tarafından çağrılan gRPC yönteminin adı, `grpc.method`adında bir etiket olarak eklenir.
  * İşlemi tamamlandığında gRPC çağrısının durum kodu, `grpc.status_code`adlı bir etiket olarak eklenir.

### <a name="grpc-client-tracing"></a>gRPC istemci izleme

.NET gRPC istemcisi, gRPC çağrıları yapmak için `HttpClient` kullanır. `HttpClient`, Tanılama olaylarını yazsa da, .NET gRPC istemcisi özel bir tanılama kaynağı, etkinlik ve olaylar sağlayarak bir gRPC çağrısıyla ilgili tüm bilgilerin toplanmasına olanak tanır.

* Tanılama kaynağı adı `Grpc.Net.Client`.
* Etkinlik adı `Grpc.Net.Client.GrpcOut`.
  * GRPC çağrısı tarafından çağrılan gRPC yönteminin adı, `grpc.method`adında bir etiket olarak eklenir.
  * İşlemi tamamlandığında gRPC çağrısının durum kodu, `grpc.status_code`adlı bir etiket olarak eklenir.

### <a name="collecting-tracing"></a>İzleme toplanıyor

`DiagnosticSource` kullanmanın en kolay yolu, uygulamanızda [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) veya [opentelemetri](https://github.com/open-telemetry/opentelemetry-dotnet) gibi bir telemetri kitaplığı yapılandırmaktır. Bu kitaplık, gRPC ile diğer uygulama telemetrisini çağıran bilgileri işleyecek.

İzleme Application Insights gibi bir yönetilen hizmette görüntülenebilir veya kendi dağıtılmış izleme sisteminizi çalıştırmayı tercih edebilirsiniz. Opentelemetri, izleme verilerinin [Caeger](https://www.jaegertracing.io/) ve [zipy](https://zipkin.io/)'ye verilmesini destekler.

`DiagnosticSource`, `DiagnosticListener`kullanarak koddaki izleme olaylarını tüketebilir. Bir tanılama kaynağını kodla dinleme hakkında daha fazla bilgi için bkz. [diagnosticsource Kullanıcı Kılavuzu](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).

> [!NOTE]
> Telemetri kitaplıkları Şu anda gRPC 'ye özgü `Grpc.Net.Client.GrpcOut` telemetrisini yakalamaz. Telemetri kitaplıklarını geliştirmek için çalış bu izlemeyi devam ediyor.

## <a name="metrics"></a>Ölçümler

Ölçümler, zaman aralıklarıyla veri ölçülerinin bir gösterimidir (örneğin, saniye başına istek). Ölçüm verileri, yüksek düzeyde bir uygulamanın durumunun gözlemde yapılmasına izin verir. .NET gRPC ölçümleri `EventCounter`kullanılarak yayınlanır.

### <a name="grpc-service-metrics"></a>gRPC hizmeti ölçümleri

gRPC sunucu ölçümleri `Grpc.AspNetCore.Server` olay kaynağında raporlanır.

| Name                      | Açıklama                   |
| --------------------------|-------------------------------|
| `total-calls`             | Toplam çağrı sayısı                   |
| `current-calls`           | Geçerli çağrılar                 |
| `calls-failed`            | Toplam başarısız çağrı sayısı            |
| `calls-deadline-exceeded` | Toplam çağrı son tarihi aşıldı |
| `messages-sent`           | Gönderilen toplam Ileti sayısı           |
| `messages-received`       | Alınan toplam Ileti sayısı       |
| `calls-unimplemented`     | Uygulanmayan Toplam çağrı sayısı     |

ASP.NET Core Ayrıca, `Microsoft.AspNetCore.Hosting` olay kaynağı üzerinde kendi ölçümlerini de sağlar.

### <a name="grpc-client-metrics"></a>gRPC istemci ölçümleri

gRPC istemci ölçümleri `Grpc.Net.Client` olay kaynağında raporlanır.

| Name                      | Açıklama                   |
| --------------------------|-------------------------------|
| `total-calls`             | Toplam çağrı sayısı                   |
| `current-calls`           | Geçerli çağrılar                 |
| `calls-failed`            | Toplam başarısız çağrı sayısı            |
| `calls-deadline-exceeded` | Toplam çağrı son tarihi aşıldı |
| `messages-sent`           | Gönderilen toplam Ileti sayısı           |
| `messages-received`       | Alınan toplam Ileti sayısı       |

### <a name="observe-metrics"></a>Ölçümleri gözlemleyin

[DotNet sayaçları](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) , geçici sistem durumu izleme ve ilk düzey performans araştırması için bir performans izleme aracıdır. `Grpc.AspNetCore.Server` veya `Grpc.Net.Client` sağlayıcı adı olarak bir .NET uygulamasını izleyin.

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

GRPC ölçümlerini gözlemlemeye yönelik başka bir yol da Application Insights [Microsoft. ApplicationInsights. EventCounterCollector paketini](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)kullanarak sayaç verilerini yakalemektir. Kurulumdan sonra, Application Insights çalışma zamanında ortak .NET sayaçlarını toplar. gRPC 'nin sayaçları varsayılan olarak toplanmaz, ancak uygulama öngörüleri [ek sayaçlar içerecek şekilde özelleştirilebilir](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).

*Startup.cs*Içinde toplanacak uygulama öngörüleri Için GRPC sayaçlarını belirtin:

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
