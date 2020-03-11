---
title: Özel ASP.NET Core ara yazılımı yaz
author: rick-anderson
description: Özel ASP.NET Core ara yazılımı yazmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: e74bba9e1bd826d4f493b0ee642a198f984daada
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663928"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="bc02f-103">Özel ASP.NET Core ara yazılımı yaz</span><span class="sxs-lookup"><span data-stu-id="bc02f-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="bc02f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bc02f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bc02f-105">Ara yazılım, istekleri ve yanıtları işlemek için bir uygulama ardışık düzenine çevrilmiş yazılımdır.</span><span class="sxs-lookup"><span data-stu-id="bc02f-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="bc02f-106">ASP.NET Core, zengin bir yerleşik ara yazılım bileşenleri kümesi sağlar, ancak bazı senaryolarda özel bir ara yazılım yazmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc02f-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="bc02f-107">Ara yazılım sınıfı</span><span class="sxs-lookup"><span data-stu-id="bc02f-107">Middleware class</span></span>

<span data-ttu-id="bc02f-108">Ara yazılım genellikle bir sınıfta kapsüllenir ve bir genişletme yöntemiyle birlikte sunulur.</span><span class="sxs-lookup"><span data-stu-id="bc02f-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="bc02f-109">Bir sorgu dizesinden geçerli istek için kültürü ayarlayan aşağıdaki ara yazılımı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="bc02f-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](write/snapshot/StartupCulture.cs)]

<span data-ttu-id="bc02f-110">Yukarıdaki örnek kod, bir ara yazılım bileşeni oluşturmayı göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc02f-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="bc02f-111">ASP.NET Core yerleşik yerelleştirme desteği için bkz. <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="bc02f-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="bc02f-112">Kültürü geçirerek, ara yazılımı test edin.</span><span class="sxs-lookup"><span data-stu-id="bc02f-112">Test the middleware by passing in the culture.</span></span> <span data-ttu-id="bc02f-113">Örneğin, istek `https://localhost:5001/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="bc02f-113">For example, request `https://localhost:5001/?culture=no`.</span></span>

<span data-ttu-id="bc02f-114">Aşağıdaki kod, ara yazılım temsilcisini bir sınıfa taşıtabilirler:</span><span class="sxs-lookup"><span data-stu-id="bc02f-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddleware.cs)]

<span data-ttu-id="bc02f-115">Ara yazılım sınıfı şunları içermelidir:</span><span class="sxs-lookup"><span data-stu-id="bc02f-115">The middleware class must include:</span></span>

* <span data-ttu-id="bc02f-116"><xref:Microsoft.AspNetCore.Http.RequestDelegate>türünde bir parametreye sahip ortak Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="bc02f-116">A public constructor with a parameter of type <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span>
* <span data-ttu-id="bc02f-117">`Invoke` veya `InvokeAsync`adlı bir genel yöntem.</span><span class="sxs-lookup"><span data-stu-id="bc02f-117">A public method named `Invoke` or `InvokeAsync`.</span></span> <span data-ttu-id="bc02f-118">Bu yöntem şunları içermelidir:</span><span class="sxs-lookup"><span data-stu-id="bc02f-118">This method must:</span></span>
  * <span data-ttu-id="bc02f-119">`Task`döndürün.</span><span class="sxs-lookup"><span data-stu-id="bc02f-119">Return a `Task`.</span></span>
  * <span data-ttu-id="bc02f-120"><xref:Microsoft.AspNetCore.Http.HttpContext>türünde bir ilk parametreyi kabul edin.</span><span class="sxs-lookup"><span data-stu-id="bc02f-120">Accept a first parameter of type <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>
  
<span data-ttu-id="bc02f-121">Oluşturucu ve `Invoke`/`InvokeAsync` için ek parametreler, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="bc02f-121">Additional parameters for the constructor and `Invoke`/`InvokeAsync` are populated by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware-dependencies"></a><span data-ttu-id="bc02f-122">Ara yazılım bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="bc02f-122">Middleware dependencies</span></span>

<span data-ttu-id="bc02f-123">Ara yazılım, bağımlılıklarındaki bağımlılıklarını açığa çıkararak [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) izlemelidir.</span><span class="sxs-lookup"><span data-stu-id="bc02f-123">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="bc02f-124">Ara yazılım, *uygulama ömrü*başına bir kez oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc02f-124">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="bc02f-125">Bir istek içindeki ara yazılım ile hizmetleri paylaşmanız gerekiyorsa, [Istek başına ara yazılım bağımlılıkları](#per-request-middleware-dependencies) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="bc02f-125">See the [Per-request middleware dependencies](#per-request-middleware-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="bc02f-126">Ara yazılım bileşenleri, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) öğesinden Oluşturucu parametreleri aracılığıyla bağımlılıklarını çözümleyebilir.</span><span class="sxs-lookup"><span data-stu-id="bc02f-126">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="bc02f-127">[Useara yazılım&lt;t&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) doğrudan ek parametreleri de kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="bc02f-127">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-middleware-dependencies"></a><span data-ttu-id="bc02f-128">İstek başına ara yazılım bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="bc02f-128">Per-request middleware dependencies</span></span>

<span data-ttu-id="bc02f-129">Ara yazılım uygulama başlangıcında oluşturulduğundan, isteğe göre değil, ara yazılım oluşturucuları tarafından kullanılan *kapsamlı* ömür Hizmetleri, her istek sırasında bağımlılığı eklenen diğer türlerle paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="bc02f-129">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="bc02f-130">Bir *kapsamlı* hizmeti, ara yazılım ve diğer türler arasında paylaşmanız gerekiyorsa, bu hizmetleri `Invoke` yönteminin imzasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc02f-130">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="bc02f-131">`Invoke` yöntemi, DI tarafından doldurulan ek parametreleri kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="bc02f-131">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="middleware-extension-method"></a><span data-ttu-id="bc02f-132">Ara yazılım genişletme yöntemi</span><span class="sxs-lookup"><span data-stu-id="bc02f-132">Middleware extension method</span></span>

<span data-ttu-id="bc02f-133">Aşağıdaki genişletme yöntemi, <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>aracılığıyla ara yazılımı kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="bc02f-133">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](write/snapshot/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="bc02f-134">Aşağıdaki kod `Startup.Configure`' dan ara yazılımı çağırır:</span><span class="sxs-lookup"><span data-stu-id="bc02f-134">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](write/snapshot/Startup.cs?highlight=5)]

## <a name="additional-resources"></a><span data-ttu-id="bc02f-135">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bc02f-135">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
