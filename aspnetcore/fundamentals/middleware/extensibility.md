---
title: ASP.NET Core 'de fabrika tabanlı ara yazılım etkinleştirmesi
author: guardrex
description: ASP.NET Core ' de fabrika tabanlı etkinleştirme uygulamasıyla, türü kesin belirlenmiş bir ara yazılımı nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 17018d2dd20ed7b26bd0aa1095fa720a73f77261
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186949"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core 'de fabrika tabanlı ara yazılım etkinleştirmesi

Tarafından [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>Uzantı yöntemleri bir ara yazılımın kayıtlı türünün uygulayıp uygulamadığını <xref:Microsoft.AspNetCore.Http.IMiddleware>denetler. Bu durumda, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> kapsayıcıya kaydedilen örnek, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> , uygulamayı çözmek için kullanılır. Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.

Larından

* İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)
* Ara yazılım sıkı yazma

<xref:Microsoft.AspNetCore.Http.IMiddleware>istemci isteği başına etkinleştirilir (bağlantı), bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, requestdelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılım yürütmesini <xref:System.Threading.Tasks.Task> temsil eden bir döndürür.

Kurala göre etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Etkinleştirilen ara yazılım <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Middlewares için Uzantılar oluşturulur:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nesneleri fabrikada etkinleştirilen bir ara yazılıma <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>geçirmek mümkün değildir:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrikada etkinleştirilen ara yazılım, içindeki `Startup.ConfigureServices`yerleşik kapsayıcıya eklenir:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Her iki middlewares de `Startup.Configure`istek işleme ardışık düzeninde kaydedilir:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar. Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Varsayılan<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>uygulama, [Microsoft. aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>Uzantı yöntemleri bir ara yazılımın kayıtlı türünün uygulayıp uygulamadığını <xref:Microsoft.AspNetCore.Http.IMiddleware>denetler. Bu durumda, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> kapsayıcıya kaydedilen örnek, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> , uygulamayı çözmek için kullanılır. Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.

Larından

* İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)
* Ara yazılım sıkı yazma

<xref:Microsoft.AspNetCore.Http.IMiddleware>istemci isteği başına etkinleştirilir (bağlantı), bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, requestdelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılım yürütmesini <xref:System.Threading.Tasks.Task> temsil eden bir döndürür.

Kurala göre etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Etkinleştirilen ara yazılım <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Middlewares için Uzantılar oluşturulur:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nesneleri fabrikada etkinleştirilen bir ara yazılıma <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>geçirmek mümkün değildir:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrikada etkinleştirilen ara yazılım, içindeki `Startup.ConfigureServices`yerleşik kapsayıcıya eklenir:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Her iki middlewares de `Startup.Configure`istek işleme ardışık düzeninde kaydedilir:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar. Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Varsayılan<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>uygulama, [Microsoft. aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
