---
title: ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme
author: guardrex
description: Kesin türü belirtilmiş Ara ASP.NET Core Fabrika tabanlı etkinleştirme uygulamasında ile kullanmayı öğrenin.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 8cec5b3b498f5a23463d8c3cd5901e14f22f6eab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729122"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme

Tarafından [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) için genişletilebilirlik noktasıdır [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.

`UseMiddleware` genişletme yöntemleri denetleyin bir ara kayıtlı türün uygulayan `IMiddleware`. Aşması durumunda `IMiddlewareFactory` kapsayıcısında kayıtlı örneği çözmek için kullanılan `IMiddleware` kurala dayalı Ara etkinleştirme mantığı kullanmak yerine uygulama. Ara yazılım, uygulamanın hizmet kapsayıcısında kapsamlı veya geçici bir hizmet olarak kaydedilir.

Yararları:

* İstek (ekleme kapsamlı Hizmetleri) başına etkinleştirme
* Ara yazılım güçlü yazma

`IMiddleware` kapsamlı Hizmetleri Ara oluşturucuya yerleştirilebilir şekilde istek başına etkinleştirilir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulaması tarafından etkinleştirilen Ara gösterir:

* Kuralı. Geleneksel ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz: [ara yazılım](xref:fundamentals/middleware/index) konu.
* Bir [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması. Varsayılan [MiddlewareFactory sınıfı](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) ara yazılım etkinleştirir.

Ara yazılım uygulamaları özdeş işlev ve bir sorgu dizesi parametresi tarafından sağlanan değer kaydedin (`key`). Middlewares eklenen veritabanı bağlamı (kapsamlı bir hizmeti) sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için kullanın.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) yöntemi döndürür ve istekleri işler bir `Task` ara yazılım yürütülmesi temsil eden.

Kural tarafından etkinleştirilen ara:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Ara yazılım tarafından etkinleştirilen `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Uzantıları için middlewares oluşturulur:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nesneleri Fabrika etkinleştirilmiş Ara yazılımla geçirilecek kurulamadığı `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Fabrika etkinleştirilmiş ara yazılım yerleşik kapsayıcısında eklenen *haline*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

İstek işleme ardışık düzeninde hem middlewares kayıtlı `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara oluşturmak için yöntemleri sağlar. Ara yazılım Üreteç uygulaması kapsayıcısında kapsamlı bir hizmet olarak kaydedilir.

Varsayılan `IMiddlewareFactory` uygulaması, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), içinde bulunan [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket ([başvuru kaynağı](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Bir üçüncü taraf kapsayıcısı ile Ara yazılım etkinleştirme](xref:fundamentals/middleware/extensibility-third-party-container)
