---
title: ASP.NET core'da rol tabanlı yetkilendirme
author: rick-anderson
description: Authorize özniteliği için rolleri geçirerek ASP.NET Core denetleyici ve eylem erişimi kısıtlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356681"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="7bcef-103">ASP.NET core'da rol tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="7bcef-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="7bcef-104">Bir kimlik oluşturulduğunda bir veya daha fazla role ait olabilir.</span><span class="sxs-lookup"><span data-stu-id="7bcef-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="7bcef-105">Örneğin, Scott yalnızca kullanıcı rolünde yer artırabileceksiniz Tracy yönetici ve kullanıcı rollerine ait olabilir.</span><span class="sxs-lookup"><span data-stu-id="7bcef-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="7bcef-106">Bu roller nasıl oluşturulduğunu ve yönetilen yedekleme deposu Yetkilendirme işlemi bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="7bcef-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="7bcef-107">Rolleri geliştiricilere sunulur [IPrincipal](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodunda [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7bcef-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> <span data-ttu-id="7bcef-108">Bu konu daha önceden **değil** Razor sayfaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="7bcef-108">This topic does **not** apply to Razor Pages.</span></span> <span data-ttu-id="7bcef-109">Razor sayfaları destekler [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) ve [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span><span class="sxs-lookup"><span data-stu-id="7bcef-109">Razor Pages supports [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter).</span></span> <span data-ttu-id="7bcef-110">Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="7bcef-110">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

::: moniker-end

## <a name="adding-role-checks"></a><span data-ttu-id="7bcef-111">Rol denetimleri ekleme</span><span class="sxs-lookup"><span data-stu-id="7bcef-111">Adding role checks</span></span>

<span data-ttu-id="7bcef-112">Rol tabanlı yetkilendirme denetimleri, bildirim temelli&mdash;Geliştirici bunları kendi kodundaki bir denetleyici veya eylem bir denetleyici içinde karşı geçerli kullanıcının istenen kaynağa erişim için bir üyesi olması gereken roller belirtme katıştırır.</span><span class="sxs-lookup"><span data-stu-id="7bcef-112">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="7bcef-113">Örneğin, aşağıdaki kod üzerinde herhangi bir eylem erişimi sınırlar `AdministrationController` kullanıcılara üyesi olan `Administrator` rolü:</span><span class="sxs-lookup"><span data-stu-id="7bcef-113">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="7bcef-114">Birden çok rol virgülle ayrılmış bir liste belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7bcef-114">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="7bcef-115">Bu denetleyici yalnızca üyeleri olan kullanıcılar tarafından erişilebilir olur, `HRManager` rol veya `Finance` rol.</span><span class="sxs-lookup"><span data-stu-id="7bcef-115">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="7bcef-116">Birden çok öznitelik uygularsanız erişen bir kullanıcı belirtilen tüm rollerin bir üyesi olması gerekir; Aşağıdaki örnek bir kullanıcı hem de bir üyesi olmasını gerektirir `PowerUser` ve `ControlPanelUser` rol.</span><span class="sxs-lookup"><span data-stu-id="7bcef-116">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="7bcef-117">Daha fazla eylem düzeyinde ek rol yetkilendirme öznitelikleri uygulayarak erişimi kısıtlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7bcef-117">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="7bcef-118">Önceki kod parçacığı üyelerinde `Administrator` rol veya `PowerUser` denetleyici rolü erişebilir ve `SetTime` eylem, ancak yalnızca üyelerinin `Administrator` rol erişebilir `ShutDown` eylem.</span><span class="sxs-lookup"><span data-stu-id="7bcef-118">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="7bcef-119">Bir denetleyiciyi kilitleme ancak ayrı Eylemler anonim, kimliği doğrulanmamış erişime izin vermek.</span><span class="sxs-lookup"><span data-stu-id="7bcef-119">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="7bcef-120">İlke tabanlı rol denetimleri</span><span class="sxs-lookup"><span data-stu-id="7bcef-120">Policy based role checks</span></span>

<span data-ttu-id="7bcef-121">Rol gereksinimlerini de Geliştirici başlatma sırasında bir ilke yetkilendirme hizmet yapılandırmasının bir parçası olarak kaydettiği yeni ilke söz dizimi kullanılarak belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="7bcef-121">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="7bcef-122">Bu normalde oluşuyor `ConfigureServices()` içinde *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="7bcef-122">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="7bcef-123">İlkeleri kullanarak uygulanır `Policy` özelliği `AuthorizeAttribute` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="7bcef-123">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="7bcef-124">Birden çok izin verilen rol için bir gereksinim olarak belirtmek istediğiniz sonra parametreleri olarak belirtebilirsiniz `RequireRole` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7bcef-124">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="7bcef-125">Bu örnekte ait kullanıcılar yetkilendirir `Administrator`, `PowerUser` veya `BackupAdministrator` rolleri.</span><span class="sxs-lookup"><span data-stu-id="7bcef-125">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
