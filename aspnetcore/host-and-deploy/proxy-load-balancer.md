---
title: Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma
author: guardrex
description: Genellikle önemli istek bilgilerini gizleyen ara sunucu ve yük dengeleyiciler arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/12/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 4f04e6cae120ee88734855252542e2bfc2f194a0
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "67856166"
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

[Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketindeki Iletilen üstbilgiler ara yazılımı, bu üst bilgileri okur ve ilgili alanları üzerinde <xref:Microsoft.AspNetCore.Http.HttpContext>doldurur.

Ara yazılım güncelleştirmeleri:

* `X-Forwarded-For` Üst bilgi değerini kullanarak [HttpContext. Connection. remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; kümesi. Ek ayarlar, ara yazılımın `RemoteIpAddress`nasıl ayarlanacağını etkiler. Ayrıntılar için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options).
* `X-Forwarded-Proto` Üst bilgi değerini kullanan [HttpContext. Request. Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; kümesi.
* `X-Forwarded-Host` Üst bilgi değerini kullanan [HttpContext. Request. Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; kümesi.

İletilen üstbilgiler ara yazılımı [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir. Varsayılan ayarlar şunlardır:

* Uygulama ve isteklerin kaynağı arasında yalnızca *bir ara sunucu* vardır.
* Bilinen proxy 'ler ve bilinen ağlar için yalnızca geri döngü adresleri yapılandırılır.
* İletilen üstbilgiler ve `X-Forwarded-Proto`olarak adlandırılır `X-Forwarded-For` .

Tüm ağ gereçleri ek yapılandırma olmadan `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini eklemez. Proxy istekler uygulamaya ulaştığında bu üst bilgileri içermiyorsa, Gereç üreticinizin kılavuzuna başvurun. Gereç `X-Forwarded-For` , ve `X-Forwarded-Proto`dışındaki farklı üstbilgi adlarını kullanıyorsa, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini gereç tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın. Daha fazla bilgi için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı üstbilgi adları kullanan bir proxy için yapılandırma](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express ve ASP.NET Core modülü

İletilen üstbilgiler ara yazılımı, uygulama IIS 'nin arkasında ve ASP.NET Core modülünden sonra [işlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model) barındırılıyorsa [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) tarafından varsayılan olarak etkinleştirilir. İletilen üstbilgiler ara yazılımı, iletilen üst bilgilerle (örneğin, [IP aldatmacası](https://www.iplocation.net/ip-spoofing)) güven sorunları nedeniyle ASP.NET Core modülüne özgü kısıtlanmış bir yapılandırmaya sahip olan ara yazılım ardışık düzeninde ilk olarak çalışır. Ara yazılım, `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini iletmek üzere yapılandırılmıştır ve tek bir localhost proxy ile kısıtlanır. Ek yapılandırma gerekliyse, [Iletilen üstbilgiler ara yazılım seçeneklerine](#forwarded-headers-middleware-options)bakın.

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Diğer proxy sunucu ve yük dengeleyici senaryoları

[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında [IIS tümleştirmesi](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) 'nin kullanılması dışında, iletilen üstbilgiler ara yazılımı varsayılan olarak etkinleştirilmemiştir. İletilen üstbilgileri ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>işlemek için bir uygulamanın yönlendirilmiş üst bilgi ara yazılımı etkinleştirilmelidir. Ara yazılım için Hayır <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse, ara yazılımı etkinleştirdikten sonra varsayılan [forwardedheadersoptions. forwardedheaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) , forwardedheaders [. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dir.

İle <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> ara yazılımı, içindeki `X-Forwarded-For` `X-Forwarded-Proto` ve`Startup.ConfigureServices`üst bilgilerini iletecek şekilde yapılandırın. Diğer ara yazılım çağrılmadan `Startup.Configure` önce metodunuçağırın:<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>

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
> Hiçbir <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> `Startup.ConfigureServices` veya doğrudan genişletme yöntemine <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>belirtilmemişse, iletmek için varsayılan üstbilgiler [forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır. <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> Özelliğin, iletmek için üst bilgilerle yapılandırılması gerekir.

## <a name="nginx-configuration"></a>NGINX yapılandırması

`X-Forwarded-For` <xref:host-and-deploy/linux-nginx#configure-nginx>Ve üstbilgileriniiletmekiçinbkz.`X-Forwarded-Proto` . Daha fazla bilgi için bkz [. NGINX: Iletilen üstbilgi](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)kullanılıyor.

## <a name="apache-configuration"></a>Apache yapılandırması

`X-Forwarded-For`otomatik olarak eklenir (bkz [. Apache Module mod_proxy: Ters proxy Isteği üstbilgileri](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). `X-Forwarded-Proto` Üstbilgiyi iletme hakkında daha fazla bilgi için bkz <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>İletilen üstbilgiler ara yazılım seçenekleri

<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>Iletilen üstbilgiler ara yazılımı davranışını denetleyin. Aşağıdaki örnek varsayılan değerleri değiştirir:

* İletilen üst `2`bilgilerdeki giriş sayısını sınırlayın.
* Bilinen bir proxy adresi `127.0.10.1`ekleyin.
* İletilen üst bilgi adını varsayılan `X-Forwarded-For` değerinden olarak `X-Forwarded-For-My-Custom-Header-Name`değiştirin.

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
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | Ana bilgisayarları `X-Forwarded-Host` üst bilgiyle belirtilen değerlerle sınırlandırır.<ul><li>Değerler, Ordinal-Ignore-Case kullanılarak karşılaştırılır.</li><li>Bağlantı noktası numaraları dışlanmalıdır.</li><li>Liste boşsa, tüm konaklara izin verilir.</li><li>Üst düzey joker karakter `*` boş olmayan tüm konaklara izin verir.</li><li>Alt etki alanı Joker karakterlere izin verilir ancak kök etki alanıyla eşleşmez. Örneğin, `*.contoso.com` kök etki alanını değil `foo.contoso.com` , alt etki alanıyla `contoso.com`eşleşir.</li><li>Unicode ana bilgisayar adlarına izin verilir, ancak eşleştirme için [puni koduna](https://tools.ietf.org/html/rfc3492) dönüştürülür.</li><li>[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) sınırlayıcı ayraçları içermeli ve [geleneksel biçimde](https://tools.ietf.org/html/rfc4291#section-2.2) olmalıdır (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6 adresleri, farklı biçimler arasında mantıksal eşitlik denetimi için özel değildir ve hiçbir kurallı kullanım yapılmaz.</li><li>İzin verilen konaklar kısıtlanamaması, bir saldırganın hizmet tarafından oluşturulan bağlantıları sızdırmasına izin verebilir.</li></ul>Varsayılan değer boş `IList<string>`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName)tarafından belirtilen bir yerine kullanın. Bu seçenek, proxy/iletici `X-Forwarded-For` üstbilgiyi kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Hangi ileticilerin işleneceğini tanımlar. Uygulanan alanların listesi için [Forwardedheaders numaralandırması](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) ' ne bakın. Bu özelliğe atanan tipik değerler şunlardır `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.<br><br>Varsayılan değer [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | [Forwardedheadersdefaults varsayılan. XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın. Bu seçenek, proxy/iletici `X-Forwarded-Host` üstbilgiyi kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName)tarafından belirtilen bir yerine kullanın. Bu seçenek, proxy/iletici `X-Forwarded-Proto` üstbilgiyi kullanmıyorsa ve bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | İşlenen üst bilgilerdeki giriş sayısını sınırlandırır. Sınırı devre `null` dışı bırakmak için olarak ayarlayın, ancak bu yalnızca `KnownProxies` veya `KnownNetworks` yapılandırılmışsa yapılmalıdır. `null` Değer olmayan bir önlem, hatalı yapılandırılmış proxy 'lerde ve ağdaki yan kanallardan gelen kötü amaçlı isteklere karşı koruma sağlamak için bir önlem (garanti değil) olarak ayarlanır.<br><br>İletilen üstbilgiler ara yazılımı, üst bilgileri sağdan sola doğru sırada işler. Varsayılan değer (`1`) kullanılırsa, `ForwardLimit` değeri artmadığı müddetçe yalnızca üst bilgilerden en sağdaki değer işlenir.<br><br>Varsayılan, `1` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | İletilen üstbilgileri kabul etmek için bilinen ağların adres aralıkları. Sınıfsız Etki alanları arası yönlendirme (CıDR) gösterimini kullanarak IP aralıkları sağlayın.<br><br>Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 olarak `::ffff:10.0.0.1`temsil edilen IPv4 içinde). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın. Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.<br><br>Varsayılan, için `IList` <xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork> \< tek`IPAddress.Loopback`bir giriş içeren bir >. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | İletilen üstbilgileri kabul etmek için bilinen proxy 'lerin adresleri. Tam `KnownProxies` IP adresi eşleşmelerini belirtmek için kullanın.<br><br>Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 olarak `::ffff:10.0.0.1`temsil edilen IPv4 içinde). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın. Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.<br><br>Varsayılan, için `IList` <xref:System.Net.IPAddress> \< tek`IPAddress.IPv6Loopback`bir giriş içeren bir >. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName)tarafından belirtilen bir yerine kullanın.<br><br>Varsayılan, `X-Original-For` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | [Forwardedheadersdefaults varsayılan. XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın.<br><br>Varsayılan, `X-Original-Host` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName)tarafından belirtilen bir yerine kullanın.<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Bilgi işlem sayısının, işlenmekte olan [Forwardedheadersoptions. ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) arasında eşitlenmiş olmasını gerektir.<br><br>ASP.NET Core 1. x `true`içindeki varsayılan değer. ASP.NET Core 2,0 veya sonraki sürümlerde `false`varsayılan değer. |

## <a name="scenarios-and-use-cases"></a>Senaryolar ve kullanım örnekleri

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>İletilen üstbilgiler eklemek mümkün olmadığında ve tüm istekler güvenlidir

Bazı durumlarda, iletilen üstbilgileri uygulamaya yönelik isteklere eklemek mümkün olmayabilir. Proxy tüm genel dış isteklerin https olduğunu zortacaktır, düzen herhangi bir tür ara yazılım kullanılmadan önce içinde `Startup.Configure` el ile ayarlanabilir:

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

`Request.PathBase` `/foo` `Request.Path` `/api/1` , Olarak `/foo/api/1`geçirilen bir ara sunucu yolu için uygulama temel yolu ise, ara yazılım aşağıdaki komutla ve olarak ayarlanır: `/foo`

```csharp
app.UsePathBase("/foo");
```

Özgün yol ve yol tabanı, ara yazılım ters ' de çağrıldığında yeniden uygulanır. Ara yazılım siparişi işleme hakkında daha fazla bilgi için <xref:fundamentals/middleware/index>bkz.

Proxy, yolu kırpar (örneğin, ' ye `/foo/api/1` `/api/1`iletme), isteğin [pathbase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) özelliğini ayarlayarak yeniden yönlendirmeleri ve bağlantıları düzeltir:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Proxy, yol verileri ekliyor ise, kullanarak yeniden yönlendirmeleri ve bağlantıları onarmak üzere yolun bir bölümünü atın ve <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> özelliğini kullanarak yeniden yönlendirmeyi ve bağlantıları onarın:

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

Proxy, adlı `X-Forwarded-For` üst bilgileri kullanmıyorsa ve `X-Forwarded-Proto` proxy adresini/bağlantı noktasını ve kaynak <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> düzen bilgilerini iletmek için, ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini proxy tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>IPv6 adresi olarak temsil edilen IPv4 adresinin yapılandırması

Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 'da veya `::ffff:a00:1`olarak `::ffff:10.0.0.1` temsil edilen IPv4). Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). [HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın.

Aşağıdaki örnekte, iletilen üstbilgiler sağlayan bir ağ adresi `KnownNetworks` listeye IPv6 biçiminde eklenir.

IPv4 adresi:`10.11.12.1/8`

Dönüştürülen IPv6 adresi:`::ffff:10.11.12.1`  
Dönüştürülen önek uzunluğu: 104

Adresi onaltılık biçimde (`10.11.12.1` IPv6 olarak `::ffff:0a0b:0c01`temsil edilir) de sağlayabilirsiniz. Bir IPv4 adresini IPv6 'ya dönüştürürken, ek`8` `::ffff:` IPv6 öneki (8 + 96 = 104) için (örnekteki) CIDR önek uzunluğuna 96 ekleyin. 

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

Bir Azure Linux <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> App Service <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> , Azure Linux sanal makinesi (VM) veya IIS 'nin yanı sıra diğer tüm ters proxy 'ler için dağıtıldığında bir siteyi çağıran ve sonsuz döngüye alan uygulamalar. TLS, ters proxy tarafından sonlandırılır ve Kestrel doğru istek düzeninden haberdar değildir. OAuth ve OıDC yanlış yeniden yönlendirmeler oluşturduğundan bu yapılandırmada de başarısız olur. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>IIS 'nin arkasında çalışırken Iletilen üstbilgiler ara yazılımı ekler ve yapılandırır, ancak Linux için eşleşen otomatik yapılandırma (Apache veya NGINX tümleştirmesi) yoktur.

IIS olmayan senaryolarda düzeni proxy 'den iletmek için, Iletilen üstbilgiler ara yazılımını ekleyin ve yapılandırın. İçinde `Startup.ConfigureServices`, aşağıdaki kodu kullanın:

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

## <a name="troubleshoot"></a>Sorun giderme

Üstbilgiler beklendiği gibi iletilemediği zaman [günlüğe kaydetmeyi](xref:fundamentals/logging/index)etkinleştirin. Günlükler sorunu gidermek için yeterli bilgi sağlamıyorsa, sunucu tarafından alınan istek üstbilgilerini numaralandırın. Bir uygulama yanıtına istek üst bilgileri yazmak veya üst bilgileri günlüğe kaydetmek için satır içi ara yazılım kullanın. 

Üst bilgileri uygulamanın yanıtına yazmak için, aşağıdaki Terminal satır içi ara yazılımını, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> içindeki `Startup.Configure`çağrısından hemen sonra yerleştirin:

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

* [Başlangıçta günlükleri oluşturma](xref:fundamentals/logging/index#create-logs-in-startup)bölümünde açıklandığı gibi `Startup` sınıfına ekleyin.`ILogger<Startup>`
* <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> İçindeki`Startup.Configure`çağrısından hemen sonra aşağıdaki satır içi ara yazılımı yerleştirin.

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

İşlendiğinde, `X-Forwarded-{For|Proto|Host}` değerler öğesine `X-Original-{For|Proto|Host}`taşınır. Belirli bir üst bilgide birden çok değer varsa, Iletilen üstbilgiler ara yazılımı sağdan sola doğru sırada üst bilgileri işler. Varsayılan `ForwardLimit` değer ( `1` bir) ise, değeri arttırılmadığı müddetçe yalnızca en `ForwardLimit` sağdaki üst bilgilerden en sağdaki değer işlenir.

İletilen üstbilgiler işlenmeden önce isteğin özgün uzak IP 'si `KnownProxies` veya `KnownNetworks` listelerindeki bir girdiyle eşleşmesi gerekir. Bu, güvenilmeyen proxy 'lerden ileticileri kabul etmediği için üst bilgi yanıltmasını kısıtlar. Bilinmeyen bir proxy algılandığında günlüğe kaydetme, proxy 'nin adresini gösterir:

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

Yukarıdaki örnekte, alana 10.0.0.100 bir proxy sunucusudur. Sunucu, güvenilir bir ara sunucu ise, sunucusunun IP adresini ' `KnownProxies` `Startup.ConfigureServices`e (veya güvenilen bir ağı öğesine `KnownNetworks`ekleyin) ekleyin. Daha fazla bilgi için [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) bölümüne bakın.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> Yalnızca güvenilen proxy 'lerin ve ağların üstbilgileri iletmesine izin verir. Aksi takdirde, [IP sahtekarlığı](https://www.iplocation.net/ip-spoofing) saldırıları mümkündür.

## <a name="certificate-forwarding"></a>Sertifika iletme 

### <a name="on-azure"></a>Azure 'da

Azure Web Apps 'yi yapılandırmak için [Azure belgelerine](/azure/app-service/app-service-web-configure-tls-mutual-auth) bakın. Uygulamanızın `Startup.Configure` yönteminde, `app.UseAuthentication();`çağrısının önüne aşağıdaki kodu ekleyin:

```csharp
app.UseCertificateForwarding();
```

Ayrıca, Azure 'un kullandığı üst bilgi adını belirtmek için sertifika Iletme ara yazılımını yapılandırmanız gerekecektir. Uygulamanızın `Startup.ConfigureServices` yönteminde, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="with-other-web-proxies"></a>Diğer Web proxy 'leriyle

IIS veya Azure 'un uygulama Isteği yönlendirme Web Apps olmayan bir ara sunucu kullanıyorsanız, proxy 'nizi bir HTTP üst bilgisinde aldığı sertifikayı iletecek şekilde yapılandırın. Uygulamanızın `Startup.Configure` yönteminde, `app.UseAuthentication();`çağrısının önüne aşağıdaki kodu ekleyin:

```csharp
app.UseCertificateForwarding();
```

Ayrıca, üst bilgi adını belirtmek için sertifika Iletme ara yazılımını yapılandırmanız gerekecektir. Uygulamanızın `Startup.ConfigureServices` yönteminde, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

Son olarak, proxy (NGINX ile olduğu gibi) Base64 kodlaması dışında bir şeyi alıyorsa, `HeaderConverter` seçeneğini ayarlayın. İçinde `Startup.ConfigureServices`aşağıdaki örneği göz önünde bulundurun:

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
* [Microsoft Güvenlik Danışma belgesi CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı](https://github.com/aspnet/Announcements/issues/295)
