---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlayın ve özel değerleri kullanmak için kimlik özellikleri yapılandırmayı öğrenin."
manager: wpickett
ms.author: scaddie
ms.date: 02/21/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 6aeb85063b4b6f97822062b523a0c1f7ee6b595c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="configure-identity"></a>Kimliği yapılandırmak

ASP.NET Core kimliğini, parola ilkesi, kilitleme süresini ve tanımlama bilgisi ayarları gibi ayarları için varsayılan yapılandırmayı kullanır. Bu ayarlar uygulamanın geçersiz kılınabilir `Startup` sınıfı.

## <a name="identity-options"></a>Kimlik seçenekleri

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) sınıf kimlik sistemi yapılandırmak için kullanılan seçeneklerini temsil eder.

### <a name="claims-identity"></a>Talep Kimliği

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) belirtir [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Alır veya rol talep için kullanılan talep türünü ayarlar. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Alır veya güvenlik damgası talep için kullanılan talep türünü ayarlar. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Alır veya kullanıcı tanımlayıcısı talebi için kullanılan talep türünü ayarlar. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Alır veya kullanıcı adı talebi için kullanılan talep türünü ayarlar. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Kilitleme

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) belirtir [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği. | 5 dakika |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı. | 5 |

### <a name="password"></a>Parola

Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir. Parolalar en az altı karakter uzunluğunda olmalıdır. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) değiştirilebilir `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core eklenen 2.0 [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) özelliği. Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) belirtir [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Parola 0-9 arasında bir sayı gerektirir. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Minimum parola uzunluğu. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Yalnızca ASP.NET Core 2.0 veya sonraki bir sürüme uygulanır.<br><br> Parola ayrı karakter sayısını gerektirir. | 1. |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Bir küçük harf parola gerektirir. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Parolada alfasayısal olmayan karakter gerektirir. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Bir büyük harf karakter parola gerektirir. | `true` |

### <a name="sign-in"></a>oturum açma

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) belirtir [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Oturum açmak için bir onay e-posta gerektirir. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Oturum açmak onaylanan telefon numarası gerektirir. | `false` |

### <a name="tokens"></a>Belirteçler

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) belirtir [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Alır veya ayarlar `AuthenticatorTokenProvider` iki öğeli bir kimlik doğrulayıcı ile oturum bileşenler doğrulamak için kullanılır. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Alır veya ayarlar `ChangeEmailTokenProvider` e-posta değiştirme onayı e-postalarda kullanılan belirteçleri oluşturmak için kullanılır. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Alır veya ayarlar `ChangePhoneNumberTokenProvider` telefon numaralarını değiştirirken kullanılan belirteçleri oluşturmak için kullanılır. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Alır veya hesap doğrulama e-postalarda kullanılan belirteçleri oluşturmak için kullanılan belirteç sağlayıcısı ayarlar. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Alır veya ayarlar [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) parola sıfırlama e-postalarda kullanılan belirteçleri oluşturmak için kullanılır. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Oluşturmak için kullanılan bir [kullanıcı belirteci sağlayıcısı](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) anahtarla sağlayıcının adı olarak kullanılmıştır. |

### <a name="user"></a>Kullanıcı

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) belirtir [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) tabloda gösterilen özellikleri.

| Özellik | Açıklama | Varsayılan |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Kullanıcı adı izin verilen karakter. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Her kullanıcının benzersiz bir e-posta olmasını gerektirir. | `false` |

## <a name="cookie-settings"></a>Tanımlama bilgisi ayarları

Uygulamanın tanımlama bilgisini yapılandırın `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | İşleyici giden değiştirmeniz gerektiğini bildiren *403 Yasak* durum koduna bir *302 yeniden yönlendirme* verilen yolu üzerine.<br><br>Varsayılan değer `/Account/AccessDenied` şeklindedir. |
| [authenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Belirli kimlik doğrulaması düzeni için mantıksal adı. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> TRUE ise, kimlik doğrulaması ara yazılımı otomatik zorluklar işler. Yanlış kimlik doğrulaması ara yazılımı yalnızca açıkça belirtildiği zaman yanıtları değiştirir, `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Alır veya ayarlar oluşturulan herhangi bir talep için kullanılması gereken veren (devralınan [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Tanımlama bilgisi ile ilişkilendirilecek etki alanı. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Alır veya bir tanımlama bilgisi Sysprep'in ayarlar. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Bir tanımlama bilgisinin istemci tarafı komut dosyası tarafından erişilebilir olup olmadığını gösterir.<br><br>Varsayılan değer `true` şeklindedir. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Tanımlama bilgisinin adı.<br><br>Varsayılan değer `.AspNetCore.Cookies` şeklindedir. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Tanımlama bilgisi yolu. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | `SameSite` Tanımlama bilgisinin özniteliği.<br><br>Varsayılan değer [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) yapılandırma.<br><br>Varsayılan değer [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Tanımlama bilgisinin Burada sunulan etki alanı adı. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak.<br><br>Varsayılan değer `true` şeklindedir. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Aynı ana bilgisayar adını çalışan uygulamalar yalıtmak için kullanılır. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), ya da istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`).<br><br>Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Tanımlama bilgilerini istekten alma veya yanıtta belirleme için kullanılan bir bileşen. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Ayarlama, sağlayıcı tarafından kullanılan varsa [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) veri koruması. |
| [Açıklama](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | Yalnızca ASP.NET Core geçerlidir 1.x.<br><br> Uygulama için kullanılabilir hale getirileceğini kimlik doğrulama türü hakkında ek bilgi. |
| [Olaylar](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | İşleyici işleme oluştuğu bazı noktalarda uygulama denetimi veren sağlayıcıdaki yöntemleri çağırır. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Ayarlama, hizmet almak için tür `Events` örnek özellik yerine (devralınan [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Oluşturulduğu noktadan itibaren geçerli tanımlama bilgisi kalır depolanan kimlik doğrulaması bileti ne kadar zaman denetler.<br><br>Varsayılan değer 14 gündür. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilirsiniz.<br><br>Varsayılan değer `/Account/Login` şeklindedir. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | Bir kullanıcı oturum açıldığında, bunlar bu yolu yönlendirilirsiniz.<br><br>Varsayılan değer `/Account/Logout` şeklindedir. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler, bir *401 Yetkisiz* durum kodu değiştirildi bir *302 yeniden yönlendirme* oturum açma yoluna.<br><br>Varsayılan değer `ReturnUrl` şeklindedir. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | İsteklerinde kimlik depolanacağı isteğe bağlı bir kapsayıcı. |
| [SlidingExpiration değeri](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir.<br><br>Varsayılan değer `true` şeklindedir. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | `TicketDataFormat` Korumak ve kimliği ve tanımlama bilgisi değerinde depolanan diğer özellikleri korumasını kaldırmak için kullanılır. |
