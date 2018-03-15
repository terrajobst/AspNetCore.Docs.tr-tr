---
title: "ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme"
author: guardrex
description: "Kesin türü belirtilmiş Ara ASP.NET Core Fabrika tabanlı etkinleştirme uygulamasında ile kullanmayı öğrenin."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 76ba257abfb11e0c2950b974f837c6ae5818a6a1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="6c9a6-103">ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6c9a6-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="6c9a6-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c9a6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6c9a6-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) için genişletilebilirlik noktasıdır [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="6c9a6-106">`UseMiddleware` genişletme yöntemleri denetleyin bir ara kayıtlı türün uygulayan `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="6c9a6-107">Aşması durumunda `IMiddlewareFactory` kapsayıcısında kayıtlı örneği çözmek için kullanılan `IMiddleware` kurala dayalı Ara etkinleştirme mantığı kullanmak yerine uygulama.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="6c9a6-108">Ara yazılım, uygulamanın hizmet kapsayıcısında kapsamlı veya geçici bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="6c9a6-109">Yararları:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-109">Benefits:</span></span>

* <span data-ttu-id="6c9a6-110">İstek (ekleme kapsamlı Hizmetleri) başına etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6c9a6-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="6c9a6-111">Ara yazılım güçlü yazma</span><span class="sxs-lookup"><span data-stu-id="6c9a6-111">Strong typing of middleware</span></span>

<span data-ttu-id="6c9a6-112">`IMiddleware` kapsamlı Hizmetleri Ara oluşturucuya yerleştirilebilir şekilde istek başına etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="6c9a6-113">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c9a6-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6c9a6-114">Örnek uygulaması tarafından etkinleştirilen Ara gösterir:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="6c9a6-115">Kuralı.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-115">Convention.</span></span> <span data-ttu-id="6c9a6-116">Geleneksel ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz: [ara yazılım](xref:fundamentals/middleware/index) konu.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="6c9a6-117">Bir [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="6c9a6-118">Varsayılan [MiddlewareFactory sınıfı](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) ara yazılım etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="6c9a6-119">Ara yazılım uygulamaları özdeş işlev ve bir sorgu dizesi parametresi tarafından sağlanan değer kaydedin (`key`).</span><span class="sxs-lookup"><span data-stu-id="6c9a6-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="6c9a6-120">Middlewares eklenen veritabanı bağlamı (kapsamlı bir hizmeti) sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="6c9a6-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="6c9a6-121">IMiddleware</span></span>

<span data-ttu-id="6c9a6-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="6c9a6-123">[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) yöntemi döndürür ve istekleri işler bir `Task` ara yazılım yürütülmesi temsil eden.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="6c9a6-124">Kural tarafından etkinleştirilen ara:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="6c9a6-125">Ara yazılım tarafından etkinleştirilen `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="6c9a6-126">Uzantıları için middlewares oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="6c9a6-127">Nesneleri Fabrika etkinleştirilmiş Ara yazılımla geçirilecek kurulamadığı `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="6c9a6-128">Fabrika etkinleştirilmiş ara yazılım yerleşik kapsayıcısında eklenen *haline*:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="6c9a6-129">İstek işleme ardışık düzeninde hem middlewares kayıtlı `Configure`:</span><span class="sxs-lookup"><span data-stu-id="6c9a6-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="6c9a6-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="6c9a6-130">IMiddlewareFactory</span></span>

<span data-ttu-id="6c9a6-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara oluşturmak için yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="6c9a6-132">Ara yazılım Üreteç uygulaması kapsayıcısında kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6c9a6-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="6c9a6-133">Varsayılan `IMiddlewareFactory` uygulaması, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), içinde bulunan [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket ([başvuru kaynağı](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="6c9a6-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c9a6-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6c9a6-134">Additional resources</span></span>

* [<span data-ttu-id="6c9a6-135">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="6c9a6-135">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="6c9a6-136">Bir üçüncü taraf kapsayıcısı ile Ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6c9a6-136">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
