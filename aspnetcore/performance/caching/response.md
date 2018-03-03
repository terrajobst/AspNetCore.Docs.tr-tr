---
title: "ASP.NET Core yanıt önbelleğe alma"
author: rick-anderson
description: "Düşük bant genişliği gereksinimlerini önbelleğe alma yanıt kullanmayı öğrenin ve ASP.NET Core uygulamaları performansı arttırır."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.topic: article
uid: performance/caching/response
ms.openlocfilehash: c654cfd7c2d291849067bfd3297f940018ccb3d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core yanıt önbelleğe alma

Tarafından [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), ve [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Yanıt önbelleğe alma [Razor sayfalarında ASP.NET Core 2.0 desteklenmeyen](https://github.com/aspnet/Mvc/issues/6437). Bu özellik, desteklenen [ASP.NET Core 2.1 yayın](https://github.com/aspnet/Home/wiki/Roadmap).
  
[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Yanıt önbelleğe alma, bir istemci ya da proxy web sunucusu yapar istekleri sayısını azaltır. Yanıt önbelleğe alma ayrıca miktarını azaltan bir yanıtı oluşturmak için web sunucusu çalışma gerçekleştirir. Yanıt önbelleğe alma, istemci, proxy ve önbellek yanıtları Ara nasıl istediğinizi belirtin üstbilgileri tarafından denetlenir.

Eklediğinizde, web sunucusu yanıtlarını önbelleğe alabilir [yanıt önbelleğe alma Ara](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>HTTP tabanlı yanıt önbelleğe alma

[HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234) Internet önbellekleri nasıl hareket etmesi gerektiğini açıklar. Önbelleğe alma işlemi için kullanılan birincil HTTP üstbilgisi [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), Önbelleği belirtmek için kullanılan *yönergeleri*. Yönergeleri yollarını istemcilerden sunucularına isteklerde gibi ve yanıtı yollarını istemcilere geri sunucularından yaptıkça önbelleğe alma davranışını denetler. İsteklerin ve yanıtların proxy sunucular üzerinden taşıyın ve proxy sunucuları HTTP 1.1 önbelleğe alma belirtimine uygun olmalıdır.

Ortak `Cache-Control` yönergeleri aşağıdaki tabloda gösterilmiştir.

| Yönergesi                                                       | Eylem |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Bir önbellek yanıt depolayabilir. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Yanıt tarafından paylaşılan bir önbellek depolanması gerekir. Özel bir önbellek depolamak ve yanıt yeniden kullanabilirsiniz. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | İstemci, geçerlilik süresi belirtilen sayıda saniye büyük bir yanıtı kabul edilmeyecektir. Örnekler: `max-age=60` (60 saniye olarak), `max-age=2592000` (1 ay) |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **İsteklerinde**: bir önbellekte depolanan yanıt isteği karşılamak için kullanmaması gerekir. Not: İstemci için yanıt kaynak sunucuya yeniden oluşturur ve ara yazılım önbelleğinde depolanan yanıtta güncelleştirir.<br><br>**Yanıtları üzerinde**: kaynak sunucuda onaysız sonraki istek için yanıt kullanılmamalıdır. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **İsteklerinde**: önbellek istek depolamamayı gerekir.<br><br>**Yanıtları üzerinde**: önbellek yanıt herhangi bir kısmını depolamamayı gerekir. |

Önbelleğe alma işleminde, bir rol oynar diğer önbellek üstbilgileri aşağıdaki tabloda gösterilmiştir.

| Üstbilgi                                                     | İşlev |
| ---------------------------------------------------------- | -------- |
| [geçerlilik süresi](https://tools.ietf.org/html/rfc7234#section-5.1)     | Yanıt beri geçen saniye cinsinden süreyi tahmini oluşturulan veya kaynak sunucuda başarıyla doğrulandı. |
| [Süre sonu](https://tools.ietf.org/html/rfc7234#section-5.3) | Daha sonra yanıtı eski olarak değerlendirmeden tarih. |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Geriye dönük HTTP/1.0 ile uyumluluk için ayar önbellekleri için mevcut `no-cache` davranışı. Varsa `Cache-Control` üstbilgisi mevcutsa, `Pragma` üstbilgi göz ardı edilir. |
| [değişir](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Önbelleğe alınan yanıt sürece tüm gönderilmemesini gerekir belirtir, `Vary` üstbilgi alanları eşleşen hem önbelleğe alınan yanıtın ilk istek hem de yeni istek. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Cache-Control yönergeleri HTTP tabanlı önbelleğe alma gizliliğinize isteği

[Cache-Control üstbilgisi için HTTP 1.1 önbelleğe alma belirtimi](https://tools.ietf.org/html/rfc7234#section-5.2) geçerli bir vermenizin bir önbellek gerektirir `Cache-Control` istemci tarafından gönderilen üstbilgisi. Bir istemci isteği ile yapabilen bir `no-cache` üstbilgi değeri ve her istek için yeni bir yanıt oluşturmak için sunucu zorla.

Her zaman istemci uygularken `Cache-Control` HTTP önbelleğe alma hedef gerçekleştiriliyorsa, istek üstbilgileri anlamlı. Altında resmi belirtimi, önbelleğe alma, proxy'leri, istemcilerle sunucular, ağ üzerinden istekleri çağıran gecikme süresi ve ağ yükünü azaltmak için tasarlanmıştır. Mutlaka, kaynak sunucu üzerindeki yükü denetlemek için bir yol değil.

Kullanırken bu önbelleğe alma davranışını geçerli Geliştirici denetleyemez olan [yanıt önbelleğe alma Ara](xref:performance/caching/middleware) ara yazılım belirtimi önbelleğe alma resmi aynılarını olduğundan. [Ara yazılım gelecekteki geliştirmeler](https://github.com/aspnet/ResponseCaching/issues/96) bir isteğin yoksaymak için ara yazılım yapılandırma izin verecek `Cache-Control` önbelleğe alınan yanıt hizmet verirken üstbilgi. Ara yazılım kullandığınızda bu size daha iyi denetim yük sunucunuzda fırsat sunar.

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core önbelleğe alma diğer teknoloji

### <a name="in-memory-caching"></a>Bellek içi önbelleğe alma

Bellek içi önbelleğe alma, önbelleğe alınmış verileri depolamak için sunucu belleği kullanır. Bu tür önbelleğe alma tek bir sunucu veya kullanarak birden çok sunucu için uygun olan *Yapışkan oturumları*. Yapışkan oturumları anlamına gelir istemciden gelen istekleri işlemek için aynı sunucuya her zaman yönlendirilir.

Daha fazla bilgi için bkz: [ASP.NET çekirdek bellek içi önbelleğe alma giriş](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Dağıtılmış önbellek

Uygulama bir bulut veya sunucu grubunda barındırıldığında bellek verileri depolamak için Dağıtılmış önbellek kullanın. Önbellek istekleri işleyen sunucular arasında paylaşılır. Bir istemci, istemci için önbelleğe alınan verileri kullanılabilir ise grubundaki herhangi bir sunucu tarafından işlenen bir istek gönderebilir. ASP.NET Core, SQL Server ve dağıtılmış Redis önbellekleri sunar.

Daha fazla bilgi için bkz: [dağıtılmış bir önbellekle çalışmaya](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Cache Tag Helper

Önbellek etiket Yardımcısı'nı kullanarak bir MVC görünümü ya da Razor sayfasından içerik önbelleğe alabilir. Önbellek etiket Yardımcısı, verileri depolamak için bellek içi önbelleğe alma kullanır.

Daha fazla bilgi için bkz: [önbellek etiket Yardımcısı ASP.NET Core mvc'de](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Dağıtılmış önbellek etiket Yardımcısı

Dağıtılmış önbellek etiket Yardımcısı'nı kullanarak bir MVC görünümü ya da dağıtılmış bulut ya da web grubu senaryoları Razor sayfasından içerik önbelleğe alabilir. Dağıtılmış önbellek etiket Yardımcısı, verileri depolamak için SQL Server veya Redis kullanır.

Daha fazla bilgi için bkz: [dağıtılmış önbellek etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>ResponseCache özniteliği

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) yanıt önbelleğe alma uygun üstbilgileri ayarlamak için gerekli parametreleri belirtir.

> [!WARNING]
> Kimliği doğrulanmış istemcilerle ilgili bilgiler içeren içerik için önbelleğe almayı devre dışı bırakın. Önbelleğe alma yalnızca bir kullanıcının kimlik veya bir kullanıcının oturum açtığı göre değişmez içerik için etkinleştirilmiş olmalıdır.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) saklı yanıt sorgu anahtarları ait belirtilen liste değerleri göre değişir. Tek bir değeri olduğunda `*` ara yazılım değişir yanıtlar tarafından tüm istek sorgu dizesi parametreleri sağlanmış. `VaryByQueryKeys` ASP.NET Core 1.1 veya üstünü gerektirir.

Yanıt önbelleğe alma Ara ayarlamak için etkinleştirilmelidir `VaryByQueryKeys` özellik; Aksi halde, bir çalışma zamanı özel durum oluşur. Karşılık gelen bir HTTP üstbilgisi için hiç `VaryByQueryKeys` özelliği. Özelliği, yanıt önbelleğe alma ara yazılım tarafından işlenen bir HTTP özelliğidir. Önbelleğe alınan yanıt sunmak ara yazılımı için önceki bir istek sorgu dizesini ve sorgu dizesi değerini eşleşmelidir. Örneğin, istekleri ve sonuçları aşağıdaki tabloda gösterilen bir dizi göz önünde bulundurun.

| İstek                          | Sonuç                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Sunucusundan döndürülen     |
| `http://example.com?key1=value1` | Ara döndürdü |
| `http://example.com?key1=value2` | Sunucusundan döndürülen     |

İlk istek sunucu tarafından döndürülen ve Ara yazılımında önbelleğe alınır. Sorgu dizesi önceki istek eşleştiğinden ikinci isteği ara yazılımı tarafından döndürülür. Sorgu dizesi değerini önceki bir istek eşleşmediği için üçüncü isteği ara yazılım önbellekte değil. 

`ResponseCacheAttribute` Yapılandırmak ve oluşturmak için kullanılır (aracılığıyla `IFilterFactory`) bir [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). `ResponseCacheFilter` Uygun HTTP üstbilgilerine ve yanıt özellikleri güncelleştirme işlemlerini gerçekleştirir. Filtresi:

* Var olan tüm üstbilgileri kaldırır `Vary`, `Cache-Control`, ve `Pragma`. 
* Kümesinde özelliklerine göre uygun üstbilgileri çıkışı Yazar `ResponseCacheAttribute`. 
* HTTP özelliği, önbelleğe alma yanıt güncelleştirmeleri `VaryByQueryKeys` ayarlanır.

### <a name="vary"></a>değişir

Bu üst yalnızca zaman yazılır `VaryByHeader` özelliği ayarlanmış. Ayarlanır `Vary` özelliğin değeri. Aşağıdaki örnek kullanır `VaryByHeader` özelliği:

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Yanıt Üstbilgileri tarayıcınızın ağ araçları ile görüntüleyebilirsiniz. Aşağıdaki resim kenar çubuğunda çıktı F12 gösterir **ağ** sekmesinde `About2` eylem yöntemi yenilenir:

![About2 eylem yöntemi çağrıldığında F12 çıkış Ağ sekmesinde kenar](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore ve Location.None

`NoStore` diğer özelliklerin çoğu geçersiz kılar. Bu özellik ayarlandığında `true`, `Cache-Control` üstbilgi ayarlanmış `no-store`. Varsa `Location` ayarlanır `None`:

* `Cache-Control` ayarlanmış `no-store,no-cache`.
* `Pragma` ayarlanmış `no-cache`.

Varsa `NoStore` olan `false` ve `Location` olan `None`, `Cache-Control` ve `Pragma` ayarlanır `no-cache`.

Genelde ayarlanan `NoStore` için `true` hata sayfalarında. Örneğin:

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

Bu aşağıdaki üstbilgilerinde sonuçlanır:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Konum ve süresi

Önbelleğe alma, etkinleştirmek için `Duration` pozitif bir değere ayarlamanız gerekir ve `Location` ya da olmalıdır `Any` (varsayılan) veya `Client`. Bu durumda, `Cache-Control` üstbilgi ve ardından konum değeri ayarlanmış `max-age` yanıt.

> [!NOTE]
> `Location`kişinin seçeneklerini `Any` ve `Client` içine çevir `Cache-Control` üstbilgi değerlerini `public` ve `private`sırasıyla. Daha önce belirtildiği gibi ayarı `Location` için `None` her ikisi de ayarlar `Cache-Control` ve `Pragma` üstbilgileri `no-cache`.

Üstbilgileri gösteren bir örnek ayarlayarak aşağıda üretilen `Duration` ve varsayılan bırakarak `Location` değeri:

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Aşağıdaki üstbilgi üretir:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Önbellek profilleri

Çoğaltma yerine `ResponseCache` birçok denetleyici eylem özniteliklerinin, önbellek profilleri ayarlarını MVC'de ayarlarken seçeneği olarak yapılandırılabilir `ConfigureServices` yönteminde `Startup`. Başvurulan önbellek profilinde bulunan değerlerine göre varsayılan olarak kullanılır `ResponseCache` özniteliği ve öznitelikte belirtilen herhangi bir özellik tarafından geçersiz kılınır.

Önbellek profili ayarlama:

[!code-csharp[](response/sample/Startup.cs?name=snippet1)] 

Önbellek profili başvuruyor:

[!code-csharp[](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache` Özniteliği hem de (yöntemleri) eylemlerin ve denetleyicilerin (sınıflar) uygulanabilir. Yöntem düzeyindeki öznitelikler sınıf düzeyi özniteliklerinde belirtilen ayarları geçersiz kılar.

60 saniye olarak ayarlanmış bir süre ile bir önbellek profili yöntemi düzeyinde bir öznitelik başvuruyor ancak yukarıdaki örnekte, bir süre olan 30 saniye, sınıf düzeyinde bir öznitelik belirtir.

Sonuçta elde edilen üstbilgisi:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Ek kaynaklar

* [HTTP belirtiminden önbelleğe alma](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Bellek içi önbelleğe alma](xref:performance/caching/memory)
* [Dağıtılmış önbellek ile çalışma](xref:performance/caching/distributed)
* [Değişiklik belirteçleri değişikliklerle Algıla](xref:fundamentals/primitives/change-tokens)
* [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware)
* [Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Dağıtılmış Önbellek Etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
