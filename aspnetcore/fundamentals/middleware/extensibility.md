---
title: ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme
author: guardrex
description: ASP.NET core'da etkinleştirme Fabrika tabanlı bir uygulama türü kesin belirlenmiş bir ara yazılım kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 566a5c5f642a3f55e72a8e070c69d2bfddaee3a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207205"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme

Tarafından [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) bir genişletilebilirlik noktası [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.

`UseMiddleware` genişletme yöntemleri denetleyin bir ara kayıtlı tür uygulayan olup `IMiddleware`. Aksi halde `IMiddlewareFactory` çözümlemek için kullanılan kapsayıcıda kayıtlı örnek `IMiddleware` kural tabanlı ara yazılım etkinleştirme mantığı kullanmak yerine uygulama. Ara yazılım, uygulamanın service kapsayıcısında kapsamlı ya da geçici bir hizmet olarak kaydedilir.

Avantajlar:

* Etkinleştirme isteği (kapsamlı Hizmetleri ekleme) başına
* Güçlü ara yazılım yazma

`IMiddleware` kapsamı belirlenmiş hizmetler Ara oluşturucuya yerleştirilebilir şekilde istek başına etkinleştirilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama tarafından etkinleştirilen bir ara yazılım gösterir:

* Kuralı. Geleneksel bir ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz. [ara yazılım](xref:fundamentals/middleware/index) konu.
* Bir [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması. Varsayılan [MiddlewareFactory sınıfı](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) ara yazılım etkinleştirir.

Ara yazılım uygulamaları aynı şekilde çalışır ve bir sorgu dizesi parametresi tarafından sağlanan değerini kaydedin (`key`). Middlewares, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için bir eklenen veritabanı bağlamı (kapsamlı bir hizmet) kullanın.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) ara yazılımı için uygulamanın istek ardışık düzenini tanımlar. [InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) yöntemi isteği işler ve döndürür bir `Task` temsil eden bir ara yazılım yürütülmesi.

Ara yazılım tarafından kuralı etkinleştirildi:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Etkin bir ara yazılım tarafından `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Uzantılar için middlewares oluşturulur:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Fabrika etkinleştirildi Ara yazılımla nesneleri geçirmek mümkün değildir `UseMiddleware`:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrika etkinleştirildi ara yazılım yerleşik bir kapsayıcıda eklenir *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

İstek işleme ardışık düzeninde her iki middlewares kayıtlı `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara yazılım oluşturmak için yöntemleri sağlar. Ara yazılım Üreteç uygulaması kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

Varsayılan `IMiddlewareFactory` uygulaması [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), bulunur [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
