---
title: ASP.NET Core basit yetkilendirme
author: rick-anderson
description: Authorize öznitelik ASP.NET Core denetleyicileri ve eylemleri için erişimi kısıtlamak için nasıl kullanılacağını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961129"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core basit yetkilendirme

<a name="security-authorization-simple"></a>

MVC yetkilendirme aracılığıyla denetlenir `AuthorizeAttribute` özniteliği ve çeşitli parametreleri. En basit şekliyle, uygulama `AuthorizeAttribute` denetleyici veya eylem erişimi denetleyiciye veya eylem kimliği doğrulanmış bir kullanıcıya özniteliği.

Örneğin, aşağıdaki kod erişimi sınırlar `AccountController` tüm kimliği doğrulanmış kullanıcılar için.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Yetkilendirme denetleyicisi yerine bir eylemi uygulamak istiyorsanız, uygulama `AuthorizeAttribute` özniteliği eylem için:

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Yalnızca kimliği doğrulanmış kullanıcılar erişebilir artık `Logout` işlevi.

Aynı zamanda `AllowAnonymous` doğrulanmamış kullanıcılar için bireysel işlemlere tarafından erişime izin verecek şekilde özniteliği. Örneğin:

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Bu, yalnızca kimliği doğrulanmış kullanıcılara olanak tanır `AccountController`, dışında `Login` kimliği doğrulanmış veya doğrulanmamış / anonim durumlarını bakılmaksızın herkes tarafından erişilebilir olan eylem.

> [!WARNING]
> `[AllowAnonymous]` Tüm Yetkilendirme deyimleri atlar. Birleştiriyorsanız `[AllowAnonymous]` ve tüm `[Authorize]` özniteliği `[Authorize]` öznitelikleri yok sayılır. Örneğin, uygulama `[AllowAnonymous]` denetleyici düzeyinde herhangi `[Authorize]` öznitelikleri aynı denetleyicisine (veya içindeki herhangi bir işlem) göz ardı edilir.
