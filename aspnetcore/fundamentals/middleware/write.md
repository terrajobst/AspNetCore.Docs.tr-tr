---
title: Özel ASP.NET Core ara yazılım yazma
author: rick-anderson
description: Özel ASP.NET Core ara yazılım yazmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 352db93dd7061070c76e34f6c03883f68e2041ee
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167104"
---
# <a name="write-custom-aspnet-core-middleware"></a>Özel ASP.NET Core ara yazılım yazma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır. ASP.NET Core, zengin bir yerleşik ara yazılım bileşenleri, ancak bazı senaryolarda, özel bir ara yazılım yazmak isteyebilirsiniz kümesi sağlar.

## <a name="middleware-class"></a>Ara yazılım sınıfı

Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile kullanıma sunulan. Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır. Bkz: ASP.NET Core'nın yerleşik yerelleştirme desteğini <xref:fundamentals/localization>.

Ara yazılım, kültürü geçirerek test edin. Örneğin, istek `https://localhost:5001/?culture=no`.

Aşağıdaki kod, bir sınıf için ara yazılım temsilci taşır:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

Ara yazılım sınıfı içermelidir:

* Türünde bir parametre içeren bir genel oluşturucuya <xref:Microsoft.AspNetCore.Http.RequestDelegate>.
* Adlı bir genel yöntem `Invoke` veya `InvokeAsync`. Bu yöntem gerekir:
  * Döndürür bir `Task`.
  * Türünde bir ilk parametre kabul <xref:Microsoft.AspNetCore.Http.HttpContext>.
  
Oluşturucu için ek parametreler ve `Invoke` / `InvokeAsync` tarafından doldurulur [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).

## <a name="middleware-dependencies"></a>Ara yazılım bağımlılıkları

Ara yazılım izlemelidir [açık bağımlılıkları İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) tarafından kendi oluşturucusuna bağımlılıkları gösterme. Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*. Bkz: [istek başına ara yazılım bağımlılıkları](#per-request-middleware-dependencies) istek içinde ara yazılım ile Hizmetleri paylaşan gerekiyorsa bölümü.

Ara yazılım bileşenleri, bunların bağımlılıklarını giderebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) Oluşturucu parametresi üzerinden. [UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) ek parametreler doğrudan da kabul edebilir.

## <a name="per-request-middleware-dependencies"></a>İstek başına ara yazılım bağımlılıkları

Ara yazılım değil istek, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım oluşturucular tarafından kullanılan etkin kalma süresi olmayan paylaşılan hizmetler diğer bağımlılık eklenen türleriyle her isteği sırasında. Paylaşmanız gerekir durumunda bir *kapsamlı* hizmet Ara yazılımınızı ve diğer türleri arasında bu hizmetlere ekleme `Invoke` yöntemin imzası. `Invoke` Yöntemi tarafından DI doldurulur ek parametreleri kabul edebilir:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="middleware-extension-method"></a>Ara yazılım genişletme yöntemi

Aşağıdaki uzantı yöntemi ara yazılımı üzerinden kullanıma sunan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Aşağıdaki kod ara yazılımı gelen çağrıları `Startup.Configure`:

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
