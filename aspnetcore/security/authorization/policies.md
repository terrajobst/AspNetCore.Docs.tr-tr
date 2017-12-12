---
title: "Özel ilke tabanlı yetkilendirme"
author: rick-anderson
description: "Bu belge, oluşturma ve ASP.NET Core uygulamada özel yetkilendirme ilkesi işleyicileri kullanma açıklanmaktadır."
keywords: "ASP.NET Core, yetkilendirme, özel İlkesi, yetkilendirme ilkesi"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>Özel ilke tabanlı yetkilendirme

<a name="security-authorization-policies-based"></a>

Kapak altında [rol yetkilendirme](roles.md) ve [yetkilendirme talep](claims.md) bir gereksinim kullanımını, gereksinim ve önceden yapılandırılmış bir ilke için bir işleyici olun. Bu yapı taşları yetkilendirme değerlendirmeleri kod, bir daha zengin için yeniden kullanılabilir, izin verme ve kolayca sınanabilir yetkilendirme yapısı express olanak sağlar.

Bir yetkilendirme ilkesi bir veya daha fazla gereksinimlerini oluşur ve yetkilendirme hizmet yapılandırmasının bir parçası kayıtlı uygulama başlangıcında `ConfigureServices` içinde *haline* dosya.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Burada "Over21" ilke en az bir yaşını, parametre olarak gereksinime geçtiğinden, tek bir gereksinim ile oluşturulan görebilirsiniz.

İlkeleri kullanarak uygulanır `Authorize` ilke adını, örneğin; belirterek özniteliği

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Gereksinimler

Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl değerlendirmek için kullanabileceğiniz veri parametreleri koleksiyonudur. Minimum yaş ilkemizi sahibiz tek bir parametre minimum yaş gereksinimdir. Bir gereksinim uygulamalıdır `IAuthorizationRequirement`. Bu bir boş bir işaretleyici arabirimdir. Parametreli minimum yaş gereksinimi gibi uygulanabilir;

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Bir gereksinim veri veya özellikleri olması gerekmez.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Yetkilendirme işleyicileri

Bir yetkilendirme işleyici herhangi bir gereksinim özelliklerini değerlendirmesi için sorumludur. Yetkilendirme işleyici tarafından sağlanan karşı değerlendirilmesi gerekir `AuthorizationHandlerContext` yetkilendirme izin verilip verilmediğini karar vermek için. Bir gereksinim olabilir [birden çok işleyici](policies.md#security-authorization-policies-based-multiple-handlers). İşleyicileri devralmalıdır `AuthorizationHandler<T>` T, işleme gereksinimi olduğu.

<a name="security-authorization-handler-example"></a>

Minimum yaş işleyici şuna benzeyebilir:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Yukarıdaki kod biz öncelikle geçerli kullanıcı asıl biliyoruz bir veren ve güven tarafından verildiği talep doğum tarihi sahipse görmek için bakın. Talep eksikse, döndürürüz biz yetkisi kaldırılamıyor. Biz bir talep varsa, kullanıcının kaç yaşında olduğunu şekil ve minimum yaş gereksinimi tarafından geçirilen karşılıyorsa sonra Yetkilendirme başarılı olmuştur. Yetkilendirme başarılı olduktan sonra diyoruz `context.Succeed()` parametre olarak başarılı olmuştur gereksinim geçirme.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>İşleyici kaydı
İşleyicileri yapılandırma sırasında Hizmetleri koleksiyonundaki örneğin kayıtlı olması gerekir;

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Kullanarak Hizmetleri koleksiyonuna eklenen her işleyicisi `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` işleyici sınıfınızda geçirme.

## <a name="what-should-a-handler-return"></a>Ne bir işleyici döndürmelidir?

Görebilirsiniz bizim [işleyici örnek](policies.md#security-authorization-handler-example) , `Handle()` yöntemi sahip hiçbir değer döndürmeyen nasıl yapmak biz belirtmek başarı veya başarısızlık?

* Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirme başarıyla doğrulanmış.

* Diğer işleyicilerin aynı gereksinimi başarılı olabilir gibi hatalar genellikle işlemek bir işleyici gerekmez.

* Bir gereksinim için diğer işleyicilerin başarılı olsa bile hatası güvence altına almak için çağrı `context.Fail`.

Bir ilke gereksinim gerektirdiğinde işleyicinizi içinde çağrısı bağımsız olarak, bir gereksinim için tüm işleyiciler çağrılır. Bu her zaman gerçekleşecek günlüğe kaydetme gibi yan etkileri gereksinimleri sağlar olsa bile `context.Fail()` içinde başka bir işleyici çağrılır.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Bir gereksinim için birden çok işleyici neden kullanmalıyım?

Değerlendirme üzerinde olmasını istediğiniz durumlarda bir **veya** olarak tek bir gereksinim için birden çok işleyici uygulamak. Örneğin, Microsoft, yalnızca anahtar kartlarla açtığınız kapıları sahiptir. Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapı açar. Bu senaryoda, tek bir gereksinim yoktur *EnterBuilding*, ancak her biri bir tek gereksinim inceleniyor birden çok işleyici.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Şimdi, her iki işleyicileri ediliyor [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) bir ilke değerlendirirken `EnterBuildingRequirement` ya da işleyici başarılı olursa ilke değerlendirmesi başarılı olur.

## <a name="using-a-func-to-fulfill-a-policy"></a>Bir ilke karşılamak üzere bir func kullanma

Bir ilke gerçekleşmesine kodda express basit olduğu durumlar olabilir. Yalnızca sağlamak olası bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.

Örneğin önceki `BadgeEntryHandler` gibi yazılması:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>MVC istek bağlamı işleyiciler erişme

`Handle` Bir yetkilendirme işleyicisinde uygulanmalı yöntemi iki parametreye sahip bir `AuthorizationContext` ve `Requirement` işleme. Çerçeveler MVC veya Jabbr gibi herhangi bir nesne eklemek ücretsiz `Resource` özellikte `AuthorizationContext` ek bilgi geçirmek için.

Örneğin, MVC örneği geçirir `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` başka MVC HttpContext, RouteData ve her şeyi erişmek için kullanılan kaynak özelliği sağlar.

Kullanımını `Resource` çerçeveye özgü bir özelliktir. Bilgileri kullanarak `Resource` özelliği, yetkilendirme ilkelerini belirli çerçeveler için sınırlamak. Cast `Resource` özelliğini kullanarak `as` anahtar sözcüğü ve ardından onay cast sahip başarılı olmayan kodunuzu kilitlenme emin olmak için birlikte `InvalidCastExceptions` diğer çerçeveler; çalıştırdığınızda

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
