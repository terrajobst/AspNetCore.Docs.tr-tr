---
title: .NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama
author: jamesnk
description: .NET 'teki gRPC uygulamanızdan tanılamayı nasıl toplayacağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: ce6ad96d9e26c9cd3844093536745f8f9bea4a76
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204347"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>.NET üzerinde gRPC 'de günlüğe kaydetme ve tanılama

, [James bAyKiNg](https://twitter.com/jamesnk)

Bu makalede, sorunları gidermeye yardımcı olmak için gRPC uygulamanızdan tanılamayı toplamaya yönelik rehberlik sunulmaktadır.

## <a name="grpc-services-logging"></a>gRPC Hizmetleri günlüğü

> [!WARNING]
> Sunucu tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

GRPC Hizmetleri ASP.NET Core üzerinde barındırıldığından, bu, ASP.NET Core günlük sistemini kullanır. Varsayılan yapılandırmada, gRPC çok az bilgiyi günlüğe kaydeder, ancak bu yapılandırılabilir. ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#configuration) hakkındaki belgelere bakın.

GRPC, `Grpc` kategori altına Günlükler ekler. GRPC 'den ayrıntılı günlükleri etkinleştirmek için, aşağıdaki öğeleri `Grpc` `LogLevel` içindeki `Logging`alt bölümüne `Debug` ekleyerek, ön ekleri *appSettings. JSON* dosyanızdaki düzeye yapılandırın:

[!code-json[](diagnostics/logging-config.json?highlight=7)]

Bunu, *Startup.cs* `ConfigureLogging`içinde şu şekilde de yapılandırabilirsiniz:

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5)]

JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerini ayarlayın:

* `Logging:LogLevel:Grpc` = `Debug`

İç içe yapılandırma değerlerinin nasıl belirleneceğini belirlemek için yapılandırma sisteminizin belgelerini denetleyin. Örneğin, ortam değişkenleri kullanılırken, yerine `_` `:` iki karakter kullanılır (örneğin, `Logging__LogLevel__Grpc`).

Uygulamanız için daha ayrıntılı `Debug` tanılama toplanırken düzeyin kullanılmasını öneririz. Düzey `Trace` , çok düşük düzey Tanılamalar üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.

### <a name="sample-logging-output"></a>Örnek günlüğe kaydetme çıkışı

Aşağıda, bir GRPC hizmeti `Debug` düzeyinde konsol çıkışının bir örneği verilmiştir:

```
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

## <a name="access-server-side-logs"></a>Sunucu tarafı günlüklerine erişin

Sunucu tarafı günlüklerine erişme, çalıştırdığınız ortama bağlıdır.

### <a name="as-a-console-app"></a>Konsol uygulaması olarak

Konsol uygulamasında çalıştırıyorsanız, [konsol günlükçüsü](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmelidir. gRPC günlükleri konsolunda görünür.

### <a name="other-environments"></a>Diğer ortamlar

Uygulama başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) dağıtılırsa, ortama uygun günlük sağlayıcılarının nasıl <xref:fundamentals/logging/index> yapılandırılacağı hakkında daha fazla bilgi için bkz.

## <a name="grpc-client-logging"></a>gRPC istemci günlüğü

> [!WARNING]
> İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

.Net istemcisinden günlükleri almak için, `GrpcChannelOptions.LoggerFactory` özelliği istemci kanalının oluşturulduğu zaman ayarlayabilirsiniz. Bir ASP.NET Core uygulamasından gRPC hizmetini arıyorsanız, günlükçü fabrikası bağımlılık ekleme (DI) ' dan çözülebilir:

[!code-csharp[](diagnostics/net-client-dependency-injection.cs?highlight=7,16)]

İstemci günlüğünü etkinleştirmenin alternatif bir yolu, istemci oluşturmak için [GRPC istemci fabrikası](xref:grpc/clientfactory) kullanmaktır. İstemci fabrikasına kayıtlı ve dı 'den çözümlenen bir gRPC istemcisi, otomatik olarak uygulamanın yapılandırılmış günlüğünü kullanacaktır.

Uygulamanız dı kullanıyorsa, `ILoggerFactory` [loggerfactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)ile yeni bir örnek oluşturabilirsiniz. Bu yönteme erişmek için [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) paketini uygulamanıza ekleyin.

[!code-csharp[](diagnostics/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Örnek günlüğe kaydetme çıkışı

Aşağıda, bir GRPC istemcisinin `Debug` düzeyindeki konsol çıkışının bir örneği verilmiştir:

```
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
