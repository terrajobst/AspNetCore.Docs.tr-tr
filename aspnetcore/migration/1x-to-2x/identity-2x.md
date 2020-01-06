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
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>Kimlik doğrulaması ve kimliği 2,0 ASP.NET Core geçirin

[Scott Ade](https://github.com/scottaddie) ve [Hao Kung](https://github.com/HaoK) tarafından

ASP.NET Core [2,0, kimlik doğrulama ve kimlik](xref:security/authentication/identity) için Hizmetleri kullanarak yapılandırmayı basitleştiren yeni bir modeline sahiptir. Kimlik doğrulama veya kimlik kullanan ASP.NET Core 1. x uygulamaları, aşağıda özetlenen yeni modeli kullanacak şekilde güncelleştirilemeyebilir.

## <a name="update-namespaces"></a>Ad alanlarını güncelleştirme

1\. x içinde, `Microsoft.AspNetCore.Identity.EntityFrameworkCore` ad alanında `IdentityRole` ve `IdentityUser` sınıfları bulundu.

2,0 ' de <xref:Microsoft.AspNetCore.Identity> ad alanı bu tür sınıfların birkaçı için yeni giriş haline geldi. Varsayılan kimlik koduyla, etkilenen sınıflar `ApplicationUser` ve `Startup`içerir. Etkilenen başvuruları çözümlemek için `using` deyimlerinizi ayarlayın.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Kimlik doğrulama ara yazılımı ve Hizmetleri

1\. x projelerinde kimlik doğrulaması, ara yazılım aracılığıyla yapılandırılır. Desteklemek istediğiniz her kimlik doğrulama düzeni için bir ara yazılım yöntemi çağırılır.

Aşağıdaki 1. x örneği, Facebook kimlik doğrulamasını *Startup.cs*içinde kimliğiyle yapılandırır:

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

2,0 projesinde, kimlik doğrulaması hizmetler aracılığıyla yapılandırılır. Her kimlik doğrulama şeması, *Startup.cs*`ConfigureServices` yöntemine kaydedilir. `UseIdentity` yöntemi `UseAuthentication`ile değiştirilmiştir.

Aşağıdaki 2,0 örnek, Facebook kimlik doğrulamasını *Startup.cs*içinde kimliğiyle yapılandırır:

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

`UseAuthentication` yöntemi, otomatik kimlik doğrulamasından ve uzak kimlik doğrulama isteklerinin işlenmesinden sorumlu olan tek bir kimlik doğrulama ara yazılım bileşeni ekler. Tek, ortak bir ara yazılım bileşeniyle tüm bireysel ara yazılım bileşenlerini değiştirir.

Her bir büyük kimlik doğrulama şeması için 2,0 geçiş yönergeleri aşağıda verilmiştir.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması

Aşağıdaki iki seçenekten birini seçin ve *Startup.cs*'de gerekli değişiklikleri yapın:

1. Kimlik tanımlama bilgilerini kullanın
    - `Configure` yönteminde `UseAuthentication` `UseIdentity` değiştirin:

        ```csharp
        app.UseAuthentication();
        ```

    - Tanımlama bilgisi kimlik doğrulama hizmetlerini eklemek için `ConfigureServices` yönteminde `AddIdentity` yöntemi çağırın.
    - İsteğe bağlı olarak, kimlik tanımlama bilgisi ayarlarını ince ayar için `ConfigureServices` yönteminde `ConfigureApplicationCookie` veya `ConfigureExternalCookie` yöntemini çağırın.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Kimlik bilgilerini kimlik olmadan kullan
    - `Configure` yönteminde `UseCookieAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

        ```csharp
        app.UseAuthentication();
        ```

    - `ConfigureServices` yönteminde `AddAuthentication` ve `AddCookie` yöntemlerini çağırın:

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

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:
- `Configure` yönteminde `UseJwtBearerAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddJwtBearer` yöntemi çağırın:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Bu kod parçacığı kimlik kullanmaz, bu nedenle `AddAuthentication` yöntemine `JwtBearerDefaults.AuthenticationScheme` geçirerek varsayılan düzenin ayarlanması gerekir.

### <a name="openid-connect-oidc-authentication"></a>OpenID Connect (OıDC) kimlik doğrulaması

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:

- `Configure` yönteminde `UseOpenIdConnectAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddOpenIdConnect` yöntemi çağırın:

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

- `OpenIdConnectOptions` Eylemdeki `PostLogoutRedirectUri` özelliğini `SignedOutRedirectUri`ile değiştirin:

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Facebook kimlik doğrulaması

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:
- `Configure` yönteminde `UseFacebookAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddFacebook` yöntemi çağırın:

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google kimlik doğrulaması

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:
- `Configure` yönteminde `UseGoogleAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddGoogle` yöntemi çağırın:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Microsoft Hesabı kimlik doğrulaması

Microsoft hesabı kimlik doğrulaması hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore.Docs/issues/14455)bakın.

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:
- `Configure` yönteminde `UseMicrosoftAccountAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddMicrosoftAccount` yöntemi çağırın:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Twitter kimlik doğrulaması

*Startup.cs*içinde aşağıdaki değişiklikleri yapın:
- `Configure` yönteminde `UseTwitterAuthentication` yöntemi çağrısını `UseAuthentication`ile değiştirin:

    ```csharp
    app.UseAuthentication();
    ```

- `ConfigureServices` yönteminde `AddTwitter` yöntemi çağırın:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Varsayılan kimlik doğrulama düzenlerini ayarlama

1\. x içinde, [Authenticationoptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) temel sınıfının `AutomaticAuthenticate` ve `AutomaticChallenge` özellikleri tek bir kimlik doğrulama düzeninde ayarlamaya yöneliktir. Bunu zorlayacağından iyi bir yol yoktu.

2,0 ' de, bu iki özellik ayrı `AuthenticationOptions` örneğindeki özellikler olarak kaldırılmıştır. Bunlar, *Startup.cs*yönteminin `ConfigureServices` yöntemi içinde `AddAuthentication` Yöntem çağrısında yapılandırılabilirler:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Önceki kod parçacığında, varsayılan düzen `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies") olarak ayarlanır.

Alternatif olarak, birden fazla özellik ayarlamak için `AddAuthentication` yönteminin aşırı yüklenmiş bir sürümünü kullanın. Aşağıdaki aşırı yüklenmiş yöntem örneğinde, varsayılan düzen `CookieAuthenticationDefaults.AuthenticationScheme`olarak ayarlanmıştır. Kimlik doğrulama düzeni, farklı `[Authorize]` özniteliklerde veya yetkilendirme ilkeleriniz içinde de belirtilebilir.

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Aşağıdaki koşullardan biri doğru ise, 2,0 'de varsayılan bir düzen tanımlayın:
- Kullanıcının otomatik olarak oturum açabilmesi istiyorsunuz
- Şemaları belirtmeden `[Authorize]` özniteliğini veya yetkilendirme ilkelerini kullanın

Bu kural için bir özel durum `AddIdentity` yöntemidir. Bu yöntem sizin için tanımlama bilgileri ekler ve varsayılan kimlik doğrulama ve zorluk düzenlerini `IdentityConstants.ApplicationScheme`uygulama tanımlama bilgisine ayarlar. Ayrıca, varsayılan oturum açma düzenini `IdentityConstants.ExternalScheme`dış tanımlama bilgisine ayarlar.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>HttpContext kimlik doğrulama uzantılarını kullanma

`IAuthenticationManager` arabirimi, 1. x kimlik doğrulama sistemine ana giriş noktasıdır. `Microsoft.AspNetCore.Authentication` ad alanında yeni bir `HttpContext` uzantısı yöntemleriyle değiştirilmiştir.

Örneğin, 1. x projeleri bir `Authentication` özelliğine başvurur:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

2,0 projesinde `Microsoft.AspNetCore.Authentication` ad alanını içeri aktarın ve `Authentication` Özellik başvurularını silin:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows kimlik doğrulaması (HTTP. sys/Iisıntegration)

Windows kimlik doğrulamasının iki farklı çeşidi vardır:

* Konak yalnızca kimliği doğrulanmış kullanıcılara izin verir. Bu çeşitleme 2,0 değişikliklerden etkilenmez.
* Konak hem anonim hem de kimliği doğrulanmış kullanıcılara izin verir. Bu çeşitleme 2,0 değişikliklerden etkilenir. Örneğin, uygulamanın [IIS](xref:host-and-deploy/iis/index) veya [http. sys](xref:fundamentals/servers/httpsys) katmanında anonim kullanıcılara izin verilmesi, ancak kullanıcıları denetleyici düzeyinde yetkilendirmeniz gerekir. Bu senaryoda, `Startup.ConfigureServices` yönteminde varsayılan düzeni ayarlayın.

  [Microsoft. AspNetCore. Server. ıısıntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)için varsayılan düzeni `IISDefaults.AuthenticationScheme`olarak ayarlayın:

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)için varsayılan düzeni `HttpSysDefaults.AuthenticationScheme`olarak ayarlayın:

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  Varsayılan düzenin ayarlanamaması, yetkilendirme (sınama) isteğinin şu özel durumla çalışmasını engeller:

  > `System.InvalidOperationException`: authenticationScheme belirtilmedi ve hiç Defaultchallenbescheme bulunamadı.

Daha fazla bilgi için bkz. <xref:security/authentication/windowsauth>.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>Identityıeoptions örnekleri

2,0 değişikliğin yan etkisi, tanımlama bilgisi seçenekleri örnekleri yerine adlandırılmış seçenekleri kullanma anahtarıdır. Kimlik tanımlama bilgisi düzen adlarını özelleştirme özelliği kaldırılır.

Örneğin, 1. x projeleri bir `IdentityCookieOptions` parametresini *accountcontroller.cs* ve *ManageController.cs*öğesine geçirmek için [Oluşturucu Ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) kullanır. Dış tanımlama bilgisi kimlik doğrulama şemasına, belirtilen örnekten erişilir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Belirtilen Oluşturucu Ekleme, 2,0 projesinde gereksiz hale gelir ve `_externalCookieScheme` alanı silinebilir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

1. x projeleri `_externalCookieScheme` alanını şu şekilde kullandı:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

2,0 projesinde, yukarıdaki kodu aşağıdaki kodla değiştirin. `IdentityConstants.ExternalScheme` sabiti doğrudan kullanılabilir.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

Aşağıdaki ad alanını içeri aktararak yeni eklenen `SignOutAsync` çağrısını çözümleyin:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Identityuser POCO gezinti özellikleri ekle

Taban `IdentityUser` POCO (düz eski CLR nesnesi) Entity Framework (EF) temel gezinti özellikleri kaldırılmıştır. 1\. x projeniz bu özellikleri kullandıysanız, bunları el ile 2,0 projesine ekleyin:

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

EF Core geçişleri çalıştırırken yinelenen yabancı anahtarların oluşmasını engellemek için, aşağıdakileri `IdentityDbContext` sınıfınıza ' `OnModelCreating` metoduna ekleyin (`base.OnModelCreating();` çağrısından sonra):

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

## <a name="replace-getexternalauthenticationschemes"></a>Getexternalauthenticationdüzenlerini değiştirme

Zaman uyumlu Yöntem `GetExternalAuthenticationSchemes` zaman uyumsuz bir sürüm yararına kaldırılmıştır. 1. x projeleri *denetleyicileri/ManageController. cs*dosyasında aşağıdaki koda sahiptir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Bu yöntem, *Görünümler/Account/Login. cshtml* içinde de görüntülenir:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

2,0 projesinde <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> yöntemini kullanın. *ManageController.cs* içindeki değişiklik aşağıdaki koda benzer:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

*Login. cshtml*içinde, `foreach` döngüsünde erişilen `AuthenticationScheme` özelliği `Name`olarak değişir:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel özelliği değişikliği

Bir `ManageLoginsViewModel` nesnesi, *ManageController.cs*`ManageLogins` eyleminde kullanılır. 1\. x projelerinde, nesnenin `OtherLogins` özelliği dönüş türü `IList<AuthenticationDescription>`. Bu dönüş türü `Microsoft.AspNetCore.Http.Authentication`içeri aktarmayı gerektirir:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

2,0 projesinde, dönüş türü `IList<AuthenticationScheme>`olarak değişir. Bu yeni dönüş türü, `Microsoft.AspNetCore.Http.Authentication` içeri aktarmanın `Microsoft.AspNetCore.Authentication` içeri aktarma ile değiştirilmesi gerekiyor.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. GitHub 'da [kimlik doğrulama 2,0 sorunu tartışması](https://github.com/aspnet/Security/issues/1338) .
