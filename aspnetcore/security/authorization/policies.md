---
title: "ASP.NET Core özel ilke tabanlı yetkilendirme"
author: rick-anderson
description: "Oluşturmayı ve ASP.NET Core uygulama yetkilendirme gereksinimleri zorlama için özel yetkilendirme ilkesi işleyicileri kullanmayı öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 0eb5451828a51771d9388c2db610ede6231ced51
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="custom-policy-based-authorization"></a>Özel ilke tabanlı yetkilendirme

Kapak altında [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [talep tabanlı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın. Bu yapı taşları yetkilendirme değerlendirmeleri ifade kodda destekler. Sonuç daha zengin, yeniden kullanılabilir, sınanabilir yetkilendirme yapısıdır.

Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur. Yetkilendirme hizmet yapılandırmasının bir parçası da kaydedilir `ConfigureServices` yöntemi `Startup` sınıfı:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

Önceki örnekte, bir "AtLeast21" ilke oluşturulur. En az bir yaşını hangi gereksinim parametre olarak sağlanan bir tek gereksinim vardır.

İlkeleri kullanarak uygulanır `[Authorize]` ilke adı özniteliği. Örneğin:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Gereksinimler

Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl değerlendirmek için kullanabileceğiniz veri parametreleri koleksiyonudur. "AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;minimum yaş. Bir gereklilik uyguladığı `IAuthorizationRequirement`, boş işaretçi arabirim olduğu. Parametreli minimum yaş gereksinimi gibi uygulanabilir:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Bir gereksinim veri veya özellikleri olması gerekmez.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Yetkilendirme işleyicileri

Bir yetkilendirme işleyici gereksinimi ait özellikler değerlendirmesi için sorumludur. Yetkilendirme işleyici gereksinimlerine göre bir sağlanan değerlendirir `AuthorizationHandlerContext` erişim izin verilip verilmediğini belirlemek için. Bir gereksinim olabilir [birden çok işleyici](#security-authorization-policies-based-multiple-handlers). İşleyicileri devral `AuthorizationHandler<T>`, burada `T` işlenecek gereksinimdir.

<a name="security-authorization-handler-example"></a>

Minimum yaş işleyici şuna benzeyebilir:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Geçerli kullanıcı asıl bilinen ve güvenilir bir veren tarafından verildiği talep doğum tarihi sahipse önceki kod belirler. Talep eksik olduğunda yetkilendirme yapılmaz, tamamlanmış bir görevi; bu durumda döndürülür. Bir talep mevcut olduğunda, kullanıcının yaşı hesaplanır. Kullanıcı tarafından ihtiyaç tanımlanan minimum yaş karşılıyorsa, Yetkilendirme başarılı kabul edilir. Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle bir parametre olarak çağrılır.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>İşleyici kaydı

İşleyicileri yapılandırma sırasında Hizmetleri koleksiyonundaki kaydedilir. Örneğin:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Harekete geçirerek services koleksiyonuna eklenen her işleyicisi `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Ne bir işleyici döndürmelidir?

Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür. Nasıl başarı veya başarısızlık belirtilen durumunu mi?

* Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirme başarıyla doğrulanmış.

* Diğer işleyicilerin aynı gereksinimi başarılı olabilir gibi hatalar genellikle işlemek bir işleyici gerekmez.

* Diğer gereksinim işleyicileri başarılı olsa bile hatası, güvence altına almak için çağrı `context.Fail`.

Bir ilke gereksinim gerektirdiğinde işleyicinizi içinde çağrısı bağımsız olarak, bir gereksinim için tüm işleyiciler çağrılır. Bu her zaman gerçekleşecek günlüğe kaydetme gibi yan etkileri gereksinimleri sağlar olsa bile `context.Fail()` içinde başka bir işleyici çağrılır.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Bir gereksinim için birden çok işleyici neden kullanmalıyım?

Değerlendirme üzerinde olmasını istediğiniz durumlarda bir **veya** olarak tek bir gereksinim için birden çok işleyici uygulayın. Örneğin, Microsoft, yalnızca anahtar kartlarla açtığınız kapıları sahiptir. Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapı açar. Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri bir tek gereksinim inceleniyor birden çok işleyici.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Bir ilke, her iki işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.

## <a name="using-a-func-to-fulfill-a-policy"></a>Bir ilke karşılamak üzere bir func kullanma

Hangi gerçekleşmesine bir ilke kodda express basit olduğu durumlar olabilir. Tedarik mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.

Örneğin, önceki `BadgeEntryHandler` gibi yazılması:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>MVC istek bağlamı işleyiciler erişme

`HandleRequirementAsync` Bir yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işleme. Çerçeveler MVC veya Jabbr gibi herhangi bir nesne eklemek ücretsiz `Resource` özellikte `AuthorizationHandlerContext` ek bilgi geçirmek için.

Örneğin, MVC örneği geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği. Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfalarının tarafından sağlanan her şeyi.

Kullanımını `Resource` çerçeveye özgü bir özelliktir. Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveler için sınırlar. Cast `Resource` özelliğini kullanarak `as` anahtar sözcüğü ve sahip dönüştürme başarılı olmayan kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
