---
title: ASP.NET Core ilke tabanlı yetkilendirme
author: rick-anderson
description: Oluşturmayı ve ASP.NET Core uygulama yetkilendirme gereksinimleri zorlama için yetkilendirme ilkesi işleyicileri kullanmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072867"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="4990e-103">ASP.NET Core ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="4990e-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="4990e-104">Kapak altında [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [talep tabanlı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın.</span><span class="sxs-lookup"><span data-stu-id="4990e-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="4990e-105">Bu yapı taşları yetkilendirme değerlendirmeleri ifade kodda destekler.</span><span class="sxs-lookup"><span data-stu-id="4990e-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="4990e-106">Sonuç daha zengin, yeniden kullanılabilir, sınanabilir yetkilendirme yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="4990e-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="4990e-107">Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur.</span><span class="sxs-lookup"><span data-stu-id="4990e-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="4990e-108">Yetkilendirme hizmet yapılandırmasının bir parçası da kaydedilir `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4990e-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="4990e-109">Önceki örnekte, bir "AtLeast21" ilke oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4990e-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="4990e-110">Tek bir gereksinim olan&mdash;, gereksinim parametre olarak sağlanan bir minimum yaş.</span><span class="sxs-lookup"><span data-stu-id="4990e-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="4990e-111">İlkeleri kullanarak uygulanır `[Authorize]` ilke adı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4990e-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="4990e-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4990e-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="4990e-113">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="4990e-113">Requirements</span></span>

<span data-ttu-id="4990e-114">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl değerlendirmek için kullanabileceğiniz veri parametreleri koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="4990e-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="4990e-115">"AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;minimum yaş.</span><span class="sxs-lookup"><span data-stu-id="4990e-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="4990e-116">Bir gereklilik uyguladığı [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), boş işaretçi arabirim olduğu.</span><span class="sxs-lookup"><span data-stu-id="4990e-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="4990e-117">Parametreli minimum yaş gereksinimi gibi uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="4990e-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="4990e-118">Bir gereksinim veri veya özellikleri olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4990e-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="4990e-119">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="4990e-119">Authorization handlers</span></span>

<span data-ttu-id="4990e-120">Bir yetkilendirme işleyici gereksinimi ait özellikler değerlendirmesi için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="4990e-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="4990e-121">Yetkilendirme işleyici gereksinimlerine göre bir sağlanan değerlendirir [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) erişim izin verilip verilmediğini belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="4990e-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="4990e-122">Bir gereksinim olabilir [birden çok işleyici](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="4990e-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="4990e-123">Bir işleyici devral [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), burada `TRequirement` işlenecek gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="4990e-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="4990e-124">Alternatif olarak, bir işleyici uygulayabilir [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) gereksinim birden fazla tür işlemek için.</span><span class="sxs-lookup"><span data-stu-id="4990e-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="4990e-125">Bir gereksinim için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="4990e-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="4990e-126">Minimum yaş işleyici tek gereksinim yararlanan bire bir ilişki örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4990e-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="4990e-127">Geçerli kullanıcı asıl bilinen ve güvenilir bir veren tarafından verildiği talep doğum tarihi sahipse önceki kod belirler.</span><span class="sxs-lookup"><span data-stu-id="4990e-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="4990e-128">Talep eksik olduğunda yetkilendirme yapılmaz, tamamlanmış bir görevi; bu durumda döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4990e-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="4990e-129">Bir talep mevcut olduğunda, kullanıcının yaşı hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="4990e-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="4990e-130">Kullanıcı tarafından ihtiyaç tanımlanan minimum yaş karşılıyorsa, Yetkilendirme başarılı kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4990e-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="4990e-131">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle, tek parametresi olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4990e-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="4990e-132">Birden çok gereksinimleri için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="4990e-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="4990e-133">İzni işleyici üç gereksinimleri yararlanan bir-çok ilişkisi örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4990e-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="4990e-134">Önceki kod geçeceğini [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;gereksinimlerini içermeyen bir özelliğin başarılı olarak işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="4990e-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="4990e-135">Kullanıcı Okuma izni varsa çözemiyorsa sahibi veya bir sponsoru istenen kaynağa erişmek için olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4990e-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="4990e-136">Kullanıcı düzenleme veya silme izni varsa buldukça istenen kaynağa erişmek için bir sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4990e-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="4990e-137">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle, tek parametresi olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4990e-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="4990e-138">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="4990e-138">Handler registration</span></span>

<span data-ttu-id="4990e-139">İşleyicileri yapılandırma sırasında Hizmetleri koleksiyonundaki kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="4990e-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="4990e-140">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4990e-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="4990e-141">Harekete geçirerek services koleksiyonuna eklenen her işleyicisi `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="4990e-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="4990e-142">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="4990e-142">What should a handler return?</span></span>

<span data-ttu-id="4990e-143">Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="4990e-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="4990e-144">Nasıl başarı veya başarısızlık belirtilen durumunu mi?</span><span class="sxs-lookup"><span data-stu-id="4990e-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="4990e-145">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirme başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="4990e-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="4990e-146">Diğer işleyicilerin aynı gereksinimi başarılı olabilir gibi hatalar genellikle işlemek bir işleyici gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4990e-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="4990e-147">Diğer gereksinim işleyicileri başarılı olsa bile hatası, güvence altına almak için çağrı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="4990e-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="4990e-148">Ayarlandığında `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) özelliği (ASP.NET Core 1.1 kullanılabilir ve üzeri) short-circuits işleyicileri yürütülmesi zaman `context.Fail` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4990e-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="4990e-149">`InvokeHandlersAfterFailure` Varsayılan olarak `true`, tüm işleyiciler; bu durumda denir.</span><span class="sxs-lookup"><span data-stu-id="4990e-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="4990e-150">Bu yan, her zaman gerçekleşmesi etkiler, günlük kaydı gibi üretmek gereksinimleri sağlar olsa bile `context.Fail` içinde başka bir işleyici çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4990e-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="4990e-151">Bir gereksinim için birden çok işleyici neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="4990e-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="4990e-152">Değerlendirme üzerinde olmasını istediğiniz durumlarda bir **veya** olarak tek bir gereksinim için birden çok işleyici uygulayın.</span><span class="sxs-lookup"><span data-stu-id="4990e-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="4990e-153">Örneğin, Microsoft, yalnızca anahtar kartlarla açtığınız kapıları sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4990e-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="4990e-154">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapı açar.</span><span class="sxs-lookup"><span data-stu-id="4990e-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="4990e-155">Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri bir tek gereksinim inceleniyor birden çok işleyici.</span><span class="sxs-lookup"><span data-stu-id="4990e-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="4990e-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="4990e-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="4990e-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="4990e-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="4990e-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="4990e-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="4990e-159">Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="4990e-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="4990e-160">Bir ilke, her iki işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="4990e-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="4990e-161">Bir ilke karşılamak üzere bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="4990e-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="4990e-162">Hangi gerçekleşmesine bir ilke kodda express basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="4990e-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="4990e-163">Tedarik mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="4990e-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="4990e-164">Örneğin, önceki `BadgeEntryHandler` gibi yazılması:</span><span class="sxs-lookup"><span data-stu-id="4990e-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="4990e-165">MVC istek bağlamı işleyiciler erişme</span><span class="sxs-lookup"><span data-stu-id="4990e-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="4990e-166">`HandleRequirementAsync` Bir yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işleme.</span><span class="sxs-lookup"><span data-stu-id="4990e-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="4990e-167">Çerçeveler MVC veya Jabbr gibi herhangi bir nesne eklemek ücretsiz `Resource` özellikte `AuthorizationHandlerContext` ek bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="4990e-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="4990e-168">Örneğin, MVC örneği geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4990e-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="4990e-169">Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfalarının tarafından sağlanan her şeyi.</span><span class="sxs-lookup"><span data-stu-id="4990e-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="4990e-170">Kullanımını `Resource` çerçeveye özgü bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="4990e-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="4990e-171">Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveler için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="4990e-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="4990e-172">Cast `Resource` özelliğini kullanarak `as` anahtar sözcüğü ve sahip dönüştürme başarılı olmayan kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:</span><span class="sxs-lookup"><span data-stu-id="4990e-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
