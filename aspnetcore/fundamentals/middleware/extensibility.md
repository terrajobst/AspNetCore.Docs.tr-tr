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
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="27510-103">ASP.NET Core 'de fabrika tabanlı ara yazılım etkinleştirmesi</span><span class="sxs-lookup"><span data-stu-id="27510-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="27510-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="27510-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="27510-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="27510-105"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="27510-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>Uzantı yöntemleri bir ara yazılımın kayıtlı türünün uygulayıp uygulamadığını <xref:Microsoft.AspNetCore.Http.IMiddleware>denetler.</span><span class="sxs-lookup"><span data-stu-id="27510-106"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="27510-107">Bu durumda, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> kapsayıcıya kaydedilen örnek, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> , uygulamayı çözmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27510-107">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="27510-108">Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27510-108">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="27510-109">Larından</span><span class="sxs-lookup"><span data-stu-id="27510-109">Benefits:</span></span>

* <span data-ttu-id="27510-110">İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)</span><span class="sxs-lookup"><span data-stu-id="27510-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="27510-111">Ara yazılım sıkı yazma</span><span class="sxs-lookup"><span data-stu-id="27510-111">Strong typing of middleware</span></span>

<span data-ttu-id="27510-112"><xref:Microsoft.AspNetCore.Http.IMiddleware>istemci isteği başına etkinleştirilir (bağlantı), bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="27510-112"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="27510-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27510-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="27510-114">Imıddliware</span><span class="sxs-lookup"><span data-stu-id="27510-114">IMiddleware</span></span>

<span data-ttu-id="27510-115"><xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="27510-115"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="27510-116">[InvokeAsync (HttpContext, requestdelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılım yürütmesini <xref:System.Threading.Tasks.Task> temsil eden bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="27510-116">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="27510-117">Kurala göre etkinleştirilen ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="27510-117">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="27510-118">Etkinleştirilen ara yazılım <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="27510-118">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="27510-119">Middlewares için Uzantılar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="27510-119">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="27510-120">Nesneleri fabrikada etkinleştirilen bir ara yazılıma <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>geçirmek mümkün değildir:</span><span class="sxs-lookup"><span data-stu-id="27510-120">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="27510-121">Fabrikada etkinleştirilen ara yazılım, içindeki `Startup.ConfigureServices`yerleşik kapsayıcıya eklenir:</span><span class="sxs-lookup"><span data-stu-id="27510-121">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="27510-122">Her iki middlewares de `Startup.Configure`istek işleme ardışık düzeninde kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="27510-122">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="27510-123">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="27510-123">IMiddlewareFactory</span></span>

<span data-ttu-id="27510-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="27510-124"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="27510-125">Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27510-125">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="27510-126"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Varsayılan<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>uygulama, [Microsoft. aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="27510-126">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="27510-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware>, [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="27510-127"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="27510-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>Uzantı yöntemleri bir ara yazılımın kayıtlı türünün uygulayıp uygulamadığını <xref:Microsoft.AspNetCore.Http.IMiddleware>denetler.</span><span class="sxs-lookup"><span data-stu-id="27510-128"><xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> extension methods check if a middleware's registered type implements <xref:Microsoft.AspNetCore.Http.IMiddleware>.</span></span> <span data-ttu-id="27510-129">Bu durumda, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> kapsayıcıya kaydedilen örnek, kural tabanlı ara yazılım etkinleştirme mantığını kullanmak yerine <xref:Microsoft.AspNetCore.Http.IMiddleware> , uygulamayı çözmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="27510-129">If it does, the <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> instance registered in the container is used to resolve the <xref:Microsoft.AspNetCore.Http.IMiddleware> implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="27510-130">Ara yazılım, uygulamanın hizmet kapsayıcısında [kapsamlı veya geçici bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes) olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27510-130">The middleware is registered as a [scoped or transient service](xref:fundamentals/dependency-injection#service-lifetimes) in the app's service container.</span></span>

<span data-ttu-id="27510-131">Larından</span><span class="sxs-lookup"><span data-stu-id="27510-131">Benefits:</span></span>

* <span data-ttu-id="27510-132">İstemci isteği başına etkinleştirme (kapsamlı hizmetler ekleme)</span><span class="sxs-lookup"><span data-stu-id="27510-132">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="27510-133">Ara yazılım sıkı yazma</span><span class="sxs-lookup"><span data-stu-id="27510-133">Strong typing of middleware</span></span>

<span data-ttu-id="27510-134"><xref:Microsoft.AspNetCore.Http.IMiddleware>istemci isteği başına etkinleştirilir (bağlantı), bu nedenle kapsamlı hizmetler ara yazılım oluşturucusuna eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="27510-134"><xref:Microsoft.AspNetCore.Http.IMiddleware> is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="27510-135">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27510-135">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="imiddleware"></a><span data-ttu-id="27510-136">Imıddliware</span><span class="sxs-lookup"><span data-stu-id="27510-136">IMiddleware</span></span>

<span data-ttu-id="27510-137"><xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="27510-137"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="27510-138">[InvokeAsync (HttpContext, requestdelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) yöntemi istekleri işler ve ara yazılım yürütmesini <xref:System.Threading.Tasks.Task> temsil eden bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="27510-138">The [InvokeAsync(HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) method handles requests and returns a <xref:System.Threading.Tasks.Task> that represents the execution of the middleware.</span></span>

<span data-ttu-id="27510-139">Kurala göre etkinleştirilen ara yazılım:</span><span class="sxs-lookup"><span data-stu-id="27510-139">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="27510-140">Etkinleştirilen ara yazılım <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span><span class="sxs-lookup"><span data-stu-id="27510-140">Middleware activated by <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="27510-141">Middlewares için Uzantılar oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="27510-141">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="27510-142">Nesneleri fabrikada etkinleştirilen bir ara yazılıma <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>geçirmek mümkün değildir:</span><span class="sxs-lookup"><span data-stu-id="27510-142">It isn't possible to pass objects to the factory-activated middleware with <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="27510-143">Fabrikada etkinleştirilen ara yazılım, içindeki `Startup.ConfigureServices`yerleşik kapsayıcıya eklenir:</span><span class="sxs-lookup"><span data-stu-id="27510-143">The factory-activated middleware is added to the built-in container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="27510-144">Her iki middlewares de `Startup.Configure`istek işleme ardışık düzeninde kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="27510-144">Both middlewares are registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="27510-145">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="27510-145">IMiddlewareFactory</span></span>

<span data-ttu-id="27510-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="27510-146"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span> <span data-ttu-id="27510-147">Ara yazılım Fabrikası Uygulama, kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27510-147">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="27510-148"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> Varsayılan<xref:Microsoft.AspNetCore.Http.MiddlewareFactory>uygulama, [Microsoft. aspnetcore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paketinde bulunur.</span><span class="sxs-lookup"><span data-stu-id="27510-148">The default <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementation, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="27510-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="27510-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
