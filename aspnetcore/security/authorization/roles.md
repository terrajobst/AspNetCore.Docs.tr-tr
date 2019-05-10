---
title: ASP.NET core'da rol tabanlı yetkilendirme
author: rick-anderson
description: Authorize özniteliği için rolleri geçirerek ASP.NET Core denetleyici ve eylem erişimi kısıtlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0e01e1976e2721ca64720a67c6341661f646395c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900335"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="1a826-103">ASP.NET core'da rol tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="1a826-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="1a826-104">Bir kimlik oluşturulduğunda bir veya daha fazla role ait olabilir.</span><span class="sxs-lookup"><span data-stu-id="1a826-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="1a826-105">Örneğin, Scott yalnızca kullanıcı rolünde yer artırabileceksiniz Tracy yönetici ve kullanıcı rollerine ait olabilir.</span><span class="sxs-lookup"><span data-stu-id="1a826-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="1a826-106">Bu roller nasıl oluşturulduğunu ve yönetilen yedekleme deposu Yetkilendirme işlemi bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1a826-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="1a826-107">Rolleri geliştiricilere sunulur [IPrincipal](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodunda [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1a826-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="1a826-108">Rol denetimleri ekleme</span><span class="sxs-lookup"><span data-stu-id="1a826-108">Adding role checks</span></span>

<span data-ttu-id="1a826-109">Rol tabanlı yetkilendirme denetimleri, bildirim temelli&mdash;Geliştirici bunları kendi kodundaki bir denetleyici veya eylem bir denetleyici içinde karşı geçerli kullanıcının istenen kaynağa erişim için bir üyesi olması gereken roller belirtme katıştırır.</span><span class="sxs-lookup"><span data-stu-id="1a826-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="1a826-110">Örneğin, aşağıdaki kod üzerinde herhangi bir eylem erişimi sınırlar `AdministrationController` kullanıcılara üyesi olan `Administrator` rolü:</span><span class="sxs-lookup"><span data-stu-id="1a826-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="1a826-111">Birden çok rol virgülle ayrılmış bir liste belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1a826-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="1a826-112">Bu denetleyici yalnızca üyeleri olan kullanıcılar tarafından erişilebilir olur, `HRManager` rol veya `Finance` rol.</span><span class="sxs-lookup"><span data-stu-id="1a826-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="1a826-113">Birden çok öznitelik uygularsanız erişen bir kullanıcı belirtilen tüm rollerin bir üyesi olması gerekir; Aşağıdaki örnek bir kullanıcı hem de bir üyesi olmasını gerektirir `PowerUser` ve `ControlPanelUser` rol.</span><span class="sxs-lookup"><span data-stu-id="1a826-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="1a826-114">Daha fazla eylem düzeyinde ek rol yetkilendirme öznitelikleri uygulayarak erişimi kısıtlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1a826-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="1a826-115">Önceki kod parçacığı üyelerinde `Administrator` rol veya `PowerUser` denetleyici rolü erişebilir ve `SetTime` eylem, ancak yalnızca üyelerinin `Administrator` rol erişebilir `ShutDown` eylem.</span><span class="sxs-lookup"><span data-stu-id="1a826-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="1a826-116">Bir denetleyiciyi kilitleme ancak ayrı Eylemler anonim, kimliği doğrulanmamış erişime izin vermek.</span><span class="sxs-lookup"><span data-stu-id="1a826-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1a826-117">Razor sayfaları için `AuthorizeAttribute` ya da uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="1a826-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="1a826-118">Kullanarak bir [kuralı](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), veya</span><span class="sxs-lookup"><span data-stu-id="1a826-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="1a826-119">Uygulama `AuthorizeAttribute` için `PageModel` örneği:</span><span class="sxs-lookup"><span data-stu-id="1a826-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="1a826-120">Özniteliklerini dahil olmak üzere, filtre `AuthorizeAttribute`Pagemodel'a yalnızca uygulanabilir ve belirli bir sayfaya işleyici yöntemleri için uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="1a826-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="1a826-121">İlke tabanlı rol denetimleri</span><span class="sxs-lookup"><span data-stu-id="1a826-121">Policy based role checks</span></span>

<span data-ttu-id="1a826-122">Rol gereksinimlerini de Geliştirici başlatma sırasında bir ilke yetkilendirme hizmet yapılandırmasının bir parçası olarak kaydettiği yeni ilke söz dizimi kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="1a826-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="1a826-123">Bu normalde oluşuyor `ConfigureServices()` içinde *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1a826-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="1a826-124">İlkeleri kullanarak uygulanır `Policy` özelliği `AuthorizeAttribute` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="1a826-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="1a826-125">Birden çok izin verilen rol için bir gereksinim olarak belirtmek istediğiniz sonra parametreleri olarak belirtebilirsiniz `RequireRole` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1a826-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="1a826-126">Bu örnekte ait kullanıcılar yetkilendirir `Administrator`, `PowerUser` veya `BackupAdministrator` rolleri.</span><span class="sxs-lookup"><span data-stu-id="1a826-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="1a826-127">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="1a826-127">Add Role services to Identity</span></span>

<span data-ttu-id="1a826-128">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="1a826-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
