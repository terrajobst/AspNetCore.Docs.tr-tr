---
title: ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme
author: guardrex
description: ASP.NET core'da etkinleştirme Fabrika tabanlı bir uygulama türü kesin belirlenmiş bir ara yazılım kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: b4d71c2c7f09acb58b73e84080e8574d77f8b326
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087008"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme

Tarafından [Luke Latham](https://github.com/guardrex)

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> bir genişletilebilirlik noktası [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> genişletme yöntemleri denetleyin bir ara kayıtlı tür uygulayan olup <xref:Microsoft.AspNetCore.Http.IMiddleware>. Aksi halde <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> çözümlemek için kullanılan kapsayıcıda kayıtlı örnek <xref:Microsoft.AspNetCore.Http.IMiddleware> kural tabanlı ara yazılım etkinleştirme mantığı kullanmak yerine uygulama. Ara yazılım olarak kaydedilmiş bir [kapsamlı veya geçici hizmet](xref:fundamentals/dependency-injection#service-lifetimes) uygulamanın service kapsayıcısında.

Avantajlar:

* İstemci istek (kapsamlı Hizmetleri ekleme) başına etkinleştirme
* Güçlü ara yazılım yazma

<xref:Microsoft.AspNetCore.Http.IMiddleware> kapsamı belirlenmiş hizmetler eklenen bu nedenle Ara oluşturucuya (bağlantı), istemci istek etkinleştirilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar. [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi isteği işler ve döndürür bir <xref:System.Threading.Tasks.Task> temsil eden bir ara yazılım yürütülmesi.

Ara yazılım tarafından kuralı etkinleştirildi:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Etkin bir ara yazılım tarafından <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Uzantılar için middlewares oluşturulur:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Fabrika etkinleştirildi Ara yazılımla nesneleri geçirmek mümkün değildir <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Fabrika etkinleştirildi ara yazılım yerleşik bir kapsayıcıda eklenir `Startup.ConfigureServices`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

İstek işleme ardışık düzeninde her iki middlewares kayıtlı `Startup.Configure`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Ara yazılım oluşturmak için yöntemleri sağlar. Ara yazılım Üreteç uygulaması kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.

Varsayılan <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> uygulaması <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, bulunur [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
