---
title: ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme
author: guardrex
description: ASP.NET core'da etkinleştirme Fabrika tabanlı bir uygulama türü kesin belirlenmiş bir ara yazılım kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: d29c4d3d72ddd8ec3c2a726ee35ae1dc82774537
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750532"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="f1733-103">ASP.NET core'da Fabrika tabanlı ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f1733-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="f1733-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1733-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1733-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) bir genişletilebilirlik noktası [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="f1733-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="f1733-106">`UseMiddleware` genişletme yöntemleri denetleyin bir ara kayıtlı tür uygulayan olup `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="f1733-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="f1733-107">Aksi halde `IMiddlewareFactory` çözümlemek için kullanılan kapsayıcıda kayıtlı örnek `IMiddleware` kural tabanlı ara yazılım etkinleştirme mantığı kullanmak yerine uygulama.</span><span class="sxs-lookup"><span data-stu-id="f1733-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="f1733-108">Ara yazılım, uygulamanın service kapsayıcısında kapsamlı ya da geçici bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f1733-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="f1733-109">Avantajlar:</span><span class="sxs-lookup"><span data-stu-id="f1733-109">Benefits:</span></span>

* <span data-ttu-id="f1733-110">İstemci istek (kapsamlı Hizmetleri ekleme) başına etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f1733-110">Activation per client request (injection of scoped services)</span></span>
* <span data-ttu-id="f1733-111">Güçlü ara yazılım yazma</span><span class="sxs-lookup"><span data-stu-id="f1733-111">Strong typing of middleware</span></span>

<span data-ttu-id="f1733-112">`IMiddleware` kapsamı belirlenmiş hizmetler eklenen bu nedenle Ara oluşturucuya (bağlantı), istemci istek etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f1733-112">`IMiddleware` is activated per client request (connection), so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="f1733-113">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1733-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f1733-114">Örnek uygulama tarafından etkinleştirilen bir ara yazılım gösterir:</span><span class="sxs-lookup"><span data-stu-id="f1733-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="f1733-115">Kuralı.</span><span class="sxs-lookup"><span data-stu-id="f1733-115">Convention.</span></span> <span data-ttu-id="f1733-116">Geleneksel bir ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz. [ara yazılım](xref:fundamentals/middleware/index) konu.</span><span class="sxs-lookup"><span data-stu-id="f1733-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="f1733-117">Bir [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="f1733-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="f1733-118">Varsayılan [MiddlewareFactory sınıfı](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) ara yazılım etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="f1733-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="f1733-119">Ara yazılım uygulamaları aynı şekilde çalışır ve bir sorgu dizesi parametresi tarafından sağlanan değerini kaydedin (`key`).</span><span class="sxs-lookup"><span data-stu-id="f1733-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="f1733-120">Middlewares, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için bir eklenen veritabanı bağlamı (kapsamlı bir hizmet) kullanın.</span><span class="sxs-lookup"><span data-stu-id="f1733-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="f1733-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="f1733-121">IMiddleware</span></span>

<span data-ttu-id="f1733-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) ara yazılımı için uygulamanın istek ardışık düzenini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f1733-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="f1733-123">[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) yöntemi isteği işler ve döndürür bir `Task` temsil eden bir ara yazılım yürütülmesi.</span><span class="sxs-lookup"><span data-stu-id="f1733-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="f1733-124">Ara yazılım tarafından kuralı etkinleştirildi:</span><span class="sxs-lookup"><span data-stu-id="f1733-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="f1733-125">Etkin bir ara yazılım tarafından `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="f1733-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="f1733-126">Uzantılar için middlewares oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="f1733-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="f1733-127">Fabrika etkinleştirildi Ara yazılımla nesneleri geçirmek mümkün değildir `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="f1733-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

<span data-ttu-id="f1733-128">Fabrika etkinleştirildi ara yazılım yerleşik bir kapsayıcıda eklenir *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1733-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="f1733-129">İstek işleme ardışık düzeninde her iki middlewares kayıtlı `Configure`:</span><span class="sxs-lookup"><span data-stu-id="f1733-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="f1733-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="f1733-130">IMiddlewareFactory</span></span>

<span data-ttu-id="f1733-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara yazılım oluşturmak için yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="f1733-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="f1733-132">Ara yazılım Üreteç uygulaması kapsayıcıda kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f1733-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="f1733-133">Varsayılan `IMiddlewareFactory` uygulaması [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), bulunur [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket.</span><span class="sxs-lookup"><span data-stu-id="f1733-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1733-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f1733-134">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
