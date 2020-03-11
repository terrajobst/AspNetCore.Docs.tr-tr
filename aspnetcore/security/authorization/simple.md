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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="b5de6-103">ASP.NET Core basit yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="b5de6-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="b5de6-104">MVC 'de yetkilendirme, `AuthorizeAttribute` özniteliği ve çeşitli parametreleri aracılığıyla denetlenir.</span><span class="sxs-lookup"><span data-stu-id="b5de6-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="b5de6-105">En basit aşamasında, bir denetleyiciye veya eyleme `AuthorizeAttribute` özniteliğini uygulamak, kimliği doğrulanmış herhangi bir kullanıcıya denetleyiciye veya eyleme erişimi sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="b5de6-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="b5de6-106">Örneğin, aşağıdaki kod kimliği doğrulanmış herhangi bir kullanıcıyla `AccountController` erişimi kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="b5de6-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="b5de6-107">Denetleyici yerine bir eyleme yetkilendirme uygulamak istiyorsanız, eyleme `AuthorizeAttribute` özniteliğini uygulayın:</span><span class="sxs-lookup"><span data-stu-id="b5de6-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="b5de6-108">Artık yalnızca kimliği doğrulanmış kullanıcılar `Logout` işleve erişebilir.</span><span class="sxs-lookup"><span data-stu-id="b5de6-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="b5de6-109">Kimliği doğrulanmamış kullanıcıların tek tek eylemlere erişimine izin vermek için `AllowAnonymous` özniteliğini de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b5de6-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="b5de6-110">Örnek:</span><span class="sxs-lookup"><span data-stu-id="b5de6-110">For example:</span></span>

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

<span data-ttu-id="b5de6-111">Bu, kimliği doğrulanmış veya kimliği doğrulanmamış/anonim durumundan bağımsız olarak herkes tarafından erişilebilen `Login` eylemi hariç yalnızca kimliği doğrulanmış kullanıcıların `AccountController`izin verir.</span><span class="sxs-lookup"><span data-stu-id="b5de6-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="b5de6-112">`[AllowAnonymous]` tüm yetkilendirme deyimlerini atlar.</span><span class="sxs-lookup"><span data-stu-id="b5de6-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="b5de6-113">`[AllowAnonymous]` ve `[Authorize]` özniteliğini birleştirirseniz `[Authorize]` öznitelikleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="b5de6-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="b5de6-114">Örneğin, denetleyici düzeyinde `[AllowAnonymous]` uygularsanız, aynı denetleyicideki (veya içindeki herhangi bir eylemde) `[Authorize]` öznitelikleri yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="b5de6-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
