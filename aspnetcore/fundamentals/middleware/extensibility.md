---
title: ASP.NET Core 'de fabrika tabanlı ara yazılım etkinleştirmesi
author: rick-anderson
description: ASP.NET Core ' de fabrika tabanlı etkinleştirme uygulamasıyla, türü kesin belirlenmiş bir ara yazılımı nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: abc6268584d12fe43d972c79a99316b94e8bee4b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663095"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET Core 'de fabrika tabanlı ara yazılım etkinleştirmesi

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> uzantısı yöntemleri bir ara yazılımın kayıtlı türünün <xref:Microsoft.AspNetCore.Http.IMiddleware>uygulayıp uygulamadığını denetler. Bu durumda, kapsayıcıda kayıtlı <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> örneği, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> uygulamasını çözümlemek için kullanılır. Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.

Larından

* İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)
* Ara yazılım sıkı yazma

<xref:Microsoft.AspNetCore.Http.IMiddleware> istemci isteği (bağlantı) başına etkinleştirilir, bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>, uygulamanın istek ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılımın yürütülmesini temsil eden bir <xref:System.Threading.Tasks.Task> döndürür.

Kurala göre etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>tarafından etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Middlewares için Uzantılar oluşturulur:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>ile, fabrika özellikli bir ara yazılıma nesne geçirmek mümkün değildir:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrika özellikli ara yazılım, `Startup.ConfigureServices`yerleşik kapsayıcısına eklenir:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Her iki middlewares de `Startup.Configure`içindeki istek işleme ardışık düzenine kaydedilir:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, ara yazılım oluşturmak için yöntemler sağlar. Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

Varsayılan <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> uygulama <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> uzantısı yöntemleri bir ara yazılımın kayıtlı türünün <xref:Microsoft.AspNetCore.Http.IMiddleware>uygulayıp uygulamadığını denetler. Bu durumda, kapsayıcıda kayıtlı <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> örneği, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> uygulamasını çözümlemek için kullanılır. Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.

Larından

* İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)
* Ara yazılım sıkı yazma

<xref:Microsoft.AspNetCore.Http.IMiddleware> istemci isteği (bağlantı) başına etkinleştirilir, bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>Imıddliware

<xref:Microsoft.AspNetCore.Http.IMiddleware>, uygulamanın istek ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılımın yürütülmesini temsil eden bir <xref:System.Threading.Tasks.Task> döndürür.

Kurala göre etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>tarafından etkinleştirilen ara yazılım:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Middlewares için Uzantılar oluşturulur:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>ile, fabrika özellikli bir ara yazılıma nesne geçirmek mümkün değildir:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrika özellikli ara yazılım, `Startup.ConfigureServices`yerleşik kapsayıcısına eklenir:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Her iki middlewares de `Startup.Configure`içindeki istek işleme ardışık düzenine kaydedilir:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, ara yazılım oluşturmak için yöntemler sağlar. Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

Varsayılan <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> uygulama <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
