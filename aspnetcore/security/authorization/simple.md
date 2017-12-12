---
title: Basit yetkilendirme
author: rick-anderson
description: "Bu belgede Authorize öznitelik ASP.NET Core denetleyicileri ve eylemleri için erişimi kısıtlamak için nasıl kullanılacağı açıklanmaktadır."
keywords: ASP.NET Core, yetkilendirme, AuthorizeAttribute
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f2dad58ffa17259412077d31f512b561e79ac595
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="e8590-104">Basit yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="e8590-104">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="e8590-105">MVC yetkilendirme aracılığıyla denetlenir `AuthorizeAttribute` özniteliği ve çeşitli parametreleri.</span><span class="sxs-lookup"><span data-stu-id="e8590-105">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="e8590-106">En basit şekliyle, uygulama `AuthorizeAttribute` denetleyici veya eylem erişimi denetleyiciye veya eylem kimliği doğrulanmış bir kullanıcıya özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e8590-106">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="e8590-107">Örneğin, aşağıdaki kod erişimi sınırlar `AccountController` tüm kimliği doğrulanmış kullanıcılar için.</span><span class="sxs-lookup"><span data-stu-id="e8590-107">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="e8590-108">Yetkilendirme denetleyicisi yerine bir eylemi uygulamak istiyorsanız, uygulama `AuthorizeAttribute` özniteliği eylem için:</span><span class="sxs-lookup"><span data-stu-id="e8590-108">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="e8590-109">Yalnızca kimliği doğrulanmış kullanıcılar erişebilir artık `Logout` işlevi.</span><span class="sxs-lookup"><span data-stu-id="e8590-109">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="e8590-110">Aynı zamanda `AllowAnonymousAttribute` doğrulanmamış kullanıcılar için bireysel işlemlere tarafından erişime izin verecek şekilde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e8590-110">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="e8590-111">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e8590-111">For example:</span></span>

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

<span data-ttu-id="e8590-112">Bu, yalnızca kimliği doğrulanmış kullanıcılara olanak tanır `AccountController`, dışında `Login` kimliği doğrulanmış veya doğrulanmamış / anonim durumlarını bakılmaksızın herkes tarafından erişilebilir olan eylem.</span><span class="sxs-lookup"><span data-stu-id="e8590-112">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="e8590-113">`[AllowAnonymous]`Tüm Yetkilendirme deyimleri atlar.</span><span class="sxs-lookup"><span data-stu-id="e8590-113">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="e8590-114">Birleştirme uygularsanız `[AllowAnonymous]` ve tüm `[Authorize]` özniteliği sonra Authorize öznitelikleri her zaman yok sayılacak.</span><span class="sxs-lookup"><span data-stu-id="e8590-114">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="e8590-115">Örneğin, uygulama `[AllowAnonymous]` denetleyicisinde herhangi düzey `[Authorize]` öznitelikleri aynı denetleyicisi veya içindeki herhangi bir işlem yok sayılacak.</span><span class="sxs-lookup"><span data-stu-id="e8590-115">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
