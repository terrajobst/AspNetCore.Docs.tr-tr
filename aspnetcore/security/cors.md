---
title: ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme ASP.NET Core uygulaması çıkış noktaları arası istekleri için standart olarak.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: 655d9be894c677f8adf0fecc2b465d5ae7af2b61
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903053"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makalede, bir ASP.NET Core uygulamada CORS'yi etkinleştirme gösterilmektedir.

Tarayıcı güvenlik, bir web sayfası web sayfada sunulandan daha farklı bir etki alanı istekleri yapmasını engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*. Aynı kaynak İlkesi, kötü niyetli site başka bir siteden hassas verileri okumasını önler. Bazı durumlarda, uygulamanız için diğer sitelere çıkış noktaları arası isteklerde izin vermek isteyebilirsiniz. Daha fazla bilgi için [Mozilla CORS makale](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Çıkış noktaları arası kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):

* Gevşek bir aynı çıkış noktası İlkesi bir sunucu izin veren bir W3C standarttır.
* Olan **değil** CORS bir güvenlik özelliği güvenlik rahatlatır;. API CORS tanıyarak güvenli değil Daha fazla bilgi için [CORS nasıl çalıştığını](#how-cors).
* Açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini sunucuya sağlar.
* Güvenli ve önceki teknikleri, daha esnek olduğu gibi [JSONP](/dotnet/framework/wcf/samples/jsonp).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Aynı kaynak

İki URL aynı düzenleri, konaklar ve bağlantı noktaları varsa aynı kaynağa sahip ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Bu iki URL aynı kaynağa sahip:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Bu URL'ler, önceki iki URL değerinden farklı çıkış noktaları vardır:

* `https://example.net` &ndash; Farklı bir etki alanı
* `https://www.example.com/foo.html` &ndash; Farklı alt etki alanı
* `http://example.com/foo.html` &ndash; Farklı düzeni
* `https://example.com:9000/foo.html` &ndash; Farklı bir bağlantı noktası

Internet Explorer kaynakları karşılaştırılırken, bağlantı noktası dikkate almaz.

## <a name="cors-with-named-policy-and-middleware"></a>CORS adlandırılmış ilke ve ara yazılım

CORS Ara çıkış noktaları arası istekleri işler. Aşağıdaki kod, uygulamanın tamamında ile belirtilen kaynak için CORS sağlar:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Yukarıdaki kod:

* İlke adını ayarlar "\_myAllowSpecificOrigins". İlke adı isteğe bağlıdır.
* Çağrıları <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> CORS sağlayan genişletme yöntemi.
* Çağrıları <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> ile bir [lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda alan bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesne. [Yapılandırma seçenekleri](#cors-policy-options), gibi `WithOrigins`, bu makalenin sonraki bölümlerinde açıklanmıştır.

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Yöntem çağrısının CORS Hizmetleri uygulamanın hizmet kapsayıcıya ekler:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Daha fazla bilgi için [CORS ilkesi seçenekleri](#cpo) bu belgedeki.

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri, zincir:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Aşağıdaki vurgulanmış kodu CORS ara yazılımı üzerinden tüm uygulamaları uç noktalar için CORS ilkelerini uygular:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

Bkz: [Razor sayfaları, denetleyiciler ve eylem yöntemlerine CORS'yi etkinleştirme](#ecors) CORS ilkesini denetleyicisi/sayfa/eylemi düzeyinde uygulamak için.

Not:

* `UseCors` önce çağrılmalıdır `UseMvc`.
* URL gerekir **değil** sonunda eğik çizgi içermelidir (`/`). URL ile sona ererse `/`, karşılaştırma döndürür `false` ve üst bilgi döndürülür.

Bkz: [Test CORS](#test) yönergeler için yukarıdaki kod test etme.

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>Özniteliklerle CORS'yi etkinleştirme

[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) öznitelik CORS genel uygulama için bir alternatif sağlar. `[EnableCors]` Özniteliği, tüm uç noktaları yerine seçili uç noktaları için CORS sağlar.

Kullanım `[EnableCors]` varsayılan ilkesi belirlemek ve `[EnableCors("{Policy String}")]` ilke belirtmek için.

`[EnableCors]` Özniteliği uygulanabilir:

* Razor sayfası `PageModel`
* Denetleyici
* Denetleyici eylem yöntemi

Denetleyici/sayfası-model/eylem ile farklı ilkeler uygulayabilirsiniz `[EnableCors]` özniteliği. Zaman `[EnableCors]` özniteliği denetleyicileri/sayfası-model/eylem yöntemine uygulanan ve CORS Ara yazılımında etkin, her iki ilke uygulanır. İlkeleri birleştirme karşı öneririz. Kullanım `[EnableCors]` özniteliği veya her ikisini birden aynı uygulama ara yazılım.

Aşağıdaki kod, farklı bir ilke her yönteme uygulanır:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Aşağıdaki kod bir CORS varsayılan ilkesi ve adlı bir ilke oluşturur `"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>CORS devre dışı bırak

[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği devre dışı bırakır CORS denetleyicisi/sayfası-model/için eylem.

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS ilkesi seçenekleri

Bu bölümde, CORS ilke ayarlanabilir çeşitli seçenekler açıklanmaktadır:

* [İzin verilen çıkış noktaları ayarlama](#set-the-allowed-origins)
* [İzin verilen HTTP yöntemleri Ayarla](#set-the-allowed-http-methods)
* [İzin verilen istek üstbilgilerini Ayarla](#set-the-allowed-request-headers)
* [İfşa edilen yanıt üstbilgilerini Ayarla](#set-the-exposed-response-headers)
* [Çıkış noktaları arası istekleri kimlik bilgileri](#credentials-in-cross-origin-requests)
* [Denetim öncesi sona erme saati ayarla](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> çağrılma yeri `Startup.ConfigureServices`. Bazı seçenekleri okumak yardımcı olabilecek [CORS nasıl çalıştığını](#how-cors) ilk bölümü.

## <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktaları ayarlama

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; CORS istekleri herhangi şeması ile tüm kaynaklara izin verir (`http` veya `https`). `AllowAnyOrigin` güvenli değil çünkü *herhangi bir Web sitesinde* çıkış noktaları arası istekleri için uygulama yapabilir.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Belirtme `AllowAnyOrigin` ve `AllowCredentials` güvensiz bir yapılandırmadır ve siteler arası istek sahteciliğini neden olabilir. Bir uygulamanın her iki yöntemde de yapılandırıldığında CORS hizmeti geçersiz bir CORS yanıt döndürür.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> Belirtme `AllowAnyOrigin` ve `AllowCredentials` güvensiz bir yapılandırmadır ve siteler arası istek sahteciliğini neden olabilir. İstemci sunucu kaynaklarına erişmek için yetkilendirmeniz gerekiyorsa güvenli bir uygulama için başlangıç noktaları tam bir listesini belirtin.

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` etkiler ön kontrol istekleri ve `Access-Control-Allow-Origin` başlığı. Daha fazla bilgi için [öncesi istekleri](#preflight-requests) bölümü.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Kümeleri <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> yapılandırılmış joker karakter etki alanı kaynağını izin verilip verilmediğini değerlendirirken eşleşecek şekilde kaynakları sağlayan bir işlevi ilkesinin özelliği.

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Herhangi bir HTTP yöntemini sağlar:
* Etkiler ön kontrol istekleri ve `Access-Control-Allow-Methods` başlığı. Daha fazla bilgi için [öncesi istekleri](#preflight-requests) bölümü.

### <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

Adlı bir CORS isteğinde gönderilecek belirli üstbilgilere izin için *yazar, istek üst bilgilerini*, çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ve izin verilen üstbilgileri belirtin:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Bu ayar Denetim öncesi isteği etkiler ve `Access-Control-Request-Headers` başlığı. Daha fazla bilgi için [öncesi istekleri](#preflight-requests) bölümü.

::: moniker range=">= aspnetcore-2.2"

CORS ara yazılımı ilke eşleşmesi için belirli üst bilgileri tarafından belirtilen `WithHeaders` gönderilen üst bilgiler, yalnızca mümkündür `Access-Control-Request-Headers` belirtilen üst bilgilerin tam olarak eşleşmesi `WithHeaders`.

Örneğin, şu şekilde yapılandırılmış bir uygulama göz önünde bulundurun:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ara yazılımı, çünkü bir denetim öncesi isteği şu istek üst bilgisi ile azalma `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) içinde listelenmiyor `WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Uygulama döndürür bir *200 Tamam* yanıt geri CORS üst bilgileri göndermez ancak. Bu nedenle, tarayıcının çıkış noktaları arası istek çalışmaz.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS ara yazılımı, her zaman dört üst bilgilerinde sağlar `Access-Control-Request-Headers` CorsPolicy.Headers içinde yapılandırılan değerlere bakılmaksızın gönderilecek. Üst bilgi bu liste aşağıdakileri içerir:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Örneğin, şu şekilde yapılandırılmış bir uygulama göz önünde bulundurun:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ara yazılımın yanıt başarıyla bir denetim öncesi isteği şu istek üst bilgisi için çünkü `Content-Language` her zaman izin verilenler listesinde olur:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>İfşa edilen yanıt üstbilgilerini Ayarla

Varsayılan olarak, tüm yanıt üstbilgilerini uygulamaya tarayıcı ortaya çıkarmıyor. Daha fazla bilgi için [W3C çıkış noktaları arası kaynak paylaşımı (terminolojisi): Basit bir yanıt üstbilgisi](https://www.w3.org/TR/cors/#simple-response-header).

Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Bu üst CORS belirtimi çağırır *basit yanıt üstbilgilerini*. Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Çıkış noktaları arası istekleri kimlik bilgileri

Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir. Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile kimlik bilgileri göndermez. Tanımlama bilgileri ve kimlik doğrulama düzeni HTTP kimlik bilgileri içerir. İstemci kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için ayarlamalısınız `XMLHttpRequest.withCredentials` için `true`.

Kullanarak `XMLHttpRequest` doğrudan:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery kullanarak:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

Kullanarak [Fetch API'sini](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Sunucu kimlik bilgilerine izin vermeniz gerekir. Çıkış noktaları arası kimlik bilgilerini sağlamak için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP yanıtı içeren bir `Access-Control-Allow-Credentials` sunucu çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler. başlığı.

Tarayıcı bilgilerini gönderir, ancak yanıt geçerli bir içermez `Access-Control-Allow-Credentials` üst bilgi, tarayıcı olmayan uygulama yanıtı kullanımına sunun ve çıkış noktaları arası istek başarısız olur.

Çıkış noktaları arası kimlik bilgilerini sağlayan bir güvenlik riski oluşturur. Başka bir etki alanındaki bir Web sitesi kullanıcı bilgisi olmadan kullanıcı adına uygulamasında oturum açmış kullanıcının kimlik bilgilerini gönderebilirsiniz. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS belirtimi Ayrıca bu ayar durumları için kaynakları `"*"` (tüm kaynaklar) geçersiz, `Access-Control-Allow-Credentials` başlığı.

### <a name="preflight-requests"></a>Denetim öncesi isteği

Bazı CORS istekleri için tarayıcı, gerçek isteği yapmadan önce ek bir istek gönderir. Bu istek adında bir *denetim öncesi isteği*. Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:

* İstek yöntemini GET, HEAD veya POST ' dir.
* Uygulama dışında istek üst bilgilerini ayarlayıp ayarlamadığını `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, veya `Last-Event-ID`.
* `Content-Type` Üst bilgi, ayarla, aşağıdaki değerlerden birine sahiptir:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

İstek üstbilgileri kural kümesi çağırarak uygulama ayarlar üst bilgileri istemci isteği uygulandığı için `setRequestHeader` üzerinde `XMLHttpRequest` nesne. Bu üst CORS belirtimi çağırır *yazar, istek üst bilgilerini*. Tarayıcı ayarlayabilir, gibi üstbilgileri kuralı uygulanmaz `User-Agent`, `Host`, veya `Content-Length`.

Denetim öncesi isteğinin bir örneği verilmiştir:

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır. Bu, iki özel üst bilgileri içerir:

* `Access-Control-Request-Method`: Fiili istek için kullanılacak HTTP yöntemi.
* `Access-Control-Request-Headers`: Uygulamayı gerçek istekte ayarlar istek üst bilgilerini içeren bir liste. Daha önce belirtildiği gibi bu tarayıcı ayarlar, aşağıdaki gibi üst bilgiler içermez `User-Agent`.

CORS denetim öncesi isteği içerebilir bir `Access-Control-Request-Headers` üst bilgi sunucuya gerçek isteğiyle gönderilen üst bilgiler gösterir.

Özel üst bilgiler izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Bunlar nasıl kümesinde tamamen tutarlı olmayan tarayıcıları `Access-Control-Request-Headers`. Üst bilgileri için herhangi bir şey dışında ayarlarsanız `"*"` (veya <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), en az içermelidir `Accept`, `Content-Type`, ve `Origin`, artı, desteklemek istediğiniz tüm özel üst.

(Sunucu isteği izin varsayılarak) denetim öncesi isteği için bir örnek yanıt aşağıdaki gibidir:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Yanıt içeren bir `Access-Control-Allow-Methods` izin verilen yöntemleri listeleyen üst bilgi ve isteğe bağlı olarak bir `Access-Control-Allow-Headers` izin verilen üstbilgileri listeleyen üst bilgisi. Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı gerçek isteği gönderir.

Denetim öncesi isteği reddedilirse, uygulamayı döndürür bir *200 Tamam* yanıt geri CORS üst bilgileri göndermez ancak. Bu nedenle, tarayıcının çıkış noktaları arası istek çalışmaz.

### <a name="set-the-preflight-expiration-time"></a>Denetim öncesi sona erme saati ayarla

`Access-Control-Max-Age` Üstbilgisini belirtir ne kadar süreyle denetim öncesi isteğin yanıtını önbelleğe alınabilir. Bu başlık ayarlamak için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, ne açıklar bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) HTTP iletileri düzeyinde istek.

* CORS olduğu **değil** bir güvenlik özelliğidir. CORS bir sunucunun gevşek bir aynı çıkış noktası İlkesi olanak W3C standart ' dir.
  * Örneğin, kötü amaçlı bir aktör kullanabilirsiniz [önlemek siteler arası betik (XSS)](xref:security/cross-site-scripting) sitenize karşı ve bilgilerini çalmak için siteler arası istek kendi CORS etkin siteye yürütün.
* CORS vererek özelliği API'nizi güvenli değil.
  * İstemci (tarayıcı) CORS zorunlu aittir. Sunucusu isteği yürütür ve yanıtı döndürür, hata ve blokları yanıtı döndürür istemcidir. Örneğin, aşağıdaki araçlardan herhangi birini sunucu yanıtı görüntülenir:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * Adres çubuğuna URL'yi girerek bir web tarayıcısı.
* Bir çıkış noktaları arası yürütmek için bir yol tarayıcılar izin vermek bir sunucu için olan [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API'sini](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) aksi alınamaz istek.
  * Çıkış noktaları arası istekleri (CORS) olmayan tarayıcıları yapamazsınız. CORS önce [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) bu kısıtlama aşmak için kullanıldı. JSONP XHR kullanmaz, kullandığı `<script>` yanıtını almak için etiket. Betikleri yüklenen çıkış noktaları arası izin verilir.

[CORS belirtimi](https://www.w3.org/TR/cors/) çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üstbilgileri sunmuştur. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar. Özel JavaScript kodu, CORS'yi etkinleştirmek için gerekli değildir.

Çıkış noktaları arası isteğinin bir örneği verilmiştir. `Origin` Üst bilgi isteği yapan site etki alanı sağlar:

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Sunucu isteği izin veriyorsa, bu ayarlar `Access-Control-Allow-Origin` yanıt üst bilgisi. Bu üst bilgi değeri ya da eşleşen `Origin` istekteki üstbilgi veya joker karakter değeri `"*"`, yani her türlü kaynağa izin verilir:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Yanıt içermiyorsa `Access-Control-Allow-Origin` üst bilgi çıkış noktaları arası istek başarısız olur. Özellikle, tarayıcının isteği izin vermiyor. Sunucunun başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulamasının kullanımına yapmaz.

<a name="test"></a>

## <a name="test-cors"></a>CORS'yi test etme

CORS test etmek için:

1. [Bir API projesi oluşturma](xref:tutorials/first-web-api). Alternatif olarak, [örneği indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Yaklaşımlardan biri bu belgeyi kullanarak CORS'yi etkinleştirin. Örneğin:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` benzer şekilde bir örnek uygulamayı test etmek için yalnızca kullanılmalıdır [örnek kodu indirdikten](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).

1. (Razor sayfaları veya MVC) bir web uygulaması projesi oluşturun. Razor sayfaları örnek kullanır. API projesi olarak aynı çözüm içindeki web uygulaması oluşturabilirsiniz.
1. Aşağıdaki vurgulanmış kodu ekleyin *Index.cshtml* dosyası:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Önceki kod içinde `url: 'https://<web app>.azurewebsites.net/api/values/1',` dağıtılan uygulamanın URL'siyle.
1. API projesini dağıtın. Örneğin, [Azure'a dağıtma](xref:host-and-deploy/azure-apps/index).
1. Masaüstünden Razor sayfaları veya MVC uygulamayı çalıştırın ve tıklayarak **Test** düğmesi. Hata iletilerini gözden geçirmek için F12 araçları kullanın.
1. Localhost merkezinden kaldırın `WithOrigins` ve uygulamayı dağıtırsınız. Alternatif olarak, farklı bir bağlantı noktası ile istemci uygulaması çalıştırın. Örneğin, Visual Studio'dan çalıştırma.
1. İstemci uygulaması ile test edin. CORS hatalarını hata döndürür, ancak hata iletisi, JavaScript için kullanılamaz. Konsolu sekmesine, hatayı görmek için F12 araçlarındaki kullanın. Tarayıcıya bağlı olarak, bir hata (F12 araçları konsolunu) aşağıdakine benzer da alın:

   * Microsoft Edge kullanarak:

     **SEC7120: [CORS] kaynağını `https://localhost:44375` değil buldunuz `https://localhost:44375` çıkış noktaları arası kaynak için Access-Control-Allow-Origin yanıtı üstbilgisi içinde `https://webapi.azurewebsites.net/api/values/1`**

   * Chrome kullanarak:

     **Erişim sırasında XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` kaynaktan `https://localhost:44375` CORS İlkesi tarafından engellendi: İstenen kaynak üzerinde 'Access-Control-Allow-Origin' üst bilgi yok.**

## <a name="additional-resources"></a>Ek kaynaklar

* [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
