---
title: ASP.NET Core dış sağlayıcılardan ek talepler ve belirteçler kalıcı hale getirme
author: rick-anderson
description: Dış sağlayıcılardan ek talepler ve belirteçler oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 9dfe5745125e34ed813d078529471a0ba2a53ab0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666833"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>ASP.NET Core dış sağlayıcılardan ek talepler ve belirteçler kalıcı hale getirme

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core bir uygulama, Facebook, Google, Microsoft ve Twitter gibi dış kimlik doğrulama sağlayıcılarından ek talepler ve belirteçler oluşturabilir. Her sağlayıcı, platformdaki kullanıcılar hakkında farklı bilgiler ortaya koyar, ancak kullanıcı verilerini alma ve ek taleplere dönüştürme için olan model aynı olur.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Uygulamada hangi dış kimlik doğrulama sağlayıcılarının destekileceğine karar verin. Her sağlayıcı için, uygulamayı kaydedin ve bir istemci KIMLIĞI ve istemci parolası alın. Daha fazla bilgi için bkz. <xref:security/authentication/social/index>. Örnek uygulama [Google kimlik doğrulama sağlayıcısını](xref:security/authentication/google-logins)kullanır.

## <a name="set-the-client-id-and-client-secret"></a>İstemci KIMLIĞINI ve gizli anahtarı ayarlama

OAuth kimlik doğrulama sağlayıcısı, istemci KIMLIĞI ve istemci parolası kullanarak bir uygulamayla güven ilişkisi kurar. Uygulama sağlayıcıya kaydedildiğinde dış kimlik doğrulama sağlayıcısı tarafından uygulama için istemci KIMLIĞI ve istemci gizli anahtarı değerleri oluşturulur. Uygulamanın kullandığı her bir dış sağlayıcı, sağlayıcının istemci KIMLIĞI ve istemci parolası ile bağımsız olarak yapılandırılmalıdır. Daha fazla bilgi için, senaryonuza uygulanan dış kimlik doğrulama sağlayıcısı konularına bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Google kimlik doğrulaması](xref:security/authentication/google-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer kimlik doğrulama sağlayıcıları](xref:security/authentication/otherlogins)
* [Openıdconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Örnek uygulama Google kimlik doğrulama sağlayıcısını Google tarafından sağlanmış istemci KIMLIĞI ve istemci gizli anahtarı ile yapılandırır:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Kimlik doğrulama kapsamını oluşturma

<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>belirterek sağlayıcıdan alma izinlerinin listesini belirtin. Ortak dış sağlayıcılar için kimlik doğrulama kapsamları aşağıdaki tabloda görüntülenir.

| Sağlayıcı  | `Scope`                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Örnek uygulamada, <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder><xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> çağrıldığında, Google 'ın `userinfo.profile` kapsamı Framework tarafından otomatik olarak eklenir. Uygulama ek kapsamlar gerektiriyorsa, bunları seçeneklere ekleyin. Aşağıdaki örnekte, bir kullanıcının Doğum gününü almak için Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı eklenmiştir:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Kullanıcı veri anahtarlarını eşleme ve talepler oluşturma

Sağlayıcının seçeneklerinde, oturum açma sırasında okunacak uygulama kimliği için dış sağlayıcının JSON Kullanıcı verilerinde her anahtar/alt anahtar için bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> belirtin. Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.

Örnek uygulama, Google user Data 'daki `locale` ve `picture` anahtarlarından yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) talepleri oluşturur:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

`Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, bir <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>ile uygulamada oturum açtı. Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601>, <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>Kullanıcı verileri için `ApplicationUser` taleplerini depolayabilirler.

Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*), <xref:System.Security.Claims.ClaimTypes.GivenName>için bir talep da dahil olmak üzere oturum açmış `ApplicationUser`için yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) taleplerini belirler:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Varsayılan olarak, bir kullanıcının talepleri kimlik doğrulama tanımlama bilgisinde depolanır. Kimlik doğrulama tanımlama bilgisi çok büyükse, bunun nedeni uygulamanın başarısız olmasına neden olabilir:

* Tarayıcı, tanımlama bilgisi üstbilgisinin çok uzun olduğunu algılar.
* İsteğin genel boyutu çok büyük.

Kullanıcı isteklerini işlemek için büyük miktarda Kullanıcı verisi gerekliyse:

* İstek işleme için Kullanıcı taleplerinin sayısını ve boyutunu yalnızca uygulamanın gerektirdiği şekilde sınırlayın.
* Kimlik bilgisi kimlik doğrulaması ara yazılımı için özel bir <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> kullanın, kimlik istekleri arasında depolama <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> Yalnızca istemciye küçük bir oturum tanımlayıcı anahtarı gönderilirken sunucuda büyük miktarlarda kimlik bilgilerini koruyun.

## <a name="save-the-access-token"></a>Erişim belirtecini Kaydet

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>, başarılı bir yetkilendirmeden sonra erişim ve yenileme belirteçlerinin <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> depolanması gerekip gerekmediğini tanımlar. Son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için `SaveTokens` varsayılan olarak `false` ayarlanır.

Örnek uygulama, `SaveTokens` değerini <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>`true` olarak ayarlar:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

`OnPostConfirmationAsync` yürütüldüğünde, erişim belirtecini ([Externalloginınfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) `ApplicationUser``AuthenticationProperties`dış sağlayıcıdan depolayın.

Örnek uygulama, erişim belirtecini *Hesap/ExternalLogin. cshtml. cs*içinde `OnPostConfirmationAsync` (Yeni Kullanıcı kaydı) ve `OnGetCallbackAsync` (önceden kaydedilmiş Kullanıcı) olarak kaydeder:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Ek özel belirteçler ekleme

`SaveTokens`bir parçası olarak depolanan özel bir belirtecin nasıl ekleneceğini göstermek için, örnek uygulama, `TicketCreated`[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) geçerli <xref:System.DateTime> ile bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> ekler:

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>Talepler oluşturma ve ekleme

Framework, koleksiyona talepler oluşturmak ve eklemek için ortak eylemler ve genişletme yöntemleri sağlar. Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>bakın.

Kullanıcılar, <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> türeterek ve soyut <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metodunu uygulayarak özel eylemleri tanımlayabilir.

Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Talep eylemlerinin ve taleplerin kaldırılması

[Claimactioncollection. Remove (dize)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) , verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> için tüm talep eylemlerini koleksiyondan kaldırır. [Claimactioncollectionmapextensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) , kimliğin verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> talebini siler. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>, öncelikle protokol tarafından oluşturulan talepleri kaldırmak için [OpenID Connect (OıDC)](/azure/active-directory/develop/v2-protocols-oidc) ile kullanılır.

## <a name="sample-app-output"></a>Örnek uygulama çıkışı

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core bir uygulama, Facebook, Google, Microsoft ve Twitter gibi dış kimlik doğrulama sağlayıcılarından ek talepler ve belirteçler oluşturabilir. Her sağlayıcı, platformdaki kullanıcılar hakkında farklı bilgiler ortaya koyar, ancak kullanıcı verilerini alma ve ek taleplere dönüştürme için olan model aynı olur.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Uygulamada hangi dış kimlik doğrulama sağlayıcılarının destekileceğine karar verin. Her sağlayıcı için, uygulamayı kaydedin ve bir istemci KIMLIĞI ve istemci parolası alın. Daha fazla bilgi için bkz. <xref:security/authentication/social/index>. Örnek uygulama [Google kimlik doğrulama sağlayıcısını](xref:security/authentication/google-logins)kullanır.

## <a name="set-the-client-id-and-client-secret"></a>İstemci KIMLIĞINI ve gizli anahtarı ayarlama

OAuth kimlik doğrulama sağlayıcısı, istemci KIMLIĞI ve istemci parolası kullanarak bir uygulamayla güven ilişkisi kurar. Uygulama sağlayıcıya kaydedildiğinde dış kimlik doğrulama sağlayıcısı tarafından uygulama için istemci KIMLIĞI ve istemci gizli anahtarı değerleri oluşturulur. Uygulamanın kullandığı her bir dış sağlayıcı, sağlayıcının istemci KIMLIĞI ve istemci parolası ile bağımsız olarak yapılandırılmalıdır. Daha fazla bilgi için, senaryonuza uygulanan dış kimlik doğrulama sağlayıcısı konularına bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Google kimlik doğrulaması](xref:security/authentication/google-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer kimlik doğrulama sağlayıcıları](xref:security/authentication/otherlogins)
* [Openıdconnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

Örnek uygulama Google kimlik doğrulama sağlayıcısını Google tarafından sağlanmış istemci KIMLIĞI ve istemci gizli anahtarı ile yapılandırır:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>Kimlik doğrulama kapsamını oluşturma

<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>belirterek sağlayıcıdan alma izinlerinin listesini belirtin. Ortak dış sağlayıcılar için kimlik doğrulama kapsamları aşağıdaki tabloda görüntülenir.

| Sağlayıcı  | `Scope`                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

Örnek uygulamada, <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder><xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> çağrıldığında, Google 'ın `userinfo.profile` kapsamı Framework tarafından otomatik olarak eklenir. Uygulama ek kapsamlar gerektiriyorsa, bunları seçeneklere ekleyin. Aşağıdaki örnekte, bir kullanıcının Doğum gününü almak için Google `https://www.googleapis.com/auth/user.birthday.read` kapsamı eklenmiştir:

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>Kullanıcı veri anahtarlarını eşleme ve talepler oluşturma

Sağlayıcının seçeneklerinde, oturum açma sırasında okunacak uygulama kimliği için dış sağlayıcının JSON Kullanıcı verilerinde her anahtar/alt anahtar için bir <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> veya <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> belirtin. Talep türleri hakkında daha fazla bilgi için bkz. <xref:System.Security.Claims.ClaimTypes>.

Örnek uygulama, Google user Data 'daki `locale` ve `picture` anahtarlarından yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) talepleri oluşturur:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

`Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, bir <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>ile uygulamada oturum açtı. Oturum açma işlemi sırasında <xref:Microsoft.AspNetCore.Identity.UserManager%601>, <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>Kullanıcı verileri için `ApplicationUser` taleplerini depolayabilirler.

Örnek uygulamada `OnPostConfirmationAsync` (*Account/ExternalLogin. cshtml. cs*), <xref:System.Security.Claims.ClaimTypes.GivenName>için bir talep da dahil olmak üzere oturum açmış `ApplicationUser`için yerel ayar (`urn:google:locale`) ve resim (`urn:google:picture`) taleplerini belirler:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

Varsayılan olarak, bir kullanıcının talepleri kimlik doğrulama tanımlama bilgisinde depolanır. Kimlik doğrulama tanımlama bilgisi çok büyükse, bunun nedeni uygulamanın başarısız olmasına neden olabilir:

* Tarayıcı, tanımlama bilgisi üstbilgisinin çok uzun olduğunu algılar.
* İsteğin genel boyutu çok büyük.

Kullanıcı isteklerini işlemek için büyük miktarda Kullanıcı verisi gerekliyse:

* İstek işleme için Kullanıcı taleplerinin sayısını ve boyutunu yalnızca uygulamanın gerektirdiği şekilde sınırlayın.
* Kimlik bilgisi kimlik doğrulaması ara yazılımı için özel bir <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> kullanın, kimlik istekleri arasında depolama <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> Yalnızca istemciye küçük bir oturum tanımlayıcı anahtarı gönderilirken sunucuda büyük miktarlarda kimlik bilgilerini koruyun.

## <a name="save-the-access-token"></a>Erişim belirtecini Kaydet

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>, başarılı bir yetkilendirmeden sonra erişim ve yenileme belirteçlerinin <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> depolanması gerekip gerekmediğini tanımlar. Son kimlik doğrulama tanımlama bilgisinin boyutunu azaltmak için `SaveTokens` varsayılan olarak `false` ayarlanır.

Örnek uygulama, `SaveTokens` değerini <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>`true` olarak ayarlar:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

`OnPostConfirmationAsync` yürütüldüğünde, erişim belirtecini ([Externalloginınfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) `ApplicationUser``AuthenticationProperties`dış sağlayıcıdan depolayın.

Örnek uygulama, erişim belirtecini *Hesap/ExternalLogin. cshtml. cs*içinde `OnPostConfirmationAsync` (Yeni Kullanıcı kaydı) ve `OnGetCallbackAsync` (önceden kaydedilmiş Kullanıcı) olarak kaydeder:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>Ek özel belirteçler ekleme

`SaveTokens`bir parçası olarak depolanan özel bir belirtecin nasıl ekleneceğini göstermek için, örnek uygulama, `TicketCreated`[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) geçerli <xref:System.DateTime> ile bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> ekler:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>Talepler oluşturma ve ekleme

Framework, koleksiyona talepler oluşturmak ve eklemek için ortak eylemler ve genişletme yöntemleri sağlar. Daha fazla bilgi için <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> ve <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>bakın.

Kullanıcılar, <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> türeterek ve soyut <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> metodunu uygulayarak özel eylemleri tanımlayabilir.

Daha fazla bilgi için bkz. <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.

## <a name="removal-of-claim-actions-and-claims"></a>Talep eylemlerinin ve taleplerin kaldırılması

[Claimactioncollection. Remove (dize)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) , verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> için tüm talep eylemlerini koleksiyondan kaldırır. [Claimactioncollectionmapextensions. DeleteClaim (ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) , kimliğin verilen <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> talebini siler. <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>, öncelikle protokol tarafından oluşturulan talepleri kaldırmak için [OpenID Connect (OıDC)](/azure/active-directory/develop/v2-protocols-oidc) ile kullanılır.

## <a name="sample-app-output"></a>Örnek uygulama çıkışı

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

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [DotNet/aspnetcore mühendislik SocialSample uygulaması](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; bağlantılı örnek uygulama [DotNet/aspnetcore GitHub deposunun](https://github.com/dotnet/AspNetCore) `master` mühendislik dalında bulunur. `master` dalı, ASP.NET Core sonraki sürümü için etkin geliştirme kapsamındaki kodu içerir. Yayınlanmış bir ASP.NET Core sürümü için örnek uygulamanın bir sürümünü görmek için, **dal** açılan listesini kullanarak bir yayın dalı seçin (örneğin `release/{X.Y}`).
