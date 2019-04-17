---
title: ASP.NET core'da özel yetkilendirme ilkesi sağlayıcıları
author: mjrousos
description: Özel IAuthorizationPolicyProvider Yetkilendirme İlkeleri dinamik olarak oluşturmak için ASP.NET Core uygulamanızı kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614415"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Özel yetkilendirme ilkesi IAuthorizationPolicyProvider kullanarak ASP.NET Core sağlayıcıları 

Tarafından [Mike Rousos](https://github.com/mjrousos)

Genellikle kullanırken [ilke tabanlı yetkilendirme](xref:security/authorization/policies), ilkeleri çağırarak kayıtlı `AuthorizationOptions.AddPolicy` yetkilendirme hizmet yapılandırmasının bir parçası olarak. Bazı senaryolarda, bunu (veya istenmediğinde) olabileceği tüm yetkilendirme ilkeleri bu şekilde kaydedilecek. Bu gibi durumlarda, özel bir kullanabileceğiniz `IAuthorizationPolicyProvider` Yetkilendirme İlkeleri nasıl sağlanan denetlemek için.

Senaryo örnekleri özel durumlarda [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) yararlı olabilecek içerir:

* İlke değerlendirmesi sağlamak için bir dış hizmet kullanma.
* (İçin farklı bir oda numaralarını veya örnek şu yaşlardaki) birçok farklı ilkeleri kullanarak, bu nedenle, değil mantıklı her tek yetkilendirme ilkesi ile eklemek için bir `AuthorizationOptions.AddPolicy` çağırın.
* Çalışma zamanında bir dış veri kaynağı (örneğin, bir veritabanı) içindeki bilgileri temel alan ilkeleri oluşturma veya yetkilendirme gereksinimleriyle başka bir mekanizma aracılığıyla dinamik olarak belirleme.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) gelen [AspNetCore GitHub deposu](https://github.com/aspnet/AspNetCore). ASP.NET/AspNetCore depo ZIP dosyasını indirin. Dosyanın sıkıştırmasını açın. Gidin *src/güvenlik/samples/CustomPolicyProvider* proje klasörü.

## <a name="customize-policy-retrieval"></a>İlke alma işlemi özelleştirme

ASP.NET Core uygulamaları kullanma uygulaması `IAuthorizationPolicyProvider` yetkilendirme ilkelerini almak için arabirim. Varsayılan olarak, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) kayıtlı ve kullanılır. `DefaultAuthorizationPolicyProvider` ilkelerden döndürür `AuthorizationOptions` sağlanan bir `IServiceCollection.AddAuthorization` çağırın.

Farklı bir'na kaydederek bu davranışı özelleştirebilirsiniz `IAuthorizationPolicyProvider` uygulamanın uygulamasında [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. 

`IAuthorizationPolicyProvider` Arabirimi iki API içerir:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) belirtilen bir ad için bir yetkilendirme ilkesi döndürür.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) varsayılan yetkilendirme ilkesi döndürür (için kullanılan ilkeyi `[Authorize]` öznitelikleri belirtilen bir ilke olmadan). 

Bu iki API uygulayarak, Yetkilendirme İlkeleri nasıl sağlanan özelleştirebilirsiniz.

## <a name="parameterized-authorize-attribute-example"></a>Parametreli özniteliği örnek Yetkilendir

Bir senaryo burada `IAuthorizationPolicyProvider` yararlıdır özel etkinleştirme `[Authorize]` öznitelikleri, gereksinimleri bir parametresine bağlıdır. Örneğin, [ilke tabanlı yetkilendirme](xref:security/authorization/policies) belgeleri, ("ilke bir örnek olarak kullanılan bir yaş tabanlı AtLeast21"). Farklı denetleyici eylemleri bir uygulamada kullanıcıları için kullanılabilir olması durumunda *farklı* yaş, birçok farklı yaş tabanlı ilkeleri sahip olmak yararlı olabilir. Uygulamanın gereksinim duyacağınız tüm farklı yaş tabanlı ilkelerinin kaydetme yerine `AuthorizationOptions`, özel bir ilkeleriyle dinamik olarak oluşturabileceğiniz `IAuthorizationPolicyProvider`. İlkeleri daha kolay hale getirmek için Eylemler gibi özel yetkilendirme özniteliğiyle açıklama ekleyebilirsiniz `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Özel yetkilendirme öznitelikleri

Yetkilendirme İlkeleri, adlarına göre tanımlanır. Özel `MinimumAgeAuthorizeAttribute` açıklanan daha önce bağımsız değişkenleri karşılık gelen bir yetkilendirme ilkesi almak için kullanılan bir dizeye eşlemesi gerekir. Bunu türeterek yapmak `AuthorizeAttribute` ve yaparak `Age` özelliği kaydırma `AuthorizeAttribute.Policy` özelliği.

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

Bu öznitelik türü sahip bir `Policy` dize sabit kodlanmış ön ekini temel alarak (`"MinimumAge"`) ve bir tamsayı geçirilen Oluşturucusu.

Aynı şekilde diğer eylemler için geçerli `Authorize` dışında bir parametre olarak bir tamsayı sürer, öznitelikleri.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Özel IAuthorizationPolicyProvider

Özel `MinimumAgeAuthorizeAttribute` gerekli en düşük yaş için isteği Yetkilendirme İlkeleri kolaylaştırır. Sonraki sorunu çözmek için Yetkilendirme İlkeleri tüm bu farklı yaş için kullanılabildiğinden emin yapıyor. Burada bir `IAuthorizationPolicyProvider` yararlıdır.

Kullanırken `MinimumAgeAuthorizationAttribute`, yetkilendirme ilkesi adları deseni takip `"MinimumAge" + Age`, bu nedenle özel `IAuthorizationPolicyProvider` tarafından yetkilendirme ilkeleri oluşturmanız gerekir:

* İlke adı yaş ayrıştırılıyor.
* Kullanarak `AuthorizationPolicyBuilder` yeni oluşturmak için `AuthorizationPolicy`
* Gereksinimleri ilkeye eklemeyi temel ile yaş `AuthorizationPolicyBuilder.AddRequirements`. Diğer senaryolarda kullanabileceğinize `RequireClaim`, `RequireRole`, veya `RequireUserName` yerine.

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

## <a name="multiple-authorization-policy-providers"></a>Birden çok yetkilendirme ilkesi sağlayıcıları

Özel kullanırken `IAuthorizationPolicyProvider` uygulamaları ASP.NET Core, yalnızca bir örneği kullanan aklınızda tutun `IAuthorizationPolicyProvider`. Özel bir sağlayıcı kullanılacak tüm ilke adları için Yetkilendirme İlkeleri sağlayamadığı ise yeniden bir yedekleme sağlayıcısına içerilmesi gerekir. 

Örneğin, özel yaşı ilkeleri hem daha geleneksel rol tabanlı ilke alımı gerektiren bir uygulamayı düşünün. Böyle bir uygulamayı bir özel yetkilendirme ilkesi sağlayıcısı kullanabilir:

* İlke adları ayrıştırmayı dener. 
* Farklı bir ilke sağlayıcısı çağırıyor (gibi `DefaultAuthorizationPolicyProvider`) ilke adı bir yaş içermiyorsa.

Örnek `IAuthorizationPolicyProvider` yukarıda gösterilen uygulama kullanmak için güncelleştirilebilir `DefaultAuthorizationPolicyProvider` bir geri dönüş ilke sağlayıcısı (ilke adı 'MinimumAge' + yaş, beklenen desene eşleşmiyor durumunda kullanılmak üzere) oluşturucusunda oluşturarak.

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Ardından, `GetPolicyAsync` yöntemi kullanmak için güncelleştirilebilir `FallbackPolicyProvider` null döndürmek yerine:

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Varsayılan ilke

Adlandırılmış Yetkilendirme İlkeleri, özel bir sağlama yanı sıra `IAuthorizationPolicyProvider` uygulamak gereken `GetDefaultPolicyAsync` sağlamak için bir yetkilendirme ilkesi için `[Authorize]` belirtilen bir ilke adı olmadan öznitelikleri.

Çoğu durumda, gerekli ilke çağrısıyla verebilmeniz için bu yetkilendirme öznitelik yalnızca kimliği doğrulanmış bir kullanıcı gerektirir. `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Özel bir tüm yönlerini olduğu gibi `IAuthorizationPolicyProvider`, bu, gerektiği şekilde özelleştirebilirsiniz. Bazı durumlarda, bir geri dönüşü varsayılan ilkesini almak için ilki arzu edilebilir `IAuthorizationPolicyProvider`.

## <a name="required-policy"></a>Gerekli ilke

Özel bir `IAuthorizationPolicyProvider` uygulamak gereken `GetRequiredPolicyAsync` isteğe bağlı olarak, her zaman gerekli olan bir ilke sağlamak için. Varsa `GetRequiredPolicyAsync` null olmayan bir ilkesi döndürür. diğer (adlı veya varsayılan) Bu ilkeyi birleştirilecek istenen ilke.

Gerekli ilke yok gerekirse, sağlayıcı yalnızca null döndürmesine veya geri dönüş sağlayıcıya Ertele:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Özel IAuthorizationPolicyProvider kullanın

Özel ilkeleri'nden kullanmak için bir `IAuthorizationPolicyProvider`, şunları yapmalısınız:

* Uygun kaydetme `AuthorizationHandler` bağımlılık ekleme türleriyle (açıklanan [ilke tabanlı yetkilendirme](xref:security/authorization/policies#authorization-handlers)), tüm ilke tabanlı yetkilendirme senaryoları gibi.
* Özel kayıt `IAuthorizationPolicyProvider` uygulamanın bağımlılık ekleme hizmeti koleksiyon türü (içinde `Startup.ConfigureServices`) varsayılan ilke sağlayıcısı değiştirilecek.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Tam bir özel `IAuthorizationPolicyProvider` örnek kullanılabilir [aspnet/AuthSamples GitHub deposu](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
