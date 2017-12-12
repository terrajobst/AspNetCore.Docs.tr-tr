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
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="0f0b4-104">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="0f0b4-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="0f0b4-105">Kapak altında [rol yetkilendirme](roles.md) ve [yetkilendirme talep](claims.md) bir gereksinim kullanımını, gereksinim ve önceden yapılandırılmış bir ilke için bir işleyici olun.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="0f0b4-106">Bu yapı taşları yetkilendirme değerlendirmeleri kod, bir daha zengin için yeniden kullanılabilir, izin verme ve kolayca sınanabilir yetkilendirme yapısı express olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="0f0b4-107">Bir yetkilendirme ilkesi bir veya daha fazla gereksinimlerini oluşur ve yetkilendirme hizmet yapılandırmasının bir parçası kayıtlı uygulama başlangıcında `ConfigureServices` içinde *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="0f0b4-108">Burada "Over21" ilke en az bir yaşını, parametre olarak gereksinime geçtiğinden, tek bir gereksinim ile oluşturulan görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="0f0b4-109">İlkeleri kullanarak uygulanır `Authorize` ilke adını, örneğin; belirterek özniteliği</span><span class="sxs-lookup"><span data-stu-id="0f0b4-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="0f0b4-110">Gereksinimler</span><span class="sxs-lookup"><span data-stu-id="0f0b4-110">Requirements</span></span>

<span data-ttu-id="0f0b4-111">Bir yetkilendirme gereksinimi, bir ilke geçerli kullanıcı asıl değerlendirmek için kullanabileceğiniz veri parametreleri koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="0f0b4-112">Minimum yaş ilkemizi sahibiz tek bir parametre minimum yaş gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="0f0b4-113">Bir gereksinim uygulamalıdır `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="0f0b4-114">Bu bir boş bir işaretleyici arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-114">This is an empty, marker interface.</span></span> <span data-ttu-id="0f0b4-115">Parametreli minimum yaş gereksinimi gibi uygulanabilir;</span><span class="sxs-lookup"><span data-stu-id="0f0b4-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="0f0b4-116">Bir gereksinim veri veya özellikleri olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="0f0b4-117">Yetkilendirme işleyicileri</span><span class="sxs-lookup"><span data-stu-id="0f0b4-117">Authorization handlers</span></span>

<span data-ttu-id="0f0b4-118">Bir yetkilendirme işleyici herhangi bir gereksinim özelliklerini değerlendirmesi için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="0f0b4-119">Yetkilendirme işleyici tarafından sağlanan karşı değerlendirilmesi gerekir `AuthorizationHandlerContext` yetkilendirme izin verilip verilmediğini karar vermek için.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="0f0b4-120">Bir gereksinim olabilir [birden çok işleyici](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="0f0b4-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="0f0b4-121">İşleyicileri devralmalıdır `AuthorizationHandler<T>` T, işleme gereksinimi olduğu.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="0f0b4-122">Minimum yaş işleyici şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="0f0b4-122">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="0f0b4-123">Yukarıdaki kod biz öncelikle geçerli kullanıcı asıl biliyoruz bir veren ve güven tarafından verildiği talep doğum tarihi sahipse görmek için bakın.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="0f0b4-124">Talep eksikse, döndürürüz biz yetkisi kaldırılamıyor.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="0f0b4-125">Biz bir talep varsa, kullanıcının kaç yaşında olduğunu şekil ve minimum yaş gereksinimi tarafından geçirilen karşılıyorsa sonra Yetkilendirme başarılı olmuştur.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="0f0b4-126">Yetkilendirme başarılı olduktan sonra diyoruz `context.Succeed()` parametre olarak başarılı olmuştur gereksinim geçirme.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="0f0b4-127">İşleyici kaydı</span><span class="sxs-lookup"><span data-stu-id="0f0b4-127">Handler registration</span></span>
<span data-ttu-id="0f0b4-128">İşleyicileri yapılandırma sırasında Hizmetleri koleksiyonundaki örneğin kayıtlı olması gerekir;</span><span class="sxs-lookup"><span data-stu-id="0f0b4-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="0f0b4-129">Kullanarak Hizmetleri koleksiyonuna eklenen her işleyicisi `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` işleyici sınıfınızda geçirme.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="0f0b4-130">Ne bir işleyici döndürmelidir?</span><span class="sxs-lookup"><span data-stu-id="0f0b4-130">What should a handler return?</span></span>

<span data-ttu-id="0f0b4-131">Görebilirsiniz bizim [işleyici örnek](policies.md#security-authorization-handler-example) , `Handle()` yöntemi sahip hiçbir değer döndürmeyen nasıl yapmak biz belirtmek başarı veya başarısızlık?</span><span class="sxs-lookup"><span data-stu-id="0f0b4-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="0f0b4-132">Çağırarak bir işleyici başarı belirten `context.Succeed(IAuthorizationRequirement requirement)`, gereksinim geçirme başarıyla doğrulanmış.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="0f0b4-133">Diğer işleyicilerin aynı gereksinimi başarılı olabilir gibi hatalar genellikle işlemek bir işleyici gerekmez.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="0f0b4-134">Bir gereksinim için diğer işleyicilerin başarılı olsa bile hatası güvence altına almak için çağrı `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="0f0b4-135">Bir ilke gereksinim gerektirdiğinde işleyicinizi içinde çağrısı bağımsız olarak, bir gereksinim için tüm işleyiciler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="0f0b4-136">Bu her zaman gerçekleşecek günlüğe kaydetme gibi yan etkileri gereksinimleri sağlar olsa bile `context.Fail()` içinde başka bir işleyici çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="0f0b4-137">Bir gereksinim için birden çok işleyici neden kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="0f0b4-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="0f0b4-138">Değerlendirme üzerinde olmasını istediğiniz durumlarda bir **veya** olarak tek bir gereksinim için birden çok işleyici uygulamak.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="0f0b4-139">Örneğin, Microsoft, yalnızca anahtar kartlarla açtığınız kapıları sahiptir.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="0f0b4-140">Anahtar kartınız evde bırakırsanız resepsiyonist geçici bir etiket yazdırır ve kapı açar.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="0f0b4-141">Bu senaryoda, tek bir gereksinim yoktur *EnterBuilding*, ancak her biri bir tek gereksinim inceleniyor birden çok işleyici.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="0f0b4-142">Şimdi, her iki işleyicileri ediliyor [kayıtlı](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) bir ilke değerlendirirken `EnterBuildingRequirement` ya da işleyici başarılı olursa ilke değerlendirmesi başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="0f0b4-143">Bir ilke karşılamak üzere bir func kullanma</span><span class="sxs-lookup"><span data-stu-id="0f0b4-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="0f0b4-144">Bir ilke gerçekleşmesine kodda express basit olduğu durumlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="0f0b4-145">Yalnızca sağlamak olası bir `Func<AuthorizationHandlerContext, bool>` ilkenizle yapılandırırken `RequireAssertion` İlkesi Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="0f0b4-146">Örneğin önceki `BadgeEntryHandler` gibi yazılması:</span><span class="sxs-lookup"><span data-stu-id="0f0b4-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="0f0b4-147">MVC istek bağlamı işleyiciler erişme</span><span class="sxs-lookup"><span data-stu-id="0f0b4-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="0f0b4-148">`Handle` Bir yetkilendirme işleyicisinde uygulanmalı yöntemi iki parametreye sahip bir `AuthorizationContext` ve `Requirement` işleme.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="0f0b4-149">Çerçeveler MVC veya Jabbr gibi herhangi bir nesne eklemek ücretsiz `Resource` özellikte `AuthorizationContext` ek bilgi geçirmek için.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="0f0b4-150">Örneğin, MVC örneği geçirir `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` başka MVC HttpContext, RouteData ve her şeyi erişmek için kullanılan kaynak özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="0f0b4-151">Kullanımını `Resource` çerçeveye özgü bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="0f0b4-152">Bilgileri kullanarak `Resource` özelliği, yetkilendirme ilkelerini belirli çerçeveler için sınırlamak.</span><span class="sxs-lookup"><span data-stu-id="0f0b4-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="0f0b4-153">Cast `Resource` özelliğini kullanarak `as` anahtar sözcüğü ve ardından onay cast sahip başarılı olmayan kodunuzu kilitlenme emin olmak için birlikte `InvalidCastExceptions` diğer çerçeveler; çalıştırdığınızda</span><span class="sxs-lookup"><span data-stu-id="0f0b4-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
