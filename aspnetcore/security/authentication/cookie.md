---
title: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan
author: rick-anderson
description: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622738"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

ASP.NET Core kimliği bir tam, tam özellikli bir kimlik doğrulama sağlayıcısı oluşturma ve oturum açma bilgileri korumak içindir. Ancak, bir ASP.NET Core kimliği olmadan tanımlama bilgisi tabanlı kimlik doğrulaması kimlik doğrulama sağlayıcısı kullanılabilir. Daha fazla bilgi için bkz. <xref:security/authentication/identity>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulamada tanıtım amacıyla kullanıcı kuramsal Maria Rodriguez, kullanıcının uygulamada oturum kodlanmış hesabıdır. Kullanım **e-posta** kullanıcı adı `maria.rodriguez@contoso.com` ve kullanıcı oturum açmak için herhangi bir parola. Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya. Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.

## <a name="configuration"></a>Yapılandırma

Uygulama kullanmıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), proje dosyası için bir paket başvuruya [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paket.

İçinde `Startup.ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmeti oluşturma <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ve <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> yöntemleri:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar. `AuthenticationScheme` tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme). Ayarı `AuthenticationScheme` için [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) düzeni için "Tanımlama bilgileri" değerini sağlar. Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz.

Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulaması düzeninden farklıdır. Ne zaman bir tanımlama bilgisi kimlik doğrulama düzeni değildir sağlanan <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, kullandığı `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").

Kimlik doğrulama tanımlama bilgisinin <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> özelliği `true` varsayılan olarak. Kimlik doğrulaması tanımlama bilgileri, bir ziyaretçi için veri toplama tarafından onaylanan taşınmadığından olduğunda izin verilir. Daha fazla bilgi için bkz. <xref:security/gdpr#essential-cookies>.

İçinde `Startup.Configure` yöntemi, çağrı `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği. Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.

Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `Startup.ConfigureServices` yöntemi:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Tanımlama bilgisi ilkesi Ara

[Tanımlama bilgisi ilkesi Ara](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) tanımlama bilgisi ilkesi özellikleri sağlar. Uygulama işleme ardışık düzenine bir ara yazılım ekleme sırası gizli olduğu&mdash;yalnızca bir işlem hattı, kayıtlı bir aşağı akış bileşenleri etkiler.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Kullanım <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme işleyicileri tanımlama bilgisi işleme ve kanca genel özelliklerini denetlemek için tanımlama bilgisi ilkesi Ara sağlanan.

Varsayılan <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> değer `SameSiteMode.Lax` OAuth2 kimlik doğrulaması izin vermek için. Bir site aynı ilke kesinlikle uygulamak `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`. Bu ayar, OAuth2 ve diğer kaynaklar arası kimlik doğrulaması şemalarını keser olsa da, başka türden bir çıkış noktaları arası istek işleme güvenmeyin uygulamalar tanımlama bilgisinin güvenlik düzeyini yükseltir.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Tanımlama bilgisi ilkesi Ara ayarı `MinimumSameSitePolicy` ayarını etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` aşağıdaki matris göre ayarlar.

| MinimumSameSitePolicy | Cookie.SameSite | Sonuç Cookie.SameSite ayarı |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Bir kimlik doğrulama tanımlama bilgisi oluştur

Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturun bir <xref:System.Security.Claims.ClaimsPrincipal>. Kullanıcı bilgileri serileştirilmiş ve tanımlama bilgisinde depolanır. 

Oluşturma bir <xref:System.Security.Claims.ClaimsIdentity> gerekli ile <xref:System.Security.Claims.Claim>s ve çağrı <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> kullanıcının oturum açmak için:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler. Varsa `AuthenticationScheme` belirtilmediyse, varsayılan düzenini kullanılır.

ASP.NET Core'nın [veri koruma](xref:security/data-protection/using-data-protection) sistem şifreleme için kullanılır. Birden fazla makinede barındırılan bir uygulama için uygulama arasında dengeleme ya da bir web grubu yük [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtarı halka ve uygulama tanımlayıcısı.

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Varsa `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") sağlayın (örneğin, "ContosoCookie"), düzeni gibi kimlik doğrulama sağlayıcısı yapılandırılırken kullanılan şema kullanılmaz. Aksi takdirde, varsayılan düzenini kullanılır.

## <a name="react-to-back-end-changes"></a>Arka uç değişikliklerine tepki

Bir tanımlama bilgisi oluşturulduktan sonra tanımlama bilgisi kimliği tek kaynaktır. Arka uç sistemleri bir kullanıcı hesabı devre dışı bırakılırsa:

* Uygulamanın tanımlama bilgisi kimlik doğrulama sistemi, kimlik doğrulama tanımlama bilgisinde istekleri işlemeye devam eder.
* Kimlik doğrulama tanımlama bilgisinin geçerli olduğu sürece, kullanıcı uygulamada oturum imzalı kalır.

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Olay kesebilir ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir. Her istekte tanımlama bilgisi doğrulama iptal edilen kullanıcıların uygulamaya erişmeden riskini azaltır.

Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanına değiştiğinde izleyen üzerinde temel alır. Kullanıcının tanımlama bilgisi düzenlendiğinden veritabanının değişip değişmediğini, kendi tanımlama bilgisini hala geçerli ise kullanıcının yeniden kimlik doğrulaması gerek yoktur. Örnek uygulamada, veritabanı uygulanan `IUserRepository` ve depolayan bir `LastChanged` değeri. Bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.

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

Uygulama için bir geçersiz kılma `ValidatePrincipal` olay, bir yöntemin, türetilen bir sınıfta imzayla yazma <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Aşağıdaki örnek uygulamasıdır `CookieAuthenticationEvents`:

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

Tanımlama bilgisi hizmet kaydı sırasında olayları örneğini kaydetme `Startup.ConfigureServices` yöntemi. Sağlayan bir [hizmet kayıt kapsamı](xref:fundamentals/dependency-injection#service-lifetimes) için `CustomCookieAuthenticationEvents` sınıfı:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Kullanıcının name güncelleştirildiği bir durum düşünün&mdash;karar güvenlik herhangi bir şekilde etkilemez. Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ayarlayıp `context.ShouldRenew` özelliğini `true`.

> [!WARNING]
> Burada açıklanan yaklaşımı, her istek tetiklenir. Her istekte tüm kullanıcılar için kimlik doğrulaması tanımlama bilgileri doğrulanıyor uygulama için bir büyük bir performans sorununa neden olabilir.

## <a name="persistent-cookies"></a>Kalıcı tanımlama bilgileri

Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz. Oturum açma "Beni Hatırla" onay kutusu veya benzer bir mekanizma ile açık kullanıcı onayı ile yalnızca bu Kalıcılık etkinleştirilmesi gerekir. 

Aşağıdaki kod parçacığı, bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır ilgili tanımlama bilgisi oluşturur. Daha önce yapılandırılmış tüm kayan zaman aşımı ayarları dikkate alınır. Tarayıcı kapalıyken tanımlama bilgisi süresi dolarsa, yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.

Ayarlama <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> için `true` içinde <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

Mutlak sona erme süresini ayarlanabilir <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Kalıcı bir tanımlama bilgisi oluşturmak için `IsPersistent` de ayarlamanız gerekir. Aksi takdirde tanımlama bilgisi ile oturum tabanlı bir yaşam süresi oluşturulur ve önce veya sonra tuttuğu kimlik doğrulama anahtarının süresi. Zaman `ExpiresUtc` , değerini geçersiz kılar, ayarlanmış <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> seçeneği <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, ayarlayın.

Aşağıdaki kod parçacığı, bir kimlik ve 20 dakika sürer ilgili tanımlama bilgisi oluşturur. Bu, daha önce yapılandırılmış tüm kayan zaman aşımı ayarları yok sayar.

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [İlke tabanlı rol denetimleri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
