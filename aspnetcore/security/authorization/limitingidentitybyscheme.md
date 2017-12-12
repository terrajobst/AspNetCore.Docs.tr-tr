---
title: "Belirli bir düzen - ASP.NET Core yetkilendirmek"
author: rick-anderson
description: "Bu makalede, birden çok kimlik doğrulama yöntemleri ile çalışırken, belirli bir düzeni kimliğine sınırlamak açıklanmaktadır."
keywords: "ASP.NET Core, kimlik, kimlik doğrulama şeması"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 8c9d068b88263d0c06b11a6b87416fb02885c475
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="authorize-with-a-specific-scheme"></a>Belirli bir düzeniyle yetkilendirmek

Tek sayfa uygulamaları (SPAs) gibi bazı senaryolarda birden çok kimlik doğrulama yöntemlerini kullanmayı yaygındır. Örneğin, uygulama oturum açma tanımlama bilgisi tabanlı kimlik doğrulaması ve JWT taşıyıcı kimlik doğrulaması için JavaScript istekleri kullanabilir. Bazı durumlarda, uygulama bir kimlik doğrulama işleyicisi birden çok örneği olabilir. Örneğin, iki tanımlama bilgisi işleyicileri burada temel bir kimlik içeriyor ve bir oluşturulduğunda bir multi-Factor authentication (MFA) tetiklendiğinde. Kullanıcıya ek güvenlik gerektiren bir işlem istediğinden MFA tetiklenebilir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Kimlik doğrulama hizmeti kimlik doğrulama işlemi sırasında yapılandırıldığında, bir kimlik doğrulama düzeni olarak adlandırılır. Örneğin:

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

Önceki kod, iki kimlik doğrulama işleyicisi eklenmiştir: tanımlama bilgileri, diğeri taşıyıcı için için.

>[!NOTE]
>Varsayılan düzeni belirtme sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği. Bu davranışı isterseniz, bu değil parametresiz biçiminde çağırarak devre dışı `AddAuthentication`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Kimlik doğrulama sırasında kimlik doğrulama middlewares yapılandırıldığında kimlik doğrulama şemasını adlandırılır. Örneğin:

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

Önceki kod, iki kimlik doğrulama middlewares eklenmiştir: tanımlama bilgileri, diğeri taşıyıcı için için.

>[!NOTE]
>Varsayılan düzeni belirtme sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği. Bu davranışı isterseniz, bu değil ayarlayarak devre dışı `AuthenticationOptions.AutomaticAuthenticate` özelliğine `false`.

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Authorize özniteliği düzeniyle seçme

Yetkilendirme noktasında kullanılacak işleyici uygulamayı gösterir. Uygulama ile yetkilendirmek için kimlik doğrulama şemasını virgülle ayrılmış bir listesini geçirerek işleyici seçin `[Authorize]`. `[Authorize]` Özniteliği, kimlik doğrulama düzeni veya varsayılan bir yapılandırılmış bağımsız olarak kullanılacak düzenleri belirtir. Örneğin:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

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

Önceki örnekte tanımlama bilgisi ve taşıyıcı işleyicileri çalıştırın ve oluşturma ve geçerli kullanıcı için bir kimlik ekleme fırsatına sahip. Yalnızca tek bir düzen belirterek, karşılık gelen işleyici çalışır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

Önceki kodda yalnızca işleyici "Bearer" şema ile çalışır. Tanımlama bilgisi tabanlı kimlikleri göz ardı edilir.

## <a name="selecting-the-scheme-with-policies"></a>İlkeleri düzeniyle seçme

İstenen düzenleri belirtmek istiyorsanız [İlkesi](xref:security/authorization/policies), ayarlayabileceğiniz `AuthenticationSchemes` ilkeniz eklerken koleksiyonu:

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

Önceki örnekte, "Over18" ilke "Bearer" işleyicisi tarafından oluşturulan kimlik karşı yalnızca çalışır. İlke ayarı kullanın `[Authorize]` özniteliğin `Policy` özelliği:

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
