---
title: ASP.NET core'da önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını
author: steve-smith
description: Web uygulamaları kötü amaçlı bir Web sitesi istemci tarayıcısına ve uygulama arasındaki etkileşim burada etkileyebilir saldırıları önlemek nasıl keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 88a2d127407378b9e83df7f48b1938ed081f9bb2
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208534"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>ASP.NET core'da önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını

Tarafından [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Siteler arası istek sahteciliği (XSRF veya CSRF, olarak da telaffuz *bakın surf*) barındırılan web uygulamaları ile bir kötü amaçlı web uygulaması etkilemek istemci tarayıcısı, güvenen bir web uygulaması arasındaki etkileşimi karşı bir saldırı olma Tarayıcı. Web tarayıcıları, bir Web sitesine kimlik doğrulama belirteçlerinizi bazı türleri her istek ile otomatik olarak göndermek için bu saldırıları mümkündür. Olarak da bilinen bu yararlanma biçiminde olan bir *tek tıklamayla saldırı* veya *arabası oturumu* saldırı yararlanır çünkü kullanıcı daha önce oturum kimliği doğrulanmış.

CSRF saldırı örneği:

1. İçinde bir kullanıcı oturum açtığında `www.good-banking-site.com` forms kimlik doğrulaması kullanma. Sunucusu kullanıcının kimliğini doğrular ve bir kimlik doğrulama tanımlama bilgisi içeren yanıt verir. Site bir geçerli bir kimlik doğrulama tanımlama bilgisi ile aldığı herhangi bir istek güvendiği için saldırılara karşı savunmasız durumdadır.
1. Kullanıcı bir kötü amaçlı sitesini ziyaret `www.bad-crook-site.com`.

   Kötü niyetli site `www.bad-crook-site.com`, aşağıdakine benzer bir HTML formuna içerir:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Dikkat formun `action` gönderileri savunmasız sitesine, kötü niyetli site uygulanmaz. CSRF "siteler arası" parçasıdır.

1. Kullanıcının Gönder düğmesini seçer. Tarayıcı istekte bulunur ve otomatik olarak istenen etki alanı için kimlik doğrulama tanımlama bilgisi ekler `www.good-banking-site.com`.
1. İstek üzerinde çalıştığı `www.good-banking-site.com` kullanıcının kimlik doğrulaması bağlamı sunucusuyla ve kimliği doğrulanmış bir kullanıcıyı gerçekleştirmek için izin verilen herhangi bir eylem gerçekleştirebilirsiniz.

Kullanıcının seçebileceği formunun düğme senaryo ek olarak, kötü niyetli site yüklenemedi:

* Otomatik olarak formu gönderdiği bir betiği çalıştırın.
* Form gönderme bir AJAX isteği gönderin.
* CSS kullanarak form gizleyin.

Bu alternatif senaryoların herhangi bir eylem veya kötü niyetli site ilk kez ziyaret dışındaki kullanıcı girişi gerekmez.

HTTPS kullanarak CSRF saldırı engellemez. Kötü niyetli site göndermek için bir `https://www.good-banking-site.com/` kolayca güvenli olmayan bir istek gönderebilir gibi istek.

Bazı saldırıları, bu durumda bir resim etiketi eylemi gerçekleştirmek için kullanılabilir GET isteklerine yanıt veren uç noktaları hedefleyin. Bu tür bir saldırı görüntüleri izin verir, ancak JavaScript block forum siteleri yaygındır. GET isteklerinde değişkenleri veya kaynaklar nerede değiştirilir, durum değişikliği uygulamaları kötü amaçlı saldırılara karşı savunmasızdır. **Durum değişikliği GET istekleri güvenli değil. Hiçbir zaman bir GET isteği durumu değiştirmek iyi bir uygulamadır.**

CSRF saldırılarına karşı olduğundan kimlik doğrulaması için tanımlama bilgilerini kullanan web uygulamaları desteklenir:

* Tarayıcılar, bir web uygulaması tarafından verilen tanımlama bilgilerini depolar.
* Depolanan bir tanımlama bilgisi kimliği doğrulanmış kullanıcılar için oturum tanımlama bilgileri içerir.
* Tarayıcılar, her isteği istek uygulamasına bir tarayıcıdan nasıl oluşturulduğunu bağımsız olarak tüm tanımlama bilgilerini ilişkili bir etki alanını web uygulaması ile gönderin.

Ancak, CSRF saldırıları için sınırlı olmayan tanımlama bilgilerini kötüye. Örneğin, temel ve Özet kimlik doğrulaması ayrıca savunmasız. Bir kullanıcı, temel veya Özet kimlik doğrulaması ile oturum açtıktan sonra tarayıcı kadar oturum kimlik bilgileri otomatik olarak gönderir.&dagger; sona erer.

&dagger;Bu bağlamda *oturumu* istemci-tarafı oturumu sırasında kullanıcı kimliğinin ifade eder. Bu sunucu tarafı oturum ilgisi olmayan veya [ASP.NET Core oturumu ara yazılım](xref:fundamentals/app-state).

Kullanıcılar, CSRF güvenlik açıklarına karşı önlemler alarak önleyebilirsiniz:

* Bunları kullanarak bittiğinde, web uygulamaları dışına oturum açın.
* Düzenli aralıklarla açık tarayıcı tanımlama.

Ancak, CSRF güvenlik açıkları, temelde web uygulaması, son kullanıcı ile ilgili bir sorun değildir.

## <a name="authentication-fundamentals"></a>Kimlik doğrulaması temelleri

Tanımlama bilgisi tabanlı kimlik doğrulaması, kimlik doğrulama popüler bir biçimidir. Belirteç tabanlı kimlik doğrulama sistemleri popülerliği, özellikle tek sayfalık uygulamalar için (Spa'lar) büyüyor.

### <a name="cookie-based-authentication"></a>Tanımlama bilgisi tabanlı kimlik doğrulaması

Bir kullanıcı, kullanıcı adı ve parolasını kullanarak kimliğini doğrular, kimlik doğrulama ve yetkilendirme için kullanılan bir kimlik doğrulama anahtarı içeren bir belirteç, verilen. Belirteç, her istemci isteğe eşlik eden bir tanımlama bilgisi şekilde depolanır. Oluşturma ve bu tanımlama bilgisi doğrulama tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından gerçekleştirilir. [Ara yazılım](xref:fundamentals/middleware/index) şifrelenmiş bir tanımlama bilgisine kullanıcı asıl serileştirir. Sonraki istekler, ara yazılım tanımlama bilgisini doğrular, asıl yeniden oluşturur ve sorumlusuna atar [kullanıcı](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) özelliği [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Belirteç tabanlı kimlik doğrulaması

Bir kullanıcının kimliği doğrulandığında, bir belirteç (antiforgery bir belirteç değil) verilen. Kullanıcı bilgileri'biçimindeki belirteç içeren [talep](/dotnet/framework/security/claims-based-identity-model) veya uygulama uygulamayı korunan kullanıcı durumunu gösteren bir başvuru belirteci. Bir kullanıcı kimlik doğrulaması gerektiren bir kaynağa erişmeye çalıştığında, belirteç bir ek yetkilendirme üst bilgisinde taşıyıcı belirteç biçimi ile uygulamaya gönderilir. Bu uygulama durum bilgisiz hale getirir. Sonraki her istek için sunucu tarafı doğrulama isteği belirteci geçirildi. Bu belirteci değil *şifrelenmiş*; sahip *kodlanmış*. Sunucuda, belirteç bilgilerini erişmek için kodu. Belirtecin sonraki istekleri göndermek için belirteç tarayıcının yerel depolama alanında depolayın. Belirteç tarayıcının yerel depolama alanında saklanıyorsa CSRF güvenlik açığı hakkında endişelenmeyin. Belirteç bir tanımlama bilgisinde depolandığında CSRF bir konudur.

### <a name="multiple-apps-hosted-at-one-domain"></a>Bir etki alanında barındırılan birden fazla uygulama

Paylaşılan barındırma ortamları oturumu ele geçirme, oturum açma CSRF ve diğer saldırılara karşı savunmasız.

Ancak `example1.contoso.net` ve `example2.contoso.net` farklı konaklar altında konaklar arasında örtük güven ilişkisi yoktur `*.contoso.net` etki alanı. Bu örtük güven ilişkisi (AJAX isteklerini yöneten bir aynı çıkış noktası ilkeleri mutlaka HTTP tanımlama bilgileri için geçerli değildir) birbirlerinin tanımlama etkilemek potansiyel olarak güvenilmeyen konakları sağlar.

Aynı etki alanında barındırılan uygulamalar arasında güvenilir tanımlama bilgilerini açığından yararlanma saldırıları, etki alanları kullanılmadığı engellenebilir. Her uygulama kendi etki alanı üzerinde barındırıldığında yararlanmak için örtük bir tanımlama bilgisi güven ilişkisi yoktur.

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery yapılandırma

> [!WARNING]
> ASP.NET Core uygulayan antiforgery kullanarak [ASP.NET Core veri koruma](xref:security/data-protection/introduction). Veri koruma yığınından bir sunucu grubunda çalışacak şekilde yapılandırılması gerekir. Bkz: [veri korumayı yapılandırma](xref:security/data-protection/configuration/overview) daha fazla bilgi için.

ASP.NET Core 2.0 veya sonraki sürümlerde, [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery belirteçleri HTML form öğelerini yerleştirir. Bir Razor dosyasında aşağıdaki biçimlendirme antiforgery belirteçleri otomatik olarak oluşturur:

```cshtml
<form method="post">
    ...
</form>
```

Benzer şekilde, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) formun yöntemi GET değilse, varsayılan olarak antiforgery belirteçleri oluşturur.

Otomatik olarak oluşturulmasını antiforgery belirteçleri için bir HTML form öğelerini olur olduğunda `<form>` etiketinde `method="post"` özniteliği ve aşağıdakilerden birini geçerliyse:

* Eylem özniteliktir boş (`action=""`).
* Eylem öznitelik verilmiyorsa (`<form method="post">`).

HTML form öğelerini için antiforgery belirteçleri otomatik olarak oluşturulmasını devre dışı bırakılabilir:

* Antiforgery belirteçleri ile açıkça devre dışı `asp-antiforgery` özniteliği:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form öğesi kabul etiket Yardımcıları etiket Yardımcısı kullanarak genişletme [! çevirme sembol](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Kaldırma `FormTagHelper` görünümü. `FormTagHelper` Razor görünüme aşağıdaki yönerge ekleyerek bir görünümden kaldırılabilir:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor sayfaları](xref:razor-pages/index) XSRF/CSRF otomatik olarak korunur. Daha fazla bilgi için [XSRF/CSRF ve Razor sayfaları](xref:razor-pages/index#xsrf).

CSRF saldırılarına karşı savunma için en yaygın yaklaşımı *Eşitleyici belirteci deseni* (STP). Kullanıcı form verilerini içeren bir sayfa istediğinde STP kullanılır:

1. Sunucu, istemci için geçerli kullanıcının kimliğiyle ilişkili bir belirteç gönderir.
1. İstemci belirteci doğrulama sunucusuna geri gönderir.
1. Sunucu kimliği doğrulanmış kullanıcının kimliğine eşleşmeyen bir belirteç alırsa, istek reddedilir.

Belirteç benzersiz ve tahmin edilemez. Belirteç, bir dizi isteğin doğru sıralama emin olmak için de kullanılabilir (örneğin, istek sırası sağlama: sayfa 1 &ndash; sayfa 2 &ndash; sayfa 3). Tüm ASP.NET Core MVC ve Razor sayfaları şablonları formlarında antiforgery belirteçleri oluşturur. Örnekleri görüntüle çiftiyle antiforgery belirteçleri oluşturun:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Açıkça bir antiforgery belirtecine ekleme bir `<form>` etiket Yardımcıları ile HTML Yardımcısını kullanmadan öğesi [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

Her önceki örneklerin, ASP.NET Core aşağıdakine benzer bir gizli form alanının ekler:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core içeren üç [filtreleri](xref:mvc/controllers/filters) antiforgery belirteçleri ile çalışmak için:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery seçenekleri

Özelleştirme [antiforgery seçenekleri](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) içinde `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;Antiforgery ayarlamak `Cookie` özelliklerini kullanarak özelliklerini [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) sınıfı.

| Seçenek | Açıklama |
| ------ | ----------- |
| [Tanımlama bilgisi](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Antiforgery tanımlama bilgisi oluşturmak için kullanılan ayarları belirler. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Antiforgery sistem tarafından görünümlerde antiforgery belirteçleri işlemek için kullanılan gizli form alanının adı. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery sistemi tarafından kullanılan üstbilginin adı. Varsa `null`, sistem yalnızca form verileri olarak değerlendirir. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` başlığı. Varsayılan olarak, başlığı "SAMEORIGIN" bir değerle oluşturulur. Varsayılan olarak `false`. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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
| [Tanımlama bilgisi](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Antiforgery tanımlama bilgisi oluşturmak için kullanılan ayarları belirler. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Tanımlama bilgisinin etki alanı. Varsayılan olarak `null`. Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak. Önerilen Cookie.Domain alternatiftir. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Tanımlama bilgisinin adı. Ayarlı değil, system ile başlayan bir benzersiz ad oluşturursa [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak. Önerilen Cookie.Name alternatiftir. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Yolu tanımlama bilgisinde ayarlayın. Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak. Önerilen Cookie.Path alternatiftir. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Antiforgery sistem tarafından görünümlerde antiforgery belirteçleri işlemek için kullanılan gizli form alanının adı. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Antiforgery sistemi tarafından kullanılan üstbilginin adı. Varsa `null`, sistem yalnızca form verileri olarak değerlendirir. |
| [İçindeki requireSSL öğesini](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | HTTPS antiforgery sistem tarafından gerekli olup olmadığını belirtir. Varsa `true`, HTTPS olmayan istekleri başarısız olur. Varsayılan olarak `false`. Bu özellik artık kullanılmıyor ve gelecekte yayımlanacak bir sürümde kaldırılacak. Önerilen alternatif Cookie.SecurePolicy ayarlamaktır. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Nesil engellenip engellenmeyeceğini belirtir `X-Frame-Options` başlığı. Varsayılan olarak, başlığı "SAMEORIGIN" bir değerle oluşturulur. Varsayılan olarak `false`. |

::: moniker-end

Daha fazla bilgi için [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>İle IAntiforgery antiforgery özellikleri yapılandırma

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) antiforgery özellikleri yapılandırmak için API sağlar. `IAntiforgery` içinde istenen `Configure` yöntemi `Startup` sınıfı. Aşağıdaki örnek uygulamanın giriş sayfasından ara yazılım antiforgery bir belirteç oluşturun ve yanıt olarak bir tanımlama bilgisi (Bu konunun ilerleyen bölümlerinde açıklanan varsayılan Angular adlandırma kuralını kullanarak) olarak göndermek için kullanır:

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

### <a name="require-antiforgery-validation"></a>Antiforgery doğrulama gerektir

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) tek bir eylem, bir denetleyici uygulanabilir bir eylem filtresi veya genel olarak. Geçerli bir antiforgery belirteç isteği eklemediği sürece bu filtre uygulanmış olan eylemler için yapılan istekleri engellenir.

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

`ValidateAntiForgeryToken` Özniteliği, bu düzenler, HTTP GET istekleri dahil olmak üzere istekleri eylem yöntemleri için bir belirteç gerektirir. Varsa `ValidateAntiForgeryToken` özniteliği, uygulamanın denetleyicilerinde uygulandığında, ile kılınabilir `IgnoreAntiforgeryToken` özniteliği.

> [!NOTE]
> ASP.NET Core antiforgery belirtecinin GET istekleri için otomatik olarak eklenmesini desteklemez.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Yalnızca güvenli olmayan HTTP yöntemleri için antiforgery belirteçleri otomatik olarak doğrulama

ASP.NET Core uygulamaları için Güvenli HTTP yöntemleri (GET, HEAD, SEÇENEKLERİNİ ve izleme) antiforgery belirteçleri oluşturma. Geniş çapta uygulamak yerine `ValidateAntiForgeryToken` özniteliği ve ile geçersiz kılma `IgnoreAntiforgeryToken` öznitelikleri [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) özniteliği kullanılabilir. Bu öznitelik için aynı şekilde çalışır `ValidateAntiForgeryToken` aşağıdaki HTTP yöntemleri kullanılarak yapılan istekler için belirteçleri gerektirmeyen dışında öznitelik:

* GET
* HEAD
* SEÇENEKLER
* TRACE

Kullanılmasını öneririz `AutoValidateAntiforgeryToken` problem API olmayan senaryolar için. Bu işlem sonrası eylemler varsayılan olarak korumalıdır sağlar. Sürece varsayılan olarak, antiforgery belirteçleri yoksaymak için alternatif olan `ValidateAntiForgeryToken` tek tek eylem yöntemlerine uygulanır. Bu büyük olasılıkla kalma sonrası eylem yöntemi için bu senaryoda yanlışlıkla uygulama CSRF saldırılarına karşı savunmasız bırakır korumasız. Tüm gönderileri antiforgery belirteci göndermesi gerekir.

API'leri tanımlama bilgisi olmayan belirtecinin bir parçası olarak göndermek için otomatik bir mekanizma yoktur. Uygulama, büyük olasılıkla istemci kodu uygulamasının bağlıdır. Bazı örnekler aşağıda verilmiştir:

Örnek sınıf düzeyi:

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>Geçersiz kılma genel veya denetleyici antiforgery öznitelikleri

[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtre, belirli bir eylem (veya denetleyici) için bir antiforgery belirteci gereksinimini ortadan kaldırmak için kullanılır. Uygulandığında, bu filtresi geçersiz kılmaları `ValidateAntiForgeryToken` ve `AutoValidateAntiforgeryToken` (genel olarak veya bir denetleyicisinde) daha yüksek bir düzeyde belirtilen filtreler.

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

Kullanıcı, kullanıcı bir görünüm veya Razor sayfaları sayfasına yönlendirerek doğrulandıktan sonra belirteçleri yenilenmesi.

## <a name="javascript-ajax-and-spas"></a>JavaScript ve AJAX Spa'lar

Geleneksel HTML tabanlı uygulamalarda antiforgery belirteçleri gizli form alanları kullanarak sunucuya geçirilir. JavaScript tabanlı modern uygulamalar ve Spa'lar, çok sayıda istek program aracılığıyla yapılır. Bu AJAX istekleri (veya istek üst bilgilerini tanımlama bilgileri gibi) başka teknikler belirteç göndermek için kullanabilir.

Tanımlama bilgileri kimlik doğrulama belirteçlerinizi depolamak ve sunucuda API isteklerinin kimliğini doğrulamak için kullanılıyorsa, CSRF olası bir sorundur. Yerel depolama belirteç depolamak için kullanılır, çünkü her istek ile sunucu için yerel depolama alanından değerleri otomatik olarak gönderilmez CSRF güvenlik açığı azaltılması gereken. Bu nedenle, istemci ve istek üst bilgisi önerilen bir yaklaşım olarak belirteç gönderme antiforgery belirteç saklamak için yerel depolama ile.

### <a name="javascript"></a>JavaScript

Görünümler ile JavaScript kullanarak belirteç hizmeti aracılığıyla görünümü içinde oluşturulabilir. Ekleme [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) görüntüleyebileceği ve çağırabileceği uygulamasına service [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Bu yaklaşım, doğrudan sunucudan tanımlama bilgilerini ayarlama ya da istemciden okumadan altyapıdaki gereğini ortadan kaldırır.

Yukarıdaki örnekte AJAX posta üst bilgisi için gizli bir alan değerini okumak için JavaScript kullanır.

JavaScript, ayrıca tanımlama bilgilerini belirteçlerinde erişebilir ve tanımlama bilgisinin içeriği belirtecin değeri ile bir başlığı oluşturmak için kullanın.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Betik varsayılarak istekleri belirteç adı verilen bir üst bilgisinde göndermek için `X-CSRF-TOKEN`, aranacak antiforgery hizmetini yapılandırma `X-CSRF-TOKEN` üst bilgi:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Aşağıdaki örnek, uygun üst bilgisiyle bir AJAX isteği yapmak için JavaScript kullanır:

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

AngularJS adresine CSRF kuralını kullanır. Sunucu adı ile bir tanımlama bilgisi gönderirse `XSRF-TOKEN`, AngularJS `$http` hizmeti sunucusuna bir istek gönderdiğinde bu tanımlama bilgisi değeri için bir başlık ekler. Bu işlemi otomatik olarak gerçekleşir. Üst bilgi istemci açıkça ayarlanmış olması gerekmez. Üst bilgi adı: `X-XSRF-TOKEN`. Sunucu, bu başlığı algılamak ve içeriğini doğrulayın.

ASP.NET Core uygulaması startup'ınız bu kurala çalışmak API için:

* Bir belirteç olarak adlandırılan bir tanımlama bilgisinde sağlamak için uygulamanızı yapılandırma `XSRF-TOKEN`.
* Adlı bir üst bilgisini antiforgery hizmetinin yapılandırma `X-XSRF-TOKEN`.

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Antiforgery genişletme

[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) türü tarafından gidiş dönüşü ek veriler her belirteç, CSRF önleme sisteminin davranışını genişletmek geliştiricilerin kullanmasına izin verir. [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) yöntemi her çağrıldığında bir alan belirteç oluşturulur ve dönüş değeri içinde oluşturulan belirtecin katıştırılır. Bir uygulayan bir zaman damgası, nonce veya başka bir değer döndürür ve sonra çağrı [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) belirteç doğrulandığında, bu verileri doğrulamak için. Bu bilgiyi içer gerekmez. Bu nedenle istemcinin kullanıcı adını oluşturulan belirteçlere zaten eklenir. Bir belirteç yok, ancak ek veriler içeriyorsa `IAntiForgeryAdditionalDataProvider` olan yapılandırılmış, ek veriler doğrulanmış değil.

## <a name="additional-resources"></a>Ek kaynaklar

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) üzerinde [Web uygulaması güvenlik projesi açın](https://www.owasp.org/index.php/Main_Page) (OWASP).
* <xref:host-and-deploy/web-farm>
