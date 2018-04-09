---
title: ASP.NET Core de belirli bir düzeni ile yetkilendirmek
author: rick-anderson
description: Bu makalede, birden çok kimlik doğrulama yöntemleri ile çalışırken, belirli bir düzeni kimliğine sınırlamak açıklanmaktadır.
manager: wpickett
ms.author: riande
ms.date: 10/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 81a01d7de8221fcb3bf90a108d9df6633ca2b696
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="5fc87-103">ASP.NET Core de belirli bir düzeni ile yetkilendirmek</span><span class="sxs-lookup"><span data-stu-id="5fc87-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="5fc87-104">Tek sayfa uygulamaları (SPAs) gibi bazı senaryolarda birden çok kimlik doğrulama yöntemlerini kullanmayı yaygındır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="5fc87-105">Örneğin, uygulama oturum açma tanımlama bilgisi tabanlı kimlik doğrulaması ve JWT taşıyıcı kimlik doğrulaması için JavaScript istekleri kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="5fc87-106">Bazı durumlarda, uygulama bir kimlik doğrulama işleyicisi birden çok örneği olabilir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="5fc87-107">Örneğin, iki tanımlama bilgisi işleyicileri burada temel bir kimlik içeriyor ve bir oluşturulduğunda bir multi-Factor authentication (MFA) tetiklendiğinde.</span><span class="sxs-lookup"><span data-stu-id="5fc87-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="5fc87-108">Kullanıcıya ek güvenlik gerektiren bir işlem istediğinden MFA tetiklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5fc87-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5fc87-110">Kimlik doğrulama hizmeti kimlik doğrulama işlemi sırasında yapılandırıldığında, bir kimlik doğrulama düzeni olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="5fc87-111">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5fc87-111">For example:</span></span>

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

<span data-ttu-id="5fc87-112">Önceki kod, iki kimlik doğrulama işleyicisi eklenmiştir: tanımlama bilgileri, diğeri taşıyıcı için için.</span><span class="sxs-lookup"><span data-stu-id="5fc87-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5fc87-113">Varsayılan düzeni belirtme sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği.</span><span class="sxs-lookup"><span data-stu-id="5fc87-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5fc87-114">Bu davranışı isterseniz, bu değil parametresiz biçiminde çağırarak devre dışı `AddAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="5fc87-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5fc87-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5fc87-116">Kimlik doğrulama sırasında kimlik doğrulama middlewares yapılandırıldığında kimlik doğrulama şemasını adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="5fc87-117">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5fc87-117">For example:</span></span>

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

<span data-ttu-id="5fc87-118">Önceki kod, iki kimlik doğrulama middlewares eklenmiştir: tanımlama bilgileri, diğeri taşıyıcı için için.</span><span class="sxs-lookup"><span data-stu-id="5fc87-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="5fc87-119">Varsayılan düzeni belirtme sonuçlanıyor `HttpContext.User` kimliğe ayarlanan özelliği.</span><span class="sxs-lookup"><span data-stu-id="5fc87-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="5fc87-120">Bu davranışı isterseniz, bu değil ayarlayarak devre dışı `AuthenticationOptions.AutomaticAuthenticate` özelliğine `false`.</span><span class="sxs-lookup"><span data-stu-id="5fc87-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="5fc87-121">Authorize özniteliği düzeniyle seçme</span><span class="sxs-lookup"><span data-stu-id="5fc87-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="5fc87-122">Yetkilendirme noktasında kullanılacak işleyici uygulamayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="5fc87-123">Uygulama ile yetkilendirmek için kimlik doğrulama şemasını virgülle ayrılmış bir listesini geçirerek işleyici seçin `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="5fc87-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="5fc87-124">`[Authorize]` Özniteliği, kimlik doğrulama düzeni veya varsayılan bir yapılandırılmış bağımsız olarak kullanılacak düzenleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="5fc87-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5fc87-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5fc87-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5fc87-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="5fc87-128">Önceki örnekte tanımlama bilgisi ve taşıyıcı işleyicileri çalıştırın ve oluşturma ve geçerli kullanıcı için bir kimlik ekleme fırsatına sahip.</span><span class="sxs-lookup"><span data-stu-id="5fc87-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="5fc87-129">Yalnızca tek bir düzen belirterek, karşılık gelen işleyici çalışır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5fc87-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5fc87-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5fc87-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="5fc87-132">Önceki kodda yalnızca işleyici "Bearer" şema ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="5fc87-133">Tanımlama bilgisi tabanlı kimlikleri göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="5fc87-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="5fc87-134">İlkeleri düzeniyle seçme</span><span class="sxs-lookup"><span data-stu-id="5fc87-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="5fc87-135">İstenen düzenleri belirtmek istiyorsanız [İlkesi](xref:security/authorization/policies), ayarlayabileceğiniz `AuthenticationSchemes` ilkeniz eklerken koleksiyonu:</span><span class="sxs-lookup"><span data-stu-id="5fc87-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="5fc87-136">Önceki örnekte, "Over18" ilke "Bearer" işleyicisi tarafından oluşturulan kimlik karşı yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="5fc87-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="5fc87-137">İlke ayarı kullanın `[Authorize]` özniteliğin `Policy` özelliği:</span><span class="sxs-lookup"><span data-stu-id="5fc87-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
