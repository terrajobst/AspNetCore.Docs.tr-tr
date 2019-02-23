---
title: Özel ASP.NET Core ara yazılım yazma
author: rick-anderson
description: Özel ASP.NET Core ara yazılım yazmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744776"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="03a74-103">Özel ASP.NET Core ara yazılım yazma</span><span class="sxs-lookup"><span data-stu-id="03a74-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="03a74-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="03a74-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="03a74-105">Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="03a74-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="03a74-106">ASP.NET Core, zengin bir yerleşik ara yazılım bileşenleri, ancak bazı senaryolarda, özel bir ara yazılım yazmak isteyebilirsiniz kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="03a74-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="03a74-107">Ara yazılım sınıfı</span><span class="sxs-lookup"><span data-stu-id="03a74-107">Middleware class</span></span>

<span data-ttu-id="03a74-108">Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile kullanıma sunulan.</span><span class="sxs-lookup"><span data-stu-id="03a74-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="03a74-109">Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="03a74-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="03a74-110">Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="03a74-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="03a74-111">Bkz: ASP.NET Core'nın yerleşik yerelleştirme desteğini <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="03a74-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="03a74-112">Ara yazılım, kültürü geçirerek test edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="03a74-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="03a74-113">Örneğin: `http://localhost:7997/?culture=no`</span><span class="sxs-lookup"><span data-stu-id="03a74-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="03a74-114">Aşağıdaki kod, bir sınıf için ara yazılım temsilci taşır:</span><span class="sxs-lookup"><span data-stu-id="03a74-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="03a74-115">Ara yazılım `Task` yöntemin adı olmalıdır `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="03a74-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="03a74-116">ASP.NET Core 2.0 veya sonraki sürümlerde, ad ya da olabilir `Invoke` veya `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="03a74-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="03a74-117">Ara yazılım genişletme yöntemi</span><span class="sxs-lookup"><span data-stu-id="03a74-117">Middleware extension method</span></span>

<span data-ttu-id="03a74-118">Aşağıdaki uzantı yöntemi ara yazılımı üzerinden kullanıma sunan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="03a74-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="03a74-119">Aşağıdaki kod ara yazılımı gelen çağrıları `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="03a74-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="03a74-120">Ara yazılım izlemelidir [açık bağımlılıkları İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) tarafından kendi oluşturucusuna bağımlılıkları gösterme.</span><span class="sxs-lookup"><span data-stu-id="03a74-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="03a74-121">Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*.</span><span class="sxs-lookup"><span data-stu-id="03a74-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="03a74-122">Bkz: [istek başına bağımlılıkları](#per-request-dependencies) istek içinde ara yazılım ile Hizmetleri paylaşan gerekiyorsa bölümü.</span><span class="sxs-lookup"><span data-stu-id="03a74-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="03a74-123">Ara yazılım bileşenleri, bunların bağımlılıklarını giderebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) Oluşturucu parametresi üzerinden.</span><span class="sxs-lookup"><span data-stu-id="03a74-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="03a74-124">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) ek parametreler doğrudan da kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="03a74-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="03a74-125">İstek başına bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="03a74-125">Per-request dependencies</span></span>

<span data-ttu-id="03a74-126">Ara yazılım değil istek, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım oluşturucular tarafından kullanılan etkin kalma süresi olmayan paylaşılan hizmetler diğer bağımlılık eklenen türleriyle her isteği sırasında.</span><span class="sxs-lookup"><span data-stu-id="03a74-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="03a74-127">Paylaşmanız gerekir durumunda bir *kapsamlı* hizmet Ara yazılımınızı ve diğer türleri arasında bu hizmetlere ekleme `Invoke` yöntemin imzası.</span><span class="sxs-lookup"><span data-stu-id="03a74-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="03a74-128">`Invoke` Yöntemi tarafından DI doldurulur ek parametreleri kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="03a74-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="03a74-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="03a74-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
