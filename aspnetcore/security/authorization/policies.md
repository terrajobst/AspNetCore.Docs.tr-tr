---
title: ASP.NET core'da ilke tabanlı yetkilendirme
author: rick-anderson
description: Oluşturma ve ASP.NET Core uygulaması yetkilendirme gereksinimlerini zorunlu tutmak için yetkilendirme ilkesi işleyicileri kullanma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: c2bc626b2dd341dda878a151def6b405884357d7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665408"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="fd3f5-103">ASP.NET core'da ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="fd3f5-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="fd3f5-104">Aslında, [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [beyana dayalı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="fd3f5-105">Bu yapı taşlarını kodu yetkilendirme değerlendirmeleri ifade destekler.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="fd3f5-106">Daha zengin, yeniden kullanılabilir, test edilebilir yetkilendirme yapısı sonucudur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="fd3f5-107">Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="fd3f5-108">İçinde yetkilendirme hizmet yapılandırmasının bir parçası olarak kayıtlı olduğu `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="fd3f5-109">Önceki örnekte, bir "AtLeast21" ilke oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="fd3f5-110">Tek bir gereksinim olan&mdash;, gereksinim parametre olarak sağlanan bir en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="fd3f5-111">İlkeleri kullanarak uygulanır `[Authorize]` özniteliği ile ilke adı.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="fd3f5-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="fd3f5-113">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="fd3f5-113">Requirements</span></span>

<span data-ttu-id="fd3f5-114">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl adı değerlendirmek için kullanabileceğiniz parametreler koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="fd3f5-115">"AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="fd3f5-116">Bir gereksinim uygulayan [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), bir boş işaretleyici arabirim olduğu.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="fd3f5-117">Bir parametreli en düşük yaş gereksinim şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="fd3f5-118">Bir yetkilendirme ilkesi birden fazla yetkilendirme gereksinimi varsa, başarılı olması ilke değerlendirmesi için sırayla tüm gereksinimleri geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="fd3f5-119">Diğer bir deyişle, tek bir Yetkilendirme İlkesi'ne eklemiş birden çok yetkilendirme gereksinimlerini şirket kabul edilir bir **ve** temel.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="fd3f5-120">Bir gereksinim veri veya özellikleri sahip olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="fd3f5-121">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="fd3f5-121">Authorization handlers</span></span>

<span data-ttu-id="fd3f5-122">Bir gereksinimin özelliklerin değerlendirilmesi için bir yetkilendirme işleyicisi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="fd3f5-123">Yetkilendirme işleyici gereksinimleri bir sağlanan karşı değerlendirir [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) erişim izninin olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="fd3f5-124">Bir gereksinim olabilir [birden fazla işleyici](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="fd3f5-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="fd3f5-125">Bir işleyici devralabilir [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)burada `TRequirement` işlenecek gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="fd3f5-126">Alternatif olarak, bir işleyici uygulayabilir [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) gereksinim birden fazla tür işlemek için.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="fd3f5-127">Bir gereksinim için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="fd3f5-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="fd3f5-128">Tek gereksinim, en düşük yaş işleyici yararlanan bire bir ilişki örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="fd3f5-129">Yukarıdaki kod, geçerli kullanıcının asıl bilinen ve güvenilir bir veren tarafından verildi talep doğum tarihi varsa belirler.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="fd3f5-130">Yetkilendirme talep eksik olduğunda olamaz, bu durumda tamamlanmış bir görevin döndürülür.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="fd3f5-131">Bir talep mevcut olduğunda, kullanıcının yaşını hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="fd3f5-132">Kullanıcı ihtiyaç tarafından tanımlanan en düşük yaş karşılıyorsa, Yetkilendirme başarılı olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="fd3f5-133">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle tek parametre olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="fd3f5-134">Birden fazla gereksinimi için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="fd3f5-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="fd3f5-135">Bir izni işleyici gereksinimleri üç farklı türde işleyebilir bire çok ilişkisi örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="fd3f5-136">Yukarıdaki kod trafiğiyle [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;gereksinimlerini içermeyen bir özelliğin başarılı olarak işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="fd3f5-137">İçin bir `ReadPermission` gereksinimi, kullanıcının sahibi veya bir sponsor istenen kaynağa erişim için olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="fd3f5-138">Durumunda, bir `EditPermission` veya `DeletePermission` gereksinimi, isterse istenen kaynağa erişim için bir sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="fd3f5-139">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="fd3f5-139">Handler registration</span></span>

<span data-ttu-id="fd3f5-140">İşleyicileri, yapılandırma sırasında Hizmetleri koleksiyondaki kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="fd3f5-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="fd3f5-142">Yukarıdaki kod kaydeder `MinimumAgeHandler` çağırarak singleton olarak `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-142">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="fd3f5-143">Yerleşik birini kullanarak işleyicileri kaydedilebilir [hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="fd3f5-143">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="fd3f5-144">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="fd3f5-144">What should a handler return?</span></span>

<span data-ttu-id="fd3f5-145">Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-145">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="fd3f5-146">Başarı veya başarısızlık belirtilen durumunun nasıl mi?</span><span class="sxs-lookup"><span data-stu-id="fd3f5-146">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="fd3f5-147">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirmeden başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-147">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="fd3f5-148">Bir işleyici olarak aynı gereksinim için diğer işleyicilerin başarabilir hatalarını genellikle işlemek zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-148">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="fd3f5-149">Diğer gereksinim işleyicilerine başarılı olsa bile hata, garanti çağrısı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-149">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="fd3f5-150">Ayarlandığında `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) özelliği (ASP.NET Core 1.1 bulunan ve üzeri) short-circuits işleyicileri yürütülmesi zaman `context.Fail` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-150">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="fd3f5-151">`InvokeHandlersAfterFailure` Varsayılan olarak `true`, bu durumda tüm işleyicileri olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-151">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="fd3f5-152">Böylece, her zaman gerçekleşmesi etkilere, günlük kaydı gibi üretmek gereksinimleri bile `context.Fail` başka bir işleyicisinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-152">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="fd3f5-153">Birden fazla işleyici için bir gereksinim neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="fd3f5-153">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="fd3f5-154">Değerlendirme olmasını istediğiniz durumlarda bir **veya** temelinde tek bir gereksinim için birden çok işleyicisini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-154">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="fd3f5-155">Örneğin, Microsoft yalnızca anahtar kartlarla açtığınız kapılar sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-155">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="fd3f5-156">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapısını açar.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-156">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="fd3f5-157">Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri tek bir gereksinim İnceleme birden fazla işleyici.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-157">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="fd3f5-158">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="fd3f5-158">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="fd3f5-159">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fd3f5-159">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="fd3f5-160">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fd3f5-160">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="fd3f5-161">Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="fd3f5-161">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="fd3f5-162">Bir ilke olduğunda ya da işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-162">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="fd3f5-163">Bir ilkeyi karşılamak için bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="fd3f5-163">Using a func to fulfill a policy</span></span>

<span data-ttu-id="fd3f5-164">Hangi getirdikten bir ilke kod içinde ifade basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-164">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="fd3f5-165">Sağlamanız mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-165">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="fd3f5-166">Örneğin, önceki `BadgeEntryHandler` şu şekilde yazılması:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-166">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="fd3f5-167">MVC istek bağlamı işleyicilerde erişme</span><span class="sxs-lookup"><span data-stu-id="fd3f5-167">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="fd3f5-168">`HandleRequirementAsync` Yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işlemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-168">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="fd3f5-169">MVC veya Jabbr gibi çerçeveleri için herhangi bir nesne eklemek ücretsiz `Resource` özelliği `AuthorizationHandlerContext` fazladan bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-169">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="fd3f5-170">Örneğin, MVC bir örneğini geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-170">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="fd3f5-171">Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfaları tarafından sağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-171">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="fd3f5-172">Kullanımını `Resource` framework belirli bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-172">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="fd3f5-173">Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveleri için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="fd3f5-173">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="fd3f5-174">Cast `Resource` özelliğini kullanarak `is` anahtar sözcüğü, dönüştürme işleminin başarılı değil, kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:</span><span class="sxs-lookup"><span data-stu-id="fd3f5-174">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
