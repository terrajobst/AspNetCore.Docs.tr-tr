---
title: Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı
author: guardrex
description: Ek talep ve dış sağlayıcılardan gelen belirteçleri oluşturma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874853"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>Ek talep ve ASP.NET Core, dış sağlayıcılardan gelen belirteçleri kalıcı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulaması ek talep ve Facebook, Google, Microsoft ve Twitter gibi dış kimlik sağlayıcılardan gelen belirteçleri oluşturabilirsiniz. Her bir sağlayıcı platformu farklı kullanıcılar hakkında bilgi gösterir, ancak alma ve kullanıcı verilerini ek talepleri dönüştürme için desen aynıdır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Uygulamayı desteklemek için hangi Dış kimlik doğrulama sağlayıcıları karar verin. Tüm sağlayıcılar için uygulamayı kaydetme ve bir istemci kimliği ve istemci gizli anahtarını alın. Daha fazla bilgi için bkz. <xref:security/authentication/social/index>. Örnek uygulama kullandığı [Google kimlik doğrulama sağlayıcısı](xref:security/authentication/google-logins).

## <a name="set-the-client-id-and-client-secret"></a>İstemci Kimliğini ve istemci gizli dizisi ayarlayın

OAuth kimlik doğrulama sağlayıcısı, bir istemci kimliği ve istemci gizli anahtarı kullanarak bir uygulama ile bir güven ilişkisi oluşturur. Uygulamayı sağlayıcıda kaydedildiğinde istemci Kimliğini ve istemci gizli değerleri uygulama için Dış kimlik doğrulama sağlayıcısı tarafından oluşturulur. Uygulamanın kullandığı her bir dış sağlayıcı bağımsız olarak sağlayıcının istemci Kimliğini ve istemci gizli anahtarı ile yapılandırılması gerekir. Daha fazla bilgi için senaryonuz için geçerli olan dış kimlik doğrulama sağlayıcısı konulara bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Google kimlik doğrulaması](xref:security/authentication/google-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer kimlik doğrulama sağlayıcıları](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Örnek uygulama, bir istemci kimliği ve istemci gizli anahtarı Google tarafından sağlanan Google kimlik doğrulama sağlayıcısı yapılandırır:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Kimlik doğrulama kapsamı oluşturmak

Belirterek sağlayıcıdan almak için izinler listesinden belirtin <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>. Yaygın dış sağlayıcıları için kimlik doğrulaması kapsamları aşağıdaki tabloda görüntülenir.

| Sağlayıcı  | Kapsam                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Örnek uygulamanın, Google `userinfo.profile` kapsam framework tarafından otomatik olarak eklenir, <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> üzerinde çağrılır <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>. Uygulamayı ek kapsamlarla gerektiriyorsa, bunları seçenekleri ekleyin. Aşağıdaki örnekte, Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı, bir kullanıcının doğum gününü almak için eklenir:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Kullanıcı veri anahtarları eşleyin ve talep oluşturma

Sağlayıcının seçeneklerinde belirtin bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> her anahtar/alt oturum açma okumak uygulama kimliği için harici sağlayıcıya ait JSON kullanıcı verileri. Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.

Örnek uygulamayı yerel ayar oluşturur (`urn:google:locale`) ve resim (`urn:google:picture`) gelen talepleri `locale` ve `picture` Google kullanıcı verileri anahtarları:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

İçinde <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>e <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) ile uygulama oturum açmış <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>. Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601> depolayabilirsiniz bir `ApplicationUser` bulunan kullanıcı verileri için talep <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.

Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) yerel ayarı oluşturur (`urn:google:locale`) ve resim (`urn:google:picture`) imzalı için talepleri de `ApplicationUser`, için talep dahil olmak üzere <xref:System.Security.Claims.ClaimTypes.GivenName> :

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Varsayılan olarak, bir kullanıcının talebi kimlik doğrulama tanımlama bilgisinde depolanır. Kimlik doğrulama tanımlama çok büyükse, uygulamanın başarısız olmasına neden olabilir:

* Tarayıcı tanımlama bilgisi üstbilgisi çok uzun olduğunu algılar.
* Genel istek boyutu çok büyük.

Kullanıcı verileri, büyük bir miktarını, kullanıcı isteklerini işlemek için gerekli ise:

* Sayı ve istek işleme için yalnızca ne gerektiren uygulama için kullanıcı taleplerini boyutunu sınırlandırın.
* Özel bir kullanın <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> tanımlama bilgisi kimlik doğrulaması ara için <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> isteklerdeki kimliklerin depolanacağı için. Sunucudaki kimlik bilgileri yalnızca küçük bir oturum tanımlayıcısı anahtarı istemciye gönderme sırasında büyük miktarlarda korur.

## <a name="save-the-access-token"></a>Erişim belirtecini kaydetme

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> erişim ve yenileme belirteçleri de depolanıp depolanmayacağını tanımlar <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> başarılı yetkilendirme sonrasında. `SaveTokens` ayarlanır `false` son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için varsayılan olarak.

Örnek uygulamayı değerini ayarlar `SaveTokens` için `true` içinde <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

Zaman `OnPostConfirmationAsync` yürütür, depolama ve erişim belirteci ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) dış sağlayıcıdan `ApplicationUser`'s `AuthenticationProperties`.

Erişim belirtecinde örnek uygulamayı kaydeder `OnPostConfirmationAsync` (yeni kullanıcı kaydı) ve `OnGetCallbackAsync` (önceden kaydedilmiş kullanıcı) içinde *Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Ek özel belirteçler ekleme

Bir parçası olarak depolanan özel bir belirteç ekleme göstermek için `SaveTokens`, örnek uygulamayı ekler bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> geçerli <xref:System.DateTime> için bir [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) , `TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>Oluşturma ve talep ekleme

Framework, ortak Eylemler ve oluşturmak ve talep koleksiyona eklemek için genişletme yöntemleri sağlar. Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.

Kullanıcılar, türetilen özel eylemler tanımlayabilir <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> ve Özet uygulama <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> yöntemi.

Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Talep Eylemler ve taleplerini kaldırma

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) kaldırır tüm eylemler için talep verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> koleksiyondan. [ClaimActionCollectionMapExtensions.DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) bir talep siler verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> gelen kimlik. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> ile kullanılır [Openıd Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) protokolü tarafından oluşturulan talepleri kaldırmak için.

## <a name="sample-app-output"></a>Örnek uygulama çıktısı

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET/AspNetCore mühendislik SocialSample uygulama](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; bağlı örnek uygulamasını açıktır [aspnet/AspNetCore GitHub deponun](https://github.com/aspnet/AspNetCore) `master` mühendislik dal. `master` Dalı ASP.NET core'un sonraki active geliştirme aşamasındaki kodun içerir. ASP.NET Core yayımlanmış bir sürüm için örnek uygulama sürümünü görmek için **dal** açılan listesinden bir sürüm dalı seçin (örneğin `release/2.2`).
