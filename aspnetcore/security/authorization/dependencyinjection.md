---
title: "Gereksinim işleyiciler bağımlılık ekleme"
author: rick-anderson
description: "Bu belge, bağımlılık ekleme kullanılarak ASP.NET Core uygulamaya yetkilendirme gereksinimi işleyicileri eklemesine nasıl özetlenmektedir."
keywords: "ASP.NET Core, bağımlılık ekleme, yetkilendirme işleyicisi"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="95ac7-104">Gereksinim işleyiciler bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="95ac7-104">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="95ac7-105">[Yetkilendirme işleyicileri kaydedilmelidir](policies.md#handler-registration) yapılandırma sırasında hizmet koleksiyonundaki (kullanarak [bağımlılık ekleme](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="95ac7-105">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="95ac7-106">Yetkilendirme işleyicisinin içinden değerlendirmek istediğiniz kurallarının bir depo olan düşünün ve bu depoyu hizmet koleksiyonunda kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="95ac7-106">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="95ac7-107">Yetkilendirme çözün ve, oluşturucuya Ekle.</span><span class="sxs-lookup"><span data-stu-id="95ac7-107">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="95ac7-108">Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine.</span><span class="sxs-lookup"><span data-stu-id="95ac7-108">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="95ac7-109">Bu tür bir işleyici aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="95ac7-109">Such a handler might look like:</span></span>

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

<span data-ttu-id="95ac7-110">İşleyici ile kaydetmeniz `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="95ac7-110">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="95ac7-111">İşleyici örneği uygulamanız başladığında oluşturulması ve kayıtlı Ekle dı `ILoggerFactory` , oluşturucusu içine.</span><span class="sxs-lookup"><span data-stu-id="95ac7-111">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="95ac7-112">Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="95ac7-112">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
