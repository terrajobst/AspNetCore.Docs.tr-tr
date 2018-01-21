---
title: "Gereksinim işleyiciler bağımlılık ekleme"
author: rick-anderson
description: "Bu belge, bağımlılık ekleme kullanılarak ASP.NET Core uygulamaya yetkilendirme gereksinimi işleyicileri eklemesine nasıl özetlenmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 9aaa356fe67a7e2c8177ffa1b886ec6b3dc13ef0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="b5f3e-103">Gereksinim işleyiciler bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="b5f3e-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="b5f3e-104">[Yetkilendirme işleyicileri kaydedilmelidir](policies.md#handler-registration) yapılandırma sırasında hizmet koleksiyonundaki (kullanarak [bağımlılık ekleme](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="b5f3e-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="b5f3e-105">Yetkilendirme işleyicisinin içinden değerlendirmek istediğiniz kurallarının bir depo olan düşünün ve bu depoyu hizmet koleksiyonunda kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="b5f3e-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="b5f3e-106">Yetkilendirme çözün ve, oluşturucuya Ekle.</span><span class="sxs-lookup"><span data-stu-id="b5f3e-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="b5f3e-107">Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine.</span><span class="sxs-lookup"><span data-stu-id="b5f3e-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="b5f3e-108">Bu tür bir işleyici aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="b5f3e-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="b5f3e-109">İşleyici ile kaydetmeniz `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="b5f3e-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="b5f3e-110">İşleyici örneği uygulamanız başladığında oluşturulması ve kayıtlı Ekle dı `ILoggerFactory` , oluşturucusu içine.</span><span class="sxs-lookup"><span data-stu-id="b5f3e-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="b5f3e-111">Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b5f3e-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
