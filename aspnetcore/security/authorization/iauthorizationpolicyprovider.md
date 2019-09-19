---
title: ASP.NET Core 'de özel yetkilendirme Ilkesi sağlayıcıları
author: mjrousos
description: Bir ASP.NET Core uygulamasında özel bir ıauthorizationpolicyprovider kullanarak yetkilendirme ilkelerini dinamik olarak oluşturma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108062"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="b56f3-103">ASP.NET Core 'de ıauthorizationpolicyprovider kullanan özel yetkilendirme Ilkesi sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b56f3-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="b56f3-104">, [Mike Rousos](https://github.com/mjrousos) tarafından</span><span class="sxs-lookup"><span data-stu-id="b56f3-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="b56f3-105">Genellikle [ilke tabanlı yetkilendirme](xref:security/authorization/policies)kullanılırken, ilkeler, yetkilendirme hizmeti yapılandırmasının bir parçası `AuthorizationOptions.AddPolicy` olarak çağırarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="b56f3-106">Bazı senaryolarda, tüm yetkilendirme ilkelerini bu şekilde kaydetmek mümkün olmayabilir (veya istenebilir).</span><span class="sxs-lookup"><span data-stu-id="b56f3-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="b56f3-107">Bu durumlarda, yetkilendirme ilkelerinin nasıl sağlanmış olduğunu denetlemek `IAuthorizationPolicyProvider` için özel ' i kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="b56f3-108">Özel bir [ıauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilecek senaryolara örnek olarak şunlar verilebilir:</span><span class="sxs-lookup"><span data-stu-id="b56f3-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="b56f3-109">İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.</span><span class="sxs-lookup"><span data-stu-id="b56f3-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="b56f3-110">Büyük bir ilke yelpazesi (örneğin, farklı oda numaraları veya yaşlar için) kullanarak, her bireysel yetkilendirme ilkesini bir `AuthorizationOptions.AddPolicy` çağrısıyla eklemek mantıklı değildir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="b56f3-111">Dış veri kaynağındaki bilgilere (bir veritabanı gibi) göre veya başka bir mekanizmaya göre yetkilendirme gereksinimlerini dinamik olarak belirleyerek çalışma zamanında ilkeler oluşturma.</span><span class="sxs-lookup"><span data-stu-id="b56f3-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="b56f3-112">[Aspnetcore GitHub deposundan](https://github.com/aspnet/AspNetCore) [örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) .</span><span class="sxs-lookup"><span data-stu-id="b56f3-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="b56f3-113">ASPNET/AspNetCore depo ZIP dosyasını indirin.</span><span class="sxs-lookup"><span data-stu-id="b56f3-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="b56f3-114">Dosyayı sıkıştırmayı açın.</span><span class="sxs-lookup"><span data-stu-id="b56f3-114">Unzip the file.</span></span> <span data-ttu-id="b56f3-115">*Src/Security/Samples/CustomPolicyProvider* proje klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="b56f3-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="b56f3-116">İlke almayı özelleştirme</span><span class="sxs-lookup"><span data-stu-id="b56f3-116">Customize policy retrieval</span></span>

<span data-ttu-id="b56f3-117">ASP.NET Core uygulamalar, `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirimin bir uygulamasını kullanır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="b56f3-118">Varsayılan olarak, [Defaultauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kaydedilir ve kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="b56f3-119">`DefaultAuthorizationPolicyProvider``AuthorizationOptions` bir`IServiceCollection.AddAuthorization` çağrıda sunulan ilkeleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="b56f3-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="b56f3-120">Uygulamanın `IAuthorizationPolicyProvider` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına farklı bir uygulama kaydederek bu davranışı özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="b56f3-121">`IAuthorizationPolicyProvider` Arabirim iki API içerir:</span><span class="sxs-lookup"><span data-stu-id="b56f3-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="b56f3-122">[Getpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) , belirli bir ad için yetkilendirme ilkesi döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="b56f3-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="b56f3-123">[Getdefaultpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) varsayılan yetkilendirme ilkesini döndürür (bir ilke belirtilmeden öznitelikler için `[Authorize]` kullanılan ilke).</span><span class="sxs-lookup"><span data-stu-id="b56f3-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="b56f3-124">Bu iki API 'yi uygulayarak, yetkilendirme ilkelerinin nasıl sağlandığını özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="b56f3-125">Parametreli yetkilendirme özniteliği örneği</span><span class="sxs-lookup"><span data-stu-id="b56f3-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="b56f3-126">Tek senaryo `IAuthorizationPolicyProvider` , gereksinimleri bir parametreye bağlı özel `[Authorize]` öznitelikleri etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="b56f3-127">Örneğin, [ilke tabanlı yetkilendirme](xref:security/authorization/policies) belgelerinde, örnek olarak yaş tabanlı ("AtLeast21") ilkesi kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="b56f3-128">Bir uygulamadaki farklı denetleyici eylemleri *farklı* yaştaki kullanıcılar için kullanılabilir hale getirilse, çok sayıda farklı yaş tabanlı ilke olması faydalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="b56f3-129">Uygulamanın ihtiyaç duyduğu `AuthorizationOptions`tüm farklı yaş tabanlı ilkeleri kaydetmek yerine, ilkeleri özel `IAuthorizationPolicyProvider`olarak dinamik bir şekilde oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="b56f3-130">İlkeleri kullanmak daha kolay hale getirmek için, gibi `[MinimumAgeAuthorize(20)]`özel yetkilendirme özniteliğiyle eylemlere not ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="b56f3-131">Özel yetkilendirme öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="b56f3-131">Custom Authorization attributes</span></span>

<span data-ttu-id="b56f3-132">Yetkilendirme ilkeleri adlarıyla tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="b56f3-133">Daha önce `MinimumAgeAuthorizeAttribute` açıklanan özel, bağımsız değişkenleri ilgili yetkilendirme ilkesini almak için kullanılabilecek bir dizeye eşlemenize ihtiyaç duyuyor.</span><span class="sxs-lookup"><span data-stu-id="b56f3-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="b56f3-134">Bunu öğesinden `AuthorizeAttribute` türeterek ve özelliği sararak `Age` `AuthorizeAttribute.Policy` yaparak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="b56f3-135">Bu öznitelik türü, sabit `Policy` kodlanmış ön eki (`"MinimumAge"`) ve Oluşturucu aracılığıyla geçirilen bir tamsayıyı temel alan bir dizeye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="b56f3-136">Bir parametre olarak bir tamsayı almak dışında, diğer `Authorize` özniteliklerle aynı şekilde eylemlere uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="b56f3-137">Özel ıauthorizationpolicyprovider</span><span class="sxs-lookup"><span data-stu-id="b56f3-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="b56f3-138">Özel `MinimumAgeAuthorizeAttribute` , istenen en düşük yaş için yetkilendirme ilkeleri istemeyi kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="b56f3-139">Çözecek bir sonraki sorun, yetkilendirme ilkelerinin tüm bu farklı yaşlar için kullanılabilir olduğundan emin olmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b56f3-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="b56f3-140">Bunun yararlı olduğu yerdir `IAuthorizationPolicyProvider` .</span><span class="sxs-lookup"><span data-stu-id="b56f3-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="b56f3-141">Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları, bu `"MinimumAge" + Age`sayede, özel `IAuthorizationPolicyProvider` olarak yetkilendirme ilkeleri oluşturması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="b56f3-142">İlke adından yaş ayrıştırılıyor.</span><span class="sxs-lookup"><span data-stu-id="b56f3-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="b56f3-143">Yeni `AuthorizationPolicyBuilder` bir oluşturmak için kullanarak`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="b56f3-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="b56f3-144">Bu ve aşağıdaki örneklerde, kullanıcının kimliğinin bir tanımlama bilgisi aracılığıyla doğrulandığını varsayacaktır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="b56f3-145">`AuthorizationPolicyBuilder` En az bir yetkilendirme düzeni adıyla oluşturulmalıdır ya da her zaman başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="b56f3-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="b56f3-146">Aksi takdirde, kullanıcıya nasıl bir sınama sağlayacaksınız ve bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b56f3-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="b56f3-147">Yaş temelinde ilkeye gereksinimler ekleme `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="b56f3-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="b56f3-148">Diğer senaryolarda, veya `RequireClaim` `RequireUserName` yerine kullanabilirsiniz `RequireRole`.</span><span class="sxs-lookup"><span data-stu-id="b56f3-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="b56f3-149">Birden çok yetkilendirme ilkesi sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="b56f3-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="b56f3-150">Özel `IAuthorizationPolicyProvider` uygulamalar kullanırken ASP.NET Core yalnızca bir `IAuthorizationPolicyProvider`örneğini kullandığını aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="b56f3-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="b56f3-151">Özel bir sağlayıcı kullanılacak tüm ilke adları için yetkilendirme ilkeleri sağlayamazsa, bir yedekleme sağlayıcısına geri dönecektir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="b56f3-152">Örneğin, özel yaş ilkelerine ve daha geleneksel rol tabanlı ilke almaya ihtiyacı olan bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="b56f3-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="b56f3-153">Bu tür bir uygulama, özel bir yetkilendirme ilkesi sağlayıcısını şu şekilde kullanabilir:</span><span class="sxs-lookup"><span data-stu-id="b56f3-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="b56f3-154">İlke adlarını ayrıştırmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="b56f3-155">İlke adı bir yaş içermiyorsa farklı bir ilke `DefaultAuthorizationPolicyProvider`sağlayıcısına (gibi) çağrı yapılır.</span><span class="sxs-lookup"><span data-stu-id="b56f3-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="b56f3-156">Yukarıda gösterilen `IAuthorizationPolicyProvider` örnek uygulama, `DefaultAuthorizationPolicyProvider` Oluşturucusu içinde bir geri dönüş ilkesi oluşturma (ilke adının beklenen ' minimum umage ' ve Age ile eşleşmemesi durumunda kullanılmak üzere) kullanılarak kullanılmak üzere güncelleştirilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="b56f3-157">Daha sonra, `GetPolicyAsync` yöntemi null döndürmek `FallbackPolicyProvider` yerine öğesini kullanacak şekilde yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b56f3-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="b56f3-158">Varsayılan ilke</span><span class="sxs-lookup"><span data-stu-id="b56f3-158">Default policy</span></span>

<span data-ttu-id="b56f3-159">Adlandırılmış Yetkilendirme İlkeleri sağlamaya ek olarak, bir ilke adı `IAuthorizationPolicyProvider` belirtmeden öznitelikler için `GetDefaultPolicyAsync` `[Authorize]` bir yetkilendirme ilkesi sağlamak üzere özel bir uygulama gerekir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="b56f3-160">Çoğu durumda, bu yetkilendirme özniteliği yalnızca kimliği doğrulanmış bir kullanıcı gerektirir, bu nedenle gerekli ilkeyi öğesine `RequireAuthenticatedUser`bir çağrı ile yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b56f3-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="b56f3-161">Özel `IAuthorizationPolicyProvider`bir tüm yönlere göre, bunu gerektiği şekilde özelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b56f3-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="b56f3-162">Bazı durumlarda, varsayılan ilkeyi bir geri `IAuthorizationPolicyProvider`yüklemeden almak istenebilir.</span><span class="sxs-lookup"><span data-stu-id="b56f3-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="b56f3-163">Gerekli ilke</span><span class="sxs-lookup"><span data-stu-id="b56f3-163">Required policy</span></span>

<span data-ttu-id="b56f3-164">İçin uygulanması `IAuthorizationPolicyProvider` `GetRequiredPolicyAsync` gereken özel bir, isteğe bağlı olarak her zaman gerekli bir ilke sağlar.</span><span class="sxs-lookup"><span data-stu-id="b56f3-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="b56f3-165">Null olmayan bir ilke döndürürse,builkeistenendiğer(adlandırılmışveyavarsayılan)ilkeylebirleştirilir.`GetRequiredPolicyAsync`</span><span class="sxs-lookup"><span data-stu-id="b56f3-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="b56f3-166">Gerekli bir ilke gerekmiyorsa, sağlayıcı yalnızca null döndürebilir veya geri dönüş sağlayıcısına erteleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b56f3-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="b56f3-167">Özel bir ıauthorizationpolicyprovider kullanın</span><span class="sxs-lookup"><span data-stu-id="b56f3-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="b56f3-168">Özel ilkeleri bir `IAuthorizationPolicyProvider`ile kullanmak için şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="b56f3-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="b56f3-169">İlke tabanlı yetkilendirme `AuthorizationHandler` senaryolarında olduğu gibi, bağımlılık ekleme ( [ilke tabanlı yetkilendirme](xref:security/authorization/policies#authorization-handlers)bölümünde açıklanmıştır) ile uygun türleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b56f3-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="b56f3-170">Varsayılan ilke sağlayıcısını `IAuthorizationPolicyProvider` değiştirmek için uygulamanın bağımlılık ekleme hizmeti koleksiyonuna (içine `Startup.ConfigureServices`) özel türü kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b56f3-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="b56f3-171">Tüm özel `IAuthorizationPolicyProvider` bir örnek, [ASPNET/authsamples GitHub deposunda](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)bulunur.</span><span class="sxs-lookup"><span data-stu-id="b56f3-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
