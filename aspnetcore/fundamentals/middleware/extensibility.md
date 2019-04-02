---
title: ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme
author: guardrex
description: ASP.NET core'da etkinleştirme Fabrika tabanlı bir uygulama türü kesin belirlenmiş bir ara yazılım kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 9305616ce3f2ef49cf9dfcab719f673c0fb4b51e
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809172"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="544e0-103">ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="544e0-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="544e0-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="544e0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="544e0-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> bir genişletilebilirlik noktası [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="544e0-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="544e0-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> genişletme yöntemleri denetleyin bir ara kayıtlı tür uygulayan olup <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span><span class="sxs-lookup"><span data-stu-id="544e0-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="544e0-107">Aksi halde <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> çözümlemek için kullanılan kapsayıcıda kayıtlı örnek <xref:Microsoft.AspNetCore.Http.IMiddleware> kural tabanlı ara yazılım etkinleştirme mantığı kullanmak yerine uygulama.</span><span class="sxs-lookup"><span data-stu-id="544e0-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="544e0-108">Ara yazılım olarak kaydedilmiş bir [kapsamlı veya geçici hizmet](xref:fundamentals/dependency-injection#service-lifetimes) uygulamanın service kapsayıcısında.</span><span class="sxs-lookup"><span data-stu-id="544e0-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="544e0-109">Avantajlar:</span><span class="sxs-lookup"><span data-stu-id="544e0-109">Benefits:</span></span>

* <span data-ttu-id="544e0-110">İstemci istek (kapsamlı Hizmetleri ekleme) başına etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="544e0-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="544e0-111">Güçlü ara yazılım yazma</span><span class="sxs-lookup"><span data-stu-id="544e0-111">Strong typing of middleware</span></span>

<span data-ttu-id="544e0-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> kapsamı belirlenmiş hizmetler eklenen bu nedenle Ara oluşturucuya (bağlantı), istemci istek etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="544e0-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="544e0-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="544e0-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="544e0-114">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="544e0-114">IMiddleware</span></span>

<span data-ttu-id="544e0-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="544e0-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="544e0-116">[InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi isteği işler ve döndürür bir <xref:System.Threading.Tasks.Task> temsil eden bir ara yazılım yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="544e0-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="544e0-117">Ara yazılım tarafından kuralı etkinleştirildi:</span><span class="sxs-lookup"><span data-stu-id="544e0-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="544e0-118">Etkin bir ara yazılım tarafından <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="544e0-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="544e0-119">Uzantılar için middlewares oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="544e0-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="544e0-120">Fabrika etkinleştirildi Ara yazılımla nesneleri geçirmek mümkün değildir <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span><span class="sxs-lookup"><span data-stu-id="544e0-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="544e0-121">Fabrika etkinleştirildi ara yazılım yerleşik bir kapsayıcıda eklenir `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="544e0-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="544e0-122">İstek işleme ardışık düzeninde her iki middlewares kayıtlı `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="544e0-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="544e0-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="544e0-123">IMiddlewareFactory</span></span>

<span data-ttu-id="544e0-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Ara yazılım oluşturmak için yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="544e0-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="544e0-125">Ara yazılım Üreteç uygulaması kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="544e0-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="544e0-126">Varsayılan <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> uygulaması <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, bulunur [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.</span><span class="sxs-lookup"><span data-stu-id="544e0-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="544e0-127">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="544e0-127">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
