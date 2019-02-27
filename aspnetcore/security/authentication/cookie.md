---
title: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan
author: rick-anderson
description: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullanarak bir açıklama
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: 29370a3ff25469b34edc2a71e00601cf6ecc00ca
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899298"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

Önceki kimlik doğrulaması konularındaki gördüğünüz gibi [ASP.NET Core kimliği](xref:security/authentication/identity) oluşturma ve oturum açma bilgileri korumak için bir tam, tam özellikli kimlik doğrulamasıdır. Ancak, tanımlama bilgisi tabanlı kimlik doğrulaması ile zaman zaman kendi özel kimlik doğrulama mantığı kullanmak isteyebilirsiniz. Tanımlama bilgisi tabanlı kimlik doğrulaması, ASP.NET Core kimliği olmadan tek başına kimlik doğrulama sağlayıcısı olarak kullanabilirsiniz.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulamada tanıtım amacıyla kullanıcı kuramsal Maria Rodriguez, kullanıcının uygulamada oturum kodlanmış hesabıdır. E-posta kullanıcı adını kullanın "maria.rodriguez@contoso.com" ve kullanıcı oturum açmak için herhangi bir parola. Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya. Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.

Geçirme tanımlama bilgisi tabanlı kimlik doğrulamasını ASP.NET Core hakkında bilgi için bkz 1.x sürümünden 2.0 sürümüne, [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 konu (tanımlama bilgisi tabanlı kimlik doğrulaması)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

ASP.NET Core kimliği kullanmak için bkz: [kimliğe giriş](xref:security/authentication/identity) konu.

## <a name="configuration"></a>Yapılandırma

::: moniker range=">= aspnetcore-2.0"

Uygulama kullanmıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), proje dosyası için bir paket başvuruya [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paketi (sürüm 2.1.0 veya Daha sonra).

İçinde `ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmeti oluşturma `AddAuthentication` ve `AddCookie` yöntemleri:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar. `AuthenticationScheme` tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme). Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar. Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz.

Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulaması düzeninden farklıdır. Ne zaman bir tanımlama bilgisi kimlik doğrulama düzeni değildir sağlanan <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, kullandığı [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("tanımlama bilgileri").

Kimlik doğrulama tanımlama bilgisinin <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> özelliği `true` varsayılan olarak. Kimlik doğrulaması tanımlama bilgileri, bir ziyaretçi için veri toplama tarafından onaylanan taşınmadığından olduğunda izin verilir. Daha fazla bilgi için bkz. <xref:security/gdpr#essential-cookies>.

İçinde `Configure` yöntemi, kullanım `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği. Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie seçenekleri**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

| Seçenek | Açıklama |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Bir 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yolunu tarafından tetiklendiğinde `HttpContext.ForbidAsync`. Varsayılan değer `/Account/AccessDenied` şeklindedir. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulama hizmeti tarafından oluşturulan herhangi bir talep özelliği. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Tanımlama bilgisi Burada sunulan etki alanı adı. Varsayılan olarak, isteğin ana bilgisayar adı budur. Tarayıcı yalnızca tanımlama bilgisi istekleri için eşleşen bir konak adı gönderir. Bu etki alanınızda tanımlama bilgilerini herhangi bir konağa kullanılabilir olmasını isteyebilirsiniz. Örneğin, tanımlama bilgisinin etki alanı ayarlama `.contoso.com` kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak. Bu değer ile değiştirmek `false` tanımlama bilgisi erişmek için istemci tarafı betikleri verir ve uygulamanızı tanımlama bilgisi hırsızlığı için uygulamanızı olmalıdır açın bir [siteler arası betik (XSS)](xref:security/cross-site-scripting) güvenlik açığı. Varsayılan değer `true` şeklindedir. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Tanımlama bilgisinin adını ayarlar. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Aynı ana bilgisayar adını çalışan uygulamaları yalıtmak için kullanılır. Çalışan bir uygulama varsa `/app1` ve bu uygulama için tanımlama bilgileri kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğini `/app1`. Bunu yaptığınızda, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Tarayıcı yalnızca site aynı isteklerine eklenecek tanımlama bilgisinin izin verip vermeyeceğini belirtir (`SameSiteMode.Strict`) veya Güvenli HTTP yöntemleri ve aynı sitede isteklerini kullanarak siteler arası istekleri (`SameSiteMode.Lax`). Ayarlandığında `SameSiteMode.None`, tanımlama bilgisi üstbilgisi değeri ayarlanmamış. Unutmayın [tanımlama bilgisi ilkesi Ara](#cookie-policy-middleware) sağladığınız değerin üzerine yazılmasına neden olabilir. OAuth kimlik doğrulamasını desteklemek için varsayılan değer: `SameSiteMode.Lax`. Daha fazla bilgi için [SameSite tanımlama bilgisi ilkesi nedeniyle bozuk OAuth kimlik doğrulaması](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), veya istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`). Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Kümeleri `DataProtectionProvider` varsayılan oluşturmak için kullanılan `TicketDataFormat`. Varsa `TicketDataFormat` özelliği ayarlanmış `DataProtectionProvider` seçeneği kullanılmaz. Sağlanmazsa, uygulamanın varsayılan veri koruma sağlayıcısı kullanılır. |
| [Olaylar](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | İşleyici, bazı işleme noktalarda uygulama denetimi sunan sağlayıcıda yöntemler çağırır. Varsa `Events` varsayılan örnek yöntemler çağrıldığında, hiçbir şey yapmaz olarak sağlanır, değildir. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Alınacak hizmet türü kullanılan `Events` örnek özelliği yerine. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan` , Tanımlama bilgisi içinde depolanan kimlik doğrulaması bileti süresi dolduktan sonra. `ExpireTimeSpan` anahtar sona erme süresini oluşturmak için geçerli zaman eklenir. `ExpiredTimeSpan` Değeri her zaman gider ve sunucu tarafından doğrulanan şifrelenmiş AuthTicket. Ayrıca içine gidebilir [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) üstbilgisi, ancak yalnızca `IsPersistent` ayarlanır. Ayarlanacak `IsPersistent` için `true`, yapılandırma [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) geçirilen `SignInAsync`. Varsayılan değer olan `ExpireTimeSpan` 14 gündür. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Bir 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yolunu tarafından tetiklendiğinde `HttpContext.ChallengeAsync`. 401'i oluşturan geçerli URL eklenir `LoginPath` tarafından adlandırılan bir sorgu dizesi parametresi olarak `ReturnUrlParameter`. Bir istek için bir kez `LoginPath` yeni bir oturum kimliği, veren `ReturnUrlParameter` değeri, tarayıcının özgün yetkilendirilmemiş durum koduna neden olan URL'ye yeniden yönlendirmek için kullanılır. Varsayılan değer `/Account/Login` şeklindedir. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Varsa `LogoutPath` yol bir isteği yeniden yönlendiren işleyicisine belirtilmemesi değerine göre `ReturnUrlParameter`. Varsayılan değer `/Account/Logout` şeklindedir. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Bir 302 bulundu (URL yeniden yönlendirme) yanıt işleyicisi tarafından eklenen sorgu dizesi parametresinin adını belirler. `ReturnUrlParameter` bir istek ulaştığında kullanılan `LoginPath` veya `LogoutPath` oturum açma veya kapatma eylemi gerçekleştirdikten sonra tarayıcının özgün URL'ye geri dönün. Varsayılan değer `ReturnUrl` şeklindedir. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | İsteklerdeki kimliklerin depolanacağı için kullanılan isteğe bağlı bir kapsayıcı. Kullanıldığında istemciye yalnızca bir oturum tanımlayıcısı gönderilir. `SessionStore` büyük kimliklerle olası sorunları gidermek için kullanılabilir. |
| [ilerlemiş](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Yeni bir tanımlama bilgisi güncelleştirilmiş sona erme süresini ile dinamik olarak verilmesi, belirten bir bayrak. Bu işlem, % 50'den burada geçerli tanımlama bilgisi süre sonu dönemi süresi doldu herhangi bir istek üzerinde oluşabilir. Yeni sona erme tarihi geçerli tarih olmasını İleri taşınır artı `ExpireTimespan`. Bir [mutlak tanımlama bilgisi süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıfı çağrılırken `SignInAsync`. Mutlak sona erme süresini, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak uygulamanızın güvenliğini artırabilirsiniz. Varsayılan değer `true` şeklindedir. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat` Korumak ve kimliği ve tanımlama bilgisi değerinde depolanan diğer özellikleri korumasını kaldırmak için kullanılır. Sağlanmazsa, bir `TicketDataFormat` kullanılarak oluşturulan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Doğrulama](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Bu yöntem seçenekleri geçerli olduğunu denetler. |

Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `ConfigureServices` yöntemi:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x kullandığı tanımlama bilgisi [ara yazılım](xref:fundamentals/middleware/index) şifrelenmiş bir tanımlama bilgisi, kullanıcı asıl serileştirir. Sonraki isteklerde authenticateasync tanımlama bilgisi doğrulanır ve asıl yeniden ve atanan `HttpContext.User` özelliği.

Yükleme [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketini projenize. Bu paket tanımlama bilgisi Ara içerir.

Kullanım `UseCookieAuthentication` yönteminde `Configure` yönteminde, *Startup.cs* önce dosya `UseMvc` veya `UseMvcWithDefaultRoute`:

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

**CookieAuthenticationOptions seçenekleri**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

| Seçenek | Açıklama |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Kimlik doğrulama şeması ayarlar. `AuthenticationScheme` kimlik doğrulamasının birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme). Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar. Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Tanımlama bilgisi kimlik doğrulamasını ve her istekte doğrulamak ve herhangi bir seri hale getirilmiş oluşturulduğu asıl yeniden denemesi gerektiğini belirtmek için bir değer ayarlar. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | TRUE ise, kimlik doğrulaması ara yazılımı otomatik zorlukları işler. False, kimlik doğrulaması ara yazılımı yalnızca açıkça belirttiği zaman yanıtlarını değiştirir, `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından oluşturulan herhangi bir talep özelliği. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Tanımlama bilgisi Burada sunulan etki alanı adı. Varsayılan olarak, isteğin ana bilgisayar adı budur. Tarayıcı tanımlama bilgisi eşleşen bir konak adı için yalnızca işlevi görür. Bu etki alanınızda tanımlama bilgilerini herhangi bir konağa kullanılabilir olmasını isteyebilirsiniz. Örneğin, tanımlama bilgisinin etki alanı ayarlama `.contoso.com` kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak. Bu değer ile değiştirmek `false` tanımlama bilgisi erişmek için istemci tarafı betikleri verir ve uygulamanızı tanımlama bilgisi hırsızlığı için uygulamanızı olmalıdır açın bir [siteler arası betik (XSS)](xref:security/cross-site-scripting) güvenlik açığı. Varsayılan değer `true` şeklindedir. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Aynı ana bilgisayar adını çalışan uygulamaları yalıtmak için kullanılır. Çalışan bir uygulama varsa `/app1` ve bu uygulama için tanımlama bilgileri kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğini `/app1`. Bunu yaptığınızda, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), veya istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`). Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir. |
| [Açıklama](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Uygulama için kullanılabilir hale getirileceğini kimlik doğrulama türü hakkında ek bilgi. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan` Hangi kimlik doğrulama anahtarının süresi dolduktan sonra. Anahtar sona erme süresini oluşturmak için geçerli zaman eklenir. Kullanılacak `ExpireTimeSpan`, ayarlamalısınız `IsPersistent` için `true` içinde `AuthenticationProperties` geçirilen `SignInAsync`. Varsayılan değer 14 gündür. |
| [ilerlemiş](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Tanımlama bilgisi süre sonu ne zaman birden fazla yarısını sıfırlar olup olmadığını belirten bir bayrak `ExpireTimeSpan` aralığı sonlanana. Yeni exipiration saati geçerli tarihi olacak şekilde İleri taşınır artı `ExpireTimespan`. Bir [mutlak tanımlama bilgisi süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıfı çağrılırken `SignInAsync`. Mutlak sona erme süresini, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak uygulamanızın güvenliğini artırabilirsiniz. Varsayılan değer `true` şeklindedir. |

Ayarlama `CookieAuthenticationOptions` tanımlama bilgisi kimlik doğrulaması ara yazılımı `Configure` yöntemi:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Tanımlama bilgisi ilkesi Ara

[Tanımlama bilgisi ilkesi Ara](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) bir uygulamada tanımlama bilgisi ilkesi özellikleri sağlar. Uygulama işleme ardışık düzenine bir ara yazılım ekleme hassas sırasıdır; yalnızca bundan sonra işlem hattı, kayıtlı bileşenleri de etkiler.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) tanımlama bilgisi ilkesi ara yazılımı için sağlanan tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme işleyicileri tanımlama bilgisi işleme ve kanca genel özelliklerini denetlemenize izin.

| Özellik | Açıklama |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Tanımlama bilgilerini HttpOnly mi, bayrak tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını gösteren etkiler. Varsayılan değer `HttpOnlyPolicy.None` şeklindedir. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Tanımlama bilgisinin site aynı özniteliği (aşağıya bakın) etkiler. Varsayılan değer `SameSiteMode.Lax` şeklindedir. Bu seçenek için ASP.NET Core 2.0 + kullanılabilir. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Bir tanımlama bilgisi eklenir çağrılır. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Bir tanımlama bilgisi silindiğinde çağırılır. |
| [Güvenlik](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Tanımlama bilgilerini güvenli mi etkiler. Varsayılan değer `CookieSecurePolicy.None` şeklindedir. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + yalnızca)

Varsayılan `MinimumSameSitePolicy` değer `SameSiteMode.Lax` OAuth2 kimlik doğrulaması izin vermek için. Bir site aynı ilke kesinlikle uygulamak `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`. Bu ayar, OAuth2 ve diğer kaynaklar arası kimlik doğrulaması şemalarını keser olsa da, başka türden bir çıkış noktaları arası istek işleme güvenmeyin uygulamalar tanımlama bilgisinin güvenlik düzeyini yükseltir.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Tanımlama bilgisi ilkesi Ara ayarı `MinimumSameSitePolicy` , ayarı etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` aşağıdaki matris göre ayarlar.

| MinimumSameSitePolicy | Cookie.SameSite | Sonuç Cookie.SameSite ayarı |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Bir kimlik doğrulama tanımlama bilgisi oluştur

Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturmalıdır bir [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal). Kullanıcı bilgileri serileştirilmiş ve tanımlama bilgisinde depolanır. 

::: moniker range=">= aspnetcore-2.0"

Oluşturma bir [Claimsıdentity](/dotnet/api/system.security.claims.claimsidentity) gerekli ile [talep](/dotnet/api/system.security.claims.claim)s ve çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) kullanıcının oturum açmak için:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) kullanıcının oturum açmak için:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler. Belirtmezseniz bir `AuthenticationScheme`, varsayılan düzenini kullanılır.

Kullanılan şifreleme ASP.NET Core'nın temel alıyor [veri koruma](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistem. Birden çok makine, uygulamalar arasında Yük Dengeleme veya bir web grubu kullanarak uygulama düzenliyoruz. sonra yapmanız gerekenler [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtarı halka ve uygulama tanımlayıcısı.

## <a name="sign-out"></a>Oturumu kapat

::: moniker range=">= aspnetcore-2.0"

Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

Kullanmıyorsanız `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") kimlik doğrulama sağlayıcısı yapılandırırken kullandığınız şeması (örneğin, "ContosoCookie") düzeni olarak sağlayın. Aksi takdirde, varsayılan düzenini kullanılır.

## <a name="react-to-back-end-changes"></a>Arka uç değişikliklerine tepki

Bir tanımlama bilgisi oluşturulduktan sonra kimlik tek kaynağı haline gelir. Arka uç sistemlerinizi bir kullanıcıyı devre dışı olsa bile, bu bilgi tanımlama bilgisi kimlik doğrulama sistemi vardır ve kendi tanımlama bilgisinin geçerli olduğu sürece bir kullanıcı olarak oturum açmış kalır.

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) olayı ASP.NET Core 2.x veya [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) yöntemi ASP.NET Core 1.x kesebilir ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir. Bu yaklaşım, iptal edilen kullanıcıların uygulamaya erişmeden riskini azaltır.

Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanına değiştirildiğinde izleyen üzerinde temel alır. Kullanıcının tanımlama bilgisi düzenlendiğinden veritabanının değişip değişmediğini, kendi tanımlama bilgisini hala geçerli ise kullanıcının yeniden kimlik doğrulaması gerek yoktur. Bu senaryo, uygulanan veritabanı uygulamak için `IUserRepository` depolar Bu örnekte, bir `LastChanged` değeri. Herhangi bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.

Veritabanı değişikliklerini temel alan bir tanımlama bilgisi geçersiz kılmak için `LastChanged` değeri, tanımlama bilgisi ile oluşturma bir `LastChanged` geçerli içeren talep `LastChanged` veritabanından değer:

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

::: moniker range=">= aspnetcore-2.0"

Uygulama için bir geçersiz kılma `ValidatePrincipal` olay, bir yöntemin öğesinden türetilen bir sınıfta imzayla yazma [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Bir örnek, aşağıdaki gibi görünür:

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

Tanımlama bilgisi hizmet kaydı sırasında olayları örneğini kaydetme `ConfigureServices` yöntemi. Kapsamlı hizmeti kaydı için sağlayın, `CustomCookieAuthenticationEvents` sınıfı:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Uygulama için bir geçersiz kılma `ValidateAsync` olay, bir yöntemin imzayla yazma:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core kimliği bir parçası olarak bu onay uygulayan kendi [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Bir örnek, aşağıdaki gibi görünür:

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

Tanımlama bilgisi kimlik doğrulaması yapılandırması sırasında olay kaydetme `Configure` yöntemi:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

Kullanıcının name güncelleştirildiği bir durum düşünün &mdash; karar güvenlik herhangi bir şekilde etkilemez. Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ayarlayıp `context.ShouldRenew` özelliğini `true`.

> [!WARNING]
> Burada açıklanan yaklaşımı, her istek tetiklenir. Bu uygulama için bir büyük bir performans sorununa neden olabilir.

## <a name="persistent-cookies"></a>Kalıcı tanımlama bilgileri

Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz. Oturum açma veya benzer bir mekanizma "Beni Hatırla" onay kutusu açık kullanıcı onayı ile yalnızca bu Kalıcılık etkinleştirilmesi gerekir. 

Aşağıdaki kod parçacığı, bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır ilgili tanımlama bilgisi oluşturur. Daha önce yapılandırılmış tüm kayan zaman aşımı ayarları dikkate alınır. Tarayıcı kapalıyken tanımlama bilgisi süresi dolarsa, yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) sınıfı bulunduğu `Microsoft.AspNetCore.Authentication` ad alanı.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) sınıfı bulunduğu `Microsoft.AspNetCore.Http.Authentication` ad alanı.

::: moniker-end

## <a name="absolute-cookie-expiration"></a>Mutlak tanımlama bilgisi süre sonu

İle mutlak sona erme süresini ayarlayabilirsiniz `ExpiresUtc`. Kalıcı bir tanımlama bilgisi oluşturmak için de ayarlamalısınız `IsPersistent`; Aksi takdirde tanımlama bilgisi ile oturum tabanlı bir yaşam süresi oluşturulur ve önce ya da sona veya kimlik doğrulaması bileti sonra bu tutar. Zaman `ExpiresUtc` ayarlanır `SignInAsync`, değerini geçersiz kılar `ExpireTimeSpan` seçeneği `CookieAuthenticationOptions`, ayarlayın.

Aşağıdaki kod parçacığı, bir kimlik ve 20 dakika sürer ilgili tanımlama bilgisi oluşturur. Bu, daha önce yapılandırılmış tüm kayan zaman aşımı ayarları yok sayar.

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Kimlik doğrulama 2.0 değişiklikleri / geçiş Duyurusu](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [İlke tabanlı rol denetimleri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
