---
title: ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma
author: guardrex
description: Proxy sunucuları ve yük Dengeleyiciler, genellikle önemli bilgi gizlememeniz arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: bcafc33b8faf81912d536d3df8941d196685ecad
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837359"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma

Tarafından [Luke Latham](https://github.com/guardrex) ve [Chris Ross](https://github.com/Tratcher)

ASP.NET Core için önerilen yapılandırma, IIS/ASP.NET çekirdek modülü, Ngınx veya Apache kullanarak uygulamayı barındırılır. Proxy sunucuları, yük Dengeleyiciler ve diğer ağ Gereçleri, isteğiyle ilgili bilgileri genellikle uygulama ulaşmadan önce gizlememeniz:

* HTTPS isteklerini HTTP üzerinden taşınır, özgün şeması (HTTPS) kaybolur ve bir üst bilgisinde iletilmesi gerekir.
* Uygulama proxy'si ve doğru kaynağına değil Internet veya kurumsal ağ üzerindeki bir istek aldığından, kaynak istemci IP adresi de üst bilgisinde gönderilmelidir.

Bu bilgiler, örneğin yeniden yönlendirmeleri, kimlik doğrulaması, bağlama oluşturmayı, ilke değerlendirmesi ve istemci coğrafi konum isteği işleme önemli olabilir.

## <a name="forwarded-headers"></a>İletilen üstbilgileri

Kural gereği, HTTP üst bilgisinde proxy'leri iletin.

| Üstbilgi | Açıklama |
| ------ | ----------- |
| X-iletilen-için | Proxy, zincirdeki sonraki proxy ve istek başlatılan istemci ile ilgili bilgileri tutar. Bu parametre, IP adresleri (ve isteğe bağlı olarak, bağlantı noktası numaraları) içerebilir. Bir proxy sunucu zincirinde ilk parametre istek ilk yapıldığı istemci gösterir. Sonraki proxy tanımlayıcıları izleyin. Son proxy zincirdeki parametreler listesinde değil. Son proxy IP adresini ve isteğe bağlı olarak bir bağlantı noktası numarası aktarım katmanında uzak IP adresi olarak kullanılabilir. |
| X iletilen Proto | Kaynak Düzen (HTTP/HTTPS) değeri. İstek birden çok proxy'leri geçiş değilse değer düzenleri listesi de olabilir. |
| X iletilen konak | Ana bilgisayar üstbilgi alanının özgün değer. Genellikle proxy ana bilgisayar üst bilgisini değiştirmez. Bkz: [Microsoft Güvenlik Danışma CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) proxy burada değil veya doğrulamak için iyi bilinen değerler barındırma üstbilgileri kısıtlamak sistemleri etkileyen ayrıcalıklar yükseltme güvenlik açığı hakkında bilgi için. |

İletilen üstbilgileri Ara gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketi, bu üst bilgilerini okur ve ilişkili alanları doldurur <xref:Microsoft.AspNetCore.Http.HttpContext>.

Ara yazılım güncelleştirmeleri:

* [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; kullanılarak ayarlanan `X-Forwarded-For` üstbilgi değeri. Ek ayarları etkiler nasıl ara yazılım ayarlar `RemoteIpAddress`. Ayrıntılar için bkz [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; kullanılarak ayarlanan `X-Forwarded-Proto` üstbilgi değeri.
* [HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; kullanılarak ayarlanan `X-Forwarded-Host` üstbilgi değeri.

Üst bilgileri ara yazılım iletilen [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir. Varsayılan ayarlar şunlardır:

* Yok, yalnızca *bir proxy* uygulama ve kaynak istekleri arasında.
* Yalnızca bir geri döngü adresine bilinen proxy'leri için yapılandırılır ve bilinen ağlar.
* İletilen üstbilgileri adlı `X-Forwarded-For` ve `X-Forwarded-Proto`.

Tüm ağ Gereçleri ekleyin `X-Forwarded-For` ve `X-Forwarded-Proto` ek yapılandırma olmadan üstbilgileri. Bunlar uygulamaya ulaştığında bu üstbilgileri proxy istekleri içermeyen Gereci üreticinin Kılavuzu danışın. Gereç değerinden farklı bir üst bilgi adları kullanıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto`ayarlayın <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> gereç tarafından kullanılan üstbilgi adlarını eşleştirmek için Seçenekler. Daha fazla bilgi için [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı bir üst bilgi adları kullanan bir proxy yapılandırması](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express ve ASP.NET Core Modülü

İletilen üstbilgileri ara yazılım, varsayılan olarak etkindir [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) uygulamayı barındırılan zaman [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model) IIS ve ASP.NET Core modülü. İletilen üstbilgileri ara yazılım, ASP.NET Core modülü iletilen üst bilgilerine güven sorunları nedeniyle ilk ara yazılım ardışık düzenini belirli kısıtlı bir yapılandırma ile olarak çalıştırmak için etkinleştirilir (örneğin, [IP yanıltma](https://www.iplocation.net/ip-spoofing)). Ara yazılım iletecek şekilde yapılandırılmış `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri ve tek localhost proxy ile sınırlıdır. Ek yapılandırma gerekli olup olmadığını, [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Diğer bir ara sunucu ve yük dengeleyici senaryoları

Kullanarak dışında [IIS tümleştirme](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) barındırırken [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model), iletilen üstbilgileri ara yazılım, varsayılan olarak etkin değildir. İletilen üstbilgileri ara yazılım etkin, iletilen işlemi üst bilgilerine bir uygulama için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>. Hayır ise, ara yazılım etkinleştirdikten sonra <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> ara yazılımı varsayılan belirtilen [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) olan [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).

Ara yazılımla yapılandırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> iletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerinde `Startup.ConfigureServices`. Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` diğer ara yazılımdan çağırmadan önce:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Hayır ise <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilen `Startup.ConfigureServices` veya doğrudan genişletme yöntemi ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, iletmek için varsayılan başlıkları [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders). <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> Özelliği iletmek üzere üst bilgileri ile yapılandırılması gerekir.

## <a name="nginx-configuration"></a>Ngınx yapılandırma

İletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini bkz <xref:host-and-deploy/linux-nginx#configure-nginx>. Daha fazla bilgi için [NGINX: İletilen üst bilgisini kullanarak](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Apache yapılandırma

`X-Forwarded-For` otomatik olarak eklenir (bkz [Apache modülü mod_proxy: Ters proxy istek üstbilgilerini](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). İletme hakkında bilgi için `X-Forwarded-Proto` başlığını görmek <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>İletilen üstbilgileri ara yazılım seçenekleri

<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> İletilen üstbilgileri Ara yazılımının davranışını denetler. Aşağıdaki örnek, varsayılan değerleri değiştirir:

* İletilen üst bilgilerde giriş sayısını sınırlamak `2`.
* Bir bilinen proxy adresini ekleyin `127.0.10.1`.
* Varsayılan iletilen üst bilgi adı değiştirmek `X-Forwarded-For` için `X-Forwarded-For-My-Custom-Header-Name`.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| Seçenek | Açıklama |
| ------ | ----------- |
| AllowedHosts | Ana bilgisayar tarafından sınırlar `X-Forwarded-Host` sağlanan değerler için üst bilgi.<ul><li>Sıra yoksay örneği kullanarak değerleri karşılaştırılır.</li><li>Bağlantı noktası numaralarını tutulması gerekir.</li><li>Liste boşsa, tüm konaklar izin verilir.</li><li>Üst düzey bir joker karakter `*` tüm boş konaklar sağlar.</li><li>Alt etki alanı joker karakterlere izin verilir, ancak kök etki alanı eşleşmiyor. Örneğin, `*.contoso.com` alt etki alanıyla eşleşen `foo.contoso.com` ancak kök etki alanı değil `contoso.com`.</li><li>Unicode ana bilgisayar adları kullanılabilir, ancak dönüştürülür [Punycode](https://tools.ietf.org/html/rfc3492) eşlemek için.</li><li>[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) ayraçlar sınırlayıcı içerir ve olması gereken [geleneksel form](https://tools.ietf.org/html/rfc4291#section-2.2) (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6 adresleri farklı biçimler arasında mantıksal eşitlik denetlemek için özel harfleri değil ve yok Standartlaştırma gerçekleştirilir.</li><li>İzin verilen konakları sınırlamak için başarısızlık, bir saldırganın hizmeti tarafından oluşturulan bağlantıları sızmasını.</li></ul>Boş bir varsayılan değer: `IList<string>`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-For` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Hangi ileticileri işlenmesi gerektiğini tanımlar. Bkz: [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) için geçerli olan alanların listesi. Bu özelliğe atanmış tipik değerler ' ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.<br><br>Varsayılan değer [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders). |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Host` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Proto` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | İşlenen üstbilgileri içerisindeki giriş sayısını sınırlar. Kümesine `null` sınırı, ancak bunu devre dışı bırakmak için yalnızca, yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.<br><br>Varsayılan değer 1'dir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | Adres aralıklarını iletilen üst bilgiler kabul etmek için bilinen ağlar. Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanan IP aralıklarını belirtin.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir `IList` \< <xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> için tek bir giriş içeren `IPAddress.Loopback`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | İletilen üst bilgiler kabul etmek için bilinen Proxy adresleri. Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir `IList` \< <xref:System.Net.IPAddress>> için tek bir giriş içeren `IPAddress.IPv6Loopback`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).<br><br>Varsayılan, `X-Original-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).<br><br>Varsayılan, `X-Original-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Üstbilgi değerleri arasında eşit olacak şekilde sayısı gerektir [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) işleniyor.<br><br>ASP.NET Core 1.x olan varsayılan `true`. ASP.NET Core 2.0 veya sonraki sürümlerde varsayılan `false`. |

## <a name="scenarios-and-use-cases"></a>Senaryolar ve kullanım örnekleri

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Üst bilgiler ve tüm istekleri iletilen eklemek mümkün olmadığında güvenli

Bazı durumlarda, uygulamaya proxy istekleri iletilen üstbilgilerini eklemek mümkün olmayabilir. Tüm genel dış istekler HTTPS olduğunu proxy zorlama, Düzen el ile ayarlanabilir `Startup.Configure` herhangi bir türde bir ara yazılım kullanmadan önce:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Bu kod bir ortam değişkenine ya da diğer geliştirme ya da hazırlık ortamı yapılandırma ayarı ile devre dışı bırakılabilir.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Yolu temel ve istek yolu değiştirmek proxy'ler ile Dağıt

Bazı Ara sunucular yolu sağlam geçirmek ancak bir uygulamayla yönlendirme böylece kaldırılmalıdır temel yolu düzgün çalışır. [UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) ara yazılım yolu böler [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) ve uygulama temel yolu [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).

Varsa `/foo` proxy yolu olarak geçirilen uygulama temel yolu aranır `/foo/api/1`, ara yazılım kümeleri `Request.PathBase` için `/foo` ve `Request.Path` için `/api/1` aşağıdaki komutla:

```csharp
app.UsePathBase("/foo");
```

Ara yazılım tersten yeniden çağrıldığında, orijinal yolunu ve yolu tabanı yeniden uygulanır. Ara yazılım sipariş işleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

Proxy yolu kırpar varsa (örneğin, iletme `/foo/api/1` için `/api/1`), düzeltme yönlendirir ve bağlantılar isteğin ayarlayarak [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) özelliği:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Proxy yol verileri ekliyorsanız yeniden yönlendirir ve bağlantıları kullanarak düzeltmek için yolunun parçası at <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> ve atama <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> özelliği:

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a>Farklı üst bilgi adları kullanan proxy için yapılandırma

Proxy adlı üstbilgileri kullanmıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto` proxy adresini/bağlantı noktası ve şema bilgilerini kaynaklanan iletmek için ayarlanmış <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> proxy tarafından kullanılan üstbilgi adlarını eşleştirmek için seçenekleri:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>Bir IPv6 adresi olarak temsil edilen bir IPv4 adresi için yapılandırma

Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1` veya `::ffff:a00:1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).

Aşağıdaki örnekte, üst bilgileri iletilen sağlayan bir ağ adresi için eklenen `KnownNetworks` IPv6 biçiminde listesi.

IPv4 adresi: `10.11.12.1/8`

Dönüştürülen IPv6 adresi: `::ffff:10.11.12.1`  
Dönüştürülen önek uzunluğu: 104

Onaltılık biçimde adresi de sağlayabilirsiniz (`10.11.12.1` IPv6 temsil edilen `::ffff:0a0b:0c01`). Bir IPv4 adresi için IPv6 dönüştürülürken, CIDR ön ek uzunluğu için 96 ekleyin (`8` örnekte) için ek hesap için `::ffff:` IPv6 ön eki (8 + 96 = 104). 

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:10.11.12.1"), 104));
});
```

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a>Linux ve IIS olmayan Düzen ters proxy'ler ileriye doğru

.NET core şablonları çağrı <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> ve <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>. Bu yöntemler bir site için bir Azure Linux App Service, Azure Linux sanal makinesini (VM) veya diğer ters proxy IIS yanı sıra arkasında dağıtılırsa sonsuz döngüye yerleştirin. TLS ters proxy tarafından sonlandırılır ve Kestrel doğru istek düzeni haberdar değildir. OAuth ve OIDC, bu yapılandırmada Ayrıca bunlar yanlış yeniden yönlendirmeleri oluşturması nedeniyle başarısız. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> ekler ve IIS çalıştırırken iletilen üstbilgileri ara yazılım yapılandırır ancak Linux (Apache veya Ngınx tümleştirme) için eşleşen bir otomatik yapılandırma yok.

IIS olmayan senaryolarda Proxy'den gelen düzeni iletmek için ekleme ve iletilen üstbilgileri ara yazılımını yapılandırma. İçinde `Startup.ConfigureServices`, aşağıdaki kodu kullanın:

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

if (string.Equals(
    Environment.GetEnvironmentVariable("ASPNETCORE_FORWARDEDHEADERS_ENABLED"), 
    "true", StringComparison.OrdinalIgnoreCase))
{
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
            ForwardedHeaders.XForwardedProto;
        // Only loopback proxies are allowed by default.
        // Clear that restriction because forwarders are enabled by explicit 
        // configuration.
        options.KnownNetworks.Clear();
        options.KnownProxies.Clear();
    });
}
```

Yeni kapsayıcı görüntülerini Azure'da sağlanana kadar için bir uygulama ayarı (ortam değişkeni) oluşturmalısınız `ASPNETCORE_FORWARDEDHEADERS_ENABLED` kümesine `true`. Daha fazla bilgi için [şablonları çalışmaz Antares Linux'ta düzeni ileticileri eksik olduğundan (aspnet/AspNetCore #4135)](https://github.com/aspnet/AspNetCore/issues/4135).

::: moniker-end

## <a name="troubleshoot"></a>Sorun giderme

Üst bilgiler, beklendiği gibi iletilen olmayan etkinleştirirsiniz [günlüğü](xref:fundamentals/logging/index). Günlükleri sorunu gidermek için yeterli bilgi sağlamazsanız, sunucu tarafından alınan isteği üstbilgileri sıralar. Satır içi ara yazılım istek üstbilgileri, bir uygulama yanıtı yazmak veya üst bilgileri kaydetmek için kullanın. 

Uygulamanın yanıtı üstbilgileri yazmak için çağırdıktan hemen sonra aşağıdaki terminal satır içi ara yazılım yerleştirin <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> içinde `Startup.Configure`:

```csharp
app.Run(async (context) =>
{
    context.Response.ContentType = "text/plain";

    // Request method, scheme, and path
    await context.Response.WriteAsync(
        $"Request Method: {context.Request.Method}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Path: {context.Request.Path}{Environment.NewLine}");

    // Headers
    await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

    foreach (var header in context.Request.Headers)
    {
        await context.Response.WriteAsync($"{header.Key}: " +
            $"{header.Value}{Environment.NewLine}");
    }

    await context.Response.WriteAsync(Environment.NewLine);

    // Connection: RemoteIp
    await context.Response.WriteAsync(
        $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
});
```

Yanıt gövdesi yerine günlükleri yazabilirsiniz. Günlükleri yazmak hata ayıklarken işlev siteye normalde sağlar.

Günlükleri yazmak yerine yanıt gövdesi:

* Ekleme `ILogger<Startup>` içine `Startup` sınıfı açıklandığı [başlangıç günlükleri oluşturma](xref:fundamentals/logging/index#create-logs-in-startup).
* Çağırdıktan hemen sonra aşağıdaki satır içi ara yazılım yerleştirin <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> içinde `Startup.Configure`.

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {METHOD}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {SCHEME}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {PATH}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {KEY}: {VALUE}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {REMOTE_IP_ADDRESS}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

İşlendiğinde `X-Forwarded-{For|Proto|Host}` değerleri taşınmıştır `X-Original-{For|Proto|Host}`. Belirli bir üst bilgisinde birden çok değer varsa, ters sırada sağdan sola iletilen üstbilgileri ara yazılım işlemleri üstbilgileri unutmayın. Varsayılan `ForwardLimit` 1 (bir), bu nedenle olduğu sürece yalnızca başlıklarından en sağdaki değer işlenen değerini `ForwardLimit` artırılır.

İsteğin özgün uzak IP bir girişe eşleşmelidir `KnownProxies` veya `KnownNetworks` iletilen üstbilgileri işlenmeden önce listeler. Bu, güvenilir olmayan proxy'si İleticilerden kabul etmeyerek üst bilgi sızdırma sınırlar. Bilinmeyen bir proxy algılandığında, günlüğe kaydetme proxy adresini belirtir:

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

Önceki örnekte 10.0.0.100 bir proxy sunucusudur. Sunucunun IP adresi için güvenilir bir proxy sunucu ise ekleme `KnownProxies` (veya güvenilen bir ağa ekleyin `KnownNetworks`) içinde `Startup.ConfigureServices`. Daha fazla bilgi için [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options) bölümü.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> Yalnızca güvenilen proxy'leri ve ağları üst bilgiler iletmek izin verilir. Aksi takdirde, [IP yanıltma](https://www.iplocation.net/ip-spoofing) saldırıları mümkündür.

## <a name="certificate-forwarding"></a>Sertifika iletme 

### <a name="on-azure"></a>Azure üzerinde

Bkz: [Azure belgeleri](/azure/app-service/app-service-web-configure-tls-mutual-auth) Azure Web Apps yapılandırmak için. Uygulamanızın `Startup.Configure` yöntemi çağırmadan önce aşağıdaki kodu ekleyin `app.UseAuthentication();`:

```csharp
app.UseCertificateForwarding();
```

Azure kullanan üst bilgi adı belirtmek için sertifika iletme ara yazılımını yapılandırma gerekir. Uygulamanızın `Startup.ConfigureServices` yöntemi, ara yazılım bir sertifika oluşturur üst bilgi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="with-other-web-proxies"></a>Diğer web proxy'leri

IIS veya Azure'nın Web Apps uygulama isteği yönlendirme bulunmayan bir ara sunucu kullanıyorsanız Ara sunucunuz bir HTTP üst bilgisinde alınan sertifika iletecek şekilde yapılandırın. Uygulamanızın `Startup.Configure` yöntemi çağırmadan önce aşağıdaki kodu ekleyin `app.UseAuthentication();`:

```csharp
app.UseCertificateForwarding();
```

Üst bilgi adı belirtmek için sertifika iletme ara yazılımını yapılandırma gerekir. Uygulamanızın `Startup.ConfigureServices` yöntemi, ara yazılım bir sertifika oluşturur üst bilgi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

Son olarak, base64 dışında bir sertifika (Ngınx ile olduğu gibi) kodlama proxy yapıyorsa, ayarlayın `HeaderConverter` seçeneği. Aşağıdaki örnekte göz önünde bulundurun `Startup.ConfigureServices`:

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "YOUR_CUSTOM_HEADER_NAME";
    options.HeaderConverter = (headerValue) => 
    {
        var clientCertificate = 
           /* some conversion logic to create an X509Certificate2 */
        return clientCertificate;
    }
});
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/web-farm>
* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı](https://github.com/aspnet/Announcements/issues/295)
