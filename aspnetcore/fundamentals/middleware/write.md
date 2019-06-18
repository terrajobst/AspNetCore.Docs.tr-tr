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
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="7ade8-103">Özel ASP.NET Core ara yazılım yazma</span><span class="sxs-lookup"><span data-stu-id="7ade8-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="7ade8-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7ade8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7ade8-105">Ara yazılım isteklerini ve yanıtlarını işlemek için bir uygulama ardışık birleştirilmiş bir yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="7ade8-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="7ade8-106">ASP.NET Core, zengin bir yerleşik ara yazılım bileşenleri, ancak bazı senaryolarda, özel bir ara yazılım yazmak isteyebilirsiniz kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ade8-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="7ade8-107">Ara yazılım sınıfı</span><span class="sxs-lookup"><span data-stu-id="7ade8-107">Middleware class</span></span>

<span data-ttu-id="7ade8-108">Ara yazılım genellikle bir sınıfta kapsüllenmiş ve bir genişletme yöntemi ile kullanıma sunulan.</span><span class="sxs-lookup"><span data-stu-id="7ade8-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="7ade8-109">Kültür geçerli istek için Sorgu dizesinden ayarlar aşağıdaki Ara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="7ade8-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="7ade8-110">Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturma göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7ade8-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="7ade8-111">Bkz: ASP.NET Core'nın yerleşik yerelleştirme desteğini <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="7ade8-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="7ade8-112">Ara yazılım, kültürü geçirerek test edin.</span><span class="sxs-lookup"><span data-stu-id="7ade8-112">Test the middleware by passing in the culture.</span></span> <span data-ttu-id="7ade8-113">Örneğin, istek `https://localhost:5001/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="7ade8-113">For example, request `https://localhost:5001/?culture=no`.</span></span>

<span data-ttu-id="7ade8-114">Aşağıdaki kod, bir sınıf için ara yazılım temsilci taşır:</span><span class="sxs-lookup"><span data-stu-id="7ade8-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="7ade8-115">Ara yazılım sınıfı içermelidir:</span><span class="sxs-lookup"><span data-stu-id="7ade8-115">The middleware class must include:</span></span>

* <span data-ttu-id="7ade8-116">Türünde bir parametre içeren bir genel oluşturucuya <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span><span class="sxs-lookup"><span data-stu-id="7ade8-116">A public constructor with a parameter of type <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="7ade8-117">Adlı bir genel yöntem `Invoke` veya `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="7ade8-117">A public method named `Invoke` or `InvokeAsync`.</span></span> <span data-ttu-id="7ade8-118">Bu yöntem gerekir:</span><span class="sxs-lookup"><span data-stu-id="7ade8-118">This method must:</span></span>
  * <span data-ttu-id="7ade8-119">Döndürür bir `Task`.</span><span class="sxs-lookup"><span data-stu-id="7ade8-119">Return a `Task`.</span></span>
  * <span data-ttu-id="7ade8-120">Türünde bir ilk parametre kabul <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="7ade8-120">Accept a first parameter of type <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>
  
<span data-ttu-id="7ade8-121">Oluşturucu için ek parametreler ve `Invoke` / `InvokeAsync` tarafından doldurulur [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7ade8-121">Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware-dependencies"></a><span data-ttu-id="7ade8-122">Ara yazılım bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="7ade8-122">Middleware dependencies</span></span>

<span data-ttu-id="7ade8-123">Ara yazılım izlemelidir [açık bağımlılıkları İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) tarafından kendi oluşturucusuna bağımlılıkları gösterme.</span><span class="sxs-lookup"><span data-stu-id="7ade8-123">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="7ade8-124">Ara yazılım yapılandırılmıştır kez başına *uygulama ömrü*.</span><span class="sxs-lookup"><span data-stu-id="7ade8-124">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="7ade8-125">Bkz: [istek başına ara yazılım bağımlılıkları](#per-request-middleware-dependencies) istek içinde ara yazılım ile Hizmetleri paylaşan gerekiyorsa bölümü.</span><span class="sxs-lookup"><span data-stu-id="7ade8-125">See the [Per-request middleware dependencies](#per-request-middleware-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="7ade8-126">Ara yazılım bileşenleri, bunların bağımlılıklarını giderebilir [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) Oluşturucu parametresi üzerinden.</span><span class="sxs-lookup"><span data-stu-id="7ade8-126">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="7ade8-127">[UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) ek parametreler doğrudan da kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="7ade8-127">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-middleware-dependencies"></a><span data-ttu-id="7ade8-128">İstek başına ara yazılım bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="7ade8-128">Per-request middleware dependencies</span></span>

<span data-ttu-id="7ade8-129">Ara yazılım değil istek, uygulama başlatma sırasında oluşturulur çünkü *kapsamlı* ara yazılım oluşturucular tarafından kullanılan etkin kalma süresi olmayan paylaşılan hizmetler diğer bağımlılık eklenen türleriyle her isteği sırasında.</span><span class="sxs-lookup"><span data-stu-id="7ade8-129">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="7ade8-130">Paylaşmanız gerekir durumunda bir *kapsamlı* hizmet Ara yazılımınızı ve diğer türleri arasında bu hizmetlere ekleme `Invoke` yöntemin imzası.</span><span class="sxs-lookup"><span data-stu-id="7ade8-130">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="7ade8-131">`Invoke` Yöntemi tarafından DI doldurulur ek parametreleri kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="7ade8-131">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="middleware-extension-method"></a><span data-ttu-id="7ade8-132">Ara yazılım genişletme yöntemi</span><span class="sxs-lookup"><span data-stu-id="7ade8-132">Middleware extension method</span></span>

<span data-ttu-id="7ade8-133">Aşağıdaki uzantı yöntemi ara yazılımı üzerinden kullanıma sunan <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="7ade8-133">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="7ade8-134">Aşağıdaki kod ara yazılımı gelen çağrıları `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="7ade8-134">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

## <a name="additional-resources"></a><span data-ttu-id="7ade8-135">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7ade8-135">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
