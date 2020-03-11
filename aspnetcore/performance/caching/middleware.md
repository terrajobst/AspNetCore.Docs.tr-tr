---
title: ASP.NET Core 'de yanıt önbelleğe alma ara yazılımı
author: rick-anderson
description: ASP.NET Core 'de yanıt önbelleğe alma ara yazılımını yapılandırmayı ve kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/caching/middleware
ms.openlocfilehash: 4deac15538d4607bd611c4e072daae39447681c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655738"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>ASP.NET Core 'de yanıt önbelleğe alma ara yazılımı

[John Luo](https://github.com/JunTaoLuo) tarafından

::: moniker range=">= aspnetcore-3.0"

Bu makalede, yanıt önbelleğe alma ara yazılımı ASP.NET Core bir uygulamada nasıl yapılandırılacağı açıklanmaktadır. Ara yazılım, yanıtların önbelleklenebilir olup olmadığını belirler, yanıtları depolar ve önbellekten yanıt verir. HTTP önbelleği ve [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliğine giriş için bkz. [Yanıt önbelleği](xref:performance/caching/response).

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>Yapılandırma

Yanıt önbelleğe alma ara yazılımı, paylaşılan Framework aracılığıyla ASP.NET Core uygulamalar için örtülü olarak kullanılabilir.

`Startup.ConfigureServices`, yanıt önbelleğe alma ara yazılımını hizmet koleksiyonuna ekleyin:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Uygulamayı, `Startup.Configure`içindeki istek işleme ardışık düzenine ekleyen <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> uzantısı yöntemiyle ara yazılımı kullanacak şekilde yapılandırın:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

Örnek uygulama, sonraki isteklerde önbelleğe almayı denetlemek için üstbilgiler ekler:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; önbelleklenebilir yanıtları 10 saniyeye kadar önbelleğe alır.
* [Değişiklik](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash;, ara yazılımı yalnızca sonraki Isteklerin [kabul etme-kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4) üst bilgisi özgün istekten eşleşiyorsa, önbelleğe alınmış bir yanıta sunacak şekilde yapılandırır.

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

Yanıt önbelleğe alma ara yazılımı yalnızca 200 (Tamam) durum kodu ile sonuçlanan sunucu yanıtlarını önbelleğe alır. [Hata sayfaları](xref:fundamentals/error-handling)dahil diğer tüm yanıtlar, ara yazılım tarafından yok sayılır.

> [!WARNING]
> Kimliği doğrulanmış istemciler için içerik içeren yanıtların, ara yazılımın bu yanıtları depolamasını ve hizmet vermek için önbelleğe alınamaz olarak işaretlenmesi gerekir. Bir yanıtın önbelleklenmesini nasıl belirlediği hakkında bilgi için bkz. [önbelleğe alma koşulları](#conditions-for-caching) .

## <a name="options"></a>Seçenekler

Yanıt önbelleğe alma seçenekleri aşağıdaki tabloda gösterilmiştir.

| Seçenek | Açıklama |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Yanıt gövdesi için bayt cinsinden en büyük önbelleklenebilir boyut. Varsayılan değer `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Yanıt önbelleği ara yazılımı için bayt cinsinden boyut sınırı. Varsayılan değer `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Yanıtların büyük/küçük harfe duyarlı yollarda önbelleğe alınıp alınmayacağını belirler. Varsayılan değer: `false`. |

Aşağıdaki örnek, şu şekilde bir ara yazılım yapılandırır:

* Gövde boyutu 1.024 bayttan küçük veya buna eşit olan önbellek yanıtları.
* Yanıtları büyük/küçük harfe duyarlı yollarla depolayın. Örneğin, `/page1` ve `/Page1` ayrı olarak depolanır.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

MVC/web API denetleyicilerini veya Razor Pages sayfa modellerini kullanırken, [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliği yanıt önbelleğe alma için uygun üst bilgileri ayarlamak için gereken parametreleri belirtir. Ara yazılımı kesinlikle gerektiren `[ResponseCache]` özniteliğinin tek parametresi, gerçek bir HTTP üst bilgisine karşılık gelen <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>. Daha fazla bilgi için bkz. <xref:performance/caching/response#responsecache-attribute>.

`[ResponseCache]` özniteliği kullanılırken, yanıt önbelleğe alma `VaryByQueryKeys`değiştirilebilir. <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> doğrudan [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features)içinden kullanın:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

`VaryByQueryKeys` `*` eşit tek bir değer kullanmak, tüm istek sorgu parametrelerine göre önbelleği değiştirir.

## <a name="http-headers-used-by-response-caching-middleware"></a>Yanıt önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri

Aşağıdaki tabloda, yanıt önbelleğini etkileyen HTTP üstbilgileri hakkında bilgi verilmektedir.

| Üst bilgi | Ayrıntılar |
| ------ | ------- |
| `Authorization` | Üst bilgi varsa yanıt önbelleğe alınmaz. |
| `Cache-Control` | Ara yazılım yalnızca `public` cache yönergesi ile işaretlenmiş önbelleğe alma yanıtlarını dikkate alır. Aşağıdaki parametrelerle önbelleğe alma denetimi:<ul><li>Maksimum yaş</li><li>en fazla-eski&#8224;</li><li>en az-yeni</li><li>yeniden doğrulama gerekir</li><li>önbellek yok</li><li>mağaza yok</li><li>yalnızca-if-önbelleğe alındı</li><li>özel</li><li>{1&gt;public&lt;1}</li><li>s-maxage</li><li>ara sunucu-yeniden doğrulama&#8225;</li></ul>&#8224;`max-stale`için hiçbir sınır belirtilmemişse, ara yazılım hiçbir işlem gerçekleşmez.<br>&#8225;`proxy-revalidate` `must-revalidate`ile aynı etkiye sahiptir.<br><br>Daha fazla bilgi için bkz. [RFC 7231: Istek Cache-Control yönergeleri](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | İstekteki bir `Pragma: no-cache` üst bilgisi `Cache-Control: no-cache`ile aynı etkiyi üretir. Bu üst bilgi, varsa `Cache-Control` üst bilgisinde ilgili yönergeler tarafından geçersiz kılınır. HTTP/1.0 ile geriye dönük uyumluluk için değerlendirilir. |
| `Set-Cookie` | Üst bilgi varsa yanıt önbelleğe alınmaz. İstek işleme ardışık düzeninde bir veya daha fazla tanımlama bilgisi ayarlayan herhangi bir ara yazılım, yanıt önbelleğe alma ara hattının yanıtı önbelleğe almasını önler (örneğin, [tanımlama bilgisi tabanlı TempData sağlayıcısı](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | `Vary` üstbilgisi, başka bir üst bilgi tarafından önbelleğe alınan yanıtı değiştirmek için kullanılır. Örneğin, üst bilgileri `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı olarak istekler için yanıtları önbelleğe alan `Vary: Accept-Encoding` üst bilgisini ekleyerek, kodlamaya göre yanıtları önbelleğe alır. `*` üstbilgi değeri olan bir yanıt hiçbir şekilde depolanmaz. |
| `Expires` | Bu üst bilgi tarafından kabul edilen bir yanıt, diğer `Cache-Control` üstbilgileri tarafından geçersiz kılınmadıkça depolanmaz veya alınamaz. |
| `If-None-Match` | Tam yanıt, değerin `*` olmaması ve yanıtın `ETag` verilen değerlerden hiçbiriyle eşleşmezse önbellekten sunulur. Aksi takdirde, 304 (değiştirilmez) yanıtı sunulur. |
| `If-Modified-Since` | `If-None-Match` üstbilgisi yoksa, önbelleğe alınmış yanıt tarihi verilen değerden daha yeniyse önbellekten tam bir yanıt sunulur. Aksi takdirde, *304 olarak değiştirilmemiş* bir yanıt sunulur. |
| `Date` | Önbellekten hizmet verirken, özgün yanıtta sağlanmadıysa `Date` üstbilgisi ara yazılım tarafından ayarlanır. |
| `Content-Length` | Önbellekten hizmet verirken, özgün yanıtta sağlanmadıysa `Content-Length` üstbilgisi ara yazılım tarafından ayarlanır. |
| `Age` | Özgün yanıtta gönderilen `Age` üst bilgisi yok sayılır. Ara yazılım, önbelleğe alınmış bir yanıta hizmet verirken yeni bir değeri hesaplar. |

## <a name="caching-respects-request-cache-control-directives"></a>İstekleri önbelleğe alma isteği Cache-Control yönergeleri

Ara yazılım, [HTTP 1,1 önbelleğe alma belirtiminin](https://tools.ietf.org/html/rfc7234#section-5.2)kurallarına uyar. Kurallar, istemci tarafından gönderilen geçerli bir `Cache-Control` üst bilgisini kabul etmek için bir önbellek gerektirir. Belirtim altında istemci, `no-cache` üst bilgi değeri ile istek yapabilir ve sunucuyu her istek için yeni bir yanıt oluşturmaya zorlayabilir. Şu anda, ara yazılım resmi önbelleğe alma belirtimine bağlı olduğundan, ara yazılım kullanılırken bu önbelleğe alma davranışı üzerinde geliştirici denetimi yoktur.

Önbelleğe alma davranışı üzerinde daha fazla denetim için ASP.NET Core diğer önbelleğe alma özelliklerine göz atın. Aşağıdaki konulara bakın:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Sorun giderme

Önbelleğe alma davranışı beklenmiyorsa, yanıtların önbelleklenmesini ve önbellekten sunulduğunu doğrulayın. İsteğin gelen üst bilgilerini ve yanıtın giden üst bilgilerini inceleyin. Hata ayıklamaya yardımcı olmak için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

Önbelleğe alma davranışını test ederken ve sorun giderirken, bir tarayıcı, istenmeyen yollarla önbelleğe almayı etkileyen istek üst bilgilerini ayarlayabilir. Örneğin, bir tarayıcı, bir sayfa yenilenirken `Cache-Control` üst bilgisini `no-cache` veya `max-age=0` olarak ayarlayabilir. Aşağıdaki araçlar, istek üst bilgilerini açık bir şekilde ayarlayabilir ve önbelleğe alma testi için tercih edilir:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Önbelleğe alma koşulları

* İstek, 200 (Tamam) durum kodu ile bir sunucu yanıtı ile sonuçlanmalıdır.
* İstek yöntemi GET veya HEAD olmalıdır.
* `Startup.Configure`, yanıt önbelleği ara yazılımı, önbelleğe alma gerektiren ara yazılımlar için yerleştirilmelidir. Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.
* `Authorization` üst bilgisi mevcut olmamalıdır.
* `Cache-Control` üst bilgi parametreleri geçerli olmalıdır ve yanıt `public` olarak işaretlenmelidir ve `private`işaretlenmemiş olmalıdır.
* `Pragma: no-cache` üst bilgisi mevcut olmadığında, `Cache-Control` üst bilgisi mevcut olduğunda `Pragma` üst bilgisini geçersiz kılıyorsa, `Cache-Control` üstbilgisi mevcut olmamalıdır.
* `Set-Cookie` üst bilgisi mevcut olmamalıdır.
* `Vary` üst bilgi parametreleri geçerli ve `*`eşit olmalıdır.
* `Content-Length` üst bilgi değeri (ayarlandıysa), yanıt gövdesinin boyutuyla aynı olmalıdır.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> kullanılmıyor.
* Yanıt, `Expires` üst bilgisi ve `max-age` ve `s-maxage` önbellek yönergeleri tarafından belirtilen eski olmamalıdır.
* Yanıt arabelleğe alma başarılı olmalıdır. Yanıtın boyutu yapılandırılmış veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>daha küçük olmalıdır. Yanıtın gövde boyutu yapılandırılmış veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>küçük olmalıdır.
* Yanıt, [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimlerine göre önbelleklenebilir olmalıdır. Örneğin, istek veya yanıt üst bilgisi alanlarında `no-store` yönergesi bulunmamalıdır. Ayrıntılar için bkz. 3. Bölüm: [RFC 7234](https://tools.ietf.org/html/rfc7234) *önbelleklerinde yanıtları depolama* .

> [!NOTE]
> Siteler arası Istek sahteciliği (CSRF) saldırılarını önlemeye yönelik güvenli belirteçler oluşturmaya yönelik Antiforgery sistemi, `Cache-Control` ve `Pragma` üst bilgilerini `no-cache` olarak ayarlar ve böylece yanıtlar önbelleğe alınmaz. HTML form öğeleri için antiforgery belirteçlerini devre dışı bırakma hakkında daha fazla bilgi için bkz. <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu makalede, yanıt önbelleğe alma ara yazılımı ASP.NET Core bir uygulamada nasıl yapılandırılacağı açıklanmaktadır. Ara yazılım, yanıtların önbelleklenebilir olup olmadığını belirler, yanıtları depolar ve önbellekten yanıt verir. HTTP önbelleği ve [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliğine giriş için bkz. [Yanıt önbelleği](xref:performance/caching/response).

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>Yapılandırma

Microsoft. [aspnetcore. app metapackage](xref:fundamentals/metapackage-app) kullanın veya [Microsoft. Aspnetcore. responsecaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) paketine bir paket başvurusu ekleyin.

`Startup.ConfigureServices`, yanıt önbelleğe alma ara yazılımını hizmet koleksiyonuna ekleyin:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Uygulamayı, `Startup.Configure`içindeki istek işleme ardışık düzenine ekleyen <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> uzantısı yöntemiyle ara yazılımı kullanacak şekilde yapılandırın:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

Örnek uygulama, sonraki isteklerde önbelleğe almayı denetlemek için üstbilgiler ekler:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; önbelleklenebilir yanıtları 10 saniyeye kadar önbelleğe alır.
* [Değişiklik](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash;, ara yazılımı yalnızca sonraki Isteklerin [kabul etme-kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4) üst bilgisi özgün istekten eşleşiyorsa, önbelleğe alınmış bir yanıta sunacak şekilde yapılandırır.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

Yanıt önbelleğe alma ara yazılımı yalnızca 200 (Tamam) durum kodu ile sonuçlanan sunucu yanıtlarını önbelleğe alır. [Hata sayfaları](xref:fundamentals/error-handling)dahil diğer tüm yanıtlar, ara yazılım tarafından yok sayılır.

> [!WARNING]
> Kimliği doğrulanmış istemciler için içerik içeren yanıtların, ara yazılımın bu yanıtları depolamasını ve hizmet vermek için önbelleğe alınamaz olarak işaretlenmesi gerekir. Bir yanıtın önbelleklenmesini nasıl belirlediği hakkında bilgi için bkz. [önbelleğe alma koşulları](#conditions-for-caching) .

## <a name="options"></a>Seçenekler

Yanıt önbelleğe alma seçenekleri aşağıdaki tabloda gösterilmiştir.

| Seçenek | Açıklama |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Yanıt gövdesi için bayt cinsinden en büyük önbelleklenebilir boyut. Varsayılan değer `64 * 1024 * 1024` (64 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Yanıt önbelleği ara yazılımı için bayt cinsinden boyut sınırı. Varsayılan değer `100 * 1024 * 1024` (100 MB). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Yanıtların büyük/küçük harfe duyarlı yollarda önbelleğe alınıp alınmayacağını belirler. Varsayılan değer: `false`. |

Aşağıdaki örnek, şu şekilde bir ara yazılım yapılandırır:

* Gövde boyutu 1.024 bayttan küçük veya buna eşit olan önbellek yanıtları.
* Yanıtları büyük/küçük harfe duyarlı yollarla depolayın. Örneğin, `/page1` ve `/Page1` ayrı olarak depolanır.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

MVC/web API denetleyicilerini veya Razor Pages sayfa modellerini kullanırken, [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) özniteliği yanıt önbelleğe alma için uygun üst bilgileri ayarlamak için gereken parametreleri belirtir. Ara yazılımı kesinlikle gerektiren `[ResponseCache]` özniteliğinin tek parametresi, gerçek bir HTTP üst bilgisine karşılık gelen <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>. Daha fazla bilgi için bkz. <xref:performance/caching/response#responsecache-attribute>.

`[ResponseCache]` özniteliği kullanılırken, yanıt önbelleğe alma `VaryByQueryKeys`değiştirilebilir. <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> doğrudan [HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features)içinden kullanın:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

`VaryByQueryKeys` `*` eşit tek bir değer kullanmak, tüm istek sorgu parametrelerine göre önbelleği değiştirir.

## <a name="http-headers-used-by-response-caching-middleware"></a>Yanıt önbelleğe alma ara yazılımı tarafından kullanılan HTTP üstbilgileri

Aşağıdaki tabloda, yanıt önbelleğini etkileyen HTTP üstbilgileri hakkında bilgi verilmektedir.

| Üst bilgi | Ayrıntılar |
| ------ | ------- |
| `Authorization` | Üst bilgi varsa yanıt önbelleğe alınmaz. |
| `Cache-Control` | Ara yazılım yalnızca `public` cache yönergesi ile işaretlenmiş önbelleğe alma yanıtlarını dikkate alır. Aşağıdaki parametrelerle önbelleğe alma denetimi:<ul><li>Maksimum yaş</li><li>en fazla-eski&#8224;</li><li>en az-yeni</li><li>yeniden doğrulama gerekir</li><li>önbellek yok</li><li>mağaza yok</li><li>yalnızca-if-önbelleğe alındı</li><li>özel</li><li>{1&gt;public&lt;1}</li><li>s-maxage</li><li>ara sunucu-yeniden doğrulama&#8225;</li></ul>&#8224;`max-stale`için hiçbir sınır belirtilmemişse, ara yazılım hiçbir işlem gerçekleşmez.<br>&#8225;`proxy-revalidate` `must-revalidate`ile aynı etkiye sahiptir.<br><br>Daha fazla bilgi için bkz. [RFC 7231: Istek Cache-Control yönergeleri](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | İstekteki bir `Pragma: no-cache` üst bilgisi `Cache-Control: no-cache`ile aynı etkiyi üretir. Bu üst bilgi, varsa `Cache-Control` üst bilgisinde ilgili yönergeler tarafından geçersiz kılınır. HTTP/1.0 ile geriye dönük uyumluluk için değerlendirilir. |
| `Set-Cookie` | Üst bilgi varsa yanıt önbelleğe alınmaz. İstek işleme ardışık düzeninde bir veya daha fazla tanımlama bilgisi ayarlayan herhangi bir ara yazılım, yanıt önbelleğe alma ara hattının yanıtı önbelleğe almasını önler (örneğin, [tanımlama bilgisi tabanlı TempData sağlayıcısı](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | `Vary` üstbilgisi, başka bir üst bilgi tarafından önbelleğe alınan yanıtı değiştirmek için kullanılır. Örneğin, üst bilgileri `Accept-Encoding: gzip` ve `Accept-Encoding: text/plain` ayrı olarak istekler için yanıtları önbelleğe alan `Vary: Accept-Encoding` üst bilgisini ekleyerek, kodlamaya göre yanıtları önbelleğe alır. `*` üstbilgi değeri olan bir yanıt hiçbir şekilde depolanmaz. |
| `Expires` | Bu üst bilgi tarafından kabul edilen bir yanıt, diğer `Cache-Control` üstbilgileri tarafından geçersiz kılınmadıkça depolanmaz veya alınamaz. |
| `If-None-Match` | Tam yanıt, değerin `*` olmaması ve yanıtın `ETag` verilen değerlerden hiçbiriyle eşleşmezse önbellekten sunulur. Aksi takdirde, 304 (değiştirilmez) yanıtı sunulur. |
| `If-Modified-Since` | `If-None-Match` üstbilgisi yoksa, önbelleğe alınmış yanıt tarihi verilen değerden daha yeniyse önbellekten tam bir yanıt sunulur. Aksi takdirde, *304 olarak değiştirilmemiş* bir yanıt sunulur. |
| `Date` | Önbellekten hizmet verirken, özgün yanıtta sağlanmadıysa `Date` üstbilgisi ara yazılım tarafından ayarlanır. |
| `Content-Length` | Önbellekten hizmet verirken, özgün yanıtta sağlanmadıysa `Content-Length` üstbilgisi ara yazılım tarafından ayarlanır. |
| `Age` | Özgün yanıtta gönderilen `Age` üst bilgisi yok sayılır. Ara yazılım, önbelleğe alınmış bir yanıta hizmet verirken yeni bir değeri hesaplar. |

## <a name="caching-respects-request-cache-control-directives"></a>İstekleri önbelleğe alma isteği Cache-Control yönergeleri

Ara yazılım, [HTTP 1,1 önbelleğe alma belirtiminin](https://tools.ietf.org/html/rfc7234#section-5.2)kurallarına uyar. Kurallar, istemci tarafından gönderilen geçerli bir `Cache-Control` üst bilgisini kabul etmek için bir önbellek gerektirir. Belirtim altında istemci, `no-cache` üst bilgi değeri ile istek yapabilir ve sunucuyu her istek için yeni bir yanıt oluşturmaya zorlayabilir. Şu anda, ara yazılım resmi önbelleğe alma belirtimine bağlı olduğundan, ara yazılım kullanılırken bu önbelleğe alma davranışı üzerinde geliştirici denetimi yoktur.

Önbelleğe alma davranışı üzerinde daha fazla denetim için ASP.NET Core diğer önbelleğe alma özelliklerine göz atın. Aşağıdaki konulara bakın:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Sorun giderme

Önbelleğe alma davranışı beklenmiyorsa, yanıtların önbelleklenmesini ve önbellekten sunulduğunu doğrulayın. İsteğin gelen üst bilgilerini ve yanıtın giden üst bilgilerini inceleyin. Hata ayıklamaya yardımcı olmak için [günlük kaydını](xref:fundamentals/logging/index) etkinleştirin.

Önbelleğe alma davranışını test ederken ve sorun giderirken, bir tarayıcı, istenmeyen yollarla önbelleğe almayı etkileyen istek üst bilgilerini ayarlayabilir. Örneğin, bir tarayıcı, bir sayfa yenilenirken `Cache-Control` üst bilgisini `no-cache` veya `max-age=0` olarak ayarlayabilir. Aşağıdaki araçlar, istek üst bilgilerini açık bir şekilde ayarlayabilir ve önbelleğe alma testi için tercih edilir:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Önbelleğe alma koşulları

* İstek, 200 (Tamam) durum kodu ile bir sunucu yanıtı ile sonuçlanmalıdır.
* İstek yöntemi GET veya HEAD olmalıdır.
* `Startup.Configure`, yanıt önbelleği ara yazılımı, önbelleğe alma gerektiren ara yazılımlar için yerleştirilmelidir. Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.
* `Authorization` üst bilgisi mevcut olmamalıdır.
* `Cache-Control` üst bilgi parametreleri geçerli olmalıdır ve yanıt `public` olarak işaretlenmelidir ve `private`işaretlenmemiş olmalıdır.
* `Pragma: no-cache` üst bilgisi mevcut olmadığında, `Cache-Control` üst bilgisi mevcut olduğunda `Pragma` üst bilgisini geçersiz kılıyorsa, `Cache-Control` üstbilgisi mevcut olmamalıdır.
* `Set-Cookie` üst bilgisi mevcut olmamalıdır.
* `Vary` üst bilgi parametreleri geçerli ve `*`eşit olmalıdır.
* `Content-Length` üst bilgi değeri (ayarlandıysa), yanıt gövdesinin boyutuyla aynı olmalıdır.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> kullanılmıyor.
* Yanıt, `Expires` üst bilgisi ve `max-age` ve `s-maxage` önbellek yönergeleri tarafından belirtilen eski olmamalıdır.
* Yanıt arabelleğe alma başarılı olmalıdır. Yanıtın boyutu yapılandırılmış veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>daha küçük olmalıdır. Yanıtın gövde boyutu yapılandırılmış veya varsayılan <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>küçük olmalıdır.
* Yanıt, [RFC 7234](https://tools.ietf.org/html/rfc7234) belirtimlerine göre önbelleklenebilir olmalıdır. Örneğin, istek veya yanıt üst bilgisi alanlarında `no-store` yönergesi bulunmamalıdır. Ayrıntılar için bkz. 3. Bölüm: [RFC 7234](https://tools.ietf.org/html/rfc7234) *önbelleklerinde yanıtları depolama* .

> [!NOTE]
> Siteler arası Istek sahteciliği (CSRF) saldırılarını önlemeye yönelik güvenli belirteçler oluşturmaya yönelik Antiforgery sistemi, `Cache-Control` ve `Pragma` üst bilgilerini `no-cache` olarak ayarlar ve böylece yanıtlar önbelleğe alınmaz. HTML form öğeleri için antiforgery belirteçlerini devre dışı bırakma hakkında daha fazla bilgi için bkz. <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
