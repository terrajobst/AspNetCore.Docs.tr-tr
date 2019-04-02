---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809409"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a><span data-ttu-id="b3cd4-101">ASP.NET Core ara yazılım genişletilebilirlik örneği</span><span class="sxs-lookup"><span data-stu-id="b3cd4-101">ASP.NET Core Middleware Extensibility Sample</span></span>

<span data-ttu-id="b3cd4-102">Bu örnek, açıklanan senaryoları gösterir [ASP.NET Core Fabrika tabanlı ara yazılım etkinleştirme](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span><span class="sxs-lookup"><span data-stu-id="b3cd4-102">This sample demonstrates the scenarios described in [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span></span>

<span data-ttu-id="b3cd4-103">Örnek uygulama tarafından etkinleştirilen bir ara yazılım gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3cd4-103">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="b3cd4-104">Kuralı.</span><span class="sxs-lookup"><span data-stu-id="b3cd4-104">Convention.</span></span> <span data-ttu-id="b3cd4-105">Geleneksel bir ara yazılım etkinleştirme hakkında daha fazla bilgi için bkz. [ara yazılım](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) konu.</span><span class="sxs-lookup"><span data-stu-id="b3cd4-105">For more information on conventional middleware activation, see the [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) topic.</span></span>
* <span data-ttu-id="b3cd4-106">Bir [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="b3cd4-106">An [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="b3cd4-107">Varsayılan [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) ara yazılım sınıfı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="b3cd4-107">The default [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) class activates the middleware.</span></span>

<span data-ttu-id="b3cd4-108">Ara yazılım uygulamaları aynı şekilde çalışır ve bir sorgu dizesi parametresi tarafından sağlanan değerini kaydedin (`key`).</span><span class="sxs-lookup"><span data-stu-id="b3cd4-108">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="b3cd4-109">Middlewares, sorgu dizesi değerini bir bellek içi veritabanına kaydetmek için bir eklenen veritabanı bağlamı (kapsamlı bir hizmet) kullanın.</span><span class="sxs-lookup"><span data-stu-id="b3cd4-109">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>
