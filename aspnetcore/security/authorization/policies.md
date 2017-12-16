---
title: "ASP.NET Core özel ilke tabanlı yetkilendirme"
author: rick-anderson
description: "Oluşturmayı ve ASP.NET Core uygulama yetkilendirme gereksinimleri zorlama için özel yetkilendirme ilkesi işleyicileri kullanmayı öğrenin."
keywords: "ASP.NET Core, yetkilendirme, özel İlkesi, yetkilendirme ilkesi"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="ad654-104">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="ad654-104">Custom policy-based authorization</span></span>

<span data-ttu-id="ad654-105">Kapak altında [rol tabanlı yetkilendirme](xref:security/authorization/roles) ve [talep tabanlı yetkilendirme](xref:security/authorization/claims) bir gereksinim, bir gereksinim işleyici ve önceden yapılandırılmış bir ilke kullanın.</span><span class="sxs-lookup"><span data-stu-id="ad654-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="ad654-106">Bu yapı taşları yetkilendirme değerlendirmeleri ifade kodda destekler.</span><span class="sxs-lookup"><span data-stu-id="ad654-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="ad654-107">Sonuç daha zengin, yeniden kullanılabilir, sınanabilir yetkilendirme yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="ad654-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="ad654-108">Bir yetkilendirme ilkesi, bir veya daha fazla gereksinimlerini oluşur.</span><span class="sxs-lookup"><span data-stu-id="ad654-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="ad654-109">Yetkilendirme hizmet yapılandırmasının bir parçası da kaydedilir `ConfigureServices` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ad654-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="ad654-110">Önceki örnekte, bir "AtLeast21" ilke oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ad654-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="ad654-111">En az bir yaşını hangi gereksinim parametre olarak sağlanan bir tek gereksinim vardır.</span><span class="sxs-lookup"><span data-stu-id="ad654-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="ad654-112">İlkeleri kullanarak uygulanır `[Authorize]` ilke adı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="ad654-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="ad654-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ad654-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="ad654-114">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="ad654-114">Requirements</span></span>

<span data-ttu-id="ad654-115">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl değerlendirmek için kullanabileceğiniz veri parametreleri koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="ad654-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="ad654-116">"AtLeast21" ilkemizi tek bir parametre gereksinimidir&mdash;minimum yaş.</span><span class="sxs-lookup"><span data-stu-id="ad654-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="ad654-117">Bir gereklilik uyguladığı `IAuthorizationRequirement`, boş işaretçi arabirim olduğu.</span><span class="sxs-lookup"><span data-stu-id="ad654-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="ad654-118">Parametreli minimum yaş gereksinimi gibi uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="ad654-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="ad654-119">Bir gereksinim veri veya özellikleri olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ad654-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="ad654-120">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="ad654-120">Authorization handlers</span></span>

<span data-ttu-id="ad654-121">Bir yetkilendirme işleyici gereksinimi ait özellikler değerlendirmesi için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="ad654-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="ad654-122">Yetkilendirme işleyici gereksinimlerine göre bir sağlanan değerlendirir `AuthorizationHandlerContext` erişim izin verilip verilmediğini belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="ad654-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="ad654-123">Bir gereksinim olabilir [birden çok işleyici](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="ad654-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="ad654-124">İşleyicileri devral `AuthorizationHandler<T>`, burada `T` işlenecek gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="ad654-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="ad654-125">Minimum yaş işleyici şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="ad654-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="ad654-126">Geçerli kullanıcı asıl bilinen ve güvenilir bir veren tarafından verildiği talep doğum tarihi sahipse önceki kod belirler.</span><span class="sxs-lookup"><span data-stu-id="ad654-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="ad654-127">Talep eksik olduğunda yetkilendirme yapılmaz, tamamlanmış bir görevi; bu durumda döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ad654-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="ad654-128">Bir talep mevcut olduğunda, kullanıcının yaşı hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="ad654-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="ad654-129">Kullanıcı tarafından ihtiyaç tanımlanan minimum yaş karşılıyorsa, Yetkilendirme başarılı kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="ad654-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="ad654-130">Yetkilendirme başarılı olduğunda `context.Succeed` memnun gereksinimiyle bir parametre olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ad654-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="ad654-131">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="ad654-131">Handler registration</span></span>

<span data-ttu-id="ad654-132">İşleyicileri yapılandırma sırasında Hizmetleri koleksiyonundaki kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ad654-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="ad654-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ad654-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="ad654-134">Harekete geçirerek services koleksiyonuna eklenen her işleyicisi `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="ad654-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="ad654-135">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="ad654-135">What should a handler return?</span></span>

<span data-ttu-id="ad654-136">Unutmayın `Handle` yönteminde [işleyici örnek](#security-authorization-handler-example) herhangi bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="ad654-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="ad654-137">Nasıl başarı veya başarısızlık belirtilen durumunu mi?</span><span class="sxs-lookup"><span data-stu-id="ad654-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="ad654-138">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirme başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="ad654-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="ad654-139">Diğer işleyicilerin aynı gereksinimi başarılı olabilir gibi hatalar genellikle işlemek bir işleyici gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ad654-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="ad654-140">Diğer gereksinim işleyicileri başarılı olsa bile hatası, güvence altına almak için çağrı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="ad654-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="ad654-141">Bir ilke gereksinim gerektirdiğinde işleyicinizi içinde çağrısı bağımsız olarak, bir gereksinim için tüm işleyiciler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ad654-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="ad654-142">Bu her zaman gerçekleşecek günlüğe kaydetme gibi yan etkileri gereksinimleri sağlar olsa bile `context.Fail()` içinde başka bir işleyici çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ad654-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="ad654-143">Bir gereksinim için birden çok işleyici neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="ad654-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="ad654-144">Değerlendirme üzerinde olmasını istediğiniz durumlarda bir **veya** olarak tek bir gereksinim için birden çok işleyici uygulayın.</span><span class="sxs-lookup"><span data-stu-id="ad654-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="ad654-145">Örneğin, Microsoft, yalnızca anahtar kartlarla açtığınız kapıları sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ad654-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="ad654-146">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapı açar.</span><span class="sxs-lookup"><span data-stu-id="ad654-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="ad654-147">Bu senaryoda, tek bir gereksinim yoktur *BuildingEntry*, ancak her biri bir tek gereksinim inceleniyor birden çok işleyici.</span><span class="sxs-lookup"><span data-stu-id="ad654-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="ad654-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="ad654-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="ad654-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="ad654-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="ad654-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="ad654-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="ad654-151">Her iki işleyicileri olduğundan emin olun [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="ad654-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="ad654-152">Bir ilke, her iki işleyici başarılı olursa değerlendirir `BuildingEntryRequirement`, ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="ad654-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="ad654-153">Bir ilke karşılamak üzere bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="ad654-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="ad654-154">Hangi gerçekleşmesine bir ilke kodda express basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="ad654-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="ad654-155">Tedarik mümkündür bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="ad654-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="ad654-156">Örneğin, önceki `BadgeEntryHandler` gibi yazılması:</span><span class="sxs-lookup"><span data-stu-id="ad654-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="ad654-157">MVC istek bağlamı işleyiciler erişme</span><span class="sxs-lookup"><span data-stu-id="ad654-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="ad654-158">`HandleRequirementAsync` Bir yetkilendirme işleyicisinde uygulaması yöntemi iki parametreye sahiptir: bir `AuthorizationHandlerContext` ve `TRequirement` işleme.</span><span class="sxs-lookup"><span data-stu-id="ad654-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="ad654-159">Çerçeveler MVC veya Jabbr gibi herhangi bir nesne eklemek ücretsiz `Resource` özellikte `AuthorizationHandlerContext` ek bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="ad654-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="ad654-160">Örneğin, MVC örneği geçirir [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) içinde `Resource` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ad654-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="ad654-161">Bu özellik erişim sağlayan `HttpContext`, `RouteData`ve başka MVC ve Razor sayfalarının tarafından sağlanan her şeyi.</span><span class="sxs-lookup"><span data-stu-id="ad654-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="ad654-162">Kullanımını `Resource` çerçeveye özgü bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ad654-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="ad654-163">Bilgileri kullanarak `Resource` özelliği, Yetkilendirme İlkeleri belirli çerçeveler için sınırlar.</span><span class="sxs-lookup"><span data-stu-id="ad654-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="ad654-164">Cast `Resource` özelliğini kullanarak `as` anahtar sözcüğü ve sahip dönüştürme başarılı olmayan kodunuzu kilitlenme emin olmak için onaylayın ile bir `InvalidCastException` diğer çerçeveler çalıştırdığınızda:</span><span class="sxs-lookup"><span data-stu-id="ad654-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
