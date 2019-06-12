---
title: ASP.NET core'da ilke tabanlı yetkilendirme
author: rick-anderson
description: Oluşturma ve ASP.NET Core uygulaması yetkilendirme gereksinimlerini zorunlu tutmak için yetkilendirme ilkesi işleyicileri kullanma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837351"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET core'da ilke tabanlı yetkilendirme

Aslında, [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [beyana dayalı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın. Bu yapı taşlarını kodu yetkilendirme değerlendirmeleri ifade destekler. Daha zengin, yeniden kullanılabilir, test edilebilir yetkilendirme yapısı sonucudur.

Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur. İçinde yetkilendirme hizmet yapılandırmasının bir parçası olarak kayıtlı olduğu `Startup.ConfigureServices` yöntemi:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

Önceki örnekte, bir "AtLeast21" ilke oluşturulur. Tek bir gereksinim olan&mdash;, gereksinim parametre olarak sağlanan bir en düşük yaş.

## <a name="iauthorizationservice"></a>Iauthorizationservice 

Yetkilendirme başarılı olup olmadığını belirleyen birincil hizmet <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Yukarıdaki kod, iki yöntem vurgular [Iauthorizationservice](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> bir işaretleyici yok yöntemleri ve yetkilendirme başarılı olup olmadığını izleme mekanizması ile hizmetidir.

Her <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> gereksinimler karşılanırsa denetlemek için sorumludur:
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> Sınıfı, işleyici gereksinimlerin karşılanmış olmadığını işaretlemek için kullanır:

```csharp
 context.Succeed(requirement)
```

Aşağıdaki kod Basitleştirilmiş (ve ek açıklamalı yorumlar ile) gösterir. varsayılan kimlik doğrulama servisi uygulaması:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

Aşağıdaki kod tipik bir gösterir `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

Kullanım <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> veya `[Authorize(Policy = "Something"]` yetkilendirme için.

## <a name="applying-policies-to-mvc-controllers"></a>MVC denetleyicileri için ilkelerini uygulama

Razor sayfaları kullanıyorsanız, bkz. [ilkeleri Razor sayfaları için uygulama](#applying-policies-to-razor-pages) bu belgedeki.

İlkeleri kullanarak denetleyicilerine uygulanır `[Authorize]` özniteliği ile ilke adı. Örneğin:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Razor sayfaları için ilkelerini uygulama

İlkeleri kullanarak Razor sayfaları için uygulanır `[Authorize]` özniteliği ile ilke adı. Örneğin:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

İlkeler ayrıca uygulanabilir Razor sayfaları kullanarak bir [yetkilendirme kuralı](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Gereksinimler

Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl adı değerlendirmek için kullanabileceğiniz parametreler koleksiyonudur. "AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;en düşük yaş. Bir gereksinim uygulayan [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), bir boş işaretleyici arabirim olduğu. Bir parametreli en düşük yaş gereksinim şu şekilde uygulanabilir:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Bir yetkilendirme ilkesi birden fazla yetkilendirme gereksinimi varsa, başarılı olması ilke değerlendirmesi için sırayla tüm gereksinimleri geçmesi gerekir. Diğer bir deyişle, tek bir Yetkilendirme İlkesi'ne eklemiş birden çok yetkilendirme gereksinimlerini şirket kabul edilir bir **ve** temel.

> [!NOTE]
> Bir gereksinim veri veya özellikleri sahip olması gerekmez.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Yetkilendirme işleyicileri

Bir gereksinimin özelliklerin değerlendirilmesi için bir yetkilendirme işleyicisi sorumludur. Yetkilendirme işleyici gereksinimleri bir sağlanan karşı değerlendirir [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) erişim izninin olup olmadığını belirlemek için.

Bir gereksinim olabilir [birden fazla işleyici](#security-authorization-policies-based-multiple-handlers). Bir işleyici devralabilir [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)burada `TRequirement` işlenecek gereksinimdir. Alternatif olarak, bir işleyici uygulayabilir [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) gereksinim birden fazla tür işlemek için.

### <a name="use-a-handler-for-one-requirement"></a>Bir gereksinim için bir işleyici kullanın

<a name="security-authorization-handler-example"></a>

Tek gereksinim, en düşük yaş işleyici yararlanan bire bir ilişki örneği verilmiştir:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Yukarıdaki kod, geçerli kullanıcının asıl bilinen ve güvenilir bir veren tarafından verildi talep doğum tarihi varsa belirler. Yetkilendirme talep eksik olduğunda olamaz, bu durumda tamamlanmış bir görevin döndürülür. Bir talep mevcut olduğunda, kullanıcının yaşını hesaplanır. Kullanıcı ihtiyaç tarafından tanımlanan en düşük yaş karşılıyorsa, Yetkilendirme başarılı olduğunu varsayar. Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle tek parametre olarak çağrılır.

### <a name="use-a-handler-for-multiple-requirements"></a>Birden fazla gereksinimi için bir işleyici kullanın

Bir izni işleyici gereksinimleri üç farklı türde işleyebilir bire çok ilişkisi örneği verilmiştir:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Yukarıdaki kod trafiğiyle [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;gereksinimlerini içermeyen bir özelliğin başarılı olarak işaretlenmiş. İçin bir `ReadPermission` gereksinimi, kullanıcının sahibi veya bir sponsor istenen kaynağa erişim için olmalıdır. Durumunda, bir `EditPermission` veya `DeletePermission` gereksinimi, isterse istenen kaynağa erişim için bir sahip olması gerekir.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>İşleyici kaydı

İşleyicileri, yapılandırma sırasında Hizmetleri koleksiyondaki kaydedilir. Örneğin:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Yukarıdaki kod kaydeder `MinimumAgeHandler` çağırarak singleton olarak `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Yerleşik birini kullanarak işleyicileri kaydedilebilir [hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Ne bir işleyici döndürmelidir?

Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür. Başarı veya başarısızlık belirtilen durumunun nasıl mi?

* Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirmeden başarıyla doğrulanmış.

* Bir işleyici olarak aynı gereksinim için diğer işleyicilerin başarabilir hatalarını genellikle işlemek zorunda değildir.

* Diğer gereksinim işleyicilerine başarılı olsa bile hata, garanti çağrısı `context.Fail`.

Bir işleyici çağırırsa `context.Succeed` veya `context.Fail`, diğer tüm işleyiciler yine de denir. Bu, yan etkileri gibi başka bir işleyiciye başarıyla doğrulandı veya bir gereksinim başarısız olsa bile gerçekleşir günlüğünü üretmek gereksinimleri sağlar. Ayarlandığında `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) özelliği (ASP.NET Core 1.1 bulunan ve üzeri) short-circuits işleyicileri yürütülmesi zaman `context.Fail` çağrılır. `InvokeHandlersAfterFailure` Varsayılan olarak `true`, bu durumda tüm işleyicileri olarak da adlandırılır.

> [!NOTE]
> Kimlik doğrulaması başarısız olursa yetkilendirme işleyicileri çağrılır.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Birden fazla işleyici için bir gereksinim neden kullanmalıyım?

Değerlendirme olmasını istediğiniz durumlarda bir **veya** temelinde tek bir gereksinim için birden çok işleyicisini uygulayın. Örneğin, Microsoft yalnızca anahtar kartlarla açtığınız kapılar sahiptir. Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapısını açar. Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri tek bir gereksinim İnceleme birden fazla işleyici.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Bir ilke olduğunda ya da işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.

## <a name="using-a-func-to-fulfill-a-policy"></a>Bir ilkeyi karşılamak için bir func kullanma

Hangi getirdikten bir ilke kod içinde ifade basit olduğu durumlar olabilir. Sağlamanız mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.

Örneğin, önceki `BadgeEntryHandler` şu şekilde yazılması:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>MVC istek bağlamı işleyicilerde erişme

`HandleRequirementAsync` Yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işlemekte olduğunuz. MVC veya Jabbr gibi çerçeveleri için herhangi bir nesne eklemek ücretsiz `Resource` özelliği `AuthorizationHandlerContext` fazladan bilgi geçirmek için.

Örneğin, MVC bir örneğini geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği. Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfaları tarafından sağlanan her şey.

Kullanımını `Resource` framework belirli bir özelliktir. Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveleri için sınırlar. Cast `Resource` özelliğini kullanarak `is` anahtar sözcüğü, dönüştürme işleminin başarılı değil, kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
