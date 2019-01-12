---
title: ASP.NET core'da özel yetkilendirme ilkesi sağlayıcıları
author: mjrousos
description: Özel IAuthorizationPolicyProvider Yetkilendirme İlkeleri dinamik olarak oluşturmak için ASP.NET Core uygulamanızı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ef3e81da6fb9e2e332b553607be35fcd79e9362d
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249379"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="9bd53-103">Özel yetkilendirme ilkesi IAuthorizationPolicyProvider kullanarak ASP.NET Core sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="9bd53-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="9bd53-104">Tarafından [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="9bd53-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="9bd53-105">Genellikle kullanırken [ilke tabanlı yetkilendirme](xref:security/authorization/policies), ilkeleri çağırarak kayıtlı `AuthorizationOptions.AddPolicy` yetkilendirme hizmet yapılandırmasının bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="9bd53-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="9bd53-106">Bazı senaryolarda, bunu (veya istenmediğinde) olabileceği tüm yetkilendirme ilkeleri bu şekilde kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="9bd53-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="9bd53-107">Bu gibi durumlarda, özel bir kullanabileceğiniz `IAuthorizationPolicyProvider` Yetkilendirme İlkeleri nasıl sağlanan denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="9bd53-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="9bd53-108">Senaryo örnekleri özel durumlarda [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilecek içerir:</span><span class="sxs-lookup"><span data-stu-id="9bd53-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="9bd53-109">İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.</span><span class="sxs-lookup"><span data-stu-id="9bd53-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="9bd53-110">(İçin farklı bir oda numaralarını veya örnek şu yaşlardaki) birçok farklı ilkeleri kullanarak, bu nedenle, değil mantıklı her tek yetkilendirme ilkesi ile eklemek için bir `AuthorizationOptions.AddPolicy` çağırın.</span><span class="sxs-lookup"><span data-stu-id="9bd53-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="9bd53-111">Çalışma zamanında bir dış veri kaynağı (örneğin, bir veritabanı) içindeki bilgileri temel alan ilkeleri oluşturma veya yetkilendirme gereksinimleriyle başka bir mekanizma aracılığıyla dinamik olarak belirleme.</span><span class="sxs-lookup"><span data-stu-id="9bd53-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="9bd53-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/AuthSamples/) gelen [AspNetCore GitHub deposu](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="9bd53-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/AuthSamples/) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="9bd53-113">ASP.NET/AuthSamples depo ZIP dosyasını indirin.</span><span class="sxs-lookup"><span data-stu-id="9bd53-113">Download the aspnet/AuthSamples repository ZIP file.</span></span>
<span data-ttu-id="9bd53-114">Unzip *AuthSamples-master.zip* dosya.</span><span class="sxs-lookup"><span data-stu-id="9bd53-114">Unzip the *AuthSamples-master.zip* file.</span></span> <span data-ttu-id="9bd53-115">Gidin *samples/CustomPolicyProvider* proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="9bd53-115">Navigate to the *samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="9bd53-116">İlke alma işlemi özelleştirme</span><span class="sxs-lookup"><span data-stu-id="9bd53-116">Customize policy retrieval</span></span>

<span data-ttu-id="9bd53-117">ASP.NET Core uygulamaları kullanma uygulaması `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="9bd53-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="9bd53-118">Varsayılan olarak, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kayıtlı ve kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9bd53-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="9bd53-119">`DefaultAuthorizationPolicyProvider` ilkelerden döndürür `AuthorizationOptions` sağlanan bir `IServiceCollection.AddAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="9bd53-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="9bd53-120">Farklı bir'na kaydederek bu davranışı özelleştirebilirsiniz `IAuthorizationPolicyProvider` uygulamanın uygulamasında [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="9bd53-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="9bd53-121">`IAuthorizationPolicyProvider` Arabirimi iki API içerir:</span><span class="sxs-lookup"><span data-stu-id="9bd53-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="9bd53-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) belirtilen bir ad için bir yetkilendirme ilkesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="9bd53-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="9bd53-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) varsayılan yetkilendirme ilkesi döndürür (için kullanılan ilkeyi `[Authorize]` öznitelikleri belirtilen bir ilke olmadan).</span><span class="sxs-lookup"><span data-stu-id="9bd53-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="9bd53-124">Bu iki API uygulayarak, Yetkilendirme İlkeleri nasıl sağlanan özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9bd53-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="9bd53-125">Parametreli özniteliği örnek Yetkilendir</span><span class="sxs-lookup"><span data-stu-id="9bd53-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="9bd53-126">Bir senaryo burada `IAuthorizationPolicyProvider` yararlıdır özel etkinleştirme `[Authorize]` öznitelikleri, gereksinimleri bir parametresine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9bd53-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="9bd53-127">Örneğin, [ilke tabanlı yetkilendirme](xref:security/authorization/policies) belgeleri, ("ilke bir örnek olarak kullanılan bir yaş tabanlı AtLeast21").</span><span class="sxs-lookup"><span data-stu-id="9bd53-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="9bd53-128">Farklı denetleyici eylemleri bir uygulamada kullanıcıları için kullanılabilir olması durumunda *farklı* yaş, birçok farklı yaş tabanlı ilkeleri sahip olmak yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9bd53-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="9bd53-129">Uygulamanın gereksinim duyacağınız tüm farklı yaş tabanlı ilkelerinin kaydetme yerine `AuthorizationOptions`, özel bir ilkeleriyle dinamik olarak oluşturabileceğiniz `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9bd53-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9bd53-130">İlkeleri daha kolay hale getirmek için Eylemler gibi özel yetkilendirme özniteliğiyle açıklama ekleyebilirsiniz `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="9bd53-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="9bd53-131">Özel yetkilendirme öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="9bd53-131">Custom Authorization Attributes</span></span>

<span data-ttu-id="9bd53-132">Yetkilendirme İlkeleri, adlarına göre tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9bd53-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="9bd53-133">Özel `MinimumAgeAuthorizeAttribute` açıklanan daha önce bağımsız değişkenleri karşılık gelen bir yetkilendirme ilkesi almak için kullanılan bir dizeye eşlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9bd53-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="9bd53-134">Bunu türeterek yapmak `AuthorizeAttribute` ve yaparak `Age` özelliği kaydırma `AuthorizeAttribute.Policy` özelliği.</span><span class="sxs-lookup"><span data-stu-id="9bd53-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="9bd53-135">Bu öznitelik türü sahip bir `Policy` dize sabit kodlanmış ön ekini temel alarak (`"MinimumAge"`) ve bir tamsayı geçirilen Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="9bd53-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="9bd53-136">Aynı şekilde diğer eylemler için geçerli `Authorize` dışında bir parametre olarak bir tamsayı sürer, öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="9bd53-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9bd53-137">Özel IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="9bd53-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9bd53-138">Özel `MinimumAgeAuthorizeAttribute` gerekli en düşük yaş için isteği Yetkilendirme İlkeleri kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="9bd53-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="9bd53-139">Sonraki sorunu çözmek için Yetkilendirme İlkeleri tüm bu farklı yaş için kullanılabildiğinden emin yapıyor.</span><span class="sxs-lookup"><span data-stu-id="9bd53-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="9bd53-140">Burada bir `IAuthorizationPolicyProvider` yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9bd53-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="9bd53-141">Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları deseni takip `"MinimumAge" + Age`, bu nedenle özel `IAuthorizationPolicyProvider` tarafından yetkilendirme ilkeleri oluşturmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="9bd53-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="9bd53-142">İlke adı yaş ayrıştırılıyor.</span><span class="sxs-lookup"><span data-stu-id="9bd53-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="9bd53-143">Kullanarak `AuthorizationPolicyBuilder` yeni oluşturmak için `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="9bd53-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="9bd53-144">Gereksinimleri ilkeye eklemeyi temel ile yaş `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="9bd53-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="9bd53-145">Diğer senaryolarda kullanabileceğinize `RequireClaim`, `RequireRole`, veya `RequireUserName` yerine.</span><span class="sxs-lookup"><span data-stu-id="9bd53-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="9bd53-146">Birden çok yetkilendirme ilkesi sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="9bd53-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="9bd53-147">Özel kullanırken `IAuthorizationPolicyProvider` uygulamaları ASP.NET Core, yalnızca bir örneği kullanan aklınızda tutun `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9bd53-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9bd53-148">Özel bir sağlayıcı için tüm ilke adları Yetkilendirme İlkeleri sağlayamadığı ise yeniden bir yedekleme sağlayıcısına içerilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9bd53-148">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="9bd53-149">İlke adları için varsayılan ilkesi gelenleri içerebilir `[Authorize]` adı olmayan bir öznitelik.</span><span class="sxs-lookup"><span data-stu-id="9bd53-149">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="9bd53-150">Örneğin, bir uygulamanın özel yaşı ilkeleri hem daha geleneksel rol tabanlı ilke alımı gerekli göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="9bd53-150">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="9bd53-151">Böyle bir uygulamayı bir özel yetkilendirme ilkesi sağlayıcısı kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="9bd53-151">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="9bd53-152">İlke adları ayrıştırmayı dener.</span><span class="sxs-lookup"><span data-stu-id="9bd53-152">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="9bd53-153">Farklı bir ilke sağlayıcısı çağırıyor (gibi `DefaultAuthorizationPolicyProvider`) ilke adı bir yaş içermiyorsa.</span><span class="sxs-lookup"><span data-stu-id="9bd53-153">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="9bd53-154">Varsayılan ilke</span><span class="sxs-lookup"><span data-stu-id="9bd53-154">Default policy</span></span>

<span data-ttu-id="9bd53-155">Adlandırılmış Yetkilendirme İlkeleri, özel bir sağlama yanı sıra `IAuthorizationPolicyProvider` uygulamak gereken `GetDefaultPolicyAsync` sağlamak için bir yetkilendirme ilkesi için `[Authorize]` belirtilen bir ilke adı olmadan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="9bd53-155">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="9bd53-156">Çoğu durumda, gerekli ilke çağrısıyla verebilmeniz için bu yetkilendirme öznitelik yalnızca kimliği doğrulanmış bir kullanıcı gerektirir. `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="9bd53-156">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="9bd53-157">Özel bir tüm yönlerini olduğu gibi `IAuthorizationPolicyProvider`, bu, gerektiği şekilde özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9bd53-157">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="9bd53-158">Bazı durumlarda:</span><span class="sxs-lookup"><span data-stu-id="9bd53-158">In some cases:</span></span>

* <span data-ttu-id="9bd53-159">Varsayılan Yetkilendirme İlkeleri kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="9bd53-159">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="9bd53-160">Varsayılan ilkeyi almak için bir geri dönüş aktarılabileceği `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9bd53-160">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9bd53-161">Özel IAuthorizationPolicyProvider kullanın</span><span class="sxs-lookup"><span data-stu-id="9bd53-161">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9bd53-162">Özel ilkeleri'nden kullanmak için bir `IAuthorizationPolicyProvider`, şunları yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="9bd53-162">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="9bd53-163">Uygun kaydetme `AuthorizationHandler` bağımlılık ekleme türleriyle (açıklanan [ilke tabanlı yetkilendirme](xref:security/authorization/policies#authorization-handlers)), tüm ilke tabanlı yetkilendirme senaryoları gibi.</span><span class="sxs-lookup"><span data-stu-id="9bd53-163">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="9bd53-164">Özel kayıt `IAuthorizationPolicyProvider` uygulamanın bağımlılık ekleme hizmeti koleksiyon türü (içinde `Startup.ConfigureServices`) varsayılan ilke sağlayıcısı değiştirilecek.</span><span class="sxs-lookup"><span data-stu-id="9bd53-164">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="9bd53-165">Tam bir özel `IAuthorizationPolicyProvider` örnek kullanılabilir [aspnet/AuthSamples GitHub deposu](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="9bd53-165">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span></span>
