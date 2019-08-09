---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: ASP.NET Core ile gRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862936"
---
# <a name="grpc-services-with-aspnet-core"></a>ASP.NET Core içeren gRPC Hizmetleri

Bu belgede, ASP.NET Core kullanarak gRPC Hizmetleri ile çalışmaya başlama gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama

[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

GRPC projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [GRPC hizmetlerini kullanmaya başlama](xref:tutorials/grpc/grpc-start) .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Mac için Visual Studio](#tab/visual-studio-code+visual-studio-mac)

Komut `dotnet new grpc -o GrpcGreeter` satırından çalıştırın.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>ASP.NET Core uygulamasına gRPC Hizmetleri ekleme

gRPC, [GRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) paketini gerektirir.

### <a name="configure-grpc"></a>GRPC 'yi yapılandırma

GRPC, `AddGrpc` yöntemiyle etkinleştirilir:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Her GRPC hizmeti yönlendirme ardışık düzenine `MapGrpcService` yöntemi aracılığıyla eklenir:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core middlewares ve Özellikler yönlendirme işlem hattını paylaşır, bu nedenle uygulama ek istek işleyicileri sunacak şekilde yapılandırılabilir. MVC denetleyicileri gibi ek istek işleyicileri, yapılandırılmış gRPC hizmetleriyle paralel olarak çalışır.

## <a name="integration-with-aspnet-core-apis"></a>ASP.NET Core API 'Leri ile tümleştirme

gRPC Hizmetleri, [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğe kaydetme](xref:fundamentals/logging/index)gibi ASP.NET Core özelliklerine tam erişime sahiptir. Örneğin, hizmet uygulama, Oluşturucu aracılığıyla bir günlükçü hizmetini dı kapsayıcısından çözümleyebilir:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Varsayılan olarak, gRPC hizmeti uygulama diğer dı hizmetlerini herhangi bir yaşam süresi (tek, kapsamlı veya geçici) ile çözümleyebilir.

### <a name="resolve-httpcontext-in-grpc-methods"></a>GRPC yöntemlerinde HttpContext 'i çözümle

GRPC API 'SI, yöntem, ana bilgisayar, üst bilgi ve tanıtımları gibi bazı HTTP/2 ileti verilerine erişim sağlar. Erişim, her GRPC yöntemine geçirilen bağımsızdeğişkendir:`ServerCallContext`

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`tüm ASP.NET API 'lerinde tam erişim `HttpContext` sağlamaz. Genişletme yöntemi, ASP.NET API 'lerinde temel alınan `HttpContext` http/2 iletisini temsil eden öğesine tam erişim sağlar: `GetHttpContext`

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a>macOS 'ta gRPC ve ASP.NET Core

Kestrel, macOS 'ta [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246) ile http/2 ' i desteklemez. ASP.NET Core gRPC şablonu ve örnekleri varsayılan olarak TLS kullanır. GRPC sunucusunu başlatmayı denediğinizde aşağıdaki hata iletisini görürsünüz:

> https://localhost:5001 IPv4 geri döngü arabirimine bağlanılamıyor: Eksik ALPN desteği nedeniyle, macOS 'ta TLS üzerinden HTTP/2 desteklenmez. '.

Bu sorunu geçici olarak çözmek için Kestrel ve gRPC istemcisini TLS **olmadan** http/2 kullanacak şekilde yapılandırın. Bunu yalnızca geliştirme sırasında yapmanız gerekir. TLS 'nin kullanılması, gRPC iletilerinin şifrelenmeden gönderilmesine neden olur.

Kestrel, içinde `Program.cs`TLS olmadan bir http/2 uç noktası yapılandırmalıdır:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

GRPC istemcisinin, sunucu adresinde `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` anahtarı olarak `true` ayarlanması ve kullanması `http` gerekir:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> HTTP/2, TLS olmadan yalnızca uygulama geliştirme sırasında kullanılmalıdır. Üretim uygulamaları her zaman aktarım güvenliği kullanmalıdır. Daha fazla bilgi için bkz. [gRPC 'Deki güvenlik konuları ASP.NET Core](xref:grpc/security#transport-security).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
