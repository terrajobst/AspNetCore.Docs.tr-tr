---
title: URL yeniden yazma ara yazılımı ASP.NET core'da
author: guardrex
description: URL yeniden yazma ve URL yeniden yazma ara yazılımı ile ASP.NET Core uygulamalarında yeniden yönlendirme hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/url-rewriting
ms.openlocfilehash: 5be53baf4b9eb8774501fbf7f781370f7f687d0c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814946"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL yeniden yazma ara yazılımı ASP.NET core'da

Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)

::: moniker range="<= aspnetcore-1.1"

Bu konuda 1.1 sürümü için indirme [URL yeniden yazma ara yazılımı ASP.NET core'da (sürüm 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf).

::: moniker-end

Bu belge, URL yeniden yazma URL yeniden yazma ara yazılımı ASP.NET Core uygulamalarında kullanma hakkında yönergeler sunar.

URL yeniden yazma URL bir veya daha fazla önceden tanımlanmış kurallara göre istek değiştirme işlemidir. Böylece konumları ve adresleri sıkı şekilde bağlı olmayan URL yeniden yazma kaynak konumları ve adresleri arasında bir Özet oluşturur. URL yeniden yazma için çeşitli senaryolarda kullanışlıdır:

* Taşıyın veya sunucu kaynaklarına geçici veya kalıcı olarak değiştirin ve korumak için bu kaynakları kararlı bulucular.
* İstek işleme bir uygulamanın alanları arasında veya farklı uygulamalar arasında bölün.
* Kaldır, ekleme veya URL kesimleri gelen isteklerden yeniden düzenleyin.
* Genel URL, arama motoru iyileştirmesi (SEO) iyileştirin.
* Bir kaynak isteyerek döndürülen içerik tahmin edebilmesi için ortak kolay URL'leri kullanımına izin verir.
* Uç noktalarını güvenli hale getirmek için güvenli istekleri yönlendirin.
* Burada dış bir siteye barındırılan bir statik varlık başka bir siteye kendi içeriğinize varlık bağlayarak kullanır, hotlinking engelleyin.

> [!NOTE]
> URL yeniden yazma uygulama performansını düşürebilir. Uygun olduğunda, sayısı ve karmaşıklığı kurallar sınırlayın.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="url-redirect-and-url-rewrite"></a>URL yeniden yönlendirme ve URL yeniden yazma

İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* inceliklidir ancak istemcilere kaynakları sağlamak için önemli etkilere sahiptir. ASP.NET Core'nın URL yeniden yazma ara yazılımı her ikisi de gereksinimini toplantı yeteneğine sahiptir.

A *URL yeniden yönlendirme* istemci burada belirtildiği başlangıçta istenen istemci farklı bir adresten bir kaynağa erişmek için bir istemci tarafı işlemi içerir. Bu sunucuya gidiş dönüş gerektirir. İstemci kaynak için yeni bir istekte bulunduğunda istemciye döndürülen yeniden yönlendirme URL'sini tarayıcınızın adres çubuğunda görünür.

Varsa `/resource` olduğu *yeniden yönlendirilen* için `/different-resource`, sunucu istemcinin kaynakta edinmelidir yanıt verir. `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu gösteren bir durum kodu ile.

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) 1 (v1) sürümünden geçici olarak değiştirildi. Bir istemci, sürüm 1 yolu /v1/api hizmeti için bir istek gönderir. Sunucu, sürüm 2 /v2/api hizmet için yeni, geçici yoluyla 302 bir (bulunamadı) yanıtı geri gönderir. İstemci, hizmeti yeniden yönlendirme URL'si için ikinci bir talep gönderir. Sunucu bir 200 (Tamam) durum koduyla yanıt verir.](url-rewriting/_static/url_redirect.png)

İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme ile yanıt durum kodunu belirterek kalıcı veya geçici olup olmadığını gösterir:

* *301 - kalıcı olarak taşındı* durum kodu kullanılan burada yeni, kalıcı bir URL kaynağı vardır ve istediğiniz istemci gelecekteki tüm istekler kaynak için yeni URL kullanması gerektiğini bildirin. *İstemci önbellek ve 301 durum kodu alındığında yanıt yeniden.*

* *302 bulundu -* durum kodu yeniden yönlendirme geçici olduğu veya değiştirilebilir genel olarak kullanılır. 302 durum kodu olmayan mağaza URL'sini ve gelecekte kullanmak üzere istemciye gösterir.

Durum kodları hakkında daha fazla bilgi için bkz. [RFC 2616: Durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *URL yeniden yazma* istenen istemci farklı bir kaynak adresten bir kaynak sağlayan bir sunucu tarafı işlemdir. Bir URL yeniden yazma sunucuya gidiş dönüş gerektirmez. Yeniden URL istemciye döndürülen değildir ve tarayıcınızın adres çubuğuna görünmez.

Varsa `/resource` olduğu *yazılan* için `/different-resource`, sunucunun *dahili olarak* getirir ve kaynağı döndürür `/different-resource`.

İstemci yeniden URL'deki kaynak alınamadı olabilir, ancak istemci, isteği yapar ve yanıtı alır, kaynağın yeniden URL'de mevcut haberdar değildir.

![Webapı hizmet uç noktası, sunucu üzerindeki sürüm 2 (v2) için 1 (v1) sürümünden değiştirildi. Bir istemci, sürüm 1 yolu /v1/api hizmeti için bir istek gönderir. İstek URL'si, sürüm 2 yolu /v2/api hizmetine erişmek için yeniden. Hizmet istemcisi bir 200 (Tamam) durum kodu ile yanıt verir.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL yeniden yazma örnek uygulaması

URL yeniden yazma ara yazılımı ile özelliklerini inceleyebilirsiniz [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Uygulama yeniden yönlendirme uygulanır ve yeniden yazma kuralları ve çeşitli senaryolar için yeniden yönlendirilen veya yeniden URL gösterir.

## <a name="when-to-use-url-rewriting-middleware"></a>URL yeniden yazma ara yazılımı kullanma zamanı

URL yeniden yazma ara yazılımı, aşağıdaki yaklaşımlardan kullanamaz olduğunuzda kullanın:

* [URL yeniden yazma modülü ile Windows Server'da IIS](https://www.iis.net/downloads/microsoft/url-rewrite)
* [Apache sunucuda Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/)
* [Ngınx üzerinde yeniden yazma URL'si](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

Uygulama üzerinde barındırılıyorsa, ayrıca, kullanılacak Ara yazılımları kullanmayı [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla WebListener olarak adlandırılır).

IIS, Apache, Nginx teknolojileri yeniden sunucu tabanlı URL'sini kullanmak üzere temel neden şunlardır:

* Ara yazılım, bu modüllerin tam özelliklerini desteklemez.

  Sunucu modüllerinin özelliklerinden bazıları gibi ASP.NET Core projeleriyle çalışmıyor `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları. Bu senaryolarda, ara yazılım kullanın.
* Ara yazılım performansını büyük olasılıkla, modül ile eşleşmiyor.

  Değerlendirmesi emin olmak için hangi yaklaşımın en iyi performansı düşürür veya performans düzeyi düşürülmüş göz ardı edilebilir ise bilmek tek yoludur.

## <a name="package"></a>Paket

Ara yazılım projenize eklemek için paket başvurusu ekleme [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) proje dosyasında içeren [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) paket.

Değil kullanırken `Microsoft.AspNetCore.App` metapackage, bir proje başvurusu Ekle `Microsoft.AspNetCore.Rewrite` paket.

## <a name="extension-and-options"></a>Uzantı ve seçenekleri

URL yeniden yazma oluşturmak ve bir örneğini oluşturarak kuralları yeniden yönlendirme [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) sınıfı her biri, yeniden yazma kuralları için genişletme yöntemleri. İşlenen bunları istediğiniz sırayla birden çok kural zincir. `RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma ara yazılımı geçirilen <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*>:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a>Www www olmayan yeniden yönlendirme

Üç seçenek olmayan yönlendirmek için uygulama izin`www` ister `www`:

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; İstek için kalıcı yeniden yönlendirme `www` istek olmayan ise alt etki alanı`www`. İle yeniden yönlendiren bir [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) durum kodu.

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; Yeniden yönlendirme isteği `www` gelen istek olmayan ise alt etki alanı`www`. İle yeniden yönlendiren bir [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) durum kodu. Aşırı yanıtı durum kodu sağlamanıza izin verir. Bir alanı kullanan <xref:Microsoft.AspNetCore.Http.StatusCodes> bir durum kodu ataması için sınıf.

### <a name="url-redirect"></a>URL yeniden yönlendirme

Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> isteklerini yeniden yönlendirmek için. İlk parametre, normal ifade gelen URL yolunda eşleşen içerir. İkinci değiştirme dizesi parametresidir. Üçüncü parametre varsa, durum kodu belirtir. Durum kodu durum kodu belirtmezseniz varsayılan *302 bulundu -* , kaynak geçici olarak taşındı veya değiştirilinceye olduğunu gösterir.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

Geliştirici Araçları etkin bir tarayıcıda yolu kullanarak örnek uygulamaya istekte bulunmak `/redirect-rule/1234/5678`. Normal ifade üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`. Yeniden yönlendirme URL'si ile istemciye gönderilen bir *302 bulundu -* durum kodu. Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL'sindeki yeni bir istek gönderir. Yeniden yönlendirme URL'sini örnek uygulama eşleşmeye hiçbir kural itibaren:

* İkinci isteği aldığında bir *200 - OK* uygulama yanıtı.
* Yanıt gövdesinin yeniden yönlendirme URL'sini gösterir.

Bir gidiş dönüş yapılan sunucuya bir URL olduğunda *yeniden yönlendirilen*.

> [!WARNING]
> Yeniden yönlendirme kuralları oluştururken dikkatli olun. Yeniden yönlendirme kuralları, sonra bir yeniden yönlendirme de dahil olmak üzere uygulama her istekte değerlendirilir. Yanlışlıkla oluşturmak kolaydır bir *döngü sonsuz yeniden yönlendirmeleri,* .

Özgün istek: `/redirect-rule/1234/5678`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect.png)

Parantez içinde yer alan bir ifade parçası olarak adlandırılan bir *yakalama grubu*. Nokta (`.`) ifade anlamına gelir *herhangi bir karakterle eşleştir*. Yıldız işareti (`*`) gösteren *önceki karakteri sıfır veya daha fazla kez eşleştir*. Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`. Sonra istek URL'sindeki sağladığınız herhangi bir değer `redirect-rule/` bu tek bir yakalama grubu tarafından yakalanır.

Değiştirme dizesinde yakalanan gruplar dolar işareti ile dizesine eklenen (`$`) yakalama dizisi sayı takip eder. İlk yakalama grubu değer ile elde edilen `$1`ikinci ile `$2`, ve bunların sırası, normal ifade yakalama grupları için devam edin. Var. yalnızca bir yakalanan gruba içinde yeniden yönlendirme kuralı regex örnek uygulamada, olan değiştirme dizesinde tek eklenen grubu için `$1`. Kural uygulandığında, URL olur `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Güvenli bir uç noktası için URL yeniden yönlendirme

Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> aynı konak ve yol HTTPS protokolünü kullanarak HTTP isteklerini yeniden yönlendirmek için. Durum kodu verilmiyorsa, ara yazılım için varsayılan olarak *302 bulundu -* . Bağlantı noktası verilmiyorsa varsa:

* Varsayılan olarak ara yazılım `null`.
* Şema değişikliklerini `https` (HTTPS protokolü) ve istemci kaynak bağlantı noktası 443 üzerinden erişir.

Aşağıdaki örnek, durum kodu ayarlamak için gösterilmektedir *301 - kalıcı olarak taşındı* ve 5001 için bağlantı noktasını değiştirin.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> aynı konak ve yol bağlantı noktası 443 üzerinden güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için. Ara yazılım durum kodunu ayarlar *301 - kalıcı olarak taşındı*.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Ek yeniden yönlendirme kuralları gereksinimi olmadan güvenli bir uç noktasına yeniden yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılım kullanmanızı öneririz. Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl#require-https) konu.

Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`. Uzantı yöntemine ekleyin `RewriteOptions`. Güvenli olmayan bir istek, herhangi bir URL konumundaki uygulamaya olun. Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısını kapatmak veya sertifikaya güvenmek için bir özel durum oluşturun.

Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https.png)

Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL yeniden yazma

Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> URL yeniden yazma için bir kural oluşturmak için. İlk parametre gelen URL yolu temel eşleştirme için normal ifade içeriyor. İkinci değiştirme dizesi parametresidir. Üçüncü parametreyi `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız, ek bir yeniden yazma kuralları atlamak gerekip gerekmediğini belirtir.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

Özgün istek: `/rewrite-rule/1234/5678`

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_rewrite.png)

Simgeyi seçtiğinizde (`^`) URL yolunun başında, eşleşen ifade anlamına gelir. başlangıçtan başlar.

Yeniden yönlendirme kuralı ile bir önceki örnekte `redirect-rule/(.*)`, hiçbir ayar yoktur (`^`) normal ifade başlangıcı. Bu nedenle, herhangi bir karakter önünde `redirect-rule/` yolunda başarılı bir eşleşme.

| Yol                               | Eşleştirme |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Evet   |
| `/my-cool-redirect-rule/1234/5678` | Evet   |
| `/anotherredirect-rule/1234/5678`  | Evet   |

Yeniden üretme kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız, yalnızca yollarıyla eşleşen `rewrite-rule/`. Aşağıdaki tabloda, eşleşen içinde farka dikkat edin.

| Yol                              | Eşleştirme |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Evet   |
| `/my-cool-rewrite-rule/1234/5678` | Hayır    |
| `/anotherrewrite-rule/1234/5678`  | Hayır    |

Aşağıdaki `^rewrite-rule/` bölümü ifadesi, iki yakalama grupları vardır `(\d+)/(\d+)`. `\d` Gösterir *bir basamağı (sayı) eşleşen*. Artı işaretini (`+`) anlamına gelir *bir veya daha önceki karakter eşleşen*. Bu nedenle, URL, başka bir sayının İleri-eğik çizgi ardından bir sayı içermesi gerekir. Bu yakalama grupları yeniden URL eklenmiş `$1` ve `$2`. Yeniden yazma kuralı değiştirme dizesi, sorgu dizesinde yakalanan gruplar yerleştirir. İstenen yolunu `/rewrite-rule/1234/5678` kaynağı almak için yazılan `/rewritten?var1=1234&var2=5678`. Özgün istek üzerinde bir sorgu dizesi varsa, URL'yi yeniden zaman korunur.

Hiçbir kaynak almak için sunucuya gidiş dönüş yoktur. Kaynağın mevcut durumunda getirilen ve istemciye döndürülen bir *200 - OK* durum kodu. Tarayıcının adres çubuğuna URL'yi, istemci yeniden yönlendirilen değildir çünkü değiştirmez. İstemciler, bir URL yeniden yazma işlemi sunucuda oluştu algılayamaz.

> [!NOTE]
> Kullanım `skipRemainingRules: true` her olası kurallarına çünkü hesaplama açısından pahalıdır ve uygulama yanıt süresini artırır. Hızlı Uygulama yanıtı:
>
> * Sipariş en sık eşleşen kural kurallardan az sık eşleşen kural yeniden yazın.
> * Bir eşleşme olursa ve hiçbir ek kural işleme gerekli olduğunda kalan kurallarının işlenmesini atlayın.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Apache mod_rewrite kurallarıyla uygulamak <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*>. Kurallar dosyası uygulamayla dağıtıldığından emin olun. Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

A <xref:System.IO.StreamReader> kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`. Yanıt durum kodu *302 bulundu -* .

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

Özgün istek: `/apache-mod-rules-redirect/1234`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_apache_mod_redirect.png)

Ara yazılım, aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* SAAT
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL yeniden yazma modülü kuralları

IIS URL yeniden yazma modülü uygulanan aynı kural kümesi kullanmak için <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>. Kurallar dosyası uygulamayla dağıtıldığından emin olun. Uygulamanın kullanılacak Ara yazılımının doğrudan olmayan *web.config* dosya Windows Server IIS üzerinde çalışırken. Uygulamanın dışında IIS ile bu kuralları depolanması gereken *web.config* IIS yeniden yazma modülü ile çakışmaları önlemek için dosya. Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz. [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

A <xref:System.IO.StreamReader> kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

Örnek uygulama, gelen istekleri yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`. Yanıtı istemciye gönderilen bir *200 - OK* durum kodu.

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

Özgün istek: `/iis-rules-rewrite/1234`

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_iis_url_rewrite.png)

Etkin bir IIS yeniden yazma modülü ile uygulamanızı istenmeyen yollarla erişememeleri yapılandırılmış sunucu düzeyinde kurallar varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz. Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Desteklenmeyen özellikler

Yayımlanan bir ara yazılım ile ASP.NET Core 2.x, aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:

* Giden kuralları
* Özel sunucu değişkenleri
* Joker karakterler
* LogRewrittenUrl

#### <a name="supported-server-variables"></a>Desteklenen sunucu değişkenleri

Ara yazılım, aşağıdaki IIS URL yeniden yazma modülü sunucu değişkenleri destekler:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> De edinebilirsiniz bir <xref:Microsoft.Extensions.FileProviders.IFileProvider> aracılığıyla bir <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>. Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir. Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtıldığından emin emin olun.
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Metot tabanlı kuralı

Kullanım <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> bir yöntemde kendi kural mantığı uygulamak için. `Add` kullanıma sunan <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>, hangi kullanımınıza <xref:Microsoft.AspNetCore.Http.HttpContext> yönteminiz olarak kullanmak için. [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*) nasıl ek işlem hattı belirler işleme gerçekleştirilir. Değer birine ayarlayın <xref:Microsoft.AspNetCore.Rewrite.RuleResult> aşağıdaki tabloda açıklanan alanları.

| `RewriteContext.Result`              | Eylem                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| `RuleResult.ContinueRules` (varsayılan) | Kurallarını uygulama devam edin.                                         |
| `RuleResult.EndResponse`             | Kuralları uygulamak durdurun ve bir yanıt gönderir.                       |
| `RuleResult.SkipRemainingRules`      | Kuralları uygulamak durdurun ve bağlam için bir sonraki ara yazılım gönderin. |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

Örnek uygulama ile biten istekleri yolları için yönlendiren bir yöntemi gösterir *.xml*. Bir istek yaptıysanız `/file.xml`, istek yönlendireceği `/xmlfiles/file.xml`. Durum kodu kümesine *301 - kalıcı olarak taşındı*. Ne zaman tarayıcı yapar yeni bir istek */xmlfiles/file.xml*, statik dosya ara yazılımlarını dosya istemciden hizmet *wwwroot/xmlfiles* klasör. İçin bir yeniden yönlendirme, yanıtın durum kodu açıkça ayarlayın. Aksi takdirde, bir *200 - OK* durum kodu döndürülür ve istemcide yeniden yönlendirme gerçekleşmez.

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

Bu yaklaşım, istekleri de yazabilirsiniz. Örnek uygulama sunmak herhangi bir metin dosyası talep için yol yeniden yazma gösterir *dosya.txt* metin dosyasından *wwwroot* klasör. Statik dosya ara yazılımlarını dosyanın güncelleştirilmiş isteği yola göre hizmet eder:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

*RewriteRules.cs*:

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a>Kural Irule tabanlı

Kullanma <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> kuralı mantığını uygulayan bir sınıf içinde kullanılacak <xref:Microsoft.AspNetCore.Rewrite.IRule> arabirimi. `IRule` metot tabanlı kural yaklaşımı kullanarak üzerinden daha fazla esneklik sağlar. Uygulama sınıfınız için parametreleri geçirebilirsiniz izin veren bir oluşturucu içerebilir <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> yöntemi.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

Örnek uygulama için parametre değerlerini `extension` ve `newPath` çeşitli koşullara uyması için denetlenir. `extension` Bir değer içermelidir ve değer olmalıdır *.png*, *.jpg*, veya *.gif*. Varsa `newPath` geçerli olmayan bir <xref:System.ArgumentException> oluşturulur. Bir istek yaptıysanız *image.png*, istek yönlendireceği `/png-images/image.png`. Bir istek yaptıysanız *image.jpg*, istek yönlendireceği `/jpg-images/image.jpg`. Durum kodu kümesine *301 - kalıcı olarak taşındı*ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

Özgün istek: `/image.png`

![Tarayıcının geliştirici araçları istek ve yanıtların image.png için izleme ile](url-rewriting/_static/add_redirect_png_requests.png)

Özgün istek: `/image.jpg`

![Tarayıcının geliştirici araçları istek ve yanıtların image.jpg için izleme ile](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Normal ifade örnekleri

| Hedef | Normal ifade dizesini &<br>Eşleşme örneği | Değiştirme dizesi &<br>Çıkış örneği |
| ---- | ------------------------------- | -------------------------------------- |
| Querystring yol yeniden yazma | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Şerit sonunda eğik çizgi | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Eğik zorla | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Belirli isteklere yeniden yazma kaçının | `^(.*)(?<!\.axd)$` veya `^(?!.*\.axd$)(.*)$`<br>Evet: `/resource.htm`<br>Hayır: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| URL kesimleri bunları yeniden düzenleme | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| URL kesimi değiştirin | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Ek kaynaklar

* [Uygulama Başlatma](startup.md)
* [Ara Yazılım](xref:fundamentals/middleware/index)
* [.NET içinde normal ifadeler](/dotnet/articles/standard/base-types/regular-expressions)
* [Normal ifade dili - hızlı başvuru](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [URL yeniden yazma modülü 2.0 (IIS) kullanma](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [IIS URL yeniden yazma modülü Forumu](https://forums.iis.net/1152.aspx)
* [Basit bir URL yapısı tutun](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 URL yeniden yazma, ipuçları ve püf noktaları](https://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Eğik çizgi veya değil eğik çizgi](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
