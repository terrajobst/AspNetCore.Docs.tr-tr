---
title: Kimlik doğrulaması ve kimliği 2,0 ASP.NET Core geçirin
author: scottaddie
description: Bu makalede ASP.NET Core 1. x kimlik doğrulamasını ve kimliğini ASP.NET Core 2,0 'e geçirmeye yönelik en yaygın adımlar özetlenmektedir.
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: f3817fa1808c331f7e167618e3bb00d68ad08571
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355175"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="589f1-103">Kimlik doğrulaması ve kimliği 2,0 ASP.NET Core geçirin</span><span class="sxs-lookup"><span data-stu-id="589f1-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="589f1-104">[Scott Ade](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK) tarafından</span><span class="sxs-lookup"><span data-stu-id="589f1-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="589f1-105">ASP.NET Core [2,0, kimlik doğrulama ve kimlik](xref:security/authentication/identity) için Hizmetleri kullanarak yapılandırmayı basitleştiren yeni bir modeline sahiptir.</span><span class="sxs-lookup"><span data-stu-id="589f1-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="589f1-106">Kimlik doğrulama veya kimlik kullanan ASP.NET Core 1. x uygulamaları, aşağıda özetlenen yeni modeli kullanacak şekilde güncelleştirilemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="589f1-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="589f1-107">Ad alanlarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="589f1-107">Update namespaces</span></span>

<span data-ttu-id="589f1-108">1\. x içinde, `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ad alanında `IdentityRole` ve `IdentityUser` sınıfları bulundu.</span><span class="sxs-lookup"><span data-stu-id="589f1-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="589f1-109">2,0 ' de <xref:Microsoft.AspNetCore.Identity> ad alanı bu tür sınıfların birkaçı için yeni giriş haline geldi.</span><span class="sxs-lookup"><span data-stu-id="589f1-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="589f1-110">Varsayılan kimlik koduyla, etkilenen sınıflar `ApplicationUser` ve `Startup`içerir.</span><span class="sxs-lookup"><span data-stu-id="589f1-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="589f1-111">Etkilenen başvuruları çözümlemek için `using` deyimlerinizi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="589f1-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="589f1-112">Kimlik doğrulama ara yazılımı ve Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="589f1-112">Authentication Middleware and services</span></span>

<span data-ttu-id="589f1-113">1\. x projelerinde kimlik doğrulaması, ara yazılım aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="589f1-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="589f1-114">Desteklemek istediğiniz her kimlik doğrulama düzeni için bir ara yazılım yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="589f1-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="589f1-115">Aşağıdaki 1. x örneği, Facebook kimlik doğrulamasını *Startup.cs*içinde kimliğiyle yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="589f1-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

<span data-ttu-id="589f1-116">2,0 projesinde, kimlik doğrulaması hizmetler aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="589f1-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="589f1-117">Her kimlik doğrulama şeması, *Startup.cs*`ConfigureServices` yöntemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="589f1-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="589f1-118">`UseIdentity` yöntemi `UseAuthentication`ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="589f1-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="589f1-119">Aşağıdaki 2,0 örnek, Facebook kimlik doğrulamasını *Startup.cs*içinde kimliğiyle yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="589f1-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="589f1-120">`UseAuthentication` yöntemi, otomatik kimlik doğrulamasından ve uzak kimlik doğrulama isteklerinin işlenmesinden sorumlu olan tek bir kimlik doğrulama ara yazılım bileşeni ekler.</span><span class="sxs-lookup"><span data-stu-id="589f1-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="589f1-121">Tek, ortak bir ara yazılım bileşeniyle tüm bireysel ara yazılım bileşenlerini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="589f1-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="589f1-122">Her bir büyük kimlik doğrulama şeması için 2,0 geçiş yönergeleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="589f1-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="589f1-123">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-123">Cookie-based authentication</span></span>

<span data-ttu-id="589f1-124">Aşağıdaki iki seçenekten birini seçin ve *Startup.cs*'de gerekli değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="589f1-125">Kimlik tanımlama bilgilerini kullanın</span><span class="sxs-lookup"><span data-stu-id="589f1-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="589f1-126">`Configure` yönteminde `UseAuthentication` `UseIdentity` değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="589f1-127">Tanımlama bilgisi kimlik doğrulama hizmetlerini eklemek için `ConfigureServices` yönteminde `AddIdentity` yöntemi çağırın.</span><span class="sxs-lookup"><span data-stu-id="589f1-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="589f1-128">İsteğe bağlı olarak, kimlik tanımlama bilgisi ayarlarını ince ayar için `ConfigureServices` yönteminde `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="589f1-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="589f1-129">Kimlik bilgilerini kimlik olmadan kullan</span><span class="sxs-lookup"><span data-stu-id="589f1-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="589f1-130">`Configure` yönteminde `UseCookieAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="589f1-131">`ConfigureServices` yönteminde `AddAuthentication` ve `AddCookie` yöntemlerini çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="589f1-132">JWT taşıyıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="589f1-133">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="589f1-134">`Configure` yönteminde `UseJwtBearerAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-135">`ConfigureServices` yönteminde `AddJwtBearer` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="589f1-136">Bu kod parçacığı kimlik kullanmaz, bu nedenle `AddAuthentication` yöntemine `JwtBearerDefaults.AuthenticationScheme` geçirerek varsayılan düzenin ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="589f1-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="589f1-137">OpenID Connect (OıDC) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="589f1-138">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="589f1-139">`Configure` yönteminde `UseOpenIdConnectAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-140">`ConfigureServices` yönteminde `AddOpenIdConnect` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

- <span data-ttu-id="589f1-141">`OpenIdConnectOptions` Eylemdeki `PostLogoutRedirectUri` özelliğini `SignedOutRedirectUri`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="589f1-142">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-142">Facebook authentication</span></span>

<span data-ttu-id="589f1-143">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="589f1-144">`Configure` yönteminde `UseFacebookAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-145">`ConfigureServices` yönteminde `AddFacebook` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="589f1-146">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-146">Google authentication</span></span>

<span data-ttu-id="589f1-147">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="589f1-148">`Configure` yönteminde `UseGoogleAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-149">`ConfigureServices` yönteminde `AddGoogle` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="589f1-150">Microsoft Hesabı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-150">Microsoft Account authentication</span></span>

<span data-ttu-id="589f1-151">Microsoft hesabı kimlik doğrulaması hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/14455)bakın.</span><span class="sxs-lookup"><span data-stu-id="589f1-151">For more information on Microsoft account authentication, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/14455).</span></span>

<span data-ttu-id="589f1-152">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-152">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="589f1-153">`Configure` yönteminde `UseMicrosoftAccountAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-153">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-154">`ConfigureServices` yönteminde `AddMicrosoftAccount` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-154">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="589f1-155">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="589f1-155">Twitter authentication</span></span>

<span data-ttu-id="589f1-156">*Startup.cs*içinde aşağıdaki değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="589f1-156">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="589f1-157">`Configure` yönteminde `UseTwitterAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="589f1-157">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="589f1-158">`ConfigureServices` yönteminde `AddTwitter` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="589f1-158">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="589f1-159">Varsayılan kimlik doğrulama düzenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="589f1-159">Setting default authentication schemes</span></span>

<span data-ttu-id="589f1-160">1\. x içinde, [Authenticationoptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfının `AutomaticAuthenticate` ve `AutomaticChallenge` özellikleri tek bir kimlik doğrulama düzeninde ayarlamaya yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="589f1-160">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="589f1-161">Bunu zorlayacağından iyi bir yol yoktu.</span><span class="sxs-lookup"><span data-stu-id="589f1-161">There was no good way to enforce this.</span></span>

<span data-ttu-id="589f1-162">2,0 ' de, bu iki özellik ayrı `AuthenticationOptions` örneğindeki özellikler olarak kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="589f1-162">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="589f1-163">Bunlar, *Startup.cs*yönteminin `ConfigureServices` yöntemi içinde `AddAuthentication` Yöntem çağrısında yapılandırılabilirler:</span><span class="sxs-lookup"><span data-stu-id="589f1-163">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="589f1-164">Önceki kod parçacığında, varsayılan düzen `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies") olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="589f1-164">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="589f1-165">Alternatif olarak, birden fazla özellik ayarlamak için `AddAuthentication` yönteminin aşırı yüklenmiş bir sürümünü kullanın.</span><span class="sxs-lookup"><span data-stu-id="589f1-165">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="589f1-166">Aşağıdaki aşırı yüklenmiş yöntem örneğinde, varsayılan düzen `CookieAuthenticationDefaults.AuthenticationScheme`olarak ayarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="589f1-166">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="589f1-167">Kimlik doğrulama düzeni, farklı `[Authorize]` özniteliklerde veya yetkilendirme ilkeleriniz içinde de belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="589f1-167">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="589f1-168">Aşağıdaki koşullardan biri doğru ise, 2,0 'de varsayılan bir düzen tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="589f1-168">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="589f1-169">Kullanıcının otomatik olarak oturum açabilmesi istiyorsunuz</span><span class="sxs-lookup"><span data-stu-id="589f1-169">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="589f1-170">Şemaları belirtmeden `[Authorize]` özniteliğini veya yetkilendirme ilkelerini kullanın</span><span class="sxs-lookup"><span data-stu-id="589f1-170">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="589f1-171">Bu kural için bir özel durum `AddIdentity` yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="589f1-171">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="589f1-172">Bu yöntem sizin için tanımlama bilgileri ekler ve varsayılan kimlik doğrulama ve zorluk düzenlerini `IdentityConstants.ApplicationScheme`uygulama tanımlama bilgisine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="589f1-172">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="589f1-173">Ayrıca, varsayılan oturum açma düzenini `IdentityConstants.ExternalScheme`dış tanımlama bilgisine ayarlar.</span><span class="sxs-lookup"><span data-stu-id="589f1-173">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="589f1-174">HttpContext kimlik doğrulama uzantılarını kullanma</span><span class="sxs-lookup"><span data-stu-id="589f1-174">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="589f1-175">`IAuthenticationManager` arabirimi, 1. x kimlik doğrulama sistemine ana giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="589f1-175">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="589f1-176">`Microsoft.AspNetCore.Authentication` ad alanında yeni bir `HttpContext` uzantısı yöntemleriyle değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="589f1-176">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="589f1-177">Örneğin, 1. x projeleri bir `Authentication` özelliğine başvurur:</span><span class="sxs-lookup"><span data-stu-id="589f1-177">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="589f1-178">2,0 projesinde `Microsoft.AspNetCore.Authentication` ad alanını içeri aktarın ve `Authentication` Özellik başvurularını silin:</span><span class="sxs-lookup"><span data-stu-id="589f1-178">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="589f1-179">Windows kimlik doğrulaması (HTTP. sys/Iisıntegration)</span><span class="sxs-lookup"><span data-stu-id="589f1-179">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="589f1-180">Windows kimlik doğrulamasının iki farklı çeşidi vardır:</span><span class="sxs-lookup"><span data-stu-id="589f1-180">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="589f1-181">Konak yalnızca kimliği doğrulanmış kullanıcılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="589f1-181">The host only allows authenticated users.</span></span> <span data-ttu-id="589f1-182">Bu çeşitleme 2,0 değişikliklerden etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="589f1-182">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="589f1-183">Konak hem anonim hem de kimliği doğrulanmış kullanıcılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="589f1-183">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="589f1-184">Bu çeşitleme 2,0 değişikliklerden etkilenir.</span><span class="sxs-lookup"><span data-stu-id="589f1-184">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="589f1-185">Örneğin, uygulamanın [IIS](xref:host-and-deploy/iis/index) veya [http. sys](xref:fundamentals/servers/httpsys) katmanında anonim kullanıcılara izin verilmesi, ancak kullanıcıları denetleyici düzeyinde yetkilendirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="589f1-185">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="589f1-186">Bu senaryoda, `Startup.ConfigureServices` yönteminde varsayılan düzeni ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="589f1-186">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="589f1-187">[Microsoft. AspNetCore. Server. ıısıntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)için varsayılan düzeni `IISDefaults.AuthenticationScheme`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="589f1-187">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="589f1-188">[Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)için varsayılan düzeni `HttpSysDefaults.AuthenticationScheme`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="589f1-188">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="589f1-189">Varsayılan düzenin ayarlanamaması, yetkilendirme (sınama) isteğinin şu özel durumla çalışmasını engeller:</span><span class="sxs-lookup"><span data-stu-id="589f1-189">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="589f1-190">`System.InvalidOperationException`: authenticationScheme belirtilmedi ve hiç Defaultchallenbescheme bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="589f1-190">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="589f1-191">Daha fazla bilgi için bkz. <xref:security/authentication/windowsauth>.</span><span class="sxs-lookup"><span data-stu-id="589f1-191">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="589f1-192">Identityıeoptions örnekleri</span><span class="sxs-lookup"><span data-stu-id="589f1-192">IdentityCookieOptions instances</span></span>

<span data-ttu-id="589f1-193">2,0 değişikliğin yan etkisi, tanımlama bilgisi seçenekleri örnekleri yerine adlandırılmış seçenekleri kullanma anahtarıdır.</span><span class="sxs-lookup"><span data-stu-id="589f1-193">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="589f1-194">Kimlik tanımlama bilgisi düzen adlarını özelleştirme özelliği kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="589f1-194">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="589f1-195">Örneğin, 1. x projeleri bir `IdentityCookieOptions` parametresini *accountcontroller.cs* ve *ManageController.cs*öğesine geçirmek için [Oluşturucu Ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) kullanır.</span><span class="sxs-lookup"><span data-stu-id="589f1-195">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="589f1-196">Dış tanımlama bilgisi kimlik doğrulama şemasına, belirtilen örnekten erişilir:</span><span class="sxs-lookup"><span data-stu-id="589f1-196">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="589f1-197">Belirtilen Oluşturucu Ekleme, 2,0 projesinde gereksiz hale gelir ve `_externalCookieScheme` alanı silinebilir:</span><span class="sxs-lookup"><span data-stu-id="589f1-197">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="589f1-198">1. x projeleri `_externalCookieScheme` alanını şu şekilde kullandı:</span><span class="sxs-lookup"><span data-stu-id="589f1-198">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="589f1-199">2,0 projesinde, yukarıdaki kodu aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="589f1-199">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="589f1-200">`IdentityConstants.ExternalScheme` sabiti doğrudan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="589f1-200">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="589f1-201">Aşağıdaki ad alanını içeri aktararak yeni eklenen `SignOutAsync` çağrısını çözümleyin:</span><span class="sxs-lookup"><span data-stu-id="589f1-201">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="589f1-202">Identityuser POCO gezinti özellikleri ekle</span><span class="sxs-lookup"><span data-stu-id="589f1-202">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="589f1-203">Taban `IdentityUser` POCO (düz eski CLR nesnesi) Entity Framework (EF) temel gezinti özellikleri kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="589f1-203">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="589f1-204">1\. x projeniz bu özellikleri kullandıysanız, bunları el ile 2,0 projesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="589f1-204">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="589f1-205">EF Core geçişleri çalıştırırken yinelenen yabancı anahtarların oluşmasını engellemek için, aşağıdakileri `IdentityDbContext` sınıfınıza ' `OnModelCreating` metoduna ekleyin (`base.OnModelCreating();` çağrısından sonra):</span><span class="sxs-lookup"><span data-stu-id="589f1-205">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="589f1-206">Getexternalauthenticationdüzenlerini değiştirme</span><span class="sxs-lookup"><span data-stu-id="589f1-206">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="589f1-207">Zaman uyumlu Yöntem `GetExternalAuthenticationSchemes` zaman uyumsuz bir sürüm yararına kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="589f1-207">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="589f1-208">1. x projeleri *denetleyicileri/ManageController. cs*dosyasında aşağıdaki koda sahiptir:</span><span class="sxs-lookup"><span data-stu-id="589f1-208">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="589f1-209">Bu yöntem, *Görünümler/Account/Login. cshtml* içinde de görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="589f1-209">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="589f1-210">2,0 projesinde <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="589f1-210">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="589f1-211">*ManageController.cs* içindeki değişiklik aşağıdaki koda benzer:</span><span class="sxs-lookup"><span data-stu-id="589f1-211">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="589f1-212">*Login. cshtml*içinde, `foreach` döngüsünde erişilen `AuthenticationScheme` özelliği `Name`olarak değişir:</span><span class="sxs-lookup"><span data-stu-id="589f1-212">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="589f1-213">ManageLoginsViewModel özelliği değişikliği</span><span class="sxs-lookup"><span data-stu-id="589f1-213">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="589f1-214">Bir `ManageLoginsViewModel` nesnesi, *ManageController.cs*`ManageLogins` eyleminde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="589f1-214">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="589f1-215">1\. x projelerinde, nesnenin `OtherLogins` özelliği dönüş türü `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="589f1-215">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="589f1-216">Bu dönüş türü `Microsoft.AspNetCore.Http.Authentication`içeri aktarmayı gerektirir:</span><span class="sxs-lookup"><span data-stu-id="589f1-216">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="589f1-217">2,0 projesinde, dönüş türü `IList<AuthenticationScheme>`olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="589f1-217">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="589f1-218">Bu yeni dönüş türü, `Microsoft.AspNetCore.Http.Authentication` içeri aktarmanın `Microsoft.AspNetCore.Authentication` içeri aktarma ile değiştirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="589f1-218">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="589f1-219">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="589f1-219">Additional resources</span></span>

<span data-ttu-id="589f1-220">Daha fazla bilgi için bkz. GitHub 'da [kimlik doğrulama 2,0 sorunu tartışması](https://github.com/aspnet/Security/issues/1338) .</span><span class="sxs-lookup"><span data-stu-id="589f1-220">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
