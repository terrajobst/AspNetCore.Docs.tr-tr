---
title: "ASP.NET Core kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullanma"
author: rick-anderson
description: "ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullanarak bir açıklama"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 781b89a222bbf26b51e56fad707c591779f2e4bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullanma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

Önceki kimlik doğrulaması konularında gördüğünüz gibi [ASP.NET Core kimliği](xref:security/authentication/identity) oluşturmak ve oturum açma bilgileri korumak için bir tam, tam özellikli kimlik doğrulaması sağlayıcıdır. Ancak, tanımlama bilgisi tabanlı kimlik doğrulaması ile zaman zaman kendi özel kimlik doğrulama mantığı kullanmak isteyebilirsiniz. Tanımlama bilgisi tabanlı kimlik doğrulaması, ASP.NET Core kimliği olmadan tek başına kimlik doğrulama sağlayıcısı olarak kullanabilirsiniz.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

ASP.NET Core geçirme tanımlama bilgisi tabanlı kimlik hakkında bilgi için 1.x 2.0 için bkz: [geçirme kimlik doğrulama ve ASP.NET Core 2.0 konuya (tanımlama bilgisi tabanlı kimlik doğrulaması) kimlik](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Yapılandırma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kullanmıyorsanız [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), 2.0 + sürümü yüklemek [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketi.

İçinde `ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmet oluşturma `AddAuthentication` ve `AddCookie` yöntemleri:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme`geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar. `AuthenticationScheme`tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz kullanışlıdır [belirli düzeniyle yetkilendirmek](xref:security/authorization/limitingidentitybyscheme). Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar. Düzen ayırt herhangi bir dize değeri sağlayabilir.

İçinde `Configure` yöntemi, kullanım `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği. Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

**AddCookie Options**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

| Seçenek | Açıklama |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yol sağlayan tarafından tetiklendiğinde `HttpContext.ForbidAsync`. Varsayılan değer `/Account/AccessDenied` şeklindedir. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulama hizmeti tarafından oluşturulan herhangi bir talep özelliği. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Tanımlama bilgisinin Burada sunulan etki alanı adı. Varsayılan olarak, istek ana bilgisayar adını budur. Tarayıcı eşleşen bir ana bilgisayar adı tanımlama bilgisinin yalnızca istekleri gönderir. Etki alanınızda tanımlama bilgilerini herhangi bir ana bilgisayara kullanılabilir olması için bunu ayarlamak isteyebilirsiniz. Örneğin, tanımlama bilgisi etki alanını ayarlamak `.contoso.com` için kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Alır veya bir tanımlama bilgisi Sysprep'in ayarlar. Şu anda Hayır ops seçeneği ve ASP.NET çekirdek 2.1 + geçersiz hale gelir. Kullanım `ExpireTimeSpan` tanımlama bilgisi bitiş tarihini ayarlamak için seçeneği. Daha fazla bilgi için bkz: [CookieAuthenticationOptions.Cookie.Expiration davranışını açıklamak](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak. Bu değer ile değiştirmek `false` tanımlama bilgisinin erişmek için istemci tarafı betikler seçmenize izin verir ve tanımlama bilgisi hırsızlığı uygulamanıza olmalıdır, uygulamanızın açılabilir bir [siteler arası komut dosyası (XSS)](xref:security/cross-site-scripting) güvenlik açığı. Varsayılan değer `true` şeklindedir. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Tanımlama bilgisinin adını ayarlar. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Aynı ana bilgisayar adını çalışan uygulamalar yalıtmak için kullanılır. Konumunda çalışan bir uygulamanız varsa `/app1` ve bu uygulama için tanımlama bilgilerini kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğine `/app1`. Bunu yaparak, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Tarayıcı yalnızca aynı sitede isteklerine eklenecek tanımlama bilgisinin izin verip vermeyeceğini belirtir (`SameSiteMode.Strict`) veya Güvenli HTTP yöntemleri ve aynı sitede isteklerini kullanarak siteler arası istek (`SameSiteMode.Lax`). Ayarlandığında `SameSiteMode.None`, tanımlama bilgisi üstbilgisi değeri ayarlanmamış. Unutmayın [tanımlama bilgisi ilke Ara](#cookie-policy-middleware) sağladığınız değerin üzerine yazılmasına neden olabilir. OAuth kimlik doğrulamasını desteklemek için varsayılan değer: `SameSiteMode.Lax`. Daha fazla bilgi için bkz: [SameSite tanımlama bilgisi ilkesi nedeniyle bozuk OAuth kimlik doğrulaması](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), ya da istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`). Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Ayarlar `DataProtectionProvider` varsayılan oluşturmak için kullanılan `TicketDataFormat`. Varsa `TicketDataFormat` özelliği ayarlanmış `DataProtectionProvider` değil seçenek kullanılır. Sağlanmazsa, uygulamanın varsayılan veri koruma sağlayıcısı kullanılır. |
| [Olaylar](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | İşleyicinin belirli işleme noktalarda uygulama denetime sağlayıcısı yöntemleri çağırır. Varsa `Events` varsayılan örnek yöntem çağrıldığında, hiçbir şey yapmaz sağlanır değil. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Hizmet türü olarak almak için kullanılan `Events` özelliği yerine örneği. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` Hangi tanımlama bilgisi içinde depolanan kimlik doğrulaması biletinin süresinin sonra. `ExpireTimeSpan`anahtar için kullanım süresi sonu oluşturmak için geçerli süre eklenir. `ExpiredTimeSpan` Değeri her zaman gider sunucu tarafından doğrulanan şifrelenmiş AuthTicket içine. Ayrıca içine gidebilir [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) üstbilgisi, ancak yalnızca `IsPersistent` ayarlanır. Ayarlamak için `IsPersistent` için `true`, yapılandırma [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) geçirilen `SignInAsync`. Varsayılan değer olan `ExpireTimeSpan` 14 gündür. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yol sağlayan tarafından tetiklendiğinde `HttpContext.ChallengeAsync`. 401'i oluşturan geçerli URL eklenen `LoginPath` tarafından adlı bir sorgu dizesi parametresi olarak `ReturnUrlParameter`. Bir istek için bir kez `LoginPath` yeni bir oturum açma kimliği, veren `ReturnUrlParameter` değeri, tarayıcının özgün yetkilendirilmemiş durum koduna neden olan URL'ye yeniden yönlendirmek için kullanılır. Varsayılan değer `/Account/Login` şeklindedir. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Varsa `LogoutPath` bu yol için bir istek yönlendiren sonra işleyicisine sağlanan değerine göre `ReturnUrlParameter`. Varsayılan değer `/Account/Logout` şeklindedir. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 302 bulundu (URL yeniden yönlendirme) yanıt işleyici tarafından eklenen sorgu dizesi parametresinin adını belirler. `ReturnUrlParameter`bir istek ulaştığında kullanılan `LoginPath` veya `LogoutPath` tarayıcı oturum açma veya oturum kapatma eylem gerçekleştirildikten sonra özgün URL'ye geri dönmek için. Varsayılan değer `ReturnUrl` şeklindedir. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | İsteklerinde kimlik depolamak için kullanılan isteğe bağlı bir kapsayıcı. Kullanıldığında istemciye yalnızca bir oturum tanımlayıcısı gönderilir. `SessionStore`büyük kimlikleri ile ilgili olası sorunları hafifletmek için kullanılabilir. |
| [SlidingExpiration değeri](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Yeni bir tanımlama bilgisi güncelleştirilmiş süre sonu zamanı ile dinamik olarak verilmesi olmadığını belirten bir bayrak. Bu, geçerli tanımlama bilgisi sona erme süresi % 50'den fazla burada sona herhangi bir istek üzerinde meydana gelebilir. Yeni sona erme tarihi geçerli tarih olarak İleri taşınır artı `ExpireTimespan`. Bir [mutlak tanımlama bilgisinin süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıf çağrılırken `SignInAsync`. Mutlak zaman aşımı süresi, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak, uygulamanızın güvenliğini artırabilirsiniz. Varsayılan değer `true` şeklindedir. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Korumak ve kimliği ve tanımlama bilgisi değerinde depolanan diğer özellikleri korumasını kaldırmak için kullanılır. Sağlanmazsa, bir `TicketDataFormat` kullanılarak oluşturulan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Doğrula](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Seçenekleri geçerli olduğunu denetler yöntemi. |

Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `ConfigureServices` yöntemi:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.x kullanır tanımlama bilgisi [ara yazılım](xref:fundamentals/middleware) şifrelenmiş bir tanımlama bilgisi, kullanıcı asıl serileştirir. Sonraki isteklerde tanımlama bilgisinin doğrulanır ve asıl yeniden ve atanan `HttpContext.User` özelliği.

Yükleme [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketini projenize. Bu paket tanımlama bilgisi Ara içerir.

Kullanım `UseCookieAuthentication` yönteminde `Configure` yönteminde, *haline* önce dosya `UseMvc` veya `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**CookieAuthenticationOptions Options**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

| Seçenek | Açıklama |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Kimlik doğrulama şeması ayarlar. `AuthenticationScheme`kimlik doğrulamasının birden çok örneği vardır ve istediğiniz kullanışlıdır [belirli düzeniyle yetkilendirmek](xref:security/authorization/limitingidentitybyscheme). Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar. Düzen ayırt herhangi bir dize değeri sağlayabilir. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Tanımlama bilgisi kimlik doğrulamasını ve her istekte doğrulamak ve herhangi bir seri hale getirilmiş oluşturulduğu asıl yeniden denemesi gerektiğini belirtmek için bir değer ayarlar. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | TRUE ise, kimlik doğrulaması ara yazılımı otomatik zorluklar işler. Yanlış kimlik doğrulaması ara yazılımı yalnızca açıkça belirtildiği zaman yanıtları değiştirir, `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından oluşturulan herhangi bir talep özelliği. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Tanımlama bilgisinin Burada sunulan etki alanı adı. Varsayılan olarak, istek ana bilgisayar adını budur. Tarayıcı tanımlama bilgisi eşleşen bir ana bilgisayar adı yalnızca işlevi görür. Etki alanınızda tanımlama bilgilerini herhangi bir ana bilgisayara kullanılabilir olması için bunu ayarlamak isteyebilirsiniz. Örneğin, tanımlama bilgisi etki alanını ayarlamak `.contoso.com` için kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak. Bu değer ile değiştirmek `false` tanımlama bilgisinin erişmek için istemci tarafı betikler seçmenize izin verir ve tanımlama bilgisi hırsızlığı uygulamanıza olmalıdır, uygulamanızın açılabilir bir [siteler arası komut dosyası (XSS)](xref:security/cross-site-scripting) güvenlik açığı. Varsayılan değer `true` şeklindedir. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Aynı ana bilgisayar adını çalışan uygulamalar yalıtmak için kullanılır. Konumunda çalışan bir uygulamanız varsa `/app1` ve bu uygulama için tanımlama bilgilerini kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğine `/app1`. Bunu yaparak, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), ya da istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`). Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir. |
| [Açıklama](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Uygulama için kullanılabilir hale getirileceğini kimlik doğrulama türü hakkında ek bilgi. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` Hangi kimlik doğrulama anahtarının süresi dolduktan sonra. Anahtar için kullanım süresi sonu oluşturmak için geçerli süre eklenir. Kullanılacak `ExpireTimeSpan`, ayarlamalısınız `IsPersistent` için `true` içinde `AuthenticationProperties` geçirilen `SignInAsync`. Varsayılan değer 14 gündür. |
| [SlidingExpiration değeri](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Tanımlama bilgisi sona erme tarihi ne zaman birden fazla yarısı sıfırlar olup olmadığını belirten bir bayrak `ExpireTimeSpan` aralığı geçtikten. Yeni exipiration saati geçerli tarih olması İleri taşınır artı `ExpireTimespan`. Bir [mutlak tanımlama bilgisinin süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıf çağrılırken `SignInAsync`. Mutlak zaman aşımı süresi, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak, uygulamanızın güvenliğini artırabilirsiniz. Varsayılan değer `true` şeklindedir. |

Ayarlama `CookieAuthenticationOptions` tanımlama bilgisi kimlik doğrulaması ara için `Configure` yöntemi:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Tanımlama bilgisi ilke Ara

[Tanımlama bilgisi ilke Ara](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) bir uygulamada tanımlama bilgisi ilkesi özellikleri sağlar. Ara yazılım uygulama işleme ardışık düzenine ekleme hassas sıradır; yalnızca ardışık düzeninde sonra kayıtlı bileşenleri de etkiler.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) tanımlama bilgisi ilke Ara sağlanan, tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme ve kanca genel özelliklerini tanımlama bilgisi işleme işleyicileri denetlemenize olanak sağlar.

| Özellik | Açıklama |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Tanımlama bilgilerini HttpOnly olmalıdır olup, bir bayrak tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını gösteren etkiler. Varsayılan değer `HttpOnlyPolicy.None` şeklindedir. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Tanımlama bilgisinin site aynı öznitelik (aşağıya bakın) etkiler. Varsayılan değer `SameSiteMode.Lax` şeklindedir. Bu seçenek için ASP.NET Core 2.0 + kullanılabilir. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Bir tanımlama bilgisi eklenir çağrılır. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Bir tanımlama bilgisi silindiğinde çağrılır. |
| [Güvenli](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Tanımlama bilgilerini güvenli mi etkiler. Varsayılan değer `CookieSecurePolicy.None` şeklindedir. |

**MinimumSameSitePolicy** (ASP.NET çekirdek 2.0 + yalnızca)

Varsayılan `MinimumSameSitePolicy` değer `SameSiteMode.Lax` OAuth2 kimlik doğrulama izin vermek için. Bir site aynı ilke kesinlikle zorlamak için `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`. Bu ayar OAuth2 ve diğer çıkış noktaları arası kimlik doğrulama şemasını keser rağmen çıkış noktaları arası istek işleme güvenmeyin uygulamalarının diğer türleri tanımlama bilgisinin güvenlik düzeyini yükseltir.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Tanımlama bilgisi ilke Ara ayarını `MinimumSameSitePolicy` , ayarı etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` matris göre ayarlar.

| MinimumSameSitePolicy | Cookie.SameSite | Sonuç Cookie.SameSite ayarı |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Bir kimlik doğrulama tanımlama bilgisi oluşturma

Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturmalıdır bir [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Kullanıcı bilgilerini serileştirilmiş ve tanımlama bilgisinde depolanır. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) kullanıcıyla oturum açmak için:

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) kullanıcıyla oturum açmak için:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync`şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler. Belirtmediyseniz bir `AuthenticationScheme`, varsayılan düzeni kullanılır.

Perde arkasında kullanılan ASP.NET Core'nın şifrelemesidir [veri koruması](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistem. Birden fazla makine, uygulamalar arasında Yük Dengeleme veya bir web çiftliği kullanarak uygulama barındırma sonra yapmanız gerekenler [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtar halkası ve uygulama tanımlayıcısı kullanmak üzere.

## <a name="signing-out"></a>Oturumu kapatma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Out geçerli kullanıcı oturum açabilir ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Out geçerli kullanıcı oturum açabilir ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Kullanmıyorsanız `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") kimlik doğrulama sağlayıcısı yapılandırırken kullanılan şema şema (örneğin, "ContosoCookie") olarak sağlayın. Aksi takdirde, varsayılan düzeni kullanılır.

## <a name="reacting-to-back-end-changes"></a>Arka uç değişiklikler tepki

Bir tanımlama bilgisi oluşturulduktan sonra tek kaynak kimliğinin haline gelir. Arka uç sistemlerinizde bir kullanıcıyı devre dışı olsa bile bu olanağıyla tanımlama bilgisi kimlik doğrulaması sistemde var ve bunların tanımlama bilgisinin geçerli olduğu sürece bir kullanıcı olarak oturum açmış kalır.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) ASP.NET Core olayda 2.x veya [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) yönteminde ASP.NET Core 1.x kesecek ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir. Bu yaklaşım, uygulama erişimini iptal edilen kullanıcıların riskini azaltır.

Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanı değiştiğinde izleyen üzerinde temel alır. Kullanıcının tanımlama bilgisi verilmiş olduğundan veritabanı değişip değişmediğini, kendi tanımlama bilgisi hala geçerli ise, kullanıcının yeniden kimlik doğrulaması için gerek yoktur. Bu senaryo, uygulanan veritabanı uygulamak için `IUserRepository` Bu örnekte, depolayan bir `LastChanged` değeri. Herhangi bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.

Veritabanı değişikliklerini temel alan bir tanımlama bilgisi geçersiz kılmak için `LastChanged` değeri, tanımlama bilgisi ile oluşturma bir `LastChanged` geçerli içeren talep `LastChanged` veritabanından değeri:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

İçin bir geçersiz kılma uygulamak için `ValidatePrincipal` olay, aşağıdaki imzası öğesinden türetilen bir sınıfta yöntemiyle bir yazma [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Bir örnek aşağıdaki gibi görünür:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Tanımlama bilgisi hizmet kaydı sırasında olayları örneği kaydedin `ConfigureServices` yöntemi. Kapsamlı hizmeti kaydı için sağlayın, `CustomCookieAuthenticationEvents` sınıfı:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

İçin bir geçersiz kılma uygulamak için `ValidateAsync` olay, aşağıdaki imzası yöntemiyle bir yazma:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core kimliği parçası olarak bu onay uygulayan kendi [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Bir örnek aşağıdaki gibi görünür:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Tanımlama bilgisi kimlik doğrulama yapılandırması sırasında olay kaydetmek `Configure` yöntemi:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Kullanıcının adını güncelleştirilir bir durum düşünün &mdash; güvenlik herhangi bir şekilde etkilemez bir karardır. Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ve `context.ShouldRenew` özelliğine `true`.

> [!WARNING]
> Burada açıklanan yaklaşımı, her istek tetiklenir. Bu uygulama için büyük performans cezası neden olabilir.

## <a name="persistent-cookies"></a>Kalıcı tanımlama bilgileri

Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz. Bu Kalıcılık açık kullanıcı izni olan bir "Beni anımsa" oturum açma veya benzer bir mekanizma ile yalnızca etkinleştirilmiş olmalıdır. 

Aşağıdaki kod parçacığını bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır karşılık gelen tanımlama bilgisi oluşturur. Önceden yapılandırılmış kayan sona erme ayarları dikkate alınır. Tarayıcı kapalıyken tanımlama bilgisinin süresi dolarsa, hizmet yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) sınıfı bulunur `Microsoft.AspNetCore.Authentication` ad alanı.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) sınıfı bulunur `Microsoft.AspNetCore.Http.Authentication` ad alanı.

---

## <a name="absolute-cookie-expiration"></a>Mutlak tanımlama bilgisinin süre sonu

İle bir mutlak zaman aşımı süresini ayarlayabilirsiniz `ExpiresUtc`. De ayarlamalısınız `IsPersistent`; Aksi takdirde `ExpiresUtc` göz ardı edilir ve tek oturum tanımlama bilgisi oluşturulur. Zaman `ExpiresUtc` ayarlanan `SignInAsync`, değerini geçersiz kılar `ExpireTimeSpan` seçeneği `CookieAuthenticationOptions`, ayarlayın.

Aşağıdaki kod parçacığını bir kimlik ve 20 dakika sürer karşılık gelen tanımlama bilgisi oluşturur. Bu, önceden yapılandırılmış kayan sona erme ayarları yok sayar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>Ayrıca bkz.

* [Auth 2.0 değişiklikleri / geçiş Duyurusu](https://github.com/aspnet/Announcements/issues/262)
* [Şemayla kimliği sınırlama](xref:security/authorization/limitingidentitybyscheme)
