---
title: "ASP.NET Core üçüncü taraf kapsayıcısında ile ara yazılımı Fabrika tabanlı etkinleştirme"
author: guardrex
description: "Kesin türü belirtilmiş ara yazılımı Fabrika tabanlı etkinleştirme ve bir üçüncü taraf kapsayıcı ASP.NET çekirdek kullanmayı öğrenin."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: bbfd9d748df59418345ddd3eacd6f44b75f6e851
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="factory-based-middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="ccac8-103">ASP.NET Core üçüncü taraf kapsayıcısında ile ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ccac8-103">Factory-based middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="ccac8-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ccac8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ccac8-105">Bu makalede nasıl kullanılacağı gösterilmektedir [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ve [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) için genişletilebilirlik noktası olarak [ara yazılımı](xref:fundamentals/middleware/index) bir üçüncü taraf kapsayıcısı ile etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="ccac8-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="ccac8-106">Hakkında tanıtıcı bilgi `IMiddlewareFactory` ve `IMiddleware`, bkz: [ara yazılımı Fabrika tabanlı etkinleştirme](xref:fundamentals/middleware/extensibility) konu.</span><span class="sxs-lookup"><span data-stu-id="ccac8-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="ccac8-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ccac8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ccac8-108">Örnek uygulama tarafından ara yazılım etkinleştirme gösterir bir `IMiddlewareFactory` uygulaması, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="ccac8-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="ccac8-109">Örnek kullanır [basit Injector](https://github.com/simpleinjector/SimpleInjector) bağımlılık ekleme (dı) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="ccac8-109">The sample uses the [Simple Injector](https://github.com/simpleinjector/SimpleInjector) dependency injection (DI) container.</span></span>

<span data-ttu-id="ccac8-110">Bir sorgu dizesi parametresi tarafından sağlanan değer örnek 's Ara uygulama kaydeder (`key`).</span><span class="sxs-lookup"><span data-stu-id="ccac8-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="ccac8-111">Ara yazılım, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için eklenen veritabanı bağlamı (kapsamlı bir hizmeti) kullanır.</span><span class="sxs-lookup"><span data-stu-id="ccac8-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="ccac8-112">Örnek uygulama kullandığı [basit Injector](https://github.com/simpleinjector/SimpleInjector) yalnızca tanıtım amacıyla.</span><span class="sxs-lookup"><span data-stu-id="ccac8-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="ccac8-113">Basit Injector kullanımını bir onay değil.</span><span class="sxs-lookup"><span data-stu-id="ccac8-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="ccac8-114">Ara yazılım etkinleştirme yaklaşım basit Injector belgelerinde açıklanan ve GitHub sorunları basit Injector maintainers tarafından önerilir.</span><span class="sxs-lookup"><span data-stu-id="ccac8-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="ccac8-115">Daha fazla bilgi için bkz: [basit Injector belgelerine](https://simpleinjector.readthedocs.io/en/latest/index.html) ve [basit Injector GitHub deposunu](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="ccac8-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="ccac8-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="ccac8-116">IMiddlewareFactory</span></span>

<span data-ttu-id="ccac8-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara oluşturmak için yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ccac8-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="ccac8-118">Örnek uygulaması oluşturmak için bir ara yazılım fabrikası uygulanan bir `SimpleInjectorActivatedMiddleware` örneği.</span><span class="sxs-lookup"><span data-stu-id="ccac8-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="ccac8-119">Ara yazılım Fabrika ara yazılım çözümlemek için basit Injector kapsayıcı kullanır:</span><span class="sxs-lookup"><span data-stu-id="ccac8-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="ccac8-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="ccac8-120">IMiddleware</span></span>

<span data-ttu-id="ccac8-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulamanın isteği ardışık düzeni için ara yazılımı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ccac8-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="ccac8-122">Ara yazılım tarafından etkinleştirilen bir `IMiddlewareFactory` uygulaması (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="ccac8-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="ccac8-123">Uzantı ara yazılımı için oluşturulan (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ccac8-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="ccac8-124">`Startup.ConfigureServices`çeşitli görevleri gerçekleştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="ccac8-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="ccac8-125">Basit Injector kapsayıcısı ayarlama ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ccac8-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="ccac8-126">Fabrika ve ara yazılımın kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ccac8-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="ccac8-127">Uygulamanın veritabanı bağlamı basit Injector kapsayıcı için bir Razor sayfası kullanımına.</span><span class="sxs-lookup"><span data-stu-id="ccac8-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="ccac8-128">Ara yazılım istek işleme ardışık düzeninde kayıtlı `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ccac8-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a><span data-ttu-id="ccac8-129">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ccac8-129">Additional resources</span></span>

* [<span data-ttu-id="ccac8-130">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="ccac8-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ccac8-131">Ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ccac8-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="ccac8-132">Basit Injector GitHub deposunu</span><span class="sxs-lookup"><span data-stu-id="ccac8-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="ccac8-133">Basit Injector belgeleri</span><span class="sxs-lookup"><span data-stu-id="ccac8-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
