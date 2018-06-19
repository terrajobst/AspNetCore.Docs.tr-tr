---
title: ASP.NET Core özel yetkilendirme ilkesi sağlayıcıları
author: mjrousos
description: Dinamik olarak yetkilendirme ilkeleri oluşturmak için bir ASP.NET Core uygulamada özel IAuthorizationPolicyProvider kullanmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233449"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="f64a1-103">Özel yetkilendirme ilkesi IAuthorizationPolicyProvider ASP.NET Core kullanarak sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="f64a1-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="f64a1-104">Tarafından [CAN Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="f64a1-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="f64a1-105">Genellikle kullanırken [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies), ilkeleri çağırarak kayıtlı `AuthorizationOptions.AddPolicy` yetkilendirme hizmet yapılandırmasının bir parçası olarak.</span><span class="sxs-lookup"><span data-stu-id="f64a1-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="f64a1-106">Bazı senaryolarda bu olası (veya istenmediğinde) olmayabilir tüm yetkilendirme ilkeleri bu şekilde kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="f64a1-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="f64a1-107">Bu gibi durumlarda, özel bir kullanabilirsiniz `IAuthorizationPolicyProvider` Yetkilendirme İlkeleri nasıl sağlanan denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="f64a1-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="f64a1-108">Aşağıda örnek senaryolar özel bir yerde [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilir içerir:</span><span class="sxs-lookup"><span data-stu-id="f64a1-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="f64a1-109">İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.</span><span class="sxs-lookup"><span data-stu-id="f64a1-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="f64a1-110">İlkeleri büyük bir dizi (farklı oda numaralarını veya örneğin yaş) kullanarak, bu nedenle değil kolaylaştırır her bireysel yetkilendirme ilkesi ile eklemek için anlamlı bir `AuthorizationOptions.AddPolicy` çağırın.</span><span class="sxs-lookup"><span data-stu-id="f64a1-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="f64a1-111">Dış veri kaynağı (örneğin, bir veritabanı) içindeki bilgileri temel çalışma zamanında ilkeleri oluşturma veya başka bir mekanizma yetkilendirme gereksinimleri dinamik olarak belirleme.</span><span class="sxs-lookup"><span data-stu-id="f64a1-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="f64a1-112">İlke alma işlemini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f64a1-112">Customizing policy retrieval</span></span>

<span data-ttu-id="f64a1-113">ASP.NET Core uygulamaları kullanma uygulaması `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="f64a1-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="f64a1-114">Varsayılan olarak, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kayıtlı ve kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f64a1-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="f64a1-115">`DefaultAuthorizationPolicyProvider` ilkelerden döndürür `AuthorizationOptions` sağlanan bir `IServiceCollection.AddAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="f64a1-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="f64a1-116">Bu davranış farklı bir kaydederek özelleştirebileceğiniz `IAuthorizationPolicyProvider` uygulamanın uygulamasında [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="f64a1-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="f64a1-117">`IAuthorizationPolicyProvider` Arabirimi iki API'leri içerir:</span><span class="sxs-lookup"><span data-stu-id="f64a1-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="f64a1-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) bir verilen ad için bir yetkilendirme ilkesi döndürür.</span><span class="sxs-lookup"><span data-stu-id="f64a1-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="f64a1-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) varsayılan yetkilendirme ilkesi döndürür (için kullanılan ilkeyi `[Authorize]` öznitelikleri belirtilen bir ilke olmadan).</span><span class="sxs-lookup"><span data-stu-id="f64a1-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="f64a1-120">Bu iki API uygulayarak, Yetkilendirme İlkeleri nasıl sağlanır özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f64a1-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="f64a1-121">Parametreli özniteliği örnek yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="f64a1-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="f64a1-122">Bir senaryo nerede `IAuthorizationPolicyProvider` yararlıdır özel etkinleştirme `[Authorize]` öznitelikleri olan gereksinimleri bir parametresine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f64a1-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="f64a1-123">Örneğin, [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies) belgeleri ("ilke bir örnek olarak kullanılan bir yaş tabanlı AtLeast21").</span><span class="sxs-lookup"><span data-stu-id="f64a1-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="f64a1-124">Bir uygulamada farklı denetleyici eylemleri kullanıcıları için kullanılabilir olması durumunda *farklı* yaş, birçok farklı yaş tabanlı ilkeleri yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f64a1-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="f64a1-125">Uygulamanın gereksinim duyacağınız tüm farklı yaş tabanlı ilkeleri kaydetme yerine `AuthorizationOptions`, özel bir ilkeleriyle dinamik olarak oluşturabilir `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f64a1-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f64a1-126">İlkelerle daha kolay hale getirmek için Eylemler gibi özel yetkilendirme özniteliğiyle açıklama ekleyebilir `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="f64a1-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="f64a1-127">Özel yetkilendirme öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="f64a1-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="f64a1-128">Yetkilendirme İlkeleri, adlarına göre tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="f64a1-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="f64a1-129">Özel `MinimumAgeAuthorizeAttribute` açıklanan bağımsız değişkenleri karşılık gelen yetkilendirme ilkesini almak için kullanılan bir dizeye eşlemek daha önce gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="f64a1-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="f64a1-130">Bunu türetme tarafından yapmak `AuthorizeAttribute` ve yaparak `Age` özelliği kaydırma `AuthorizeAttribute.Policy` özelliği.</span><span class="sxs-lookup"><span data-stu-id="f64a1-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
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

<span data-ttu-id="f64a1-131">Bu öznitelik türüne sahip bir `Policy` dize sabit kodlanmış ön ekini temel alarak (`"MinimumAge"`) ve bir tamsayı geçirilen Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="f64a1-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="f64a1-132">Aynı şekilde diğer eylemler için geçerli `Authorize` tamsayı bir parametre olarak alır ancak bu öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f64a1-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f64a1-133">Özel IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="f64a1-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f64a1-134">Özel `MinimumAgeAuthorizeAttribute` istenen minimum yaş için isteği Yetkilendirme İlkeleri kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="f64a1-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="f64a1-135">Sonraki sorunu çözmek için Yetkilendirme İlkeleri tüm bu farklı yaş için kullanılabilir olduğundan emin olmak.</span><span class="sxs-lookup"><span data-stu-id="f64a1-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="f64a1-136">Bu yerdir bir `IAuthorizationPolicyProvider` yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="f64a1-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="f64a1-137">Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları deseni izler `"MinimumAge" + Age`, bu nedenle özel `IAuthorizationPolicyProvider` yetkilendirme ilkeleri tarafından oluşturmak:</span><span class="sxs-lookup"><span data-stu-id="f64a1-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="f64a1-138">İlke adı yaş ayrıştırma.</span><span class="sxs-lookup"><span data-stu-id="f64a1-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="f64a1-139">Kullanarak `AuthorizationPolicyBuiler` yeni oluşturmak için `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="f64a1-139">Using `AuthorizationPolicyBuiler` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="f64a1-140">İlkeyi gereksinimler ekleme temel geçerlilik süresi ile `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="f64a1-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="f64a1-141">Diğer senaryolarda kullanabilirsiniz `RequireClaim`, `RequireRole`, veya `RequireUserName` yerine.</span><span class="sxs-lookup"><span data-stu-id="f64a1-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="f64a1-142">Birden çok yetkilendirme ilkesi sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="f64a1-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="f64a1-143">Özel kullanırken `IAuthorizationPolicyProvider` uygulamaları ASP.NET Core yalnızca bir örneğini kullanan göz önünde bulundurun `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f64a1-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f64a1-144">Özel bir sağlayıcı için tüm ilke adları Yetkilendirme İlkeleri sağlamak mümkün değilse, bir yedekleme sağlayıcısına geri girecektir.</span><span class="sxs-lookup"><span data-stu-id="f64a1-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="f64a1-145">İlke adları için varsayılan bir ilke alınması o içerebilir `[Authorize]` bir ad olmadan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f64a1-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="f64a1-146">Örneğin, bir uygulamanın özel yaşı ilkeleri ve daha geleneksel rol tabanlı ilke alma işlemi gerekli göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f64a1-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="f64a1-147">Bu tür bir uygulama bir özel yetkilendirme ilkesi sağlayıcısı kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f64a1-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="f64a1-148">İlke adları ayrıştırmak çalışır.</span><span class="sxs-lookup"><span data-stu-id="f64a1-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="f64a1-149">Farklı bir ilke sağlayıcısı çağrılar (gibi `DefaultAuthorizationPolicyProvider`) ilkesi adı bir yaş içermiyorsa.</span><span class="sxs-lookup"><span data-stu-id="f64a1-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="f64a1-150">Varsayılan ilke</span><span class="sxs-lookup"><span data-stu-id="f64a1-150">Default policy</span></span>

<span data-ttu-id="f64a1-151">Adlandırılmış Yetkilendirme İlkeleri, özel bir sağlama yanı sıra `IAuthorizationPolicyProvider` uygulamak gereken `GetDefaultPolicyAsync` için bir yetkilendirme ilkesi sağlamak için `[Authorize]` belirtilen bir ilke adı olmadan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="f64a1-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="f64a1-152">Gerekli ilke çağrısıyla olabilmeniz çoğu durumda, bu yetkilendirme öznitelik kimliği doğrulanmış bir kullanıcı yalnızca gerektirir. `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="f64a1-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="f64a1-153">Özel bir tüm yönlerini olduğu gibi `IAuthorizationPolicyProvider`, bu, gerektiği gibi özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f64a1-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="f64a1-154">Bazı durumlarda:</span><span class="sxs-lookup"><span data-stu-id="f64a1-154">In some cases:</span></span>

* <span data-ttu-id="f64a1-155">Varsayılan Yetkilendirme İlkeleri kullanılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="f64a1-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="f64a1-156">Varsayılan ilkeyi almak için bir geri dönüş aktarılabileceği `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f64a1-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f64a1-157">Özel IAuthorizationPolicyProvider kullanma</span><span class="sxs-lookup"><span data-stu-id="f64a1-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f64a1-158">Özel ilkelerden kullanmak için bir `IAuthorizationPolicyProvider`, şunları yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="f64a1-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="f64a1-159">Uygun kaydetmek `AuthorizationHandler` bağımlılık ekleme türleriyle (açıklanan [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies#authorization-handlers)), tüm ilke tabanlı bir yetkilendirme senaryoları gibi.</span><span class="sxs-lookup"><span data-stu-id="f64a1-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="f64a1-160">Özel kayıt `IAuthorizationPolicyProvider` uygulamanın bağımlılık ekleme hizmeti koleksiyonundaki türü (içinde `Startup.ConfigureServices`) varsayılan ilke sağlayıcısı değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="f64a1-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="f64a1-161">Tam bir özel `IAuthorizationPolicyProvider` örnek sağlanmıştır [aspnet/AuthSamples GitHub deposunu](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="f64a1-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
