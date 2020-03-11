---
title: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanma
author: rick-anderson
description: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 64f881441a7a7f9a5529cb6ee5ce81142ccd69e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663004"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanma

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core kimlik, oturum açma işlemleri oluşturmaya ve korumaya yönelik eksiksiz, tam özellikli bir kimlik doğrulama sağlayıcısıdır. Ancak, ASP.NET Core kimliği olmayan tanımlama bilgisi tabanlı bir kimlik doğrulama sağlayıcısı kullanılabilir. Daha fazla bilgi için bkz. <xref:security/authentication/identity>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulamadaki tanıtım amacıyla, Maria Rodriguez olan kuramsal kullanıcının Kullanıcı hesabı, uygulamaya sabit olarak kodlanmıştır. Kullanıcı oturumu açmak için **e-posta** adresi `maria.rodriguez@contoso.com` ve herhangi bir parolayı kullanın. Kullanıcının kimliği, *Sayfalar/Account/Login. cshtml. cs* dosyasındaki `AuthenticateUser` yönteminde doğrulanır. Gerçek dünyada bir örnekte, kullanıcının kimliği bir veritabanında doğrulanır.

## <a name="configuration"></a>Yapılandırma

Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmıyorsa, [Microsoft. Aspnetcore. Authentication. Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paketi için proje dosyasında bir paket başvurusu oluşturun.

`Startup.ConfigureServices` yönteminde, <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ve <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> yöntemleriyle kimlik doğrulama ara yazılım hizmetleri oluşturun:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

`AddAuthentication` geçirilen <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, uygulamanın varsayılan kimlik doğrulama şemasını ayarlar. `AuthenticationScheme`, tanımlama bilgisi kimlik doğrulamasının birden çok örneği olduğunda ve [belirli bir şemayla yetkilendirmek](xref:security/authorization/limitingidentitybyscheme)istediğiniz durumlarda kullanışlıdır. `AuthenticationScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) olarak ayarlanması, düzen Için "tanımlama bilgileri" değeri sağlar. Düzeni ayıran herhangi bir dize değeri sağlayabilirsiniz.

Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulama düzeninden farklıdır. <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>için bir tanımlama bilgisi kimlik doğrulama düzeni sağlanmamışsa, `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies") kullanır.

Kimlik doğrulama tanımlama bilgisinin <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> özelliği varsayılan olarak `true` olarak ayarlanır. Bir site ziyaretçisi veri toplamaya onay vermemişse kimlik doğrulama tanımlama bilgilerine izin verilir. Daha fazla bilgi için bkz. <xref:security/gdpr#essential-cookies>.

`Startup.Configure`, `HttpContext.User` özelliğini ayarlamak ve istekler için yetkilendirme ara yazılımını çalıştırmak için `UseAuthentication` ve `UseAuthorization` çağırın. `UseEndpoints`çağrılmadan önce `UseAuthentication` ve `UseAuthorization` yöntemlerini çağırın:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

`Startup.ConfigureServices` yönteminde kimlik doğrulaması için hizmet yapılandırmasında `CookieAuthenticationOptions` ayarlayın:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Tanımlama bilgisi Ilkesi ara yazılımı

[Tanımlama bilgisi Ilkesi ara yazılımı](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) , tanımlama bilgisi İlkesi yeteneklerini sunar. Ara yazılımı uygulama işleme işlem hattına eklemek&mdash;, yalnızca işlem hattına kayıtlı aşağı akış bileşenlerini etkiler.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Tanımlama bilgisi işlemenin genel özelliklerini denetlemek için tanımlama bilgisi Ilkesi ara yazılımı ' nı <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> kullanın ve tanımlama bilgileri eklenmiş veya silinmiş olduğunda tanımlama bilgisi işleme işleyicilerine kanca ekleyin.

Varsayılan <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> değeri, OAuth2 kimlik doğrulamasına izin verecek şekilde `SameSiteMode.Lax`. `SameSiteMode.Strict`aynı site ilkesini kesinlikle zorlamak için `MinimumSameSitePolicy`ayarlayın. Bu ayar, OAuth2 ve diğer çapraz kaynak kimlik doğrulama düzenlerini kesse de, çıkış noktaları arası istek işlemeye bağlı olmayan diğer uygulama türleri için tanımlama bilgisi güvenlik düzeyini yükseltir.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

`MinimumSameSitePolicy` için tanımlama bilgisi Ilkesi ara yazılım ayarı, aşağıdaki matriye göre `CookieAuthenticationOptions` ayarlarındaki `Cookie.SameSite` ayarını etkileyebilir.

| MinimumSameSitePolicy | Cookie. SameSite | Sonuç tanımlama bilgisi. SameSite ayarı |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode. None     | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict |
| SameSiteMode. LAX      | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. LAX<br>SameSiteMode. LAX<br>SameSiteMode. Strict |
| SameSiteMode. Strict   | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. Strict<br>SameSiteMode. Strict<br>SameSiteMode. Strict |

## <a name="create-an-authentication-cookie"></a>Kimlik doğrulama tanımlama bilgisi oluşturma

Kullanıcı bilgilerini tutan bir tanımlama bilgisi oluşturmak için bir <xref:System.Security.Claims.ClaimsPrincipal>oluşturun. Kullanıcı bilgileri serileştirilir ve tanımlama bilgisinde depolanır. 

Gerekli <xref:System.Security.Claims.Claim>s <xref:System.Security.Claims.ClaimsIdentity> oluşturun ve Kullanıcı oturumu açmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> çağırın:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` şifreli bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler. `AuthenticationScheme` belirtilmemişse, varsayılan düzen kullanılır.

ASP.NET Core [veri koruma](xref:security/data-protection/using-data-protection) sistemi şifreleme için kullanılır. Birden çok makinede barındırılan bir uygulama, uygulamalar arasında yük dengeleme veya bir Web grubu kullanma için, [veri korumayı](xref:security/data-protection/configuration/overview) aynı anahtar halkasını ve uygulama tanımlayıcısını kullanacak şekilde yapılandırın.

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>çağırın:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

`CookieAuthenticationDefaults.AuthenticationScheme` (veya "Cookies"), düzen olarak (örneğin, "ContosoCookie") kullanılmazsa, kimlik doğrulama sağlayıcısını yapılandırırken kullanılan düzeni sağlayın. Aksi takdirde, varsayılan düzen kullanılır.

## <a name="react-to-back-end-changes"></a>Arka uç değişikliklerine tepki verme

Tanımlama bilgisi oluşturulduktan sonra tanımlama bilgisi tek kimlik kaynağıdır. Arka uç sistemlerinde bir kullanıcı hesabı devre dışı bırakılmışsa:

* Uygulamanın tanımlama bilgisi kimlik doğrulama sistemi, kimlik doğrulama tanımlama bilgisine göre istekleri işlemeye devam eder.
* Kimlik doğrulama tanımlama bilgisi geçerli olduğu sürece kullanıcı uygulamada oturum açmış durumda kalır.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> olayı, tanımlama bilgisi kimliğinin doğrulanmasını ve geçersiz kılınması için kullanılabilir. Her istekte tanımlama bilgisinin doğrulanması, uygulamaya erişen kullanıcıların iptal edilmesinin riskini azaltır.

Tanımlama bilgisi doğrulamasına yönelik bir yaklaşım, kullanıcı veritabanının değiştiği zaman izlemenin izlenmesine bağlıdır. Kullanıcının tanımlama bilgisi verildikten sonra veritabanı değiştirilmediyse, tanımlama bilgisi hala geçerliyse kullanıcının kimliğini yeniden doğrulamaya gerek yoktur. Örnek uygulamada, veritabanı `IUserRepository` uygulanır ve bir `LastChanged` değeri depolar. Veritabanında bir Kullanıcı güncelleştirildiği zaman, `LastChanged` değeri geçerli saate ayarlanır.

Veritabanı `LastChanged` değerine göre değiştiğinde bir tanımlama bilgisini geçersiz kılmak için, veritabanından geçerli `LastChanged` değerini içeren bir `LastChanged` talep ile tanımlama bilgisini oluşturun:

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

`ValidatePrincipal` olayına bir geçersiz kılma uygulamak için, <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>türetilen bir sınıfa aşağıdaki imzaya sahip bir yöntem yazın:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Aşağıda `CookieAuthenticationEvents`örnek bir uygulamasıdır:

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

`Startup.ConfigureServices` yönteminde tanımlama bilgisi hizmeti kaydı sırasında olay örneğini kaydedin. `CustomCookieAuthenticationEvents` sınıfınız için [kapsamlı bir hizmet kaydı](xref:fundamentals/dependency-injection#service-lifetimes) sağlayın:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Kullanıcı adının güncelleştirildiği bir durumu, güvenliği hiçbir şekilde etkilemeyen bir kararı&mdash;göz önünde bulundurun. Kullanıcı sorumlusunu kalıcı olarak güncelleştirmek istiyorsanız, `context.ReplacePrincipal` çağırın ve `context.ShouldRenew` özelliğini `true`olarak ayarlayın.

> [!WARNING]
> Burada açıklanan yaklaşım her istekte tetiklenir. Her istekteki tüm kullanıcılar için kimlik doğrulama tanımlama bilgilerinin doğrulanması, uygulama için büyük bir performans cezası oluşmasına neden olabilir.

## <a name="persistent-cookies"></a>Kalıcı tanımlama bilgileri

Tanımlama bilgisinin tarayıcı oturumları arasında kalıcı olmasını isteyebilirsiniz. Bu Kalıcılık yalnızca oturum açma veya benzer bir mekanizmanın "Beni anımsa" onay kutusu ile açık kullanıcı onayı ile etkinleştirilmelidir. 

Aşağıdaki kod parçacığı, tarayıcı kapanışları aracılığıyla ilerlikli bir kimlik ve ilgili tanımlama bilgisi oluşturur. Daha önce yapılandırılmış tüm Kayan süre sonu ayarları kabul edilir. Tarayıcı kapalıyken tanımlama bilgisinin süresi dolarsa tarayıcı, yeniden başlatıldıktan sonra tanımlama bilgisini temizler.

<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>`true` olarak ayarlayın:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Mutlak tanımlama bilgisi süre sonu

Mutlak bir süre sonu, <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>ile ayarlanabilir. Kalıcı bir tanımlama bilgisi oluşturmak için, `IsPersistent` de ayarlanması gerekir. Aksi takdirde, tanımlama bilgisi oturum tabanlı bir yaşam süresi ile oluşturulur ve bu kimlik doğrulama biletinden önce ya da sonra zaman alabilir. `ExpiresUtc` ayarlandığında, ayarlandıysa, <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> seçeneğinin değerini geçersiz kılar.

Aşağıdaki kod parçacığı, 20 dakika boyunca bir kimlik ve karşılık gelen tanımlama bilgisi oluşturur. Bu, daha önce yapılandırılmış olan tüm Kayan süre sonu ayarlarını yoksayar.

```csharp
// using Microsoft.AspNetCore.Authentication;

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

::: moniker range="< aspnetcore-3.0"

ASP.NET Core kimlik, oturum açma işlemleri oluşturmaya ve korumaya yönelik eksiksiz, tam özellikli bir kimlik doğrulama sağlayıcısıdır. Ancak, ASP.NET Core kimliği olmayan tanımlama bilgisi tabanlı kimlik doğrulama sağlayıcısı kullanılabilir. Daha fazla bilgi için bkz. <xref:security/authentication/identity>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulamadaki tanıtım amacıyla, Maria Rodriguez olan kuramsal kullanıcının Kullanıcı hesabı, uygulamaya sabit olarak kodlanmıştır. Kullanıcı oturumu açmak için **e-posta** adresi `maria.rodriguez@contoso.com` ve herhangi bir parolayı kullanın. Kullanıcının kimliği, *Sayfalar/Account/Login. cshtml. cs* dosyasındaki `AuthenticateUser` yönteminde doğrulanır. Gerçek dünyada bir örnekte, kullanıcının kimliği bir veritabanında doğrulanır.

## <a name="configuration"></a>Yapılandırma

Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)kullanmıyorsa, [Microsoft. Aspnetcore. Authentication. Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paketi için proje dosyasında bir paket başvurusu oluşturun.

`Startup.ConfigureServices` yönteminde, kimlik doğrulama ara yazılım hizmetini <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ve <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> yöntemlerle oluşturun:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AddAuthentication` geçirilen <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, uygulamanın varsayılan kimlik doğrulama şemasını ayarlar. `AuthenticationScheme`, tanımlama bilgisi kimlik doğrulamasının birden çok örneği olduğunda ve [belirli bir şemayla yetkilendirmek](xref:security/authorization/limitingidentitybyscheme)istediğiniz durumlarda kullanışlıdır. `AuthenticationScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) olarak ayarlanması, düzen Için "tanımlama bilgileri" değeri sağlar. Düzeni ayıran herhangi bir dize değeri sağlayabilirsiniz.

Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulama düzeninden farklıdır. <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>için bir tanımlama bilgisi kimlik doğrulama düzeni sağlanmamışsa, `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies") kullanır.

Kimlik doğrulama tanımlama bilgisinin <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> özelliği varsayılan olarak `true` olarak ayarlanır. Bir site ziyaretçisi veri toplamaya onay vermemişse kimlik doğrulama tanımlama bilgilerine izin verilir. Daha fazla bilgi için bkz. <xref:security/gdpr#essential-cookies>.

`Startup.Configure` yönteminde, `HttpContext.User` özelliğini ayarlayan kimlik doğrulama ara yazılımını çağırmak için `UseAuthentication` yöntemini çağırın. `UseMvcWithDefaultRoute` veya `UseMvc`çağrılmadan önce `UseAuthentication` yöntemi çağırın:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

`Startup.ConfigureServices` yönteminde kimlik doğrulaması için hizmet yapılandırmasında `CookieAuthenticationOptions` ayarlayın:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Tanımlama bilgisi Ilkesi ara yazılımı

[Tanımlama bilgisi Ilkesi ara yazılımı](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) , tanımlama bilgisi İlkesi yeteneklerini sunar. Ara yazılımı uygulama işleme işlem hattına eklemek&mdash;, yalnızca işlem hattına kayıtlı aşağı akış bileşenlerini etkiler.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Tanımlama bilgisi işlemenin genel özelliklerini denetlemek için tanımlama bilgisi Ilkesi ara yazılımı ' nı <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> kullanın ve tanımlama bilgileri eklenmiş veya silinmiş olduğunda tanımlama bilgisi işleme işleyicilerine kanca ekleyin.

Varsayılan <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> değeri, OAuth2 kimlik doğrulamasına izin verecek şekilde `SameSiteMode.Lax`. `SameSiteMode.Strict`aynı site ilkesini kesinlikle zorlamak için `MinimumSameSitePolicy`ayarlayın. Bu ayar, OAuth2 ve diğer çapraz kaynak kimlik doğrulama düzenlerini kesse de, çıkış noktaları arası istek işlemeye bağlı olmayan diğer uygulama türleri için tanımlama bilgisi güvenlik düzeyini yükseltir.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

`MinimumSameSitePolicy` için tanımlama bilgisi Ilkesi ara yazılım ayarı, aşağıdaki matriye göre `CookieAuthenticationOptions` ayarlarındaki `Cookie.SameSite` ayarını etkileyebilir.

| MinimumSameSitePolicy | Cookie. SameSite | Sonuç tanımlama bilgisi. SameSite ayarı |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode. None     | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict |
| SameSiteMode. LAX      | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. LAX<br>SameSiteMode. LAX<br>SameSiteMode. Strict |
| SameSiteMode. Strict   | SameSiteMode. None<br>SameSiteMode. LAX<br>SameSiteMode. Strict | SameSiteMode. Strict<br>SameSiteMode. Strict<br>SameSiteMode. Strict |

## <a name="create-an-authentication-cookie"></a>Kimlik doğrulama tanımlama bilgisi oluşturma

Kullanıcı bilgilerini tutan bir tanımlama bilgisi oluşturmak için bir <xref:System.Security.Claims.ClaimsPrincipal>oluşturun. Kullanıcı bilgileri serileştirilir ve tanımlama bilgisinde depolanır. 

Gerekli <xref:System.Security.Claims.Claim>s <xref:System.Security.Claims.ClaimsIdentity> oluşturun ve Kullanıcı oturumu açmak için <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> çağırın:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` şifreli bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler. `AuthenticationScheme` belirtilmemişse, varsayılan düzen kullanılır.

ASP.NET Core [veri koruma](xref:security/data-protection/using-data-protection) sistemi şifreleme için kullanılır. Birden çok makinede barındırılan bir uygulama, uygulamalar arasında yük dengeleme veya bir Web grubu kullanma için, [veri korumayı](xref:security/data-protection/configuration/overview) aynı anahtar halkasını ve uygulama tanımlayıcısını kullanacak şekilde yapılandırın.

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>çağırın:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

`CookieAuthenticationDefaults.AuthenticationScheme` (veya "Cookies"), düzen olarak (örneğin, "ContosoCookie") kullanılmazsa, kimlik doğrulama sağlayıcısını yapılandırırken kullanılan düzeni sağlayın. Aksi takdirde, varsayılan düzen kullanılır.

## <a name="react-to-back-end-changes"></a>Arka uç değişikliklerine tepki verme

Tanımlama bilgisi oluşturulduktan sonra tanımlama bilgisi tek kimlik kaynağıdır. Arka uç sistemlerinde bir kullanıcı hesabı devre dışı bırakılmışsa:

* Uygulamanın tanımlama bilgisi kimlik doğrulama sistemi, kimlik doğrulama tanımlama bilgisine göre istekleri işlemeye devam eder.
* Kimlik doğrulama tanımlama bilgisi geçerli olduğu sürece kullanıcı uygulamada oturum açmış durumda kalır.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> olayı, tanımlama bilgisi kimliğinin doğrulanmasını ve geçersiz kılınması için kullanılabilir. Her istekte tanımlama bilgisinin doğrulanması, uygulamaya erişen kullanıcıların iptal edilmesinin riskini azaltır.

Tanımlama bilgisi doğrulamasına yönelik bir yaklaşım, kullanıcı veritabanının değiştiği zaman izlemenin izlenmesine bağlıdır. Kullanıcının tanımlama bilgisi verildikten sonra veritabanı değiştirilmediyse, tanımlama bilgisi hala geçerliyse kullanıcının kimliğini yeniden doğrulamaya gerek yoktur. Örnek uygulamada, veritabanı `IUserRepository` uygulanır ve bir `LastChanged` değeri depolar. Veritabanında bir Kullanıcı güncelleştirildiği zaman, `LastChanged` değeri geçerli saate ayarlanır.

Veritabanı `LastChanged` değerine göre değiştiğinde bir tanımlama bilgisini geçersiz kılmak için, veritabanından geçerli `LastChanged` değerini içeren bir `LastChanged` talep ile tanımlama bilgisini oluşturun:

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

`ValidatePrincipal` olayına bir geçersiz kılma uygulamak için, <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>türetilen bir sınıfa aşağıdaki imzaya sahip bir yöntem yazın:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Aşağıda `CookieAuthenticationEvents`örnek bir uygulamasıdır:

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

`Startup.ConfigureServices` yönteminde tanımlama bilgisi hizmeti kaydı sırasında olay örneğini kaydedin. `CustomCookieAuthenticationEvents` sınıfınız için [kapsamlı bir hizmet kaydı](xref:fundamentals/dependency-injection#service-lifetimes) sağlayın:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Kullanıcı adının güncelleştirildiği bir durumu, güvenliği hiçbir şekilde etkilemeyen bir kararı&mdash;göz önünde bulundurun. Kullanıcı sorumlusunu kalıcı olarak güncelleştirmek istiyorsanız, `context.ReplacePrincipal` çağırın ve `context.ShouldRenew` özelliğini `true`olarak ayarlayın.

> [!WARNING]
> Burada açıklanan yaklaşım her istekte tetiklenir. Her istekteki tüm kullanıcılar için kimlik doğrulama tanımlama bilgilerinin doğrulanması, uygulama için büyük bir performans cezası oluşmasına neden olabilir.

## <a name="persistent-cookies"></a>Kalıcı tanımlama bilgileri

Tanımlama bilgisinin tarayıcı oturumları arasında kalıcı olmasını isteyebilirsiniz. Bu Kalıcılık yalnızca oturum açma veya benzer bir mekanizmanın "Beni anımsa" onay kutusu ile açık kullanıcı onayı ile etkinleştirilmelidir. 

Aşağıdaki kod parçacığı, tarayıcı kapanışları aracılığıyla ilerlikli bir kimlik ve ilgili tanımlama bilgisi oluşturur. Daha önce yapılandırılmış tüm Kayan süre sonu ayarları kabul edilir. Tarayıcı kapalıyken tanımlama bilgisinin süresi dolarsa tarayıcı, yeniden başlatıldıktan sonra tanımlama bilgisini temizler.

<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>`true` olarak ayarlayın:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Mutlak tanımlama bilgisi süre sonu

Mutlak bir süre sonu, <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>ile ayarlanabilir. Kalıcı bir tanımlama bilgisi oluşturmak için, `IsPersistent` de ayarlanması gerekir. Aksi takdirde, tanımlama bilgisi oturum tabanlı bir yaşam süresi ile oluşturulur ve bu kimlik doğrulama biletinden önce ya da sonra zaman alabilir. `ExpiresUtc` ayarlandığında, ayarlandıysa, <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions><xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> seçeneğinin değerini geçersiz kılar.

Aşağıdaki kod parçacığı, 20 dakika boyunca bir kimlik ve karşılık gelen tanımlama bilgisi oluşturur. Bu, daha önce yapılandırılmış olan tüm Kayan süre sonu ayarlarını yoksayar.

```csharp
// using Microsoft.AspNetCore.Authentication;

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [İlke tabanlı rol denetimleri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
