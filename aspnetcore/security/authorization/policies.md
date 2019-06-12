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
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="55353-103">ASP.NET core'da ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="55353-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="55353-104">Aslında, [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [beyana dayalı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın.</span><span class="sxs-lookup"><span data-stu-id="55353-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="55353-105">Bu yapı taşlarını kodu yetkilendirme değerlendirmeleri ifade destekler.</span><span class="sxs-lookup"><span data-stu-id="55353-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="55353-106">Daha zengin, yeniden kullanılabilir, test edilebilir yetkilendirme yapısı sonucudur.</span><span class="sxs-lookup"><span data-stu-id="55353-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="55353-107">Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur.</span><span class="sxs-lookup"><span data-stu-id="55353-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="55353-108">İçinde yetkilendirme hizmet yapılandırmasının bir parçası olarak kayıtlı olduğu `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="55353-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="55353-109">Önceki örnekte, bir "AtLeast21" ilke oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="55353-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="55353-110">Tek bir gereksinim olan&mdash;, gereksinim parametre olarak sağlanan bir en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="55353-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="55353-111">Iauthorizationservice</span><span class="sxs-lookup"><span data-stu-id="55353-111">IAuthorizationService</span></span> 

<span data-ttu-id="55353-112">Yetkilendirme başarılı olup olmadığını belirleyen birincil hizmet <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="55353-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="55353-113">Yukarıdaki kod, iki yöntem vurgular [Iauthorizationservice](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="55353-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="55353-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> bir işaretleyici yok yöntemleri ve yetkilendirme başarılı olup olmadığını izleme mekanizması ile hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="55353-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="55353-115">Her <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> gereksinimler karşılanırsa denetlemek için sorumludur:</span><span class="sxs-lookup"><span data-stu-id="55353-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="55353-116"><xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> Sınıfı, işleyici gereksinimlerin karşılanmış olmadığını işaretlemek için kullanır:</span><span class="sxs-lookup"><span data-stu-id="55353-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="55353-117">Aşağıdaki kod Basitleştirilmiş (ve ek açıklamalı yorumlar ile) gösterir. varsayılan kimlik doğrulama servisi uygulaması:</span><span class="sxs-lookup"><span data-stu-id="55353-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="55353-118">Aşağıdaki kod tipik bir gösterir `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="55353-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="55353-119">Kullanım <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> veya `[Authorize(Policy = "Something"]` yetkilendirme için.</span><span class="sxs-lookup"><span data-stu-id="55353-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something"]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="55353-120">MVC denetleyicileri için ilkelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="55353-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="55353-121">Razor sayfaları kullanıyorsanız, bkz. [ilkeleri Razor sayfaları için uygulama](#applying-policies-to-razor-pages) bu belgedeki.</span><span class="sxs-lookup"><span data-stu-id="55353-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="55353-122">İlkeleri kullanarak denetleyicilerine uygulanır `[Authorize]` özniteliği ile ilke adı.</span><span class="sxs-lookup"><span data-stu-id="55353-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="55353-123">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="55353-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="55353-124">Razor sayfaları için ilkelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="55353-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="55353-125">İlkeleri kullanarak Razor sayfaları için uygulanır `[Authorize]` özniteliği ile ilke adı.</span><span class="sxs-lookup"><span data-stu-id="55353-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="55353-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="55353-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="55353-127">İlkeler ayrıca uygulanabilir Razor sayfaları kullanarak bir [yetkilendirme kuralı](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="55353-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="55353-128">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="55353-128">Requirements</span></span>

<span data-ttu-id="55353-129">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl adı değerlendirmek için kullanabileceğiniz parametreler koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="55353-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="55353-130">"AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="55353-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="55353-131">Bir gereksinim uygulayan [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), bir boş işaretleyici arabirim olduğu.</span><span class="sxs-lookup"><span data-stu-id="55353-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="55353-132">Bir parametreli en düşük yaş gereksinim şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="55353-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="55353-133">Bir yetkilendirme ilkesi birden fazla yetkilendirme gereksinimi varsa, başarılı olması ilke değerlendirmesi için sırayla tüm gereksinimleri geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55353-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="55353-134">Diğer bir deyişle, tek bir Yetkilendirme İlkesi'ne eklemiş birden çok yetkilendirme gereksinimlerini şirket kabul edilir bir **ve** temel.</span><span class="sxs-lookup"><span data-stu-id="55353-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="55353-135">Bir gereksinim veri veya özellikleri sahip olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="55353-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="55353-136">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="55353-136">Authorization handlers</span></span>

<span data-ttu-id="55353-137">Bir gereksinimin özelliklerin değerlendirilmesi için bir yetkilendirme işleyicisi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="55353-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="55353-138">Yetkilendirme işleyici gereksinimleri bir sağlanan karşı değerlendirir [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) erişim izninin olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="55353-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="55353-139">Bir gereksinim olabilir [birden fazla işleyici](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="55353-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="55353-140">Bir işleyici devralabilir [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)burada `TRequirement` işlenecek gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="55353-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="55353-141">Alternatif olarak, bir işleyici uygulayabilir [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) gereksinim birden fazla tür işlemek için.</span><span class="sxs-lookup"><span data-stu-id="55353-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="55353-142">Bir gereksinim için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="55353-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="55353-143">Tek gereksinim, en düşük yaş işleyici yararlanan bire bir ilişki örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="55353-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="55353-144">Yukarıdaki kod, geçerli kullanıcının asıl bilinen ve güvenilir bir veren tarafından verildi talep doğum tarihi varsa belirler.</span><span class="sxs-lookup"><span data-stu-id="55353-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="55353-145">Yetkilendirme talep eksik olduğunda olamaz, bu durumda tamamlanmış bir görevin döndürülür.</span><span class="sxs-lookup"><span data-stu-id="55353-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="55353-146">Bir talep mevcut olduğunda, kullanıcının yaşını hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="55353-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="55353-147">Kullanıcı ihtiyaç tarafından tanımlanan en düşük yaş karşılıyorsa, Yetkilendirme başarılı olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="55353-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="55353-148">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle tek parametre olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="55353-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="55353-149">Birden fazla gereksinimi için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="55353-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="55353-150">Bir izni işleyici gereksinimleri üç farklı türde işleyebilir bire çok ilişkisi örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="55353-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="55353-151">Yukarıdaki kod trafiğiyle [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;gereksinimlerini içermeyen bir özelliğin başarılı olarak işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="55353-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="55353-152">İçin bir `ReadPermission` gereksinimi, kullanıcının sahibi veya bir sponsor istenen kaynağa erişim için olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="55353-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="55353-153">Durumunda, bir `EditPermission` veya `DeletePermission` gereksinimi, isterse istenen kaynağa erişim için bir sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55353-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="55353-154">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="55353-154">Handler registration</span></span>

<span data-ttu-id="55353-155">İşleyicileri, yapılandırma sırasında Hizmetleri koleksiyondaki kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="55353-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="55353-156">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="55353-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="55353-157">Yukarıdaki kod kaydeder `MinimumAgeHandler` çağırarak singleton olarak `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="55353-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="55353-158">Yerleşik birini kullanarak işleyicileri kaydedilebilir [hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="55353-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="55353-159">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="55353-159">What should a handler return?</span></span>

<span data-ttu-id="55353-160">Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="55353-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="55353-161">Başarı veya başarısızlık belirtilen durumunun nasıl mi?</span><span class="sxs-lookup"><span data-stu-id="55353-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="55353-162">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirmeden başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="55353-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="55353-163">Bir işleyici olarak aynı gereksinim için diğer işleyicilerin başarabilir hatalarını genellikle işlemek zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="55353-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="55353-164">Diğer gereksinim işleyicilerine başarılı olsa bile hata, garanti çağrısı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="55353-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="55353-165">Bir işleyici çağırırsa `context.Succeed` veya `context.Fail`, diğer tüm işleyiciler yine de denir.</span><span class="sxs-lookup"><span data-stu-id="55353-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="55353-166">Bu, yan etkileri gibi başka bir işleyiciye başarıyla doğrulandı veya bir gereksinim başarısız olsa bile gerçekleşir günlüğünü üretmek gereksinimleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="55353-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="55353-167">Ayarlandığında `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) özelliği (ASP.NET Core 1.1 bulunan ve üzeri) short-circuits işleyicileri yürütülmesi zaman `context.Fail` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="55353-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="55353-168">`InvokeHandlersAfterFailure` Varsayılan olarak `true`, bu durumda tüm işleyicileri olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="55353-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="55353-169">Kimlik doğrulaması başarısız olursa yetkilendirme işleyicileri çağrılır.</span><span class="sxs-lookup"><span data-stu-id="55353-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="55353-170">Birden fazla işleyici için bir gereksinim neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="55353-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="55353-171">Değerlendirme olmasını istediğiniz durumlarda bir **veya** temelinde tek bir gereksinim için birden çok işleyicisini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="55353-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="55353-172">Örneğin, Microsoft yalnızca anahtar kartlarla açtığınız kapılar sahiptir.</span><span class="sxs-lookup"><span data-stu-id="55353-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="55353-173">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapısını açar.</span><span class="sxs-lookup"><span data-stu-id="55353-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="55353-174">Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri tek bir gereksinim İnceleme birden fazla işleyici.</span><span class="sxs-lookup"><span data-stu-id="55353-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="55353-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="55353-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="55353-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="55353-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="55353-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="55353-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="55353-178">Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="55353-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="55353-179">Bir ilke olduğunda ya da işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="55353-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="55353-180">Bir ilkeyi karşılamak için bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="55353-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="55353-181">Hangi getirdikten bir ilke kod içinde ifade basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="55353-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="55353-182">Sağlamanız mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="55353-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="55353-183">Örneğin, önceki `BadgeEntryHandler` şu şekilde yazılması:</span><span class="sxs-lookup"><span data-stu-id="55353-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="55353-184">MVC istek bağlamı işleyicilerde erişme</span><span class="sxs-lookup"><span data-stu-id="55353-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="55353-185">`HandleRequirementAsync` Yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işlemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="55353-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="55353-186">MVC veya Jabbr gibi çerçeveleri için herhangi bir nesne eklemek ücretsiz `Resource` özelliği `AuthorizationHandlerContext` fazladan bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="55353-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="55353-187">Örneğin, MVC bir örneğini geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği.</span><span class="sxs-lookup"><span data-stu-id="55353-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="55353-188">Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfaları tarafından sağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="55353-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="55353-189">Kullanımını `Resource` framework belirli bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="55353-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="55353-190">Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveleri için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="55353-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="55353-191">Cast `Resource` özelliğini kullanarak `is` anahtar sözcüğü, dönüştürme işleminin başarılı değil, kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:</span><span class="sxs-lookup"><span data-stu-id="55353-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
