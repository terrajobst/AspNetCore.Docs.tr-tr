---
title: ASP.NET core'da rol tabanlı yetkilendirme
author: rick-anderson
description: Authorize özniteliği için rolleri geçirerek ASP.NET Core denetleyici ve eylem erişimi kısıtlama hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 0467ea82831bffe6882e584930c2fa1212a244c7
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248101"
---
# <a name="role-based-authorization-in-aspnet-core"></a>ASP.NET core'da rol tabanlı yetkilendirme

<a name="security-authorization-role-based"></a>

Bir kimlik oluşturulduğunda bir veya daha fazla role ait olabilir. Örneğin, Scott yalnızca kullanıcı rolünde yer artırabileceksiniz Tracy yönetici ve kullanıcı rollerine ait olabilir. Bu roller nasıl oluşturulduğunu ve yönetilen yedekleme deposu Yetkilendirme işlemi bağlıdır. Rolleri geliştiricilere sunulur [IPrincipal](/dotnet/api/system.security.principal.genericprincipal.isinrole) metodunda [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) sınıfı.

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> Bu konu daha önceden **değil** Razor sayfaları için geçerlidir. Razor sayfaları destekler [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) ve [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter). Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).

::: moniker-end

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