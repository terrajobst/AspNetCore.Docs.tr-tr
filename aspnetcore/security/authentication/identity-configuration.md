---
title: ASP.NET Core kimliğini yapılandırma
author: AdrienTorris
description: ASP.NET Core kimlik varsayılan değerlerini anlayın ve kimlik özelliklerini özel değerleri kullanacak şekilde yapılandırmayı öğrenin.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661408"
---
# <a name="configure-aspnet-core-identity"></a>ASP.NET Core kimliğini yapılandırma

ASP.NET Core kimlik, parola ilkesi, kilitleme ve tanımlama bilgisi yapılandırması gibi ayarların varsayılan değerlerini kullanır. Bu ayarlar `Startup` sınıfında geçersiz kılınabilir.

## <a name="identity-options"></a>Kimlik seçenekleri

[Identityoptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) sınıfı, kimlik sistemini yapılandırmak için kullanılabilen seçenekleri temsil eder. `IdentityOptions`, `AddIdentity` veya `AddDefaultIdentity`çağrıldıktan **sonra** ayarlanmalıdır.

### <a name="claims-identity"></a>Talep kimliği

[Identityoptions. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) , aşağıdaki tabloda gösterilen özelliklerle [Claimsıdentityoptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) 'ı belirtir.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Rol talebi için kullanılan talep türünü alır veya ayarlar. | [ClaimTypes. Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Güvenlik damga talebi için kullanılan talep türünü alır veya ayarlar. | `AspNet.Identity.SecurityStamp` |
| [Userıdclaimtype](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Kullanıcı tanımlayıcısı talebi için kullanılan talep türünü alır veya ayarlar. | [ClaimTypes. NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Kullanıcı adı talebi için kullanılan talep türünü alır veya ayarlar. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Kilitleme

[Passwordsignınasync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) yönteminde kilitleme ayarlanır:

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Yukarıdaki kod `Login` Identity şablonunu temel alır. 

Kilitleme seçenekleri `StartUp.ConfigureServices`olarak ayarlanır:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Önceki kod, [ıdentityoptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [lockoutoptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) değerlerini varsayılan değerlerle ayarlar.

Başarılı bir kimlik doğrulaması başarısız erişim denemesi sayısını sıfırlar ve saatini sıfırlar.

[Identityoptions. kilitleme](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) , tabloda gösterilen özelliklerle [Lockoutoptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) değerini belirtir.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Yeni bir kullanıcının kilitlenip kilitlenmeyeceğini belirler. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Bir kilitleme gerçekleştiğinde kullanıcının kilitlediği zaman miktarı. | 5 dakika |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Kilitleme etkinse, bir Kullanıcı kilitlenene kadar başarısız olan erişim girişimlerinin sayısı. | 5 |

### <a name="password"></a>Parola

Varsayılan olarak, kimlik, parolaların büyük harf, küçük harf, rakam ve alfasayısal olmayan bir karakter içermesini gerektirir. Parolalar en az altı karakter uzunluğunda olmalıdır. [Passwordoseçenekleri](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) `Startup.ConfigureServices`içinde ayarlanabilir.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[Identityoptions. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) , tabloda gösterilen özelliklerle birlikte [passwordotions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) belirler.

::: moniker range=">= aspnetcore-2.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Parolada 0-9 arasında bir sayı gerekir. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Parola uzunluğu alt sınırı. | 6 |
| [Requireküçük harf](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Parolada küçük harfli bir karakter gerektirir. | `true` |
| [Talep ırenonalfasayısal](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Parolada alfasayısal olmayan bir karakter gerektirir. | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Yalnızca ASP.NET Core 2,0 veya üzeri için geçerlidir.<br><br> Paroladaki ayrı karakter sayısını gerektirir. | 1\. |
| [Requirebüyük](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Parolada büyük harfli bir karakter gerektirir. | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Parolada 0-9 arasında bir sayı gerekir. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Parola uzunluğu alt sınırı. | 6 |
| [Requireküçük harf](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Parolada küçük harfli bir karakter gerektirir. | `true` |
| [Talep ırenonalfasayısal](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Parolada alfasayısal olmayan bir karakter gerektirir. | `true` |
| [Requirebüyük](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Parolada büyük harfli bir karakter gerektirir. | `true` |

::: moniker-end

### <a name="sign-in"></a>Oturum açma

Aşağıdaki kod `SignIn` ayarlarını ayarlar (varsayılan değerlere):

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[Identityoptions. signıd](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) , tabloda gösterilen özelliklerle [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 'ı belirtir.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Oturum açmak için onaylanan bir e-posta gerekir. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Oturum açmak için onaylanmış bir telefon numarası gerektirir. | `false` |

### <a name="tokens"></a>Belirteçler

[Identityoptions. Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) , tabloda gösterilen özelliklerle birlikte [tokenoptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) belirler.

|                                                        Özellik                                                         |                                                                                      Açıklama                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [Kimlik doğrulayan Tortokenprovider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       İki öğeli oturum açma işlemlerini kimlik doğrulayıcı ile doğrulamak için kullanılan `AuthenticatorTokenProvider` alır veya ayarlar.                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     E-posta değişikliği onay e-postalarında kullanılan belirteçleri oluşturmak için kullanılan `ChangeEmailTokenProvider` alır veya ayarlar.                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      Telefon numaralarını değiştirirken kullanılan belirteçleri oluşturmak için kullanılan `ChangePhoneNumberTokenProvider` alır veya ayarlar.                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             Hesap onaylama e-postalarında kullanılan belirteçleri oluşturmak için kullanılan belirteç sağlayıcısını alır veya ayarlar.                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | Parola sıfırlama e-postalarında kullanılan belirteçleri oluşturmak için kullanılan [ıusertwofactortokenprovider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) alır veya ayarlar. |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                Sağlayıcının adı olarak kullanılan anahtarla bir [Kullanıcı belirteci sağlayıcısı](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) oluşturmak için kullanılır.                 |

### <a name="user"></a>Kullanıcı

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[Identityoptions. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) , tabloda gösterilen özelliklerle [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 'ı belirtir.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Kullanıcı adında izin verilen karakterler. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Her kullanıcının benzersiz bir e-postasına sahip olmasını gerektirir. | `false` |

### <a name="cookie-settings"></a>Tanımlama bilgisi ayarları

`Startup.ConfigureServices`içinde uygulamanın tanımlama bilgilerini yapılandırın. `AddIdentity` veya `AddDefaultIdentity`çağrıldıktan **sonra** [configureapplicationcookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) çağrılmalıdır.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

Daha fazla bilgi için bkz. [tanımlama, ıeauthenticationoptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Parola hasher seçenekleri

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> parola karmalama seçeneklerini alır ve ayarlar.

| Seçenek | Açıklama |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | Yeni parolalar Karma oluşturulurken kullanılan uyumluluk modu. <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3> değerini varsayılan olarak alır. Karma parolanın *Biçim işaretleyicisi*olarak adlandırılan ilk baytı, parolayı karma olarak kullanılan karma algoritmanın sürümünü belirtir. Bir karmaya karşı bir parola doğrulanırken <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> yöntemi ilk bayta göre doğru algoritmayı seçer. Bir istemci, parolayı karma hale almak için hangi algoritma sürümünün kullanıldığına bakılmaksızın kimlik doğrulaması yapabilir. Uyumluluk modunun ayarlanması *yeni parolaların*karmasını etkiler. |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | PBKDF2 kullanarak parolaları karmalama sırasında kullanılan yineleme sayısı. Bu değer yalnızca <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>olarak ayarlandığında kullanılır. Değerin pozitif bir tamsayı olması ve varsayılan olarak `10000`olması gerekir. |

Aşağıdaki örnekte <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount>, `Startup.ConfigureServices``12000` olarak ayarlanmıştır:

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
