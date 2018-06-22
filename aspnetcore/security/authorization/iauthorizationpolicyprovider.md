---
title: ASP.NET Core özel yetkilendirme ilkesi sağlayıcıları
author: mjrousos
description: Dinamik olarak yetkilendirme ilkeleri oluşturmak için bir ASP.NET Core uygulamada özel IAuthorizationPolicyProvider kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277263"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Özel yetkilendirme ilkesi IAuthorizationPolicyProvider ASP.NET Core kullanarak sağlayıcılar 

Tarafından [CAN Rousos](https://github.com/mjrousos)

Genellikle kullanırken [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies), ilkeleri çağırarak kayıtlı `AuthorizationOptions.AddPolicy` yetkilendirme hizmet yapılandırmasının bir parçası olarak. Bazı senaryolarda bu olası (veya istenmediğinde) olmayabilir tüm yetkilendirme ilkeleri bu şekilde kaydetmek için. Bu gibi durumlarda, özel bir kullanabilirsiniz `IAuthorizationPolicyProvider` Yetkilendirme İlkeleri nasıl sağlanan denetlemek için.

Aşağıda örnek senaryolar özel bir yerde [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilir içerir:

* İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.
* İlkeleri büyük bir dizi (farklı oda numaralarını veya örneğin yaş) kullanarak, bu nedenle değil kolaylaştırır her bireysel yetkilendirme ilkesi ile eklemek için anlamlı bir `AuthorizationOptions.AddPolicy` çağırın.
* Dış veri kaynağı (örneğin, bir veritabanı) içindeki bilgileri temel çalışma zamanında ilkeleri oluşturma veya başka bir mekanizma yetkilendirme gereksinimleri dinamik olarak belirleme.

## <a name="customizing-policy-retrieval"></a>İlke alma işlemini özelleştirme

ASP.NET Core uygulamaları kullanma uygulaması `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirim. Varsayılan olarak, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kayıtlı ve kullanılır. `DefaultAuthorizationPolicyProvider` ilkelerden döndürür `AuthorizationOptions` sağlanan bir `IServiceCollection.AddAuthorization` çağırın.

Bu davranış farklı bir kaydederek özelleştirebileceğiniz `IAuthorizationPolicyProvider` uygulamanın uygulamasında [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. 

`IAuthorizationPolicyProvider` Arabirimi iki API'leri içerir:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) bir verilen ad için bir yetkilendirme ilkesi döndürür.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) varsayılan yetkilendirme ilkesi döndürür (için kullanılan ilkeyi `[Authorize]` öznitelikleri belirtilen bir ilke olmadan). 

Bu iki API uygulayarak, Yetkilendirme İlkeleri nasıl sağlanır özelleştirebilirsiniz.

## <a name="parameterized-authorize-attribute-example"></a>Parametreli özniteliği örnek yetkilendirmek

Bir senaryo nerede `IAuthorizationPolicyProvider` yararlıdır özel etkinleştirme `[Authorize]` öznitelikleri olan gereksinimleri bir parametresine bağlıdır. Örneğin, [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies) belgeleri ("ilke bir örnek olarak kullanılan bir yaş tabanlı AtLeast21"). Bir uygulamada farklı denetleyici eylemleri kullanıcıları için kullanılabilir olması durumunda *farklı* yaş, birçok farklı yaş tabanlı ilkeleri yararlı olabilir. Uygulamanın gereksinim duyacağınız tüm farklı yaş tabanlı ilkeleri kaydetme yerine `AuthorizationOptions`, özel bir ilkeleriyle dinamik olarak oluşturabilir `IAuthorizationPolicyProvider`. İlkelerle daha kolay hale getirmek için Eylemler gibi özel yetkilendirme özniteliğiyle açıklama ekleyebilir `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Özel yetkilendirme öznitelikleri

Yetkilendirme İlkeleri, adlarına göre tanımlanır. Özel `MinimumAgeAuthorizeAttribute` açıklanan bağımsız değişkenleri karşılık gelen yetkilendirme ilkesini almak için kullanılan bir dizeye eşlemek daha önce gerekiyor. Bunu türetme tarafından yapmak `AuthorizeAttribute` ve yaparak `Age` özelliği kaydırma `AuthorizeAttribute.Policy` özelliği.

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

Bu öznitelik türüne sahip bir `Policy` dize sabit kodlanmış ön ekini temel alarak (`"MinimumAge"`) ve bir tamsayı geçirilen Oluşturucusu.

Aynı şekilde diğer eylemler için geçerli `Authorize` tamsayı bir parametre olarak alır ancak bu öznitelikleri.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Özel IAuthorizationPolicyProvider

Özel `MinimumAgeAuthorizeAttribute` istenen minimum yaş için isteği Yetkilendirme İlkeleri kolay hale getirir. Sonraki sorunu çözmek için Yetkilendirme İlkeleri tüm bu farklı yaş için kullanılabilir olduğundan emin olmak. Bu yerdir bir `IAuthorizationPolicyProvider` yararlıdır.

Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları deseni izler `"MinimumAge" + Age`, bu nedenle özel `IAuthorizationPolicyProvider` yetkilendirme ilkeleri tarafından oluşturmak:

* İlke adı yaş ayrıştırma.
* Kullanarak `AuthorizationPolicyBuiler` yeni oluşturmak için `AuthorizationPolicy`
* İlkeyi gereksinimler ekleme temel geçerlilik süresi ile `AuthorizationPolicyBuilder.AddRequirements`. Diğer senaryolarda kullanabilirsiniz `RequireClaim`, `RequireRole`, veya `RequireUserName` yerine.

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

## <a name="multiple-authorization-policy-providers"></a>Birden çok yetkilendirme ilkesi sağlayıcısı

Özel kullanırken `IAuthorizationPolicyProvider` uygulamaları ASP.NET Core yalnızca bir örneğini kullanan göz önünde bulundurun `IAuthorizationPolicyProvider`. Özel bir sağlayıcı için tüm ilke adları Yetkilendirme İlkeleri sağlamak mümkün değilse, bir yedekleme sağlayıcısına geri girecektir. İlke adları için varsayılan bir ilke alınması o içerebilir `[Authorize]` bir ad olmadan öznitelikleri.

Örneğin, bir uygulamanın özel yaşı ilkeleri ve daha geleneksel rol tabanlı ilke alma işlemi gerekli göz önünde bulundurun. Bu tür bir uygulama bir özel yetkilendirme ilkesi sağlayıcısı kullanabilirsiniz:

* İlke adları ayrıştırmak çalışır. 
* Farklı bir ilke sağlayıcısı çağrılar (gibi `DefaultAuthorizationPolicyProvider`) ilkesi adı bir yaş içermiyorsa.

## <a name="default-policy"></a>Varsayılan ilke

Adlandırılmış Yetkilendirme İlkeleri, özel bir sağlama yanı sıra `IAuthorizationPolicyProvider` uygulamak gereken `GetDefaultPolicyAsync` için bir yetkilendirme ilkesi sağlamak için `[Authorize]` belirtilen bir ilke adı olmadan öznitelikleri.

Gerekli ilke çağrısıyla olabilmeniz çoğu durumda, bu yetkilendirme öznitelik kimliği doğrulanmış bir kullanıcı yalnızca gerektirir. `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Özel bir tüm yönlerini olduğu gibi `IAuthorizationPolicyProvider`, bu, gerektiği gibi özelleştirebilirsiniz. Bazı durumlarda:

* Varsayılan Yetkilendirme İlkeleri kullanılmayabilir.
* Varsayılan ilkeyi almak için bir geri dönüş aktarılabileceği `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Özel IAuthorizationPolicyProvider kullanma

Özel ilkelerden kullanmak için bir `IAuthorizationPolicyProvider`, şunları yapmalısınız:

* Uygun kaydetmek `AuthorizationHandler` bağımlılık ekleme türleriyle (açıklanan [ilke tabanlı bir yetkilendirme](xref:security/authorization/policies#authorization-handlers)), tüm ilke tabanlı bir yetkilendirme senaryoları gibi.
* Özel kayıt `IAuthorizationPolicyProvider` uygulamanın bağımlılık ekleme hizmeti koleksiyonundaki türü (içinde `Startup.ConfigureServices`) varsayılan ilke sağlayıcısı değiştirmek için.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Tam bir özel `IAuthorizationPolicyProvider` örnek sağlanmıştır [aspnet/AuthSamples GitHub deposunu](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
