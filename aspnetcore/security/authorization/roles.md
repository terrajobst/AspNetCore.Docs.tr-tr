---
title: ASP.NET core'da rol tabanlı yetkilendirme
author: rick-anderson
description: Authorize özniteliği için rolleri geçirerek ASP.NET Core denetleyici ve eylem erişimi kısıtlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0e01e1976e2721ca64720a67c6341661f646395c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209102"
---
# <a name="role-based-authorization-in-aspnet-core"></a>ASP.NET core'da rol tabanlı yetkilendirme

<a name="security-authorization-role-based"></a>

Bir kimlik oluşturulduğunda bir veya daha fazla role ait olabilir. Örneğin, Scott yalnızca kullanıcı rolünde yer artırabileceksiniz Tracy yönetici ve kullanıcı rollerine ait olabilir. Bu roller nasıl oluşturulduğunu ve yönetilen yedekleme deposu Yetkilendirme işlemi bağlıdır. Rolleri geliştiricilere sunulur [IPrincipal](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodunda [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) sınıfı.

## <a name="adding-role-checks"></a>Rol denetimleri ekleme

Rol tabanlı yetkilendirme denetimleri, bildirim temelli&mdash;Geliştirici bunları kendi kodundaki bir denetleyici veya eylem bir denetleyici içinde karşı geçerli kullanıcının istenen kaynağa erişim için bir üyesi olması gereken roller belirtme katıştırır.

Örneğin, aşağıdaki kod üzerinde herhangi bir eylem erişimi sınırlar `AdministrationController` kullanıcılara üyesi olan `Administrator` rolü:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Birden çok rol virgülle ayrılmış bir liste belirtebilirsiniz:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Bu denetleyici yalnızca üyeleri olan kullanıcılar tarafından erişilebilir olur, `HRManager` rol veya `Finance` rol.

Birden çok öznitelik uygularsanız erişen bir kullanıcı belirtilen tüm rollerin bir üyesi olması gerekir; Aşağıdaki örnek bir kullanıcı hem de bir üyesi olmasını gerektirir `PowerUser` ve `ControlPanelUser` rol.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Daha fazla eylem düzeyinde ek rol yetkilendirme öznitelikleri uygulayarak erişimi kısıtlayabilirsiniz:

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

Önceki kod parçacığı üyelerinde `Administrator` rol veya `PowerUser` denetleyici rolü erişebilir ve `SetTime` eylem, ancak yalnızca üyelerinin `Administrator` rol erişebilir `ShutDown` eylem.

Bir denetleyiciyi kilitleme ancak ayrı Eylemler anonim, kimliği doğrulanmamış erişime izin vermek.

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

Razor sayfaları için `AuthorizeAttribute` ya da uygulanabilir:

* Kullanarak bir [kuralı](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), veya
* Uygulama `AuthorizeAttribute` için `PageModel` örneği:

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
> Özniteliklerini dahil olmak üzere, filtre `AuthorizeAttribute`Pagemodel'a yalnızca uygulanabilir ve belirli bir sayfaya işleyici yöntemleri için uygulanamaz.
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>İlke tabanlı rol denetimleri

Rol gereksinimlerini de Geliştirici başlatma sırasında bir ilke yetkilendirme hizmet yapılandırmasının bir parçası olarak kaydettiği yeni ilke söz dizimi kullanılarak belirtilebilir. Bu normalde oluşuyor `ConfigureServices()` içinde *Startup.cs* dosya.

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

İlkeleri kullanarak uygulanır `Policy` özelliği `AuthorizeAttribute` özniteliği:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Birden çok izin verilen rol için bir gereksinim olarak belirtmek istediğiniz sonra parametreleri olarak belirtebilirsiniz `RequireRole` yöntemi:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

Bu örnekte ait kullanıcılar yetkilendirir `Administrator`, `PowerUser` veya `BackupAdministrator` rolleri.

### <a name="add-role-services-to-identity"></a>Kimlik için rol hizmetleri Ekle

Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]
