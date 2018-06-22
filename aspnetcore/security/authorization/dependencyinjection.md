---
title: ASP.NET Core uygulamasında gereksinim işleyicileri bağımlılık ekleme
author: rick-anderson
description: Bağımlılık ekleme kullanılarak ASP.NET Core uygulamaya yetkilendirme gereksinimi işleyicileri ekleme hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273728"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="ae89e-103">ASP.NET Core uygulamasında gereksinim işleyicileri bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="ae89e-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="ae89e-104">[Yetkilendirme işleyicileri kaydedilmelidir](xref:security/authorization/policies#handler-registration) yapılandırma sırasında hizmet koleksiyonundaki (kullanarak [bağımlılık ekleme](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="ae89e-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="ae89e-105">Yetkilendirme işleyicisinin içinden değerlendirmek istediğiniz kurallarının bir depo olan düşünün ve bu depoyu hizmet koleksiyonunda kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="ae89e-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="ae89e-106">Yetkilendirme çözün ve, oluşturucuya Ekle.</span><span class="sxs-lookup"><span data-stu-id="ae89e-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="ae89e-107">Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine.</span><span class="sxs-lookup"><span data-stu-id="ae89e-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="ae89e-108">Bu tür bir işleyici aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="ae89e-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="ae89e-109">İşleyici ile kaydetmeniz `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="ae89e-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="ae89e-110">İşleyici örneği uygulamanız başladığında oluşturulması ve kayıtlı Ekle dı `ILoggerFactory` , oluşturucusu içine.</span><span class="sxs-lookup"><span data-stu-id="ae89e-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="ae89e-111">Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae89e-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
