---
title: "Yanıt Ara yazılımında ASP.NET çekirdek önbelleğe alma"
author: guardrex
description: "Yapılandırma ve ASP.NET Core uygulamaları yanıt önbelleğe alma Ara kullanma öğrenin."
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: f3312d0c333b47169c71891eea79f03be0abcfa3
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Yanıt Ara yazılımında ASP.NET çekirdek önbelleğe alma

Tarafından [Luke Latham](https://github.com/guardrex) ve [John Luo](https://github.com/JunTaoLuo)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu belge, ASP.NET Core uygulamaları yanıt önbelleğe alma ara yazılım yapılandırma hakkında ayrıntılar sağlar. Ara yazılım yanıtları alınabilir olduğunda, depoları yanıtlar ve görevi görür yanıtları önbelleğinden belirler. HTTP önbellek giriş ve `ResponseCache` özniteliği için bkz: [yanıt önbelleğe alma](response.md).

## <a name="package"></a>Paket
Ara yazılım bir proje eklemek için bir başvuru ekleyin [ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paketini veya kullanmak [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paket.

## <a name="configuration"></a>Yapılandırma
İçinde `ConfigureServices`, ara yazılım hizmeti koleksiyonuna ekleyin.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Ara yazılımla kullanmak için uygulamayı yapılandırma `UseResponseCaching` ara yazılım istek işleme ardışık düzenine ekler genişletme yöntemi. Örnek uygulaması ekler bir [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) alınabilir yanıtları 10 saniye için önbelleğe yanıtı üstbilgisi. Örnek gönderir bir [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) önbelleğe alınan yanıt eksikse sunmak için ara yazılımını yapılandırma üstbilgi [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) sonraki istekleri üstbilgisinin, özgün istek eşleşir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]

---

Yanıt önbelleğe alma ara yazılımı yalnızca 200 (Tamam) sunucu yanıtlarını önbelleğe alır. Dahil olmak üzere diğer bir yanıtlar [hata sayfaları](xref:fundamentals/error-handling), ara yazılım tarafından göz ardı edilir.

> [!WARNING]
> Kimliği doğrulanmış istemciler için içeriğin bulunduğu yanıtları depolamak ve bu yanıtlara hizmet ara yazılım önlemek için alınabilir olarak işaretlenmesi gerekir. Bkz: [önbelleğe alma için koşulları](#conditions-for-caching) ilişkin ayrıntılar nasıl ara yazılım bir yanıt alınabilir olup olmadığını belirler.

## <a name="options"></a>Seçenekler
Ara yazılım denetleme yanıt önbelleğe alma için üç seçenek sunar.

| Seçenek                | Varsayılan Değer |
| --------------------- | ------------- |
| UseCaseSensitivePaths | Büyük küçük harfe duyarlı yollarında yanıtlarını önbelleğe alınmaz belirler.</p><p>Varsayılan değer `false` şeklindedir. |
| MaximumBodySize       | Yanıt gövdesi bayt cinsinden en büyük alınabilir boyutu.</p>Varsayılan değer `64 * 1024 * 1024` (64 MB). |
| SizeLimit             | Bayt cinsinden yanıt önbellek ara yazılımı için boyut sınırı. Varsayılan değer `100 * 1024 * 1024` (100 MB). |

Aşağıdaki örnek önbellek yanıtları değerinden küçük veya 1.024 yanıtlarını depolamak büyük küçük harfe duyarlı yollar kullanılarak bayta eşit Ara yapılandırır `/page1` ve `/Page1` ayrı olarak.

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys
MVC, kullanırken `ResponseCache` özniteliği yanıt önbelleğe alma için uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir. Yalnızca parametresinin `ResponseCache` kesinlikle ara yazılımı gerektirir özniteliği `VaryByQueryKeys`, hangi gerçek bir HTTP üstbilgisi karşılık değil. Daha fazla bilgi için bkz: [ResponseCache özniteliği](response.md#responsecache-attribute).

MVC kullanmadığınızda, yanıt önbelleğe almayı değişebilir `VaryByQueryKeys` özelliği. Kullanım `ResponseCachingFeature` doğrudan `IFeatureCollection` , `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>Yanıt önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri
Ara yazılım tarafından önbelleğe alma yanıt HTTP üstbilgileri yapılandırılır. İlgili üstbilgileri önbelleğe alma nasıl etkilediklerini üzerinde notları birlikte aşağıda listelenmiştir.

| Üstbilgi | Ayrıntılar |
| ------ | ------- |
| Yetkilendirme | Yanıt üstbilgisi mevcutsa önbelleğe değil. |
| Cache-Control | İle işaretlenen yanıtlarını önbelleğe alma ara yazılımı yalnızca dikkate `public` önbellek yönergesi. Aşağıdaki parametrelerle önbelleğe alma denetleyebilirsiniz:<ul><li>Maksimum yaş</li><li>max eski &#8224;</li><li>Min-yeni</li><li>gereken revalidate</li><li>Hayır-önbellek</li><li>Hayır deposu</li><li>yalnızca IF-önbelleğe alma</li><li>private</li><li>public</li><li>s-maxage</li><li>Proxy-revalidate &#8225;</li></ul>& # için sınır belirtilmişse 8224; `max-stale`, ara yazılımın herhangi bir eylemi alır.<br>&#8225; `proxy-revalidate` aynı etkiye sahip `must-revalidate`.<br><br>Daha fazla bilgi için bkz: [RFC 7231: İstek önbellek denetimi yönergelerini](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Pragma | A `Pragma: no-cache` istek üstbilgisinde üreten aynı etkiye `Cache-Control: no-cache`. Bu üst ilgili yönergeleri tarafından geçersiz kılınır `Cache-Control` üstbilgisi, varsa. HTTP/1.0 ile geriye dönük uyumluluk için kabul. |
| Tanımlama bilgisi-Ayarla | Yanıt üstbilgisi mevcutsa önbelleğe değil. |
| değişir | `Vary` Üstbilgisi önbelleğe alınan yanıtın farklılık için kullanılan başka bir üstbilgisi tarafından. Örneğin, ekleyerek kodlayarak yanıtlarını önbelleğe alabilir `Vary: Accept-Encoding` üstbilgileri olan istekler için yanıtlar önbelleğe alır, üst `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı olarak. Bir yanıt üstbilgisi değerini `*` hiçbir zaman depolanır. |
| Süre sonu | Bu üstbilgisi tarafından eski kabul yanıt değil veya depolanan diğer tarafından geçersiz kılınmadığı sürece alınan `Cache-Control` üstbilgileri. |
| If-None-Match | Değer yoksa tam yanıtı önbelleğinden sunulan `*` ve `ETag` yanıtını sağlanan değerlerden herhangi birini eşleşmiyor. Aksi takdirde 304 (değişiklik) yanıt sunulur. |
| If-Modified-Since | Varsa `If-None-Match` üstbilgisi mevcut değilse, önbelleğe alınan yanıt tarih sağlanan değerden daha yeniyse tam yanıtı önbelleğinden sunulur. Aksi takdirde 304 (değişiklik) yanıt sunulur. |
| Tarih | Önbelleğe alınan hizmet veren zaman `Date` üstbilgi ayarlanmışsa ara yazılım tarafından özgün yanıtta sağlanmadı. |
| İçerik Uzunluğu | Önbelleğe alınan hizmet veren zaman `Content-Length` üstbilgi ayarlanmışsa ara yazılım tarafından özgün yanıtta sağlanmadı. |
| geçerlilik süresi | `Age` Özgün yanıt olarak gönderilen üstbilgisi göz ardı edilir. Ara yazılım, önbelleğe alınan yanıt hizmet veren zaman yeni bir değer hesaplar. |

## <a name="caching-respects-request-cache-control-directives"></a>Önbelleğe alma isteği Cache-Control yönergeleri dikkate alır

Ara yazılım kurallarına uyar [HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2). Geçerli bir vermenizin bir önbellek kuralları gerektiren `Cache-Control` istemci tarafından gönderilen üstbilgisi. Altında belirtimi, istemci istekleriyle yapabilirsiniz bir `no-cache` üstbilgi değeri ve zorla her istek için yeni bir yanıt oluşturmak için bir sunucu. Şu anda, ara yazılım resmi önbelleğe alma belirtimi aynılarını ara yazılım kullanırken bu önbelleğe alma davranışını Geliştirici denetleyemez yoktur.

[Ara yazılım gelecekteki geliştirmeler](https://github.com/aspnet/ResponseCaching/issues/96) senaryoları önbelleğe alma için ara yazılım yapılandırma izin verecek burada isteği `Cache-Control` üstbilgi dikkate önbelleğe alınan yanıt hizmet verirken. Önbelleğe alma davranışı üzerinde daha fazla denetim aradığınızda, ASP.NET Core diğer önbelleğe alma özelliklerini keşfedin. Aşağıdaki konulara bakın:

* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [ASP.NET Core MVC etiketi yardımcı önbelleğe alma](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>Sorun giderme
Beklediğiniz gibi önbelleğe alma davranışı değilse, yanıtları alınabilir ve önbellekten isteğin gelen üstbilgileri ve yanıtın giden üstbilgileri inceleyerek sunulmasını, özelliğine sahip olduğundan emin olun. Etkinleştirme [günlüğü](xref:fundamentals/logging/index) hata ayıklama sırasında yardımcı olur. Ara yazılım davranışını ve ne zaman bir yanıt önbellekten alınır önbelleğe alma günlüğe kaydedilir.

Sınama ve önbelleğe alma davranışını sorun giderme, bir tarayıcı istenmeyen şekilde önbelleğe almayı etkileyen istek üstbilgilerini Ayarla. Örneğin, bir tarayıcı ayarlayabilir `Cache-Control` başlığına `no-cache` sayfa yenilediğinizde. Aşağıdaki araçlar, istek üstbilgileri açıkça ayarlayabilir ve önbelleğe almayı test etmek için tercih edilir:

* [Fiddler](http://www.telerik.com/fiddler)
* [Firebug](http://getfirebug.com/)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Önbelleğe alma için koşullar
* İstek sunucusundan 200 (Tamam) yanıt neden gerekir.
* İstek yöntemini GET veya HEAD olması gerekir.
* Statik dosya ara yazılımlarını gibi Terminal ara yazılımın yanıt önbelleğe alma yanıt Ara önce işlem gerekir.
* `Authorization` Üstbilgisi mevcut olmamalıdır.
* `Cache-Control`Üstbilgi parametreleri geçerli olmalıdır ve yanıt işaretlenmelidir `public` ve işaretlenmemiş `private`.
* `Pragma: no-cache` Üstbilgi/değer mevcut olmamalıdır, `Cache-Control` üstbilgisi olarak mevcut değil `Cache-Control` üstbilgisi geçersiz kılar `Pragma` üstbilgi varsa.
* `Set-Cookie` Üstbilgisi mevcut olmamalıdır.
* `Vary`Üstbilgi parametreleri geçerli ve eşit değil olmalıdır `*`.
* `Content-Length` Üstbilgi değeri (varsa ayarlayın) yanıt gövdesi boyutu ile eşleşmelidir.
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) kullanılmaz.
* Yanıt belirtildiği gibi eski olmamalıdır `Expires` üstbilgi ve `max-age` ve `s-maxage` önbelleğe yönergeleri.
* Yanıt arabelleğe alma başarılı olur ve yanıt boyutunun yapılandırılmış küçük veya varsayılan `SizeLimit`.
* Yanıt göre alınabilir [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimleri. Örneğin, `no-store` yönergesi istek veya yanıt üstbilgi alanları mevcut olmalıdır. Bkz: *bölüm 3: önbellekleri depolama yanıtlarında* , [RFC 7234](https://tools.ietf.org/html/rfc7234) Ayrıntılar için.

> [!NOTE]
> Siteler arası istek sahteciliği (CSRF) önlemek için güvenli belirteçleri oluşturmak için Antiforgery sistem kümeleri saldırıları `Cache-Control` ve `Pragma` üstbilgileri `no-cache` böylece önbelleğe alınan yanıtları değil.

## <a name="additional-resources"></a>Ek kaynaklar

* [Uygulama başlatma](xref:fundamentals/startup)
* [Ara yazılım](xref:fundamentals/middleware)
* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıt önbelleğe alma](xref:performance/caching/response)
* [Önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
