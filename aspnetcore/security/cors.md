---
title: ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında çapraz kaynak isteklerini izin vermek veya reddetmek için CORS 'nin nasıl standart olduğunu öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187259"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>ASP.NET Core 'de çıkış noktaları arası Istekleri (CORS) etkinleştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makalede, ASP.NET Core uygulamasında CORS 'nin nasıl etkinleştirileceği gösterilmektedir.

Tarayıcı güvenliği, bir Web sayfasının Web sayfasını sunduğundan farklı bir etki alanına istek yapmasını engeller. Bu kısıtlamaya *aynı-Origin ilkesi*adı verilir. Aynı-kaynak ilkesi, kötü niyetli bir sitenin gizli verileri başka bir siteden okumasını engeller. Bazen diğer sitelerin uygulamanıza çapraz çıkış istekleri yapmasına izin vermek isteyebilirsiniz. Daha fazla bilgi için bkz. [MOZILLA CORS makalesi](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

[Çapraz kaynak kaynak paylaşımı](https://www.w3.org/TR/cors/) (CORS):

* , Bir sunucunun aynı kaynak ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.
* Güvenlik özelliği **değil** , CORS güvenliği. CORS 'nin izin vermesini sağlayan bir API daha güvenli değildir. Daha fazla bilgi için bkz. [CORS çalışma](#how-cors).
* Bir sunucunun bazı çapraz kaynak isteklerine, diğerlerini reddetirken açık olarak izin almasına izin verir.
* , [JSONP](/dotnet/framework/wcf/samples/jsonp)gibi önceki tekniklerin daha güvenli ve daha esnektir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>Aynı kaynak

Özdeş şemaları, konakları ve bağlantı noktalarına sahip olmaları durumunda iki URL aynı kaynağa sahiptir ([RFC 6454](https://tools.ietf.org/html/rfc6454)).

Bu iki URL aynı kaynağa sahiptir:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

Bu URL 'Ler, önceki iki URL 'den farklı kaynaklardan farklıdır:

* `https://example.net`&ndash; Farklı etki alanı
* `https://www.example.com/foo.html`&ndash; Farklı alt etki alanı
* `http://example.com/foo.html`&ndash; Farklı düzen
* `https://example.com:9000/foo.html`&ndash; Farklı bağlantı noktası

Internet Explorer, kaynakları karşılaştırırken bağlantı noktasını dikkate almaz.

## <a name="cors-with-named-policy-and-middleware"></a>Adlandırılmış ilke ve ara yazılım ile CORS

CORS ara yazılımı, çıkış noktaları arası istekleri işler. Aşağıdaki kod, belirtilen kaynağa sahip tüm uygulama için CORS 'yi sunar:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

Yukarıdaki kod:

* İlke adını "\_myallowspecifickaynaklarına" olarak ayarlar. İlke adı rastgele olur.
* CORS 'yi sağlayan genişletme yöntemini çağırır. <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>
* <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)ile çağrılar. Lambda bir <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> nesnesi alır. Gibi [yapılandırma seçenekleri](#cors-policy-options) `WithOrigins`, bu makalenin ilerleyen kısımlarında açıklanmıştır.

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Yöntem çağrısı, uygulamanın hizmet kapsayıcısına CORS Hizmetleri ekler:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

Daha fazla bilgi için bu belgedeki [CORS ilke seçenekleri](#cpo) bölümüne bakın.

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Yöntemi aşağıdaki kodda gösterildiği gibi yöntemleri zincirleyebilir:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

Not: URL sonunda eğik çizgi (`/` **) bulunmamalıdır.** URL ile `/`sonlandığında `false` karşılaştırma döndürülür ve üst bilgi döndürülmez.

::: moniker range=">= aspnetcore-3.0"

Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> Endpoint Routing ile CORS ara yazılımı, `UseRouting` ve `UseEndpoints`çağrıları arasında yürütülecek şekilde yapılandırılmalıdır. Yanlış yapılandırma, ara yazılımın doğru çalışmayı durdurmasına neden olur.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
Aşağıdaki kod, CORS ara yazılımı aracılığıyla CORS ilkelerini tüm uygulama uç noktalarına uygular:
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
Note: `UseCors` öğesinden önce `UseMvc`çağrılmalıdır.

::: moniker-end

Bu sayfa/denetleyici/eylem düzeyinde CORS ilkesini uygulamak için [Razor Pages, denetleyiciler ve eylem YÖNTEMLERINDE CORS 'Yi etkinleştirin](#ecors) konusuna bakın.

Önceki kodun test edilmesine ilişkin yönergeler için bkz. [Test CORS](#test) .

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>Uç nokta yönlendirme ile CORS 'yi etkinleştirme

Uç nokta yönlendirme ile, CORS, uzantı yöntemleri `RequireCors` kümesi kullanılarak uç nokta temelinde etkinleştirilebilir.

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

Benzer şekilde, CORS tüm denetleyiciler için de etkinleştirilebilir:

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a>CORS 'yi özniteliklerle etkinleştir

[ &lbrack;Enablecors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) özniteliği, CORS 'yi küresel olarak uygulamaya bir alternatif sağlar. `[EnableCors]` Öznitelik, tüm bitiş noktaları yerine, seçilen bitiş noktaları için CORS 'yi mümkün.

Varsayılan `[EnableCors]` ilkeyi belirtmek ve `[EnableCors("{Policy String}")]` bir ilke belirlemek için kullanın.

`[EnableCors]` Özniteliği öğesine uygulanabilir:

* Razor sayfası`PageModel`
* Kumandasını
* Denetleyici eylemi yöntemi

`[EnableCors]` Özniteliği ile denetleyici/sayfa-model/eylem 'e farklı ilkeler uygulayabilirsiniz. `[EnableCors]` Özniteliği bir denetleyiciler/sayfa modeli/eylem yöntemine uygulandığında ve bu işlem, ara yazılım içinde CORS 'yi etkinleştirmişse her iki ilke de uygulanır. İlkelerin birleştirilmesi önerilir. Aynı uygulamada değil, özniteliğiniveyaarayazılımınıkullanın.`[EnableCors]`

Aşağıdaki kod her bir yönteme farklı bir ilke uygular:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

Aşağıdaki kod, bir CORS varsayılan ilkesi ve adlı `"AnotherPolicy"`bir ilke oluşturur:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>CORS 'yi devre dışı bırak

Disablecors özniteliği denetleyici/sayfa modeli/eylem için CORS 'yi devre dışı bırakır. [ &lbrack;&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS ilke seçenekleri

Bu bölümde, bir CORS ilkesinde ayarlanmakta olabilecek çeşitli seçenekler açıklanmaktadır:

* [İzin verilen kaynakları ayarla](#set-the-allowed-origins)
* [İzin verilen HTTP yöntemlerini ayarlama](#set-the-allowed-http-methods)
* [İzin verilen istek üst bilgilerini ayarlama](#set-the-allowed-request-headers)
* [Gösterilen yanıt üst bilgilerini ayarlama](#set-the-exposed-response-headers)
* [Kaynaklar arası isteklerde kimlik bilgileri](#credentials-in-cross-origin-requests)
* [Ön kontrol sona erme süresini ayarlama](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>içinde `Startup.ConfigureServices`çağırılır. Bazı seçenekler için, ilk olarak [CORS 'Nin nasıl çalıştığı](#how-cors) bölümü okumanız yararlı olabilir.

## <a name="set-the-allowed-origins"></a>İzin verilen kaynakları ayarla

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Herhangi bir düzen (`http` veya `https`) ile tüm kaynaklardan gelen CORS isteklerine izin verir. &ndash; `AllowAnyOrigin`, *herhangi bir Web sitesi* uygulamaya çapraz kaynak istekleri yapabildiğinden güvenli değildir.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> `AllowAnyOrigin` Ve`AllowCredentials` güvenli olmayan bir yapılandırma belirtip, siteler arası istek sahteciliği ile sonuçlanabilir. Bir uygulama her iki yöntemle yapılandırıldığında, CORS hizmeti geçersiz bir CORS yanıtı döndürür.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> `AllowAnyOrigin` Ve`AllowCredentials` güvenli olmayan bir yapılandırma belirtip, siteler arası istek sahteciliği ile sonuçlanabilir. Güvenli bir uygulama için, istemcinin sunucu kaynaklarına erişim yetkisi olması gerekiyorsa, kaynakların tam bir listesini belirtin.

::: moniker-end

`AllowAnyOrigin`ön kontrol isteklerini ve `Access-Control-Allow-Origin` üstbilgiyi etkiler. Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>Kaynağa izin verilip verilmediğini değerlendirirken, kaynağın bir yapılandırılmış joker karakterle eşleşmesini sağlayan bir işlev olarak ilkenin özelliğiniayarlar.<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> &ndash;

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemlerini ayarlama

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* Herhangi bir HTTP yöntemine izin verir:
* Ön kontrol isteklerini ve `Access-Control-Allow-Methods` üstbilgiyi etkiler. Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.

### <a name="set-the-allowed-request-headers"></a>İzin verilen istek üst bilgilerini ayarlama

Belirli başlıkların, *Yazar isteği üstbilgileri*adlı bir CORS isteğinde gönderilmesine izin vermek için, ' i çağırın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ve izin verilen üst bilgileri belirtin:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Tüm yazar isteği başlıklarına izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Bu ayar, `Access-Control-Request-Headers` ön kontrol isteklerini ve üstbilgiyi etkiler. Daha fazla bilgi için bkz. [ön kontrol istekleri](#preflight-requests) bölümü.

::: moniker range=">= aspnetcore-2.2"

Tarafından belirtilen belirli başlıklarıyla `WithHeaders` eşleşen bir CORS ara yazılım ilkesi, yalnızca ' de `WithHeaders`belirtilen üstbilgiler ile `Access-Control-Request-Headers` tam olarak eşleşiyorsa mümkündür.

Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ara yazılımı, ([headernames. contentlanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) `WithHeaders`içinde `Content-Language` listelenmediği için aşağıdaki istek üst bilgisi ile bir ön denetim isteğini reddettiğinde:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

Uygulama *200 ok* yanıtı DÖNDÜRÜYOR ancak CORS üst bilgilerini geri göndermez. Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS ara yazılımı her zaman CorsPolicy `Access-Control-Request-Headers` . Headers ' de yapılandırılan değerlerden bağımsız olarak, ' deki dört üst bilgilerin gönderilmesine izin verir. Bu üst bilgi listesi şunları içerir:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

Örneğin, aşağıdaki gibi yapılandırılmış bir uygulamayı göz önünde bulundurun:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS ara yazılımı, her zaman beyaz listelenmiş olduğundan `Content-Language` , aşağıdaki istek üstbilgisiyle bir ön kontrol isteğine başarıyla yanıt veriyor:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>Gösterilen yanıt üst bilgilerini ayarlama

Varsayılan olarak tarayıcı, tüm yanıt üst bilgilerini uygulamaya sunmaz. Daha fazla bilgi için bkz [. W3C çıkış noktaları arası kaynak paylaşımı (terminoloji): Basit yanıt üst](https://www.w3.org/TR/cors/#simple-response-header)bilgisi.

Varsayılan olarak kullanılabilen yanıt üstbilgileri şunlardır:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS belirtimi, bu üst bilgiler *basit yanıt üst bilgilerini*çağırır. Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>çağırın:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>Kaynaklar arası isteklerde kimlik bilgileri

Kimlik bilgileri CORS isteğinde özel işleme gerektirir. Varsayılan olarak tarayıcı, kimlik bilgilerini bir çapraz kaynak isteğiyle göndermez. Kimlik bilgileri, tanımlama bilgileri ve HTTP kimlik doğrulama düzenlerini içerir. Bir çapraz kaynak isteğiyle kimlik bilgilerini göndermek için, istemcisinin olarak `XMLHttpRequest.withCredentials` `true`ayarlanması gerekir.

Doğrudan `XMLHttpRequest` kullanarak:

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

[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)'sini kullanma:

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

Sunucu kimlik bilgilerine izin vermelidir. Çıkış noktaları arası kimlik bilgilerine izin vermek için <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>şunu arayın:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP yanıtı, tarayıcıya sunucunun `Access-Control-Allow-Credentials` bir çapraz kaynak isteği için kimlik bilgileri verdiğini bildiren bir üst bilgi içerir.

Tarayıcı kimlik bilgilerini gönderirse ancak yanıt geçerli `Access-Control-Allow-Credentials` bir üst bilgi içermiyorsa, tarayıcı uygulamaya yanıtı kullanıma sunmaz ve çapraz kaynak isteği başarısız olur.

Çıkış noktaları arası kimlik bilgilerine izin vermek bir güvenlik riskidir. Başka bir etki alanındaki Web sitesi, kullanıcının bilgisi olmadan kullanıcı adına, oturum açmış bir kullanıcının kimlik bilgilerini uygulamaya gönderebilir. <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS belirtimi Ayrıca `"*"` `Access-Control-Allow-Credentials` üst bilgi varsa, çıkış (tüm kaynaklar) ayarının geçersiz olduğunu belirtir.

### <a name="preflight-requests"></a>Ön kontrol istekleri

Bazı CORS istekleri için, tarayıcı gerçek isteği yapmadan önce ek bir istek gönderir. Bu isteğe bir *ön kontrol isteği*denir. Aşağıdaki koşullar doğruysa tarayıcı, ön kontrol isteğini atlayabilir:

* İstek yöntemi al, HEAD veya POST.
* Uygulama `Accept` ,`Accept-Language` ,,`Last-Event-ID`veya dışındaki istek üst bilgilerini ayarlanmamış. `Content-Language` `Content-Type`
* , Ayarlandıysa, aşağıdaki değerlerden birine sahip olan üstbilgi:`Content-Type`
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

İstemci isteği için ayarlanan istek üst bilgileri kuralı, uygulamanın, `setRequestHeader` `XMLHttpRequest` nesne üzerinde çağırarak ayarladığı üst bilgiler için geçerlidir. CORS belirtimi, bu üst bilgiler *Yazar istek üst bilgilerini*çağırır. Kural `User-Agent` `Content-Length`, `Host`, veya gibi tarayıcının ayarlayabilmesi için, veya gibi bir üst bilgiye uygulanmaz.

Aşağıda bir ön denetim isteğine örnek verilmiştir:

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

Uçuş öncesi isteği HTTP SEÇENEKLERI yöntemini kullanır. İki özel üst bilgi içerir:

* `Access-Control-Request-Method`: Gerçek istek için kullanılacak HTTP yöntemi.
* `Access-Control-Request-Headers`: Uygulamanın gerçek istekte ayarladığı istek üst bilgilerinin listesi. Daha önce belirtildiği gibi, bu, tarayıcının ayarladığı `User-Agent`üst bilgileri içermez.

CORS ön kontrol isteği, sunucuya gerçek `Access-Control-Request-Headers` istekle gönderilen üstbilgileri belirten bir üst bilgi içerebilir.

Belirli üstbilgilere izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>arayın:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

Tüm yazar isteği başlıklarına izin vermek için şunu <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>çağırın:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

Tarayıcılar, nasıl ayarlandıklarından `Access-Control-Request-Headers`tamamen tutarlı değildir. Üst bilgileri ( `"*"` veya kullanımı <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>) dışında bir şeye ayarlarsanız, desteklemek istediğiniz en az `Accept`, `Content-Type`, ve ve `Origin`Ek üstbilgileri dahil etmelisiniz.

Aşağıda, ön kontrol isteğine örnek bir yanıt verilmiştir (sunucunun isteğe izin verdiği varsayıldığında):

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

Yanıt, izin verilen `Access-Control-Allow-Methods` üst bilgileri listeleyen, izin verilen yöntemleri ve isteğe `Access-Control-Allow-Headers` bağlı olarak bir üst bilgiyi listeleyen bir üst bilgi içerir. Ön kontrol isteği başarılı olursa, tarayıcı gerçek isteği gönderir.

Ön kontrol isteği reddedilirse, uygulama *200 ok* yanıtı döndürür ancak CORS üst bilgilerini geri göndermez. Bu nedenle tarayıcı, çıkış noktaları arası isteği denemez.

### <a name="set-the-preflight-expiration-time"></a>Ön kontrol sona erme süresini ayarlama

`Access-Control-Max-Age` Üst bilgi, ön kontrol isteğine olan yanıtın ne kadar süreyle önbelleğe alınacağını belirtir. Bu üstbilgiyi ayarlamak için şunu arayın <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS nasıl kullanılır?

Bu bölümde, HTTP iletileri düzeyindeki bir [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) isteğinde ne olacağı açıklanmaktadır.

* CORS bir güvenlik özelliği **değil** . CORS, bir sunucunun aynı kaynaklı ilkeyi rahat bir şekilde sağlamasına izin veren bir W3C standardıdır.
  * Örneğin, kötü niyetli bir aktör sitenize karşı [siteler arası komut dosyalarını (XSS) engelliyor](xref:security/cross-site-scripting) ve bilgileri çalmak için CORS özellikli sitesine bir siteler arası istek yürütebilir.
* CORS 'nin izin vermesini sağlayan API 'niz daha güvenli değildir.
  * CORS 'yi zorlamak için istemciye (tarayıcı) sahiptir. Sunucu isteği yürütür ve yanıtı döndürür. Bu, bir hatayı döndüren ve yanıtı engelleyen istemcdir. Örneğin, aşağıdaki araçlardan herhangi birinde sunucu yanıtı görüntülenir:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * Adres çubuğuna URL girerek bir Web tarayıcısı.
* Bir sunucu, tarayıcıların çapraz kaynak [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) veya [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) isteği çalıştırmasına izin vermesinin yasaktır bir yoludur.
  * Tarayıcılar (CORS olmadan), çapraz kaynak istekleri yapamıyor. CORS 'den önce, bu kısıtlamayı aşmak için [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) kullanılmıştır. JSONP XHR kullanmaz, yanıtı almak için `<script>` etiketini kullanır. Betiklerin, çapraz kaynak olarak yüklenmesine izin verilir.

[CORS belirtimi](https://www.w3.org/TR/cors/) , çıkış noktaları arası istekleri etkinleştiren bırkaç yeni http üst bilgisi sunmuştur. Bir tarayıcı CORS 'yi destekliyorsa, bu üst bilgileri, çıkış noktaları arası istekler için otomatik olarak ayarlar. CORS 'yi etkinleştirmek için özel JavaScript kodu gerekli değildir.

Aşağıda, bir çapraz kaynak isteğine bir örnek verilmiştir. `Origin` Üst bilgi, isteği yapan sitenin etki alanını sağlar:

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

Sunucu isteğe izin veriyorsa, yanıttaki `Access-Control-Allow-Origin` üstbilgiyi ayarlar. Bu üstbilginin değeri, istekten gelen `Origin` üstbilgiyle eşleşir veya joker değerdir `"*"`, yani herhangi bir kaynağa izin verilir.

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

Yanıt `Access-Control-Allow-Origin` üstbilgiyi içermiyorsa, çapraz kaynak isteği başarısız olur. Özellikle, tarayıcı isteğe izin vermez. Sunucu başarılı bir yanıt döndürse bile tarayıcı, yanıtı istemci uygulama için kullanılabilir hale getirir.

<a name="test"></a>

## <a name="test-cors"></a>Test CORS

CORS 'yi sınamak için:

1. [BIR API projesi oluşturun](xref:tutorials/first-web-api). Alternatif olarak, [örneği de indirebilirsiniz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).
1. Bu belgedeki yaklaşımlardan birini kullanarak CORS 'yi etkinleştirin. Örneğin:

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`yalnızca [indirme örnek koduna](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)benzer bir örnek uygulamanın test edilmesi için kullanılmalıdır.

1. Web uygulaması projesi (Razor Pages veya MVC) oluşturun. Örnek Razor Pages kullanır. Web uygulamasını, API projesiyle aynı çözümde oluşturabilirsiniz.
1. Aşağıdaki Vurgulanan kodu *Index. cshtml* dosyasına ekleyin:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. Yukarıdaki kodda, dağıtılan uygulamanın URL `url: 'https://<web app>.azurewebsites.net/api/values/1',` 'siyle değiştirin.
1. API projesini dağıtın. Örneğin, [Azure 'a dağıtın](xref:host-and-deploy/azure-apps/index).
1. Razor Pages veya MVC uygulamasını masaüstünden çalıştırın ve **Test** düğmesine tıklayın. Hata iletilerini gözden geçirmek için F12 araçlarını kullanın.
1. Localhost kaynağını `WithOrigins` kaldırın ve uygulamayı dağıtın. Alternatif olarak, istemci uygulamasını farklı bir bağlantı noktasıyla çalıştırın. Örneğin, Visual Studio 'dan çalıştırın.
1. İstemci uygulamasıyla test edin. CORS hataları bir hata döndürüyor, ancak hata iletisi JavaScript için kullanılabilir değil. Hatayı görmek için F12 araçlarındaki konsol sekmesini kullanın. Tarayıcıya bağlı olarak, aşağıdakine benzer bir hata alırsınız (F12 araçları konsolunda):

   * Microsoft Edge 'i kullanarak:

     **SEC7120: [CORS] kaynak `https://localhost:44375` , ' deki çıkış noktaları için erişim-denetim-izin-kaynak yanıt üstbilgisinde bulunamadı `https://localhost:44375``https://webapi.azurewebsites.net/api/values/1`**

   * Chrome kullanarak:

     **`https://webapi.azurewebsites.net/api/values/1` Kaynak`https://localhost:44375` 'ten itibaren XMLHttpRequest erişimi, CORS ilkesi tarafından engellendi: İstenen kaynakta ' erişim-denetim-Izin-Origin ' üst bilgisi yok.**

## <a name="additional-resources"></a>Ek kaynaklar

* [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
