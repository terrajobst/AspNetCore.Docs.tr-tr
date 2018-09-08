---
title: ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma
author: guardrex
description: Proxy sunucuları ve yük Dengeleyiciler, genellikle önemli bilgi gizlememeniz arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 1833b5bb77b199bb5fd0257e9f33b4d6f0c23ec5
ms.sourcegitcommit: 8268cc67beb1bb1ca470abb0e28b15a7a71b8204
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44126741"
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
| X iletilen konak | Ana bilgisayar üstbilgi alanının özgün değer. Genellikle proxy ana bilgisayar üst bilgisini değiştirmez. Bkz: [Microsoft Güvenlik Danışma CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) değil proxy burada doğrulama sistemleri etkiler ayrıcalıklar yükseltme güvenlik açığı veya bilinen iyi değerlere restict konak üstbilgileri hakkında bilgi için. |

İletilen üstbilgileri Ara gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketi, bu üst bilgilerini okur ve ilişkili alanları doldurur [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

Ara yazılım güncelleştirmeleri:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; kullanılarak ayarlanan `X-Forwarded-For` üstbilgi değeri. Ek ayarları etkiler nasıl ara yazılım ayarlar `RemoteIpAddress`. Ayrıntılar için bkz [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; kullanılarak ayarlanan `X-Forwarded-Proto` üstbilgi değeri.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; kullanılarak ayarlanan `X-Forwarded-Host` üstbilgi değeri.

Üst bilgileri ara yazılım iletilen [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir. Varsayılan ayarlar şunlardır:

* Yok, yalnızca *bir proxy* uygulama ve kaynak istekleri arasında.
* Yalnızca bir geri döngü adresine bilinen proxy'leri için yapılandırılır ve bilinen ağlar.
* İletilen üstbilgileri adlı `X-Forwarded-For` ve `X-Forwarded-Proto`.

Tüm ağ Gereçleri ekleyin `X-Forwarded-For` ve `X-Forwarded-Proto` ek yapılandırma olmadan üstbilgileri. Bunlar uygulamaya ulaştığında bu üstbilgileri proxy istekleri içermeyen Gereci üreticinin Kılavuzu danışın. Gereç değerinden farklı bir üst bilgi adları kullanıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto`ayarlayın [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) ve [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) tarafından kullanılan üstbilgi adlarını eşleştirmek için seçenekleri Gereç. Daha fazla bilgi için [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı bir üst bilgi adları kullanan bir proxy yapılandırması](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express ve ASP.NET Core Modülü

IIS ve ASP.NET Core modülü arkasındaki uygulama çalıştırıldığında iletilen üstbilgileri ara yazılım IIS tümleştirme ara yazılımı tarafından varsayılan olarak etkindir. İletilen üstbilgileri ara yazılım, ASP.NET Core modülü iletilen üst bilgilerine güven sorunları nedeniyle ilk ara yazılım ardışık düzenini belirli kısıtlı bir yapılandırma ile olarak çalıştırmak için etkinleştirilir (örneğin, [IP yanıltma](https://www.iplocation.net/ip-spoofing)). Ara yazılım iletecek şekilde yapılandırılmış `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri ve tek localhost proxy ile sınırlıdır. Ek yapılandırma gerekli olup olmadığını, [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Diğer bir ara sunucu ve yük dengeleyici senaryoları

IIS tümleştirme ara yazılımı kullanarak dışında iletilen üstbilgileri ara yazılımı varsayılan olarak etkin değil. İletilen üstbilgileri ara yazılım etkin, iletilen işlemi üst bilgilerine bir uygulama için [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Hayır ise, ara yazılım etkinleştirdikten sonra [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ara yazılımı varsayılan belirtilen [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) olan [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Ara yazılımla yapılandırma [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerinde `Startup.ConfigureServices`. Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` diğer ara yazılımdan çağırmadan önce:

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
> Hayır ise [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen `Startup.ConfigureServices` veya doğrudan genişletme yöntemi ile [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), varsayılan üst bilgileri iletmek için [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) özelliği iletmek üzere üst bilgileri ile yapılandırılması gerekir.

## <a name="nginx-configuration"></a>Ngınx yapılandırma

İletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini bkz <xref:host-and-deploy/linux-nginx#configure-nginx>. Daha fazla bilgi için [NGINX: iletilen üst bilgisini kullanarak](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Apache yapılandırma

`X-Forwarded-For` otomatik olarak eklenir (bkz [Apache modülü mod_proxy: Ters Proxy istek üstbilgileri](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). İletme hakkında bilgi için `X-Forwarded-Proto` başlığını görmek <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>İletilen üstbilgileri ara yazılım seçenekleri

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletilen üstbilgileri ara yazılım davranışını kontrol. Aşağıdaki örnek, varsayılan değerleri değiştirir:

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

::: moniker range=">= aspnetcore-2.1"
| Seçenek | Açıklama |
| ------ | ----------- |
| AllowedHosts | Ana bilgisayar tarafından sınırlar `X-Forwarded-Host` sağlanan değerler için üst bilgi.<ul><li>Sıra yoksay örneği kullanarak değerleri karşılaştırılır.</li><li>Bağlantı noktası numaralarını tutulması gerekir.</li><li>Liste boşsa, tüm konaklar izin verilir.</li><li>Üst düzey bir joker karakter `*` tüm boş konaklar sağlar.</li><li>Alt etki alanı joker karakterlere izin verilir, ancak kök etki alanı eşleşmiyor. Örneğin, `*.contoso.com` alt etki alanıyla eşleşen `foo.contoso.com` ancak kök etki alanı değil `contoso.com`.</li><li>Unicode ana bilgisayar adları kullanılabilir, ancak dönüştürülür [Punycode](https://tools.ietf.org/html/rfc3492) eşlemek için.</li><li>[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) ayraçlar sınırlayıcı içerir ve olması gereken [geleneksel form](https://tools.ietf.org/html/rfc4291#section-2.2) (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). IPv6 adresleri farklı biçimler arasında mantıksal eşitlik denetlemek için özel harfleri değil ve yok Standartlaştırma gerçekleştirilir.</li><li>İzin verilen konakları sınırlamak için başarısızlık, bir saldırganın hizmeti tarafından oluşturulan bağlantıları sızmasını.</li></ul>Boş bir varsayılan değer: [IList\<dizesi >](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-For` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Hangi ileticileri işlenmesi gerektiğini tanımlar. Bkz: [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) için geçerli olan alanların listesi. Bu özelliğe atanmış tipik değerler <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Varsayılan değer [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Host` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Proto` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | İşlenen üstbilgileri içerisindeki giriş sayısını sınırlar. Kümesine `null` sınırı, ancak bunu devre dışı bırakmak için yalnızca, yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.<br><br>Varsayılan değer 1'dir. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Adres aralıklarını iletilen üst bilgiler kabul etmek için bilinen ağlar. Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanan IP aralıklarını belirtin.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP ağı](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> için tek bir giriş içeren `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | İletilen üst bilgiler kabul etmek için bilinen Proxy adresleri. Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPADDRESS](/dotnet/api/system.net.ipaddress)> için tek bir giriş içeren `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Varsayılan, `X-Original-For` değeridir. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Varsayılan, `X-Original-Host` değeridir. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Üstbilgi değerleri arasında eşit olacak şekilde sayısı gerektir [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) işleniyor.<br><br>ASP.NET Core 1.x olan varsayılan `true`. ASP.NET Core 2.0 veya sonraki sürümlerde varsayılan `false`. |
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
| Seçenek | Açıklama |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-For` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-For` değeridir. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Hangi ileticileri işlenmesi gerektiğini tanımlar. Bkz: [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) için geçerli olan alanların listesi. Bu özelliğe atanmış tipik değerler <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Varsayılan değer [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Host` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Host` değeridir. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Proto` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.<br><br>Varsayılan, `X-Forwarded-Proto` değeridir. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | İşlenen üstbilgileri içerisindeki giriş sayısını sınırlar. Kümesine `null` sınırı, ancak bunu devre dışı bırakmak için yalnızca, yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.<br><br>Varsayılan değer 1'dir. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Adres aralıklarını iletilen üst bilgiler kabul etmek için bilinen ağlar. Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanan IP aralıklarını belirtin.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP ağı](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> için tek bir giriş içeren `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | İletilen üst bilgiler kabul etmek için bilinen Proxy adresleri. Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.<br><br>Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`). Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.<br><br>Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPADDRESS](/dotnet/api/system.net.ipaddress)> için tek bir giriş içeren `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Varsayılan, `X-Original-For` değeridir. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Varsayılan, `X-Original-Host` değeridir. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Varsayılan, `X-Original-Proto` değeridir. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Üstbilgi değerleri arasında eşit olacak şekilde sayısı gerektir [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) işleniyor.<br><br>ASP.NET Core 1.x olan varsayılan `true`. ASP.NET Core 2.0 veya sonraki sürümlerde varsayılan `false`. |
::: moniker-end

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

Bazı Ara sunucular yolu sağlam geçirmek ancak bir uygulamayla yönlendirme böylece kaldırılmalıdır temel yolu düzgün çalışır. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) ara yazılım yolu böler [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) ve uygulama temel yolu [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Varsa `/foo` proxy yolu olarak geçirilen uygulama temel yolu aranır `/foo/api/1`, ara yazılım kümeleri `Request.PathBase` için `/foo` ve `Request.Path` için `/api/1` aşağıdaki komutla:

```csharp
app.UsePathBase("/foo");
```

Ara yazılım tersten yeniden çağrıldığında, orijinal yolunu ve yolu tabanı yeniden uygulanır. Ara yazılım sipariş işleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

Proxy yolu kırpar varsa (örneğin, iletme `/foo/api/1` için `/api/1`), düzeltme yönlendirir ve bağlantılar isteğin ayarlayarak [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) özelliği:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Proxy yol verileri ekliyorsanız yeniden yönlendirir ve bağlantıları kullanarak düzeltmek için yolunun parçası at [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) ve atama [yolu](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) özelliği:

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

Proxy adlı üstbilgileri kullanmıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto` proxy adresini/bağlantı noktası ve şema bilgilerini kaynaklanan iletmek için ayarlanmış [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) ve [ForwardedProtoHeaderName ](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) proxy tarafından kullanılan üstbilgi adlarını eşleştirmek için seçenekleri:

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

## <a name="troubleshoot"></a>Sorun giderme

Üst bilgiler, beklendiği gibi iletilen olmayan etkinleştirirsiniz [günlüğü](xref:fundamentals/logging/index). Günlükleri sorunu gidermek için yeterli bilgi sağlamazsanız, sunucu tarafından alınan isteği üstbilgileri sıralar. Satır içi ara yazılımı kullanarak bir uygulama yanıt üstbilgileri yazılabilir:

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
    });
}
```

Emin X - Forwarded-* üst bilgilerini beklenen değerlerle sunucu tarafından alınan. Belirli bir üst bilgisinde birden çok değer varsa, ters sırada sağdan sola iletilen üstbilgileri ara yazılım işlemleri üstbilgileri unutmayın.

İsteğin özgün uzak IP bir girişe eşleşmelidir `KnownProxies` veya `KnownNetworks` önce listeler `X-Forwarded-For` işlenir. Bu, güvenilir olmayan proxy'si İleticilerden kabul etmeyerek üst bilgi sızdırma sınırlar.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/web-farm>
* [Microsoft Security Advisory CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı](https://github.com/aspnet/Announcements/issues/295)
