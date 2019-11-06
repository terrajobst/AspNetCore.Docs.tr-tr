---
title: ASP.NET Core rol tabanlı yetkilendirme
author: rick-anderson
description: Rolleri Yetkilendir özniteliğine geçirerek ASP.NET Core denetleyicisi ve eylem erişimini nasıl kısıtlayacağınızı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 28aa3df6aa661d0b762df78fe611cd827af43f75
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73660049"
---
# <a name="role-based-authorization-in-aspnet-core"></a>ASP.NET Core rol tabanlı yetkilendirme

<a name="security-authorization-role-based"></a>

Bir kimlik oluşturulduğunda, bir veya daha fazla role ait olabilir. Örneğin, Tracy yönetici 'ye ait olabilir ve IHOST Scott yalnızca kullanıcı rolüne ait olabilir. Bu rollerin oluşturulması ve yönetilmesi, yetkilendirme işleminin yedekleme deposuna bağlıdır. Roller, [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) sınıfındaki [ıınrole](/dotnet/api/system.security.principal.genericprincipal.isinrole) yöntemi aracılığıyla geliştiriciye sunulur.

## <a name="adding-role-checks"></a>Rol denetimleri ekleme

Rol tabanlı yetkilendirme denetimleri bildirime dayalı&mdash;geliştirici, bu dosyaları bir denetleyiciye veya denetleyici içindeki bir eyleme karşı, geçerli kullanıcının istenen kaynağa erişmek için üyesi olması gereken rolleri belirterek kod içinde katıştırır.

Örneğin, aşağıdaki kod, `Administrator` rolünün bir üyesi olan kullanıcılar için `AdministrationController` tüm eylemlere erişimi kısıtlar:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Birden çok rolü, virgülle ayrılmış bir liste olarak belirtebilirsiniz:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Bu denetleyiciye yalnızca `HRManager` rolü veya `Finance` rolü üyesi olan kullanıcılar erişebilir.

Birden çok öznitelik uygularsanız, bir erişen kullanıcının belirtilen tüm rollerin üyesi olması gerekir; Aşağıdaki örnek, bir kullanıcının `PowerUser` ve `ControlPanelUser` rolünün bir üyesi olması gerekir.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

İşlem düzeyinde ek rol yetkilendirme öznitelikleri uygulayarak erişimi daha da sınırlayabilirsiniz:

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

`Administrator` rolün önceki kod parçacığı üyeleri veya `PowerUser` rolü denetleyiciye ve `SetTime` eylemine erişebilir, ancak yalnızca `Administrator` rolünün üyeleri `ShutDown` eylemine erişebilir.

Ayrıca, bir denetleyiciyi kilitleyebilir, ancak tek tek eylemlere anonim, kimliği doğrulanmamış erişime izin verebilirsiniz.

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

Razor Pages için `AuthorizeAttribute` şu şekilde uygulanabilir:

* Bir [kural](xref:razor-pages/razor-pages-conventions#page-model-action-conventions)kullanma veya
* `AuthorizeAttribute` `PageModel` örneğine uygulanıyor:

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
> `AuthorizeAttribute`dahil filtre öznitelikleri yalnızca PageModel 'e uygulanabilir ve belirli sayfa işleyicisi yöntemlerine uygulanamaz.
::: moniker-end

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>İlke tabanlı rol denetimleri

Rol gereksinimleri, bir geliştiricinin bir ilkeyi yetkilendirme hizmeti yapılandırmasının bir parçası olarak bir ilke kaydettiğinde yeni Ilke sözdizimi kullanılarak da ifade edilebilir. Bu, normal olarak *Startup.cs* dosyanızdaki `ConfigureServices()` oluşur.

::: moniker range=">= aspnetcore-3.0"
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddRazorPages();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```
::: moniker-end

::: moniker range="< aspnetcore-3.0"
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
::: moniker-end

İlkeler, `AuthorizeAttribute` özniteliğinde `Policy` özelliği kullanılarak uygulanır:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Bir gereksinimde birden fazla izin verilen rol belirtmek istiyorsanız, bunları `RequireRole` yöntemine parametre olarak belirtebilirsiniz:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

Bu örnek, `Administrator`, `PowerUser` veya `BackupAdministrator` rollerine ait olan kullanıcıları yetkilendirir.

### <a name="add-role-services-to-identity"></a>Kimliğe rol hizmetleri Ekle

Rol hizmetleri eklemek için [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) ekleyin:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](roles/samples/3_0/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

::: moniker range="< aspnetcore-3.0"
[!code-csharp[](roles/samples/2_2/Startup.cs?name=snippet&highlight=7)]
::: moniker-end

