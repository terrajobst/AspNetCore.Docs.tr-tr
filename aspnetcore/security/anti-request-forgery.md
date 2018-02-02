---
title: "ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme"
author: steve-smith
description: "ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme"
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 079c36535b8c9e7229952a2f7bcd53174effa6af
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET Core içinde siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını önleme

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>Hangi saldırı sahteciliğe karşı koruma engellemez?

Siteler arası istek sahtekarlığı (olarak da bilinen XSRF veya CSRF, belirgin *bakın surf*) web barındırılan uygulamalar yapabildiği kötü amaçlı bir web sitesi etkilemek istemci tarayıcısına ve güvendiği bir web sitesi arasındaki etkileşim karşı bir saldırı Bu tarayıcı. Web tarayıcıları bir web sitesi için kimlik doğrulama belirteçleri bazı türleri her istek ile otomatik olarak göndermek için bu saldırıların olası yapılır. Bu biçimi yararlanma, kullanıcının olarak da bilinen bir *tek tıklatmayla saldırı* veya as *arabası oturum*, saldırı yararlanır kullanıcı daha önce oturum kimliği doğrulanmış.

CSRF saldırı örneği:

1. İçine bir kullanıcı oturum `www.example.com`, forms kimlik doğrulaması kullanma.
2. Sunucu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisini içeren bir yanıt verir.
3. Kullanıcı kötü amaçlı bir siteyi ziyaret eder.

   Kötü amaçlı site aşağıdakine benzer bir HTML formuna içerir:

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

Form eylemi kötü amaçlı siteye değil savunmasız siteye yazılarını dikkat edin. CSRF "siteler arası" parçasıdır.

4. Kullanıcı gönder düğmesine tıklar. Tarayıcı, kimlik doğrulama tanımlama bilgisi istekle istenen etki alanı (Bu durumda savunmasız site) için otomatik olarak ekler.
5. İstek, kullanıcının kimlik doğrulaması bağlamı ile sunucuya çalışır ve kimliği doğrulanmış bir kullanıcı yapmak için izin verilen herhangi bir eylem gerçekleştirebilir.

Bu örnekte form düğmesini kullanıcıya gerektirir. Kötü amaçlı sayfası olabilir:

* Otomatik olarak form gönderen bir komut dosyasını çalıştırın.
* Form gönderme AJAX isteği gönderir. 
* Gizli bir form CSS ile kullanın. 

SSL olmayan engel CSRF saldırı, zararlı site gönderebilirsiniz bir `https://` isteği. 

Bazı saldırılar yanıt site uç noktaları hedef `GET` istekleri, hangi durumda resim etiketi (Bu saldırı biçimidir ortak görüntüleri izin ancak JavaScript engelleme Forumu sitelerinde) eylemi gerçekleştirmek için kullanılabilir. İle durum değişikliği uygulamaları `GET` istekleri kötü amaçlı saldırılara karşı savunmasız.

Tarayıcıların tüm ilgili tanımlama bilgilerini hedef web sitesine gönderdiği çünkü CSRF saldırılarına karşı kimlik doğrulaması için tanımlama bilgileri kullanan web siteleri mümkündür. Ancak, CSRF saldırıları tanımlama bilgisinden faydalanmakta için sınırlı değildir. Örneğin, temel ve Özet kimlik doğrulaması da savunmasız. Bir kullanıcının temel veya Özet kimlik doğrulaması ile oturum açtığı sonra oturumu sona kadar tarayıcı otomatik olarak kimlik bilgilerini gönderir.

Not: Bu bağlamda *oturum* sırasında kullanıcının kimliğinin istemci-tarafı oturumuna başvuruyor. Bu sunucu tarafı oturumlarını ilgisiz veya [oturum Ara](xref:fundamentals/app-state).

Kullanıcılar tarafından CSRF güvenlik açıklarına karşı önleyebilirsiniz:
* Bunları kullanmayı bitirdikten sonra web sitelerinde oturum.
* Kendi tarayıcının tanımlama bilgilerini düzenli olarak temizleme.

Ancak, CSRF güvenlik açıkları temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>ASP.NET Core MVC CSRF nasıl ele?

> [!WARNING]
> ASP.NET Core uygulayan anti-request-sahte kullanarak [ASP.NET Core veri koruma yığını](xref:security/data-protection/introduction). ASP.NET Core veri koruma, bir sunucu grubunda çalışmak için yapılandırılmalıdır. Bkz: [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.

ASP.NET Core anti-request-sahte varsayılan veri koruma yapılandırması 

ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) sahteciliğe karşı koruma belirteçleri için HTML form öğelerini yerleştirir. Örneğin, bir Razor dosyasının aşağıdaki biçimlendirmede sahteciliğe karşı koruma belirteçlerini otomatik olarak oluşturur:

```html
<form method="post">
  <!-- form markup -->
</form>
```

Otomatik olarak oluşturulmasını sahteciliğe karşı koruma belirteçleri için HTML form öğelerini olur zaman:

* `form` Etiketinde `method="post"` özniteliği ve

  * Boş eylem özniteliğidir. ( `action=""`) OR
  * Eylem öznitelik sağlanan değil. (`<form method="post">`)

Sahteciliğe karşı koruma belirteçlerini otomatik olarak oluşturulmasını HTML form öğelerini tarafından için devre dışı bırakabilirsiniz:

* Açıkça devre dışı bırakma `asp-antiforgery`. Örneğin

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* Etiket Yardımcısını kullanarak form öğesi etiket Yardımcıları dışında opt [! çevirin simgesi](xref:mvc/views/tag-helpers/intro#opt-out).

 ```html
  <!form method="post">
  </!form>
  ```

* Kaldırma `FormTagHelper` görünümünden. Kaldırabileceğiniz `FormTagHelper` Razor görünümüne aşağıdaki yönergesi ekleyerek görünümden:

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor sayfalarının](xref:mvc/razor-pages/index) XSRF/CSRF otomatik olarak korunur. Herhangi bir ek kod yazmak zorunda değilsiniz. Bkz: [XSRF/CSRF ve Razor sayfalarının](xref:mvc/razor-pages/index#xsrf) daha fazla bilgi için.

CSRF saldırılarına karşı savunma en yaygın Eşitleyici belirteci deseni (STP) yaklaşımdır. STP kullanıcı form verilerini içeren bir sayfa istediğinde kullanılan bir tekniktir. Sunucu, istemci için geçerli kullanıcının kimliği ile ilişkili bir belirteç gönderir. İstemci belirteci doğrulama için sunucuya geri gönderir. Sunucunun kimliği doğrulanmış kullanıcının kimliğini eşleşmeyen bir belirteç alırsa, isteği reddedilir. Belirteç, benzersiz ve tahmin edilemez. Belirteç, bir dizi (sayfa 1 sağlama sayfası 3 önündeki sayfa 2 önündeki) istekleri uygun sıralama emin olmak için de kullanılabilir. ASP.NET Core MVC şablonları tüm formlarında antiforgery belirteçleri oluşturur. Aşağıdaki iki örnek görünümü mantığı antiforgery belirteçleri oluşturur:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

Bir antiforgery belirteci açıkça ekleyebileceğiniz bir ``<form>`` etiket Yardımcıları ile HTML Yardımcısı kullanmadan öğesi ``@Html.AntiForgeryToken``:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Her önceki durumlarda, ASP.NET Core aşağıdakine benzer bir gizli bir form alanı ekleyin:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, ve ``IgnoreAntiforgeryToken``.

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

``ValidateAntiForgeryToken`` Tek tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak. İsteğin geçerli bir antiforgery belirteci içermedikçe Bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

``ValidateAntiForgeryToken`` Özniteliği eylem yöntemlerine onu süsler, dahil olmak üzere istekleri için bir belirteç gerektirir `HTTP GET` istekleri. Geniş çapta uygularsanız, kendisiyle kılabilirsiniz ``IgnoreAntiforgeryToken`` özniteliği.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

ASP.NET Core uygulamaları genellikle HTTP güvenli yöntemleri (GET, HEAD, seçenekleri ve izleme) için antiforgery belirteçleri oluşturmak yok. Kapsamlı uygulama yerine ``ValidateAntiForgeryToken`` özniteliği ve ile geçersiz kılma ``IgnoreAntiforgeryToken`` kullanabileceğiniz öznitelikleri ``AutoValidateAntiforgeryToken`` özniteliği. Bu öznitelik için aynı şekilde çalışır ``ValidateAntiForgeryToken`` için aşağıdaki HTTP yöntemleri kullanılarak yapılan istekleri belirteçleri gerektirmeyen dışında öznitelik:

* AL
* HEAD
* SEÇENEKLER
* TRACE

Şunu kullanmanızı öneririz ``AutoValidateAntiforgeryToken`` API olmayan senaryolar için kapsamlı. Bu işlem sonrası eylemler varsayılan olarak korunan sağlar. Varsayılan olarak, antiforgery belirteçleri yoksaymayı sürece alternatiftir ``ValidateAntiForgeryToken`` tek tek eylem yöntemine uygulanır. Bu senaryoda olmasını POST eylem yöntemi için büyük olasılıkla korumasız, sol uygulamanızı CSRF saldırılara karşı savunmasız bırakır. Hatta anonim GÖNDERİLERİ antiforgery belirteci göndermesi gerekir.

Not: API'leri tanımlama bilgisi olmayan belirtecinin bir parçası göndermek için bir otomatik mekanizması gerekmez; Uygulamanız, büyük olasılıkla, istemci kodu uygulamanızı bağlı olacaktır. Aşağıda bazı örnekler gösterilmektedir.


Örnek (sınıf düzeyinde):

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Örnek (Genel):

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

``IgnoreAntiforgeryToken`` Filtre, belirli bir eylem (veya denetleyicisi) olması için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır. Uygulandığında, bu filtre geçersiz kılar ``ValidateAntiForgeryToken`` ve/veya ``AutoValidateAntiforgeryToken`` daha yüksek bir düzeyde (genel olarak veya bir denetleyicisinde) belirtilen filtreler.

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX ve SPAs

Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanlarını kullanarak sunucuya geçirilir. Modern JavaScript tabanlı uygulamalar ve tek sayfa uygulamaları (SPAs), birçok istek program aracılığıyla yapılır. AJAX istekleri belirteç göndermek için başka teknikler (örneğin, istek üstbilgileri veya tanımlama bilgileri) kullanabilir. Tanımlama bilgileri kimlik doğrulama belirteçleri depolamak ve API isteklerinin sunucusunda kimlik doğrulaması için kullanılıyorsa, CSRF olası bir sorun olacaktır. Belirteç depolamak için yerel depolama alanı kullandıysanız, yerel depolama değerlerinden her yeni isteği sunucusuyla otomatik olarak gönderilmez beri ancak CSRF güvenlik açığı, azaltılması gereken. Bu nedenle, istemci ve bir istek üstbilgisini önerilen yaklaşımdır olarak belirtecin gönderme antiforgery belirteci depolamak için yerel depolama kullanma.

### <a name="angularjs"></a>AngularJS

AngularJS CSRF adresine bir kuralı kullanılır. Sunucu tanımlama bilgisi adı ile gönderirse ``XSRF-TOKEN``, Angular ``$http`` hizmet ekleyecek değeri bu tanımlama bilgisinden bir üst bilginin bu sunucu için bir istek gönderdiğinde. Bu işlemi otomatiktir; Üstbilgi açıkça ayarlamanız gerekmez. Üstbilgi adı ``X-XSRF-TOKEN``. Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.

ASP.NET Core API çalışmak için bu kural:

* Adlı bir tanımlama bilgisine bir belirteç sağlamak için uygulamanızı yapılandırma``XSRF-TOKEN``
* Adında bir başlık aramak için antiforgery hizmetini yapılandırma``X-XSRF-TOKEN``

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Görünüm örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

JavaScript görünümlerle kullanarak, görünümü içinde hizmetinden kullanarak belirteci oluşturabilirsiniz. Bunu yapmak için ekleme `Microsoft.AspNetCore.Antiforgery.IAntiforgery` görüntüleyebileceği ve çağırabileceği içine hizmet `GetAndStoreTokens`gösterildiği gibi:

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

Bu yaklaşım doğrudan sunucudan tanımlama bilgilerini ayarlama veya istemciden okuma uğraşmanız gereğini ortadan kaldırır.

JavaScript ayrıca tanımlama bilgilerini sağlanan belirteçleri erişmek ve ardından tanımlama bilgisinin içeriği üstbilgi belirtecin değeri ile oluşturmak için aşağıda gösterildiği gibi kullanın.

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Ardından, belirteci olarak adlandırılan bir üstbilgisinde göndermek için komut dosyanızı oluşturmaya varsayılarak istekleri ``X-CSRF-TOKEN``, aranacak antiforgery hizmetini yapılandırma ``X-CSRF-TOKEN`` üstbilgisi:

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Aşağıdaki örnek, uygun üstbilgiyle AJAX isteği yapmak için jQuery kullanır:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Antiforgery yapılandırma

`IAntiforgery`antiforgery sistemi yapılandırmak için API sağlar. İçinde istenebilir `Configure` yöntemi `Startup` sınıfı. Aşağıdaki örnek, antiforgery bir belirteç oluşturmak ve yanıtta (yukarıda açıklanan varsayılan Açısal adlandırma kuralını kullanarak) bir tanımlama bilgisi olarak göndermek için uygulamanın giriş sayfasından ara yazılımını kullanır:


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Seçenekler

Özelleştirebileceğiniz [antiforgery seçenekleri](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) içinde `ConfigureServices`:

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Seçenek        | Açıklama |
|------------- | ----------- |
|CookieDomain  | Tanımlama bilgisinin etki alanı. Varsayılan olarak `null`. |
|CookieName    | Tanımlama bilgisinin adı. Ayarlanmadı, sistem ile başlayan bir benzersiz bir ad oluşturur `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | Yolu tanımlama bilgisinde ayarlanır. |
|FormFieldName | Görünümlerde antiforgery belirteçleri oluşturmak için antiforgery sistem tarafından kullanılan gizli form alanının adı. |
|HeaderName    | Antiforgery sistem tarafından kullanılan üstbilginin adı. Varsa `null`, sistem yalnızca form verilerini değerlendirir. |
|requireSsl    | SSL antiforgery sistem tarafından gerekip gerekmediğini belirtir. Varsayılan olarak `false`. Varsa `true`, SSL olmayan istekleri başarısız olur. |
|SuppressXFrameOptionsHeader  | Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` üstbilgi. Varsayılan olarak, "SAMEORIGIN" değerine sahip üstbilgi oluşturulur. Varsayılan olarak `false`. |

https://docs.microsoft.com/ASPNET/Core/api/Microsoft.aspnetcore.Builder.cookieauthenticationoptions daha fazla bilgi için bkz.

### <a name="extending-antiforgery"></a>Antiforgery genişletme

[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü her belirteci ek veriler gidiş tarafından anti-XSRF sistem davranışını genişletmek geliştiricilere sağlar. [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) yöntemi her çağrıldığında bir alan belirteci oluşturulur ve dönüş değeri içinde oluşturulan belirteç katıştırılır. Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve ardından arama [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) belirteç doğrulandığında bu verileri doğrulamak için. Bu yüzden bu bilgiyi içer gerek yoktur istemcinin kullanıcı adı zaten oluşturulan belirteçlere katıştırılır. Bir belirteci ek veriler ancak hiçbir içeriyorsa `IAntiForgeryAdditionalDataProvider` bırakıldı yapılandırıldıysa, ek veriler doğrulanmış değil.

## <a name="fundamentals"></a>Temeller

Bu etki alanına yapılan her isteği bir etki alanı ile ilişkili tanımlama bilgileri gönderme varsayılan tarayıcı davranışını CSRF saldırıları kullanır. Bu tanımlama bilgileri tarayıcı içinde depolanır. Kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri sık içerirler. Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler şeklidir. Belirteç tabanlı kimlik doğrulama sistemleriyle popülerliği özellikle SPAs ve diğer "akıllı istemci" senaryoları için büyüyen.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması

Bir kullanıcı, kullanıcı adı ve parolasını kullanarak kimliğini doğrulamasından sonra bunları belirlemek ve bunlar doğrulanan olduğunu doğrulamak için kullanılan bir belirteç alacakları. Her istek istemci eşlik bir tanımlama bilgisi getirir belirteç depolanır. Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından gerçekleştirilir. ASP.NET Core sağlar tanımlama bilgisi [ara yazılımı](xref:fundamentals/middleware/index) , kullanıcı asıl şifrelenmiş bir tanımlama bilgisine serileştirir ve daha sonra sonraki isteklerde tanımlama bilgisini doğrular asıl yeniden oluşturur ve atar `User` özelliği `HttpContext`.

Bir tanımlama bilgisi kullanıldığında, kimlik doğrulama tanımlama bilgisini bir form kimlik doğrulaması bileti için kapsayıcıdır. Raporu her istek ile form kimlik doğrulaması tanımlama bilgisinin değeri olarak geçirilir ve sunucuda, form kimlik doğrulaması tarafından kimliği doğrulanmış bir kullanıcıyı tanımlamak için kullanılır.

Bir kullanıcı bir sistemde oturum açtığında, kullanıcı oturumunu sunucu tarafında oluşturulur ve bir veritabanı veya başka bir kalıcı deposunda saklanır. Sistem, veri deposunda gerçek oturumuna işaret eden bir oturum anahtarı oluşturur ve istemci tarafı tanımlama bilgisi olarak gönderilir. Web sunucusu bu oturum anahtarı bir kullanıcı yetkilendirme gerektiren kaynak istekleri her zaman kontrol eder. Sistem, ilişkili kullanıcı oturumunu istenen kaynağa erişme ayrıcalığına sahip olup olmadığını denetler. Bu durumda, istek devam eder. Aksi takdirde, istek yetkili değil olarak döndürür. Bu yaklaşım durum bilgisi olarak görünen uygulama yapmak için kullanılan tanımlama bilgileri, "unutmayın mümkün" olduğundan kullanıcı daha önce sunucuyla doğrulaması.

### <a name="user-tokens"></a>Kullanıcı belirteçleri

Belirteç tabanlı kimlik doğrulaması oturum sunucuda depolamak değil. Bir kullanıcı oturum açtığında (antiforgery bir belirteç değil) bir belirteç alacakları. Bu belirteç belirteci doğrulamak için gerekli verileri tutar. Ayrıca biçiminde kullanıcı bilgilerini içeren [talep](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Bir kullanıcı kimlik doğrulaması gerektiren bir sunucu kaynağa erişmek istediğinde, belirteç taşıyıcı {belirteci} biçiminde bir ek authorization üstbilgisi sunucusuyla gönderilir. Sonraki her istek için sunucu tarafı doğrulama istekte belirteç geçirilen beri bu uygulamayı durum bilgisiz hale getirir. Bu belirteç değil *şifrelenmiş*; bunun yerine olan *kodlanmış*. Sunucu tarafında belirteç, belirtecin içinde ham bilgilerine erişmek için çözülebilir. Belirteç sonraki istekleri göndermek için ya da onu tarayıcının yerel depolama veya bir tanımlama bilgisi saklayın. XSRF güvenlik açığı hakkında belirteç yerel depolama alanına depolanır, ancak belirteç bir tanımlama bilgisinde depolanıyorsa, bir sorun olduğundan endişelenmeyin.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Bir etki alanında barındırılan birden çok uygulamalarını

Ancak `example1.cloudapp.net` ve `example2.cloudapp.net` farklı ana altında ana bilgisayarlar arasında örtük güven ilişkisi yoktur `*.cloudapp.net` etki alanı. Bu örtük güven ilişkisi, büyük olasılıkla güvenilmeyen ana (AJAX istekleri yöneten kaynak aynı ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli olmayan) birbirlerinin tanımlama bilgileri etkiler olanak tanır. Kullanıcı adı alanı belirtece katıştırılır, ASP.NET çekirdeği çalışma zamanı bazı azaltma sağlar. Kötü amaçlı bir alt etki alanı oturum belirteci üzerine mümkün olsa bile, kullanıcı için geçerli bir alan belirteci üretilemiyor. Bu tür bir ortamda barındırıldığında, yerleşik anti-XSRF yordamlar hala oturumu ele geçirme veya oturum açma CSRF karşı saldırıları korumaya olamaz. Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara vunerable ' dir.

### <a name="additional-resources"></a>Ek kaynaklar

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projeyi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).
