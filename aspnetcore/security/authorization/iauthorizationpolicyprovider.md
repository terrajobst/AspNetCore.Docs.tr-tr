---
title: ASP.NET Core 'de özel yetkilendirme Ilkesi sağlayıcıları
author: mjrousos
description: Bir ASP.NET Core uygulamasında özel bir ıauthorizationpolicyprovider kullanarak yetkilendirme ilkelerini dinamik olarak oluşturma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: fe07a113a29ed3e14679e3f3f2249b0810c17593
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880702"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>ASP.NET Core 'de ıauthorizationpolicyprovider kullanan özel yetkilendirme Ilkesi sağlayıcıları 

, [Mike Rousos](https://github.com/mjrousos) tarafından

Genellikle [ilke tabanlı yetkilendirme](xref:security/authorization/policies)kullanılırken, ilkeler, yetkilendirme hizmeti yapılandırmasının bir parçası olarak `AuthorizationOptions.AddPolicy` çağırarak kaydedilir. Bazı senaryolarda, tüm yetkilendirme ilkelerini bu şekilde kaydetmek mümkün olmayabilir (veya istenebilir). Bu durumlarda, yetkilendirme ilkelerinin nasıl sağlanmış olduğunu denetlemek için özel bir `IAuthorizationPolicyProvider` kullanabilirsiniz.

Özel bir [ıauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilecek senaryolara örnek olarak şunlar verilebilir:

* İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.
* Büyük bir ilke yelpazesi (örneğin, farklı oda numaraları veya yaşlar için) kullanarak, her bireysel yetkilendirme ilkesini bir `AuthorizationOptions.AddPolicy` çağrısıyla eklemek mantıklı değildir.
* Dış veri kaynağındaki bilgilere (bir veritabanı gibi) göre veya başka bir mekanizmaya göre yetkilendirme gereksinimlerini dinamik olarak belirleyerek çalışma zamanında ilkeler oluşturma.

[Aspnetcore GitHub deposundan](https://github.com/aspnet/AspNetCore) [örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) . ASPNET/AspNetCore depo ZIP dosyasını indirin. Dosyayı sıkıştırmayı açın. *Src/Security/Samples/CustomPolicyProvider* proje klasörüne gidin.

## <a name="customize-policy-retrieval"></a>İlke almayı özelleştirme

ASP.NET Core uygulamalar, yetkilendirme ilkelerini almak için `IAuthorizationPolicyProvider` arabiriminin bir uygulamasını kullanır. Varsayılan olarak, [Defaultauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kaydedilir ve kullanılır. `DefaultAuthorizationPolicyProvider`, `IServiceCollection.AddAuthorization` çağrısında sunulan `AuthorizationOptions` ilkeleri döndürür.

Uygulamanın [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına farklı bir `IAuthorizationPolicyProvider` uygulaması kaydederek bu davranışı özelleştirin. 

`IAuthorizationPolicyProvider` arabirimi üç API içerir:

* [Getpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) , belirli bir ad için yetkilendirme ilkesi döndürüyor.
* [Getdefaultpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) varsayılan yetkilendirme ilkesini döndürür (ilke belirtilmeden `[Authorize]` öznitelikleri için kullanılan ilke). 
* [Getfallbackpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) , geri dönüş yetkilendirme ilkesini döndürür (Ilke belirtilmediğinde yetkilendirme ara yazılımı tarafından kullanılan ilke). 

Bu API 'Leri uygulayarak, yetkilendirme ilkelerinin nasıl sağlandığını özelleştirebilirsiniz.

## <a name="parameterized-authorize-attribute-example"></a>Parametreli yetkilendirme özniteliği örneği

`IAuthorizationPolicyProvider` yararlı olduğu bir senaryo, gereksinimleri bir parametreye bağlı olan özel `[Authorize]` özniteliklerinin etkinleştirilmesini sağlar. Örneğin, [ilke tabanlı yetkilendirme](xref:security/authorization/policies) belgelerinde, örnek olarak yaş tabanlı ("AtLeast21") ilkesi kullanılmıştır. Bir uygulamadaki farklı denetleyici eylemleri *farklı* yaştaki kullanıcılar için kullanılabilir hale getirilse, çok sayıda farklı yaş tabanlı ilke olması faydalı olabilir. Uygulamanın `AuthorizationOptions`ihtiyaç duyduğu tüm farklı yaş tabanlı ilkeleri kaydetmek yerine, ilkeleri özel bir `IAuthorizationPolicyProvider`dinamik olarak oluşturabilirsiniz. İlkeleri kullanmak daha kolay hale getirmek için `[MinimumAgeAuthorize(20)]`gibi özel yetkilendirme özniteliğiyle eylemlere not ekleyebilirsiniz.

## <a name="custom-authorization-attributes"></a>Özel yetkilendirme öznitelikleri

Yetkilendirme ilkeleri adlarıyla tanımlanır. Daha önce açıklanan özel `MinimumAgeAuthorizeAttribute` bağımsız değişkenlerin, ilgili yetkilendirme ilkesini almak için kullanılabilecek bir dizeye eşlenmesi gerekir. Bunu, `AuthorizeAttribute` türeterek ve `Age` özelliğinin `AuthorizeAttribute.Policy` özelliğini sarmasını sağlayarak yapabilirsiniz.

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

Bu öznitelik türü, sabit kodlanmış ön eki (`"MinimumAge"`) ve Oluşturucu aracılığıyla geçirilen bir tamsayıyı temel alan bir `Policy` dizesine sahiptir.

Bir parametre olarak bir tamsayı almak dışında diğer `Authorize` öznitelikleriyle aynı şekilde eylemlere uygulayabilirsiniz.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Özel ıauthorizationpolicyprovider

Özel `MinimumAgeAuthorizeAttribute`, istenen en düşük yaş için yetkilendirme ilkeleri istemeyi kolaylaştırır. Çözecek bir sonraki sorun, yetkilendirme ilkelerinin tüm bu farklı yaşlar için kullanılabilir olduğundan emin olmanızı sağlar. Burada `IAuthorizationPolicyProvider` yararlı olur.

`MinimumAgeAuthorizationAttribute`kullanırken, yetkilendirme ilkesi adları `"MinimumAge" + Age`, bu nedenle özel `IAuthorizationPolicyProvider` yetkilendirme ilkeleri oluşturması gerekir:

* İlke adından yaş ayrıştırılıyor.
* Yeni bir `AuthorizationPolicy` oluşturmak için `AuthorizationPolicyBuilder` kullanma
* Bu ve aşağıdaki örneklerde, kullanıcının kimliğinin bir tanımlama bilgisi aracılığıyla doğrulandığını varsayacaktır. `AuthorizationPolicyBuilder`, en az bir yetkilendirme düzeni adıyla oluşturulmalıdır veya her zaman başarılı olur. Aksi takdirde, kullanıcıya nasıl bir sınama sağlayacaksınız ve bir özel durum oluşturulur.
* `AuthorizationPolicyBuilder.AddRequirements`yaş temelinde ilkeye gereksinimler ekleme. Diğer senaryolarda, bunun yerine `RequireClaim`, `RequireRole`veya `RequireUserName` kullanabilirsiniz.

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

## <a name="multiple-authorization-policy-providers"></a>Birden çok yetkilendirme ilkesi sağlayıcısı

Özel `IAuthorizationPolicyProvider` uygulamalarını kullanırken, ASP.NET Core yalnızca bir `IAuthorizationPolicyProvider`örneği kullandığını aklınızda bulundurun. Özel bir sağlayıcı kullanılacak tüm ilke adları için yetkilendirme ilkeleri sağlayamıyorsa, bu, bir yedekleme sağlayıcısına ertelemelidir. 

Örneğin, özel yaş ilkelerine ve daha geleneksel rol tabanlı ilke almaya ihtiyacı olan bir uygulamayı düşünün. Bu tür bir uygulama, özel bir yetkilendirme ilkesi sağlayıcısını şu şekilde kullanabilir:

* İlke adlarını ayrıştırmaya çalışır. 
* İlke adı bir yaş içermiyorsa farklı bir ilke sağlayıcısına (`DefaultAuthorizationPolicyProvider`gibi) çağırır.

Yukarıda gösterilen örnek `IAuthorizationPolicyProvider` uygulama, oluşturucusunda bir yedekleme ilkesi sağlayıcısı oluşturarak `DefaultAuthorizationPolicyProvider` kullanacak şekilde güncelleştirilemeyebilir (ilke adı, beklenen ' MinimumAge ' + Age ' i ile eşleşmediği durumlarda kullanılmak üzere).

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Daha sonra, `GetPolicyAsync` yöntemi null döndürmek yerine `BackupPolicyProvider` kullanacak şekilde yapılandırılabilir:

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Varsayılan ilke

Adlandırılmış Yetkilendirme İlkeleri sağlamaya ek olarak, bir ilke adı belirtmeden `[Authorize]` özniteliklerine bir yetkilendirme ilkesi sağlamak için özel bir `IAuthorizationPolicyProvider` `GetDefaultPolicyAsync` uygulaması gerekir.

Çoğu durumda, bu yetkilendirme özniteliği yalnızca kimliği doğrulanmış bir kullanıcı gerektirir, bu nedenle gerekli ilkeyi `RequireAuthenticatedUser`çağrısı yapabilirsiniz:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Özel bir `IAuthorizationPolicyProvider`tüm yönleriyle olduğu gibi, bunu gerektiği şekilde özelleştirebilirsiniz. Bazı durumlarda, varsayılan ilkenin geri dönüş `IAuthorizationPolicyProvider`alınması istenebilir.

## <a name="fallback-policy"></a>Geri dönüş ilkesi

Özel bir `IAuthorizationPolicyProvider`, [ilkeler birleştirilirken](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) ve hiçbir ilke belirtilmediğinde kullanılan bir ilke sağlamak için isteğe bağlı olarak `GetFallbackPolicyAsync` uygulayabilir. `GetFallbackPolicyAsync` null olmayan bir ilke döndürürse, istek için hiçbir ilke belirtilmediğinde, döndürülen ilke yetkilendirme ara yazılımı tarafından kullanılır.

Geri dönüş ilkesi gerekmiyorsa, sağlayıcı `null` döndürebilir veya geri dönüş sağlayıcısına erteleyebilirsiniz:

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Özel bir ıauthorizationpolicyprovider kullanın

Bir `IAuthorizationPolicyProvider`özel ilkeleri kullanmak için şunları yapmanız gerekir:

* Uygun `AuthorizationHandler` türlerini, tüm ilke tabanlı yetkilendirme senaryolarında olduğu gibi bağımlılık ekleme ( [ilke tabanlı yetkilendirme](xref:security/authorization/policies#authorization-handlers)bölümünde açıklanmıştır) ile kaydedin.
* Varsayılan ilke sağlayıcısını değiştirmek için, özel `IAuthorizationPolicyProvider` türünü uygulamanın bağımlılık ekleme hizmeti koleksiyonuna kaydedin (`Startup.ConfigureServices`).

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Tüm özel `IAuthorizationPolicyProvider` örnekleri, [ASPNET/AuthSamples GitHub deposunda](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)bulunur.
