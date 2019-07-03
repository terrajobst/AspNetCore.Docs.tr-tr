---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: GRPC Hizmetleri ile ASP.NET Core yazarken, temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 5937ca9f2a783c4dabe324dae828b97953782938
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555867"
---
# <a name="grpc-services-with-aspnet-core"></a>ASP.NET Core içeren gRPC Hizmetleri

Bu belge, ASP.NET Core kullanarak gRPC Services'i kullanmaya başlama işlemi gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).

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

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Yönlendirme işlem hattı her gRPC hizmet eklenir `MapGrpcService` yöntemi:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

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

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` tam erişim sağlamaz `HttpContext` tüm ASP.NET API'lerindeki. `GetHttpContext` Genişletme yöntemi için tam erişim sağlar `HttpContext` ASP.NET API'leri temel alınan HTTP/2 iletiyi temsil eden:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
