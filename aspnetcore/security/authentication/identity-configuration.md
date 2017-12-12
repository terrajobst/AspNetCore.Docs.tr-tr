---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlamak ve özel değerleri kullanmak için çeşitli kimlik özelliklerini yapılandırın."
keywords: "ASP.NET Core, kimlik, kimlik doğrulaması, güvenlik"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a>Kimliği yapılandırmak

ASP.NET Core kimliği vardır, uygulamanızın kolayca kılabilirsiniz bazı varsayılan davranışlar `Startup` sınıfı.

## <a name="passwords-policy"></a>Parola İlkesi

Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir. Diğer bazı kısıtlamalar vardır. Parola kısıtlamaları basitleştirmek istiyorsanız, bunu yapabilirsiniz `Startup` uygulamanızın sınıfı.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

ASP.NET Core eklenen 2.0 `RequiredUniqueChars` özelliği. Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`aşağıdaki özelliklere sahiptir:
* `RequireDigit`: 0-9 parolayı arasında bir sayı gerektirir. Varsayılan olarak true.
* `RequiredLength`: Minimum parola uzunluğu. Varsayılan olarak 6.
* `RequireNonAlphanumeric`: Bir alfasayısal olmayan karakter parola gerektirir. Varsayılan olarak true.
* `RequireUppercase`: Bir büyük harf karakter parola gerektirir. Varsayılan olarak true.
* `RequireLowercase`: Bir küçük harf karakter parola gerektirir. Varsayılan olarak true.
* `RequiredUniqueChars`: Parola ayrı karakter sayısını gerektirir. Varsayılan olarak 1.


## <a name="users-lockout"></a>Kullanıcının kilitleme

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`aşağıdaki özelliklere sahiptir:
* `DefaultLockoutTimeSpan`: Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği. Varsayılan olarak 5 dakika.
* `MaxFailedAccessAttempts`: Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı. Varsayılan olarak 5.
* `AllowedForNewUsers`: Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler. Varsayılan olarak true.


## <a name="sign-in-settings"></a>Ayarları'nda oturum açın

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`aşağıdaki özelliklere sahiptir:
* `RequireConfirmedEmail`: Oturum açmak için bir onay e-posta gerektirir. Varsayılan ayar FALSE seçeneğidir.
* `RequireConfirmedPhoneNumber`: Oturum açmak onaylanan telefon numarası gerektirir. Varsayılan ayar FALSE seçeneğidir.


## <a name="user-validation-settings"></a>Kullanıcı doğrulama ayarları

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`aşağıdaki özelliklere sahiptir:
* `RequireUniqueEmail`: Her kullanıcının benzersiz bir e-posta olmasını gerektirir. Varsayılan ayar FALSE seçeneğidir.
* `AllowedUserNameCharacters`: Kullanıcı adı izin verilen karakter. Varsayılan olarak abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Uygulamanın tanımlama bilgisi ayarları

Parola İlkesi gibi uygulama tanımlama bilgisinin tüm ayarları değiştirilebilir `Startup` sınıfı.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Altında `ConfigureServices` içinde `Startup` sınıfı, uygulamanın tanımlama bilgisi yapılandırabilirsiniz.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`aşağıdaki özelliklere sahiptir:
* `Cookie.Name`: Tanımlama bilgisinin adı. Varsayılan olarak alır. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Doğru olduğunda, tanımlama bilgisinin istemci tarafı komut dosyalarından erişilebilir değil. Varsayılan olarak true.
* `ExpireTimeSpan`: Kimlik doğrulaması bileti tanımlama bilgisinde saklanan ne kadar süre oluşturulduğu noktadan itibaren geçerli kalacağını denetler. Varsayılan olarak 14 gün.
* `LoginPath`: Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilir. Varsayılan olarak/Account/oturum açma.
* `LogoutPath`: Bir kullanıcı oturum açtığında bu yoluna yönlendirilir. Varsayılan olarak/Account/oturum kapatma.
* `AccessDeniedPath`: Bir kullanıcı bir yetkilendirme denetim başarısız olduğunda, bu yolu yönlendirilir. Varsayılan olarak/Account/erişim engellendi.
* `SlidingExpiration`: Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir. Varsayılan olarak true.
* `ReturnUrlParameter`: ReturnUrlParameter bir 401 yetkilendirilmedi durum kodu oturum açma yoluna 302 yeniden yönlendirme olarak değiştirildiğinde, hangi ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler.
* `AuthenticationScheme`: Bu yalnızca ASP.NET Core için ilgili 1.x. Belirli kimlik doğrulaması düzeni için mantıksal adı.
* `AutomaticAuthenticate`: Bu bayrağı yalnızca ASP.NET Core için ilgili 1.x. Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi.

