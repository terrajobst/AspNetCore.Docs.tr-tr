---
title: ASP.NET core'da yanıt sıkıştırma
author: guardrex
description: Yanıt sıkıştırma ve ASP.NET Core uygulamalarında yanıt sıkıştırma ara yazılımı kullanma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: performance/response-compression
ms.openlocfilehash: a9f72a6816298b11e7b7d30b2b4bd44083baab3a
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099045"
---
# <a name="response-compression-in-aspnet-core"></a>ASP.NET core'da yanıt sıkıştırma

Tarafından [Luke Latham](https://github.com/guardrex)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Ağ bant genişliği sınırlı bir kaynaktır. Yanıt boyutu genellikle azaltma, uygulama yanıt verme hızını genellikle önemli ölçüde artırır. Yük boyutları azaltmak için bir uygulamanın yanıtları sıkıştırma yoludur.

## <a name="when-to-use-response-compression-middleware"></a>Yanıt sıkıştırma ara yazılımı kullanma zamanı

IIS, Apache veya Ngınx sunucu tabanlı yanıt sıkıştırma teknolojileri kullanın. Ara yazılım performansını büyük olasılıkla, sunucu modüllerinin eşleşmiyor. [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) sunucu ve [Kestrel](xref:fundamentals/servers/kestrel) sunucusu şu anda yerleşik sıkıştırma desteği sağlamaz.

Yanıt sıkıştırma ara yazılımı, işiniz kullanın:

* Aşağıdaki sunucu tabanlı sıkıştırma teknolojileri kullanılamıyor:
  * [IIS dinamik sıkıştırması Modülü](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate Modülü](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Ngınx sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Doğrudan barındırma:
  * [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla WebListener olarak adlandırılır)
  * [Kestrel'i sunucusu](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Yanıt sıkıştırma

Genellikle, yerel olarak sıkıştırılmış herhangi bir yanıt, yanıt sıkıştırması arasında yararlı olabilir. Yerel olarak genellikle sıkıştırılmış yanıtlarını içerir: CSS, JavaScript, HTML, XML ve JSON. PNG dosyaları gibi yerel olarak sıkıştırılmış varlıklar sıkıştırın olmamalıdır. Daha fazla yerel olarak sıkıştırılmış bir yanıt sıkıştırma çalışırsanız, küçük bir ek boyut ve iletim zaman kısalma sıkıştırma işlemek için geçen zaman büyük olasılıkla overshadowed. Yaklaşık 150-1000 bayt (bağlı olarak dosyanın içeriği ve sıkıştırma verimliliğini) küçük dosyaları sıkıştırma. Küçük dosyalar sıkıştırılıyor yükünü bir sıkıştırılmış dosyayı sıkıştırılmamış dosyasından büyük üretebilir.

Sıkıştırılmış içerik işleme bir istemci, istemci yeteneklerini sunucusu göndererek bilgilendirmeniz `Accept-Encoding` istek üst bilgisi. Bir sunucu sıkıştırılmış içerik gönderdiğinde bilgileri içermelidir `Content-Encoding` sıkıştırılmış yanıt nasıl kodlandığını üzerinde başlığı. Ara yazılım tarafından desteklenen içerik kodlama gösterimleri aşağıdaki tabloda gösterilmektedir.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` Üstbilgi değerleri | Desteklenen bir ara yazılım | Açıklama |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Evet (varsayılan)        | [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Hayır                   | [DEFLATE sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Hayır                   | [W3C XML verimli değişimi](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Evet                  | [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Evet                  | "Kodlaması" tanımlayıcısı: Yanıt kodlanmalı değil. |
| `pack200-gzip`                  | Hayır                   | [Ağ aktarım biçimi için Java arşivleri](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Evet                  | Kodlama açıkça istenen herhangi bir kullanılabilir içerik |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` Üstbilgi değerleri | Desteklenen bir ara yazılım | Açıklama |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Hayır                   | [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Hayır                   | [DEFLATE sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Hayır                   | [W3C XML verimli değişimi](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Evet (varsayılan)        | [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Evet                  | "Kodlaması" tanımlayıcısı: Yanıt kodlanmalı değil. |
| `pack200-gzip`                  | Hayır                   | [Ağ aktarım biçimi için Java arşivleri](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Evet                  | Kodlama açıkça istenen herhangi bir kullanılabilir içerik |

::: moniker-end

Daha fazla bilgi için [IANA resmi içerik kodlama listesi](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Ara yazılım özel ek sıkıştırmasını sağlayıcıları eklemenizi sağlayan `Accept-Encoding` üstbilgi değerleri. Daha fazla bilgi için [özel sağlayıcılar](#custom-providers) aşağıda.

Ara yazılım kalite değeri tepki uyumlu (qvalue, `q`) sıkıştırma düzeni önceliğini belirlemek için istemci tarafından gönderilen ağırlığı. Daha fazla bilgi için [RFC 7231: Kabul kodlama](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Sıkıştırma hız ve sıkıştırma verimliliğini arasında bir denge sıkıştırma algoritmaları tabidir. *Verimliliği* bu bağlamda sıkıştırma sonrasında çıkış boyutunu ifade eder. En küçük boyuta göre en sağlanır *en iyi* sıkıştırma.

Gönderme, önbelleğe alma ve sıkıştırılmış içerik almak isteyen ilgili üstbilgileri, aşağıdaki tabloda açıklanmıştır.

| Üstbilgi             | Rol |
| ------------------ | ---- |
| `Accept-Encoding`  | İstemciden sunucuya istemcinin kabul edilebilir düzenleri kodlama içeriği belirtmek için gönderilen. |
| `Content-Encoding` | Sunucudan yükünde içerik kodlamasını belirtmek için istemciye gönderilebilir. |
| `Content-Length`   | Sıkıştırma oluştuğunda `Content-Length` üstbilgi kaldırılır, yanıt sıkıştırılmışsa, gövde içeriği sonraki değişiklikler. |
| `Content-MD5`      | Sıkıştırma oluştuğunda `Content-MD5` üst bilgisi kaldırıldı, gövde içeriği değiştirildi ve karma artık geçerli değil. |
| `Content-Type`     | İçeriğin MIME türünü belirtir. Her yanıt belirtmeniz gerekir, `Content-Type`. Ara yazılımın yanıt sıkıştırılmış belirlemek için bu değer denetler. Bir ara yazılım belirtir [MIME türleri varsayılan](#mime-types) kodlama, ancak değiştirin veya MIME türleri ekleyin. |
| `Vary`             | Değerine sahip bir sunucu tarafından gönderilen `Accept-Encoding` proxy'leri ve istemcilere `Vary` üst bilgisi gösterir önbelleğe alması proxy ve istemci için (farklı) değerini temel alarak yanıtlarını `Accept-Encoding` isteği üstbilgisi. İçerik döndüren sonucunu `Vary: Accept-Encoding` başlığıdır hem sıkıştırılmış ve sıkıştırılmamış yanıtları ayrı olarak önbelleğe. |

Yanıt sıkıştırma ara yazılımı ile özelliklerini [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). Örnek gösterilmektedir:

* Gzip ve özel sıkıştırmasını sağlayıcıları kullanarak uygulama yanıtlarının sıkıştırılması.
* Bir MIME türü sıkıştırma için MIME türleri varsayılan listeye ekleme.

## <a name="package"></a>Paket

::: moniker range=">= aspnetcore-2.1"

Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), içeren [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ara yazılım bir projeye dahil etmek için bir başvuru ekleyin. [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paket.

::: moniker-end

## <a name="configuration"></a>Yapılandırma

::: moniker range=">= aspnetcore-2.2"

Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri ve sıkıştırmasını sağlayıcıları için etkinleştirileceğini gösterir ([Brotli](#brotli-compression-provider) ve [Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Aşağıdaki kod nasıl yanıt sıkıştırma ara yazılımı varsayılan MIME türleri için etkinleştirileceğini gösterir ve [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider):

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

Notlar: 

* `app.UseResponseCompression` önce çağrılmalıdır `app.UseMvc`.
* Gibi bir araç kullanın [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) ayarlanacak `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın.

Örnek uygulamaya talebinizi `Accept-Encoding` üstbilgi ve yanıt sıkıştırılmamış olup olmadığına bakın. `Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut değildir.

![Accept-Encoding üst bilgisi olmayan bir isteğinin sonucunu gösteren fiddler penceresi. Yanıt yok sıkıştırılmaz.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Örnek uygulamada talebinizi `Accept-Encoding: br` üst bilgisi (Brotli sıkıştırma) ve yanıt sıkıştırılmışsa gözlemleyin. `Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve br değerini gösteren fiddler penceresi. Değişiklik gösterebilir ve Content-Encoding üstbilgilerini yanıta eklenir. Yanıt sıkıştırılmışsa.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Örnek uygulamada talebinizi `Accept-Encoding: gzip` üstbilgi ve yanıt sıkıştırılmışsa gözlemleyin. `Content-Encoding` Ve `Vary` üstbilgileri yanıtta mevcut.

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve gzip değerini gösteren fiddler penceresi. Değişiklik gösterebilir ve Content-Encoding üstbilgilerini yanıta eklenir. Yanıt sıkıştırılmışsa.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>sağlayıcıları

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Brotli sıkıştırma sağlayıcısı

Kullanım `BrotliCompressionProvider` ile yanıtları sıkıştırma [Brotli sıkıştırılmış veri biçimi](https://tools.ietf.org/html/rfc7932).

Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Brotli sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Gzip sıkıştırma sağlayıcısı](#gzip-compression-provider).
* Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir. Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Tüm sıkıştırmasını sağlayıcıları açıkça eklendiğinde Brotoli sıkıştırma sağlayıcının eklenmesi gerekir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

Sıkıştırma düzeyi ile ayarlanmış `BrotliCompressionProviderOptions`. Brotli sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma. En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.

| Sıkıştırma düzeyi | Açıklama |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma yok gerçekleştirilmelidir. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a>Gzip sıkıştırma sağlayıcısı

Kullanım <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> ile yanıtları sıkıştırma [Gzip dosya biçimi](https://tools.ietf.org/html/rfc1952).

Sıkıştırma yok sağlayıcıları için açıkça eklenirse <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları ile birlikte bir dizi varsayılan olarak eklenir [Brotli sıkıştırma sağlayıcısı](#brotli-compression-provider).
* Sıkıştırma Brotli sıkıştırma için varsayılan olarak Brotli sıkıştırılmış veri biçimi, istemci tarafından desteklenir. Gzip sıkıştırma istemcinin desteklediğinde sıkıştırma Brotli istemci tarafından desteklenmiyorsa, Gzip için varsayılan olarak.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Gzip sıkıştırma sağlayıcısı sıkıştırmasını sağlayıcıları dizisi için varsayılan olarak eklenir.
* Gzip sıkıştırma istemcinin desteklediğinde, Gzip sıkıştırma varsayılan olarak.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Gzip sıkıştırma sağlayıcısı herhangi sıkıştırmasını sağlayıcıları açıkça eklendiğinde eklenmesi gerekir:

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

Sıkıştırma düzeyi ile ayarlanmış <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Gzip sıkıştırma sağlayıcısı varsayılan olarak en hızlı sıkıştırma düzeyini ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), hangi değil üretebilir en verimli sıkıştırma. En verimli sıkıştırma isterseniz, ara yazılım en iyi sıkıştırma için yapılandırın.

| Sıkıştırma düzeyi | Açıklama |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma, elde edilen çıkış en uygun şekilde sıkıştırılmaz olsa bile, mümkün olan en kısa sürede tamamlamanız gerekir. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma yok gerçekleştirilmelidir. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Sıkıştırma tamamlanması daha uzun sürer bile yanıtları en uygun şekilde, sıkıştırılmış olması gerekir. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a>Özel sağlayıcılar

Özel sıkıştırma uygulamalarıyla oluşturma <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Temsil eden bu kodlama içeriğe `ICompressionProvider` üretir. Ara yazılım, belirtilen liste göre sağlayıcıyı seçmek için bu bilgileri kullanır. `Accept-Encoding` isteği üstbilgisi.

Örnek uygulama kullanma, istemci bir istek gönderir `Accept-Encoding: mycustomcompression` başlığı. Ara yazılım özel sıkıştırma uygulaması kullanır ve yanıtı döndürür bir `Content-Encoding: mycustomcompression` başlığı. İstemci, sırayla çalışmak özel sıkıştırma uygulaması için özel kodlama sıkıştırmasını mümkün olması gerekir.

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Örnek uygulamada talebinizi `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üstbilgileri gözlemleyin. `Vary` Ve `Content-Encoding` üstbilgileri yanıtta mevcut. (Gösterilmemiştir) yanıt gövdesi olarak örnek sıkıştırılmaz. Sıkıştırma uygulamasında hiç `CustomCompressionProvider` sınıfının örneği. Ancak, bu tür bir sıkıştırma algoritması burada uygulamak örnek gösterir.

![Accept-Encoding üst bilgisiyle bir isteğinin sonucunu ve mycustomcompression değerini gösteren fiddler penceresi. Değişiklik gösterebilir ve Content-Encoding üstbilgilerini yanıta eklenir.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>MIME türleri

Ara yazılım bir varsayılan sıkıştırma için MIME türlerini belirtir:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Yanıt sıkıştırma ara yazılımı seçenekleri MIME türleri ekleyin veya değiştirin. Unutmayın, joker karakter MIME türleri gibi `text/*` desteklenmez. Örnek uygulama için bir MIME türü ekler `image/svg+xml` sıkıştırır ve ASP.NET Core hizmet başlık resmi (*banner.svg*).

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Güvenli protokol sıkıştırması

Sıkıştırılmış yanıtlar güvenli bağlantılar üzerinden ile denetlenebilir `EnableForHttps` seçeneği, varsayılan olarak devre dışıdır. Sıkıştırma ile dinamik olarak oluşturulan sayfaları kullanarak yol açabilir. güvenlik sorunları gibi [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.

## <a name="adding-the-vary-header"></a>Vary üstbilgi ekleme

::: moniker range=">= aspnetcore-2.0"

Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır. Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri. ASP.NET Core 2.0 veya sonraki sürümlerde, ara yazılım ekler `Vary` otomatik olarak yanıt sıkıştırılmışsa, üst bilgisi.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Yanıtları sıkıştırma zaman temelinde `Accept-Encoding` üst bilgi, potansiyel olarak birden çok sıkıştırılmış sürümlerinin yanıtın ve sıkıştırılmamış bir vardır. Birden çok sürümünün var ve depolanması gereken, istemci ve proxy önbellek isteyin için `Vary` üst bilgisi ile eklenen bir `Accept-Encoding` değeri. İçinde ASP.NET Core 1.x, ekleme `Vary` üstbilgisi yanıt için el ile gerçekleştirilir:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Ters proxy arkasında olduğunda bir Ngınx ara yazılım sorunu

Bir isteği Ngınx tarafından proxy olduğunda `Accept-Encoding` üstbilgi kaldırılır. Kaldırılmasını `Accept-Encoding` üst bilgi yanıtı sıkıştırmasını ara yazılım engeller. Daha fazla bilgi için [NGINX: Sıkıştırma ve açma](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Bu sorunu tarafından izlenen [Nginx için doğrudan sıkıştırma ekleyeceğimi (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>IIS dinamik sıkıştırması ile çalışma

Bir active IIS dinamik sıkıştırması bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan modülü varsa, ek olarak modülle devre dışı *web.config* dosya. Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Sorun giderme

Gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), veya [Postman](https://www.getpostman.com/), ayarlamak izin `Accept-Encoding` istek üst bilgisi ve yanıt üstbilgileri, boyutu ve gövde çalışın. Varsayılan olarak, aşağıdaki koşullara uyması yanıtları yanıt sıkıştırma ara yazılımı sıkıştırır:

::: moniker range=">= aspnetcore-2.2"

* `Accept-Encoding` Üst bilgi değeri olan mevcut `br`, `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama. Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.
* MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* İstek içermemelidir `Content-Range` başlığı.
* İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir. *Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* `Accept-Encoding` Üst bilgi değeri olan mevcut `gzip`, `*`, veya sizin belirlediğiniz bir özel sıkıştırma sağlayıcısı ile eşleşen özel kodlama. Değeri olmamalıdır `identity` veya bir kalite değeri (qvalue, `q`) 0 (sıfır) ayarlama.
* MIME türü (`Content-Type`) ayarlanmalıdır ve yapılandırılmış bir MIME türü eşleşmelidir <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* İstek içermemelidir `Content-Range` başlığı.
* İstek, yanıt sıkıştırma ara yazılımı seçenekleri güvenli Protokolü (https) yapılandırılmadığı sürece güvenli olmayan bir protokol (http) kullanmanız gerekir. *Tehlike Not [yukarıda açıklanan](#compression-with-secure-protocol) güvenli içerik sıkıştırmayı etkinleştirilirken.*

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla Geliştirici ağ: Kabul kodlama](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 bölüm 3.1.2.1: İçerik Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 bölüm 4.2.3: Gzip kodlama](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP dosyası biçim belirtimi sürümü 4.3](http://www.ietf.org/rfc/rfc1952.txt)
