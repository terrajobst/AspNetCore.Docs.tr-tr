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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="931a9-103">ASP.NET Core basit yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="931a9-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="931a9-104">MVC yetkilendirme aracılığıyla denetlenir `AuthorizeAttribute` özniteliği ve çeşitli parametreleri.</span><span class="sxs-lookup"><span data-stu-id="931a9-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="931a9-105">En basit şekliyle, uygulama `AuthorizeAttribute` denetleyici veya eylem erişimi denetleyiciye veya eylem kimliği doğrulanmış bir kullanıcıya özniteliği.</span><span class="sxs-lookup"><span data-stu-id="931a9-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="931a9-106">Örneğin, aşağıdaki kod erişimi sınırlar `AccountController` tüm kimliği doğrulanmış kullanıcılar için.</span><span class="sxs-lookup"><span data-stu-id="931a9-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="931a9-107">Yetkilendirme denetleyicisi yerine bir eylemi uygulamak istiyorsanız, uygulama `AuthorizeAttribute` özniteliği eylem için:</span><span class="sxs-lookup"><span data-stu-id="931a9-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="931a9-108">Yalnızca kimliği doğrulanmış kullanıcılar erişebilir artık `Logout` işlevi.</span><span class="sxs-lookup"><span data-stu-id="931a9-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="931a9-109">Aynı zamanda `AllowAnonymous` doğrulanmamış kullanıcılar için bireysel işlemlere tarafından erişime izin verecek şekilde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="931a9-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="931a9-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="931a9-110">For example:</span></span>

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

<span data-ttu-id="931a9-111">Bu, yalnızca kimliği doğrulanmış kullanıcılara olanak tanır `AccountController`, dışında `Login` kimliği doğrulanmış veya doğrulanmamış / anonim durumlarını bakılmaksızın herkes tarafından erişilebilir olan eylem.</span><span class="sxs-lookup"><span data-stu-id="931a9-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="931a9-112">`[AllowAnonymous]` Tüm Yetkilendirme deyimleri atlar.</span><span class="sxs-lookup"><span data-stu-id="931a9-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="931a9-113">Birleştiriyorsanız `[AllowAnonymous]` ve tüm `[Authorize]` özniteliği `[Authorize]` öznitelikleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="931a9-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="931a9-114">Örneğin, uygulama `[AllowAnonymous]` denetleyici düzeyinde herhangi `[Authorize]` öznitelikleri aynı denetleyicisine (veya içindeki herhangi bir işlem) göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="931a9-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
