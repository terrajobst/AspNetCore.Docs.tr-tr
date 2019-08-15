---
title: ASP.NET Core içeren gRPC Hizmetleri
author: juntaoluo
description: ASP.NET Core ile gRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 38111c152c581c50767f9cd4e5fa257bd3fd804e
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022310"
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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
