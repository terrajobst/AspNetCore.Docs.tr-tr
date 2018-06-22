---
title: ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme
author: guardrex
description: Kesin türü belirtilmiş Ara ASP.NET Core Fabrika tabanlı etkinleştirme uygulamasında ile kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 44987dbc20b0419865a23e64b60a5dc3f436743a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277078"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="ff4ed-103">ASP.NET Core ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ff4ed-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="ff4ed-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ff4ed-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ff4ed-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) için genişletilebilirlik noktasıdır [ara yazılım](xref:fundamentals/middleware/index) etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="ff4ed-106">`UseMiddleware` genişletme yöntemleri denetleyin bir ara kayıtlı türün uygulayan `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="ff4ed-107">Aşması durumunda `IMiddlewareFactory` kapsayıcısında kayıtlı örneği çözmek için kullanılan `IMiddleware` kurala dayalı Ara etkinleştirme mantığı kullanmak yerine uygulama.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="ff4ed-108">Ara yazılım, uygulamanın hizmet kapsayıcısında kapsamlı veya geçici bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="ff4ed-109">Yararları:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-109">Benefits:</span></span>

* <span data-ttu-id="ff4ed-110">İstek (ekleme kapsamlı Hizmetleri) başına etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ff4ed-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="ff4ed-111">Ara yazılım güçlü yazma</span><span class="sxs-lookup"><span data-stu-id="ff4ed-111">Strong typing of middleware</span></span>

<span data-ttu-id="ff4ed-112">`IMiddleware` kapsamlı Hizmetleri Ara oluşturucuya yerleştirilebilir şekilde istek başına etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="ff4ed-113">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ff4ed-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ff4ed-114">Örnek uygulaması tarafından etkinleştirilen Ara gösterir:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="ff4ed-115">Kuralı.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-115">Convention.</span></span> <span data-ttu-id="ff4ed-116">Geleneksel ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz: [ara yazılım](xref:fundamentals/middleware/index) konu.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="ff4ed-117">Bir [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-117">An [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="ff4ed-118">Varsayılan [MiddlewareFactory sınıfı](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) ara yazılım etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="ff4ed-119">Ara yazılım uygulamaları özdeş işlev ve bir sorgu dizesi parametresi tarafından sağlanan değer kaydedin (`key`).</span><span class="sxs-lookup"><span data-stu-id="ff4ed-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="ff4ed-120">Middlewares eklenen veritabanı bağlamı (kapsamlı bir hizmeti) sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="ff4ed-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="ff4ed-121">IMiddleware</span></span>

<span data-ttu-id="ff4ed-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="ff4ed-123">[InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) yöntemi döndürür ve istekleri işler bir `Task` ara yazılım yürütülmesi temsil eden.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="ff4ed-124">Kural tarafından etkinleştirilen ara:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-124">Middleware activated by convention:</span></span>

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="ff4ed-125">Ara yazılım tarafından etkinleştirilen `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="ff4ed-126">Uzantıları için middlewares oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="ff4ed-127">Nesneleri Fabrika etkinleştirilmiş Ara yazılımla geçirilecek kurulamadığı `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="ff4ed-128">Fabrika etkinleştirilmiş ara yazılım yerleşik kapsayıcısında eklenen *haline*:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

<span data-ttu-id="ff4ed-129">İstek işleme ardışık düzeninde hem middlewares kayıtlı `Configure`:</span><span class="sxs-lookup"><span data-stu-id="ff4ed-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="ff4ed-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="ff4ed-130">IMiddlewareFactory</span></span>

<span data-ttu-id="ff4ed-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara oluşturmak için yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="ff4ed-132">Ara yazılım Üreteç uygulaması kapsayıcısında kapsamlı bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ff4ed-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="ff4ed-133">Varsayılan `IMiddlewareFactory` uygulaması, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), içinde bulunan [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) paket ([başvuru kaynağı](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="ff4ed-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff4ed-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ff4ed-134">Additional resources</span></span>

* [<span data-ttu-id="ff4ed-135">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="ff4ed-135">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ff4ed-136">Bir üçüncü taraf kapsayıcısı ile Ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ff4ed-136">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
