---
title: ASP.NET core'da ilke tabanlı yetkilendirme
author: rick-anderson
description: Oluşturma ve ASP.NET Core uygulaması yetkilendirme gereksinimlerini zorunlu tutmak için yetkilendirme ilkesi işleyicileri kullanma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068176"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="82738-103">ASP.NET core'da ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="82738-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="82738-104">Aslında, [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [beyana dayalı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın.</span><span class="sxs-lookup"><span data-stu-id="82738-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="82738-105">Bu yapı taşlarını kodu yetkilendirme değerlendirmeleri ifade destekler.</span><span class="sxs-lookup"><span data-stu-id="82738-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="82738-106">Daha zengin, yeniden kullanılabilir, test edilebilir yetkilendirme yapısı sonucudur.</span><span class="sxs-lookup"><span data-stu-id="82738-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="82738-107">Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur.</span><span class="sxs-lookup"><span data-stu-id="82738-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="82738-108">İçinde yetkilendirme hizmet yapılandırmasının bir parçası olarak kayıtlı olduğu `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="82738-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="82738-109">Önceki örnekte, bir "AtLeast21" ilke oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="82738-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="82738-110">Tek bir gereksinim olan&mdash;, gereksinim parametre olarak sağlanan bir en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="82738-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="82738-111">MVC denetleyicileri için ilkelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="82738-111">Applying policies to MVC controllers</span></span>

<span data-ttu-id="82738-112">Razor sayfaları kullanıyorsanız, bkz. [ilkeleri Razor sayfaları için uygulama](#applying-policies-to-razor-pages) bu belgedeki.</span><span class="sxs-lookup"><span data-stu-id="82738-112">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="82738-113">İlkeleri kullanarak denetleyicilerine uygulanır `[Authorize]` özniteliği ile ilke adı.</span><span class="sxs-lookup"><span data-stu-id="82738-113">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="82738-114">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="82738-114">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="82738-115">Razor sayfaları için ilkelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="82738-115">Applying policies to Razor Pages</span></span>

<span data-ttu-id="82738-116">İlkeleri kullanarak Razor sayfaları için uygulanır `[Authorize]` özniteliği ile ilke adı.</span><span class="sxs-lookup"><span data-stu-id="82738-116">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="82738-117">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="82738-117">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="82738-118">İlkeler ayrıca uygulanabilir Razor sayfaları kullanarak bir [yetkilendirme kuralı](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="82738-118">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="82738-119">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="82738-119">Requirements</span></span>

<span data-ttu-id="82738-120">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl adı değerlendirmek için kullanabileceğiniz parametreler koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="82738-120">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="82738-121">"AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;en düşük yaş.</span><span class="sxs-lookup"><span data-stu-id="82738-121">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="82738-122">Bir gereksinim uygulayan [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), bir boş işaretleyici arabirim olduğu.</span><span class="sxs-lookup"><span data-stu-id="82738-122">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="82738-123">Bir parametreli en düşük yaş gereksinim şu şekilde uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="82738-123">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="82738-124">Bir yetkilendirme ilkesi birden fazla yetkilendirme gereksinimi varsa, başarılı olması ilke değerlendirmesi için sırayla tüm gereksinimleri geçmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="82738-124">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="82738-125">Diğer bir deyişle, tek bir Yetkilendirme İlkesi'ne eklemiş birden çok yetkilendirme gereksinimlerini şirket kabul edilir bir **ve** temel.</span><span class="sxs-lookup"><span data-stu-id="82738-125">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="82738-126">Bir gereksinim veri veya özellikleri sahip olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="82738-126">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="82738-127">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="82738-127">Authorization handlers</span></span>

<span data-ttu-id="82738-128">Bir gereksinimin özelliklerin değerlendirilmesi için bir yetkilendirme işleyicisi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="82738-128">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="82738-129">Yetkilendirme işleyici gereksinimleri bir sağlanan karşı değerlendirir [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) erişim izninin olup olmadığını belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="82738-129">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="82738-130">Bir gereksinim olabilir [birden fazla işleyici](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="82738-130">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="82738-131">Bir işleyici devralabilir [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)burada `TRequirement` işlenecek gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="82738-131">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="82738-132">Alternatif olarak, bir işleyici uygulayabilir [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) gereksinim birden fazla tür işlemek için.</span><span class="sxs-lookup"><span data-stu-id="82738-132">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="82738-133">Bir gereksinim için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="82738-133">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="82738-134">Tek gereksinim, en düşük yaş işleyici yararlanan bire bir ilişki örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="82738-134">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="82738-135">Yukarıdaki kod, geçerli kullanıcının asıl bilinen ve güvenilir bir veren tarafından verildi talep doğum tarihi varsa belirler.</span><span class="sxs-lookup"><span data-stu-id="82738-135">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="82738-136">Yetkilendirme talep eksik olduğunda olamaz, bu durumda tamamlanmış bir görevin döndürülür.</span><span class="sxs-lookup"><span data-stu-id="82738-136">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="82738-137">Bir talep mevcut olduğunda, kullanıcının yaşını hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="82738-137">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="82738-138">Kullanıcı ihtiyaç tarafından tanımlanan en düşük yaş karşılıyorsa, Yetkilendirme başarılı olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="82738-138">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="82738-139">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle tek parametre olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="82738-139">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="82738-140">Birden fazla gereksinimi için bir işleyici kullanın</span><span class="sxs-lookup"><span data-stu-id="82738-140">Use a handler for multiple requirements</span></span>

<span data-ttu-id="82738-141">Bir izni işleyici gereksinimleri üç farklı türde işleyebilir bire çok ilişkisi örneği verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="82738-141">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="82738-142">Yukarıdaki kod trafiğiyle [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;gereksinimlerini içermeyen bir özelliğin başarılı olarak işaretlenmiş.</span><span class="sxs-lookup"><span data-stu-id="82738-142">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="82738-143">İçin bir `ReadPermission` gereksinimi, kullanıcının sahibi veya bir sponsor istenen kaynağa erişim için olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="82738-143">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="82738-144">Durumunda, bir `EditPermission` veya `DeletePermission` gereksinimi, isterse istenen kaynağa erişim için bir sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="82738-144">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="82738-145">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="82738-145">Handler registration</span></span>

<span data-ttu-id="82738-146">İşleyicileri, yapılandırma sırasında Hizmetleri koleksiyondaki kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="82738-146">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="82738-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="82738-147">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="82738-148">Yukarıdaki kod kaydeder `MinimumAgeHandler` çağırarak singleton olarak `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="82738-148">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="82738-149">Yerleşik birini kullanarak işleyicileri kaydedilebilir [hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="82738-149">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="82738-150">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="82738-150">What should a handler return?</span></span>

<span data-ttu-id="82738-151">Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="82738-151">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="82738-152">Başarı veya başarısızlık belirtilen durumunun nasıl mi?</span><span class="sxs-lookup"><span data-stu-id="82738-152">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="82738-153">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirmeden başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="82738-153">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="82738-154">Bir işleyici olarak aynı gereksinim için diğer işleyicilerin başarabilir hatalarını genellikle işlemek zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="82738-154">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="82738-155">Diğer gereksinim işleyicilerine başarılı olsa bile hata, garanti çağrısı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="82738-155">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="82738-156">Bir işleyici çağırırsa `context.Succeed` veya `context.Fail`, diğer tüm işleyiciler yine de denir.</span><span class="sxs-lookup"><span data-stu-id="82738-156">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="82738-157">Bu, yan etkileri gibi başka bir işleyiciye başarıyla doğrulandı veya bir gereksinim başarısız olsa bile gerçekleşir günlüğünü üretmek gereksinimleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="82738-157">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="82738-158">Ayarlandığında `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) özelliği (ASP.NET Core 1.1 bulunan ve üzeri) short-circuits işleyicileri yürütülmesi zaman `context.Fail` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="82738-158">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="82738-159">`InvokeHandlersAfterFailure` Varsayılan olarak `true`, bu durumda tüm işleyicileri olarak da adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="82738-159">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="82738-160">Kimlik doğrulaması başarısız olursa yetkilendirme işleyicileri çağrılır.</span><span class="sxs-lookup"><span data-stu-id="82738-160">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="82738-161">Birden fazla işleyici için bir gereksinim neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="82738-161">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="82738-162">Değerlendirme olmasını istediğiniz durumlarda bir **veya** temelinde tek bir gereksinim için birden çok işleyicisini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="82738-162">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="82738-163">Örneğin, Microsoft yalnızca anahtar kartlarla açtığınız kapılar sahiptir.</span><span class="sxs-lookup"><span data-stu-id="82738-163">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="82738-164">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapısını açar.</span><span class="sxs-lookup"><span data-stu-id="82738-164">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="82738-165">Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri tek bir gereksinim İnceleme birden fazla işleyici.</span><span class="sxs-lookup"><span data-stu-id="82738-165">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="82738-166">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="82738-166">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="82738-167">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="82738-167">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="82738-168">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="82738-168">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="82738-169">Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="82738-169">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="82738-170">Bir ilke olduğunda ya da işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="82738-170">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="82738-171">Bir ilkeyi karşılamak için bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="82738-171">Using a func to fulfill a policy</span></span>

<span data-ttu-id="82738-172">Hangi getirdikten bir ilke kod içinde ifade basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="82738-172">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="82738-173">Sağlamanız mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="82738-173">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="82738-174">Örneğin, önceki `BadgeEntryHandler` şu şekilde yazılması:</span><span class="sxs-lookup"><span data-stu-id="82738-174">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="82738-175">MVC istek bağlamı işleyicilerde erişme</span><span class="sxs-lookup"><span data-stu-id="82738-175">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="82738-176">`HandleRequirementAsync` Yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işlemekte olduğunuz.</span><span class="sxs-lookup"><span data-stu-id="82738-176">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="82738-177">MVC veya Jabbr gibi çerçeveleri için herhangi bir nesne eklemek ücretsiz `Resource` özelliği `AuthorizationHandlerContext` fazladan bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="82738-177">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="82738-178">Örneğin, MVC bir örneğini geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği.</span><span class="sxs-lookup"><span data-stu-id="82738-178">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="82738-179">Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfaları tarafından sağlanan her şey.</span><span class="sxs-lookup"><span data-stu-id="82738-179">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="82738-180">Kullanımını `Resource` framework belirli bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="82738-180">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="82738-181">Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveleri için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="82738-181">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="82738-182">Cast `Resource` özelliğini kullanarak `is` anahtar sözcüğü, dönüştürme işleminin başarılı değil, kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:</span><span class="sxs-lookup"><span data-stu-id="82738-182">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
