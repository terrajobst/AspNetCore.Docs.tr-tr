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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>ASP.NET Core 'de ıauthorizationpolicyprovider kullanan özel yetkilendirme Ilkesi sağlayıcıları 

, [Mike Rousos](https://github.com/mjrousos) tarafından

Genellikle [ilke tabanlı yetkilendirme](xref:security/authorization/policies)kullanılırken, ilkeler, yetkilendirme hizmeti yapılandırmasının bir parçası `AuthorizationOptions.AddPolicy` olarak çağırarak kaydedilir. Bazı senaryolarda, tüm yetkilendirme ilkelerini bu şekilde kaydetmek mümkün olmayabilir (veya istenebilir). Bu durumlarda, yetkilendirme ilkelerinin nasıl sağlanmış olduğunu denetlemek `IAuthorizationPolicyProvider` için özel ' i kullanabilirsiniz.

Özel bir [ıauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilecek senaryolara örnek olarak şunlar verilebilir:

* İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.
* Büyük bir ilke yelpazesi (örneğin, farklı oda numaraları veya yaşlar için) kullanarak, her bireysel yetkilendirme ilkesini bir `AuthorizationOptions.AddPolicy` çağrısıyla eklemek mantıklı değildir.
* Dış veri kaynağındaki bilgilere (bir veritabanı gibi) göre veya başka bir mekanizmaya göre yetkilendirme gereksinimlerini dinamik olarak belirleyerek çalışma zamanında ilkeler oluşturma.

[Aspnetcore GitHub deposundan](https://github.com/aspnet/AspNetCore) [örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) . ASPNET/AspNetCore depo ZIP dosyasını indirin. Dosyayı sıkıştırmayı açın. *Src/Security/Samples/CustomPolicyProvider* proje klasörüne gidin.

## <a name="customize-policy-retrieval"></a>İlke almayı özelleştirme

ASP.NET Core uygulamalar, `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirimin bir uygulamasını kullanır. Varsayılan olarak, [Defaultauthorizationpolicyprovider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kaydedilir ve kullanılır. `DefaultAuthorizationPolicyProvider``AuthorizationOptions` bir`IServiceCollection.AddAuthorization` çağrıda sunulan ilkeleri döndürür.

Uygulamanın `IAuthorizationPolicyProvider` [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına farklı bir uygulama kaydederek bu davranışı özelleştirebilirsiniz. 

`IAuthorizationPolicyProvider` Arabirim iki API içerir:

* [Getpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) , belirli bir ad için yetkilendirme ilkesi döndürüyor.
* [Getdefaultpolicyasync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) varsayılan yetkilendirme ilkesini döndürür (bir ilke belirtilmeden öznitelikler için `[Authorize]` kullanılan ilke). 

Bu iki API 'yi uygulayarak, yetkilendirme ilkelerinin nasıl sağlandığını özelleştirebilirsiniz.

## <a name="parameterized-authorize-attribute-example"></a>Parametreli yetkilendirme özniteliği örneği

Tek senaryo `IAuthorizationPolicyProvider` , gereksinimleri bir parametreye bağlı özel `[Authorize]` öznitelikleri etkinleştirir. Örneğin, [ilke tabanlı yetkilendirme](xref:security/authorization/policies) belgelerinde, örnek olarak yaş tabanlı ("AtLeast21") ilkesi kullanılmıştır. Bir uygulamadaki farklı denetleyici eylemleri *farklı* yaştaki kullanıcılar için kullanılabilir hale getirilse, çok sayıda farklı yaş tabanlı ilke olması faydalı olabilir. Uygulamanın ihtiyaç duyduğu `AuthorizationOptions`tüm farklı yaş tabanlı ilkeleri kaydetmek yerine, ilkeleri özel `IAuthorizationPolicyProvider`olarak dinamik bir şekilde oluşturabilirsiniz. İlkeleri kullanmak daha kolay hale getirmek için, gibi `[MinimumAgeAuthorize(20)]`özel yetkilendirme özniteliğiyle eylemlere not ekleyebilirsiniz.

## <a name="custom-authorization-attributes"></a>Özel yetkilendirme öznitelikleri

Yetkilendirme ilkeleri adlarıyla tanımlanır. Daha önce `MinimumAgeAuthorizeAttribute` açıklanan özel, bağımsız değişkenleri ilgili yetkilendirme ilkesini almak için kullanılabilecek bir dizeye eşlemenize ihtiyaç duyuyor. Bunu öğesinden `AuthorizeAttribute` türeterek ve özelliği sararak `Age` `AuthorizeAttribute.Policy` yaparak yapabilirsiniz.

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

Bu öznitelik türü, sabit `Policy` kodlanmış ön eki (`"MinimumAge"`) ve Oluşturucu aracılığıyla geçirilen bir tamsayıyı temel alan bir dizeye sahiptir.

Bir parametre olarak bir tamsayı almak dışında, diğer `Authorize` özniteliklerle aynı şekilde eylemlere uygulayabilirsiniz.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Özel ıauthorizationpolicyprovider

Özel `MinimumAgeAuthorizeAttribute` , istenen en düşük yaş için yetkilendirme ilkeleri istemeyi kolaylaştırır. Çözecek bir sonraki sorun, yetkilendirme ilkelerinin tüm bu farklı yaşlar için kullanılabilir olduğundan emin olmanızı sağlar. Bunun yararlı olduğu yerdir `IAuthorizationPolicyProvider` .

Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları, bu `"MinimumAge" + Age`sayede, özel `IAuthorizationPolicyProvider` olarak yetkilendirme ilkeleri oluşturması gerekir.

* İlke adından yaş ayrıştırılıyor.
* Yeni `AuthorizationPolicyBuilder` bir oluşturmak için kullanarak`AuthorizationPolicy`
* Bu ve aşağıdaki örneklerde, kullanıcının kimliğinin bir tanımlama bilgisi aracılığıyla doğrulandığını varsayacaktır. `AuthorizationPolicyBuilder` En az bir yetkilendirme düzeni adıyla oluşturulmalıdır ya da her zaman başarılı olur. Aksi takdirde, kullanıcıya nasıl bir sınama sağlayacaksınız ve bir özel durum oluşturulur.
* Yaş temelinde ilkeye gereksinimler ekleme `AuthorizationPolicyBuilder.AddRequirements`. Diğer senaryolarda, veya `RequireClaim` `RequireUserName` yerine kullanabilirsiniz `RequireRole`.

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

Özel `IAuthorizationPolicyProvider` uygulamalar kullanırken ASP.NET Core yalnızca bir `IAuthorizationPolicyProvider`örneğini kullandığını aklınızda bulundurun. Özel bir sağlayıcı kullanılacak tüm ilke adları için yetkilendirme ilkeleri sağlayamazsa, bir yedekleme sağlayıcısına geri dönecektir. 

Örneğin, özel yaş ilkelerine ve daha geleneksel rol tabanlı ilke almaya ihtiyacı olan bir uygulamayı düşünün. Bu tür bir uygulama, özel bir yetkilendirme ilkesi sağlayıcısını şu şekilde kullanabilir:

* İlke adlarını ayrıştırmaya çalışır. 
* İlke adı bir yaş içermiyorsa farklı bir ilke `DefaultAuthorizationPolicyProvider`sağlayıcısına (gibi) çağrı yapılır.

Yukarıda gösterilen `IAuthorizationPolicyProvider` örnek uygulama, `DefaultAuthorizationPolicyProvider` Oluşturucusu içinde bir geri dönüş ilkesi oluşturma (ilke adının beklenen ' minimum umage ' ve Age ile eşleşmemesi durumunda kullanılmak üzere) kullanılarak kullanılmak üzere güncelleştirilemeyebilir.

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Daha sonra, `GetPolicyAsync` yöntemi null döndürmek `FallbackPolicyProvider` yerine öğesini kullanacak şekilde yapılandırılabilir:

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Varsayılan ilke

Adlandırılmış Yetkilendirme İlkeleri sağlamaya ek olarak, bir ilke adı `IAuthorizationPolicyProvider` belirtmeden öznitelikler için `GetDefaultPolicyAsync` `[Authorize]` bir yetkilendirme ilkesi sağlamak üzere özel bir uygulama gerekir.

Çoğu durumda, bu yetkilendirme özniteliği yalnızca kimliği doğrulanmış bir kullanıcı gerektirir, bu nedenle gerekli ilkeyi öğesine `RequireAuthenticatedUser`bir çağrı ile yapabilirsiniz:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Özel `IAuthorizationPolicyProvider`bir tüm yönlere göre, bunu gerektiği şekilde özelleştirebilirsiniz. Bazı durumlarda, varsayılan ilkeyi bir geri `IAuthorizationPolicyProvider`yüklemeden almak istenebilir.

## <a name="required-policy"></a>Gerekli ilke

İçin uygulanması `IAuthorizationPolicyProvider` `GetRequiredPolicyAsync` gereken özel bir, isteğe bağlı olarak her zaman gerekli bir ilke sağlar. Null olmayan bir ilke döndürürse,builkeistenendiğer(adlandırılmışveyavarsayılan)ilkeylebirleştirilir.`GetRequiredPolicyAsync`

Gerekli bir ilke gerekmiyorsa, sağlayıcı yalnızca null döndürebilir veya geri dönüş sağlayıcısına erteleyebilirsiniz:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Özel bir ıauthorizationpolicyprovider kullanın

Özel ilkeleri bir `IAuthorizationPolicyProvider`ile kullanmak için şunları yapmanız gerekir:

* İlke tabanlı yetkilendirme `AuthorizationHandler` senaryolarında olduğu gibi, bağımlılık ekleme ( [ilke tabanlı yetkilendirme](xref:security/authorization/policies#authorization-handlers)bölümünde açıklanmıştır) ile uygun türleri kaydedin.
* Varsayılan ilke sağlayıcısını `IAuthorizationPolicyProvider` değiştirmek için uygulamanın bağımlılık ekleme hizmeti koleksiyonuna (içine `Startup.ConfigureServices`) özel türü kaydedin.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Tüm özel `IAuthorizationPolicyProvider` bir örnek, [ASPNET/authsamples GitHub deposunda](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)bulunur.
