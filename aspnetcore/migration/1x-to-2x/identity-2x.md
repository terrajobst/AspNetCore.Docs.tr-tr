---
title: "Geçirme Auth ve ASP.NET Core 2.0 kimliğine"
author: scottaddie
description: "Bu makalede, ASP.NET Core 2.0 geçirme ASP.NET Core 1.x kimlik doğrulama ve kimlik için en yaygın adımlara özetlenmektedir."
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 72ad31438a344fb5fa2b357c709b923b8077e742
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>Geçirme kimlik doğrulaması ve ASP.NET Core 2.0 için kimliği

Tarafından [Scott Addie](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 kimlik doğrulaması için yeni bir modeli vardır ve [kimlik](xref:security/authentication/identity) hangi basitleştirir yapılandırma Hizmetleri kullanarak. Aşağıda özetlendiği gibi yeni model kullanılacak kimlik doğrulama veya kimliği kullanan ASP.NET Core 1.x uygulamaları güncelleştirilebilir.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Kimlik doğrulaması ara yazılımı ve Hizmetleri
1.x projelerinde kimlik doğrulaması ara yazılımı üzerinden yapılandırılır. Desteklemek istediğiniz her kimlik doğrulama şeması için bir ara yazılım yöntemi çağrılır.

Facebook kimlik doğrulaması aşağıdaki 1.x örnek kimliğinde yapılandırır *haline*:

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

2.0 projelerinde Hizmetleri aracılığıyla kimlik doğrulaması yapılandırılmaktadır. Her kimlik doğrulama şeması kaydedilir `ConfigureServices` yöntemi *haline*. `UseIdentity` Yöntemi ile değiştirilir `UseAuthentication`.

Facebook kimlik doğrulaması aşağıdaki 2.0 örnek kimliğinde yapılandırır *haline*:

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

`UseAuthentication` Yöntemi, otomatik kimlik doğrulama ve Uzaktan kimlik doğrulama isteklerini işleme için sorumlu olan tek bir kimlik doğrulaması ara yazılım bileşeni ekler. Tek tek ara yazılım bileşenlerinin tümünü tek, ortak ara yazılım bileşeni ile değiştirir.

Her ana kimlik doğrulaması düzeni için 2.0 geçiş yönergeleri aşağıda verilmiştir.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması
Aşağıdaki iki seçenekten birini belirleyin ve gerekli değişiklikleri yapın *haline*:

1. Kimliğine sahip tanımlama bilgileri kullanma
    - Değiştir `UseIdentity` ile `UseAuthentication` içinde `Configure` yöntemi:

        ```csharp
        app.UseAuthentication();
        ```

    - Çağırma `AddIdentity` yönteminde `ConfigureServices` tanımlama bilgisi kimlik doğrulama hizmetleri ekleme yöntemi.
    - İsteğe bağlı olarak, çağırma `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yönteminde `ConfigureServices` kimlik tanımlama bilgisi ayarları ince ayar yapma yöntemi.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Tanımlama bilgileri kimlik olmadan kullanma
    - Değiştir `UseCookieAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:
  
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
Aşağıdaki değişiklikleri yapın *haline*:
- Değiştir `UseJwtBearerAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:
 
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

    Varsayılan düzenini geçirerek ayarlamanız gerekir böylece bu kod parçacığını kimliği kullanmayan `JwtBearerDefaults.AuthenticationScheme` için `AddAuthentication` yöntemi.

### <a name="openid-connect-oidc-authentication"></a>Openıd Connect (OIDC) kimlik doğrulaması
Aşağıdaki değişiklikleri yapın *haline*:

- Değiştir `UseOpenIdConnectAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

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
Aşağıdaki değişiklikleri yapın *haline*:
- Değiştir `UseFacebookAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:
 
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

### <a name="google-authentication"></a>Google kimlik doğrulama
Aşağıdaki değişiklikleri yapın *haline*:
- Değiştir `UseGoogleAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:
 
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

### <a name="microsoft-account-authentication"></a>Microsoft hesabı kimlik doğrulaması
Aşağıdaki değişiklikleri yapın *haline*:
- Değiştir `UseMicrosoftAccountAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:

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
Aşağıdaki değişiklikleri yapın *haline*:
- Değiştir `UseTwitterAuthentication` yöntem çağrısı `Configure` yöntemiyle `UseAuthentication`:
 
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

### <a name="setting-default-authentication-schemes"></a>Varsayılan kimlik doğrulama şemasını ayarlama
1.x içinde `AutomaticAuthenticate` ve `AutomaticChallenge` özelliklerini [AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfı hedeflenen bir tek bir kimlik doğrulaması düzeni için ayarlanacak. Bu zorlamak için iyi hiçbir yolu yoktu.

2. 0 ', bu iki özellik tek tek özellikleri olarak kaldırılan `AuthenticationOptions` örneği. İçinde yapılandırılabilir `AddAuthentication` yöntem çağrısı içinde `ConfigureServices` yöntemi *haline*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Yukarıdaki kod parçacığında, varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").

Alternatif olarak, aşırı yüklenmiş bir sürümünü kullanmanız `AddAuthentication` birden çok özelliği ayarlamak için yöntem. Aşağıdaki aşırı yüklenmiş yöntemin örnekte varsayılan düzenini ayarlamak `CookieAuthenticationDefaults.AuthenticationScheme`. Kimlik doğrulama şeması alternatif olarak, tek tek içinde belirtilen `[Authorize]` öznitelikleri veya yetkilendirme ilkeleri.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Aşağıdaki koşullardan biri doğruysa varsayılan düzeni 2.0 tanımlayın:
- Otomatik olarak oturum açmanız kullanıcının istediğiniz
- Kullandığınız `[Authorize]` düzenleri belirtmeden özniteliği veya Yetkilendirme İlkeleri

Bu kural için bir istisna `AddIdentity` yöntemi. Bu yöntem ve varsayılan kimlik doğrulaması ve uygulama tanımlama bilgisine düzenleri sınama kümeleri için tanımlama bilgileri ekler `IdentityConstants.ApplicationScheme`. Ayrıca, oturum açma varsayılan düzeni için dış tanımlama bilgisi ayarlar `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>HttpContext kimlik doğrulama uzantıları kullanma
`IAuthenticationManager` Arabirimi 1.x kimlik doğrulaması sistemine ana giriş noktasıdır. Yeni bir kümesi ile değiştirilen `HttpContext` uzantı yöntemleri `Microsoft.AspNetCore.Authentication` ad alanı.

Örneğin, başvuru 1.x projeleri bir `Authentication` özelliği:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

2.0 projelerinde alma `Microsoft.AspNetCore.Authentication` ad alanı ve silme `Authentication` özelliği başvuruları:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows kimlik doğrulaması (HTTP.sys / IISIntegration)
Windows kimlik doğrulaması iki çeşidi vardır:
1. Ana bilgisayar kimliği doğrulanmış kullanıcılar yalnızca izin verir
2. Ana bilgisayar hem anonim verir ve kullanıcıların kimlik doğrulaması

Yukarıda açıklanan Birincisi 2.0 değişikliklerden etkilenmez.

Yukarıda açıklanan ikinci değişim 2.0 değişikliklerden etkilenir. Örnek olarak, anonim kullanıcılar uygulamanıza IIS sağlayarak veya [HTTP.sys](xref:fundamentals/servers/weblistener) katman denetleyici düzeyinde ancak yetki vererek kullanıcılar. Bu senaryoda, varsayılan düzenini ayarlamak `IISDefaults.AuthenticationScheme` içinde `ConfigureServices` yöntemi *haline*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Varsayılan düzen ayarlanamadı uygun şekilde çalışmasını sınama için authorize isteğinin engeller.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions Instances
2.0 değişiklikleri yan etkisi seçenekleri tanımlama bilgisi seçenekleri örnek yerine adlandırılmış kullanmanın anahtarıdır. Kimlik tanımlama bilgisi düzeni adları özelleştirme yeteneği kaldırılır.

Örneğin, 1.x projelerin [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) geçirmek için bir `IdentityCookieOptions` parametresine *AccountController.cs*. Dış tanımlama bilgisi kimlik doğrulama şeması sağlanan örneğinden erişilir:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Daha önce bahsedilen Oluşturucu ekleme 2.0 projelerinde gereksiz olur ve `_externalCookieScheme` alan silinebilir:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme` Sabiti doğrudan kullanılabilir:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>IdentityUser POCO Gezinti özellikleri ekleyin
Entity Framework (EF) çekirdek Gezinti özellikleri taban `IdentityUser` POCO (düz eski CLR nesnesi) kaldırıldı. El ile 1.x projenizi bu özellikleri kullandıysanız, bunları 2.0 projeye ekleyin:

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

Yinelenen yabancı anahtarlar EF çekirdek geçişler çalıştırırken önlemek için aşağıdakileri ekleyin, `IdentityDbContext` sınıfı `OnModelCreating` yöntemi (sonra `base.OnModelCreating();` çağrısı):

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

## <a name="replace-getexternalauthenticationschemes"></a>Replace GetExternalAuthenticationSchemes
Zaman uyumlu yöntemi `GetExternalAuthenticationSchemes` lehinde zaman uyumsuz bir sürümünü kaldırıldı. 1.x projeleri olmayan aşağıdaki kodu *ManageController.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Bu yöntem görünür *Login.cshtml* çok:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

2.0 projelerinde kullanma `GetExternalAuthenticationSchemesAsync` yöntemi:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

İçinde *Login.cshtml*, `AuthenticationScheme` erişilebilir özelliği `foreach` döngü değişiklikler `Name`:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel özellik değişikliği
A `ManageLoginsViewModel` nesne kullanılıyor `ManageLogins` eylemi *ManageController.cs*. 1.x projelerinde nesne 's `OtherLogins` özelliği döndürme türü `IList<AuthenticationDescription>`. Bu dönüş türü alma gerektirir `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

Dönüş türü değişikliklerini 2.0 projelerinde `IList<AuthenticationScheme>`. Bu yeni dönüş türü değiştirme gerektirir `Microsoft.AspNetCore.Http.Authentication` ile içeri aktarma bir `Microsoft.AspNetCore.Authentication` içeri aktarın.

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Ek Kaynaklar
Ek bilgi ve tartışma için bkz: [Auth 2.0 tartışma](https://github.com/aspnet/Security/issues/1338) github'da sorun.
