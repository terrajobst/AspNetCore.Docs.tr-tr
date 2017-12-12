---
title: "ASP.NET Core için yanıt sıkıştırma Ara"
author: guardrex
description: "Yanıt sıkıştırma ve ASP.NET Core uygulamaları yanıt sıkıştırma ara yazılım kullanma hakkında bilgi edinin."
keywords: "ASP.NET Core, performans, yanıt sıkıştırma, gzip, kabul-encoding, Ara"
ms.author: riande
manager: wpickett
ms.date: 08/20/2017
ms.topic: article
ms.assetid: de621887-c5c9-4ac8-9efd-f5cc0457a134
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/response-compression
ms.openlocfilehash: fdb396d8857dc9c118cc19da1f7d1d498dfaacd5
ms.sourcegitcommit: 8ab9d0065fad23400757e4e08033787e42c97d41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="response-compression-middleware-for-aspnet-core"></a>ASP.NET Core için yanıt sıkıştırma Ara

Tarafından [Luke Latham](https://github.com/guardrex)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Ağ bant genişliği sınırlı bir kaynaktır. Genellikle yanıt boyutunun azaltılması, bir uygulamanın yanıtlama genellikle önemli ölçüde artırır. Yükü boyutları azaltmak için tek bir uygulamanın yanıtları sıkıştırma yoludur.

## <a name="when-to-use-response-compression-middleware"></a>Yanıt sıkıştırma ara yazılımı kullanmak ne zaman
IIS, Apache veya Nginx sunucu tabanlı yanıt sıkıştırmayı teknolojileri kullanır. Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmeyecektir. [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) ve [Kestrel](xref:fundamentals/servers/kestrel) şu anda yerleşik sıkıştırma desteği sunmuyoruz.

Bunu olduğunuzda yanıt sıkıştırma Ara kullanın:

* Aşağıdaki sunucu tabanlı sıkıştırmayı teknolojileri kullanılamıyor:
  * [IIS dinamik sıkıştırma Modülü](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate Modülü](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [NGINX sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Doğrudan barındırma:
  * [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Yanıt sıkıştırma
Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt gelen yanıt sıkıştırma yararlı olabilir. Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON. PNG dosyaları gibi yerel sıkıştırılmış varlıklar sıkıştırın döndürmemelidir. Tüm küçük ek boyutunu ve iletim süresindeki azalma, büyük olasılıkla daha fazla yerel sıkıştırılmış yanıt Sıkıştır çalışırsanız, sıkıştırma işlemek için geçen zaman overshadowed. Yaklaşık 150-1000 bayt (göre dosyanın içeriğini ve sıkıştırma verimliliğini) daha küçük dosyalarını sıkıştırma. Küçük dosyalar sıkıştırma yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.

Bir istemci sıkıştırılmış içerik işlerken istemci yeteneklerini sunucu göndererek bilgilendirmeniz `Accept-Encoding` istek üstbilgisi. Bir sunucu sıkıştırılmış içerik gönderdiğinde, bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıtı nasıl kodlandığını üzerinde üstbilgi. Ara yazılım tarafından desteklenen içerik kodlama belirlemeleri aşağıdaki tabloda gösterilmiştir.

| `Accept-Encoding`Üstbilgi değerleri | Desteklenen Ara | Açıklama                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | Hayır                   | Brotli sıkıştırılmış veri biçimi                               |
| `compress`                      | Hayır                   | UNIX "Sıkıştır" veri biçimi                                 |
| `deflate`                       | Hayır                   | "" zlib"veri biçimi içindeki sıkıştırılmış verileri deflate"     |
| `exi`                           | Hayır                   | W3C verimli XML değişimi                               |
| `gzip`                          | Evet (varsayılan)        | gzip dosya biçimi                                            |
| `identity`                      | Evet                  | "Kodlaması yok" tanımlayıcısı: yanıt kodlanmamış gerekir. |
| `pack200-gzip`                  | Hayır                   | Java arşivlerini ağ aktarımı biçimi                   |
| `*`                             | Evet                  | Açıkça İstenen kodlama herhangi bir kullanılabilir içerik     |

Daha fazla bilgi için bkz: [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Ara yazılım, özel ek sıkıştırma sağlayıcıları eklemenize olanak tanır `Accept-Encoding` üstbilgi değerleri. Daha fazla bilgi için bkz: [özel sağlayıcılar](#custom-providers) aşağıda.

Ara yazılım kalite değeri tepki yeteneğine sahiptir (qvalue, `q`) sıkıştırma düzenleri önceliğini belirlemek için istemci tarafından gönderilen ağırlığı. Daha fazla bilgi için bkz: [RFC 7231: kabul-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Sıkıştırma algoritmaları kolaylığını sıkıştırma hızı ve sıkıştırma verimliliğini arasında tabidir. *Etkinliğini* bu bağlamda sıkıştırma sonrasında çıktı boyutuna başvuruyor. En küçük boyuta göre en elde edilen *en iyi* sıkıştırma.

Gönderme, önbelleğe alma ve sıkıştırılmış içerik alma isteyen söz konusu üstbilgileri, aşağıdaki tabloda açıklanmıştır.

| Üstbilgi             | Rol |
| ------------------ | ---- |
| `Accept-Encoding`  | İstemciden sunucuya içerik istemci için kabul edilebilir düzenleri kodlama göstermek için gönderilen. |
| `Content-Encoding` | Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir. |
| `Content-Length`   | Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa gövde içerik değiştiğinde itibaren. |
| `Content-MD5`      | Sıkıştırma oluştuğunda `Content-MD5` üstbilgi kaldırılır, gövde içeriği değişti ve karma artık geçerli değil. |
| `Content-Type`     | İçeriğin MIME türünü belirtir. Her yanıt belirtmelisiniz, `Content-Type`. Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değeri denetler. Ara yazılım bir dizi belirtir [MIME türleri varsayılan](#mime-types) kodlamak, ancak değiştirin veya MIME türleri ekleyin. |
| `Vary`             | Değerini sunucu tarafından gönderilen `Accept-Encoding` istemcileri ve proxy'ler, `Vary` üstbilgi gösterir istemci veya önbellek proxy (farklı) değeri temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi. İçerikle döndürme sonucunu `Vary: Accept-Encoding` başlığıdır her ikisi de sıkıştırılmış ve sıkıştırılmamış yanıtlarını önbelleğe alınır ayrı olarak. |

Yanıt sıkıştırma Ara yazılımla özelliklerini keşfedebilirsiniz [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). Örnek gösterilmektedir:
* Gzip ve özel sıkıştırma sağlayıcılarını kullanarak uygulama yanıtlarının sıkıştırılması.
* Bir MIME türü sıkıştırma için MIME türlerini varsayılan listesine ekleme.

## <a name="package"></a>Paket
Ara yazılım projenize eklemek için bir başvuru ekleyin [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paketini veya kullanmak [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) paket. Bu özellik veya üstünü ASP.NET Core 1.1 hedef uygulamaları için kullanılabilir.

## <a name="configuration"></a>Yapılandırma
Aşağıdaki kod ile yanıt sıkıştırma Ara yazılımlarını etkinleştir gösterilmektedir varsayılan MIME türleri için varsayılan gzip sıkıştırması ile.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlamak için `Accept-Encoding` isteği başlığı ve yanıt üstbilgileri, boyutu ve gövde araştırmak.

Örnek uygulama için bir istek göndermek `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın. `Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değil.

![Accept-Encoding üst bilgi içermeyen bir isteğinin sonucunu gösteren fiddler penceresi. Yanıt sıkıştırılmış değil.](response-compression/_static/request-uncompressed.png)

Örnek uygulama için bir istek göndermek `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa uyun. `Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.

![Accept-Encoding üstbilgiyle isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi. Değişiklik gösterebilir ve içerik kodlama üstbilgilerinin yanıta eklenir. Yanıt sıkıştırılmışsa.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>sağlayıcıları
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Kullanım `GzipCompressionProvider` gzip ile yanıtları sıkıştırma. Hiçbiri belirtilmezse, varsayılan sıkıştırma sağlayıcısı budur. Sıkıştırma düzeyi ile ayarlayabilirsiniz `GzipCompressionProviderOptions`. 

Gzip sıkıştırma sağlayıcısı hızlı sıkıştırma düzeyi varsayılan olarak (`CompressionLevel.Fastest`), hangi değil verebilir en verimli sıkıştırma. En verimli sıkıştırma istenirse, ara yazılım en iyi sıkıştırma için yapılandırabilirsiniz.

| Sıkıştırma düzeyi                | Açıklama                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Sonuçta çıktı en iyi şekilde sıkıştırılmaz olsa bile sıkıştırma olabildiğince çabuk, tamamlamanız gerekir. |
| `CompressionLevel.NoCompression` | Sıkıştırma yok gerçekleştirilmelidir.                                                                           |
| `CompressionLevel.Optimal`       | Sıkıştırma tamamlamak için daha uzun sürer olsa bile yanıtları en iyi şekilde, sıkıştırılmış.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>MIME türleri
Ara yazılım varsayılan bir sıkıştırma için MIME türlerini belirtir:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

MIME türleri yanıt sıkıştırma ara yazılım seçenekleri ile ekleme ya da değiştirin. Bu joker karakter MIME Not türleri, aşağıdaki gibi `text/*` desteklenmez. Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core başlık resmi hizmet (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Özel sağlayıcılar
Özel sıkıştırma uygulamaları ile oluşturabileceğiniz `ICompressionProvider`. `EncodingName` Bu kodlama içeriğini temsil eder `ICompressionProvider` üretir. Ara yazılım, belirtilen liste temel sağlayıcı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.

Örnek uygulaması kullanarak, istemci isteği gönderdikten `Accept-Encoding: mycustomcompression` üstbilgi. Ara yazılım ile yanıt verir ve özel sıkıştırma uygulama kullanır bir `Content-Encoding: mycustomcompression` üstbilgi. İstemci sırayla çalışması özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını kurabilmesi gerekir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Örnek uygulama için bir istek göndermek `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri uyun. `Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut. (Gösterilmez) yanıt gövdesi örnek sıkıştırılmaz. Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği. Ancak, burada bu tür bir sıkıştırma algoritması uygulamak örnek göstermektedir.

![Accept-Encoding üstbilgiyle isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi. Değişiklik gösterebilir ve içerik kodlama üstbilgilerinin yanıta eklenir.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Güvenli bir protokol ile sıkıştırma
Güvenli bağlantı üzerinden sıkıştırılmış yanıtlar ile denetlenebilir `EnableForHttps` seçeneği varsayılan olarak devre dışıdır. Dinamik olarak üretilen sayfalarıyla sıkıştırma kullanılarak açabilir güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.

## <a name="adding-the-vary-header"></a>Ayırmayı üstbilgisi ekleme
Yanıtları sıkıştırma zaman temel `Accept-Encoding` üstbilgisi, büyük olasılıkla, yanıtı birden çok sıkıştırılmış sürümlerini ve sıkıştırılmamış bir vardır. Birden çok sürümü var ve depolanması gereken, istemci ve proxy önbellekleri istemek için `Vary` üstbilgi eklenmiş olup bir `Accept-Encoding` değeri. ASP.NET Core içinde 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir. ASP.NET Core içinde 2.x, ara yazılımı ekler `Vary` yanıt otomatik olarak sıkıştırılır zaman üstbilgi.

**ASP.NET 1.x yalnızca çekirdek**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middlware-issue-when-behind-an-nginx-reverse-proxy"></a>Bir Nginx ters proxy zaman arkasında Middlware sorunu
Bir istek Nginx tarafından yönlendirilirken olduğunda `Accept-Encoding` üstbilgi kaldırılır. Bu ara yazılımın yanıt sıkıştırmasını önler. Daha fazla bilgi için bkz: [NGINX: sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Bu sorunu tarafından izlenen [nginx (BasicMiddleware #123) için geçiş sıkıştırması tahmin](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>IIS dinamik sıkıştırma ile çalışma
Bir etkin IIS dinamik sıkıştırma bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak ile bunu yapabilirsiniz, *web.config* dosya. Daha fazla bilgi için bkz: [devre dışı bırakma IIS modüllerini](xref:hosting/iis-modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Sorun giderme
Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlama izin `Accept-Encoding` isteği başlığı ve yanıt üstbilgileri, boyutu ve gövde araştırmak. Yanıt sıkıştırma Ara aşağıdaki koşullara uyan yanıtları sıkıştırır:
* `Accept-Encoding` Üstbilgi değeri olan mevcut `gzip`, `*`, veya belirlediğinize göre bir özel sıkıştırma sağlayıcısı eşleşen özel kodlama. Değer olmamalıdır `identity` veya kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.
* MIME türü (`Content-Type`) ayarlanmalı ve yapılandırılmış bir MIME türü eşleşmelidir `ResponseCompressionOptions`.
* İstek içermemelidir `Content-Range` üstbilgi.
* Güvenli bir protokol (https) yanıt sıkıştırma ara yazılımı seçenekleri yapılandırılmadığı sürece istek güvenli olmayan bir protokol (http) kullanmanız gerekir. *Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırma etkinleştirirken.*

## <a name="additional-resources"></a>Ek kaynaklar
* [Uygulama başlatma](xref:fundamentals/startup)
* [Ara yazılım](xref:fundamentals/middleware)
* [Mozilla Geliştirici ağ: Kabul kodlama](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 bölüm 3.1.2.1: İçerik Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 bölüm 4.2.3: Gzip kodlama](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP dosya biçimi belirtimi sürümü 4.3](http://www.ietf.org/rfc/rfc1952.txt)
