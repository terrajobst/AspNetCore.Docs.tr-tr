---
title: Yanıt önbelleğe alma ara yazılımı ASP.NET core'da
author: guardrex
description: Yapılandırma ve ASP.NET Core yanıt önbelleğe alma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
uid: performance/caching/middleware
ms.openlocfilehash: b5eef012356236d600d026e5161a2df5a7954e3b
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090491"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Yanıt önbelleğe alma ara yazılımı ASP.NET core'da

Tarafından [Luke Latham](https://github.com/guardrex) ve [John Luo](https://github.com/JunTaoLuo)

[Görüntülemek veya ASP.NET Core 2.1 örnek kodu indirdikten](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu makalede, yanıtları önbelleğe alma ara yazılımı ASP.NET Core uygulaması yapılandırma açıklanmaktadır. Ara yazılım, yanıtları önbelleğe olduğunda, depoları yanıtları ve önbelleğe alınan hizmet etmesi yanıtları belirler. HTTP önbelleğe alma için giriş ve `ResponseCache` özniteliği için bkz: [yanıt önbelleğe alma](xref:performance/caching/response).

## <a name="package"></a>Paket

::: moniker range=">= aspnetcore-2.1"

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paket.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paket.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Paket başvurusu ekleme [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paket.

::: moniker-end

## <a name="configuration"></a>Yapılandırma

İçinde `Startup.ConfigureServices`, ara yazılım, hizmet koleksiyona ekleyin.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Ara yazılımla kullanmak için uygulamayı yapılandırma `UseResponseCaching` ara yazılım istek işleme ardışık düzene ekler genişletme yöntemi. Örnek uygulamayı ekler bir [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) 10 saniye boyunca önbelleğe yanıtları önbelleğe yanıt üst bilgisi. Örnek gönderir bir [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) önbelleğe alınan yanıt eksikse görev yapacak bir ara yazılımını yapılandırma başlığı [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) sonraki istekleri üstbilgisinin eşleşen özgün istek. Aşağıdaki kod örneğinde [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) ve [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) gerektiren bir `using` bildirimi [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) ad alanı.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

Yanıtları önbelleğe alma ara yazılımı yalnızca bir 200 (Tamam) durum kodu neden sunucu yanıtları önbelleğe alır. Dahil olmak üzere diğer bir yanıt [hata sayfalarını](xref:fundamentals/error-handling), ara yazılım tarafından göz ardı edilir.

> [!WARNING]
> İçerik için kimlik doğrulamasından geçen istemcilerin yanıtlarla depolamak ve bu yanıtlar hizmet veren bir ara yazılım önlemek için önbelleğe olarak işaretlenmesi gerekir. Bkz: [önbelleğe alma için koşullar](#conditions-for-caching) nasıl ara yazılım bir yanıt önbelleğe olup olmadığını belirler. Ayrıntılar için.

## <a name="options"></a>Seçenekler

Ara yazılım denetleme yanıt önbelleğe alma için üç seçenek sunar.

| Seçenek                | Açıklama |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Büyük/küçük harfe yollarında yanıtları önbelleğe alınmaz belirler. Varsayılan değer `false` şeklindedir. |
| MaximumBodySize       | Yanıt gövdesini bayt cinsinden en büyük önbelleğe boyutu. Varsayılan değer `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Boyut sınırını bayt cinsinden yanıt önbelleği ara yazılımı. Varsayılan değer `100 * 1024 * 1024` (100 MB). |

Aşağıdaki örnek, ara yazılım yapılandırır:

* Önbellek yanıtları değerinden küçük veya eşit 1024 bayt.
* Büyük küçük harfe duyarlı yollar yanıtları Store (örneğin, `/page1` ve `/Page1` ayrı olarak depolanır).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

MVC/Web APİ'si denetleyicilerinin veya Razor sayfaları sayfa modeli kullanılırken `ResponseCache` özniteliği yanıt önbelleğe alma için uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir. Yalnızca parametresi `ResponseCache` ara yazılım kesinlikle gerektiren özniteliği `VaryByQueryKeys`, gerçek bir HTTP üst bilgisi gelmiyor. Daha fazla bilgi için [ResponseCache özniteliği](xref:performance/caching/response#responsecache-attribute).

Değil kullanırken `ResponseCache` özniteliği, yanıt önbelleğe alma değiştirilen ile `VaryByQueryKeys` özelliği. Kullanım `ResponseCachingFeature` doğrudan `IFeatureCollection` , `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Tek bir değer eşit kullanarak `*` içinde `VaryByQueryKeys` tüm istek sorgu parametreleri tarafından önbelleğe değişir.

## <a name="http-headers-used-by-response-caching-middleware"></a>Yanıtları önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri

Yanıtları önbelleğe alma ara yazılımı tarafından HTTP üst bilgileri kullanılarak yapılandırılır.

| Üstbilgi | Ayrıntılar |
| ------ | ------- |
| Yetkilendirme | Yanıt üstbilgisi mevcutsa önbelleğe değil. |
| Önbellek denetimi | İle işaretlenen yanıtları önbelleğe alma ara yazılımı yalnızca dikkate `public` önbellek yönergesi. Aşağıdaki parametrelerle önbelleğe alma denetler:<ul><li>Maksimum yaş</li><li>en çok eski&#8224;</li><li>Min-yeni</li><li>revalidate gerekir</li><li>önbellek yok</li><li>No-store</li><li>yalnızca IF-önbelleğe alma</li><li>private</li><li>public</li><li>s maxage</li><li>Proxy revalidate&#8225;</li></ul>&#8224;Sınır belirtilirse `max-stale`, ara yazılımın herhangi bir eylemi alır.<br>&#8225;`proxy-revalidate`aynı etkiye sahiptir `must-revalidate`.<br><br>Daha fazla bilgi için [RFC 7231: İstek ön bellek denetimi yönergelerini](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragması | A `Pragma: no-cache` istek üstbilgisinde üretir ile aynı etkiye `Cache-Control: no-cache`. Bu üstbilginin ilgili yönergeleri tarafından geçersiz kılınır `Cache-Control` üst bilgi, varsa. HTTP/1.0 ile geriye dönük uyumluluk için kabul edilir. |
| Tanımlama bilgisi-Ayarla | Yanıt üstbilgisi mevcutsa önbelleğe değil. Yanıt önbelleğe alınan yanıtları önbelleğe alma ara yazılımı Ara yazılımların, bir veya daha fazla tanımlama bilgisi ayarlar istek işleme ardışık düzeninde engeller (örneğin, [tanımlama bilgisi tabanlı TempData sağlayıcısı](xref:fundamentals/app-state#tempdata)).  |
| değişiklik | `Vary` Üst bilgisi önbelleğe alınan yanıtın farklılık için kullanılan başka bir üstbilgisi tarafından. Örneğin, dahil ederek kodlayarak yanıtları önbelleğe `Vary: Accept-Encoding` üst bilgisi için istek üst bilgilerini yanıtlarını önbelleğe kaydeder `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı olarak. Yanıt üst bilgisi değeri `*` hiçbir zaman depolanır. |
| Süre sonu | Bu üstbilgisi tarafından eski yorumlanamayacağını yanıt olmadığı veya depolanan diğer tarafından geçersiz kılınmadığı sürece alınan `Cache-Control` üstbilgileri. |
| If-None-Match | Değer yoksa tam yanıtı önbelleğinden sunulan `*` ve `ETag` yanıtı sağlanan değerlerden herhangi birini eşleşmiyor. Aksi takdirde, 304 (değiştirilmedi) yanıtı sunulur. |
| If-Modified-Since | Varsa `If-None-Match` üstbilgisi mevcut değilse, önbelleğe alınan yanıt tarihi sağlanan değer yeniyse tam yanıtı önbelleğinden sunulur. Aksi takdirde, 304 (değiştirilmedi) yanıtı sunulur. |
| Tarih | Önbellekten yüklenir, hizmet zaman `Date` üst bilgi, ara yazılım tarafından ayarlanır, özgün yanıtta verildiyse değildi. |
| İçerik Uzunluğu | Önbellekten yüklenir, hizmet zaman `Content-Length` üst bilgi, ara yazılım tarafından ayarlanır, özgün yanıtta verildiyse değildi. |
| Geçerlilik süresi | `Age` Özgün Yanıtta gönderilen üstbilgi göz ardı edilir. Ara yazılım, hizmet veren bir önbelleğe alınan yanıt zaman yeni bir değer hesaplar. |

## <a name="caching-respects-request-cache-control-directives"></a>Önbelleğe alma isteği ön bellek denetimi yönergelerini uyar

Ara yazılım kurallarına uyar [HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2). Geçerli bir kabul etmenin bir önbellek kuralları gerektiren `Cache-Control` istemci tarafından gönderilen başlığı. Altında belirtimi, bir istemci istekleri yapabilir bir `no-cache` üstbilgi değeri ve zorla her istek için yeni bir yanıt oluşturmak için sunucu. Şu anda, ara yazılım için resmi önbelleğe alma belirtimi aynılarını çünkü Ara yazılım kullanırken bu önbelleğe alma davranışı üzerinde Geliştirici denetimi yoktur.

Önbelleğe alma davranışı üzerinde daha fazla denetim için ASP.NET Core önbelleğe alma diğer özelliklerini keşfedin. Aşağıdaki konulara bakın:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Sorun giderme

Önbelleğe alma davranışını beklendiği gibi değilse, yanıtları önbelleğe alınabilir ve önbellekten sunulmasını özellikli olduğunu doğrulayın. İsteğin gelen üst bilgileri ve yanıtın giden üst bilgilerini inceleyin. Etkinleştirme [günlüğü](xref:fundamentals/logging/index) hatalarını ayıklamaya yardımcı olmak için.

Test etme ve sorun giderme önbelleğe alma davranışını bir tarayıcı istenmeyen şekilde önbelleğe almayı etkileyen istek üst bilgilerini ayarlayabilir. Örneğin, bir tarayıcı ayarlayabilir `Cache-Control` başlığına `no-cache` veya `max-age=0` bir sayfa yenileme esnasında oluşacak. Aşağıdaki araçları, istek üst bilgileri açıkça ayarlayabilir ve önbelleğe almayı test etme için tercih edilir:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Önbelleğe alma için koşullar

* İstek sunucu yanıtı bir 200 (Tamam) durum kodu ile sonuçlanması gerekir.
* İstek yöntemini GET veya HEAD olmalıdır.
* Terminal bir ara yazılım gibi [statik dosya ara yazılımlarını](xref:fundamentals/static-files), yanıtları önbelleğe alma ara yazılımı önce yanıt işlemez gerekir.
* `Authorization` Üstbilgisi mevcut olmamalıdır.
* `Cache-Control` üst bilgi parametreleri geçerli olmalıdır ve yanıt işaretlenmelidir `public` ve işaretlenmemiş `private`.
* `Pragma: no-cache` Üstbilgisi mevcut olmamalıdır, `Cache-Control` üst bilgi olarak mevcut olmayan `Cache-Control` üst bilgisi geçersiz kılar `Pragma` üst bilgisi mevcut olduğunda.
* `Set-Cookie` Üstbilgisi mevcut olmamalıdır.
* `Vary` üst bilgi parametreleri geçerli ile eşit değil olmalıdır `*`.
* `Content-Length` Üst bilgisi değeri (varsa ayarlayın) yanıt gövdesi boyutu ile eşleşmelidir.
* [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) kullanılmaz.
* Yanıt belirtildiği gibi eski olmamalıdır `Expires` üstbilgi ve `max-age` ve `s-maxage` önbelleğe yönergeleri.
* Yanıt arabelleğe alma başarılı olmalıdır ve yanıt boyutu yapılandırılmış küçük veya varsayılan `SizeLimit`.
* Yanıt önbelleğe göre [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimleri. Örneğin, `no-store` yönergesi istek veya yanıt üst bilgisi alanlarında mevcut olmalıdır. Bkz: *3. Bölüm: önbellekler yanıtlarını depolama* , [RFC 7234](https://tools.ietf.org/html/rfc7234) Ayrıntılar için.

> [!NOTE]
> Siteler arası istek sahteciliği (CSRF) önlemek için güvenli belirteçleri oluşturmak için Antiforgery sistem kümeleri saldırıları `Cache-Control` ve `Pragma` üstbilgileri `no-cache` böylece yanıtları önbelleğe değildir. Antiforgery belirteçleri için bir HTML form öğelerini devre dışı bırakma hakkında daha fazla bilgi için bkz: [ASP.NET Core antiforgery yapılandırma](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
