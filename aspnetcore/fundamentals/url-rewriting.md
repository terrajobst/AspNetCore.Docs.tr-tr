---
title: URL yeniden yazma ASP.NET Core Ara
author: guardrex
description: "URL yeniden yazma işlemi ve ASP.NET Core uygulamaları URL yeniden yazma işlemi Ara yazılımla yeniden yönlendirme hakkında bilgi edinin."
keywords: "ASP.NET Core URL yeniden yazma, URL yeniden yazma, yeniden yönlendirme, URL yeniden yönlendirme, ara yazılım, apache_mod URL'si"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: dde0b5673c9885db2fecbb24b384752e5ddf70eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL yeniden yazma ASP.NET Core Ara

Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

URL yeniden yazma işlemi bir veya daha fazla önceden tanımlanmış kurallar URL'leri dayalı isteği değiştirme işlemidir. URL yeniden yazma işlemi bir Özet kaynak konumları ve adresleri arasında adreslerini ve konumları sıkı şekilde bağlı olmayan şekilde oluşturur. URL yeniden yazma işlemi değerli olduğu birkaç senaryo vardır:
* Taşıma veya bu kaynaklar için kararlı bulucular korurken sunucu kaynaklarını geçici veya kalıcı olarak değiştirme
* İstek farklı uygulamalar arasında veya bir uygulamanın alanları genelinde işleme bölme
* Kaldırma, ekleme ya da URL kesimleri gelen istekleri üzerinde yeniden düzenleme
* Ortak URL'ler arama motoru iyileştirme (SEO) için en iyi duruma getirme
* Bir bağlantıyı izleyerek bulacaksınız içerik tahmin kişilerin yardımcı olmak üzere ortak kolay URL'leri kullanımını erişimine izin verme
* Uç noktalarını güvenli hale getirmek için güvenli olmayan istekleri yönlendirme
* Görüntü hotlinking önleme

Özel kural mantığı kullanarak birkaç yoldan URL değiştirme ve regex, Apache mod_rewrite modülü kuralları, IIS yeniden yazma modülü kuralları da dahil olmak üzere için kurallar tanımlayabilirsiniz. Bu belge, ASP.NET Core uygulamaları URL yeniden yazma işlemi Ara kullanma hakkında yönergeler içeren URL yeniden yazma işlemi sunar.

> [!NOTE]
> URL yeniden yazma işlemi bir uygulama performansını azaltabilir. Uygun yerlerde sayısı ve karmaşıklığı kuralları sınırlamanız gerekir.

## <a name="url-redirect-and-url-rewrite"></a>URL yeniden yönlendirme ve URL yeniden yazma
İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* en ince görünebilir ilk ancak istemciler için kaynaklar sağlamak için önemli etkileri vardır. ASP.NET Core'nin URL yeniden yazma işlemi Ara gereken her ikisi için de toplantı yeteneğine sahiptir.

A *URL yeniden yönlendirme* istemci burada belirtildiği başka bir adresinde bir kaynağa erişmek için bir istemci tarafı işlemi olduğu. Bu sunucuya gidiş gerektirir ve istemci kaynak için yeni bir istek yaptığında, istemciye döndürülen yeniden yönlendirme URL'si tarayıcının adres çubuğunda görünür. Varsa `/resource` olan *yeniden yönlendirilen* için `/different-resource`, istemci isteklerini `/resource`, ve istemci kaynak edinmelidir sunucunun yanıt verdiğini `/different-resource` yeniden yönlendirme olduğunu belirten bir durum kodu ile geçici veya kalıcı. İstemci yeniden yönlendirme URL'sini kaynak için yeni bir isteği yürütür.

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) için sürüm 1 (v1) geçici olarak değiştirildi. Bir istemci sürüm 1 yolu /v1/api adresindeki hizmet isteği yapar. Sunucu hizmeti için yeni, geçici yoluyla 302 bir (bulundu) yanıtı sürüm 2 /v2/api geri gönderir. İstemci hizmet yeniden yönlendirme URL'sinde ikinci bir isteği yapar. Sunucu bir 200 (Tamam) durum koduyla yanıt verir.](url-rewriting/_static/url_redirect.png)

İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme kalıcı veya geçici olup olmadığını gösterir. 301 (taşınmış kalıcı olarak) durum kodunu burada kaynak yeni, kalıcı bir URL'ye sahip ve istediğiniz tüm gelecekteki isteklerin kaynak için yeni URL kullanmalısınız istemci istemek üzere kullanılır. *301 durum kodu alındığında istemci yanıt önbelleğe alabilir.* 302 (bulundu) durum kodu, yeniden yönlendirme geçici veya genellikle konu olduğu istemci depolamak ve yeniden yönlendirme URL'sini gelecekte tekrar döndürmemelidir şekilde değiştirmek için kullanılır. Daha fazla bilgi için bkz: [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *URL yeniden yazma* farklı bir kaynak adresinden kaynak sağlamak için sunucu tarafı bir işlemdir. Bir URL yeniden yazma işlemi sunucuya gidiş gerektirmez. Yeniden URL istemciye döndürülen değil ve tarayıcının adres çubuğunda görünmez. Zaman `/resource` olan *yeniden yazılmıştır* için `/different-resource`, istemci isteklerini `/resource`ve sunucu *dahili olarak* kaynak getirir `/different-resource`. İstemci yeniden URL'sindeki kaynak alamadı olabilir, ancak istemci, isteği yapar ve yanıtı aldığında kaynağın yeniden URL'de mevcut haberdar olmaz.

![Webapı hizmet uç noktası sürüm 1 (v1) sürüm 2 (v2) sunucu üzerinde değiştirildi. Bir istemci sürüm 1 yolu /v1/api adresindeki hizmet isteği yapar. Sürüm 2 yolu /v2/api hizmetine erişmek için istek URL'si yeniden yazılmıştır. Hizmet bir 200 (Tamam) durum kodlu istemciye yanıt verir.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL yazmaksızın örnek uygulaması
URL yeniden yazma işlemi Ara yazılımla özelliklerini keşfedebilirsiniz [URL yazmaksızın örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/). Uygulama yeniden uygular ve yeniden yönlendirme kuralları ve yeniden veya yeniden yönlendirilmiş bir URL gösterir.

## <a name="when-to-use-url-rewriting-middleware"></a>URL yeniden yazma işlemi Ara kullanma zamanı
URL yeniden yazma işlemi Ara kullanamadı olduğunda kullanın [URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) Windows Server'da IIS ile [Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/) Apache sunucuda [NginxüzerindeURLyenidenyazmaişlemi](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), veya üzerinde barındırılan bir uygulamanızı [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıysa [WebListener](xref:fundamentals/servers/weblistener)). Sunucu tabanlı URL yazmaksızın teknolojileri IIS, Apache veya Nginx kullanmak için ana ara yazılım bu modülleri tüm özelliklerini desteklemeyen ve ara yazılım performansını büyük olasılıkla, modüllerin eşleşmeyecektir nedenleridir. Ancak, bazı özellikler ASP.NET Core projelerle gibi çalışmıyor sunucu modüllerinin vardır `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları. Bu senaryolarda, ara yazılımı kullanın.

## <a name="package"></a>Paket
Ara yazılım projenize eklemek için bir başvuru ekleyin [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paket. Bu özellik veya üstünü ASP.NET Core 1.1 hedef uygulamaları için kullanılabilir.

## <a name="extension-and-options"></a>Uzantı ve seçenekleri
URL yeniden yazma kurmak ve kuralları bir örneğini oluşturarak yeniden yönlendirme `RewriteOptions` sınıfı, kuralların her biri için genişletme yöntemleri ile. Birden çok kural bunları işlenen istediğiniz sırayla zincir. `RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma işlemi Ara geçirilir `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>URL yeniden yönlendirme
Kullanım `AddRedirect` isteklerini yeniden yönlendirmek için. İlk parametre gelen URL yolda eşleştirmek için regex içerir. İkinci parametre değiştirme dizedir. Üçüncü parametresi, varsa, durum kodu belirtir. Durum kodu belirtmezseniz, kaynak geçici olarak değiştirilen veya taşındığında olduğunu gösteren 302 (bulundu) varsayar.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

Geliştirici Araçları etkin bir tarayıcı yoluyla örnek uygulama için istekte `/redirect-rule/1234/5678`. Regex üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`. Yeniden yönlendirme URL'si 302 (bulundu) durum kodlu istemciye geri gönderilir. Tarayıcı, tarayıcının adres çubuğunda görünür yeniden yönlendirme URL'sinde yeni isteğinde bulunur. Yeniden yönlendirme URL'sini eşleşen kural için örnek uygulama olduğundan, ikinci isteği 200 (Tamam) yanıt uygulamadan alır ve yanıtın gövdesini yeniden yönlendirme URL'sini gösterir. Bir URL olduğunda bir geri dönüş sunucuya yapılan *yeniden yönlendirilen*.

> [!WARNING]
> Yeniden yönlendirme kurallarınızı oluştururken dikkatli olun. Yeniden yönlendirme kuralları her istekte sonra bir yeniden yönlendirme dahil olmak üzere uygulama değerlendirilir. Yanlışlıkla için kolayca sonsuz yeniden yönlendirmeleri döngüsüne oluşturun.

Özgün istek:`/redirect-rule/1234/5678`

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect.png)

Parantez içinde yer alan ifade parçası olarak adlandırılan bir *yakalama grup*. Nokta (`.`) bir ifade anlamına gelir *herhangi bir karakteri eşleştirmek*. Yıldız işareti (`*`) gösteren *sıfır veya daha fazla kez önceki karakteri eşleştirmek*. Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`. Sağladığınız istek URL'sindeki sonra herhangi bir değer `redirect-rule/` bu tek yakalama grubu tarafından kapsanır.

Dolar işareti ile dizesine eklenen değiştirme dizesini yakalanan gruplar (`$`) yakalama sıra numarası tarafından izlenen. İlk yakalama grup değeri ile elde edilen `$1`, ikinci ile `$2`, ve, regex yakalama grupları için sırayla devam eder. Yoktur tek yakalanan grubu içinde yeniden yönlendirme kuralı regex örnek uygulamasında olduğu değiştirme dizesi içinde yalnızca bir eklenen Grup şekilde `$1`. Kural uygulandığında, URL hale `/redirected/1234/5678`.

<a name="url-redirect-to-secure-endpoint"></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>Güvenli bir uç noktası için URL yeniden yönlendirme
Kullanım `AddRedirectToHttps` aynı ana bilgisayar ve HTTPS kullanarak yolu HTTP isteklerini yeniden yönlendirmek için (`https://`). Durum kodu sağlanan değil, ara yazılım 302 (bulundu) varsayılan olarak. Bağlantı noktası değil sağlandıysa ara yazılım için varsayılan olarak `null`, protokol başka bir deyişle, değişikliklerini `https://` ve istemci kaynak bağlantı noktası 443 üzerinden erişir. Örneğin, durum kodu 301 (taşınmış kalıcı olarak) ve bağlantı noktası için 5001 değiştirmek gösterilmektedir.
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
Kullanım `AddRedirectToHttpsPermanent` aynı konak ve yol güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için (`https://` bağlantı noktası 443'tür). Ara yazılım durum kodu 301 (taşınmış kalıcı olarak) ayarlar.

Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`. Add genişletme yöntemi `RewriteOptions`. Tüm URL'deki uygulamaya güvenli olmayan bir isteği oluşturun. Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısı yok sayın.

Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`:`/secure`

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https.png)

Özgün istek kullanarak `AddRedirectToHttpsPermanent`:`/secure`

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL yeniden yazma
Kullanım `AddRewrite` URL yeniden yazma işlemi için bir kural oluşturmak için. İlk parametre gelen URL yolda eşleştirmek için regex içerir. İkinci parametre değiştirme dizedir. Üçüncü parametre `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız ek yeniden yazma kuralları Atla gerekip gerekmediğini gösterir.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

Özgün istek:`/rewrite-rule/1234/5678`

![Geliştirici Araçları istek ve yanıt izleme ile bir tarayıcı penceresi](url-rewriting/_static/add_rewrite.png)

Regex fark ilk ayar şeydir (`^`) ifade başındaki. Başka bir deyişle, eşleşen URL yolunu başında başlar.

Yeniden yönlendirme kuralı ile önceki örnekte `redirect-rule/(.*)`, hiçbir ayar regex başlangıcında; bu nedenle, herhangi bir karakter koyun `redirect-rule/` yolunda başarılı bir eşleşme.

| Yol                               | Eşleştirme |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Evet   |
| `/my-cool-redirect-rule/1234/5678` | Evet   |
| `/anotherredirect-rule/1234/5678`  | Evet   |

Yeniden yazma kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız yollar yalnızca eşleşen `rewrite-rule/`. Aşağıdaki yeniden yazma kuralı ve yukarıdaki yeniden yönlendirme kuralı arasında eşleştirme içinde fark.

| Yol                              | Eşleştirme |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Evet   |
| `/my-cool-rewrite-rule/1234/5678` | Hayır    |
| `/anotherrewrite-rule/1234/5678`  | Hayır    |

Aşağıdaki `^rewrite-rule/` bölümü bir ifade, iki yakalama grubu vardır `(\d+)/(\d+)`. `\d` Güveninin *bir rakam (sayı) eşleşen*. Artı işareti (`+`) anlamına gelir *bir veya daha önceki karakteri eşleştirmek*. Bu nedenle, URL bir-başka bir sayının eğik çizgi ve ardından bir sayı içermesi gerekir. Gruplar halinde yeniden URL'si olarak eklenen bu yakalama `$1` ve `$2`. Yeniden yazma kuralı değiştirme dizesini yakalanan gruplar querystring yerleştirir. İstenen yolunu `/rewrite-rule/1234/5678` kaynak elde etmek için yeniden yazılmıştır `/rewritten?var1=1234&var2=5678`. Bir sorgu dizesi özgün istekte mevcut ise, URL yeniden yazılmıştır, korunur.

Kaynak elde etmek için sunucuya hiçbir gidiş dönüş yoktur. Kaynağın mevcut değilse, getirilen ve 200 (Tamam) durum kodlu istemciye döndürülen. İstemci yeniden yönlendirilen değil çünkü tarayıcı adres çubuğundaki URL'yi değiştirmez. İstemci ilgili oldukça hiçbir zaman URL yeniden yazma işlemi oluştu.

> [!NOTE]
> Kullanım `skipRemainingRules: true` mümkün olduğunda, çünkü eşleştirme kuralları pahalı bir işlemdir ve uygulama yanıt süresini azaltır. Hızlı uygulama yanıt için:
> * En sık eşleşen kural, yeniden yazma kuralları az sık eşleşen kural sipariş.
> * Bir eşleşme olursa ve hiçbir ek kural işlenirken gerekli olduğunda kalan kurallarının işlenmesini atlayın.

### <a name="apache-modrewrite"></a>Apache mod_rewrite
Apache mod_rewrite kurallarıyla uygulamak `AddApacheModRewrite`. Kural dosyasının uygulamayla dağıtıldığından emin olun. Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz: [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

A `StreamReader` kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

İlk parametre alan bir `IFileProvider`, aracılığıyla sağlanan [bağımlılık ekleme](dependency-injection.md). `IHostingEnvironment` Sağlamak için eklenen `ContentRootFileProvider`. İkinci parametre olan kurallar dosyanızı yoludur *ApacheModRewrite.txt* örnek uygulama.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`. Yanıt durum kodu 302 (bulundu) ' dir.

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

Özgün istek:`/apache-mod-rules-redirect/1234`

![Geliştirici isteklerini ve yanıtlarını izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Desteklenen sunucu değişkenleri
Ara yazılım aşağıdaki Apache mod_rewrite sunucu değişkenlerini destekler:
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
IIS URL yeniden yazma modülü için geçerli bir kurallar kullanmak için `AddIISUrlRewrite`. Kural dosyasının uygulamayla dağıtıldığından emin olun. Kullanılacak Ara yazılımının doğrudan yok, *web.config* Windows Server IIS üzerinde çalışırken dosya. IIS ile bu kuralları dışında depolanması gereken, *web.config* IIS yeniden yazma modülü ile çakışmaları önlemek için. Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz: [Url yeniden yazma modülü 2.0 kullanarak](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma modülü yapılandırma başvurusu](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

A `StreamReader` kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası.

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

İlk parametre alan bir `IFileProvider`, ikinci parametre olan XML kuralları dosyanızın yolu, *IISUrlRewrite.xml* örnek uygulama.

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

Örnek uygulaması isteklerden yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`. 200 (Tamam) durum kodlu istemciye gönderilen yanıtı.

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

Özgün istek:`/iis-rules-rewrite/1234`

![Geliştirici Araçları istek ve yanıt izleme ile bir tarayıcı penceresi](url-rewriting/_static/add_iis_url_rewrite.png)

Uygulamanızı istenmeyen şekilde etkileyebilecek yapılandırılmış sunucu düzeyi kurallarıyla etkin bir IIS yeniden yazma modülü varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz: [devre dışı bırakma IIS modüllerini](xref:hosting/iis-modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Desteklenmeyen özellikler

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Yayımlanan ara yazılımı ile ASP.NET Core 2.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:
* Giden kuralları
* Özel sunucu değişkenleri
* Joker karakterler
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Yayımlanan ara yazılımı ile ASP.NET Core 1.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:
* Genel kurallar
* Giden kuralları
* MAPS yeniden yazma
* CustomResponse eylemi
* Özel sunucu değişkenleri
* Joker karakterler
* Eylem: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Desteklenen sunucu değişkenleri
Ara yazılım aşağıdaki IIS URL yeniden yazma modülünü sunucu değişkenleri destekler:
* CONTENT_LENGTH İS SIFIRDAN BÜYÜK
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
> Edinebilirsiniz bir `IFileProvider` aracılığıyla bir `PhysicalFileProvider`. Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir. Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtılan emin olun.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Yöntem temelli kuralı
Kullanım `Add(Action<RewriteContext> applyRule)` bir yöntem kendi kural mantığı uygulamak için. `RewriteContext` Sunan `HttpContext` yönteminizi kullanmak için. `context.Result` Nasıl ek ardışık düzen belirler işleme işlenir.

| bağlamı. Sonuç                       | Eylem                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules`(varsayılan) | Kuralları uygulama devam                                         |
| `RuleResult.EndResponse`             | Kuralları uygulanmasını durdurmak ve yanıtı gönder                       |
| `RuleResult.SkipRemainingRules`      | Kuralları uygulanmasını durdurmak ve bağlam için bir sonraki ara yazılım gönderin |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

Örnek uygulama ile biten istekleri yollar için yönlendiren bir yöntem gösterilmektedir *.xml*. Bir istek yapıyorsa verilen `/file.xml`, onu yönlendireceği `/xmlfiles/file.xml`. Durum kodu 301 (taşınmış kalıcı olarak) ayarlanır. İçin bir yeniden yönlendirme, açıkça yanıtın durum kodu ayarlamanız gerekir; Aksi takdirde, 200 (Tamam) durum kodu döndürülür ve yeniden yönlendirme istemcide karşılaşılmaz.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

Özgün istek:`/file.xml`

![Geliştirici isteklerin ve yanıtların file.xml için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>IRule tabanlı kuralı
Kullanım `Add(IRule)` türeyen bir sınıf kendi kural mantığı uygulamak için `IRule`. Kullanarak bir `IRule` yöntemi tabanlı kural yaklaşım kullanarak üzerinde daha fazla esneklik sağlar. Burada, iletebilir parametrelerde bir oluşturucu, türetilmiş bir sınıf içerebilir `ApplyRule` yöntemi.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

Örnek uygulama için parametre değerlerini `extension` ve `newPath` birkaç koşullara uyan denetlenir. `extension` Bir değer içermelidir ve değeri olmalıdır *.png*, *.jpg*, veya *.gif*. Varsa `newPath` geçerli olmayan bir `ArgumentException` oluşturulur. Bir istek yapıyorsa verilen *image.png*, onu yönlendireceği `/png-images/image.png`. Bir istek yapıyorsa verilen *image.jpg*, onu yönlendireceği `/jpg-images/image.jpg`. Durum kodu 301 (kalıcı taşınmış) olarak ayarlanır ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

Özgün istek:`/image.png`

![Geliştirici isteklerin ve yanıtların image.png için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_png_requests.png)

Özgün istek:`/image.jpg`

![Geliştirici isteklerin ve yanıtların image.jpg için izleme araçları ile bir tarayıcı penceresi](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Regex örnekleri

| Hedef | Regex dize &<br>Eşleşme örneği | Değiştirme dizesini &<br>Çıkış örneği |
| ---- | :-----------------------------: | :------------------------------------: |
| Yol querystring yeniden yazma | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Şerit eğik | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Sondaki eğik çizgi zorla | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Belirli isteklere yeniden yazma işlemi kaçının | `(.*[^(\.axd)])$`<br>Evet:`/resource.htm`<br>Hayır:`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| URL kesimleri yeniden düzenleme | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| URL kesimi Değiştir | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Ek kaynaklar
* [Uygulama başlatma](startup.md)
* [Ara yazılım](middleware.md)
* [.NET normal ifadeler](/dotnet/articles/standard/base-types/regular-expressions)
* [Normal ifade dili - hızlı başvuru](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [URL yeniden yazma modülünü 2.0 (IIS için) kullanma](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL yeniden yazma modülü yapılandırma başvurusu](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [IIS URL yeniden yazma modülü Forumu](https://forums.iis.net/1152.aspx)
* [Basit bir URL yapıya tutun](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 URL yeniden yazma ipuçları ve püf noktaları](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Eğik çizgi veya eğik çizgi yok](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
