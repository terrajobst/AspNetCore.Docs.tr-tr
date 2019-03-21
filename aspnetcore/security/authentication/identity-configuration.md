---
title: ASP.NET Core kimliği yapılandırma
author: AdrienTorris
description: ASP.NET Core kimliği varsayılan değerleri anlamanıza ve özel değerler kullanılacak kimlik özelliklerini yapılandırmayı öğrenin.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210125"
---
# <a name="configure-aspnet-core-identity"></a>ASP.NET Core kimliği yapılandırma

ASP.NET Core kimliği, parola ilkesi ve kilitleme tanımlama bilgisi yapılandırması gibi ayarları için varsayılan değerleri kullanır. Bu ayarlar kılınabilir `Startup` sınıfı.

## <a name="identity-options"></a>Kimlik seçenekleri

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) sınıf kimlik sistemi yapılandırmak için kullanılabilir seçenekleri temsil eder. `IdentityOptions` ayarlanmalıdır **sonra** çağırma `AddIdentity` veya `AddDefaultIdentity`.

### <a name="claims-identity"></a>Talep Kimliği

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) belirtir [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) aşağıdaki tabloda gösterilen özelliklere sahip.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Alır veya bir rolü talep için kullanılan talep türünü ayarlar. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Alır veya güvenlik damgası talep için kullanılan talep türünü ayarlar. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Alır veya kullanıcı tanımlayıcısı talebi için kullanılan talep türünü ayarlar. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Alır veya kullanıcı adı talebi için kullanılan talep türünü ayarlar. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Kilitleme

Kilitleme kümesinde [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) yöntemi:

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Yukarıdaki kod dayanır `Login` kimlik şablonu. 

Kilitleme seçenekleri ayarlanır `StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Önceki kod kümeleri [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) varsayılan değerlere sahip.

Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) belirtir [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) tabloda gösterilen özelliklere sahip.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Yeni bir kullanıcı, kilitlenir belirler. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Bir kilitlenme oluştuğunda süreyi kullanıcı kilitlidir. | 5 dakika |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı. | 5 |

### <a name="password"></a>Parola

Varsayılan olarak parola bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir. Parola en az altı karakter uzunluğunda olmalıdır. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) ayarlanabilir `Startup.ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) belirtir [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) tabloda gösterilen özelliklere sahip.

::: moniker range=">= aspnetcore-2.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Parolayı 0-9 arasında bir sayı gerektirir. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimum parola uzunluğu. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Parola küçük harf karakter gerektirir. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Paroladaki alfasayısal olmayan bir karakter gerektirir. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Yalnızca ASP.NET Core 2.0 ve üzeri için geçerlidir.<br><br> Parola ayrı karakter sayısı gerektirir. | 1. |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Parola bir büyük harf karakteri gerektirir. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Parolayı 0-9 arasında bir sayı gerektirir. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimum parola uzunluğu. | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Parola küçük harf karakter gerektirir. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Paroladaki alfasayısal olmayan bir karakter gerektirir. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Parola bir büyük harf karakteri gerektirir. | `true` |

::: moniker-end

### <a name="sign-in"></a>oturum açma

Aşağıdaki kod kümeleri `SignIn` ayarlarına (varsayılan değer):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) belirtir [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) tabloda gösterilen özelliklere sahip.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Oturum açmak için bir onaylanan e-posta gerektirir. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Oturum açmak onaylanan telefon numarası gerektirir. | `false` |

### <a name="tokens"></a>Belirteçler

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) belirtir [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) tabloda gösterilen özelliklere sahip.

|                                                        Özellik                                                         |                                                                                      Açıklama                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       Alır veya ayarlar `AuthenticatorTokenProvider` iki öğeli bir kimlik doğrulayıcı ile oturum açma işlemleri doğrulamak için kullanılır.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     Alır veya ayarlar `ChangeEmailTokenProvider` e-posta değişikliği onay e-postalarda kullanılan belirteçleri oluşturmak için kullanılır.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Alır veya ayarlar `ChangePhoneNumberTokenProvider` telefon numaralarını değiştirirken kullanılan belirteçleri oluşturmak için kullanılır.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Alır veya hesap doğrulama e-postalarda kullanılan belirteçleri oluşturmak için kullanılan belirteç sağlayıcısı ayarlar.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Alır veya ayarlar [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) parola sıfırlama e-postalarda kullanılan belirteçleri oluşturmak için kullanılır. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Oluşturmak için kullanılan bir [kullanıcı belirteci sağlayıcısı](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) anahtarla sağlayıcının adı olarak kullanılır.                 |

### <a name="user"></a>Kullanıcı

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) belirtir [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) tabloda gösterilen özelliklere sahip.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Kullanıcı adı izin verilen karakter. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Her kullanıcının benzersiz bir e-posta olmasını gerektirir. | `false` |

### <a name="cookie-settings"></a>Tanımlama bilgisi ayarları

Uygulamanın tanımlama bilgisini yapılandırın `Startup.ConfigureServices`. [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) çağrılmalıdır **sonra** çağırma `AddIdentity` veya `AddDefaultIdentity`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Daha fazla bilgi için [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Parola karma değeri Oluşturucusu seçenekleri

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> parola karma seçeneklerini ayarlar ve alır.

| Seçenek | Açıklama |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Yeni parola kararken kullanılan uyumluluk modu. Varsayılan olarak <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Adlı bir karma hale getirilen parolayla ilk baytına bir *biçimi işaret*, parola karması için kullanılan karma algoritması sürümünü belirtir. Parola Karması karşı doğrulanırken <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> yöntemi ilk baytın üzerinde göre doğru algoritmayı seçer. Bir istemci bakılmaksızın kimlik doğrulaması için parola karma hale hangi algoritmanın sürümü kullanıldı. Uyumluluk modu ayarını etkiler, karma *yeni parolalar*. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | Parolaları PBKDF2 kullanarak kararken kullanılan yineleme sayısı. Bu değer, yalnızca kullanılan zaman <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> ayarlanır <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>. Değer pozitif bir tamsayı ve varsayılan olarak olmalıdır `10000`. |

Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> ayarlanır `12000` içinde `Startup.ConfigureServices`:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
