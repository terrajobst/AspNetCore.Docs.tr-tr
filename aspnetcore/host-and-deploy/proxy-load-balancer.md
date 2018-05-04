---
title: Proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırın
author: guardrex
description: Proxy sunucuları ve genellikle önemli isteği bilgileri soyutlamaması yük dengeleyici arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: f18a5c518edc739e0fe667f3aef6ffd38c06366c
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırın

Tarafından [Luke Latham](https://github.com/guardrex) ve [Chris fillerin](https://github.com/Tratcher)

ASP.NET Core için önerilen yapılandırma, IIS/ASP.NET çekirdek modülü, Nginx ya da Apache kullanarak uygulama barındırılır. Genellikle uygulama erişmeden önce proxy sunucular, yük dengeleyicileri ve başka ağ gereçlerine isteğiyle ilgili bilgileri soyutlamaması:

* HTTPS istekleri HTTP üzerinden yönlendirilirken, özgün düzenini (HTTPS) kaybolur ve bir üstbilgisinde gönderilmelidir.
* Uygulama proxy'si ve doğru kaynağına değil Internet veya kurumsal bir ağda bir istek aldığından, kaynak istemci IP adresini de başlığı gönderilmelidir.

Bu bilgiler, örneğin yeniden yönlendirmeleri, kimlik doğrulama, bağlantı oluşturma, ilke değerlendirmesi ve istemci geoloation istek işleme önemli olabilir.

## <a name="forwarded-headers"></a>İletilen üstbilgileri

Kurala göre proxy'leri HTTP üst bilgilerinde bilgileri iletin.

| Üstbilgi | Açıklama |
| ------ | ----------- |
| X-iletilen-için | İstek ve proxy'ler zinciri sonraki proxy'leri başlatılan istemci ile ilgili bilgileri tutar. Bu parametre, IP adresleri (ve isteğe bağlı olarak, bağlantı noktası numaralarını) içerebilir. Bir proxy sunucu zincirinde ilk parametre istek ilk yapıldığı istemci gösterir. Sonraki proxy tanımlayıcıları izleyin. Zincirdeki son proxy parametreleri listesinde değil. Son proxy'nin IP adresi ve isteğe bağlı olarak bir bağlantı noktası numarası aktarım katmanında uzak IP adresi olarak kullanılabilir. |
| X iletilen Proto | Kaynak şema (HTTP/HTTPS) değeri. İstek birden çok proxy'leri geçiş değilse değer düzenleri listesini de olabilir. |
| X iletilen konak | Ana bilgisayar üstbilgisi alanı özgün değeri. Genellikle, proxy'leri ana bilgisayar üstbilgisi değiştirmeyin. Bkz: [Microsoft güvenlik önerisi CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) burada değil proxy doğrulama sistemleri etkiler ayrıcalık yükseltme güvenlik açığı veya bilinen iyi değerlere restict konak üstbilgileri hakkında bilgi için. |

İletilen üstbilgileri Ara gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketini, bu üstbilgileri okur ve ilişkili alanlarında doldurur [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Ara yazılım güncelleştirmeleri:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; kullanılarak ayarlanan `X-Forwarded-For` üstbilgi değeri. Ek ayarları etkileyen nasıl ara yazılım ayarlar `RemoteIpAddress`. Ayrıntılar için bkz [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; kullanılarak ayarlanan `X-Forwarded-Proto` üstbilgi değeri.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; kullanılarak ayarlanan `X-Forwarded-Host` üstbilgi değeri.

Tüm ağ cihazları ekleme Not `X-Forwarded-For` ve `X-Forwarded-Proto` ek yapılandırma olmadan üstbilgileri. Uygulama ulaştığında yönlendirilirken istekleri bu üstbilgileri içermiyorsa Gereci üreticinin Kılavuzu başvurun.

Üstbilgiler Ara iletilen [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir. Varsayılan ayarlar şunlardır:

* Olduğundan yalnızca *bir proxy* uygulama ve istekleri kaynak arasında.
* Yalnızca geri döngü adresleri bilinen proxy'leri için yapılandırılır ve ağları bilinen.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express ve ASP.NET Core Modülü

IIS ve ASP.NET Core modülü arkasındaki uygulama çalıştırıldığında iletilen üstbilgileri Ara IIS tümleştirme ara yazılım tarafından varsayılan olarak etkindir. İletilen üstbilgileri Ara iletilen üst bilgileri ile güven sorunları nedeniyle ASP.NET Core Modülü'nü ilk ara yazılım ardışık düzenini belirli kısıtlı bir yapılandırma ile olarak çalıştırmak için etkinleştirildi (örneğin, [IP yanıltma](https://www.iplocation.net/ip-spoofing)). Ara yazılım iletmek için yapılandırılan `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgiler ve tek localhost proxy ile sınırlıdır. Ek yapılandırma gerekli olup olmadığını [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Diğer proxy sunucusu ve yük dengeleyici senaryoları

IIS tümleştirme ara yazılımı kullanarak dışında iletilen üstbilgileri ara yazılımı varsayılan olarak etkin değildir. İletilen üstbilgileri ara yazılım etkin, bir uygulama ile iletilen işlem başlıklar için [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Ara yazılım yoksa etkinleştirdikten sonra [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ara yazılımı varsayılan belirtilen [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) olan [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Ara yazılımla yapılandırma [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletmek için `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerinde `Startup.ConfigureServices`. Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` diğer ara yazılımdan çağırmadan önce:

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
> Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen `Startup.ConfigureServices` veya doğrudan uzantısı yöntemiyle [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), varsayılan iletmek için başlıkları [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) özelliği iletmek için üstbilgiler ile yapılandırılması gerekir.

## <a name="forwarded-headers-middleware-options"></a>İletilen üstbilgileri ara yazılım seçenekleri

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletilen üstbilgileri ara yazılım davranışını denetler:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| Seçenek | Açıklama |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Hangi ileticiler işlenmesi gerektiğini tanımlar. Bkz: [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) için geçerli bir alanlar listesi. Bu özelliğe atanmış tipik değerler <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Varsayılan değer [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | İşlenen üstbilgileri girişlerinde sayısını sınırlar. Kümesine `null` sınırı, ancak bu devre dışı bırakmak için yalnızca varsa yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.<br><br>Varsayılan değer 1'dir. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Adres aralıklarını iletilen üstbilgileri kabul etmek için bilinen proxy. Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak IP aralıklarını belirtin.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP ağı](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> için tek bir girdi içeren `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | İletilen üstbilgileri kabul etmek için bilinen Proxy adresleri. Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPADDRESS](/dotnet/api/system.net.ipaddress)> için tek bir girdi içeren `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Varsayılan, `X-Original-For` değeridir. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Varsayılan, `X-Original-Host` değeridir. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Üstbilgi değerleri arasında eşit sayıda gerekir [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) işleniyor.<br><br>Varsayılan olarak ASP.NET 1.x olan çekirdek `true`. Varsayılan ASP.NET Core 2.0 veya sonraki sürümlerde `false`. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Seçenek | Açıklama |
| ------ | ----------- |
| AllowedHosts | Ana bilgisayarı tarafından kısıtlayan `X-Forwarded-Host` sağlanan değerlere üstbilgi.<ul><li>Değerleri, sıra yoksay çalışması kullanılarak karşılaştırılır.</li><li>Bağlantı noktası numaralarını dışında tutulması gerekir.</li><li>Liste boşsa, tüm konaklar izin verilir.</li><li>Üst düzey bir joker karakter `*` tüm boş ana sağlar.</li><li>Joker karakterler alt etki alanı izin verilen ancak kök etki alanı eşleşmiyor. Örneğin, `*.contoso.com` alt etki alanı eşleşen `foo.contoso.com` ancak kök etki alanı değil `contoso.com`.</li><li>Unicode ana bilgisayar adlarını izin verilir ancak dönüştürülür [Punycode](https://tools.ietf.org/html/rfc3492) eşleme.</li><li>[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) köşeli sınırlayıcı içerir ve olması gereken [geleneksel form](https://tools.ietf.org/html/rfc4291#section-2.2) (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). Özel-ortası farklı biçimleri arasında mantıksal eşitlik denetlemek için IPv6 adresleri değil ve hiçbir Standartlaştırma gerçekleştirilir.</li><li>İzin verilen ana kısıtlamak için hata, bir saldırganın hizmeti tarafından oluşturulan bağlantılar sızmasını izin verebilir.</li></ul>Boş bir varsayılan değer: [IList\<dize >](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Hangi ileticiler işlenmesi gerektiğini tanımlar. Bkz: [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) için geçerli bir alanlar listesi. Bu özelliğe atanmış tipik değerler <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Varsayılan değer [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | İşlenen üstbilgileri girişlerinde sayısını sınırlar. Kümesine `null` sınırı, ancak bu devre dışı bırakmak için yalnızca varsa yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.<br><br>Varsayılan değer 1'dir. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Adres aralıklarını iletilen üstbilgileri kabul etmek için bilinen proxy. Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak IP aralıklarını belirtin.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP ağı](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> için tek bir girdi içeren `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | İletilen üstbilgileri kabul etmek için bilinen Proxy adresleri. Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPADDRESS](/dotnet/api/system.net.ipaddress)> için tek bir girdi içeren `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Varsayılan, `X-Original-For` değeridir. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Varsayılan, `X-Original-Host` değeridir. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Üstbilgi değerleri arasında eşit sayıda gerekir [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) işleniyor.<br><br>Varsayılan olarak ASP.NET 1.x olan çekirdek `true`. Varsayılan ASP.NET Core 2.0 veya sonraki sürümlerde `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Senaryolar ve kullanım örnekleri

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Üstbilgiler ve tüm isteklerin iletilen eklemek mümkün değilse güvenli

Bazı durumlarda, uygulamaya yönlendirilirken istekleri iletilen üstbilgilerini eklemek mümkün olmayabilir. Tüm genel dış istekleri HTTPS olduğunu proxy zorlama, düzeni el ile ayarlanabilir `Startup.Configure` ara yazılım herhangi bir türde kullanmadan önce:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Bu kod bir ortam değişkeni veya geliştirme veya hazırlama ortamında diğer yapılandırma ayarı devre dışı bırakılabilir.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>İstek yolu değiştirmek proxy'leri temel yolu ile Dağıt

Bazı proxy'leri yolun olduğu gibi geçirmek, ancak bir uygulamayla yönlendirme kaldırılması temel yolu düzgün çalışır. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) ara yazılım böler yola [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) ve uygulama temel yola [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Varsa `/foo` bir proxy yolu olarak geçirilen uygulama temel yolu aranır `/foo/api/1`, ara yazılımı kümeleri `Request.PathBase` için `/foo` ve `Request.Path` için `/api/1` aşağıdaki komutla:

```csharp
app.UsePathBase("/foo");
```

Geriye doğru ara yazılımı yeniden çağrıldığında orijinal yolunu ve yolu tabanı yeniden uygulanır. Ara yazılım sipariş işleme hakkında daha fazla bilgi için bkz: [Ara](xref:fundamentals/middleware/index).

Proxy yolu kırpar varsa (örneğin, iletme `/foo/api/1` için `/api/1`), düzeltme yönlendirir ve bağlantıları isteğin ayarlayarak [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) özelliği:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Proxy yolu veri ekleme, yeniden yönlendirir ve bağlantıları kullanarak düzeltmek için yolun bir kısmı atmak [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) ve atama [yolu](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) özelliği:

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

## <a name="troubleshoot"></a>Sorun giderme

Üstbilgiler beklendiği gibi iletilen değil, etkinleştirme [günlüğü](xref:fundamentals/logging/index). Günlükleri sorunu gidermek için yeterli bilgi sağlamıyorsa, sunucu tarafından alınan istek üstbilgileri sıralar. Satır içi ara yazılımı kullanarak bir uygulama yanıt üstbilgileri yazılabilir:

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
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
    }
}
```

-İletilen - X emin * beklenen değerler ile sunucu tarafından alınan üstbilgileri. Belirli bir üstbilgisinde birden çok değer varsa iletilen üstbilgileri Ara işlemleri üstbilgileri ters sırada sağdan sola unutmayın.

İsteğin özgün uzak IP bir girişe eşleşmelidir `KnownProxies` veya `KnownNetworks` X-iletilen-için işlenmeden önce listeler. Bu, güvenilmeyen proxy'leri İleticilerden kabul etmeyerek üstbilgi yanıltma sınırlar.

## <a name="additional-resources"></a>Ek kaynaklar

* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı](https://github.com/aspnet/Announcements/issues/295)
