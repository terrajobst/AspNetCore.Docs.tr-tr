---
title: "Geçirme Auth ve ASP.NET Core 2.0 kimliğine"
author: scottaddie
description: "Bu makalede, ASP.NET Core 2.0 geçirme ASP.NET Core 1.x kimlik doğrulama ve kimlik için en yaygın adımlara özetlenmektedir."
keywords: "ASP.NET Core, kimlik, kimlik doğrulama"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 1d8c75a21cd7110b3e414f0c600e9f05cbaeff45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="70b5d-104">Geçirme kimlik doğrulaması ve ASP.NET Core 2.0 için kimliği</span><span class="sxs-lookup"><span data-stu-id="70b5d-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="70b5d-105">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="70b5d-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="70b5d-106">ASP.NET Core 2.0 kimlik doğrulaması için yeni bir modeli vardır ve [kimlik](xref:security/authentication/identity) hangi basitleştirir yapılandırma Hizmetleri kullanarak.</span><span class="sxs-lookup"><span data-stu-id="70b5d-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="70b5d-107">Aşağıda özetlendiği gibi yeni model kullanılacak kimlik doğrulama veya kimliği kullanan ASP.NET Core 1.x uygulamaları güncelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="70b5d-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="70b5d-108">Kimlik doğrulaması ara yazılımı ve Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="70b5d-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="70b5d-109">1.x projelerinde kimlik doğrulaması ara yazılımı üzerinden yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="70b5d-110">Desteklemek istediğiniz her kimlik doğrulama şeması için bir ara yazılım yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="70b5d-111">Facebook kimlik doğrulaması aşağıdaki 1.x örnek kimliğinde yapılandırır *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="70b5d-112">2.0 projelerinde Hizmetleri aracılığıyla kimlik doğrulaması yapılandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="70b5d-113">Her kimlik doğrulama şeması kaydedilir `ConfigureServices` yöntemi *haline*.</span><span class="sxs-lookup"><span data-stu-id="70b5d-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="70b5d-114">`UseIdentity` Yöntemi ile değiştirilir `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="70b5d-115">Facebook kimlik doğrulaması aşağıdaki 2.0 örnek kimliğinde yapılandırır *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="70b5d-116">`UseAuthentication` Yöntemi, otomatik kimlik doğrulama ve Uzaktan kimlik doğrulama isteklerini işleme için sorumlu olan tek bir kimlik doğrulaması ara yazılım bileşeni ekler.</span><span class="sxs-lookup"><span data-stu-id="70b5d-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="70b5d-117">Tek tek ara yazılım bileşenlerinin tümünü tek, ortak ara yazılım bileşeni ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="70b5d-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="70b5d-118">Her ana kimlik doğrulaması düzeni için 2.0 geçiş yönergeleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="70b5d-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="70b5d-119">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-119">Cookie-based Authentication</span></span>
<span data-ttu-id="70b5d-120">Aşağıdaki iki seçenekten birini belirleyin ve gerekli değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="70b5d-121">Kimliğine sahip tanımlama bilgileri kullanma</span><span class="sxs-lookup"><span data-stu-id="70b5d-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="70b5d-122">Değiştir `UseIdentity` ile `UseAuthentication` içinde `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="70b5d-123">Çağırma `AddIdentity` yönteminde `ConfigureServices` tanımlama bilgisi kimlik doğrulama hizmetleri ekleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70b5d-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="70b5d-124">İsteğe bağlı olarak, çağırma `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yönteminde `ConfigureServices` kimlik tanımlama bilgisi ayarları ince ayar yapma yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70b5d-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="70b5d-125">Tanımlama bilgileri kimlik olmadan kullanma</span><span class="sxs-lookup"><span data-stu-id="70b5d-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="70b5d-126">Değiştir `UseCookieAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="70b5d-127">Çağırma `AddAuthentication` ve `AddCookie` yöntemleri `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="70b5d-128">JWT taşıyıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="70b5d-129">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="70b5d-130">Değiştir `UseJwtBearerAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-131">Çağırma `AddJwtBearer` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="70b5d-132">Varsayılan düzenini geçirerek ayarlamanız gerekir böylece bu kod parçacığını kimliği kullanmayan `JwtBearerDefaults.AuthenticationScheme` için `AddAuthentication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70b5d-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="70b5d-133">Openıd Connect (OIDC) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="70b5d-134">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="70b5d-135">Değiştir `UseOpenIdConnectAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-136">Çağırma `AddOpenIdConnect` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

### <a name="facebook-authentication"></a><span data-ttu-id="70b5d-137">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-137">Facebook Authentication</span></span>
<span data-ttu-id="70b5d-138">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="70b5d-139">Değiştir `UseFacebookAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-140">Çağırma `AddFacebook` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="70b5d-141">Google kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="70b5d-141">Google Authentication</span></span>
<span data-ttu-id="70b5d-142">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="70b5d-143">Değiştir `UseGoogleAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-144">Çağırma `AddGoogle` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="70b5d-145">Microsoft hesabı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="70b5d-146">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="70b5d-147">Değiştir `UseMicrosoftAccountAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-148">Çağırma `AddMicrosoftAccount` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="70b5d-149">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-149">Twitter Authentication</span></span>
<span data-ttu-id="70b5d-150">Aşağıdaki değişiklikleri yapın *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="70b5d-151">Değiştir `UseTwitterAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="70b5d-152">Çağırma `AddTwitter` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="70b5d-153">Varsayılan kimlik doğrulama şemasını ayarlama</span><span class="sxs-lookup"><span data-stu-id="70b5d-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="70b5d-154">1.x içinde `AutomaticAuthenticate` ve `AutomaticChallenge` özelliklerini [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfı hedeflenen bir tek bir kimlik doğrulaması düzeni için ayarlanacak.</span><span class="sxs-lookup"><span data-stu-id="70b5d-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="70b5d-155">Bu zorlamak için iyi hiçbir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="70b5d-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="70b5d-156">2. 0 ', bu iki özellik tek tek özellikleri olarak kaldırılan `AuthenticationOptions` örneği.</span><span class="sxs-lookup"><span data-stu-id="70b5d-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="70b5d-157">İçinde yapılandırılabilir `AddAuthentication` yöntem çağrısı içinde `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="70b5d-158">Yukarıdaki kod parçacığında, varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").</span><span class="sxs-lookup"><span data-stu-id="70b5d-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="70b5d-159">Alternatif olarak, aşırı yüklenmiş bir sürümünü kullanmanız `AddAuthentication` birden çok özelliği ayarlamak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="70b5d-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="70b5d-160">Aşağıdaki aşırı yüklenmiş yöntemin örnekte varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="70b5d-161">Kimlik doğrulama şeması alternatif olarak, tek tek içinde belirtilen `[Authorize]` öznitelikleri veya yetkilendirme ilkeleri.</span><span class="sxs-lookup"><span data-stu-id="70b5d-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="70b5d-162">Aşağıdaki koşullardan biri doğruysa varsayılan düzeni 2.0 tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="70b5d-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="70b5d-163">Otomatik olarak oturum açmanız kullanıcının istediğiniz</span><span class="sxs-lookup"><span data-stu-id="70b5d-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="70b5d-164">Kullandığınız `[Authorize]` düzenleri belirtmeden özniteliği veya Yetkilendirme İlkeleri</span><span class="sxs-lookup"><span data-stu-id="70b5d-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="70b5d-165">Bu kural için bir istisna `AddIdentity` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70b5d-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="70b5d-166">Bu yöntem ve varsayılan kimlik doğrulaması ve uygulama tanımlama bilgisine düzenleri sınama kümeleri için tanımlama bilgileri ekler `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="70b5d-167">Ayrıca, oturum açma varsayılan düzeni için dış tanımlama bilgisi ayarlar `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="70b5d-168">HttpContext kimlik doğrulama uzantıları kullanma</span><span class="sxs-lookup"><span data-stu-id="70b5d-168">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="70b5d-169">`IAuthenticationManager` Arabirimi 1.x kimlik doğrulaması sistemine ana giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="70b5d-170">Yeni bir kümesi ile değiştirilen `HttpContext` uzantı yöntemleri `Microsoft.AspNetCore.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="70b5d-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="70b5d-171">Örneğin, başvuru 1.x projeleri bir `Authentication` özelliği:</span><span class="sxs-lookup"><span data-stu-id="70b5d-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="70b5d-172">2.0 projelerinde alma `Microsoft.AspNetCore.Authentication` ad alanı ve silme `Authentication` özelliği başvuruları:</span><span class="sxs-lookup"><span data-stu-id="70b5d-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="70b5d-173">Windows kimlik doğrulaması (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="70b5d-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="70b5d-174">Windows kimlik doğrulaması iki çeşidi vardır:</span><span class="sxs-lookup"><span data-stu-id="70b5d-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="70b5d-175">Ana bilgisayar kimliği doğrulanmış kullanıcılar yalnızca izin verir</span><span class="sxs-lookup"><span data-stu-id="70b5d-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="70b5d-176">Ana bilgisayar hem anonim verir ve kullanıcıların kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="70b5d-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="70b5d-177">Yukarıda açıklanan Birincisi 2.0 değişikliklerden etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="70b5d-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="70b5d-178">Yukarıda açıklanan ikinci değişim 2.0 değişikliklerden etkilenir.</span><span class="sxs-lookup"><span data-stu-id="70b5d-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="70b5d-179">Örnek olarak, anonim kullanıcılar uygulamanıza IIS sağlayarak veya [HTTP.sys](xref:fundamentals/servers/weblistener) katman denetleyici düzeyinde ancak yetki vererek kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="70b5d-179">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="70b5d-180">Bu senaryoda, varsayılan düzenini ayarlamak `IISDefaults.AuthenticationScheme` içinde `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="70b5d-181">Varsayılan düzen ayarlanamadı uygun şekilde çalışmasını sınama için authorize isteğinin engeller.</span><span class="sxs-lookup"><span data-stu-id="70b5d-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="70b5d-182">IdentityCookieOptions örnekleri</span><span class="sxs-lookup"><span data-stu-id="70b5d-182">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="70b5d-183">2.0 değişiklikleri yan etkisi seçenekleri tanımlama bilgisi seçenekleri örnek yerine adlandırılmış kullanmanın anahtarıdır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="70b5d-184">Kimlik tanımlama bilgisi düzeni adları özelleştirme yeteneği kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="70b5d-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="70b5d-185">Örneğin, 1.x projelerin [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) geçirmek için bir `IdentityCookieOptions` parametresine *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="70b5d-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="70b5d-186">Dış tanımlama bilgisi kimlik doğrulama şeması sağlanan örneğinden erişilir:</span><span class="sxs-lookup"><span data-stu-id="70b5d-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="70b5d-187">Daha önce bahsedilen Oluşturucu ekleme 2.0 projelerinde gereksiz olur ve `_externalCookieScheme` alan silinebilir:</span><span class="sxs-lookup"><span data-stu-id="70b5d-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="70b5d-188">`IdentityConstants.ExternalScheme` Sabiti doğrudan kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="70b5d-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="70b5d-189">IdentityUser POCO Gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="70b5d-189">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="70b5d-190">Entity Framework (EF) çekirdek Gezinti özellikleri taban `IdentityUser` POCO (düz eski CLR nesnesi) kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="70b5d-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="70b5d-191">El ile 1.x projenizi bu özellikleri kullandıysanız, bunları 2.0 projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="70b5d-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="70b5d-192">Yinelenen yabancı anahtarlar EF çekirdek geçişler çalıştırırken önlemek için aşağıdakileri ekleyin, `IdentityDbContext` sınıfı `OnModelCreating` yöntemi (sonra `base.OnModelCreating();` çağrısı):</span><span class="sxs-lookup"><span data-stu-id="70b5d-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="70b5d-193">GetExternalAuthenticationSchemes Değiştir</span><span class="sxs-lookup"><span data-stu-id="70b5d-193">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="70b5d-194">Zaman uyumlu yöntemi `GetExternalAuthenticationSchemes` lehinde zaman uyumsuz bir sürümünü kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="70b5d-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="70b5d-195">1.x projeleri olmayan aşağıdaki kodu *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="70b5d-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="70b5d-196">Bu yöntem görünür *Login.cshtml* çok:</span><span class="sxs-lookup"><span data-stu-id="70b5d-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="70b5d-197">2.0 projelerinde kullanma `GetExternalAuthenticationSchemesAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="70b5d-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="70b5d-198">İçinde *Login.cshtml*, `AuthenticationScheme` erişilebilir özelliği `foreach` döngü değişiklikler `Name`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="70b5d-199">ManageLoginsViewModel özellik değişikliği</span><span class="sxs-lookup"><span data-stu-id="70b5d-199">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="70b5d-200">A `ManageLoginsViewModel` nesne kullanılıyor `ManageLogins` eylemi *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="70b5d-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="70b5d-201">1.x projelerinde nesne 's `OtherLogins` özelliği döndürme türü `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="70b5d-202">Bu dönüş türü alma gerektirir `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="70b5d-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="70b5d-203">Dönüş türü değişikliklerini 2.0 projelerinde `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="70b5d-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="70b5d-204">Bu yeni dönüş türü değiştirme gerektirir `Microsoft.AspNetCore.Http.Authentication` ile içeri aktarma bir `Microsoft.AspNetCore.Authentication` içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="70b5d-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="70b5d-205">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70b5d-205">Additional Resources</span></span>
<span data-ttu-id="70b5d-206">Ek bilgi ve tartışma için bkz: [Auth 2.0 tartışma](https://github.com/aspnet/Security/issues/1338) github'da sorun.</span><span class="sxs-lookup"><span data-stu-id="70b5d-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
