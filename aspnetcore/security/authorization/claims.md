---
title: ASP.NET core'da talep tabanlı yetkilendirme
author: rick-anderson
description: Yetkilendirme talep olup olmadığını denetler ASP.NET Core uygulaması eklemeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086149"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>ASP.NET core'da talep tabanlı yetkilendirme

<a name="security-authorization-claims-based"></a>

Bir kimlik oluşturulduğunda güvenilen bir taraf tarafından verilen bir veya daha fazla talep atanabilir. Bir talep hangi konu temsil eden bir ad değer çifti olan, olmayan hangi konu yapabilirsiniz. Örneğin, yerel bir sürüş lisans yetkilisi tarafından verilen bir sürücünün lisansı olabilir. Sürücünüzün lisansı Doğum tarihinize üzerinde yok. Talep adı bu durumda olacaktır `DateOfBirth`, talep değerini Doğum, örneğin olacaktır `8th June 1970` ve sürüş lisans yetki veren olur. En basit şekliyle, talep tabanlı yetkilendirme, bir talebin değerini denetler ve bu değeri alarak bir kaynağa erişim izni verir. Yetkilendirme işlemi için bir gece kulübü erişmek isterseniz örnek olabilir için:

Kapı güvenlik Müdürü, tarihini Doğum talep ve olup (sürüş lisans yetkilisi) veren size erişim vermeden önce güvendikleri değerini değerlendirecek.

Bir kimlik ile birden çok değer birden fazla talep içerebilir ve aynı türde birden fazla talep içerebilir.

## <a name="adding-claims-checks"></a>Denetimleri talep ekleme

Talep tabanlı yetkilendirme denetimleri bildirim temelli - Geliştirici bunları kendi kodundaki bir denetleyici veya eylem bir denetleyici içinde karşı geçerli kullanıcı verilerine sahip olması gerekir ve isteğe bağlı olarak, talep değeri erişimi tutmanız gereken talepleri belirtme ekler İstenen kaynak. Talepler, ilke tabanlı gereksinimleridir Geliştirici oluşturun ve talep gereksinimleri ifade bir ilkeyi kaydetmek gerekir.

En basit türü talep İlkesi talebi varlığı arar ve değeri denetlemez.

İlk oluşturun ve ilkeyi kaydetmeniz gerekir. Bu normalde bölümü alır, yetkilendirme hizmet yapılandırmasının parçası olarak gerçekleşmeden `ConfigureServices()` içinde *Startup.cs* dosya.

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

Ardından ilkeyi kullanan uygulama `Policy` özelliği `AuthorizeAttribute` ; ilke adını belirtmek için özniteliği

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

`AuthorizeAttribute` Özniteliği bu örnekte, tüm bir denetleyici için kimlikleri ilkeyle eşleşen herhangi bir işlem erişim denetleyicisinde izin verilecek yalnızca uygulanabilir.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Tarafından korunan bir denetleyici varsa `AuthorizeAttribute` özniteliği ancak uyguladığınız belirli eylemlere anonim erişime izin vermek istediğiniz `AllowAnonymousAttribute` özniteliği.

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

Çoğu talep değeri ile gelir. İlkeyi oluştururken, izin verilen değerlerin bir listesini belirtebilirsiniz. Aşağıdaki örnek 1, 2, 3, 4 veya 5 ayarlanmış çalışan numarası olan çalışanlar için yalnızca başarılı olabilir.

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

### <a name="add-a-generic-claim-check"></a>Genel talep Denetimi Ekle

Talep değeri tek bir değer değil veya bir dönüştürme gerekli değildir, kullanın [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Daha fazla bilgi için [bir ilkeyi karşılamak için bir func kullanarak](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Birden çok ilke değerlendirmesi

Bir denetleyici veya eylem için birden çok ilke uygularsanız, erişim sağlanmadan önce tüm ilkeleri geçmesi gerekir. Örneğin:

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

Yukarıdaki örnekte, karşıladığı tüm kimlik `EmployeeOnly` erişim ilkesi `Payslip` denetleyicisinde eylemi olarak bu ilke zorunlu tutulur. Ancak çağırmak için `UpdateSalary` gerekir kimlik karşılamak eylem *hem* `EmployeeOnly` ilke ve `HumanResources` ilkesi.

Daha karmaşık ilkeleri istiyorsanız, talep doğum tarihi alma gibi verilerden bir yaş hesaplama sonra geçerlilik süresi denetimi 21 veya eski rolüyse yazmanız gereken [özel ilke işleyicileri](xref:security/authorization/policies).
