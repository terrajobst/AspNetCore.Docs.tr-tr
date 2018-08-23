---
title: ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme
author: rick-anderson
description: Bilgi nasıl CORS izin verme veya reddetme ASP.NET Core uygulaması çıkış noktaları arası istekleri için standart olarak.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41757312"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme

Tarafından [Mike Wasson](https://github.com/mikewasson), [Shayne boyer'ın](https://twitter.com/spboyer), ve [Tom Dykstra](https://github.com/tdykstra)

Tarayıcı Güvenliği, bir web sayfası, başka bir etki alanına AJAX istekleri yapmasını engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen, web API'nize çıkış noktaları arası isteklerde diğer sitelere izin vermek isteyebilirsiniz.

[Kaynağın kaynak paylaşımını çapraz](http://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart. CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini. CORS güvenli ve önceki teknikler daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP). Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirme gösterilmektedir.

## <a name="what-is-same-origin"></a>"Aynı kaynak" nedir?

Aynı düzeni, konaklar ve bağlantı noktaları varsa iki URL aynı kaynağa sahip. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Bu iki URL aynı kaynağa sahip:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Bu URL'ler önceki değerinden farklı kaynakları iki vardır:

* `http://example.net` -Farklı bir etki alanı

* `http://www.example.com/foo.html` -Farklı bir alt etki alanı

* `https://example.com/foo.html` -Farklı düzeni

* `http://example.com:9000/foo.html` -Farklı bir bağlantı noktası

> [!NOTE]
> Internet Explorer kaynakları karşılaştırılırken, bağlantı noktası dikkate almaz.

## <a name="enable-cors"></a>CORS'yi etkinleştirme

::: moniker range="<= aspnetcore-1.1"

Ayarlamak için uygulamanız için CORS ekleme `Microsoft.AspNetCore.Cors` paketini projenize.

::: moniker-end

Çağrı [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) içinde `Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>CORS ile Ara yazılım etkinleştirme

CORS etkinleştirmek için CORS ara yazılım istek kullanarak işlem hattı ekleyin `UseCors` genişletme yöntemi. CORS ara yazılım tanımlı uç nokta uygulamanızda çıkış noktaları arası istekleri desteklemek istediğiniz gelmelidir (örneğin, önce yapılan tüm çağrıların `UseMvc`).

Çıkış noktaları arası ilke CORS ara yazılımı kullanarak eklerken belirtilebilir [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) sınıfı. Bunu yapmanın iki yolu vardır. İlk çağırmaktır `UseCors` bir lambda ile:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Not:** URL'nin sonunda bir eğik çizgi belirtilmelidir (`/`). URL ile sona ererse `/`, karşılaştırma döndüreceği `false` ve üst bilgi döndürülür.

Lambda alan bir `CorsPolicyBuilder` nesne. Bir listesi bulacaksınız [yapılandırma seçenekleri](#cors-policy-options) bu konuda. Bu örnekte, çıkış noktaları arası istekleri ilkeyi sağlayan `http://example.com` ve diğer kaynak.

Yöntem çağrıları zincirleyebilirsiniz şekilde CorsPolicyBuilder fluent API'sini sahiptir:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

İkinci bir veya daha fazla adlandırılmış CORS ilkelerini tanımlama ve ardından ilkeyi çalışma zamanında adına göre seçmek için bir yaklaşımdır.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Bu örnek, "AllowSpecificOrigin" adlı CORS ilkesi ekler. İlkeyi seçmek için adına geçirmek `UseCors`.

## <a name="enabling-cors-in-mvc"></a>Mvc'de CORS'yi etkinleştirme

Alternatif olarak, MVC her eylem, denetleyici başına veya genel olarak tüm denetleyicileri için belirli CORS uygulamak için de kullanabilirsiniz. MVC, CORS öğesini etkinleştirmek üzere kullanırken aynı CORS Hizmetleri kullanılır ancak CORS ara yazılımı değil.

### <a name="per-action"></a>Eylem başına

Belirtmek için özel bir eylem için CORS ilkesinin ekleyin `[EnableCors]` eyleme özniteliği. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Denetleyici

Belirtmek için belirli bir denetleyicinin CORS ilkesi ekleme `[EnableCors]` özniteliği için denetleyici sınıfı. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Genel olarak

CORS genel olarak tüm denetleyicilerinin ekleyerek etkinleştirebilirsiniz `CorsAuthorizationFilterFactory` genel filtre koleksiyonuna filtre:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Öncelik sırası: eylem, denetleyici, genel. Eylem düzeyinde ilkeler denetleyici düzeyinde ilkeler üzerinde önceliklidir ve denetleyici düzeyinde ilkeler genel ilkelere göre önceliklidir.

### <a name="disable-cors"></a>CORS devre dışı bırak

CORS bir denetleyici veya eylem için devre dışı bırakmak için `[DisableCors]` özniteliği.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS ilkesi seçenekleri

Bu bölümde, CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.

* [İzin verilen çıkış noktaları ayarlama](#set-the-allowed-origins)

* [İzin verilen HTTP yöntemleri Ayarla](#set-the-allowed-http-methods)

* [İzin verilen istek üstbilgilerini Ayarla](#set-the-allowed-request-headers)

* [İfşa edilen yanıt üstbilgilerini Ayarla](#set-the-exposed-response-headers)

* [Çıkış noktaları arası istekleri kimlik bilgileri](#credentials-in-cross-origin-requests)

* [Denetim öncesi sona erme saati ayarla](#set-the-preflight-expiration-time)

Bazı seçenekleri okumak yardımcı olabilecek [CORS nasıl çalıştığını](#how-cors-works) ilk.

### <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktaları ayarlama

Bir veya daha fazla belirli kaynaklara izin veren:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Tüm kaynaklara izin veren:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün. Bu, tam anlamıyla herhangi bir Web API AJAX çağrıları yapabileceğini anlamına gelir.

### <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

Tüm HTTP yöntemleri izin vermek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Bu, uçuş öncesi istekleri ve erişim-denetim-Allow-Methods üstbilgi etkiler.

### <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

CORS denetim öncesi isteğinin HTTP üst bilgileri uygulama tarafından ayarlanıp listeleme, bir Access-Control-Request-Headers üstbilgisi içerebilir (Malum "yazar, istek üst bilgileri").

Beyaz liste belirli üst bilgileri için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

İzin vermek için tüm istek üst bilgilerini yazar:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil. Üst bilgileri için herhangi bir şey dışında ayarlarsanız "*", "kabul", "content-type" ve "Başlangıç" yanı sıra, desteklemek istediğiniz tüm özel üst en az içermelidir.

### <a name="set-the-exposed-response-headers"></a>İfşa edilen yanıt üstbilgilerini Ayarla

Varsayılan olarak, tüm yanıt üstbilgilerini uygulama tarayıcı ortaya çıkarmıyor. (Bkz [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:

* Önbellek denetimi

* İçerik dili

* İçerik Türü

* Süre sonu

* Son değiştirilme

* Pragması

CORS spec bu çağrıları *basit yanıt üstbilgilerini*. Diğer üst bilgileri uygulama kullanılabilir hale getirmek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Çıkış noktaları arası istekleri kimlik bilgileri

Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir. Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez. Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama düzenleri içerir. Kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için istemci XMLHttpRequest.withCredentials true olarak ayarlamanız gerekir.

Kullanarak doğrudan XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery içinde:

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir. Çıkış noktaları arası kimlik bilgilerine izin vermek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Artık HTTP yanıtı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı belirten bir erişim-denetim-Allow-Credentials üst bilgisi içerir.

Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulamaya yanıt kullanıma olmaz ve AJAX isteği başarısız olur.

Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun. Başka bir etki alanındaki bir Web sitesi kullanıcı bilgisi olmadan kullanıcı adına uygulamasında oturum açmış kullanıcının kimlik bilgilerini gönderebilirsiniz. CORS belirtimi Ayrıca bu ayar durumları için kaynakları `"*"` (tüm kaynaklar) geçersiz, `Access-Control-Allow-Credentials` başlığı.

### <a name="set-the-preflight-expiration-time"></a>Denetim öncesi sona erme saati ayarla

Denetim öncesi isteğin yanıtını önbelleğe ne kadar süreyle erişim-denetim-Max-Age üstbilgisini belirtir. Bu üstbilginin ayarlamak için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır. CORS ilkesinin doğru yapılandırılmış ve beklenmeyen davranışlar ortaya çıktığında hata ayıklaması CORS nasıl çalıştığını anlamak önemlidir.

CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar. Özel JavaScript kodu, CORS'yi etkinleştirmek için gerekli değildir.

Çıkış noktaları arası isteğinin bir örneği aşağıda verilmiştir. `Origin` Üst bilgi isteği yapan site etki alanı sağlar:

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

Sunucu isteği izin veriyorsa, yanıtta Access-Control-Allow-Origin üstbilgisini ayarlar. Bu üst bilgi değeri eşleşen kaynak üst bilgisi istek veya joker karakter değeri "*", yani her türlü kaynağa izin verilir:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

Access-Control-Allow-Origin üst bilgi yanıtı içermiyorsa, AJAX isteği başarısız olur. Özellikle, tarayıcının isteği izin vermiyor. Sunucunun başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulama tarafından kullanılabilir yapmaz.

### <a name="preflight-requests"></a>Denetim öncesi isteği

Bazı CORS istekleri için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı ek bir istek gönderir. Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:

* İstek yöntemini GET, HEAD veya sonrası, olduğundan ve

* Uygulama dışındaki içeriği kabul et, Accept-Language, dil, tüm istek üst bilgilerini ayarlayıp ayarlamadığını Content-Type veya son-Event-ID ve

* Content-Type üst bilgisi (varsa ayarlayın) aşağıdakilerden biridir:

  * Application/x-www-form-urlencoded işlemek

  * multipart/form-data

  * metin/düz

İstek üst bilgileri hakkında kural, uygulama XMLHttpRequest nesnesinde setRequestHeader çağırarak ayarlar üst bilgileri için geçerlidir. (Bu "Yazar istek üstbilgilerini" CORS belirtimi çağırır.) Kural, kullanıcı aracısı, konak veya Content-Length gibi tarayıcı ayarlayabilirsiniz üstbilgi için geçerli değildir.

Denetim öncesi isteğinin bir örneği aşağıda verilmiştir:

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır. Bu, iki özel üst bilgileri içerir:

* Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.

* Access-Control-Request-Headers: Uygulama gerçek istek üzerinde ayarlanan istek üst bilgilerini içeren bir liste. (Yeniden, bu tarayıcı ayarlar üst bilgiler dahil değildir.)

Sunucunun isteği izin varsayılarak bir yanıt örneği, şu şekildedir:

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üst bilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeleyen bir Access-Control-izin ver-Headers üstbilgisi içeriyor. Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı daha önce açıklandığı gibi gerçek bir istek gönderir.
