---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: GRPC Hizmetleri ile ASP.NET Core yazarken, temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 1f019fac23982a95fa37d43099522f4b3e9d107a
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039278"
---
# <a name="grpc-services-with-aspnet-core"></a>ASP.NET Core içeren gRPC Hizmetleri

Bu belge, ASP.NET Core kullanarak gRPC Services'i kullanmaya başlama işlemi gösterilmektedir.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bkz: [gRPC Services'i kullanmaya başlama](xref:tutorials/grpc/grpc-start) gRPC proje oluşturma konusunda ayrıntılı yönergeler için.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

Çalıştırma `dotnet new grpc -o GrpcGreeter` komut satırından.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>GRPC Hizmetleri için ASP.NET Core uygulaması Ekle

gRPC aşağıdaki paketler gereklidir:

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) API'leri için protobuf iletisi.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>GRPC yapılandırın

gRPC ile etkin `AddGrpc` yöntemi:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

Yönlendirme işlem hattı her gRPC hizmet eklenir `MapGrpcService` yöntemi:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

ASP.NET Core middlewares ve özellikleri yönlendirme işlem hattı paylaşın ve bu nedenle uygulama ek istek işleyicileri sunmak için yapılandırılabilir. MVC denetleyicileri gibi ek istek işleyicileri yapılandırılmış gRPC hizmetleriyle paralel çalışır.

## <a name="integration-with-aspnet-core-apis"></a>ASP.NET Core API'ları ile tümleştirme

gRPC Hizmetleri olan ASP.NET Core özelliklerine tam erişim gibi [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) ve [günlüğü](xref:fundamentals/logging/index). Örneğin, hizmet uygulaması DI kapsayıcı Oluşturucu Günlükçü hizmetinden çözebilirsiniz:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

Varsayılan olarak, diğer tüm yaşam süresi (Singleton, kapsamındaki veya geçici) DI hizmetleriyle gRPC hizmet uygulamasında çözebilirsiniz.

### <a name="resolve-httpcontext-in-grpc-methods"></a>GRPC yöntemleri HttpContext çözümleyin

GRPC API yöntemi, konak, başlığı ve tanıtımları gibi bazı HTTP/2 ileti verilere erişim sağlar. Erişimi olan aracılığıyla `ServerCallContext` her gRPC yöntemine geçirilen bağımsız değişken:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` tam erişim sağlamaz `HttpContext` tüm ASP.NET API'lerindeki. `GetHttpContext` Genişletme yöntemi için tam erişim sağlar `HttpContext` ASP.NET API'leri temel alınan HTTP/2 iletiyi temsil eden:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
