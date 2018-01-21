---
title: Basit yetkilendirme
author: rick-anderson
description: "Bu belgede Authorize öznitelik ASP.NET Core denetleyicileri ve eylemleri için erişimi kısıtlamak için nasıl kullanılacağı açıklanmaktadır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f1d5671785da815f2f4fcf5bef1352f4c9e62877
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="1c6ee-103">Basit yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="1c6ee-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="1c6ee-104">MVC yetkilendirme aracılığıyla denetlenir `AuthorizeAttribute` özniteliği ve çeşitli parametreleri.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="1c6ee-105">En basit şekliyle, uygulama `AuthorizeAttribute` denetleyici veya eylem erişimi denetleyiciye veya eylem kimliği doğrulanmış bir kullanıcıya özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="1c6ee-106">Örneğin, aşağıdaki kod erişimi sınırlar `AccountController` tüm kimliği doğrulanmış kullanıcılar için.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="1c6ee-107">Yetkilendirme denetleyicisi yerine bir eylemi uygulamak istiyorsanız, uygulama `AuthorizeAttribute` özniteliği eylem için:</span><span class="sxs-lookup"><span data-stu-id="1c6ee-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="1c6ee-108">Yalnızca kimliği doğrulanmış kullanıcılar erişebilir artık `Logout` işlevi.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="1c6ee-109">Aynı zamanda `AllowAnonymousAttribute` doğrulanmamış kullanıcılar için bireysel işlemlere tarafından erişime izin verecek şekilde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="1c6ee-110">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="1c6ee-110">For example:</span></span>

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

<span data-ttu-id="1c6ee-111">Bu, yalnızca kimliği doğrulanmış kullanıcılara olanak tanır `AccountController`, dışında `Login` kimliği doğrulanmış veya doğrulanmamış / anonim durumlarını bakılmaksızın herkes tarafından erişilebilir olan eylem.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="1c6ee-112">`[AllowAnonymous]`Tüm Yetkilendirme deyimleri atlar.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="1c6ee-113">Birleştirme uygularsanız `[AllowAnonymous]` ve tüm `[Authorize]` özniteliği sonra Authorize öznitelikleri her zaman yok sayılacak.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="1c6ee-114">Örneğin, uygulama `[AllowAnonymous]` denetleyicisinde herhangi düzey `[Authorize]` öznitelikleri aynı denetleyicisi veya içindeki herhangi bir işlem yok sayılacak.</span><span class="sxs-lookup"><span data-stu-id="1c6ee-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
