---
title: ASP.NET Core 'de gereksinim işleyicilerde bağımlılık ekleme
author: rick-anderson
description: Bağımlılık ekleme kullanarak ASP.NET Core uygulamasına yetkilendirme gereksinimi işleyicilerini nasıl ekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666091"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>ASP.NET Core 'de gereksinim işleyicilerde bağımlılık ekleme

<a name="security-authorization-di"></a>

[Yetkilendirme işleyicilerinin](xref:security/authorization/policies#handler-registration) yapılandırma sırasında hizmet koleksiyonunda kayıtlı olması gerekir ( [bağımlılık ekleme](xref:fundamentals/dependency-injection)kullanılarak).

Bir yetkilendirme işleyicisinde değerlendirmek istediğiniz bir kural deposudur olduğunu ve bu deponun hizmet koleksiyonunda kayıtlı olduğunu varsayalım. Yetkilendirme çözülecek ve oluşturucuya eklenecektir.

Örneğin, ASP 'yi kullanmak isterseniz. İşleyicisine `ILoggerFactory` eklemek istediğiniz NET günlük altyapısı. Böyle bir işleyici şöyle görünebilir:

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

İşleyiciyi `services.AddSingleton()`kaydetmelisiniz:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Uygulamanız başlatıldığında işleyicinin bir örneği oluşturulur ve bu, kayıtlı `ILoggerFactory` oluşturucuya eklenir.

> [!NOTE]
> Entity Framework kullanan işleyiciler tekton olarak kaydedilmelidir.
