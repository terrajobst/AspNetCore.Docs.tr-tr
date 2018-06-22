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
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core uygulamasında gereksinim işleyicileri bağımlılık ekleme

<a name="security-authorization-di"></a>

[Yetkilendirme işleyicileri kaydedilmelidir](xref:security/authorization/policies#handler-registration) yapılandırma sırasında hizmet koleksiyonundaki (kullanarak [bağımlılık ekleme](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

Yetkilendirme işleyicisinin içinden değerlendirmek istediğiniz kurallarının bir depo olan düşünün ve bu depoyu hizmet koleksiyonunda kayıtlı. Yetkilendirme çözün ve, oluşturucuya Ekle.

Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine. Bu tür bir işleyici aşağıdaki gibi görünmelidir:

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

İşleyici ile kaydetmeniz `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

İşleyici örneği uygulamanız başladığında oluşturulması ve kayıtlı Ekle dı `ILoggerFactory` , oluşturucusu içine.

> [!NOTE]
> Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.
