---
title: ASP.NET core'da belirli bir düzeni ile yetkilendirme
author: rick-anderson
description: Bu makalede, birden çok kimlik doğrulama yöntemleri ile çalışırken, belirli bir düzen kimliğini sınırlamak açıklanmaktadır.
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903005"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>ASP.NET core'da belirli bir düzeni ile yetkilendirme

Tek sayfa uygulamaları (Spa'lar) gibi bazı senaryolarda birden çok kimlik doğrulama yöntemleri kullanan yaygındır. Örneğin, uygulama oturum açma tanımlama bilgisi tabanlı kimlik doğrulaması ve JWT taşıyıcı kimlik doğrulaması için JavaScript istekleri kullanabilir. Bazı durumlarda, uygulama birden fazla örneğini bir kimlik doğrulama işleyicisi olabilir. Örneğin, iki tanımlama bilgisi işleyicileri burada temel bir kimlik içerir ve bir oluşturulduğunda bir multi-Factor authentication (MFA) tetiklendiğinde. Kullanıcıya ek güvenlik gerektiren bir işlem istediğinden MFA tetiklenebilir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kimlik doğrulaması sırasında kimlik doğrulama hizmeti tarafından yapılandırıldığında bir kimlik doğrulama düzeni olarak adlandırılır. Örneğin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

Önceki kodda, iki kimlik doğrulaması işleyici eklendi: biri tanımlama bilgileri, diğeri taşıyıcı için.

>[!NOTE]
>Varsayılan düzenini belirten sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği. Bu davranışı gerekli değildir, parametresiz derleyeceği harekete geçirerek devre dışı `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kimlik doğrulaması sırasında kimlik doğrulaması middlewares yapılandırıldığında kimlik doğrulama düzeni olarak adlandırılır. Örneğin:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

Önceki kodda, iki kimlik doğrulaması middlewares eklenmiştir: biri tanımlama bilgileri, diğeri taşıyıcı için.

>[!NOTE]
>Varsayılan düzenini belirten sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği. Bu davranışı gerekli değildir, ayarı devre dışı `AuthenticationOptions.AutomaticAuthenticate` özelliğini `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Authorize özniteliği düzeni seçme

Yetkilendirme noktasında kullanılacak işleyici uygulamayı gösterir. Uygulama ile yetkilendirmek için kimlik doğrulama düzenleri, virgülle ayrılmış listesini geçirerek işleyiciyi seçin `[Authorize]`. `[Authorize]` Kimlik doğrulama düzeni veya düzenleri varsayılan yapılandırılmış bağımsız olarak kullanılacak özniteliği belirtir. Örneğin:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

Yukarıdaki örnekte tanımlama bilgisi ve taşıyıcı işleyicileri çalıştırın ve oluşturun ve geçerli kullanıcı için bir kimlik eklenecek şansına sahip olabilirsiniz. Yalnızca tek bir düzen belirterek, karşılık gelen işleyici çalıştırır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

Önceki kodda; yalnızca işleyici "Bearer" düzeni ile çalışır. Tanımlama bilgisi tabanlı hiç kimlik yok sayılır.

## <a name="selecting-the-scheme-with-policies"></a>İlkeleriyle düzenini seçme

İstenen düzenleri belirtmek istiyorsanız [ilke](xref:security/authorization/policies), ayarlayabileceğiniz `AuthenticationSchemes` ilkenizi eklerken koleksiyonu:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

Önceki örnekte, "Over18" ilke karşı "Bearer" işleyicisi tarafından oluşturulan kimlik yalnızca çalışır. Bir ilke ayarlayarak kullanın `[Authorize]` özniteliğin `Policy` özelliği:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>Birden fazla kimlik doğrulama şeması kullanma

Bazı uygulamalar birden çok kimlik doğrulama türlerini desteklemek gerekebilir. Örneğin, uygulamanız kullanıcıların Azure Active Directory'den ve kullanıcıların veritabanından kimlik doğrulaması. Başka bir örnek, hem Active Directory Federasyon Hizmetleri, hem de Azure Active Directory B2C kullanıcılarının kimliğini doğrulayan bir uygulamadır. Bu durumda, uygulama, JWT taşıyıcı belirtecinden birkaç verenler kabul etmelidir.

Kabul etmek istediğiniz tüm kimlik doğrulama düzenleri ekleyin. Aşağıdaki örnek, kod `Startup.ConfigureServices` farklı verenler ile iki JWT taşıyıcı kimlik doğrulama düzenleri ekler:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> Yalnızca bir JWT taşıyıcı kimlik doğrulaması varsayılan kimlik doğrulama düzeni kayıtlı `JwtBearerDefaults.AuthenticationScheme`. Ek kimlik doğrulama bir benzersiz kimlik doğrulama düzeni ile kayıtlı olması gerekir.

Sonraki adım her iki kimlik doğrulama düzenleri kabul etmek için varsayılan yetkilendirme ilkesi güncelleştirmektir. Örneğin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

Varsayılan yetkilendirme ilkesi geçersiz kılınan gibi kullanmak mümkün mü `[Authorize]` denetleyicileri özniteliği. Denetleyici ardından istekleri ile ilk veya ikinci veren tarafından verilen JWT kabul eder.

::: moniker-end
