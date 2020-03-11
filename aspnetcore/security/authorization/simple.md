---
title: ASP.NET Core basit yetkilendirme
author: rick-anderson
description: ASP.NET Core denetleyicilerine ve eylemlerine erişimi kısıtlamak için yetkilendir özniteliğini nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663585"
---
# <a name="simple-authorization-in-aspnet-core"></a>ASP.NET Core basit yetkilendirme

<a name="security-authorization-simple"></a>

MVC 'de yetkilendirme, `AuthorizeAttribute` özniteliği ve çeşitli parametreleri aracılığıyla denetlenir. En basit aşamasında, bir denetleyiciye veya eyleme `AuthorizeAttribute` özniteliğini uygulamak, kimliği doğrulanmış herhangi bir kullanıcıya denetleyiciye veya eyleme erişimi sınırlandırır.

Örneğin, aşağıdaki kod kimliği doğrulanmış herhangi bir kullanıcıyla `AccountController` erişimi kısıtlar.

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

Denetleyici yerine bir eyleme yetkilendirme uygulamak istiyorsanız, eyleme `AuthorizeAttribute` özniteliğini uygulayın:

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

Artık yalnızca kimliği doğrulanmış kullanıcılar `Logout` işleve erişebilir.

Kimliği doğrulanmamış kullanıcıların tek tek eylemlere erişimine izin vermek için `AllowAnonymous` özniteliğini de kullanabilirsiniz. Örnek:

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

Bu, kimliği doğrulanmış veya kimliği doğrulanmamış/anonim durumundan bağımsız olarak herkes tarafından erişilebilen `Login` eylemi hariç yalnızca kimliği doğrulanmış kullanıcıların `AccountController`izin verir.

> [!WARNING]
> `[AllowAnonymous]` tüm yetkilendirme deyimlerini atlar. `[AllowAnonymous]` ve `[Authorize]` özniteliğini birleştirirseniz `[Authorize]` öznitelikleri yok sayılır. Örneğin, denetleyici düzeyinde `[AllowAnonymous]` uygularsanız, aynı denetleyicideki (veya içindeki herhangi bir eylemde) `[Authorize]` öznitelikleri yok sayılır.
