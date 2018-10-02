---
title: ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme ASP.NET Core uygulaması çıkış noktaları arası istekleri için standart olarak.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045594"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme

Tarafından [Mike Wasson](https://github.com/mikewasson), [Shayne boyer'ın](https://twitter.com/spboyer), ve [Tom Dykstra](https://github.com/tdykstra)

Tarayıcı güvenlik, bir web sayfası web sayfada sunulandan daha farklı bir etki alanı istekleri yapmasını engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*. Aynı kaynak İlkesi, kötü niyetli site başka bir siteden hassas verileri okumasını önler. Bazı durumlarda, uygulamanız için diğer sitelere çıkış noktaları arası isteklerde izin vermek isteyebilirsiniz.

[Kaynağın kaynak paylaşımını çapraz](https://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart. CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini. CORS güvenli ve önceki teknikleri, daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP). Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirme gösterilmektedir.

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

> [!NOTE]
> Internet Explorer kaynakları karşılaştırılırken, bağlantı noktası dikkate almaz.

## <a name="register-cors-services"></a>CORS Hizmetleri'ne kaydetme

::: moniker range=">= aspnetcore-2.1"

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Paket başvurusu ekleme [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) paket.

::: moniker-end

Çağrı <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> içinde `Startup.ConfigureServices` uygulamanın hizmet kapsayıcıya CORS Hizmetleri eklemek için:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>CORS'yi etkinleştirme

CORS Hizmetleri kaydolduktan sonra aşağıdaki yaklaşımlardan birini CORS ASP.NET Core uygulamanızı etkinleştirmek için kullanın:

* [CORS ara yazılımı](#enable-cors-with-cors-middleware) &ndash; uygulamak CORS ilkelerini genel ara yazılım aracılığıyla.
* [CORS mvc'de](#enable-cors-in-mvc) &ndash; uygulamak CORS ilkelerini eylem veya denetleyici başına. CORS ara yazılımı kullanılmaz.

### <a name="enable-cors-with-cors-middleware"></a>CORS ara yazılımı ile CORS'yi etkinleştirme

CORS ara yazılımı, uygulamaya çıkış noktaları arası istekleri işler. CORS ara yazılım istek işleme ardışık etkinleştirmek için çağrı <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> uzantı yönteminde `Startup.Configure`.

CORS ara yazılımı gelmelidir tanımlanmış tüm uç noktalar uygulamanızda çıkış noktaları arası istekleri desteklemek istediğiniz (örneğin, çağırmadan önce `UseMvc` MVC/Razor sayfaları ara yazılımı için).

A *çıkış noktaları arası ilke* CORS ara yazılımı kullanarak eklerken belirtilebilir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> sınıfı. CORS ilkesini tanımlamak için iki yaklaşım vardır:

* Çağrı `UseCors` bir lambda ile:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Lambda alan bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesne. [Yapılandırma seçenekleri](#cors-policy-options), gibi `WithOrigins`, bu konunun ilerleyen bölümlerinde açıklanmıştır. Önceki örnekte, çıkış noktaları arası istekleri ilkeyi sağlar `https://example.com` ve diğer kaynak.

  URL'nin sonunda bir eğik çizgi belirtilmelidir (`/`). URL ile sona ererse `/`, karşılaştırma döndürür `false` ve üst bilgi döndürülür.

  `CorsPolicyBuilder` Yöntem çağrıları zincirleyebilirsiniz şekilde fluent API'sini sahiptir:

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* Bir veya daha fazla adlandırılmış CORS ilkelerini tanımlama ve çalışma zamanında adıyla ilkeyi seçin. Aşağıdaki örnekte adlı bir kullanıcı tanımlı CORS ilkesinin ekler *AllowSpecificOrigin*. İlkeyi seçmek için adına geçirmek `UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>Mvc'de CORS'yi etkinleştirme

Alternatif olarak, MVC eylem veya denetleyici başına belirli CORS ilkelerini uygulamak için de kullanabilirsiniz. MVC, CORS'yi etkinleştirmek için kullanılırken kaydedilen CORS Hizmetleri kullanılır. CORS ara yazılımı kullanılmaz.

### <a name="per-action"></a>Eylem başına

Belirli bir eylem için CORS ilkesinin belirtmek için [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) eyleme özniteliği. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>Denetleyici

CORS ilkesini belirli bir denetleyicinin belirtmek için [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) özniteliği için denetleyici sınıfı. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

Öncelik sırası şöyledir:

1. Eylem
1. denetleyici

### <a name="disable-cors"></a>CORS devre dışı bırak

CORS bir denetleyici veya eylem için devre dışı bırakmak için [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) özniteliği:

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>CORS ilkesi seçenekleri

Bu bölümde, CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.

* [İzin verilen çıkış noktaları ayarlama](#set-the-allowed-origins)
* [İzin verilen HTTP yöntemleri Ayarla](#set-the-allowed-http-methods)
* [İzin verilen istek üstbilgilerini Ayarla](#set-the-allowed-request-headers)
* [İfşa edilen yanıt üstbilgilerini Ayarla](#set-the-exposed-response-headers)
* [Çıkış noktaları arası istekleri kimlik bilgileri](#credentials-in-cross-origin-requests)
* [Denetim öncesi sona erme saati ayarla](#set-the-preflight-expiration-time)

Bazı seçenekleri okumak yardımcı olabilecek [CORS nasıl çalıştığını](#how-cors-works) ilk bölümü.

### <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktaları ayarlama

Bir veya daha fazla belirli kaynaklara izin veren çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

Tüm kaynaklara izin veren çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün. Her türlü kaynağa gelen isteklere izin vererek anlamına *herhangi bir Web sitesinde* çıkış noktaları arası istekleri uygulamanıza yapabilirsiniz.

Bu ayar etkiler [istekleri ve Access-Control-Allow-Origin üst bilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).

### <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

Tüm HTTP yöntemleri izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

Bu ayar etkiler [istekleri ve erişim-denetim-Allow-Methods üst bilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).

### <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

Adlı bir CORS isteğinde gönderilecek belirli üstbilgilere izin için *yazar, istek üst bilgilerini*, çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ve izin verilen üstbilgileri belirtin:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

Bu ayar etkiler [istekleri ve Access-Control-Request-Headers üstbilgisi ön kontrol](#preflight-requests) (Bu konunun ilerleyen bölümlerinde açıklanmıştır).

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

Varsayılan olarak, tüm yanıt üstbilgilerini uygulamaya tarayıcı ortaya çıkarmıyor. Daha fazla bilgi için [W3C çıkış noktaları arası kaynak paylaşımı (terminolojisi): basit yanıt üst bilgisi](https://www.w3.org/TR/cors/#simple-response-header).

Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Bu üst CORS belirtimi çağırır *basit yanıt üstbilgilerini*. Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Çıkış noktaları arası istekleri kimlik bilgileri

Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir. Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile kimlik bilgileri göndermez. Tanımlama bilgileri ve kimlik doğrulama düzeni HTTP kimlik bilgileri içerir. İstemci kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için ayarlamalısınız `XMLHttpRequest.withCredentials` için `true`.

Kullanarak `XMLHttpRequest` doğrudan:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery içinde:

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir. Çıkış noktaları arası kimlik bilgilerini sağlamak için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

HTTP yanıtı içeren bir `Access-Control-Allow-Credentials` sunucu çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler. başlığı.

Tarayıcı bilgilerini gönderir, ancak yanıt geçerli bir içermez `Access-Control-Allow-Credentials` üst bilgi, tarayıcı olmayan uygulama yanıtı kullanımına sunun ve çıkış noktaları arası istek başarısız olur.

Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun. Başka bir etki alanındaki bir Web sitesi kullanıcı bilgisi olmadan kullanıcı adına uygulamasında oturum açmış kullanıcının kimlik bilgilerini gönderebilirsiniz.

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
* `Access-Control-Request-Headers`: Uygulamayı gerçek istekte ayarlar istek üst bilgilerini bir listesi. Daha önce belirtildiği gibi bu tarayıcı ayarlar, aşağıdaki gibi üst bilgiler içermez `User-Agent`.

CORS denetim öncesi isteği içerebilir bir `Access-Control-Request-Headers` üst bilgi sunucuya gerçek isteğiyle gönderilen üst bilgiler gösterir.

Özel üst bilgiler izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

Tüm istek üstbilgilerini Yazar izin vermek için çağrı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

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

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır. CORS ilkesinin doğru yapılandırılmış ve beklenmeyen davranışlar ortaya çıktığında hata ayıklaması CORS nasıl çalıştığını anlamak önemlidir.

CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar. Özel JavaScript kodu, CORS'yi etkinleştirmek için gerekli değildir.

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

## <a name="additional-resources"></a>Ek kaynaklar

* [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
