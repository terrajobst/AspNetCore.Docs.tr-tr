---
title: URL yeniden yazma ara yazılımı ASP.NET core'da
author: guardrex
description: URL yeniden yazma ve URL yeniden yazma ara yazılımı ile ASP.NET Core uygulamalarında yeniden yönlendirme hakkında bilgi edinin.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326075"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL yeniden yazma ara yazılımı ASP.NET core'da

Tarafından [Luke Latham](https://github.com/guardrex) ve [Mikael Mengistu](https://github.com/mikaelm12)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

URL yeniden yazma URL bir veya daha fazla önceden tanımlanmış kurallara göre istek değiştirme işlemidir. Böylece konumları ve adresleri sıkı şekilde bağlı olmayan URL yeniden yazma kaynak konumları ve adresleri arasında bir Özet oluşturur. URL yeniden yazma yararlı olduğu bazı senaryolar vardır:

* Taşıma veya bu kaynaklar için kararlı bulucular korurken sunucu kaynaklarını geçici veya kalıcı olarak değiştiriliyor.
* İstek farklı uygulamalar arasında veya tek bir uygulama alanları genelinde işleme bölme.
* Kaldırma, ekleme veya URL kesimleri gelen isteklerden yeniden düzenleme.
* En iyi duruma getirme Genel URL'ler arama motoru iyileştirmesi (SEO).
* Bir bağlantıyı izleyerek bulabilirsiniz içeriği tahmin kolaylaştıracak kolay genel URL kullanımını erişimine izin verme.
* Uç noktalarını güvenli hale getirmek için güvenli istekleri yeniden yönlendirme.
* Görüntü hotlinking engelliyor.

URL çeşitli şekillerde değiştirme, Regex, Apache mod_rewrite modülü kuralları, IIS yeniden yazma modülü kuralları da dahil olmak üzere ve özel kural mantığı kullanarak kurallar tanımlayabilirsiniz. Bu belge, URL yeniden yazma URL yeniden yazma ara yazılımı ASP.NET Core uygulamalarında kullanma hakkında yönergeler sunar.

> [!NOTE]
> URL yeniden yazma uygulama performansını düşürebilir. Uygun olduğunda, sayısı ve karmaşıklığı kurallar sınırlamanız gerekir.

## <a name="url-redirect-and-url-rewrite"></a>URL yeniden yönlendirme ve URL yeniden yazma

İfade arasındaki farkı *URL yeniden yönlendirme* ve *URL yeniden yazma* en küçük görünebilir ancak ilk kaynakları istemcilere sağlamak için önemli etkileri vardır. ASP.NET Core'nın URL yeniden yazma ara yazılımı her ikisi de gereksinimini toplantı yeteneğine sahiptir.

A *URL yeniden yönlendirme* istemci burada belirtildiği başka bir adresten bir kaynağa erişmek için bir istemci tarafı işlemi olduğundan. Bu sunucu için bir gidiş dönüş gerektirir. İstemci kaynak için yeni bir istekte bulunduğunda istemciye döndürülen yeniden yönlendirme URL'sini tarayıcınızın adres çubuğunda görünür. 

Varsa `/resource` olduğu *yeniden yönlendirilen* için `/different-resource`, istemci isteklerini `/resource`. İstemci kaynak edinmelidir sunucunun yanıt `/different-resource` yeniden yönlendirme geçici veya kalıcı olduğunu gösteren bir durum kodu ile. İstemci yeniden yönlendirme URL'sini kaynak için yeni bir isteği yürütür.

![Webapı hizmet uç noktası sunucusundaki sürüm 2 (v2) 1 (v1) sürümünden geçici olarak değiştirildi. Bir istemci, sürüm 1 yolu /v1/api hizmeti için bir istek gönderir. Sunucu, sürüm 2 /v2/api hizmet için yeni, geçici yoluyla 302 bir (bulunamadı) yanıtı geri gönderir. İstemci, hizmeti yeniden yönlendirme URL'si için ikinci bir talep gönderir. Sunucu bir 200 (Tamam) durum koduyla yanıt verir.](url-rewriting/_static/url_redirect.png)

İstekleri için farklı bir URL yeniden yönlendirme, yeniden yönlendirme kalıcı veya geçici olup olmadığını gösterir. 301 (kalıcı taşındı) durum kodunu burada yeni, kalıcı bir URL kaynağı vardır ve istediğiniz tüm gelecek istekleri kaynak için yeni URL kullanması gerektiğini istemci istemek için kullanılır. *301 durum kodu alındığında istemci yanıtı önbelleğe alabilir.* (Bulunamadı) durum kodunu 302 yeniden yönlendirme geçici veya genel konu olduğu istemci depolamak olmamalıdır ve gelecekte yeniden yönlendirme URL'sini yeniden şekilde değiştirmek için kullanılır. Daha fazla bilgi için [RFC 2616: durum kodu tanımları](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *URL yeniden yazma* farklı bir kaynak adresten bir kaynak sağlamak için bir sunucu tarafı işlemi. Bir URL yeniden yazma sunucuya gidiş dönüşlü hale getirmek gerekmez. Yeniden URL istemciye döndürülen değildir ve tarayıcının adres çubuğunda görüntülenmez. Zaman `/resource` olduğu *yazılan* için `/different-resource`, istemci isteklerini `/resource`ve sunucu *dahili olarak* kaynakta getirir `/different-resource`. İstemci yeniden URL'deki kaynak alınamadı olabilir, ancak istemci, isteği yapar ve yanıtı alır, kaynağın yeniden URL'de mevcut haberdar olmaz.

![Webapı hizmet uç noktası, sunucu üzerindeki sürüm 2 (v2) için 1 (v1) sürümünden değiştirildi. Bir istemci, sürüm 1 yolu /v1/api hizmeti için bir istek gönderir. İstek URL'si, sürüm 2 yolu /v2/api hizmetine erişmek için yeniden. Hizmet istemcisi bir 200 (Tamam) durum kodu ile yanıt verir.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL yeniden yazma örnek uygulaması

URL yeniden yazma ara yazılımı ile özelliklerini inceleyebilirsiniz [URL yeniden yazma örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Uygulamaya yeniden uygular ve yeniden yönlendirme kuralları ve yeniden ya da yeniden yönlendirilen URL'sini gösterir.

## <a name="when-to-use-url-rewriting-middleware"></a>URL yeniden yazma ara yazılımı kullanma zamanı

URL yeniden yazma ara yazılımı kullanamaz olduğunda kullanın [URL yeniden yazma Modülü](https://www.iis.net/downloads/microsoft/url-rewrite) Windows Server, IIS ile [Apache mod_rewrite Modülü](https://httpd.apache.org/docs/2.4/rewrite/) Apache sunucusundaki [NgınxüzerindeURLyenidenyazma](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), veya uygulamanız üzerinde barındırılan [HTTP.sys sunucu](xref:fundamentals/servers/httpsys) (eski adıyla [WebListener](xref:fundamentals/servers/weblistener)). Sunucu tabanlı URL yeniden yazma teknolojileri IIS, Apache veya Ngınx kullanmak için ana ara yazılım, bu modüllerin tam özelliklerini desteklemiyor ve ara yazılım performansını büyük olasılıkla, modül eşleşmeyecektir nedenleridir. Ancak, ASP.NET Core projeleriyle gibi çalışmayan sunucu modülü bazı özellikler mevcuttur `IsFile` ve `IsDirectory` IIS yeniden yazma modülü kısıtlamaları. Bu senaryolarda, ara yazılım kullanın.

## <a name="package"></a>Paket

Ara yazılım projenize eklemek için bir başvuru ekleyin. [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) paket. Bu özellik, ASP.NET Core 1.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.

## <a name="extension-and-options"></a>Uzantı ve seçenekleri

URL yeniden yazma oluşturmak ve bir örneğini oluşturarak kuralları yeniden yönlendirme [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) sınıfı, kuralların her biri için genişletme yöntemleri. İşlenen bunları istediğiniz sırayla birden çok kural zincir. `RewriteOptions` İle istek ardışık düzenine eklenen URL yeniden yazma ara yazılımı geçirilen `app.UseRewriter(options);`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a>Www www olmayan yeniden yönlendirme

Üç seçenek olmayan yönlendirmek için uygulama izin`www` ister `www`:

* [AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; kalıcı olarak isteği yönlendirmek `www` istek olmayan ise alt etki alanı`www`. İle yeniden yönlendiren bir [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) durum kodu.
* [AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`. İle yeniden yönlendiren bir [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) durum kodu.
* [AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; isteği yönlendirmek `www` gelen istek olmayan ise alt etki alanı`www`. Yanıt durum kodu girmenizi sağlar. Alanlarını kullanın [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) sınıfı atamaların `AddRedirectToWww`.

::: moniker-end

### <a name="url-redirect"></a>URL yeniden yönlendirme

Kullanım `AddRedirect` isteklerini yeniden yönlendirmek için. İlk parametre, normal ifade gelen URL yolunda eşleşen içerir. İkinci değiştirme dizesi parametresidir. Üçüncü parametre varsa, durum kodu belirtir. Durum kodu belirtmezseniz, kaynak geçici olarak taşındı veya değiştirilinceye olduğunu gösteren 302 (bulunamadı) için varsayılanları.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

Geliştirici Araçları etkin bir tarayıcıda yolu kullanarak örnek uygulamaya istekte bulunmak `/redirect-rule/1234/5678`. Normal ifade üzerinde istek yolunun eşleşmesi `redirect-rule/(.*)`, ve yolu ile değiştirilir `/redirected/1234/5678`. Yeniden yönlendirme URL'si istemcisine bir 302 (bulunamadı) durum kodu ile gönderilir. Tarayıcı, tarayıcının adres çubuğunda görüntülenen yeniden yönlendirme URL'sindeki yeni bir istek gönderir. Örnek uygulamada hiçbir kural üzerinde yeniden yönlendirme URL'si aynı olduğundan, ikinci isteği uygulamadan 200 (Tamam) yanıtı alır ve yanıt gövdesinin yeniden yönlendirme URL'sini gösterir. Bir URL ise bir gidiş dönüş sunucuya yapılan *yeniden yönlendirilen*.

> [!WARNING]
> Yeniden yönlendirme kurallarınızı oluştururken dikkatli olun. Yeniden yönlendirme kuralları, sonra bir yeniden yönlendirme de dahil olmak üzere uygulama, her istekte değerlendirilir. Yanlışlıkla için kolay bir sonsuz yeniden yönlendirmeleri döngüsünü oluşturun.

Özgün istek: `/redirect-rule/1234/5678`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect.png)

Parantez içinde yer alan bir ifade parçası olarak adlandırılan bir *yakalama grubu*. Nokta (`.`) ifade anlamına gelir *herhangi bir karakterle eşleştir*. Yıldız işareti (`*`) gösteren *önceki karakteri sıfır veya daha fazla kez eşleştir*. Bu nedenle, son iki yol kesimleri URL'sinin `1234/5678`, yakalama grubu tarafından yakalanan `(.*)`. Sonra istek URL'sindeki sağladığınız herhangi bir değer `redirect-rule/` bu tek bir yakalama grubu tarafından yakalanır.

Değiştirme dizesinde yakalanan gruplar dolar işareti ile dizesine eklenen (`$`) yakalama dizisi sayı takip eder. İlk yakalama grubu değer ile elde edilen `$1`ikinci ile `$2`, ve bunların sırası, normal ifade yakalama grupları için devam edin. Var. yalnızca bir yakalanan gruba içinde yeniden yönlendirme kuralı regex örnek uygulamada, olan değiştirme dizesinde tek eklenen grubu için `$1`. Kural uygulandığında, URL olur `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Güvenli bir uç noktası için URL yeniden yönlendirme

Kullanım `AddRedirectToHttps` aynı konak ve yol HTTPS kullanarak HTTP isteklerini yeniden yönlendirmek için (`https://`). Durum kodu verilmiyorsa, ara yazılım 302 (bulunamadı) için varsayılan olarak. Bağlantı noktası verilmiyorsa, ara yazılım için varsayılan olarak `null`, Protokolü yani değişikliklerini `https://` ve istemci kaynak bağlantı noktası 443 üzerinden erişir. Örneğin, durum kodu 301 (kalıcı taşındı) ve bağlantı noktasını değiştirmek için 5001 gösterilmektedir.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Kullanım `AddRedirectToHttpsPermanent` aynı konak ve yol güvenli HTTPS protokolü ile güvenli isteklerini yeniden yönlendirmek için (`https://` bağlantı noktası 443 üzerinden). Ara yazılım 301 (kalıcı taşındı) durum kodunu ayarlar.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> HTTPS için ek yeniden yönlendirme kuralları gereksinimi olmadan yönlendirirken, HTTPS yeniden yönlendirmesi ara yazılım kullanmanızı öneririz. Daha fazla bilgi için [HTTPS zorlama](xref:security/enforcing-ssl#require-https) konu.

Örnek uygulamayı nasıl kullanılacağını gösteren özellikli `AddRedirectToHttps` veya `AddRedirectToHttpsPermanent`. Uzantı yöntemine ekleyin `RewriteOptions`. Güvenli olmayan bir istek, herhangi bir URL konumundaki uygulamaya olun. Tarayıcı güvenlik otomatik olarak imzalanan sertifika güvenilmeyen uyarısını kapatmak veya sertifikaya güvenmek için bir özel durum oluşturun.

Özgün istek kullanarak `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https.png)

Özgün istek kullanarak `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Tarayıcının geliştirici araçları istek ve yanıtların izleme ile](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL yeniden yazma

Kullanım `AddRewrite` URL yeniden yazma için bir kural oluşturmak için. İlk parametre gelen URL yolu temel eşlemek için normal ifade içeriyor. İkinci değiştirme dizesi parametresidir. Üçüncü parametreyi `skipRemainingRules: {true|false}`, ara yazılımı için geçerli kural uyguladıysanız, ek bir yeniden yazma kuralları atlamak gerekip gerekmediğini belirtir.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

Özgün istek: `/rewrite-rule/1234/5678`

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_rewrite.png)

Simgeyi seçtiğinizde normal ifade fark ilk şey olduğunu (`^`) ifadenin başında. Başka bir deyişle, eşleşen bir URL yolu başlangıcında başlar.

Yeniden yönlendirme kuralı ile bir önceki örnekte `redirect-rule/(.*)`, normal ifade başlangıcında hiçbir ayar yoktur; bu nedenle, herhangi bir karakter önünde `redirect-rule/` yolunda başarılı bir eşleşme.

| Yol                               | Eşleştirme |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Evet   |
| `/my-cool-redirect-rule/1234/5678` | Evet   |
| `/anotherredirect-rule/1234/5678`  | Evet   |

Yeniden üretme kuralı `^rewrite-rule/(\d+)/(\d+)`, ile başlatırsanız, yalnızca yollarıyla eşleşen `rewrite-rule/`. İçinde eşleşen aşağıdaki yeniden yazma kuralı ve yukarıdaki yeniden yönlendirme kuralı farka dikkat edin.

| Yol                              | Eşleştirme |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Evet   |
| `/my-cool-rewrite-rule/1234/5678` | Hayır    |
| `/anotherrewrite-rule/1234/5678`  | Hayır    |

Aşağıdaki `^rewrite-rule/` bölümü ifadesi, iki yakalama grupları vardır `(\d+)/(\d+)`. `\d` Gösterir *bir basamağı (sayı) eşleşen*. Artı işaretini (`+`) anlamına gelir *bir veya daha önceki karakter eşleşen*. Bu nedenle, URL, başka bir sayının İleri-eğik çizgi ardından bir sayı içermesi gerekir. Bu yakalama grupları yeniden URL eklenmiş `$1` ve `$2`. Yeniden yazma kuralı değiştirme dizesi yakalanan gruplar querystring yerleştirir. İstenen yolunu `/rewrite-rule/1234/5678` kaynağı almak için yazılan `/rewritten?var1=1234&var2=5678`. Özgün istek üzerinde bir sorgu dizesi varsa, URL'yi yeniden zaman korunur.

Hiçbir geri dönüş kaynağı almak için sunucuya yoktur. Kaynağın varolup olmadığını getirildi ve 200 (Tamam) durum kodu ile istemciye döndürülen. Tarayıcı adres çubuğundaki URL'yi, istemci yeniden yönlendirilen değildir çünkü değiştirmez. İstemci endişelenmiştir kadar hiçbir zaman URL yeniden yazma işlemi oluştu.

> [!NOTE]
> Kullanım `skipRemainingRules: true` mümkün olduğunda, çünkü kurallarına pahalı bir işlemdir ve uygulama yanıt süresini azaltır. Hızlı Uygulama yanıtı:
> * En sık eşleşen kural, yeniden yazma kuralları az sık eşleşen kural sipariş.
> * Bir eşleşme olursa ve hiçbir ek kural işleme gerekli olduğunda kalan kurallarının işlenmesini atlayın.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Apache mod_rewrite kurallarıyla uygulamak `AddApacheModRewrite`. Kurallar dosyası uygulamayla dağıtıldığından emin olun. Daha fazla bilgi ve mod_rewrite kuralları örnekleri için bkz. [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` kurallardan okumak için kullanılan *ApacheModRewrite.txt* kurallar dosyası.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

İlk parametre bir `IFileProvider`, aracılığıyla sağlanan [bağımlılık ekleme](dependency-injection.md). `IHostingEnvironment` Sağlamak için eklenen `ContentRootFileProvider`. İkinci parametre olan kurallar dosyası yolu olan *ApacheModRewrite.txt* örnek uygulamada.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

Örnek uygulama isteklerinden yönlendiren `/apache-mod-rules-redirect/(.\*)` için `/redirected?id=$1`. Yanıt durum kodu 302 (bulunamadı) ' dir.

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

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

IIS URL yeniden yazma modülü uygulanan kurallarını kullanmak için `AddIISUrlRewrite`. Kurallar dosyası uygulamayla dağıtıldığından emin olun. Kullanılacak Ara yazılımının doğrudan yoksa, *web.config* dosya Windows Server IIS üzerinde çalışırken. IIS ile bu kuralları dışında depolanması gereken, *web.config* IIS yeniden yazma modülü ile çakışmalarını önlemek için. Daha fazla bilgi ve IIS URL yeniden yazma modülü kuralları örnekleri için bkz. [Url yeniden yazma modülü 2.0 kullanarak](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) ve [URL yeniden yazma Module yapılandırma başvurusu](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` kurallardan okumak için kullanılan *IISUrlRewrite.xml* kurallar dosyası.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

İlk parametre bir `IFileProvider`, ikinci parametre olan, XML kuralları dosyası yolu olsa da *IISUrlRewrite.xml* örnek uygulamada.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

Örnek uygulama, gelen istekleri yeniden yazar `/iis-rules-rewrite/(.*)` için `/rewritten?id=$1`. İstemciye bir 200 (Tamam) durum koduyla yanıt gönderilir.

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Özgün istek: `/iis-rules-rewrite/1234`

![Tarayıcının geliştirici araçları istek ve yanıt izleme](url-rewriting/_static/add_iis_url_rewrite.png)

Etkin bir IIS yeniden yazma modülü ile uygulamanızı istenmeyen yollarla erişememeleri yapılandırılmış sunucu düzeyinde kurallar varsa, IIS yeniden yazma modülü bir uygulama için devre dışı bırakabilirsiniz. Daha fazla bilgi için [devre dışı bırakma IIS modüllerini](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Desteklenmeyen özellikler

::: moniker range=">= aspnetcore-2.0"

Yayımlanan bir ara yazılım ile ASP.NET Core 2.x, aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:

* Giden kuralları
* Özel sunucu değişkenleri
* Joker karakterler
* LogRewrittenUrl

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Yayımlanan bir ara yazılım ile ASP.NET Core 1.x aşağıdaki IIS URL yeniden yazma modülü özellikleri desteklemez:

* Genel kurallar
* Giden kuralları
* Yeniden eşlemeleri
* CustomResponse eylemi
* Özel sunucu değişkenleri
* Joker karakterler
* Eylem: CustomResponse
* LogRewrittenUrl

::: moniker-end

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
> De edinebilirsiniz bir `IFileProvider` aracılığıyla bir `PhysicalFileProvider`. Bu yaklaşım, yeniden yazma konumu için daha fazla esneklik kuralları dosyaları sağlayabilir. Sağladığınız yolun sunucusuna yeniden yazma kuralları dosyalarınızı dağıtıldığından emin emin olun.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Metot tabanlı kuralı

Kullanım `Add(Action<RewriteContext> applyRule)` bir yöntemde kendi kural mantığı uygulamak için. `RewriteContext` Sunan `HttpContext` yönteminiz olarak kullanmak için. `context.Result` Nasıl ek işlem hattı belirler işleme gerçekleştirilir.

| bağlamı. Sonuç                       | Eylem                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (varsayılan) | Devam kuralları uygulama                                         |
| `RuleResult.EndResponse`             | Kuralları uygulanmasını durdurmak ve yanıtı gönder                       |
| `RuleResult.SkipRemainingRules`      | Kuralları uygulanmasını durdurmak ve sonraki Ara yazılıma bağlamı gönderme |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

Örnek uygulama ile biten istekleri yolları için yönlendiren bir yöntemi gösterir *.xml*. Bir istek yapıp yapmadığını `/file.xml`, onu yönlendireceği `/xmlfiles/file.xml`. Durum kodu 301 (kalıcı taşındı) ayarlanır. İçin bir yeniden yönlendirme, yanıtın durum kodu açıkça ayarlamalısınız; Aksi takdirde, bir 200 (Tamam) durum kodu döndürülür ve istemcide yeniden yönlendirme karşılaşılmaz.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Özgün istek: `/file.xml`

![Tarayıcının geliştirici araçları istek ve yanıtların file.xml için izleme ile](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Kural Irule tabanlı

Kullanım `Add(IRule)` türetildiği bir sınıf kendi kural mantığı kullanmak `IRule`. Kullanarak bir `IRule` yöntemi dayalı kural yaklaşımı kullanarak üzerinde daha fazla esneklik sağlar. Burada, geçirebilirsiniz parametreleri için bir oluşturucu, türetilmiş sınıfınızın içerebilir `ApplyRule` yöntemi.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

Örnek uygulama için parametre değerlerini `extension` ve `newPath` çeşitli koşullara uyması için denetlenir. `extension` Bir değer içermelidir ve değer olmalıdır *.png*, *.jpg*, veya *.gif*. Varsa `newPath` geçerli olmayan bir `ArgumentException` oluşturulur. Bir istek yapıp yapmadığını *image.png*, onu yönlendireceği `/png-images/image.png`. Bir istek yapıp yapmadığını *image.jpg*, onu yönlendireceği `/jpg-images/image.jpg`. Durum kodu 301 (kalıcı taşındı) olarak ayarlanır ve `context.Result` kural işlemeyi durdur ve yanıt göndermek için ayarlanır.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Özgün istek: `/image.png`

![Tarayıcının geliştirici araçları istek ve yanıtların image.png için izleme ile](url-rewriting/_static/add_redirect_png_requests.png)

Özgün istek: `/image.jpg`

![Tarayıcının geliştirici araçları istek ve yanıtların image.jpg için izleme ile](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Normal ifade örnekleri

| Hedef | Normal ifade dizesini &<br>Eşleşme örneği | Değiştirme dizesi &<br>Çıkış örneği |
| ---- | :-----------------------------: | :------------------------------------: |
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
* [10 URL yeniden yazma, ipuçları ve püf noktaları](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Eğik çizgi veya değil eğik çizgi](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
