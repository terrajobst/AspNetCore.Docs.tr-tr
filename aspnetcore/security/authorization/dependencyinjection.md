---
title: ASP.NET core'da gereksinim işleyicilerine bağımlılık ekleme
author: rick-anderson
description: Yetkilendirme gereksinim işleyicilerine bağımlılık ekleme kullanılarak ASP.NET Core uygulaması ekleme hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342120"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="e8c12-103">ASP.NET core'da gereksinim işleyicilerine bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e8c12-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="e8c12-104">[Yetkilendirme işleyicileri kaydedilmelidir](xref:security/authorization/policies#handler-registration) yapılandırma sırasında hizmet koleksiyondaki (kullanarak [bağımlılık ekleme](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="e8c12-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="e8c12-105">Bir yetkilendirme işleyicisi değerlendirilecek istiyordu kurallarının bir depo olan varsayalım ve bu depo hizmeti koleksiyonda kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="e8c12-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="e8c12-106">Yetkilendirme çözümleyin ve bu, oluşturucuya ekleme.</span><span class="sxs-lookup"><span data-stu-id="e8c12-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="e8c12-107">Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine.</span><span class="sxs-lookup"><span data-stu-id="e8c12-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="e8c12-108">Böyle bir işleyici aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="e8c12-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="e8c12-109">İşleyici ile kaydetmek `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="e8c12-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="e8c12-110">İşleyici, uygulamanız başlatıldığında oluşturulması örneğini ve kayıtlı ekleme DI `ILoggerFactory` atladığından içine.</span><span class="sxs-lookup"><span data-stu-id="e8c12-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="e8c12-111">Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e8c12-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
