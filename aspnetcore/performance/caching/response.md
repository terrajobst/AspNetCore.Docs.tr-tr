---
title: ASP.NET Core 'de yanıt önbelleğe alma
author: rick-anderson
description: Bant genişliği gereksinimlerini düşürmek ve ASP.NET Core uygulamalarının performansını artırmak için yanıt önbelleğe almayı nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/15/2019
uid: performance/caching/response
ms.openlocfilehash: 4ebac97689347245d25e0954b33729d78dd1b516
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378828"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core 'de yanıt önbelleğe alma

[John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/)ve [Luke Latham](https://github.com/guardrex) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Yanıt önbelleğe alma, bir istemcinin veya proxy 'nin bir Web sunucusunda yaptığı istek sayısını azaltır. Yanıt önbelleği Ayrıca, Web sunucusunun yanıt oluşturmak için gerçekleştirdiği iş miktarını azaltır. Yanıt önbelleğe alma, istemci, ara sunucu ve ara yazılım 'nin yanıtları nasıl önbellekte istediğinizi belirten üstbilgiler tarafından denetlenir.

[Responsecache özniteliği](#responsecache-attribute) , yanıtları önbelleğe alırken istemcilerin kabul olabileceği yanıt önbelleği üst bilgilerini ayarlamaya katılır. [Yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware) , sunucudaki yanıtları önbelleğe almak için kullanılabilir. Ara yazılım, sunucu tarafı önbelleğe alma davranışını etkilemek için <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> özelliklerini kullanabilir.

## <a name="http-based-response-caching"></a>HTTP tabanlı yanıt önbelleğe alma

[HTTP 1,1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234) , Internet önbelleklerinin nasıl davranması gerektiğini açıklar. Önbelleğe alma için kullanılan birincil HTTP üst bilgisi cache [-Control](https://tools.ietf.org/html/rfc7234#section-5.2)' dır, önbellek *yönergeleri*belirtmek için kullanılır. Yönergeler denetim önbelleği, istek olarak önbelleğe alma davranışını istemcilerden sunuculara ve yanıt olarak sunuculardan istemcilere geri doğru bir şekilde getirir. İstekler ve yanıtlar proxy sunucuları üzerinden taşınır ve proxy sunucuları da HTTP 1,1 önbelleğe alma belirtimine uymalıdır.

Ortak `Cache-Control` yönergeleri aşağıdaki tabloda gösterilmiştir.

| Deki                                                       | Eylem |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Önbellek, yanıtı saklayabilir. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Yanıtın paylaşılan bir önbellek tarafından depolanması gerekir. Özel bir önbellek, yanıtı depolayıp yeniden kullanabilir. |
| [Maksimum yaş](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | İstemci, yaşı belirtilen saniyeden daha büyük olan bir yanıtı kabul etmez. Örnekler: `max-age=60` (60 saniye), `max-age=2592000` (1 ay) |
| [önbellek yok](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **İsteklerde**: bir önbellek, isteği karşılamak için depolanan bir yanıt kullanmamalıdır. Kaynak sunucu, istemcinin yanıtını yeniden oluşturur ve ara yazılım, depolanan yanıtı önbelleğinde güncelleştirir.<br><br>**Yanıtlar**: yanıt, kaynak sunucuda doğrulamadan sonraki bir istek için kullanılmamalıdır. |
| [mağaza yok](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **Istekler üzerinde**: bir önbelleğin isteği depolaması gerekir.<br><br>**Yanıtlar**: bir önbellek, yanıtın herhangi bir parçasını depolamamalıdır. |

Önbelleğe alma işleminde bir rol oynatacak diğer önbellek üstbilgileri aşağıdaki tabloda gösterilmiştir.

| Üstbilgi                                                     | İşlev |
| ---------------------------------------------------------- | -------- |
| [Ölçerin](https://tools.ietf.org/html/rfc7234#section-5.1)     | Yanıt oluşturulduktan veya kaynak sunucuda başarıyla doğrulandıktan sonra geçen sürenin saniye cinsinden tahmini. |
| [Bitiminden](https://tools.ietf.org/html/rfc7234#section-5.3) | Yanıtın eski kabul edildiği zaman. |
| [Prag](https://tools.ietf.org/html/rfc7234#section-5.4)  | @No__t-0 davranışının ayarlanmasına yönelik HTTP/1.0 önbellekler ile geriye dönük uyumluluk için mevcuttur. @No__t-0 üstbilgisi varsa, `Pragma` üst bilgisi yok sayılır. |
| [Değiş](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | @No__t-0 üst bilgi alanları, hem önbelleğe alınan yanıtın özgün isteğinde hem de yeni istekte eşleşmediğinden, önbelleğe alınmış bir yanıtın gönderilmemesi gerektiğini belirtir. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>HTTP tabanlı önbelleğe alma istek önbelleği-denetim yönergeleri

[Cache-Control üst bilgisi Için HTTP 1,1 önbelleğe alma belirtiminin](https://tools.ietf.org/html/rfc7234#section-5.2) , istemci tarafından gönderilen geçerli bir `Cache-Control` üst bilgisini kabul etmek için önbellek gerekir. İstemci, `no-cache` üst bilgi değeri ile istek yapabilir ve sunucuyu her istek için yeni bir yanıt oluşturmaya zorlayabilir.

İstemci @no__t her zaman değerlendirin-0 istek üst bilgileri, HTTP önbelleği hedefini düşünüyorsanız anlamlı hale gelir. Resmi belirtim altında, önbelleğe alma, istemcilerin, proxy 'lerin ve sunucuların bir ağı üzerinde istekleri karşılayan gecikme süresini ve ağ yükünü azaltmaya yöneliktir. Kaynak sunucu üzerindeki yükü denetlemek için bir yol değildir.

Yazılım, resmi önbellek belirtimine bağlı olduğundan, [yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware) kullanılırken bu önbelleğe alma davranışının üzerinde geliştirici denetimi yoktur. [Ara yazılım Için planlı geliştirmeler](https://github.com/aspnet/AspNetCore/issues/2612) , önbelleğe alınmış bir yanıta sunmaya karar verirken bir isteğin `Cache-Control` üst bilgisini yoksayacak şekilde yapılandırmak için bir fırsattır. Planlı geliştirmeler sunucu yükünü daha iyi denetleme olanağı sağlar.

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core diğer önbelleğe alma teknolojisi

### <a name="in-memory-caching"></a>Bellek içi önbelleğe alma

Bellek içi önbelleğe alma, önbelleğe alınmış verileri depolamak için sunucu belleğini kullanır. Bu tür bir önbelleğe alma, *yapışkan oturumları*kullanan tek bir sunucu veya birden çok sunucu için uygundur. Yapışkan oturumlar, bir istemciden gelen isteklerin işlenmek üzere her zaman aynı sunucuya yönlendirildiği anlamına gelir.

Daha fazla bilgi için bkz. <xref:performance/caching/memory>.

### <a name="distributed-cache"></a>Dağıtılmış Önbellek

Uygulama bir bulutta veya sunucu grubunda barındırıldığı zaman bellekte verileri depolamak için dağıtılmış önbellek kullanın. Önbellek, istekleri işleyen sunucular arasında paylaşılır. İstemci, önbelleğe alınmış veriler varsa, gruptaki herhangi bir sunucu tarafından işlenen bir istek gönderebilir. ASP.NET Core, SQL Server ve Redsıs dağıtılmış önbellekler sunmaktadır.

Daha fazla bilgi için bkz. <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Önbellek etiketi Yardımcısı

İçeriği bir MVC görünümü veya Razor sayfasından önbellek etiketi Yardımcısı ile önbelleğe alma. Önbellek etiketi Yardımcısı, verileri depolamak için bellek içi önbelleğe alma kullanır.

Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.

### <a name="distributed-cache-tag-helper"></a>Dağıtılmış önbellek etiketi Yardımcısı

Dağıtılmış bulut etiketi Yardımcısı ile dağıtılmış bulutta veya Web grubu senaryolarında bir MVC görünümü veya Razor sayfasından içerik önbelleğe alma. Dağıtılmış önbellek etiketi Yardımcısı, verileri depolamak için SQL Server veya Redsıs kullanır.

Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.

## <a name="responsecache-attribute"></a>ResponseCache özniteliği

@No__t-0, yanıt önbelleğe alma işleminde uygun üst bilgileri ayarlamak için gereken parametreleri belirtir.

> [!WARNING]
> Kimliği doğrulanmış istemciler için bilgi içeren içerik için önbelleğe almayı devre dışı bırakın. Önbelleğe alma yalnızca kullanıcının kimliğine göre değişmeyen içerik veya bir kullanıcının oturum açmış olup olmadığı için etkinleştirilmelidir.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>, saklı yanıtı belirtilen sorgu anahtarları listesinin değerlerine göre değiştirir. Tek bir `*` değeri sağlandığında, ara yazılım tüm istek sorgu dizesi parametrelerine göre yanıtları değiştirir.

@No__t-1 özelliğini ayarlamak için [yanıt önbelleğe alma ara yazılımı](xref:performance/caching/middleware) etkinleştirilmelidir. Aksi takdirde, çalışma zamanı özel durumu oluşturulur. @No__t-0 özelliği için karşılık gelen bir HTTP üst bilgisi yok. Özelliği, yanıt önbelleğe alma ara yazılımı tarafından işlenen bir HTTP özelliğidir. Ara yazılım, önbelleğe alınmış bir yanıta hizmeti sağlamak için sorgu dizesi ve sorgu dizesi değerinin önceki bir istekle eşleşmesi gerekir. Örneğin, aşağıdaki tabloda gösterilen istek sırasını ve sonuçları göz önünde bulundurun.

| İstek                          | Sonuç                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | Sunucudan döndürüldü. |
| `http://example.com?key1=value1` | Ara yazılım tarafından döndürüldü. |
| `http://example.com?key1=value2` | Sunucudan döndürüldü. |

İlk istek sunucu tarafından döndürülür ve ara yazılım içinde önbelleğe alınır. Sorgu dizesi önceki istekle eşleştiğinden, ikinci istek ara yazılım tarafından döndürülür. Sorgu dizesi değeri önceki bir istekle eşleşmediğinden, üçüncü istek ara yazılım önbelleğinde değil.

@No__t-0 `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter` ' ye (<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) yapılandırmak ve oluşturmak için kullanılır. @No__t-0, yanıtın uygun HTTP üstbilgilerini ve özelliklerini güncelleştirme işini gerçekleştirir. Filtre:

* @No__t-0, `Cache-Control` ve `Pragma` için varolan tüm üstbilgileri kaldırır.
* @No__t-0 ' da ayarlanan özelliklere göre uygun üstbilgileri yazar.
* @No__t-0 ayarlanmışsa yanıt önbelleğe alma HTTP özelliğini güncelleştirir.

### <a name="vary"></a>Değiş

Bu üst bilgi yalnızca <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> özelliği ayarlandığında yazılır. @No__t-0 özelliğinin değerine ayarlanan özellik. Aşağıdaki örnek <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> özelliğini kullanır:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

Örnek uygulamayı kullanarak, tarayıcının ağ araçlarıyla yanıt üst bilgilerini görüntüleyin. Aşağıdaki yanıt üst bilgileri Cache1 sayfa yanıtıyla gönderilir:

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore ve Location. None

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> diğer özelliklerin çoğunu geçersiz kılar. Bu özellik `true` olarak ayarlandığında, `Cache-Control` üstbilgisi `no-store` olarak ayarlanır. @No__t-0 `None` olarak ayarlandıysa:

* `Cache-Control` `no-store,no-cache` olarak ayarlanır.
* `Pragma` `no-cache` olarak ayarlanır.

@No__t-0 ise-1 @no__t ve <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> `None`, `Cache-Control` ve `Pragma` ' i `no-cache` olarak ayarlanır.

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> genellikle hata sayfaları için `true` olarak ayarlanır. Örnek uygulamadaki Cache2 sayfası, istemcinin yanıtı depolayamamasını sağlayan yanıt üst bilgileri oluşturur.

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

Örnek uygulama, aşağıdaki üst bilgileri içeren Cache2 sayfasını döndürür:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Konum ve süre

Önbelleğe almayı etkinleştirmek için <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> pozitif bir değere ayarlanmalıdır ve <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ' in `Any` (varsayılan) veya `Client` olması gerekir. Bu durumda, `Cache-Control` üstbilgisi, yanıtın @no__t tarafından izlenen konum değerine ayarlanır.

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ' ın `Any` ve `Client` seçenekleri sırasıyla `public` ve `private` ' in `Cache-Control` üst bilgi değerlerine dönüştürülür. Daha önce belirtildiği gibi, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> olarak `None` ayarı, hem `Cache-Control` hem de `Pragma` üst bilgilerini `no-cache` ' e ayarlar.

Aşağıdaki örnek, örnek uygulamadan Cache3 sayfa modelini ve <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> ayarlanarak oluşturulan üstbilgileri ve varsayılan <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> değerini bırakarak gösterir:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

Örnek uygulama, aşağıdaki üst bilgiyle Cache3 sayfasını döndürür:

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>Önbellek profilleri

Birçok denetleyici eylem özniteliği üzerinde yanıt önbelleği ayarlarını çoğaltmak yerine, önbellek profilleri `Startup.ConfigureServices` ' da MVC/Razor Pages ayarlanırken seçenek olarak yapılandırılabilir. Başvurulan bir önbellek profilinde bulunan değerler <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> tarafından varsayılanlar olarak kullanılır ve özniteliğinde belirtilen özellikler tarafından geçersiz kılınır.

Önbellek profili ayarlayın. Aşağıdaki örnek, örnek uygulamanın @no__t (0) bir 30 saniyelik önbellek profilini göstermektedir:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

Örnek uygulamanın Cache4 sayfa modeli `Default30` önbellek profiline başvurur:

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

@No__t-0 ' a uygulanabilir:

* Razor sayfa işleyicileri (sınıflar) &ndash; öznitelikleri işleyici yöntemlerine uygulanamaz.
* MVC denetleyicileri (sınıflar).
* MVC eylemleri (Yöntemler) &ndash; Yöntem düzeyi öznitelikler, sınıf düzeyi özniteliklerde belirtilen ayarları geçersiz kılar.

@No__t-0 önbellek profili tarafından Cache4 sayfa yanıtına uygulanan sonuç üst bilgisi:

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Yanıtları önbellekte depolama](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
