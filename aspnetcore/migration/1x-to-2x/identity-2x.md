---
title: Kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 geçirme
author: scottaddie
description: Bu makalede, ASP.NET Core 2.0 için geçirme ASP.NET Core 1.x kimlik doğrulaması ve kimlik için en yaygın adımlar özetlenmektedir.
ms.author: scaddie
ms.date: 06/13/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 3e8bc75b87a85159c9668b52eea32bb7d700be6c
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196377"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="3c8e0-103">Kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="3c8e0-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="3c8e0-104">Tarafından [Scott Addie](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="3c8e0-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="3c8e0-105">ASP.NET Core 2.0 kimlik doğrulaması için yeni bir modeli vardır ve [kimlik](xref:security/authentication/identity) , basitleştirir yapılandırma hizmetlerini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="3c8e0-106">Kimlik doğrulaması veya kimlik kullanan ASP.NET Core 1.x uygulamaları, aşağıda belirtildiği gibi yeni modeli kullanmak için güncelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="3c8e0-107">Güncelleştirme ad alanları</span><span class="sxs-lookup"><span data-stu-id="3c8e0-107">Update namespaces</span></span>

<span data-ttu-id="3c8e0-108">1\.x içinde gibi sınıfları `IdentityRole` ve `IdentityUser` bulundu `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="3c8e0-109">2\.0, <xref:Microsoft.AspNetCore.Identity> ad alanı birkaç tür sınıflar için yeni giriş dönüştü.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="3c8e0-110">Varsayılan kimlik koduyla etkilenen sınıflarını `ApplicationUser` ve `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="3c8e0-111">Ayarlama, `using` etkilenen başvurularını çözümlemek için deyimleri.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="3c8e0-112">Kimlik doğrulaması ara yazılımı ve Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="3c8e0-112">Authentication Middleware and services</span></span>

<span data-ttu-id="3c8e0-113">1\.x projelerinde, kimlik doğrulaması ara yazılımı üzerinden yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="3c8e0-114">Desteklemek istediğiniz her bir kimlik doğrulama düzeni için bir ara yazılım yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="3c8e0-115">Aşağıdaki 1.x örnek kimliği ile Facebook kimlik doğrulamasını yapılandırır *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="3c8e0-116">2\.0 projelerinde, kimlik doğrulama hizmetleri aracılığıyla yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="3c8e0-117">Her kimlik doğrulama düzeni kaydedilmiştir `ConfigureServices` yöntemi *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="3c8e0-118">`UseIdentity` Yöntemi ile değiştirilir `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="3c8e0-119">Aşağıdaki 2.0 örnek kimliği ile Facebook kimlik doğrulamasını yapılandırır *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="3c8e0-120">`UseAuthentication` Yöntemi otomatik kimlik doğrulaması ve Uzaktan kimlik doğrulama isteklerinin işlenmesini sorumlu tek bir kimlik doğrulaması ara yazılım bileşeni ekler.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="3c8e0-121">Tek bir ara yazılım bileşenlerinin tümünü tek, ortak bir ara yazılım bileşeni ile değiştirir.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="3c8e0-122">Her temel kimlik doğrulaması düzeni için 2.0 geçiş yönergeleri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="3c8e0-123">Tanımlama bilgisi tabanlı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-123">Cookie-based authentication</span></span>

<span data-ttu-id="3c8e0-124">Aşağıdaki iki seçenekten birini seçin ve gerekli değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="3c8e0-125">Tanımlama bilgileri kimlik ile kullanma</span><span class="sxs-lookup"><span data-stu-id="3c8e0-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="3c8e0-126">Değiştirin `UseIdentity` ile `UseAuthentication` içinde `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="3c8e0-127">Çağırma `AddIdentity` yönteminde `ConfigureServices` tanımlama bilgisi kimlik doğrulaması hizmetlerini eklemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="3c8e0-128">İsteğe bağlı olarak, çağırma `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yönteminde `ConfigureServices` kimlik tanımlama bilgisi ayarları ince ayar için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="3c8e0-129">Kimlik olmadan tanımlama bilgileri kullanma</span><span class="sxs-lookup"><span data-stu-id="3c8e0-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="3c8e0-130">Değiştirin `UseCookieAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="3c8e0-131">Çağırma `AddAuthentication` ve `AddCookie` yöntemleri `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="3c8e0-132">JWT taşıyıcı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="3c8e0-133">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3c8e0-134">Değiştirin `UseJwtBearerAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-135">Çağırma `AddJwtBearer` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="3c8e0-136">Varsayılan düzenini geçirerek ayarlanmalıdır. Bu nedenle bu kod parçacığı kimliğini kullanmaz `JwtBearerDefaults.AuthenticationScheme` için `AddAuthentication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="3c8e0-137">Openıd Connect (OIDC) kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="3c8e0-138">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="3c8e0-139">Değiştirin `UseOpenIdConnectAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-140">Çağırma `AddOpenIdConnect` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="3c8e0-141">Değiştirin `PostLogoutRedirectUri` özelliğinde `OpenIdConnectOptions` eylemiyle `SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="3c8e0-142">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-142">Facebook authentication</span></span>

<span data-ttu-id="3c8e0-143">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3c8e0-144">Değiştirin `UseFacebookAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-145">Çağırma `AddFacebook` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="3c8e0-146">Google kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-146">Google authentication</span></span>

<span data-ttu-id="3c8e0-147">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3c8e0-148">Değiştirin `UseGoogleAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-149">Çağırma `AddGoogle` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="3c8e0-150">Microsoft Account kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-150">Microsoft Account authentication</span></span>

<span data-ttu-id="3c8e0-151">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-151">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3c8e0-152">Değiştirin `UseMicrosoftAccountAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-152">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-153">Çağırma `AddMicrosoftAccount` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-153">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="3c8e0-154">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3c8e0-154">Twitter authentication</span></span>

<span data-ttu-id="3c8e0-155">Aşağıdaki değişiklikleri yapın *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-155">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="3c8e0-156">Değiştirin `UseTwitterAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-156">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="3c8e0-157">Çağırma `AddTwitter` yönteminde `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-157">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="3c8e0-158">Varsayılan kimlik doğrulama düzenleri ayarlama</span><span class="sxs-lookup"><span data-stu-id="3c8e0-158">Setting default authentication schemes</span></span>

<span data-ttu-id="3c8e0-159">1\.x içinde `AutomaticAuthenticate` ve `AutomaticChallenge` özelliklerini [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfı yönelik tek bir kimlik doğrulama şemasını temel ayarlanacak.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-159">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="3c8e0-160">Bunu zorunlu kılmak için iyi bir yolu yoktu.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-160">There was no good way to enforce this.</span></span>

<span data-ttu-id="3c8e0-161">2\. 0 ', bu iki özellik özellikleri ayrı ayrı olarak kaldırılan `AuthenticationOptions` örneği.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-161">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="3c8e0-162">İçinde yapılandırılabilir `AddAuthentication` yöntem çağrısı içinde `ConfigureServices` yöntemi *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-162">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="3c8e0-163">Yukarıdaki kod parçacığında, varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").</span><span class="sxs-lookup"><span data-stu-id="3c8e0-163">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="3c8e0-164">Alternatif olarak, aşırı yüklenmiş bir sürümünü kullanmak `AddAuthentication` birden fazla özelliği ayarlamak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-164">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="3c8e0-165">Aşırı yüklenmiş yöntem aşağıdaki örnekte varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-165">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="3c8e0-166">Kimlik doğrulama düzeni bunun yerine bireysel içinde belirtilen `[Authorize]` öznitelikleri veya yetkilendirme ilkeleri.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-166">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="3c8e0-167">Aşağıdaki koşullardan biri doğru ise, 2. 0'varsayılan düzenini tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-167">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="3c8e0-168">Kullanıcı otomatik olarak oturum açmanız istiyor</span><span class="sxs-lookup"><span data-stu-id="3c8e0-168">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="3c8e0-169">Kullandığınız `[Authorize]` düzenleri belirtmeden özniteliği veya Yetkilendirme İlkeleri</span><span class="sxs-lookup"><span data-stu-id="3c8e0-169">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="3c8e0-170">Bu kuralın istisnası `AddIdentity` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-170">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="3c8e0-171">Bu yöntem, siz ve varsayılan kimlik doğrulaması ve uygulama tanımlama bilgisinin düzenleri meydan kümeleri için tanımlama bilgileri ekler. `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-171">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="3c8e0-172">Ayrıca, varsayılan oturum açma düzeni için dış tanımlama bilgisi ayarlar `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-172">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="3c8e0-173">HttpContext kimlik doğrulama uzantıları kullanma</span><span class="sxs-lookup"><span data-stu-id="3c8e0-173">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="3c8e0-174">`IAuthenticationManager` Arabirimi 1.x kimlik doğrulama sisteminde ana giriş noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-174">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="3c8e0-175">Yeni bir dizi ile değiştirilmiştir `HttpContext` alanında uzantı yöntemlerini `Microsoft.AspNetCore.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-175">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="3c8e0-176">Örneğin, başvuru 1.x projeleri bir `Authentication` özelliği:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-176">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="3c8e0-177">2\.0 projelerinde, içeri aktarma `Microsoft.AspNetCore.Authentication` ad alanını ve silme `Authentication` özelliği başvuruları:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-177">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="3c8e0-178">Windows kimlik doğrulaması (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="3c8e0-178">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="3c8e0-179">Windows kimlik doğrulamasının iki çeşidi vardır:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-179">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="3c8e0-180">Konak, yalnızca kimliği doğrulanmış kullanıcılara izin verir</span><span class="sxs-lookup"><span data-stu-id="3c8e0-180">The host only allows authenticated users</span></span>
2. <span data-ttu-id="3c8e0-181">Ana bilgisayar hem anonim verir ve kimliği doğrulanmış kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="3c8e0-181">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="3c8e0-182">Yukarıda açıklanan Birincisi 2.0 değişikliklerden etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-182">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="3c8e0-183">Yukarıda açıklanan İkincisiyse 2.0 değişikliklerden etkilenir.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-183">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="3c8e0-184">Örneğin, anonim kullanıcılar uygulamanıza IIS sağlayan veya [HTTP.sys](xref:fundamentals/servers/httpsys) katman denetleyici düzeyinde ancak yetki verme kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-184">For example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="3c8e0-185">Bu senaryoda, varsayılan düzenini ayarlayın `IISDefaults.AuthenticationScheme` içinde `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-185">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="3c8e0-186">Varsayılan düzen ayarlanamadı authorize isteğinin eşleşip eşleşmediğini çalışmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-186">Failure to set the default scheme prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="3c8e0-187">IdentityCookieOptions örnekleri</span><span class="sxs-lookup"><span data-stu-id="3c8e0-187">IdentityCookieOptions instances</span></span>

<span data-ttu-id="3c8e0-188">Bir yan etkisi 2.0 değişiklikleri seçeneklerini tanımlama bilgisi seçenekleri örnek yerine adlandırılmış kullanarak anahtardır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-188">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="3c8e0-189">Kimlik tanımlama bilgisi düzeni adları özelleştirme yeteneği kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-189">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="3c8e0-190">Örneğin, 1.x projelerin [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) geçirilecek bir `IdentityCookieOptions` parametrede *AccountController.cs* ve *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-190">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="3c8e0-191">Dış tanımlama bilgisi kimlik doğrulaması düzeni sağlanan örneğinden erişilebilir:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-191">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="3c8e0-192">Yukarıda sözü edilen Oluşturucu ekleme 2.0 projelerinde, gereksiz olur ve `_externalCookieScheme` alan silinebilir:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-192">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="3c8e0-193">kullanılan 1.x projelerini `_externalCookieScheme` gibi alan:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-193">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="3c8e0-194">2\.0 projelerinde, önceki kodu aşağıdakiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-194">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="3c8e0-195">`IdentityConstants.ExternalScheme` Sabiti doğrudan kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-195">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="3c8e0-196">Yeni eklenen çözmek `SignOutAsync` aşağıdaki ad alanını içeri aktararak çağırın:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-196">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="3c8e0-197">POCO IdentityUser Gezinti özellikleri ekleyin</span><span class="sxs-lookup"><span data-stu-id="3c8e0-197">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="3c8e0-198">Entity Framework (EF) çekirdek gezinme özelliklerini temel `IdentityUser` POCO (düz eski CLR nesnesi) kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-198">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="3c8e0-199">El ile 1.x projenizi bu özellikleri kullandıysanız, bunları 2.0 projeye ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-199">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="3c8e0-200">EF Core geçişleri çalıştırırken yinelenen yabancı anahtarlar önlemek için aşağıdaki ekleyin, `IdentityDbContext` sınıfının `OnModelCreating` yöntemi (sonra `base.OnModelCreating();` çağrısı):</span><span class="sxs-lookup"><span data-stu-id="3c8e0-200">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="3c8e0-201">GetExternalAuthenticationSchemes değiştirin</span><span class="sxs-lookup"><span data-stu-id="3c8e0-201">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="3c8e0-202">Zaman uyumlu yöntem `GetExternalAuthenticationSchemes` yerine zaman uyumsuz bir sürümü kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-202">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="3c8e0-203">1.x projelerini olmayan aşağıdaki kodu *Controllers/ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-203">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="3c8e0-204">Bu yöntem görünür *Views/Account/Login.cshtml* çok:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-204">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="3c8e0-205">2\.0 projelerinde kullanma <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-205">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="3c8e0-206">Değişiklik *ManageController.cs* aşağıdaki koda benzer:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-206">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="3c8e0-207">İçinde *Login.cshtml*, `AuthenticationScheme` erişilebilir özellik `foreach` döngü değişiklikleri `Name`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-207">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="3c8e0-208">ManageLoginsViewModel özellik değişikliği</span><span class="sxs-lookup"><span data-stu-id="3c8e0-208">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="3c8e0-209">A `ManageLoginsViewModel` nesnesi kullanılır `ManageLogins` eylemi *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-209">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="3c8e0-210">1\.x projelerinde, nesne 's `OtherLogins` özelliği döndürme türü `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-210">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="3c8e0-211">Bu dönüş türü alma gerektirir `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="3c8e0-211">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="3c8e0-212">Dönüş türü değişikliklerini 2.0 projelerinde `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-212">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="3c8e0-213">Bu yeni dönüş türünü değiştirme gerektirir `Microsoft.AspNetCore.Http.Authentication` ile içeri aktarma bir `Microsoft.AspNetCore.Authentication` içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-213">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="3c8e0-214">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3c8e0-214">Additional resources</span></span>

<span data-ttu-id="3c8e0-215">Daha fazla bilgi için [Auth 2.0 için tartışma](https://github.com/aspnet/Security/issues/1338) github'da sorun.</span><span class="sxs-lookup"><span data-stu-id="3c8e0-215">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
