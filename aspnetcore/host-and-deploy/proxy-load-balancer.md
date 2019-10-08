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
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="d0707-103">Proxy sunucularıyla ve yük dengeleyicilerle çalışacak ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d0707-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="d0707-104">[Luke Latham](https://github.com/guardrex) ve [Chris](https://github.com/Tratcher) 'e göre</span><span class="sxs-lookup"><span data-stu-id="d0707-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="d0707-105">ASP.NET Core için Önerilen yapılandırmada, uygulama IIS/ASP. NET Core modülü, NGINX veya Apache kullanılarak barındırılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="d0707-106">Proxy sunucuları, yük dengeleyiciler ve diğer ağ gereçleri, genellikle istek hakkında uygulamaya ulaşmadan önce bu bilgileri gizlemediğinde:</span><span class="sxs-lookup"><span data-stu-id="d0707-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="d0707-107">HTTPS istekleri HTTP üzerinden proxy olduğunda, özgün düzen (HTTPS) kaybolur ve bir üst bilgide iletilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0707-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="d0707-108">Bir uygulama, Internet veya şirket ağında gerçek kaynağını değil, ara sunucudan bir istek aldığından, kaynak istemci IP adresinin de bir üst bilgide iletilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d0707-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="d0707-109">Bu bilgiler, istek işlemede, örneğin yeniden yönlendirmeler, kimlik doğrulama, bağlantı oluşturma, ilke değerlendirmesi ve istemci coğrafi konum gibi önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geolocation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="d0707-110">İletilen üstbilgiler</span><span class="sxs-lookup"><span data-stu-id="d0707-110">Forwarded headers</span></span>

<span data-ttu-id="d0707-111">Kurala göre, proxy 'leri HTTP başlıklarında iletme bilgileri.</span><span class="sxs-lookup"><span data-stu-id="d0707-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="d0707-112">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="d0707-112">Header</span></span> | <span data-ttu-id="d0707-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d0707-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="d0707-114">X-Iletilmiş-Için</span><span class="sxs-lookup"><span data-stu-id="d0707-114">X-Forwarded-For</span></span> | <span data-ttu-id="d0707-115">, İsteği başlatan istemciyle ilgili bilgileri ve bir proxy zincirinde sonraki proxy 'leri barındırır.</span><span class="sxs-lookup"><span data-stu-id="d0707-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="d0707-116">Bu parametre IP adresleri (ve isteğe bağlı olarak bağlantı noktası numaraları) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="d0707-117">Bir ara sunucu zincirinde, ilk parametre isteğin ilk yaptığı istemciyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="d0707-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="d0707-118">Sonraki proxy tanımlayıcıları izler.</span><span class="sxs-lookup"><span data-stu-id="d0707-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="d0707-119">Zincirdeki son proxy, Parametreler listesinde değil.</span><span class="sxs-lookup"><span data-stu-id="d0707-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="d0707-120">Son proxy 'nin IP adresi ve isteğe bağlı olarak bir bağlantı noktası numarası, aktarım katmanında uzak IP adresi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="d0707-121">X-Iletilen-proto</span><span class="sxs-lookup"><span data-stu-id="d0707-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="d0707-122">Kaynak düzenin değeri (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d0707-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="d0707-123">Bu değer, istek birden çok proxy 'ye geçen bir düzen listesi de olabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="d0707-124">X-Iletilen-konak</span><span class="sxs-lookup"><span data-stu-id="d0707-124">X-Forwarded-Host</span></span> | <span data-ttu-id="d0707-125">Ana bilgisayar üst bilgisi alanının özgün değeri.</span><span class="sxs-lookup"><span data-stu-id="d0707-125">The original value of the Host header field.</span></span> <span data-ttu-id="d0707-126">Genellikle, proxy 'ler ana bilgisayar üst bilgisini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="d0707-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="d0707-127">Proxy 'nin konak üstbilgilerini doğrulamadığı veya kısıtlayamayacağı sistemleri etkileyen ayrıcalık yükselmesi güvenlik açığı hakkında bilgi için bkz. [Microsoft Güvenlik Danışmanlığı CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) .</span><span class="sxs-lookup"><span data-stu-id="d0707-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restrict Host headers to known good values.</span></span> |

<span data-ttu-id="d0707-128">[Microsoft. AspNetCore. HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketindeki Iletilen üstbilgiler ara yazılımı, bu üst bilgileri okur ve <xref:Microsoft.AspNetCore.Http.HttpContext> ' deki ilişkili alanları doldurur.</span><span class="sxs-lookup"><span data-stu-id="d0707-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="d0707-129">Ara yazılım güncelleştirmeleri:</span><span class="sxs-lookup"><span data-stu-id="d0707-129">The middleware updates:</span></span>

* <span data-ttu-id="d0707-130">[HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; `X-Forwarded-For` üst bilgi değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-130">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="d0707-131">Ek ayarlar, ara yazılım `RemoteIpAddress` ' ın nasıl ayarlanacağını etkiler.</span><span class="sxs-lookup"><span data-stu-id="d0707-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="d0707-132">Ayrıntılar için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="d0707-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="d0707-133">@No__t `X-Forwarded-Proto` üst bilgi değeri kullanılarak ayarlanan [HttpContext. Request. Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) -1.</span><span class="sxs-lookup"><span data-stu-id="d0707-133">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="d0707-134">[HttpContext. Request. Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; `X-Forwarded-Host` üst bilgi değeri kullanılarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-134">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="d0707-135">İletilen üstbilgiler ara yazılımı [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-135">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="d0707-136">Varsayılan ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d0707-136">The default settings are:</span></span>

* <span data-ttu-id="d0707-137">Uygulama ve isteklerin kaynağı arasında yalnızca *bir ara sunucu* vardır.</span><span class="sxs-lookup"><span data-stu-id="d0707-137">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="d0707-138">Bilinen proxy 'ler ve bilinen ağlar için yalnızca geri döngü adresleri yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-138">Only loopback addresses are configured for known proxies and known networks.</span></span>
* <span data-ttu-id="d0707-139">İletilen üstbilgiler `X-Forwarded-For` ve `X-Forwarded-Proto` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-139">The forwarded headers are named `X-Forwarded-For` and `X-Forwarded-Proto`.</span></span>

<span data-ttu-id="d0707-140">Tüm ağ gereçleri, ek yapılandırma olmadan `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini eklemez.</span><span class="sxs-lookup"><span data-stu-id="d0707-140">Not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="d0707-141">Proxy istekler uygulamaya ulaştığında bu üst bilgileri içermiyorsa, Gereç üreticinizin kılavuzuna başvurun.</span><span class="sxs-lookup"><span data-stu-id="d0707-141">Consult your appliance manufacturer's guidance if proxied requests don't contain these headers when they reach the app.</span></span> <span data-ttu-id="d0707-142">Gereç `X-Forwarded-For` ve `X-Forwarded-Proto` ' den farklı üstbilgi adları kullanıyorsa, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini gereç tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-142">If the appliance uses different header names than `X-Forwarded-For` and `X-Forwarded-Proto`, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the appliance.</span></span> <span data-ttu-id="d0707-143">Daha fazla bilgi için bkz. [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı üstbilgi adları kullanan bir proxy için yapılandırma](#configuration-for-a-proxy-that-uses-different-header-names).</span><span class="sxs-lookup"><span data-stu-id="d0707-143">For more information, see [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) and [Configuration for a proxy that uses different header names](#configuration-for-a-proxy-that-uses-different-header-names).</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="d0707-144">IIS/IIS Express ve ASP.NET Core modülü</span><span class="sxs-lookup"><span data-stu-id="d0707-144">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="d0707-145">İletilen üstbilgiler ara yazılımı, uygulama IIS 'nin arkasında ve ASP.NET Core modülünden sonra [işlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model) barındırılıyorsa [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) tarafından varsayılan olarak etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-145">Forwarded Headers Middleware is enabled by default by [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when the app is hosted [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="d0707-146">İletilen üstbilgiler ara yazılımı, iletilen üst bilgilerle (örneğin, [IP aldatmacası](https://www.iplocation.net/ip-spoofing)) güven sorunları nedeniyle ASP.NET Core modülüne özgü kısıtlanmış bir yapılandırmaya sahip olan ara yazılım ardışık düzeninde ilk olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="d0707-146">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="d0707-147">Ara yazılım `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini iletmek üzere yapılandırılmıştır ve tek bir localhost proxy ile kısıtlanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-147">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="d0707-148">Ek yapılandırma gerekliyse, [Iletilen üstbilgiler ara yazılım seçeneklerine](#forwarded-headers-middleware-options)bakın.</span><span class="sxs-lookup"><span data-stu-id="d0707-148">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d0707-149">Diğer proxy sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="d0707-149">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d0707-150">[İşlem dışı](xref:host-and-deploy/iis/index#out-of-process-hosting-model)barındırma sırasında [IIS tümleştirmesi](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) 'nin kullanılması dışında, iletilen üstbilgiler ara yazılımı varsayılan olarak etkinleştirilmemiştir.</span><span class="sxs-lookup"><span data-stu-id="d0707-150">Outside of using [IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="d0707-151">Bir uygulamanın iletilen üstbilgileri <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> ile işlemesi için iletilen üstbilgiler ara yazılımı etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="d0707-151">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span></span> <span data-ttu-id="d0707-152">Ara yazılım için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilmemişse, ara yazılımı etkinleştirdikten sonra varsayılan [Forwardedheadersoptions. ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) , [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-152">After enabling the middleware if no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span>

<span data-ttu-id="d0707-153">@No__t-0 ile ara yazılımı, `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerini `Startup.ConfigureServices` ' te iletmek üzere yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-153">Configure the middleware with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d0707-154">Diğer ara yazılım çağrılmadan önce `Startup.Configure` <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="d0707-154">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling other middleware:</span></span>

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
> <span data-ttu-id="d0707-155">@No__t-0 `Startup.ConfigureServices` ' de veya doğrudan <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> ile uzantı yöntemine belirtilmemişse, iletmek için varsayılan üstbilgiler [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified in `Startup.ConfigureServices` or directly to the extension method with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, the default headers to forward are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="d0707-156">@No__t-0 özelliği, iletmek için üst bilgilerle yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d0707-156">The <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> property must be configured with the headers to forward.</span></span>

## <a name="nginx-configuration"></a><span data-ttu-id="d0707-157">NGINX yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d0707-157">Nginx configuration</span></span>

<span data-ttu-id="d0707-158">@No__t-0 ve `X-Forwarded-Proto` üst bilgilerini iletmek için, bkz. <xref:host-and-deploy/linux-nginx#configure-nginx>.</span><span class="sxs-lookup"><span data-stu-id="d0707-158">To forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers, see <xref:host-and-deploy/linux-nginx#configure-nginx>.</span></span> <span data-ttu-id="d0707-159">Daha fazla bilgi için bkz. [NGıNX: Iletilen üstbilgi @ no__t-0 ' i kullanma.</span><span class="sxs-lookup"><span data-stu-id="d0707-159">For more information, see [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span></span>

## <a name="apache-configuration"></a><span data-ttu-id="d0707-160">Apache yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d0707-160">Apache configuration</span></span>

<span data-ttu-id="d0707-161">`X-Forwarded-For` otomatik olarak eklenir (bkz. [Apache Module mod_proxy: Ters proxy Istek üstbilgileri @ no__t-0).</span><span class="sxs-lookup"><span data-stu-id="d0707-161">`X-Forwarded-For` is added automatically (see [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span></span> <span data-ttu-id="d0707-162">@No__t-0 üst bilgisini iletme hakkında daha fazla bilgi için, bkz. <xref:host-and-deploy/linux-apache#configure-apache>.</span><span class="sxs-lookup"><span data-stu-id="d0707-162">For information on how to forward the `X-Forwarded-Proto` header, see <xref:host-and-deploy/linux-apache#configure-apache>.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="d0707-163">İletilen üstbilgiler ara yazılım seçenekleri</span><span class="sxs-lookup"><span data-stu-id="d0707-163">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="d0707-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> Iletilen üstbilgiler ara yazılımı davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="d0707-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> control the behavior of the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d0707-165">Aşağıdaki örnek varsayılan değerleri değiştirir:</span><span class="sxs-lookup"><span data-stu-id="d0707-165">The following example changes the default values:</span></span>

* <span data-ttu-id="d0707-166">İletilen üst bilgilerdeki giriş sayısını `2` olarak sınırlandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-166">Limit the number of entries in the forwarded headers to `2`.</span></span>
* <span data-ttu-id="d0707-167">@No__t-0 ' ın bilinen bir proxy adresini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d0707-167">Add a known proxy address of `127.0.10.1`.</span></span>
* <span data-ttu-id="d0707-168">İletilen üst bilgi adını varsayılan `X-Forwarded-For` ' dan `X-Forwarded-For-My-Custom-Header-Name` ' e değiştirin.</span><span class="sxs-lookup"><span data-stu-id="d0707-168">Change the forwarded header name from the default `X-Forwarded-For` to `X-Forwarded-For-My-Custom-Header-Name`.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| <span data-ttu-id="d0707-169">Seçenek</span><span class="sxs-lookup"><span data-stu-id="d0707-169">Option</span></span> | <span data-ttu-id="d0707-170">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d0707-170">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> | <span data-ttu-id="d0707-171">@No__t-0 üst bilgisine sahip Konakları belirtilen değerlerle kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="d0707-171">Restricts hosts by the `X-Forwarded-Host` header to the values provided.</span></span><ul><li><span data-ttu-id="d0707-172">Değerler, Ordinal-Ignore-Case kullanılarak karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-172">Values are compared using ordinal-ignore-case.</span></span></li><li><span data-ttu-id="d0707-173">Bağlantı noktası numaraları dışlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d0707-173">Port numbers must be excluded.</span></span></li><li><span data-ttu-id="d0707-174">Liste boşsa, tüm konaklara izin verilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-174">If the list is empty, all hosts are allowed.</span></span></li><li><span data-ttu-id="d0707-175">En üst düzey joker karakter `*` boş olmayan tüm konaklara izin verir.</span><span class="sxs-lookup"><span data-stu-id="d0707-175">A top-level wildcard `*` allows all non-empty hosts.</span></span></li><li><span data-ttu-id="d0707-176">Alt etki alanı Joker karakterlere izin verilir ancak kök etki alanıyla eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="d0707-176">Subdomain wildcards are permitted but don't match the root domain.</span></span> <span data-ttu-id="d0707-177">Örneğin, `*.contoso.com` alt etki alanı `foo.contoso.com` ile eşleşir, ancak `contoso.com` kök etki alanını değil.</span><span class="sxs-lookup"><span data-stu-id="d0707-177">For example, `*.contoso.com` matches the subdomain `foo.contoso.com` but not the root domain `contoso.com`.</span></span></li><li><span data-ttu-id="d0707-178">Unicode ana bilgisayar adlarına izin verilir, ancak eşleştirme için [puni koduna](https://tools.ietf.org/html/rfc3492) dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="d0707-178">Unicode host names are allowed but are converted to [Punycode](https://tools.ietf.org/html/rfc3492) for matching.</span></span></li><li><span data-ttu-id="d0707-179">[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) sınırlayıcı ayraçları içermeli ve [geleneksel biçimde](https://tools.ietf.org/html/rfc4291#section-2.2) olmalıdır (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span><span class="sxs-lookup"><span data-stu-id="d0707-179">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) must include bounding brackets and be in [conventional form](https://tools.ietf.org/html/rfc4291#section-2.2) (for example, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span></span> <span data-ttu-id="d0707-180">IPv6 adresleri, farklı biçimler arasında mantıksal eşitlik denetimi için özel değildir ve hiçbir kurallı kullanım yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="d0707-180">IPv6 addresses aren't special-cased to check for logical equality between different formats, and no canonicalization is performed.</span></span></li><li><span data-ttu-id="d0707-181">İzin verilen konaklar kısıtlanamaması, bir saldırganın hizmet tarafından oluşturulan bağlantıları sızdırmasına izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-181">Failure to restrict the allowed hosts may allow an attacker to spoof links generated by the service.</span></span></li></ul><span data-ttu-id="d0707-182">Varsayılan değer boş bir `IList<string>` ' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-182">The default value is an empty `IList<string>`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | <span data-ttu-id="d0707-183">Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName)tarafından belirtilen bir yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-183">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span></span> <span data-ttu-id="d0707-184">Bu seçenek, proxy/iletici `X-Forwarded-For` üst bilgisini kullanmadığınızda, ancak bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-184">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-For` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d0707-185">Varsayılan, `X-Forwarded-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-185">The default is `X-Forwarded-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | <span data-ttu-id="d0707-186">Hangi ileticilerin işleneceğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d0707-186">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="d0707-187">Uygulanan alanların listesi için [Forwardedheaders numaralandırması](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) ' ne bakın.</span><span class="sxs-lookup"><span data-stu-id="d0707-187">See the [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) for the list of fields that apply.</span></span> <span data-ttu-id="d0707-188">Bu özelliğe atanan tipik değerler `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto` ' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-188">Typical values assigned to this property are `ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.</span></span><br><br><span data-ttu-id="d0707-189">Varsayılan değer [Forwardedheaders. None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders)' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-189">The default value is [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | <span data-ttu-id="d0707-190">[Forwardedheadersdefaults varsayılan. XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-190">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span></span> <span data-ttu-id="d0707-191">Bu seçenek, proxy/iletici `X-Forwarded-Host` üst bilgisini kullanmadığınızda, ancak bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-191">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Host` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d0707-192">Varsayılan, `X-Forwarded-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-192">The default is `X-Forwarded-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | <span data-ttu-id="d0707-193">Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName)tarafından belirtilen bir yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-193">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span></span> <span data-ttu-id="d0707-194">Bu seçenek, proxy/iletici `X-Forwarded-Proto` üst bilgisini kullanmadığınızda, ancak bilgileri iletmek için başka bir üst bilgi kullandığında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d0707-194">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Proto` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="d0707-195">Varsayılan, `X-Forwarded-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-195">The default is `X-Forwarded-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | <span data-ttu-id="d0707-196">İşlenen üst bilgilerdeki giriş sayısını sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="d0707-196">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="d0707-197">Limiti devre dışı bırakmak için `null` olarak ayarlayın, ancak yalnızca `KnownProxies` veya `KnownNetworks` yapılandırılmışsa bu işlem yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d0707-197">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span> <span data-ttu-id="d0707-198">@No__t-0 olmayan bir değer ayarlamak, hatalı yapılandırılmış proxy 'lerde ve ağdaki yan kanallardan gelen kötü amaçlı isteklere karşı koruma sağlamak için bir önlem (garanti değildir) olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-198">Setting a non-`null` value is a precaution (but not a guarantee) to guard against misconfigured proxies and malicious requests arriving from side-channels on the network.</span></span><br><br><span data-ttu-id="d0707-199">İletilen üstbilgiler ara yazılımı, üst bilgileri sağdan sola doğru sırada işler.</span><span class="sxs-lookup"><span data-stu-id="d0707-199">Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="d0707-200">Varsayılan değer (`1`) kullanılırsa, `ForwardLimit` değeri arttırılmadığı müddetçe yalnızca üst bilgilerden en sağdaki değer işlenir.</span><span class="sxs-lookup"><span data-stu-id="d0707-200">If the default value (`1`) is used, only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span><br><br><span data-ttu-id="d0707-201">Varsayılan, `1` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-201">The default is `1`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | <span data-ttu-id="d0707-202">İletilen üstbilgileri kabul etmek için bilinen ağların adres aralıkları.</span><span class="sxs-lookup"><span data-stu-id="d0707-202">Address ranges of known networks to accept forwarded headers from.</span></span> <span data-ttu-id="d0707-203">Sınıfsız Etki alanları arası yönlendirme (CıDR) gösterimini kullanarak IP aralıkları sağlayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-203">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="d0707-204">Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 'de `::ffff:10.0.0.1` olarak temsil edilen `10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="d0707-204">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="d0707-205">Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d0707-205">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d0707-206">[HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-206">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="d0707-207">Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d0707-207">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="d0707-208">Varsayılan değer, `IPAddress.Loopback` için tek bir giriş içeren bir `IList` @ no__t-1 @ no__t-2 >.</span><span class="sxs-lookup"><span data-stu-id="d0707-208">The default is an `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> containing a single entry for `IPAddress.Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | <span data-ttu-id="d0707-209">İletilen üstbilgileri kabul etmek için bilinen proxy 'lerin adresleri.</span><span class="sxs-lookup"><span data-stu-id="d0707-209">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="d0707-210">Tam IP adresi eşleşmelerini belirtmek için `KnownProxies` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-210">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="d0707-211">Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 'de `::ffff:10.0.0.1` olarak temsil edilen `10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="d0707-211">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="d0707-212">Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d0707-212">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d0707-213">[HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-213">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="d0707-214">Daha fazla bilgi için, [IPv6 adresi olarak temsil edilen IPv4 adresi Için yapılandırma](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d0707-214">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="d0707-215">Varsayılan değer, `IPAddress.IPv6Loopback` için tek bir giriş içeren bir `IList` @ no__t-1 @ no__t-2 >.</span><span class="sxs-lookup"><span data-stu-id="d0707-215">The default is an `IList`\<<xref:System.Net.IPAddress>> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | <span data-ttu-id="d0707-216">Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName)tarafından belirtilen bir yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-216">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span></span><br><br><span data-ttu-id="d0707-217">Varsayılan, `X-Original-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-217">The default is `X-Original-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | <span data-ttu-id="d0707-218">[Forwardedheadersdefaults varsayılan. XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName)tarafından belirtilen yerine bu özellik tarafından belirtilen üstbilgiyi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-218">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span></span><br><br><span data-ttu-id="d0707-219">Varsayılan, `X-Original-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-219">The default is `X-Original-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | <span data-ttu-id="d0707-220">Bu özellik tarafından belirtilen üstbilgiyi, [Forwardedheadersdefaults varsayılan. XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName)tarafından belirtilen bir yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-220">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span></span><br><br><span data-ttu-id="d0707-221">Varsayılan, `X-Original-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="d0707-221">The default is `X-Original-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | <span data-ttu-id="d0707-222">Bilgi işlem sayısının, işlenmekte olan [Forwardedheadersoptions. ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) arasında eşitlenmiş olmasını gerektir.</span><span class="sxs-lookup"><span data-stu-id="d0707-222">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) being processed.</span></span><br><br><span data-ttu-id="d0707-223">ASP.NET Core 1. x içindeki varsayılan değer `true` ' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-223">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="d0707-224">ASP.NET Core 2,0 veya sonraki sürümlerde varsayılan değer `false` ' dır.</span><span class="sxs-lookup"><span data-stu-id="d0707-224">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="d0707-225">Senaryolar ve kullanım örnekleri</span><span class="sxs-lookup"><span data-stu-id="d0707-225">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="d0707-226">İletilen üstbilgiler eklemek mümkün olmadığında ve tüm istekler güvenlidir</span><span class="sxs-lookup"><span data-stu-id="d0707-226">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="d0707-227">Bazı durumlarda, iletilen üstbilgileri uygulamaya yönelik isteklere eklemek mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-227">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="d0707-228">Proxy tüm genel dış isteklerin HTTPS olduğunu zortacaktır, herhangi bir tür ara yazılım kullanılmadan önce Düzen `Startup.Configure` ' da el ile ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="d0707-228">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="d0707-229">Bu kod bir geliştirme veya hazırlama ortamındaki ortam değişkeniyle veya diğer yapılandırma ayarıyla devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="d0707-229">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="d0707-230">Yol tabanı ve istek yolunu değiştiren proxy 'ler ile uğraşın</span><span class="sxs-lookup"><span data-stu-id="d0707-230">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="d0707-231">Bazı proxy 'ler yolu bozulmadan, ancak yönlendirmenin düzgün çalışması için kaldırılması gereken bir uygulama temel yolu ile geçer.</span><span class="sxs-lookup"><span data-stu-id="d0707-231">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="d0707-232">[Usepathbaseextensions. usepathbase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) ara yazılımı, yolu HttpRequest. [Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) ve uygulama temeli yoluna, [HttpRequest. pathbase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase)içine böler.</span><span class="sxs-lookup"><span data-stu-id="d0707-232">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) middleware splits the path into [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) and the app base path into [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span></span>

<span data-ttu-id="d0707-233">@No__t-0, `/foo/api/1` olarak geçirilen bir ara sunucu yolu için uygulama temel yollarsa, ara yazılım `Request.PathBase` ' ye `/foo` ' e ve `Request.Path` ' e `/api/1` ' i aşağıdaki komutla ayarlar:</span><span class="sxs-lookup"><span data-stu-id="d0707-233">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="d0707-234">Özgün yol ve yol tabanı, ara yazılım ters ' de çağrıldığında yeniden uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-234">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="d0707-235">Ara yazılım sırası işleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="d0707-235">For more information on middleware order processing, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="d0707-236">Proxy, yolu kırpar (örneğin, `/foo/api/1` `/api/1` ' e iletiyorsa) isteğin [Pathbase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) özelliğini ayarlayarak yeniden yönlendirmeleri ve bağlantıları düzeltir:</span><span class="sxs-lookup"><span data-stu-id="d0707-236">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="d0707-237">Proxy yol verileri ekliyor ise, <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> kullanarak yeniden yönlendirmeleri ve bağlantıları onarmak üzere yolun bir bölümünü atın ve <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> özelliğine atama yapın:</span><span class="sxs-lookup"><span data-stu-id="d0707-237">If the proxy is adding path data, discard part of the path to fix redirects and links by using <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> and assigning to the <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> property:</span></span>

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

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a><span data-ttu-id="d0707-238">Farklı üstbilgi adları kullanan bir ara sunucu için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d0707-238">Configuration for a proxy that uses different header names</span></span>

<span data-ttu-id="d0707-239">Proxy, proxy adresi/bağlantı noktası ve kaynak düzen bilgilerini iletmek için `X-Forwarded-For` ve `X-Forwarded-Proto` adlı üstbilgileri kullanmıyorsa, <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> seçeneklerini proxy tarafından kullanılan üstbilgi adlarıyla eşleşecek şekilde ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="d0707-239">If the proxy doesn't use headers named `X-Forwarded-For` and `X-Forwarded-Proto` to forward the proxy address/port and originating scheme information, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the proxy:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a><span data-ttu-id="d0707-240">IPv6 adresi olarak temsil edilen IPv4 adresinin yapılandırması</span><span class="sxs-lookup"><span data-stu-id="d0707-240">Configuration for an IPv4 address represented as an IPv6 address</span></span>

<span data-ttu-id="d0707-241">Sunucu çift modlu yuvalar kullanıyorsa, IPv4 adresleri IPv6 biçiminde sağlanır (örneğin, IPv4 'de `::ffff:10.0.0.1` veya `::ffff:a00:1` olarak temsil edilen `10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="d0707-241">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1` or `::ffff:a00:1`).</span></span> <span data-ttu-id="d0707-242">Bkz. [IPAddress. MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="d0707-242">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="d0707-243">[HttpContext. Connection. Remoteıpaddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*)öğesine bakarak bu biçimin gerekip gerekmediğini saptayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-243">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span>

<span data-ttu-id="d0707-244">Aşağıdaki örnekte, iletilen üstbilgileri sağlayan bir ağ adresi, IPv6 biçimindeki `KnownNetworks` listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="d0707-244">In the following example, a network address that supplies forwarded headers is added to the `KnownNetworks` list in IPv6 format.</span></span>

<span data-ttu-id="d0707-245">IPv4 adresi: `10.11.12.1/8`</span><span class="sxs-lookup"><span data-stu-id="d0707-245">IPv4 address: `10.11.12.1/8`</span></span>

<span data-ttu-id="d0707-246">Dönüştürülen IPv6 adresi: `::ffff:10.11.12.1`</span><span class="sxs-lookup"><span data-stu-id="d0707-246">Converted IPv6 address: `::ffff:10.11.12.1`</span></span>  
<span data-ttu-id="d0707-247">Dönüştürülen önek uzunluğu: 104</span><span class="sxs-lookup"><span data-stu-id="d0707-247">Converted prefix length: 104</span></span>

<span data-ttu-id="d0707-248">Ayrıca, adresi onaltılık biçimde (`10.11.12.1` `::ffff:0a0b:0c01` olarak temsil edilen) sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0707-248">You can also supply the address in hexadecimal format (`10.11.12.1` represented in IPv6 as `::ffff:0a0b:0c01`).</span></span> <span data-ttu-id="d0707-249">Bir IPv4 adresini IPv6 'ya dönüştürürken, ek `::ffff:` IPv6 öneki (8 + 96 = 104) için hesaba CıDR önek uzunluğu (örnekteki `8`) 96 ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d0707-249">When converting an IPv4 address to IPv6, add 96 to the CIDR Prefix Length (`8` in the example) to account for the additional `::ffff:` IPv6 prefix (8 + 96 = 104).</span></span> 

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

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a><span data-ttu-id="d0707-250">Linux ve IIS olmayan ters proxy 'ler için Düzen iletme</span><span class="sxs-lookup"><span data-stu-id="d0707-250">Forward the scheme for Linux and non-IIS reverse proxies</span></span>

<span data-ttu-id="d0707-251">@No__t-0 ve <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> ' i çağıran uygulamalar, bir Azure Linux App Service, Azure Linux sanal makinesi (VM) veya IIS 'nin yanı sıra diğer tüm ters proxy 'ler için dağıtılan bir siteyi sonsuz döngüye koyar.</span><span class="sxs-lookup"><span data-stu-id="d0707-251">Apps that call <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> and <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*> put a site into an infinite loop if deployed to an Azure Linux App Service, Azure Linux virtual machine (VM), or behind any other reverse proxy besides IIS.</span></span> <span data-ttu-id="d0707-252">TLS, ters proxy tarafından sonlandırılır ve Kestrel doğru istek düzeninden haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="d0707-252">TLS is terminated by the reverse proxy, and Kestrel isn't made aware of the correct request scheme.</span></span> <span data-ttu-id="d0707-253">OAuth ve OıDC yanlış yeniden yönlendirmeler oluşturduğundan bu yapılandırmada de başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="d0707-253">OAuth and OIDC also fail in this configuration because they generate incorrect redirects.</span></span> <span data-ttu-id="d0707-254"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>, IIS 'nin arkasında çalışırken Iletilen üstbilgiler ara yazılımını ekler ve yapılandırır, ancak Linux için eşleşen otomatik yapılandırma (Apache veya NGINX tümleştirmesi) yoktur.</span><span class="sxs-lookup"><span data-stu-id="d0707-254"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> adds and configures Forwarded Headers Middleware when running behind IIS, but there's no matching automatic configuration for Linux (Apache or Nginx integration).</span></span>

<span data-ttu-id="d0707-255">IIS olmayan senaryolarda düzeni proxy 'den iletmek için, Iletilen üstbilgiler ara yazılımını ekleyin ve yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-255">To forward the scheme from the proxy in non-IIS scenarios, add and configure Forwarded Headers Middleware.</span></span> <span data-ttu-id="d0707-256">@No__t-0 ' da aşağıdaki kodu kullanın:</span><span class="sxs-lookup"><span data-stu-id="d0707-256">In `Startup.ConfigureServices`, use the following code:</span></span>

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

## <a name="certificate-forwarding"></a><span data-ttu-id="d0707-257">Sertifika iletme</span><span class="sxs-lookup"><span data-stu-id="d0707-257">Certificate forwarding</span></span> 

### <a name="azure"></a><span data-ttu-id="d0707-258">Azure</span><span class="sxs-lookup"><span data-stu-id="d0707-258">Azure</span></span>

<span data-ttu-id="d0707-259">Sertifika iletme için Azure App Service yapılandırmak için, bkz. [Azure App Service IÇIN TLS karşılıklı kimlik doğrulamasını yapılandırma](/azure/app-service/app-service-web-configure-tls-mutual-auth).</span><span class="sxs-lookup"><span data-stu-id="d0707-259">To configure Azure App Service for certificate forwarding, see [Configure TLS mutual authentication for Azure App Service](/azure/app-service/app-service-web-configure-tls-mutual-auth).</span></span> <span data-ttu-id="d0707-260">Aşağıdaki kılavuz ASP.NET Core uygulamasını yapılandırmaya aittir.</span><span class="sxs-lookup"><span data-stu-id="d0707-260">The following guidance pertains to configuring the ASP.NET Core app.</span></span>

<span data-ttu-id="d0707-261">@No__t-0 ' da, `app.UseAuthentication();` ' e çağrıdan önce aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0707-261">In `Startup.Configure`, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```


<span data-ttu-id="d0707-262">Sertifika Iletme ara yazılımını, Azure 'un kullandığı üst bilgi adını belirtecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-262">Configure Certificate Forwarding Middleware to specify the header name that Azure uses.</span></span> <span data-ttu-id="d0707-263">@No__t-0 ' da, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0707-263">In `Startup.ConfigureServices`, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="other-web-proxies"></a><span data-ttu-id="d0707-264">Diğer Web proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="d0707-264">Other web proxies</span></span>

<span data-ttu-id="d0707-265">IIS veya Azure App Service uygulama Isteği yönlendirme (ARR) olmayan bir ara sunucu kullanılıyorsa, proxy 'yi bir HTTP üst bilgisinde aldığı sertifikayı iletecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-265">If a proxy is used that isn't IIS or Azure App Service's Application Request Routing (ARR), configure the proxy to forward the certificate that it received in an HTTP header.</span></span> <span data-ttu-id="d0707-266">@No__t-0 ' da, `app.UseAuthentication();` ' e çağrıdan önce aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0707-266">In `Startup.Configure`, add the following code before the call to `app.UseAuthentication();`:</span></span>

```csharp
app.UseCertificateForwarding();
```

<span data-ttu-id="d0707-267">Üst bilgi adını belirtmek için sertifika Iletme ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-267">Configure the Certificate Forwarding Middleware to specify the header name.</span></span> <span data-ttu-id="d0707-268">@No__t-0 ' da, ara yazılımın bir sertifika oluşturmasındaki üstbilgiyi yapılandırmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0707-268">In `Startup.ConfigureServices`, add the following code to configure the header from which the middleware builds a certificate:</span></span>

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

<span data-ttu-id="d0707-269">Ara sunucu Base64 ile kodlanmazsa (NGINX ile olduğu gibi), `HeaderConverter` seçeneğini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d0707-269">If the proxy isn't base64-encoding the certificate (as is the case with Nginx), set the `HeaderConverter` option.</span></span> <span data-ttu-id="d0707-270">@No__t-0 ' da aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d0707-270">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

## <a name="troubleshoot"></a><span data-ttu-id="d0707-271">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="d0707-271">Troubleshoot</span></span>

<span data-ttu-id="d0707-272">Üstbilgiler beklendiği gibi iletilemediği zaman [günlüğe kaydetmeyi](xref:fundamentals/logging/index)etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="d0707-272">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="d0707-273">Günlükler sorunu gidermek için yeterli bilgi sağlamıyorsa, sunucu tarafından alınan istek üstbilgilerini numaralandırın.</span><span class="sxs-lookup"><span data-stu-id="d0707-273">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="d0707-274">Bir uygulama yanıtına istek üst bilgileri yazmak veya üst bilgileri günlüğe kaydetmek için satır içi ara yazılım kullanın.</span><span class="sxs-lookup"><span data-stu-id="d0707-274">Use inline middleware to write request headers to an app response or log the headers.</span></span> 

<span data-ttu-id="d0707-275">Üst bilgileri uygulamanın yanıtına yazmak için, `Startup.Configure` ' de <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> ' a çağrıdan sonra aşağıdaki Terminal satır içi ara yazılımını hemen yerleştirin:</span><span class="sxs-lookup"><span data-stu-id="d0707-275">To write the headers to the app's response, place the following terminal inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`:</span></span>

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

<span data-ttu-id="d0707-276">Yanıt gövdesi yerine günlüklere yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d0707-276">You can write to logs instead of the response body.</span></span> <span data-ttu-id="d0707-277">Günlüklere yazmak, sitenin hata ayıklanırken normal şekilde çalışmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d0707-277">Writing to logs allows the site to function normally while debugging.</span></span>

<span data-ttu-id="d0707-278">Yanıt gövdesi yerine günlükleri yazmak için:</span><span class="sxs-lookup"><span data-stu-id="d0707-278">To write logs rather than to the response body:</span></span>

* <span data-ttu-id="d0707-279">@No__t-0 ' `Startup` sınıfına, [Başlangıçta günlükleri oluşturma](xref:fundamentals/logging/index#create-logs-in-startup)bölümünde açıklandığı gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d0707-279">Inject `ILogger<Startup>` into the `Startup` class as described in [Create logs in Startup](xref:fundamentals/logging/index#create-logs-in-startup).</span></span>
* <span data-ttu-id="d0707-280">@No__t-1 ' de <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> ' a çağrıdan sonraki satır içi ara yazılımı hemen yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="d0707-280">Place the following inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`.</span></span>

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

<span data-ttu-id="d0707-281">İşlendiğinde `X-Forwarded-{For|Proto|Host}` değerleri `X-Original-{For|Proto|Host}` ' e taşınır.</span><span class="sxs-lookup"><span data-stu-id="d0707-281">When processed, `X-Forwarded-{For|Proto|Host}` values are moved to `X-Original-{For|Proto|Host}`.</span></span> <span data-ttu-id="d0707-282">Belirli bir üst bilgide birden çok değer varsa, Iletilen üstbilgiler ara yazılımı sağdan sola doğru sırada üst bilgileri işler.</span><span class="sxs-lookup"><span data-stu-id="d0707-282">If there are multiple values in a given header, Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="d0707-283">Varsayılan `1` ' dir (bir)  ' dir. bu nedenle, `ForwardLimit` değeri artmadığı müddetçe yalnızca en sağdaki üst bilgilerden en sağdaki değer işlenir.</span><span class="sxs-lookup"><span data-stu-id="d0707-283">The default `ForwardLimit` is `1` (one), so only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span>

<span data-ttu-id="d0707-284">İletilen üstbilgiler işlenmeden önce isteğin özgün uzak IP `KnownProxies` veya `KnownNetworks` listelerindeki bir girdiyle eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="d0707-284">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before forwarded headers are processed.</span></span> <span data-ttu-id="d0707-285">Bu, güvenilmeyen proxy 'lerden ileticileri kabul etmediği için üst bilgi yanıltmasını kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="d0707-285">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span> <span data-ttu-id="d0707-286">Bilinmeyen bir proxy algılandığında günlüğe kaydetme, proxy 'nin adresini gösterir:</span><span class="sxs-lookup"><span data-stu-id="d0707-286">When an unknown proxy is detected, logging indicates the address of the proxy:</span></span>

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

<span data-ttu-id="d0707-287">Yukarıdaki örnekte, alana 10.0.0.100 bir proxy sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="d0707-287">In the preceding example, 10.0.0.100 is a proxy server.</span></span> <span data-ttu-id="d0707-288">Sunucu güvenilir bir proxy ise, sunucunun IP adresini `KnownProxies` ' a ekleyin (veya `KnownNetworks` ' e bir güvenilen ağ ekleyin) `Startup.ConfigureServices` ' ye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d0707-288">If the server is a trusted proxy, add the server's IP address to `KnownProxies` (or add a trusted network to `KnownNetworks`) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d0707-289">Daha fazla bilgi için [Iletilen üstbilgiler ara yazılım seçenekleri](#forwarded-headers-middleware-options) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="d0707-289">For more information, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) section.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> <span data-ttu-id="d0707-290">Yalnızca güvenilen proxy 'lerin ve ağların üstbilgileri iletmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="d0707-290">Only allow trusted proxies and networks to forward headers.</span></span> <span data-ttu-id="d0707-291">Aksi takdirde, [IP sahtekarlığı](https://www.iplocation.net/ip-spoofing) saldırıları mümkündür.</span><span class="sxs-lookup"><span data-stu-id="d0707-291">Otherwise, [IP spoofing](https://www.iplocation.net/ip-spoofing) attacks are possible.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0707-292">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d0707-292">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
* <span data-ttu-id="d0707-293">[Microsoft Güvenlik Danışma CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="d0707-293">[Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability](https://github.com/aspnet/Announcements/issues/295)</span></span>
