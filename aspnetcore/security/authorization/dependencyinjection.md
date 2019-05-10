---
title: ASP.NET core'da gereksinim işleyicilerine bağımlılık ekleme
author: rick-anderson
description: Yetkilendirme gereksinim işleyicilerine bağımlılık ekleme kullanılarak ASP.NET Core uygulaması ekleme hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902507"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET core'da gereksinim işleyicilerine bağımlılık ekleme

<a name="security-authorization-di"></a>

[Yetkilendirme işleyicileri kaydedilmelidir](xref:security/authorization/policies#handler-registration) yapılandırma sırasında hizmet koleksiyondaki (kullanarak [bağımlılık ekleme](xref:fundamentals/dependency-injection)).

Bir yetkilendirme işleyicisi değerlendirilecek istiyordu kurallarının bir depo olan varsayalım ve bu depo hizmeti koleksiyonda kaydedildi. Yetkilendirme çözümleyin ve bu, oluşturucuya ekleme.

Örneğin, ASP kullanmak istiyorsanız. NET eklemesine isteyeceğiniz altyapı günlüğü `ILoggerFactory` işleyicinizi içine. Böyle bir işleyici aşağıdaki gibi görünmelidir:

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

İşleyici ile kaydetmek `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

İşleyici, uygulamanız başlatıldığında oluşturulması örneğini ve kayıtlı ekleme DI `ILoggerFactory` atladığından içine.

> [!NOTE]
> Entity Framework kullanan işleyicileri teklileri kayıtlı olması gerekir.
