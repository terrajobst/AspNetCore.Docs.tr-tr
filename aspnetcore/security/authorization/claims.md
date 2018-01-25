---
title: "Talep tabanlı yetkilendirme"
author: rick-anderson
description: "Bu belge, yetkilendirme talep denetler ASP.NET Core uygulama eklemek açıklanmaktadır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 870bdf79abc2c94745ab5da6997a37ed0e4ea4e2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="claims-based-authorization"></a>Talep tabanlı yetkilendirme

<a name="security-authorization-claims-based"></a>

Kimlikteki oluşturulduğunda, güvenilen bir taraf tarafından verilen bir veya daha fazla talep atanabilir. Bir talep hangi konu temsil eden çiftidir ad değeri, değil hangi konu yapabilirsiniz. Örneğin, bir yerel yönlendirmeli lisans yetkilisi tarafından verilen bir sürücünün lisansına sahip olabilir. Sürücünüzün lisansı Doğum tarihiniz üzerinde yok. Bu durumda talep adı olacaktır `DateOfBirth`, talep değeri, Doğum tarihiniz örneğin olur `8th June 1970` ve veren yönlendirmeli lisans yetkilisi olacaktır. En basit şekliyle, talep tabanlı yetkilendirme, bir talebin değerini denetler ve bu değer temel bir kaynağa erişim izni verir. Yetkilendirme işlemi için gece kulübü erişmek isterseniz örnek olabilir için:

Kapı güvenlik yetkilisi Doğum talep ve bunlar veren (yönlendirmeli lisans yetkilisi), erişim vermeden önce güvenip tarih değeri değerlendirmek.

Kimlikteki birden çok değer ile birden fazla talep içerebilir ve aynı türde birden fazla talep içerebilir.

## <a name="adding-claims-checks"></a>Talep denetimleri ekleme

Talep tabanlı yetkilendirme denetimleri bildirim temelli - Geliştirici bunları kendi kodundaki bir denetleyici veya eylemin bir denetleyici içinde karşı geçerli kullanıcının sahip olması gerekir ve isteğe bağlı olarak değeri talep erişimi taşıması gerekir taleplerini belirtme eklemeler İstenen kaynak. Talepler, ilke tabanlı gereksinimleridir Geliştirici oluşturma ve talep gereksinimleri ifade bir ilkeyi kaydetmek gerekir.

En basit türünü bir talep varlığını ilke arar talep ve değeri denetlemez.

İlk yapı ve ilke kaydetmeniz gerekir. Bu bölümü normalde alan içinde yetkilendirme hizmet yapılandırmasının parçası olarak gerçekleşir `ConfigureServices()` içinde *haline* dosya.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

Bu durumda `EmployeeOnly` ilke varlığını denetler bir `EmployeeNumber` geçerli kimliğini talep.

Ardından İlkesi kullanarak uygulama `Policy` özellikte `AuthorizeAttribute` ilke adı; belirtmek için özniteliği

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Özniteliği bu örnekte bir tüm denetleyicisine denetleyicisinde herhangi bir eylem erişimin ilkeyle eşleşen kimlikler izin verilecek yalnızca uygulanabilir.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Tarafından korunan bir denetleyiciniz varsa `AuthorizeAttribute` özniteliği, ancak uyguladığınız belirli eylemlere anonim erişime izin vermek istediğiniz `AllowAnonymousAttribute` özniteliği.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

Çoğu talep değeri ile gelir. İlke oluştururken, izin verilen değerler listesini belirtebilirsiniz. Aşağıdaki örnek 1, 2, 3, 4 veya 5 çalışan sayıda olan çalışanlar için yalnızca başarılı.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a>Birden çok ilke değerlendirmesi

Denetleyici veya eylem için birden çok ilke uygularsanız, erişim verilmeden önce tüm ilkeler geçmesi gerekir. Örneğin:

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

Yukarıdaki örnekte karşılayan tüm kimlik `EmployeeOnly` İlkesi erişebilir `Payslip` eylemi olarak bu ilkeyi denetleyicisinde zorlanır. Ancak çağırmak için `UpdateSalary` gerekir kimliğini karşılamak eylem *her ikisi de* `EmployeeOnly` İlkesi ve `HumanResources` ilkesi.

Daha karmaşık ilkeleri isterseniz, Doğum talep tarihi alma gibi bir yaş dışarı hesaplama sonra geçerlilik süresi denetimi 21 ya da eski sonra yazmanız gereken [özel ilke işleyicileri](policies.md).
