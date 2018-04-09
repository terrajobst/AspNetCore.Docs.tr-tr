---
title: ASP.NET çekirdeği engellemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını
author: steve-smith
description: Web uygulamaları burada kötü amaçlı bir Web sitesi istemci tarayıcısına ve uygulama arasındaki etkileşimi etkileyebilir saldırıları önlemek nasıl bulur.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET çekirdeği engellemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını

Tarafından [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Siteler arası istek sahtekarlığı (olarak da bilinen XSRF veya CSRF, belirgin *bakın surf*) web barındırılan uygulamalar alınabildiği bir kötü amaçlı web uygulaması etkilemek istemci tarayıcısına ve, güvendiği bir web uygulaması arasındaki etkileşimi karşı bir saldırı Tarayıcı. Web tarayıcıları bazı kimlik doğrulama belirteçleri türleri her istek ile otomatik olarak bir Web sitesine göndermek için bu tür saldırıları mümkündür. Bu yararlanma olarak da bilinen biçimidir bir *tek tıklatmayla saldırı* veya *arabası oturum* saldırı yararlandığı için kullanıcı daha önce oturum kimliği doğrulanmış.

CSRF saldırı örneği:

1. İçine bir kullanıcı oturum açtığında `www.good-banking-site.com` forms kimlik doğrulaması kullanma. Sunucu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisini içeren bir yanıt verir. Site için geçerli bir kimlik doğrulama tanımlama bilgisi ile aldığı herhangi bir istek güvendiği için saldırılara karşı savunmasızdır.
1. Kullanıcı kötü amaçlı bir sitesini ziyaret `www.bad-crook-site.com`.

   Kötü amaçlı site `www.bad-crook-site.com`, aşağıdakine benzer bir HTML formuna içerir:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Şunlara dikkat edin formun `action` gönderileri savunmasız siteye değil, zararlı site. CSRF "siteler arası" parçasıdır.

1. Kullanıcı düğmesi seçer. Tarayıcı isteği yapar ve otomatik olarak istenen etki alanı için kimlik doğrulama tanımlama bilgisi içeren `www.good-banking-site.com`.
1. İstek çalıştığı `www.good-banking-site.com` kullanıcının kimlik doğrulaması bağlamı sunucuyla ve kimliği doğrulanmış bir kullanıcı gerçekleştirmek için izin verilen herhangi bir eylem gerçekleştirebilir.

Kullanıcı form gönderme düğmesini seçtiğinde, zararlı site olabilir:

* Otomatik olarak form gönderen bir komut dosyasını çalıştırın.
* Form gönderme AJAX isteği gönderir. 
* Gizli bir form CSS ile kullanın. 

HTTPS kullanarak CSRF saldırı engellemez. Kötü amaçlı site gönderebilirsiniz bir `https://www.good-banking-site.com/` kolayca güvenli olmayan bir istek gönderebilir gibi isteyin.

Bazı saldırılar, eylemi gerçekleştirmek için bir resim etiketi, bu durumda kullanılabilir GET isteklerine yanıt uç noktaları hedefleyin. Bu saldırı, görüntüleri izin ancak JavaScript engelleme Forumu sitelerinde ortak biçimidir. GET isteklerinde burada değişkenleri veya kaynakları değiştirilir, durum değişikliği kötü amaçlı saldırılara açık uygulamalardır. **Durum değişikliği GET istekleri güvenli değil. Hiçbir zaman bir GET isteği durumu değiştirmek en iyi bir uygulamadır.**

CSRF saldırıları, çünkü kimlik doğrulaması için tanımlama bilgileri kullanan web uygulamaları karşı desteklenir:

* Tarayıcıları bir web uygulaması tarafından verilen tanımlama bilgilerini depolar.
* Saklı tanımlama bilgileri, kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri içerir.
* Tanımlama bilgileri web uygulaması için bir etki alanı ile uygulama isteği tarayıcı içinde nasıl oluşturulan bağımsız olarak her istek ilişkili tüm tarayıcılar gönderin.

Ancak, CSRF saldırıları için sınırlı olmayan tanımlama bilgisinden faydalanmakta. Örneğin, temel ve Özet kimlik doğrulaması da savunmasız. Temel veya Özet kimlik doğrulaması ile oturum kullanıcının oturum açtığı sonra tarayıcı kadar oturum kimlik bilgileri otomatik olarak gönderir.&dagger; sona erer.

&dagger;Bu bağlamda *oturum* sırasında kullanıcının kimliğinin istemci-tarafı oturumuna başvuruyor. Bu sunucu tarafı oturumlarını ilgisiz veya [ASP.NET Core oturum Ara](xref:fundamentals/app-state).

Kullanıcılar CSRF güvenlik açıklarına karşı önlem alarak önleyebilirsiniz:

* Dışına kullanmadan bittiğinde, web uygulamalarını imzalayın.
* Düzenli aralıklarla Temizle tarayıcı tanımlama.

Ancak, CSRF güvenlik açıkları temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.

## <a name="authentication-fundamentals"></a>Kimlik doğrulaması temelleri

Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler şeklidir. Belirteç tabanlı kimlik doğrulama sistemleriyle popülerliği içinde özellikle tek sayfa uygulamaları için (SPAs) büyüyor.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması

Bir kullanıcının kullanıcı adı ve parola kullanarak kimlik doğrulaması, kimlik doğrulama ve yetkilendirme için kullanılan kimlik doğrulama biletini içeren bir belirteç alacakları. Her istek istemci eşlik bir tanımlama bilgisi getirir belirteç depolanır. Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından gerçekleştirilir. [Ara yazılım](xref:fundamentals/middleware/index) kullanıcı asıl şifrelenmiş bir tanımlama bilgisine serileştirir. Sonraki isteklerde ara yazılım tanımlama bilgisini doğrular, asıl yeniden oluşturur ve asıl atar [kullanıcı](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) özelliği [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Belirteç tabanlı kimlik doğrulaması

Bir kullanıcının kimliği doğrulandığında, bir belirteç (antiforgery bir belirteç değil) alacakları. Belirteç biçiminde kullanıcı bilgilerini içeren [talep](/dotnet/framework/security/claims-based-identity-model) veya uygulama kullanıcı durumu uygulamada tutulan işaret eden bir başvuru belirteci. Bir kullanıcı kimlik doğrulaması gerektiren bir kaynağa erişmeyi denediğinde, belirteç taşıyıcı belirteci biçiminde bir ek authorization üstbilgisi uygulamayla gönderilir. Bu uygulamayı durum bilgisiz hale getirir. Sonraki her istek belirteci istek için sunucu tarafı doğrulama geçirilir. Bu belirteç değil *şifrelenmiş*; bunun *kodlanmış*. Sunucu üzerinde kendi bilgilerine erişmek için belirteç kodu. Belirtecin sonraki isteklerde göndermek için tarayıcının yerel depolama alanına belirteç depolar. Belirteç tarayıcının yerel depolama alanında depolanıyorsa, CSRF güvenlik açığı hakkında endişelenmeyin. Belirtecin bir tanımlama bilgisinde depolandığında CSRF bir konudur.

### <a name="multiple-apps-hosted-at-one-domain"></a>Bir etki alanında barındırılan birden fazla uygulama

Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara karşı savunmasız.

Ancak `example1.contoso.net` ve `example2.contoso.net` farklı ana altında ana bilgisayarlar arasında örtük güven ilişkisi yoktur `*.contoso.net` etki alanı. Bu örtük güven ilişkisi, büyük olasılıkla güvenilmeyen ana (AJAX istekleri yöneten kaynak aynı ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli olmayan) birbirlerinin tanımlama bilgileri etkiler olanak tanır.

Aynı etki alanında barındırılan uygulamalar arasında güvenilir tanımlama bilgilerini yararlanma saldırıları, etki alanları paylaşmıyor engellenebilir. Her uygulamanın kendi etki alanı üzerinde barındırıldığında yararlanmak için örtük tanımlama bilgisi güven ilişkisi yoktur.

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery yapılandırma

> [!WARNING]
> ASP.NET Core uygulayan antiforgery kullanarak [ASP.NET Core veri koruması](xref:security/data-protection/introduction). Veri koruma yığını bir sunucu grubunda çalışmak için yapılandırılmış olması gerekir. Bkz: [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.

ASP.NET Core 2.0 veya sonraki sürümlerde, [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery belirteçleri HTML form öğelerini yerleştirir. Razor dosyasının aşağıdaki biçimlendirmede otomatik olarak antiforgery belirteçleri oluşturur:

```cshtml
<form method="post">
    ...
</form>
```

Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) formun yöntemi GET değilse varsayılan olarak antiforgery belirteçleri oluşturur.

Otomatik olarak oluşturulmasını antiforgery belirteçleri HTML form öğelerini için olur zaman `<form>` etiketinde `method="post"` özniteliği ve aşağıdakilerden birini geçerliyse:

  * Eylem öznitelik boştur (`action=""`).
  * Eylem öznitelik sağlanan değil (`<form method="post">`).

Otomatik antiforgery belirteçleri oluşturulmasında HTML form öğelerini devre dışı bırakılabilir:

* Açıkça antiforgery belirteçleri ile devre dışı `asp-antiforgery` özniteliği:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form öğesi seçti etiket Yardımcıları etiket Yardımcısını kullanarak genişletme [! çevirin simgesi](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Kaldırma `FormTagHelper` görünümünden. `FormTagHelper` Razor görünümüne aşağıdaki yönergesi ekleyerek bir görünümden kaldırılabilir:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor sayfalarının](xref:mvc/razor-pages/index) XSRF/CSRF otomatik olarak korunur. Daha fazla bilgi için bkz: [XSRF/CSRF ve Razor sayfalarının](xref:mvc/razor-pages/index#xsrf).

CSRF saldırılarına karşı savunma en yaygın yaklaşım kullanmaktır *Eşitleyici belirteci düzeni* (STP). Kullanıcı form verilerini içeren bir sayfa istediğinde STP kullanılır:

1. Sunucu, istemci için geçerli kullanıcının kimliği ile ilişkili bir belirteç gönderir.
1. İstemci belirteci doğrulama için sunucuya geri gönderir.
1. Sunucunun kimliği doğrulanmış kullanıcının kimliğini eşleşmeyen bir belirteç alırsa, isteği reddedilir.

Belirteç, benzersiz ve tahmin edilemez. Belirteç isteklerini bir dizi uygun sıralama emin olmak için de kullanılabilir (örneğin, istek sırası sağlama: sayfa 1 &ndash; sayfa 2 &ndash; sayfa 3). Tüm ASP.NET Core MVC ve Razor sayfalarının şablonları formlarında antiforgery belirteçleri oluşturur. Görünüm örnekleri aşağıdaki çiftinin antiforgery belirteçleri oluşturur:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Açık bir antiforgery belirteci eklemek bir `<form>` etiket Yardımcıları ile HTML Yardımcısı kullanmadan öğesi [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Her önceki durumlarda, ASP.NET Core aşağıdakine benzer bir gizli bir form alanı ekler:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery seçenekleri

Özelleştirme [antiforgery seçenekleri](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) içinde `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Seçenek | Açıklama |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Antiforgery tanımlama bilgisi oluşturmak için kullanılan ayarları belirler. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Tanımlama bilgisinin etki alanı. Varsayılan olarak `null`. Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak. Önerilen Cookie.Domain alternatiftir. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Tanımlama bilgisinin adı. Ayarlanmadı, sistem ile başlayan bir benzersiz ad oluşturursa [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak. Önerilen Cookie.Name alternatiftir. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Yolu tanımlama bilgisinde ayarlanır. Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak. Önerilen Cookie.Path alternatiftir. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Görünümlerde antiforgery belirteçleri oluşturmak için antiforgery sistem tarafından kullanılan gizli form alanının adı. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery sistem tarafından kullanılan üstbilginin adı. Varsa `null`, yalnızca form verilerini sistem göz önünde bulundurur. |
| [requireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | SSL antiforgery sistem tarafından gerekip gerekmediğini belirtir. Varsa `true`, SSL olmayan istekleri başarısız olur. Varsayılan olarak `false`. Bu özellik artık kullanılmıyor ve gelecek sürümde kaldırılacak. Önerilen alternatif Cookie.SecurePolicy ayarlamaktır. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` üstbilgi. Varsayılan olarak, "SAMEORIGIN" değerine sahip üstbilgi oluşturulur. Varsayılan olarak `false`. |

Daha fazla bilgi için bkz: [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>IAntiforgery ile antiforgery özellikleri yapılandırma

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery özelliklerini yapılandırmak için API sağlar. `IAntiforgery` içinde istenen `Configure` yöntemi `Startup` sınıfı. Aşağıdaki örnek, antiforgery bir belirteç oluşturmak ve yanıtta (Bu konuda açıklanan varsayılan Açısal adlandırma kuralını kullanarak) bir tanımlama bilgisi olarak göndermek için uygulamanın giriş sayfasından ara yazılımını kullanır:

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Antiforgery doğrulaması iste

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) tek tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak. İsteğin geçerli bir antiforgery belirteci içermedikçe Bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken` Özniteliği bu süsler, HTTP GET istekleri dahil olmak üzere istekleri eylem yöntemleri için bir belirteç gerektirir. Varsa `ValidateAntiForgeryToken` özniteliği, uygulamanın denetleyicilerinde uygulandığında, ile geçersiz kılınabilir `IgnoreAntiforgeryToken` özniteliği.

> [!NOTE]
> ASP.NET Core antiforgery belirteçleri GET istekleri için otomatik olarak eklemeyi desteklemiyor.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Otomatik olarak yalnızca güvenli olmayan HTTP yöntemleri için antiforgery belirteçleri doğrulamak

ASP.NET Core uygulamaları için Güvenli HTTP yöntemleri (GET, HEAD, seçenekleri ve izleme) antiforgery belirteçleri oluşturmak yok. Kapsamlı uygulama yerine `ValidateAntiForgeryToken` özniteliği ve ile geçersiz kılma `IgnoreAntiforgeryToken` öznitelikleri [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) özniteliği kullanılabilir. Bu öznitelik için aynı şekilde çalışır `ValidateAntiForgeryToken` için aşağıdaki HTTP yöntemleri kullanılarak yapılan istekleri belirteçleri gerektirmeyen dışında öznitelik:

* AL
* HEAD
* SEÇENEKLER
* TRACE

Kullanılmasını öneririz `AutoValidateAntiforgeryToken` API olmayan senaryolar için kapsamlı. Bu işlem sonrası eylemler varsayılan olarak korunan sağlar. Varsayılan olarak, antiforgery belirteçleri yoksaymayı sürece alternatiftir `ValidateAntiForgeryToken` tek tek eylem yöntemlerine uygulanır. Bu büyük olasılıkla bırakılması POST eylem yöntemi için bu senaryoda yanlışlıkla, uygulama CSRF saldırılara karşı savunmasız bırakır korumasız. Tüm gönderileri antiforgery belirteci göndermesi gerekir.

API tanımlama bilgisi olmayan belirtecinin bir parçası göndermek için bir otomatik mekanizması yok. Uygulama, büyük olasılıkla istemci kod uygulamasına bağlıdır. Bazı örnekleri aşağıda verilmiştir:

Sınıf düzeyi örnek:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Genel örnek:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Geçersiz kılma genel veya denetleyicisi antiforgery öznitelikleri

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre, belirli bir eylem (veya denetleyici) için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır. Uygulandığında, bu filtre geçersiz kılmaları `ValidateAntiForgeryToken` ve `AutoValidateAntiforgeryToken` daha yüksek bir düzeyde (genel olarak veya bir denetleyicisinde) belirtilen filtreler.

```csharp
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

## <a name="refresh-tokens-after-authentication"></a>Belirteçlerin kimlik doğrulamasından sonra Yenile

Kullanıcı, kullanıcı bir görünüm veya Razor sayfalarının sayfasına yönlendirerek doğrulandıktan sonra belirteçleri yenilenecek.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX ve SPAs

Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanlarını kullanarak sunucuya geçirilir. Modern JavaScript tabanlı uygulamalar ve SPAs, birçok istek program aracılığıyla yapılır. AJAX istekleri belirteç göndermek için başka teknikler (örneğin, istek üstbilgileri veya tanımlama bilgileri) kullanabilir.

Tanımlama bilgileri kimlik doğrulama belirteçleri depolamak ve API isteklerinin sunucusunda kimlik doğrulaması için kullanılıyorsa, CSRF olası bir sorundur. Belirteç depolamak için yerel depolama alanı kullandıysanız, yerel depolama biriminden değerleri otomatik olarak her istek ile sunucuya gönderilen değil çünkü CSRF güvenlik açığı azaltıldığından. Bu nedenle, istemci ve bir istek üstbilgisini önerilen yaklaşımdır olarak belirtecin gönderme antiforgery belirteci depolamak için yerel depolama kullanma.

### <a name="javascript"></a>JavaScript

JavaScript görünümlerle kullanarak, belirteç görünümü içinde hizmetinden kullanılarak oluşturulabilir. Eklenmeye [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) görüntüleyebileceği ve çağırabileceği içine hizmet [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Bu yaklaşım doğrudan sunucudan tanımlama bilgilerini ayarlama veya istemciden okuma uğraşmanız gereğini ortadan kaldırır.

Önceki örnekte AJAX POST başlığı için gizli alan değeri okumak için JavaScript kullanır.

JavaScript ayrıca tanımlama bilgilerini belirteçleri erişebilir ve tanımlama bilgisinin içeriği belirtecin değeri ile bir üstbilgi oluşturmak için kullanın.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Komut dosyası varsayılarak istekleri belirteç olarak adlandırılan bir üstbilgisinde göndermek için `X-CSRF-TOKEN`, aranacak antiforgery hizmetini yapılandırma `X-CSRF-TOKEN` üstbilgisi:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Aşağıdaki örnek, uygun üstbilgiyle AJAX isteği yapmak için JavaScript kullanır:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS CSRF adresine bir kuralı kullanılır. Sunucu tanımlama bilgisi adı ile gönderirse `XSRF-TOKEN`, AngularJS `$http` hizmeti sunucusuna bir istek gönderdiğinde bu tanımlama bilgisi değeri için bir başlık ekler. Bu işlem otomatik olarak yapılır. Üstbilgi açıkça ayarlanmış olması gerekmez. Üstbilgi adı `X-XSRF-TOKEN`. Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.

ASP.NET Core API çalışmak için bu kural:

* Adlı bir tanımlama bilgisine bir belirteç sağlamak için uygulamanızın yapılandırma `XSRF-TOKEN`.
* Adında bir başlık aramak için antiforgery hizmetini yapılandırma `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Antiforgery genişletme

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü her belirteci ek veriler gidiş tarafından kötü CSRF sistem davranışını genişletmek geliştiricilere sağlar. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) yöntemi her çağrıldığında bir alan belirteci oluşturulur ve dönüş değeri içinde oluşturulan belirteç katıştırılır. Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve ardından arama [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) belirteç doğrulandığında bu verileri doğrulamak için. Bu yüzden bu bilgiyi içer gerek yoktur istemcinin kullanıcı adı zaten oluşturulan belirteçlere katıştırılır. Bir belirteci ek veriler ancak hiçbir içeriyorsa `IAntiForgeryAdditionalDataProvider` olan yapılandırılmış, ek veriler doğrulanmış değil.

## <a name="additional-resources"></a>Ek kaynaklar

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projeyi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).
