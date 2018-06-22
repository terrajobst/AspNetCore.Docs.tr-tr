---
title: ASP.NET Core basit yetkilendirme
author: rick-anderson
description: Authorize öznitelik ASP.NET Core denetleyicileri ve eylemleri için erişimi kısıtlamak için nasıl kullanılacağını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272071"
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

>[!WARNING]
> `[AllowAnonymous]` Tüm Yetkilendirme deyimleri atlar. Birleştirme uygularsanız `[AllowAnonymous]` ve tüm `[Authorize]` özniteliği sonra Authorize öznitelikleri her zaman yok sayılacak. Örneğin, uygulama `[AllowAnonymous]` denetleyicisinde herhangi düzey `[Authorize]` öznitelikleri aynı denetleyicisi veya içindeki herhangi bir işlem yok sayılacak.
