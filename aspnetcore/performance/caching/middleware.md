---
title: "Yanıt Ara yazılımında ASP.NET çekirdek önbelleğe alma"
author: guardrex
description: "Yapılandırma ve ASP.NET Core yanıt önbelleğe alma Ara kullanma öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/26/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/middleware
ms.openlocfilehash: e9a74d8f6c3945b1bc8c62d0ab21145a7c5717fb
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Yanıt Ara yazılımında ASP.NET çekirdek önbelleğe alma

Tarafından [Luke Latham](https://github.com/guardrex) ve [John Luo](https://github.com/JunTaoLuo)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu makalede, bir ASP.NET Core uygulamada yanıt önbelleğe alma ara yazılım yapılandırma açıklanmaktadır. Ara yazılım yanıtları alınabilir olduğunda, depoları yanıtlar ve görevi görür yanıtları önbelleğinden belirler. HTTP önbellek giriş ve `ResponseCache` özniteliği için bkz: [yanıt önbelleğe alma](xref:performance/caching/response).

## <a name="package"></a>Paket

Ara yazılım bir proje eklemek için bir başvuru ekleyin [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paketini veya kullanmak [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paket (ASP.NET Core 2.0 veya üstü .NET Core hedeflerken).

## <a name="configuration"></a>Yapılandırma

İçinde `ConfigureServices`, ara yazılım hizmeti koleksiyonuna ekleyin.

[!code-csharp[](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

Ara yazılımla kullanmak için uygulamayı yapılandırma `UseResponseCaching` ara yazılım istek işleme ardışık düzenine ekler genişletme yöntemi. Örnek uygulaması ekler bir [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) alınabilir yanıtları 10 saniye için önbelleğe yanıtı üstbilgisi. Örnek gönderir bir [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) önbelleğe alınan yanıt eksikse sunmak için ara yazılımını yapılandırma üstbilgi [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) sonraki istekleri üstbilgisinin, özgün istek eşleşir.

[!code-csharp[](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

Yanıt önbelleğe alma ara yazılımı yalnızca 200 (Tamam) durum koduna neden sunucu yanıtlarını önbelleğe alır. Dahil olmak üzere diğer bir yanıtlar [hata sayfaları](xref:fundamentals/error-handling), ara yazılım tarafından göz ardı edilir.

> [!WARNING]
> Kimliği doğrulanmış istemciler için içeriğin bulunduğu yanıtları depolamak ve bu yanıtlara hizmet ara yazılım önlemek için alınabilir olarak işaretlenmesi gerekir. Bkz: [önbelleğe alma için koşulları](#conditions-for-caching) ilişkin ayrıntılar nasıl ara yazılım bir yanıt alınabilir olup olmadığını belirler.

## <a name="options"></a>Seçenekler

Ara yazılım denetleme yanıt önbelleğe alma için üç seçenek sunar.

| Seçenek                | Varsayılan Değer |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Büyük küçük harfe duyarlı yollarında yanıtlarını önbelleğe alınmaz belirler.</p><p>Varsayılan değer `false` şeklindedir. |
| MaximumBodySize       | Yanıt gövdesi bayt cinsinden en büyük alınabilir boyutu.</p>Varsayılan değer `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Bayt cinsinden yanıt önbellek ara yazılımı için boyut sınırı. Varsayılan değer `100 * 1024 * 1024` (100 MB). |

Aşağıdaki örnek Ara yapılandırır:

* ' Den küçük veya eşit 1024 bayt yanıtları önbelleğe alır.
* Büyük küçük harfe duyarlı yollarıyla yanıtlarını depolamak (örneğin, `/page1` ve `/Page1` ayrı olarak depolanır).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

MVC/Web API denetleyicilerinin veya Razor sayfalarının sayfa modelleri kullanırken `ResponseCache` özniteliği yanıt önbelleğe alma için uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir. Yalnızca parametresinin `ResponseCache` kesinlikle ara yazılımı gerektirir özniteliği `VaryByQueryKeys`, hangi gerçek bir HTTP üstbilgisi karşılık değil. Daha fazla bilgi için bkz: [ResponseCache özniteliği](xref:performance/caching/response#responsecache-attribute).

Değil kullanırken `ResponseCache` özniteliği, yanıt önbelleğe alma değişmesi ile `VaryByQueryKeys` özelliği. Kullanım `ResponseCachingFeature` doğrudan `IFeatureCollection` , `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

Tek bir değer eşit kullanarak `*` içinde `VaryByQueryKeys` tüm istek sorgu parametreleri tarafından önbelleğe değişir.

## <a name="http-headers-used-by-response-caching-middleware"></a>Yanıt önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri

Yanıt ara yazılım tarafından önbelleğe alma HTTP üstbilgileri kullanılarak yapılandırılır.

| Üstbilgi | Ayrıntılar |
| ------ | ------- |
| Yetkilendirme | Yanıt üstbilgisi mevcutsa önbelleğe değil. |
| Cache-Control | İle işaretlenen yanıtlarını önbelleğe alma ara yazılımı yalnızca dikkate `public` önbellek yönergesi. Aşağıdaki parametrelerle önbelleğe alınmasını denetleyen:<ul><li>Maksimum yaş</li><li>max-stale&#8224;</li><li>Min-yeni</li><li>gereken revalidate</li><li>Hayır-önbellek</li><li>Hayır deposu</li><li>yalnızca IF-önbelleğe alma</li><li>private</li><li>public</li><li>s-maxage</li><li>proxy-revalidate&#8225;</li></ul>& # için sınır belirtilmişse 8224; `max-stale`, ara yazılımın herhangi bir eylemi alır.<br>&#8225; `proxy-revalidate` aynı etkiye sahip `must-revalidate`.<br><br>Daha fazla bilgi için bkz: [RFC 7231: İstek önbellek denetimi yönergelerini](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | A `Pragma: no-cache` istek üstbilgisinde üreten aynı etkiye `Cache-Control: no-cache`. Bu üst ilgili yönergeleri tarafından geçersiz kılınır `Cache-Control` üstbilgisi, varsa. HTTP/1.0 ile geriye dönük uyumluluk için kabul. |
| Set-Cookie | Yanıt üstbilgisi mevcutsa önbelleğe değil. |
| değişir | `Vary` Üstbilgisi önbelleğe alınan yanıtın farklılık için kullanılan başka bir üstbilgisi tarafından. Örneğin, ekleyerek kodlayarak yanıtı önbelleğe `Vary: Accept-Encoding` üstbilgileri olan istekler için yanıtlar önbelleğe alır, üst `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı olarak. Bir yanıt üstbilgisi değerini `*` hiçbir zaman depolanır. |
| Süre sonu | Bu üstbilgisi tarafından eski kabul yanıt değil veya depolanan diğer tarafından geçersiz kılınmadığı sürece alınan `Cache-Control` üstbilgileri. |
| If-None-Match | Değer yoksa tam yanıtı önbelleğinden sunulan `*` ve `ETag` yanıtını sağlanan değerlerden herhangi birini eşleşmiyor. Aksi takdirde 304 (değişiklik) yanıt sunulur. |
| If-Modified-Since | Varsa `If-None-Match` üstbilgisi mevcut değilse, önbelleğe alınan yanıt tarih sağlanan değerden daha yeniyse tam yanıtı önbelleğinden sunulur. Aksi takdirde 304 (değişiklik) yanıt sunulur. |
| Tarih | Önbelleğe alınan hizmet veren zaman `Date` üstbilgi ayarlanmışsa ara yazılım tarafından özgün yanıtta sağlanmadı. |
| İçerik Uzunluğu | Önbelleğe alınan hizmet veren zaman `Content-Length` üstbilgi ayarlanmışsa ara yazılım tarafından özgün yanıtta sağlanmadı. |
| geçerlilik süresi | `Age` Özgün yanıt olarak gönderilen üstbilgisi göz ardı edilir. Ara yazılım, önbelleğe alınan yanıt hizmet veren zaman yeni bir değer hesaplar. |

## <a name="caching-respects-request-cache-control-directives"></a>Önbelleğe alma isteği Cache-Control yönergeleri dikkate alır

Ara yazılım kurallarına uyar [HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2). Geçerli bir vermenizin bir önbellek kuralları gerektiren `Cache-Control` istemci tarafından gönderilen üstbilgisi. Altında belirtimi, istemci istekleriyle yapabilirsiniz bir `no-cache` üstbilgi değeri ve her istek için yeni bir yanıt oluşturmak için sunucu zorla. Şu anda, ara yazılım resmi önbelleğe alma belirtimi aynılarını ara yazılım kullanırken bu önbelleğe alma davranışını Geliştirici denetleyemez yoktur.

Önbelleğe alma davranışı üzerinde daha fazla denetim için ASP.NET Core diğer önbelleğe alma özelliklerini keşfedin. Aşağıdaki konulara bakın:

* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [ASP.NET Core MVC etiketi yardımcı önbelleğe alma](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Sorun giderme

Önbelleğe alma davranışı beklendiği gibi değilse, yanıtları alınabilir ve önbellekten sunulmasını, özelliğine sahip olduğundan emin olun. İsteğin gelen üstbilgileri ve yanıtın giden üstbilgileri inceleyin. Etkinleştirme [günlüğü](xref:fundamentals/logging/index) hata ayıklamaya yardımcı olmak için.

Sınama ve önbelleğe alma davranışını sorun giderme, bir tarayıcı istenmeyen şekilde önbelleğe almayı etkileyen istek üstbilgilerini Ayarla. Örneğin, bir tarayıcı ayarlayabilir `Cache-Control` başlığına `no-cache` veya `max-age=0` bir sayfa yenilerken. Aşağıdaki araçlar önbelleğe almayı test etmek için tercih edilir ve istek üstbilgileri açıkça ayarlayabilirsiniz:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Önbelleğe alma için koşullar

* İstek bir 200 (Tamam) durum koduna sahip bir sunucu yanıtı neden gerekir.
* İstek yöntemini GET veya HEAD olması gerekir.
* Terminal Ara gibi [statik dosya ara yazılımlarını](xref:fundamentals/static-files), yanıt önbelleğe alma yanıt Ara önce işlemez gerekir.
* `Authorization` Üstbilgisi mevcut olmamalıdır.
* `Cache-Control` Üstbilgi parametreleri geçerli olmalıdır ve yanıt işaretlenmelidir `public` ve işaretlenmemiş `private`.
* `Pragma: no-cache` Üstbilgisi mevcut olmamalıdır, `Cache-Control` üstbilgisi olarak mevcut değil `Cache-Control` üstbilgisi geçersiz kılar `Pragma` üstbilgi varsa.
* `Set-Cookie` Üstbilgisi mevcut olmamalıdır.
* `Vary` Üstbilgi parametreleri geçerli ve eşit değil olmalıdır `*`.
* `Content-Length` Üstbilgi değeri (varsa ayarlayın) yanıt gövdesi boyutu ile eşleşmelidir.
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) kullanılmaz.
* Yanıt belirtildiği gibi eski olmamalıdır `Expires` üstbilgi ve `max-age` ve `s-maxage` önbelleğe yönergeleri.
* Yanıt arabelleğe alma başarılı olmalıdır ve yanıt boyutunun yapılandırılmış küçük veya varsayılan `SizeLimit`.
* Yanıt göre alınabilir [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimleri. Örneğin, `no-store` yönergesi istek veya yanıt üstbilgi alanları mevcut olmalıdır. Bkz: *bölüm 3: önbellekleri depolama yanıtlarında* , [RFC 7234](https://tools.ietf.org/html/rfc7234) Ayrıntılar için.

> [!NOTE]
> Siteler arası istek sahteciliği (CSRF) önlemek için güvenli belirteçleri oluşturmak için Antiforgery sistem kümeleri saldırıları `Cache-Control` ve `Pragma` üstbilgileri `no-cache` böylece önbelleğe alınan yanıtları değil.

## <a name="additional-resources"></a>Ek kaynaklar

* [Uygulama Başlatma](xref:fundamentals/startup)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıtları önbelleğe alma](xref:performance/caching/response)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
