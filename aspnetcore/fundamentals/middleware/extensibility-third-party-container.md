---
title: ASP.NET Core bir üçüncü taraf kapsayıcısı ile ara yazılım etkinleştirme
author: guardrex
description: Fabrika tabanlı etkinleştirme ve ASP.NET Core bir üçüncü taraf kapsayıcısı ile kesin olarak belirlenmiş bir ara yazılımı nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: e54a2bd366457fa2d898b7ee26e95021aec5389b
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187087"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="9c12e-103">ASP.NET Core bir üçüncü taraf kapsayıcısı ile ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="9c12e-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="9c12e-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9c12e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9c12e-105">Bu makalede <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> , üçüncü taraf kapsayıcı ile <xref:Microsoft.AspNetCore.Http.IMiddleware> [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktası ve nasıl kullanılacağı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-105">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="9c12e-106"><xref:fundamentals/middleware/extensibility>Ve `IMiddlewareFactory` hakkında`IMiddleware`giriş bilgileri için bkz.</span><span class="sxs-lookup"><span data-stu-id="9c12e-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="9c12e-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c12e-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9c12e-108">Örnek uygulama, `IMiddlewareFactory` `SimpleInjectorMiddlewareFactory`bir uygulama tarafından ara yazılım etkinleştirmesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="9c12e-109">Örnek, [basit Injector](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="9c12e-110">Örneğin ara yazılım uygulamasının bir sorgu dizesi parametresi (`key`) tarafından belirtilen değeri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9c12e-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="9c12e-111">Ara yazılım, sorgu dizesi değerini bellek içi bir veritabanına kaydetmek için eklenen bir veritabanı bağlamını (kapsamlı bir hizmet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="9c12e-112">Örnek uygulama, yalnızca tanıtım amacıyla [basit Injector](https://github.com/simpleinjector/SimpleInjector) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="9c12e-113">Basit Injector kullanımı bir onay değildir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="9c12e-114">Basit Injector belgelerinde açıklanan ara yazılım etkinleştirme yaklaşımları ve GitHub sorunları, basit Injector 'ın sürdürülebilir tarafından önerilir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="9c12e-115">Daha fazla bilgi için bkz. [basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit injecgithub deposu](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="9c12e-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="9c12e-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="9c12e-116">IMiddlewareFactory</span></span>

<span data-ttu-id="9c12e-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c12e-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="9c12e-118">Örnek uygulamada bir `SimpleInjectorActivatedMiddleware` örnek oluşturmak için bir ara yazılım fabrikası uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="9c12e-119">Ara yazılım fabrikası, ara yazılımı çözümlemek için basit Injector kapsayıcısını kullanır:</span><span class="sxs-lookup"><span data-stu-id="9c12e-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="9c12e-120">Imıddliware</span><span class="sxs-lookup"><span data-stu-id="9c12e-120">IMiddleware</span></span>

<span data-ttu-id="9c12e-121"><xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9c12e-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="9c12e-122">Bir `IMiddlewareFactory` uygulama tarafından etkinleştirilen ara yazılım (*Ara yazılım/simpleınjectoractivatedara yazılım. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c12e-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="9c12e-123">Ara yazılım (*Ara yazılım/MiddlewareExtensions. cs*) için bir uzantı oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="9c12e-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="9c12e-124">`Startup.ConfigureServices`birkaç görev gerçekleştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="9c12e-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="9c12e-125">Basit Injector kapsayıcısını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9c12e-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="9c12e-126">Fabrika ve ara yazılımı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9c12e-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="9c12e-127">Uygulamanın veritabanı bağlamını basit ınjtor kapsayıcısından kullanılabilir hale getirin.</span><span class="sxs-lookup"><span data-stu-id="9c12e-127">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="9c12e-128">Ara yazılım, içindeki `Startup.Configure`istek işleme ardışık düzenine kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="9c12e-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/3.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9c12e-129">Bu makalede <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> , üçüncü taraf kapsayıcı ile <xref:Microsoft.AspNetCore.Http.IMiddleware> [Ara yazılım](xref:fundamentals/middleware/index) etkinleştirme için bir genişletilebilirlik noktası ve nasıl kullanılacağı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-129">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="9c12e-130"><xref:fundamentals/middleware/extensibility>Ve `IMiddlewareFactory` hakkında`IMiddleware`giriş bilgileri için bkz.</span><span class="sxs-lookup"><span data-stu-id="9c12e-130">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="9c12e-131">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c12e-131">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9c12e-132">Örnek uygulama, `IMiddlewareFactory` `SimpleInjectorMiddlewareFactory`bir uygulama tarafından ara yazılım etkinleştirmesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-132">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="9c12e-133">Örnek, [basit Injector](https://simpleinjector.org) bağımlılık ekleme (dı) kapsayıcısını kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-133">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="9c12e-134">Örneğin ara yazılım uygulamasının bir sorgu dizesi parametresi (`key`) tarafından belirtilen değeri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="9c12e-134">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="9c12e-135">Ara yazılım, sorgu dizesi değerini bellek içi bir veritabanına kaydetmek için eklenen bir veritabanı bağlamını (kapsamlı bir hizmet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-135">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="9c12e-136">Örnek uygulama, yalnızca tanıtım amacıyla [basit Injector](https://github.com/simpleinjector/SimpleInjector) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-136">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="9c12e-137">Basit Injector kullanımı bir onay değildir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-137">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="9c12e-138">Basit Injector belgelerinde açıklanan ara yazılım etkinleştirme yaklaşımları ve GitHub sorunları, basit Injector 'ın sürdürülebilir tarafından önerilir.</span><span class="sxs-lookup"><span data-stu-id="9c12e-138">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="9c12e-139">Daha fazla bilgi için bkz. [basit Injector belgeleri](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit injecgithub deposu](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="9c12e-139">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="9c12e-140">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="9c12e-140">IMiddlewareFactory</span></span>

<span data-ttu-id="9c12e-141"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>ara yazılım oluşturmak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c12e-141"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="9c12e-142">Örnek uygulamada bir `SimpleInjectorActivatedMiddleware` örnek oluşturmak için bir ara yazılım fabrikası uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9c12e-142">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="9c12e-143">Ara yazılım fabrikası, ara yazılımı çözümlemek için basit Injector kapsayıcısını kullanır:</span><span class="sxs-lookup"><span data-stu-id="9c12e-143">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="9c12e-144">Imıddliware</span><span class="sxs-lookup"><span data-stu-id="9c12e-144">IMiddleware</span></span>

<span data-ttu-id="9c12e-145"><xref:Microsoft.AspNetCore.Http.IMiddleware>uygulamanın istek ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="9c12e-145"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="9c12e-146">Bir `IMiddlewareFactory` uygulama tarafından etkinleştirilen ara yazılım (*Ara yazılım/simpleınjectoractivatedara yazılım. cs*):</span><span class="sxs-lookup"><span data-stu-id="9c12e-146">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="9c12e-147">Ara yazılım (*Ara yazılım/MiddlewareExtensions. cs*) için bir uzantı oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="9c12e-147">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="9c12e-148">`Startup.ConfigureServices`birkaç görev gerçekleştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="9c12e-148">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="9c12e-149">Basit Injector kapsayıcısını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="9c12e-149">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="9c12e-150">Fabrika ve ara yazılımı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9c12e-150">Register the factory and middleware.</span></span>
* <span data-ttu-id="9c12e-151">Uygulamanın veritabanı bağlamını basit ınjtor kapsayıcısından kullanılabilir hale getirin.</span><span class="sxs-lookup"><span data-stu-id="9c12e-151">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="9c12e-152">Ara yazılım, içindeki `Startup.Configure`istek işleme ardışık düzenine kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="9c12e-152">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=12)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9c12e-153">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9c12e-153">Additional resources</span></span>

* [<span data-ttu-id="9c12e-154">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="9c12e-154">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="9c12e-155">Fabrika tabanlı ara yazılım etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="9c12e-155">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="9c12e-156">Basit ınjecgithub deposu</span><span class="sxs-lookup"><span data-stu-id="9c12e-156">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="9c12e-157">Basit Injector belgeleri</span><span class="sxs-lookup"><span data-stu-id="9c12e-157">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
