---
title: Kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 geçirme
author: scottaddie
description: Bu makalede, ASP.NET Core 2.0 için geçirme ASP.NET Core 1.x kimlik doğrulaması ve kimlik için en yaygın adımlar özetlenmektedir.
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898733"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 geçirme

Tarafından [Scott Addie](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 kimlik doğrulaması için yeni bir modeli vardır ve [kimlik](xref:security/authentication/identity) sadeleştiren yapılandırma hizmetlerini kullanarak. Kimlik doğrulaması veya kimlik kullanan ASP.NET Core 1.x uygulamaları, aşağıda belirtildiği gibi yeni modeli kullanmak için güncelleştirilebilir.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Kimlik doğrulaması ara yazılımı ve Hizmetleri

1.x projelerinde, kimlik doğrulaması ara yazılımı üzerinden yapılandırılır. Desteklemek istediğiniz her bir kimlik doğrulama düzeni için bir ara yazılım yöntemi çağrılır.

Aşağıdaki 1.x örnek kimliği ile Facebook kimlik doğrulamasını yapılandırır *Startup.cs*:

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

2.0 projelerinde, kimlik doğrulama hizmetleri aracılığıyla yapılandırılır. Her kimlik doğrulama düzeni kaydedilmiştir `ConfigureServices` yöntemi *Startup.cs*. `UseIdentity` Yöntemi ile değiştirilir `UseAuthentication`.

Aşağıdaki 2.0 örnek kimliği ile Facebook kimlik doğrulamasını yapılandırır *Startup.cs*:

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

`UseAuthentication` Yöntemi otomatik kimlik doğrulama ve Uzaktan kimlik doğrulama isteklerinin işlenmesi için sorumlu tek bir kimlik doğrulaması ara yazılım bileşeni ekler. Tek bir ara yazılım bileşenlerinin tümünü tek, ortak bir ara yazılım bileşeni ile değiştirir.

Her temel kimlik doğrulaması düzeni için 2.0 geçiş yönergeleri aşağıda verilmiştir.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması

Aşağıdaki iki seçenekten birini seçin ve gerekli değişiklikleri yapın *Startup.cs*:

1. Tanımlama bilgileri kimlik ile kullanma
    - Değiştirin `UseIdentity` ile `UseAuthentication` içinde `Configure` yöntemi:

        ```csharp
        app.UseAuthentication();
        ```

    - Çağırma `AddIdentity` yönteminde `ConfigureServices` tanımlama bilgisi kimlik doğrulaması hizmetlerini eklemek için yöntemi.
    - İsteğe bağlı olarak, çağırma `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yönteminde `ConfigureServices` kimlik tanımlama bilgisi ayarları ince ayar için yöntemi.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Kimlik olmadan tanımlama bilgileri kullanma
    - Değiştirin `UseCookieAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - Çağırma `AddAuthentication` ve `AddCookie` yöntemleri `ConfigureServices` yöntemi:

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

### <a name="jwt-bearer-authentication"></a>JWT taşıyıcı kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:
- Değiştirin `UseJwtBearerAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddJwtBearer` yönteminde `ConfigureServices` yöntemi:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Varsayılan düzenini geçirerek ayarlanmalıdır. Bu nedenle bu kod parçacığı kimliğini kullanmaz `JwtBearerDefaults.AuthenticationScheme` için `AddAuthentication` yöntemi.

### <a name="openid-connect-oidc-authentication"></a>Openıd Connect (OIDC) kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:

- Değiştirin `UseOpenIdConnectAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddOpenIdConnect` yönteminde `ConfigureServices` yöntemi:

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

### <a name="facebook-authentication"></a>Facebook kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:
- Değiştirin `UseFacebookAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddFacebook` yönteminde `ConfigureServices` yöntemi:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:
- Değiştirin `UseGoogleAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddGoogle` yönteminde `ConfigureServices` yöntemi:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Microsoft Account kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:
- Değiştirin `UseMicrosoftAccountAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddMicrosoftAccount` yönteminde `ConfigureServices` yöntemi:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Twitter kimlik doğrulaması

Aşağıdaki değişiklikleri yapın *Startup.cs*:
- Değiştirin `UseTwitterAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Çağırma `AddTwitter` yönteminde `ConfigureServices` yöntemi:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Varsayılan kimlik doğrulama düzenleri ayarlama

1.x içinde `AutomaticAuthenticate` ve `AutomaticChallenge` özelliklerini [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfı yönelik tek bir kimlik doğrulama şemasını temel ayarlanacak. Bunu zorunlu kılmak için iyi bir yolu yoktu.

2. 0 ', bu iki özellik özellikleri ayrı ayrı olarak kaldırılan `AuthenticationOptions` örneği. İçinde yapılandırılabilir `AddAuthentication` yöntem çağrısı içinde `ConfigureServices` yöntemi *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Yukarıdaki kod parçacığında, varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").

Alternatif olarak, aşırı yüklenmiş bir sürümünü kullanmak `AddAuthentication` birden fazla özelliği ayarlamak için yöntemi. Aşırı yüklenmiş yöntem aşağıdaki örnekte varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme`. Kimlik doğrulama düzeni bunun yerine bireysel içinde belirtilen `[Authorize]` öznitelikleri veya yetkilendirme ilkeleri.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Aşağıdaki koşullardan biri doğru ise, 2. 0'varsayılan düzenini tanımlayın:
- Kullanıcı otomatik olarak oturum açmanız istiyor
- Kullandığınız `[Authorize]` düzenleri belirtmeden özniteliği veya Yetkilendirme İlkeleri

Bu kuralın istisnası `AddIdentity` yöntemi. Bu yöntem, siz ve varsayılan kimlik doğrulaması ve uygulama tanımlama bilgisinin düzenleri meydan kümeleri için tanımlama bilgileri ekler. `IdentityConstants.ApplicationScheme`. Ayrıca, varsayılan oturum açma düzeni için dış tanımlama bilgisi ayarlar `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>HttpContext kimlik doğrulama uzantıları kullanma

`IAuthenticationManager` Arabirimi 1.x kimlik doğrulama sisteminde ana giriş noktasıdır. Yeni bir dizi ile değiştirilmiştir `HttpContext` alanında uzantı yöntemlerini `Microsoft.AspNetCore.Authentication` ad alanı.

Örneğin, başvuru 1.x projeleri bir `Authentication` özelliği:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

2.0 projelerinde, içeri aktarma `Microsoft.AspNetCore.Authentication` ad alanını ve silme `Authentication` özelliği başvuruları:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows kimlik doğrulaması (HTTP.sys / IISIntegration)

Windows kimlik doğrulamasının iki çeşidi vardır:
1. Konak, yalnızca kimliği doğrulanmış kullanıcılara izin verir
2. Ana bilgisayar hem anonim verir ve kimliği doğrulanmış kullanıcılar

Yukarıda açıklanan Birincisi 2.0 değişikliklerden etkilenmez.

Yukarıda açıklanan İkincisiyse 2.0 değişikliklerden etkilenir. Örneğin, anonim kullanıcılar uygulamanıza IIS sağlayan veya [HTTP.sys](xref:fundamentals/servers/httpsys) katman denetleyici düzeyinde ancak yetki verme kullanıcılar. Bu senaryoda, varsayılan düzenini ayarlayın `IISDefaults.AuthenticationScheme` içinde `Startup.ConfigureServices` yöntemi:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Varsayılan düzen ayarlanamadı uygun şekilde çalışmasını eşleşip eşleşmediğini authorize isteğinin engeller.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions örnekleri

Bir yan etkisi 2.0 değişiklikleri seçeneklerini tanımlama bilgisi seçenekleri örnek yerine adlandırılmış kullanarak anahtardır. Kimlik tanımlama bilgisi düzeni adları özelleştirme yeteneği kaldırılır.

Örneğin, 1.x projelerin [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) geçirilecek bir `IdentityCookieOptions` parametrede *AccountController.cs*. Dış tanımlama bilgisi kimlik doğrulaması düzeni sağlanan örneğinden erişilebilir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Yukarıda sözü edilen Oluşturucu ekleme 2.0 projelerinde, gereksiz olur ve `_externalCookieScheme` alan silinebilir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Sabiti doğrudan kullanılabilir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>POCO IdentityUser Gezinti özellikleri ekleyin

Entity Framework (EF) çekirdek gezinme özelliklerini temel `IdentityUser` POCO (düz eski CLR nesnesi) kaldırıldı. El ile 1.x projenizi bu özellikleri kullandıysanız, bunları 2.0 projeye ekleyin:

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

EF Core geçişleri çalıştırırken yinelenen yabancı anahtarlar önlemek için aşağıdaki ekleyin, `IdentityDbContext` sınıfının `OnModelCreating` yöntemi (sonra `base.OnModelCreating();` çağrısı):

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

## <a name="replace-getexternalauthenticationschemes"></a>GetExternalAuthenticationSchemes değiştirin

Zaman uyumlu yöntem `GetExternalAuthenticationSchemes` yerine zaman uyumsuz bir sürümü kaldırıldı. 1.x projelerini olmayan aşağıdaki kodu *ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Bu yöntem görünür *Login.cshtml* çok:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

2.0 projelerinde kullanma `GetExternalAuthenticationSchemesAsync` yöntemi:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

İçinde *Login.cshtml*, `AuthenticationScheme` erişilebilir özellik `foreach` döngü değişiklikleri `Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel özellik değişikliği

A `ManageLoginsViewModel` nesnesi kullanılır `ManageLogins` eylemi *ManageController.cs*. 1.x projelerinde, nesne 's `OtherLogins` özelliği döndürme türü `IList<AuthenticationDescription>`. Bu dönüş türü alma gerektirir `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Dönüş türü değişikliklerini 2.0 projelerinde `IList<AuthenticationScheme>`. Bu yeni dönüş türünü değiştirme gerektirir `Microsoft.AspNetCore.Http.Authentication` ile içeri aktarma bir `Microsoft.AspNetCore.Authentication` içeri aktarın.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Ek kaynaklar

Ek ayrıntılar ve tartışma için bkz: [Auth 2.0 için tartışma](https://github.com/aspnet/Security/issues/1338) github'da sorun.
