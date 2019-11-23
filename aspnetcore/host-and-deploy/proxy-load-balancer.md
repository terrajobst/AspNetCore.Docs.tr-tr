---
title: Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma
author: guardrex
description: Genellikle önemli istek bilgilerini gizleyen ara sunucu ve yük dengeleyiciler arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 5eb69c2a253d1b8c42edd39b64b595898e6fb948
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007283"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma

[Luke Latham](https://github.com/guardrex) ve [Chris](https://github.com/Tratcher) 'e göre

ASP.NET Core için Önerilen yapılandırmada, uygulama IIS/ASP. NET Core modülü, NGINX veya Apache kullanılarak barındırılır. Proxy sunucuları, yük dengeleyiciler ve diğer ağ gereçleri, genellikle istek hakkında uygulamaya ulaşmadan önce bu bilgileri gizlemediğinde:

* HTTPS istekleri HTTP üzerinden proxy olduğunda, özgün düzen (HTTPS) kaybolur ve bir üst bilgide iletilmesi gerekir.
* Bir uygulama, Internet veya şirket ağında gerçek kaynağını değil, ara sunucudan bir istek aldığından, kaynak istemci IP adresinin de bir üst bilgide iletilmesi gerekir.

Bu bilgiler, istek işlemede, örneğin yeniden yönlendirmeler, kimlik doğrulama, bağlantı oluşturma, ilke değerlendirmesi ve istemci coğrafi konum gibi önemli olabilir.

## <a name="forwarded-headers"></a>İletilen üstbilgiler

Kurala göre, proxy 'leri HTTP başlıklarında iletme bilgileri.

| Üstbilgi | Açıklama |
| ------ | ----------- |
| X-Iletilmiş-Için | , İsteği başlatan istemciyle ilgili bilgileri ve bir proxy zincirinde sonraki proxy 'leri barındırır. Bu parametre IP adresleri (ve isteğe bağlı olarak bağlantı noktası numaraları) içerebilir. Bir ara sunucu zincirinde, ilk parametre isteğin ilk yaptığı istemciyi gösterir. Sonraki proxy tanımlayıcıları izler. Zincirdeki son proxy, Parametreler listesinde değil. Son proxy 'nin IP adresi ve isteğe bağlı olarak bir bağlantı noktası numarası, aktarım katmanında uzak IP adresi olarak kullanılabilir. |
| X-Iletilen-proto | Kaynak düzenin değeri (HTTP/HTTPS). Bu değer, istek birden çok proxy 'ye geçen bir düzen listesi de olabilir. |
| X-Iletilen-konak | Ana bilgisayar üst bilgisi alanının özgün değeri. Genellikle, proxy 'ler ana bilgisayar üst bilgisini değiştirmez. Proxy 'nin konak üstbilgilerini doğrulamadığı veya kısıtlayamayacağı sistemleri etkileyen ayrıcalık yükselmesi güvenlik açığı hakkında bilgi için bkz. [Microsoft Güvenlik Danışmanlığı CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) . |

[Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketindeki Iletilen üstbilgiler ara yazılımı, bu üst bilgileri okur ve <xref:Microsoft.AspNetCore.Http.HttpContext>ilişkili alanları doldurur.

Ara yazılım güncelleştirmeleri:

* [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; `X-Forwarded-For` üst bilgi değeri kullanılarak ayarlanır. Ek ayarlar, ara yazılımın `RemoteIpAddress`nasıl ayarlakullandığını etkiler. Ayrıntılar için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options).
* `X-Forwarded-Proto` üst bilgi değeri kullanılarak ayarlanan [HttpContext. Request. Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash;.
* &ndash; `X-Forwarded-Host` üst bilgi değeri kullanılarak ayarlanan [HttpContext. Request. Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) .

İletilen üstbilgiler ara yazılımı [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir. Varsayılan ayarlar şunlardır:

* Uygulama ve isteklerin kaynağı arasında yalnızca *bir ara sunucu* vardır.
* Bilinen proxy 'ler ve bilinen ağlar için yalnızca geri döngü adresleri yapılandırılır.
* İletilen üst bilgiler `X-Forwarded-For` ve `X-Forwarded-Proto`olarak adlandırılır.

Tüm ağ gereçleri, ek yapılandırma olmadan `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini eklemez. Proxy istekler uygulamaya ulaştığında bu üst bilgileri içermiyorsa, Gereç üreticinizin kılavuzuna başvurun. Gereç `X-Forwarded-For` ve `X-Forwarded-Proto`farklı üstbilgi adları kullanıyorsa, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini gereç tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın. Daha fazla bilgi için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı üstbilgi adları kullanan bir proxy için yapılandırma](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express ve ASP.NET Core modülü

İletilen üstbilgiler ara yazılımı, uygulama IIS 'nin arkasında ve ASP.NET Core modülünden sonra [işlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model) barındırılıyorsa [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) tarafından varsayılan olarak etkinleştirilir. İletilen üstbilgiler ara yazılımı, iletilen üst bilgilerle (örneğin, [IP aldatmacası](https://www.iplocation.net/ip-spoofing)) güven sorunları nedeniyle ASP.NET Core modülüne özgü kısıtlanmış bir yapılandırmaya sahip olan ara yazılım ardışık düzeninde ilk olarak çalışır. Ara yazılım, `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini iletmek üzere yapılandırılır ve tek bir localhost proxy ile kısıtlanır. Ek yapılandırma gerekliyse, [Iletilen üstbilgiler ara yazılım seçeneklerine](#forwarded-headers-middleware-options)bakın.

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Diğer proxy sunucu ve yük dengeleyici senaryoları

[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında [IIS tümleştirmesi](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) 'nin kullanılması dışında, iletilen üstbilgiler ara yazılımı varsayılan olarak etkinleştirilmemiştir. Bir uygulamanın iletilen üstbilgileri <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>işlemesi için iletilen üstbilgiler ara yazılımı etkinleştirilmelidir. Ara yazılım için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse, ara yazılımı etkinleştirdikten sonra varsayılan [Forwardedheadersoptions. ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) , [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dir.

`X-Forwarded-For` ve `X-Forwarded-Proto` üst `Startup.ConfigureServices`bilgilerini iletmek için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> ile ara yazılımı yapılandırın. Diğer ara yazılım çağrılmadan önce `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yöntemi çağırın:

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
> `Startup.ConfigureServices` içinde <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse veya doğrudan uzantı yöntemine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, iletmek için varsayılan üstbilgiler [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır. <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> özelliği, iletmek için üst bilgilerle yapılandırılmalıdır.

## <a name="nginx-configuration"></a>NGINX yapılandırması

`X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini iletmek için bkz. <xref:host-and-deploy/linux-nginx#configure-nginx>. Daha fazla bilgi için bkz. [NGINX: iletilen üstbilgiyi kullanma](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Apache yapılandırması

`X-Forwarded-For` otomatik olarak eklenir (bkz. [Apache modülü mod_proxy: ters proxy Isteği üstbilgileri](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). `X-Forwarded-Proto` üst bilgisini iletme hakkında daha fazla bilgi için bkz. <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>İletilen üstbilgiler ara yazılım seçenekleri

Iletilen üstbilgiler ara yazılımı davranışını kontrol <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>. Aşağıdaki örnek varsayılan değerleri değiştirir:

* İletilen üst bilgilerdeki giriş sayısını `2`olarak sınırlandırın.
* `127.0.10.1`bilinen bir proxy adresi ekleyin.
* İletilen üst bilgi adını varsayılan `X-Forwarded-For` `X-Forwarded-For-My-Custom-Header-Name`olacak şekilde değiştirin.

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
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | Ana bilgisayarları `X-Forwarded-Host` üst bilgisini belirtilen değerlerle kısıtlar.<ul><li>Değerler, Ordinal-Ignore-Case kullanılarak karşılaştırılır.</li><li>Bağlantı noktası numaraları dışlanmalıdır.</li><li>Liste boşsa, tüm konaklara izin verilir.</li><li>Üst düzey joker karakter `*` boş olmayan tüm konaklara izin verir.</li><li>Alt etki alanı Joker karakterlere izin verilir ancak kök etki alanıyla eşleşmez. Örneğin `*.contoso.com`, kök etki alanı `contoso.com`değil, alt etki alanıyla eşleşir `foo.contoso.com`.</li><li>Unicode ana bilgisayar adlarına izin verilir, ancak eşleştirme için [puni koduna](https://tools.ietf.org/html/rfc3492) dönüştürülür.</li><li>[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) sınırlayıcı ayraçları içermeli ve [geleneksel biçimde](https://tools.ietf.org/html/rfc4291#section-2.2) olmalıdır (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6 adresleri, farklı biçimler arasında mantıksal eşitlik denetimi için özel değildir ve hiçbir kurallı kullanım yapılmaz.</li><li>İzin verilen konaklar kısıtlanamaması, bir saldırganın hizmet tarafından oluşturulan bağlantıları sızdırmasına izin verebilir.</li></ul>Varsayılan değer boş bir `IList<string>`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName)tarafından belirtilen bir yerine kullanın. Bu seçenek, proxy/iletici `X-Forwarded-For` üstbilgisini kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Hangi ileticilerin işleneceğini tanımlar. Uygulanan alanların listesi için [Forwardedheaders numaralandırması](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) ' ne bakın. Bu özelliğe atanan tipik değerler `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.<br><br>Varsayılan değer [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | [Forwardedheadersdefaults varsayılan. XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın. Bu seçenek, proxy/iletici `X-Forwarded-Host` üstbilgisini kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName)tarafından belirtilen bir yerine kullanın. Bu seçenek, proxy/iletici `X-Forwarded-Proto` üstbilgisini kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | İşlenen üst bilgilerdeki giriş sayısını sınırlandırır. Limiti devre dışı bırakmak için `null` olarak ayarlayın, ancak bu yalnızca `KnownProxies` veya `KnownNetworks` yapılandırılırsa yapılmalıdır. `null` olmayan bir değer ayarlamak, yanlış yapılandırılmış proxy 'lerde ve ağdaki yan kanallardan gelen kötü amaçlı isteklere karşı koruma sağlamak için bir önlem (garanti değildir) olarak ayarlanır.<br><br>İletilen üstbilgiler ara yazılımı, üst bilgileri sağdan sola doğru sırada işler. Varsayılan değer (`1`) kullanılırsa, `ForwardLimit` değeri artmadığı müddetçe yalnızca üst bilgilerden en sağdaki değer işlenir.<br><br>Varsayılan, `1` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | İletilen üstbilgileri kabul etmek için bilinen ağların adres aralıkları. Sınıfsız Etki alanları arası yönlendirme (CıDR) gösterimini kullanarak IP aralıkları sağlayın.<br><br>Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 içinde, `::ffff:10.0.0.1`olarak temsil edilen `10.0.0.1`). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın. Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.<br><br>Varsayılan değer, `IPAddress.Loopback`için tek bir giriş içeren bir `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>>. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | İletilen üstbilgileri kabul etmek için bilinen proxy 'lerin adresleri. Tam IP adresi eşleşmelerini belirtmek için `KnownProxies` kullanın.<br><br>Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 içinde, `::ffff:10.0.0.1`olarak temsil edilen `10.0.0.1`). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın. Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.<br><br>Varsayılan değer, `IPAddress.IPv6Loopback`için tek bir giriş içeren bir `IList`\<<xref:System.Net.IPAddress>>. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName)tarafından belirtilen bir yerine kullanın.<br><br>Varsayılan, `X-Original-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | [Forwardedheadersdefaults varsayılan. XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın.<br><br>Varsayılan, `X-Original-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName)tarafından belirtilen bir yerine kullanın.<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Bilgi işlem sayısının, işlenmekte olan [Forwardedheadersoptions. ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) arasında eşitlenmiş olmasını gerektir.<br><br>ASP.NET Core 1. x içindeki varsayılan değer `true`. ASP.NET Core 2,0 veya sonraki sürümlerde varsayılan değer `false`. |

## <a name="scenarios-and-use-cases"></a>Senaryolar ve kullanım örnekleri

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>İletilen üstbilgiler eklemek mümkün olmadığında ve tüm istekler güvenlidir

Bazı durumlarda, iletilen üstbilgileri uygulamaya yönelik isteklere eklemek mümkün olmayabilir. Proxy tüm genel dış isteklerin HTTPS olduğunu zortacaktır, düzen herhangi bir tür ara yazılım kullanılmadan önce `Startup.Configure` el ile ayarlanabilir:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Bu kod bir geliştirme veya hazırlama ortamındaki ortam değişkeniyle veya diğer yapılandırma ayarıyla devre dışı bırakılabilir.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Yol tabanı ve istek yolunu değiştiren proxy 'ler ile uğraşın

Bazı proxy 'ler yolu bozulmadan, ancak yönlendirmenin düzgün çalışması için kaldırılması gereken bir uygulama temel yolu ile geçer. [Usepathbaseextensions. usepathbase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) ara yazılımı, yolu HttpRequest. [Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) ve uygulama temeli yoluna, [HttpRequest. pathbase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)içine böler.

`/foo`, `/foo/api/1`olarak geçirilen bir ara sunucu yolu için uygulama temel yolu ise, ara yazılım `Request.PathBase` `/foo` ve `Request.Path` aşağıdaki komutla `/api/1` olarak ayarlar:

```csharp
app.UsePathBase("/foo");
```

Özgün yol ve yol tabanı, ara yazılım ters ' de çağrıldığında yeniden uygulanır. Ara yazılım sırası işleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

Proxy, yolu kırpar (örneğin, `/api/1``/foo/api/1` iletme), isteğin [Pathbase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) özelliğini ayarlayarak yeniden yönlendirmeleri ve bağlantıları düzeltir:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Proxy yol verileri ekliyor ise, <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> kullanarak yeniden yönlendirmeleri ve bağlantıları onarmak üzere yolun bir parçasını atın ve <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> özelliğine atama yapın:

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

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a>Farklı üstbilgi adları kullanan bir ara sunucu için yapılandırma

Proxy, `X-Forwarded-For` adlı üstbilgileri kullanmıyorsa ve proxy adresini/bağlantı noktasını ve kaynak düzen bilgilerini iletmek için `X-Forwarded-Proto`, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini proxy tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>IPv6 adresi olarak temsil edilen IPv4 adresinin yapılandırması

Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 'de `::ffff:10.0.0.1` veya `::ffff:a00:1`olarak temsil edilen `10.0.0.1`). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın.

Aşağıdaki örnekte, iletilen üstbilgileri sağlayan bir ağ adresi `KnownNetworks` listesine IPv6 biçiminde eklenir.

IPv4 adresi: `10.11.12.1/8`

Dönüştürülen IPv6 adresi: `::ffff:10.11.12.1`  
Dönüştürülen önek uzunluğu: 104

Ayrıca, adresi onaltılık biçimde (`10.11.12.1` `::ffff:0a0b:0c01`IPv6 olarak temsil edilir) da sağlayabilirsiniz. Bir IPv4 adresini IPv6 'ya dönüştürürken, ek `::ffff:` IPv6 öneki (8 + 96 = 104) için hesaba (örnekteki`8`) CıDR ön eki uzunluğuna 96 ekleyin. 

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

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a>Linux ve IIS olmayan ters proxy 'ler için Düzen iletme

<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> ve <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> çağıran ve bir Azure Linux App Service, Azure Linux sanal makinesi (VM) veya IIS 'in yanı sıra diğer tüm ters proxy 'ler için dağıtılan bir siteyi sonsuz döngüye koymak için kullanılan uygulamalar. TLS, ters proxy tarafından sonlandırılır ve Kestrel doğru istek düzeninden haberdar değildir. OAuth ve OıDC yanlış yeniden yönlendirmeler oluşturduğundan bu yapılandırmada de başarısız olur. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>, IIS 'nin arkasında çalışırken Iletilen üstbilgiler ara yazılımını ekler ve yapılandırır, ancak Linux için eşleşen otomatik yapılandırma (Apache veya NGINX tümleştirmesi) yoktur.

IIS olmayan senaryolarda düzeni proxy 'den iletmek için, Iletilen üstbilgiler ara yazılımını ekleyin ve yapılandırın. `Startup.ConfigureServices`' de aşağıdaki kodu kullanın:

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

::: moniker range=">= aspnetcore-3.0"

## <a name="certificate-forwarding"></a>Sertifika iletme 

### <a name="azure"></a>Azure

Sertifika iletme için Azure App Service yapılandırmak için, bkz. [Azure App Service IÇIN TLS karşılıklı kimlik doğrulamasını yapılandırma](/azure/app-service/app-service-web-configure-tls-mutual-auth). Aşağıdaki kılavuz ASP.NET Core uygulamasını yapılandırmaya aittir.

`Startup.Configure`, `app.UseAuthentication();`çağrısından önce aşağıdaki kodu ekleyin:

```csharp
app.UseCertificateForwarding();
```


Sertifika Iletme ara yazılımını, Azure 'un kullandığı üst bilgi adını belirtecek şekilde yapılandırın. `Startup.ConfigureServices`' de, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="other-web-proxies"></a>Diğer Web proxy 'leri

IIS veya Azure App Service uygulama Isteği yönlendirme (ARR) olmayan bir ara sunucu kullanılıyorsa, proxy 'yi bir HTTP üst bilgisinde aldığı sertifikayı iletecek şekilde yapılandırın. `Startup.Configure`, `app.UseAuthentication();`çağrısından önce aşağıdaki kodu ekleyin:

```csharp
app.UseCertificateForwarding();
```

Üst bilgi adını belirtmek için sertifika Iletme ara yazılımını yapılandırın. `Startup.ConfigureServices`' de, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

Ara sunucu Base64 ile kodlanmazsa (NGINX ile olduğu gibi), `HeaderConverter` seçeneğini ayarlayın. `Startup.ConfigureServices`içinde aşağıdaki örneği göz önünde bulundurun:

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

::: moniker-end

## <a name="troubleshoot"></a>Sorun giderme

Üstbilgiler beklendiği gibi iletilemediği zaman [günlüğe kaydetmeyi](xref:fundamentals/logging/index)etkinleştirin. Günlükler sorunu gidermek için yeterli bilgi sağlamıyorsa, sunucu tarafından alınan istek üstbilgilerini numaralandırın. Bir uygulama yanıtına istek üst bilgileri yazmak veya üst bilgileri günlüğe kaydetmek için satır içi ara yazılım kullanın. 

Üst bilgileri uygulamanın yanıtına yazmak için aşağıdaki Terminal satır içi ara yazılımları `Startup.Configure`<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> çağrısından hemen sonra yerleştirin:

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

Yanıt gövdesi yerine günlüklere yazabilirsiniz. Günlüklere yazmak, sitenin hata ayıklanırken normal şekilde çalışmasına olanak tanır.

Yanıt gövdesi yerine günlükleri yazmak için:

* `ILogger<Startup>` [Başlangıçta günlükleri oluşturma](xref:fundamentals/logging/index#create-logs-in-startup)bölümünde açıklandığı gibi `Startup` sınıfına ekleyin.
* `Startup.Configure`<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> çağrısından hemen sonra aşağıdaki satır içi ara yazılımı yerleştirin.

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {Method}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {Scheme}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {Path}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {Key}: {Value}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {RemoteIpAddress}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

İşlendiğinde `X-Forwarded-{For|Proto|Host}` değerler `X-Original-{For|Proto|Host}`taşınır. Belirli bir üst bilgide birden çok değer varsa, Iletilen üstbilgiler ara yazılımı sağdan sola doğru sırada üst bilgileri işler. Varsayılan `ForwardLimit` `1` (bir), bu nedenle `ForwardLimit` değeri artmadığı müddetçe yalnızca üst bilgilerden en sağdaki değer işlenir.

İletilen üstbilgiler işlenmeden önce isteğin özgün uzak IP 'si `KnownProxies` veya `KnownNetworks` listelerindeki bir girdiyle eşleşmelidir. Bu, güvenilmeyen proxy 'lerden ileticileri kabul etmediği için üst bilgi yanıltmasını kısıtlar. Bilinmeyen bir proxy algılandığında günlüğe kaydetme, proxy 'nin adresini gösterir:

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

Yukarıdaki örnekte, alana 10.0.0.100 bir proxy sunucusudur. Sunucu güvenilir bir ara sunucu ise, `Startup.ConfigureServices`içindeki sunucunun IP adresini `KnownProxies` ekleyin (veya `KnownNetworks`bir güvenilen ağ ekleyin). Daha fazla bilgi için [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) bölümüne bakın.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> Yalnızca güvenilen proxy 'lerin ve ağların üstbilgileri iletmesine izin verir. Aksi takdirde, [IP sahtekarlığı](https://www.iplocation.net/ip-spoofing) saldırıları mümkündür.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/web-farm>
* [Microsoft güvenlik danışmanlık CVE-2018-0787: ayrıcalık yükselmesi güvenlik açığı ASP.NET Core](https://github.com/aspnet/Announcements/issues/295)
