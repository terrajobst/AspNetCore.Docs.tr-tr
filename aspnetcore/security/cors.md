---
title: "Cross-Origin istekleri (CORS) etkinleştirme"
author: rick-anderson
description: "Bu belge CORS izin verme veya reddetme bir ASP.NET Core uygulamasında cross-origin istekleri için bir standart olarak tanıtır."
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: ee61798fc1bde89ca3712eae9b7c4413e58cf70d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>Cross-Origin istekleri (CORS) etkinleştirme

Tarafından [CAN Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), ve [zel Dykstra](https://github.com/tdykstra)

Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller. Bu kısıtlama adlı *kaynak aynı ilke*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen web API'nize çıkış noktaları arası isteklerde diğer sitelere izin vermek isteyebilirsiniz.

[Kaynak kaynak paylaşma arası](http://www.w3.org/TR/cors/) (CORS) olan W3C standart bir kaynak aynı ilke hafifletin sunucusunun sağlar. CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz. CORS daha güvenli ve önceki teknikleri daha esnek gibi [JSONP](https://wikipedia.org/wiki/JSONP). Bu konuda, bir ASP.NET Core uygulamada CORS'yi etkinleştirmeniz gösterilmektedir.

## <a name="what-is-same-origin"></a>"Aynı kaynak" nedir?

Aynı düzenleri, konakları ve bağlantı noktaları varsa iki URL'leri aynı kaynağa sahip. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Bu iki URL'leri aynı kaynağa sahip:

* `http://example.com/foo.html`

* `http://example.com/bar.html`

Bu URL'leri iki farklı çıkış önceki daha vardır:

* `http://example.net` -Farklı bir etki alanı

* `http://www.example.com/foo.html` -Farklı bir alt etki alanı

* `https://example.com/foo.html` -Farklı düzeni

* `http://example.com:9000/foo.html` -Farklı bir bağlantı noktası

> [!NOTE]
> Internet Explorer bağlantı noktası kaynakları karşılaştırma göz önünde değil.

## <a name="setting-up-cors"></a>CORS ayarlama

Ayarlamak için uygulamanız için CORS eklemek `Microsoft.AspNetCore.Cors` projenize paket.

CORS Hizmetleri içinde haline ekleyin:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>CORS Ara yazılımla etkinleştirme

Etkinleştirmek için tüm uygulamanız için CORS CORS ara yazılım, istek ardışık düzen kullanarak eklemeniz `UseCors` genişletme yöntemi. Not CORS Ara (örn. cross-origin istekleri desteklemek istediğiniz uygulamanızda tanımlanmış uç nokta gelmelidir Tüm çağrıdan önce `UseMvc`).

CORS ara yazılımı kullanarak eklerken bir çıkış noktaları arası ilkesi belirtebilirsiniz `CorsPolicyBuilder` sınıfı. Bunu yapmanın iki yolu vardır. İlk UseCors ile bir lambda çağırmaktır:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**Not:** URL eğik belirtilmelidir (`/`). URL ile ererse `/`, karşılaştırma döndürülecek `false` ve üst bilgi döndürülür.

Lambda geçen bir `CorsPolicyBuilder` nesnesi. Bir listesini bulabilirsiniz [yapılandırma seçenekleri](#cors-policy-options) bu konuda daha sonra. Bu örnekte, ilke gelen çıkış noktaları arası isteklere izin verir. `http://example.com` ve başka bir kaynak yok.

Yöntem çağrıları zincir şekilde CorsPolicyBuilder fluent API olduğuna dikkat edin:

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

İkinci bir veya daha fazla adlandırılmış CORS ilkelerini tanımlamak ve ardından ilkeyi çalışma zamanında adına göre seçmek için bir yaklaşımdır.

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

Bu örnekte "AllowSpecificOrigin" adlı bir CORS ilkesi ekler. İlkeyi seçmek için adına geçirmek `UseCors`.

## <a name="enabling-cors-in-mvc"></a>MVC CORS'yi etkinleştirme

MVC, eylem, denetleyici başına veya genel olarak tüm denetleyicileri için başına belirli CORS uygulamak için alternatif olarak kullanabilirsiniz. MVC CORS'yi etkinleştirmeniz kullanırken aynı CORS Hizmetleri kullanılır, ancak CORS Ara değil.

### <a name="per-action"></a>Her eylem

Belirtmek için belirli bir eylemi için CORS ilkesinin ekleyin `[EnableCors]` özniteliği eylem. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>Denetleyici

Belirtmek için belirli bir denetleyicinin CORS ilkesi ekleyin `[EnableCors]` özniteliği için denetleyici sınıfı. İlke adı belirtin.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>Genel

CORS genel olarak tüm denetleyicilerinin ekleyerek etkinleştirebilirsiniz `CorsAuthorizationFilterFactory` genel filtre koleksiyonuna filtre:

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

Öncelik sırası: eylem, denetleyici, genel. Eylem düzeyi ilkeleri denetleyicisi düzeyi ilkeleri göre önceliklidir ve denetleyici düzeyinde ilkeleri genel ilkelere göre önceliklidir.

### <a name="disable-cors"></a>CORS devre dışı bırak

Denetleyici veya eylem için CORS devre dışı bırakmak için `[DisableCors]` özniteliği.

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS ilkesi seçenekleri

Bu bölümde CORS ilke ayarlayabileceğiniz çeşitli seçenekler açıklanmaktadır.

* [İzin verilen çıkış noktası ayarlama](#set-the-allowed-origins)

* [İzin verilen HTTP yöntemleri Ayarla](#set-the-allowed-http-methods)

* [İzin verilen istek üstbilgilerini Ayarla](#set-the-allowed-request-headers)

* [Sunulan yanıt üstbilgilerini Ayarla](#set-the-exposed-response-headers)

* [Cross-origin istekleri kimlik bilgileri](#credentials-in-cross-origin-requests)

* [Denetim öncesi sona erme süresini ayarlama](#set-the-preflight-expiration-time)

İçin bazı seçenekler okumak yararlı olabilir [nasıl CORS çalışır](#how-cors-works) ilk.

### <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktası ayarlama

Bir veya daha fazla belirli kaynaklara izin veren:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

Tüm kaynaklara izin vermek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün. Bu, tam anlamıyla herhangi bir Web apı'nize AJAX çağrıları yapabilirsiniz anlamına gelir.

### <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

Tüm HTTP yöntemleri izin vermek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

Bu ön uçuş istekleri ve erişim-denetim-Allow-Methods üstbilgi etkiler.

### <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

CORS denetim öncesi isteği uygulama tarafından ayarlanıp HTTP üst bilgileri listeleyen bir Access-Control-Request-Headers üstbilgisi içerebilir (sözde "yazar, istek üstbilgileri").

Beyaz liste belirli üstbilgileri:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

İzin vermek için tüm istek üstbilgileri Yazar:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

Tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil. Üstbilgi için herhangi bir şey dışında ayarlarsanız "*", "kabul", "content-type" ve "kaynak" artı desteklemek istediğiniz tüm özel üstbilgileri en az içermelidir.

### <a name="set-the-exposed-response-headers"></a>Sunulan yanıt üstbilgilerini Ayarla

Varsayılan olarak, tüm uygulama yanıt üstbilgilerinin tarayıcı açığa çıkarmıyor. (Bkz [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Varsayılan olarak kullanılabilir yanıt üstbilgilerini şunlardır:

* Cache-Control

* İçerik dili

* İçerik Türü

* Süre sonu

* Son değiştiren

* Pragma

CORS spec bunlar çağırır *basit yanıt üstbilgilerini*. Diğer üstbilgileri uygulama kullanılabilir hale getirmek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>Cross-origin istekleri kimlik bilgileri

Kimlik bilgileri bir CORS istek özel işleme gerektirir. Varsayılan olarak, tarayıcı çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez. Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama şemasını içerir. Kimlik bilgileri ile bir çıkış noktaları arası istek göndermek için istemci XMLHttpRequest.withCredentials true olarak ayarlamanız gerekir.

XMLHttpRequest doğrudan kullanma:

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

Ayrıca, sunucu kimlik bilgilerine izin vermeniz gerekir. Çıkış noktaları arası kimlik bilgilerine izin vermek için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

Artık HTTP yanıtı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler bir erişim-denetim-Allow-Credentials üstbilgisi içerecektir.

Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulaması yanıtı kullanıma olmaz ve AJAX isteği başarısız olur.

Çıkış noktaları arası kimlik bilgilerine izin verirken dikkatli olun. Başka bir etki alanındaki bir Web sitesi uygulama kullanıcının adına kullanıcının bilgisi olmadan oturum açmış kullanıcının kimlik bilgileri gönderebilir. CORS belirtimi de bu ayar durumları kaynakları için "*" (tüm kaynaklara) geçersiz durumunda `Access-Control-Allow-Credentials` başlığıdır mevcut.

### <a name="set-the-preflight-expiration-time"></a>Denetim öncesi sona erme süresini ayarlama

Ne kadar süreyle denetim öncesi isteğinin yanıtı önbelleğe erişim-denetim-Max-Age üstbilgisini belirtir. Bu üst ayarlamak için:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, bir CORS İstek HTTP iletileri düzeyde neler açıklanmaktadır. Böylece CORS İlkesi doğru şekilde yapılandırılabilir CORS nasıl çalıştığını ve troubleshooted beklenmeyen davranışları ortaya çıktığında anlamak önemlidir.

CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üstbilgileri tanıtır. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri cross-origin istekleri için otomatik olarak ayarlar. Özel JavaScript kodu CORS etkinleştirmek için gerekli değildir.

Çıkış noktaları arası istek bir örneği burada verilmiştir. `Origin` Üstbilgisi isteği yapan sitenin etki alanını sağlar:

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

Sunucu isteği izin veriyorsa, yanıtta Access-Control-Allow-Origin üstbilgisini ayarlar. Bu üstbilgi değerini istek kaynağı başlığından eşleşen veya joker karakter değeri "*", yani her türlü kaynağa izin verilir:

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

Access-Control-Allow-Origin üstbilgisi yanıt içermiyorsa, AJAX isteği başarısız olur. Özellikle, tarayıcı istek izin vermez. Sunucu başarılı bir yanıt döndürürse bile, tarayıcı yanıtı istemci uygulamasının kullanımına yapmaz.

### <a name="preflight-requests"></a>Denetim öncesi isteği

Bazı CORS istekler için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı bir ek istek gönderir. Aşağıdaki koşullar doğruysa, tarayıcı denetim öncesi isteği atlayabilirsiniz:

* İstek yöntemini GET, HEAD veya POST, olduğundan ve

* Uygulama kabul etme, Accept-Language, Content-Language, dışındaki tüm istek üstbilgileri ayarlamaz Content-Type veya son-olay-ID ve

* Content-Type üstbilgisi (varsa ayarlayın) aşağıdakilerden biri:

  * Uygulama/x-www-form-urlencoded

  * multipart/form-data

  * Metin/düz

İstek üstbilgileri hakkında kural XMLHttpRequest nesnesinde setRequestHeader çağırarak uygulama ayarlar üstbilgileri uygulanır. (CORS belirtimi bu "Yazar istek üstbilgileri" çağırır.) Kural User-Agent, ana bilgisayar veya Content-Length gibi tarayıcı ayarlayabilirsiniz üstbilgileri uygulanmaz.

Bir denetim öncesi isteği bir örneği burada verilmiştir:

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

Ön uçuş isteği HTTP OPTIONS yöntemini kullanır. İki özel üstbilgi içerir:

* Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.

* Access-Control-Request-Headers: Uygulama gerçek isteği ayarlama istek üstbilgileri listesi. (Yeniden, bu tarayıcı ayarlar üstbilgileri içermez.)

Sunucu isteği izin verdiğini varsayarak bir örnek yanıt şöyledir:

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

Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üstbilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeler bir Access-Control-izin ver-Headers üstbilgisi içeriyor. Denetim öncesi isteği başarılı olursa, tarayıcı daha önce açıklandığı gibi gerçek isteğini gönderir.
