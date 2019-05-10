---
title: Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı
author: guardrex
description: Ek talep ve dış sağlayıcılardan gelen belirteçleri oluşturma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 37c7a51217576669bcaed79d4a212e6412aa8945
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903128"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulaması ek talep ve Facebook, Google, Microsoft ve Twitter gibi dış kimlik sağlayıcılardan gelen belirteçleri oluşturabilirsiniz. Her bir sağlayıcı platformu farklı kullanıcılar hakkında bilgi gösterir, ancak alma ve kullanıcı verilerini ek talepleri dönüştürme için desen aynıdır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Uygulamayı desteklemek için hangi Dış kimlik doğrulama sağlayıcıları karar verin. Tüm sağlayıcılar için uygulamayı kaydetme ve bir istemci kimliği ve istemci gizli anahtarını alın. Daha fazla bilgi için bkz. <xref:security/authentication/social/index>. [Örnek uygulaması](#sample-app-instructions) kullanan [Google kimlik doğrulama sağlayıcısı](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>İstemci Kimliğini ve istemci gizli dizisi ayarlayın

OAuth kimlik doğrulama sağlayıcısı, bir istemci kimliği ve istemci gizli anahtarı kullanarak bir uygulama ile bir güven ilişkisi oluşturur. Uygulamayı sağlayıcıda kaydedildiğinde istemci Kimliğini ve istemci gizli değerleri uygulama için Dış kimlik doğrulama sağlayıcısı tarafından oluşturulur. Uygulamanın kullandığı her bir dış sağlayıcı bağımsız olarak sağlayıcının istemci Kimliğini ve istemci gizli anahtarı ile yapılandırılması gerekir. Daha fazla bilgi için senaryonuz için geçerli olan dış kimlik doğrulama sağlayıcısı konulara bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Google kimlik doğrulaması](xref:security/authentication/google-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer kimlik doğrulama sağlayıcıları](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Örnek uygulama, bir istemci kimliği ve istemci gizli anahtarı Google tarafından sağlanan Google kimlik doğrulama sağlayıcısı yapılandırır:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a>Kimlik doğrulama kapsamı oluşturmak

Belirterek sağlayıcıdan almak için izinler listesinden belirtin <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Yaygın dış sağlayıcıları için kimlik doğrulaması kapsamları aşağıdaki tabloda görüntülenir.

| Sağlayıcı  | Kapsam                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Google örnek uygulamanın eklediği `plus.login` Google + oturum açma izinleri istemek için kapsamı:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a>Kullanıcı veri anahtarları eşleyin ve talep oluşturma

Sağlayıcının seçeneklerinde belirtin bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> her anahtarın dış sağlayıcının JSON kullanıcı verileri için oturum açma okumak uygulama kimliği. Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.

Örnek uygulamayı oluşturur bir <xref:System.Security.Claims.ClaimTypes.Gender> gelen talep `gender` Google kullanıcı verileri anahtar:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

İçinde <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>e <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ile uygulama oturum açmış <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601> depolayabilirsiniz bir `ApplicationUser` bulunan kullanıcı verileri için talep <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) oluşturan bir <xref:System.Security.Claims.ClaimTypes.Gender> imzalı için talep içinde `ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a>Erişim belirtecini kaydetme

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> erişim ve yenileme belirteçleri de depolanıp depolanmayacağını tanımlar <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> başarılı yetkilendirme sonrasında. `SaveTokens` ayarlanır `false` son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak.

Örnek uygulamayı değerini ayarlar `SaveTokens` için `true` içinde <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

Zaman `OnPostConfirmationAsync` yürütür, depolama ve erişim belirteci ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dış sağlayıcıdan `ApplicationUser`'s `AuthenticationProperties`.

Örnek uygulama, erişim belirtecinde kaydeder:

* `OnPostConfirmationAsync` &ndash; Yeni kullanıcı kaydı için yürütür.
* `OnGetCallbackAsync` &ndash; Önceden kaydedilmiş kullanıcı uygulamada oturum açtığında yürütür.

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a>Ek özel belirteçler ekleme

Bir parçası olarak depolanan özel bir belirteç ekleme göstermek için `SaveTokens`, örnek uygulamayı ekler bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> geçerli <xref:System.DateTime> için bir [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) , `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>Örnek uygulama yönergeleri

Örnek uygulamayı gösterir nasıl yapılır:

* Kullanıcının cinsiyeti Google'dan alın ve bir cinsiyet talebi değeri depolar.
* Google erişim belirtecini kullanıcının Store `AuthenticationProperties`.

Örnek uygulamayı kullanmak için:

1. Uygulamayı kaydetme ve geçerli istemci Kimliğini ve Google kimlik doğrulaması için istemci parolası alın. Daha fazla bilgi için bkz. <xref:security/authentication/google-logins>.
1. İstemci Kimliğini ve istemci gizli anahtarı'nda sağlamak <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> , `Startup.ConfigureServices`.
1. Uygulamayı çalıştırın ve My talep sayfası isteme. Kullanıcı oturum açmadı, uygulama için Google yönlendirir. Google ile oturum aç. Google kullanıcı uygulamaya geri yönlendirir (`/Home/MyClaims`). Kullanıcının kimliği doğrulanır ve My talep sayfası yüklenir. Cinsiyet talep altında **kullanıcı taleplerini** Google'dan alınan değer. Erişim belirteci görünür **kimlik doğrulaması özelliklerini**.

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
