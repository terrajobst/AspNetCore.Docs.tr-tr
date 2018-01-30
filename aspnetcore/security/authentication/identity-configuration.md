---
title: "ASP.NET Core kimliğini yapılandırmak"
author: AdrienTorris
description: "ASP.NET Core kimlik varsayılan değerleri anlamak ve özel değerleri kullanmak için çeşitli kimlik özelliklerini yapılandırın."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a>Kimliği yapılandırmak

ASP.NET Core kimliği parola ilkesi, kilitleme süresini ve uygulamanızın kolayca kılabilirsiniz tanımlama bilgisi ayarları gibi uygulamalarda ortak davranışları sahip `Startup` sınıfı.

## <a name="passwords-policy"></a>Parola İlkesi

Varsayılan olarak, parolalar bir büyük harf, küçük harf, rakam ve alfasayısal olmayan karakter içerdiğini kimlik gerektirir. Diğer bazı kısıtlamalar vardır. Parola kısıtlamaları basitleştirmek için değişiklik `ConfigureServices` yöntemi `Startup` uygulamanızın sınıfı.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core eklenen 2.0 `RequiredUniqueChars` özelliği. Aksi takdirde, ASP.NET Core aynı seçeneklerdir 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`aşağıdaki özelliklere sahiptir:

| Özellik                | Açıklama                       | Varsayılan |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Parola 0-9 arasında bir sayı gerektirir. | true |
| `RequiredLength`        | Minimum parola uzunluğu. | 6 |
| `RequireNonAlphanumeric`| Parolada alfasayısal olmayan karakter gerektirir. | true |
| `RequireUppercase`      | Bir büyük harf karakter parola gerektirir. | true |
| `RequireLowercase`      | Parola küçük harf karakter gerektirir. | true |
| `RequiredUniqueChars`   | Parola ayrı karakter sayısını gerektirir. | 1. |


## <a name="users-lockout"></a>Kullanıcının kilitleme

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`aşağıdaki özelliklere sahiptir:

| Özellik                | Açıklama                       | Varsayılan |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | Bir kilitleme oluştuğunda süreyi bir kullanıcının kilitlendiği.  | 5 dakika  |
| `MaxFailedAccessAttempts` | Kilitleme etkinse, bir kullanıcı kilitli kadar başarısız erişim denemesi sayısı.  | 5 |
| `AllowedForNewUsers` | Yeni bir kullanıcı kilitlenip kilitlenmeyeceği olmadığını belirler.  | true |

## <a name="sign-in-settings"></a>Ayarları'nda oturum açın

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`aşağıdaki özelliklere sahiptir:

| Özellik                | Açıklama                       | Varsayılan |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Oturum açmak için bir onay e-posta gerektirir. | false  |
| `RequireConfirmedPhoneNumber` |  Oturum açmak onaylanan telefon numarası gerektirir. | false  |

## <a name="user-validation-settings"></a>Kullanıcı doğrulama ayarları

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`aşağıdaki özelliklere sahiptir:

| Özellik                | Açıklama                       | Varsayılan |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Her kullanıcının benzersiz bir e-posta olmasını gerektirir. | false  |
| `AllowedUserNameCharacters`  | Kullanıcı adı izin verilen karakter. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Uygulamanın tanımlama bilgisi ayarları

Parola İlkesi gibi uygulama tanımlama bilgisinin tüm ayarları değiştirilebilir `Startup` sınıfı.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Altında `ConfigureServices` içinde `Startup` sınıfı, uygulamanın tanımlama bilgisi yapılandırabilirsiniz.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`aşağıdaki özelliklere sahiptir:

| Özellik                | Açıklama                       | Varsayılan |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Tanımlama bilgisinin adı.  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Doğru olduğunda, tanımlama bilgisinin istemci tarafı komut dosyalarından erişilebilir değil.  |  true |
| `ExpireTimeSpan`  | Kimlik doğrulaması bileti tanımlama bilgisinde saklanan ne kadar süre oluşturulduğu noktadan itibaren geçerli kalacağını denetler.  | 14 gün  |
| `LoginPath`  | Bir kullanıcı yetkisiz olduğunda, bu oturum açma yoluna yönlendirilir. | / Hesap/oturum açma  |
| `LogoutPath`  | Bir kullanıcı oturum açıldığında, bu yolu yönlendirilir.  | / Hesap/oturum kapatma  |
| `AccessDeniedPath`  | Bir kullanıcı bir yetkilendirme denetim başarısız olduğunda, bu yolu yönlendirilir.  |   |
| `SlidingExpiration`  | Doğru olduğunda, yeni bir tanımlama bilgisi zaman geçerli tanımlama bilgisi sona erme penceresinin yarısından fazlasını ilerlemiş yeni bir sona erme saati ile verilir.  | /Account/AccessDenied |
| `ReturnUrlParameter`  | Bir 401 yetkilendirilmedi durum kodu oturum açma yoluna 302 yeniden yönlendirme olarak değiştirildiğinde, hangi ara yazılım tarafından eklenen sorgu dizesi parametresinin adını belirler.  |  true |
| `AuthenticationScheme`  | Bu yalnızca ASP.NET Core için ilgili 1.x. Belirli kimlik doğrulaması düzeni için mantıksal adı. |  |
| `AutomaticAuthenticate`  | Bu bayrak yalnızca ASP.NET Core için ilgili 1.x. Doğru olduğunda, tanımlama bilgisi kimlik doğrulamasını her istek çalıştırın ve oluşturulan herhangi bir seri hale getirilmiş asıl yeniden yapılandırma ve doğrulama girişimi.  |  |
