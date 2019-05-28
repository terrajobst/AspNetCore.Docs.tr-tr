---
title: ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma
author: guardrex
description: Proxy sunucuları ve yük Dengeleyiciler, genellikle önemli bilgi gizlememeniz arkasında barındırılan uygulamalar için yapılandırma hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/24/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 2423b5bed760ad879d1c47c5e64b0f815b50397e
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251389"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="e2c4e-103">ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e2c4e-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="e2c4e-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e2c4e-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="e2c4e-105">ASP.NET Core için önerilen yapılandırma, IIS/ASP.NET çekirdek modülü, Ngınx veya Apache kullanarak uygulamayı barındırılır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="e2c4e-106">Proxy sunucuları, yük Dengeleyiciler ve diğer ağ Gereçleri, isteğiyle ilgili bilgileri genellikle uygulama ulaşmadan önce gizlememeniz:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="e2c4e-107">HTTPS isteklerini HTTP üzerinden taşınır, özgün şeması (HTTPS) kaybolur ve bir üst bilgisinde iletilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="e2c4e-108">Uygulama proxy'si ve doğru kaynağına değil Internet veya kurumsal ağ üzerindeki bir istek aldığından, kaynak istemci IP adresi de üst bilgisinde gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="e2c4e-109">Bu bilgiler, örneğin yeniden yönlendirmeleri, kimlik doğrulaması, bağlama oluşturmayı, ilke değerlendirmesi ve istemci coğrafi konum isteği işleme önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geolocation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="e2c4e-110">İletilen üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="e2c4e-110">Forwarded headers</span></span>

<span data-ttu-id="e2c4e-111">Kural gereği, HTTP üst bilgisinde proxy'leri iletin.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="e2c4e-112">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="e2c4e-112">Header</span></span> | <span data-ttu-id="e2c4e-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e2c4e-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="e2c4e-114">X-iletilen-için</span><span class="sxs-lookup"><span data-stu-id="e2c4e-114">X-Forwarded-For</span></span> | <span data-ttu-id="e2c4e-115">Proxy, zincirdeki sonraki proxy ve istek başlatılan istemci ile ilgili bilgileri tutar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="e2c4e-116">Bu parametre, IP adresleri (ve isteğe bağlı olarak, bağlantı noktası numaraları) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="e2c4e-117">Bir proxy sunucu zincirinde ilk parametre istek ilk yapıldığı istemci gösterir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="e2c4e-118">Sonraki proxy tanımlayıcıları izleyin.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="e2c4e-119">Son proxy zincirdeki parametreler listesinde değil.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="e2c4e-120">Son proxy IP adresini ve isteğe bağlı olarak bir bağlantı noktası numarası aktarım katmanında uzak IP adresi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="e2c4e-121">X iletilen Proto</span><span class="sxs-lookup"><span data-stu-id="e2c4e-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="e2c4e-122">Kaynak Düzen (HTTP/HTTPS) değeri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="e2c4e-123">İstek birden çok proxy'leri geçiş değilse değer düzenleri listesi de olabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="e2c4e-124">X iletilen konak</span><span class="sxs-lookup"><span data-stu-id="e2c4e-124">X-Forwarded-Host</span></span> | <span data-ttu-id="e2c4e-125">Ana bilgisayar üstbilgi alanının özgün değer.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-125">The original value of the Host header field.</span></span> <span data-ttu-id="e2c4e-126">Genellikle proxy ana bilgisayar üst bilgisini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="e2c4e-127">Bkz: [Microsoft Güvenlik Danışma CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) proxy burada değil veya doğrulamak için iyi bilinen değerler barındırma üstbilgileri kısıtlamak sistemleri etkileyen ayrıcalıklar yükseltme güvenlik açığı hakkında bilgi için.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restrict Host headers to known good values.</span></span> |

<span data-ttu-id="e2c4e-128">İletilen üstbilgileri Ara gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketi, bu üst bilgilerini okur ve ilişkili alanları doldurur <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

<span data-ttu-id="e2c4e-129">Ara yazılım güncelleştirmeleri:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-129">The middleware updates:</span></span>

* <span data-ttu-id="e2c4e-130">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; kullanılarak ayarlanan `X-Forwarded-For` üstbilgi değeri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-130">[HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="e2c4e-131">Ek ayarları etkiler nasıl ara yazılım ayarlar `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="e2c4e-132">Ayrıntılar için bkz [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="e2c4e-133">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; kullanılarak ayarlanan `X-Forwarded-Proto` üstbilgi değeri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-133">[HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="e2c4e-134">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; kullanılarak ayarlanan `X-Forwarded-Host` üstbilgi değeri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-134">[HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="e2c4e-135">Üst bilgileri ara yazılım iletilen [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-135">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="e2c4e-136">Varsayılan ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-136">The default settings are:</span></span>

* <span data-ttu-id="e2c4e-137">Yok, yalnızca *bir proxy* uygulama ve kaynak istekleri arasında.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-137">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="e2c4e-138">Yalnızca bir geri döngü adresine bilinen proxy'leri için yapılandırılır ve bilinen ağlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-138">Only loopback addresses are configured for known proxies and known networks.</span></span>
* <span data-ttu-id="e2c4e-139">İletilen üstbilgileri adlı `X-Forwarded-For` ve `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-139">The forwarded headers are named `X-Forwarded-For` and `X-Forwarded-Proto`.</span></span>

<span data-ttu-id="e2c4e-140">Tüm ağ Gereçleri ekleyin `X-Forwarded-For` ve `X-Forwarded-Proto` ek yapılandırma olmadan üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-140">Not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="e2c4e-141">Bunlar uygulamaya ulaştığında bu üstbilgileri proxy istekleri içermeyen Gereci üreticinin Kılavuzu danışın.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-141">Consult your appliance manufacturer's guidance if proxied requests don't contain these headers when they reach the app.</span></span> <span data-ttu-id="e2c4e-142">Gereç değerinden farklı bir üst bilgi adları kullanıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto`ayarlayın <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> gereç tarafından kullanılan üstbilgi adlarını eşleştirmek için Seçenekler.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-142">If the appliance uses different header names than `X-Forwarded-For` and `X-Forwarded-Proto`, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the appliance.</span></span> <span data-ttu-id="e2c4e-143">Daha fazla bilgi için [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options) ve [farklı bir üst bilgi adları kullanan bir proxy yapılandırması](#configuration-for-a-proxy-that-uses-different-header-names).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-143">For more information, see [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) and [Configuration for a proxy that uses different header names](#configuration-for-a-proxy-that-uses-different-header-names).</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="e2c4e-144">IIS/IIS Express ve ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="e2c4e-144">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="e2c4e-145">İletilen üstbilgileri ara yazılım, varsayılan olarak etkindir [IIS tümleştirme ara yazılımı](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) uygulamayı barındırılan zaman [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model) IIS ve ASP.NET Core modülü.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-145">Forwarded Headers Middleware is enabled by default by [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when the app is hosted [out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model) behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="e2c4e-146">İletilen üstbilgileri ara yazılım, ASP.NET Core modülü iletilen üst bilgilerine güven sorunları nedeniyle ilk ara yazılım ardışık düzenini belirli kısıtlı bir yapılandırma ile olarak çalıştırmak için etkinleştirilir (örneğin, [IP yanıltma](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-146">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="e2c4e-147">Ara yazılım iletecek şekilde yapılandırılmış `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgileri ve tek localhost proxy ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-147">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="e2c4e-148">Ek yapılandırma gerekli olup olmadığını, [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-148">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="e2c4e-149">Diğer bir ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="e2c4e-149">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="e2c4e-150">Kullanarak dışında [IIS tümleştirme](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) barındırırken [işlem dışı](xref:fundamentals/servers/index#out-of-process-hosting-model), iletilen üstbilgileri ara yazılım, varsayılan olarak etkin değildir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-150">Outside of using [IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) when hosting [out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="e2c4e-151">İletilen üstbilgileri ara yazılım etkin, iletilen işlemi üst bilgilerine bir uygulama için <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-151">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>.</span></span> <span data-ttu-id="e2c4e-152">Hayır ise, ara yazılım etkinleştirdikten sonra <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> ara yazılımı varsayılan belirtilen [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) olan [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-152">After enabling the middleware if no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span>

<span data-ttu-id="e2c4e-153">Ara yazılımla yapılandırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> iletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üst bilgilerinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-153">Configure the middleware with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e2c4e-154">Çağırma <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> yönteminde `Startup.Configure` diğer ara yazılımdan çağırmadan önce:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-154">Invoke the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in `Startup.Configure` before calling other middleware:</span></span>

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
> <span data-ttu-id="e2c4e-155">Hayır ise <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> belirtilen `Startup.ConfigureServices` veya doğrudan genişletme yöntemi ile <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, iletmek için varsayılan başlıkları [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-155">If no <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> are specified in `Startup.ConfigureServices` or directly to the extension method with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, the default headers to forward are [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> <span data-ttu-id="e2c4e-156"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> Özelliği iletmek üzere üst bilgileri ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-156">The <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> property must be configured with the headers to forward.</span></span>

## <a name="nginx-configuration"></a><span data-ttu-id="e2c4e-157">Ngınx yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e2c4e-157">Nginx configuration</span></span>

<span data-ttu-id="e2c4e-158">İletecek şekilde `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerini bkz <xref:host-and-deploy/linux-nginx#configure-nginx>.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-158">To forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers, see <xref:host-and-deploy/linux-nginx#configure-nginx>.</span></span> <span data-ttu-id="e2c4e-159">Daha fazla bilgi için [NGINX: İletilen üst bilgisini kullanarak](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-159">For more information, see [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).</span></span>

## <a name="apache-configuration"></a><span data-ttu-id="e2c4e-160">Apache yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e2c4e-160">Apache configuration</span></span>

<span data-ttu-id="e2c4e-161">`X-Forwarded-For` otomatik olarak eklenir (bkz [Apache modülü mod_proxy: Ters proxy istek üstbilgilerini](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-161">`X-Forwarded-For` is added automatically (see [Apache Module mod_proxy: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)).</span></span> <span data-ttu-id="e2c4e-162">İletme hakkında bilgi için `X-Forwarded-Proto` başlığını görmek <xref:host-and-deploy/linux-apache#configure-apache>.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-162">For information on how to forward the `X-Forwarded-Proto` header, see <xref:host-and-deploy/linux-apache#configure-apache>.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="e2c4e-163">İletilen üstbilgileri ara yazılım seçenekleri</span><span class="sxs-lookup"><span data-stu-id="e2c4e-163">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="e2c4e-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> İletilen üstbilgileri Ara yazılımının davranışını denetler.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-164"><xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> control the behavior of the Forwarded Headers Middleware.</span></span> <span data-ttu-id="e2c4e-165">Aşağıdaki örnek, varsayılan değerleri değiştirir:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-165">The following example changes the default values:</span></span>

* <span data-ttu-id="e2c4e-166">İletilen üst bilgilerde giriş sayısını sınırlamak `2`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-166">Limit the number of entries in the forwarded headers to `2`.</span></span>
* <span data-ttu-id="e2c4e-167">Bir bilinen proxy adresini ekleyin `127.0.10.1`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-167">Add a known proxy address of `127.0.10.1`.</span></span>
* <span data-ttu-id="e2c4e-168">Varsayılan iletilen üst bilgi adı değiştirmek `X-Forwarded-For` için `X-Forwarded-For-My-Custom-Header-Name`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-168">Change the forwarded header name from the default `X-Forwarded-For` to `X-Forwarded-For-My-Custom-Header-Name`.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| <span data-ttu-id="e2c4e-169">Seçenek</span><span class="sxs-lookup"><span data-stu-id="e2c4e-169">Option</span></span> | <span data-ttu-id="e2c4e-170">Açıklama</span><span class="sxs-lookup"><span data-stu-id="e2c4e-170">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="e2c4e-171">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="e2c4e-171">AllowedHosts</span></span> | <span data-ttu-id="e2c4e-172">Ana bilgisayar tarafından sınırlar `X-Forwarded-Host` sağlanan değerler için üst bilgi.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-172">Restricts hosts by the `X-Forwarded-Host` header to the values provided.</span></span><ul><li><span data-ttu-id="e2c4e-173">Sıra yoksay örneği kullanarak değerleri karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-173">Values are compared using ordinal-ignore-case.</span></span></li><li><span data-ttu-id="e2c4e-174">Bağlantı noktası numaralarını tutulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-174">Port numbers must be excluded.</span></span></li><li><span data-ttu-id="e2c4e-175">Liste boşsa, tüm konaklar izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-175">If the list is empty, all hosts are allowed.</span></span></li><li><span data-ttu-id="e2c4e-176">Üst düzey bir joker karakter `*` tüm boş konaklar sağlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-176">A top-level wildcard `*` allows all non-empty hosts.</span></span></li><li><span data-ttu-id="e2c4e-177">Alt etki alanı joker karakterlere izin verilir, ancak kök etki alanı eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-177">Subdomain wildcards are permitted but don't match the root domain.</span></span> <span data-ttu-id="e2c4e-178">Örneğin, `*.contoso.com` alt etki alanıyla eşleşen `foo.contoso.com` ancak kök etki alanı değil `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-178">For example, `*.contoso.com` matches the subdomain `foo.contoso.com` but not the root domain `contoso.com`.</span></span></li><li><span data-ttu-id="e2c4e-179">Unicode ana bilgisayar adları kullanılabilir, ancak dönüştürülür [Punycode](https://tools.ietf.org/html/rfc3492) eşlemek için.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-179">Unicode host names are allowed but are converted to [Punycode](https://tools.ietf.org/html/rfc3492) for matching.</span></span></li><li><span data-ttu-id="e2c4e-180">[IPv6 adresleri](https://tools.ietf.org/html/rfc4291) ayraçlar sınırlayıcı içerir ve olması gereken [geleneksel form](https://tools.ietf.org/html/rfc4291#section-2.2) (örneğin, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-180">[IPv6 addresses](https://tools.ietf.org/html/rfc4291) must include bounding brackets and be in [conventional form](https://tools.ietf.org/html/rfc4291#section-2.2) (for example, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`).</span></span> <span data-ttu-id="e2c4e-181">IPv6 adresleri farklı biçimler arasında mantıksal eşitlik denetlemek için özel harfleri değil ve yok Standartlaştırma gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-181">IPv6 addresses aren't special-cased to check for logical equality between different formats, and no canonicalization is performed.</span></span></li><li><span data-ttu-id="e2c4e-182">İzin verilen konakları sınırlamak için başarısızlık, bir saldırganın hizmeti tarafından oluşturulan bağlantıları sızmasını.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-182">Failure to restrict the allowed hosts may allow an attacker to spoof links generated by the service.</span></span></li></ul><span data-ttu-id="e2c4e-183">Boş bir varsayılan değer: `IList<string>`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-183">The default value is an empty `IList<string>`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | <span data-ttu-id="e2c4e-184">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-184">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName).</span></span> <span data-ttu-id="e2c4e-185">Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-For` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-185">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-For` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="e2c4e-186">Varsayılan, `X-Forwarded-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-186">The default is `X-Forwarded-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | <span data-ttu-id="e2c4e-187">Hangi ileticileri işlenmesi gerektiğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-187">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="e2c4e-188">Bkz: [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) için geçerli olan alanların listesi.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-188">See the [ForwardedHeaders Enum](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) for the list of fields that apply.</span></span> <span data-ttu-id="e2c4e-189">Bu özelliğe atanmış tipik değerler ' ForwardedHeaders.XForwardedFor</span><span class="sxs-lookup"><span data-stu-id="e2c4e-189">Typical values assigned to this property are \`ForwardedHeaders.XForwardedFor</span></span> | <span data-ttu-id="e2c4e-190">ForwardedHeaders.XForwardedProto\`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-190">ForwardedHeaders.XForwardedProto\`.</span></span><br><br><span data-ttu-id="e2c4e-191">Varsayılan değer [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-191">The default value is [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | <span data-ttu-id="e2c4e-192">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-192">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName).</span></span> <span data-ttu-id="e2c4e-193">Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Host` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-193">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Host` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="e2c4e-194">Varsayılan, `X-Forwarded-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-194">The default is `X-Forwarded-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | <span data-ttu-id="e2c4e-195">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-195">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName).</span></span> <span data-ttu-id="e2c4e-196">Proxy/iletici kullanmıyorsa bu seçenek kullanıldığında `X-Forwarded-Proto` üstbilgisi ancak kullanan başka bir üst bilgi bilgileri iletmek için.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-196">This option is used when the proxy/forwarder doesn't use the `X-Forwarded-Proto` header but uses some other header to forward the information.</span></span><br><br><span data-ttu-id="e2c4e-197">Varsayılan, `X-Forwarded-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-197">The default is `X-Forwarded-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | <span data-ttu-id="e2c4e-198">İşlenen üstbilgileri içerisindeki giriş sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-198">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="e2c4e-199">Kümesine `null` sınırı, ancak bunu devre dışı bırakmak için yalnızca, yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-199">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="e2c4e-200">Varsayılan değer 1'dir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-200">The default is 1.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | <span data-ttu-id="e2c4e-201">Adres aralıklarını iletilen üst bilgiler kabul etmek için bilinen ağlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-201">Address ranges of known networks to accept forwarded headers from.</span></span> <span data-ttu-id="e2c4e-202">Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanan IP aralıklarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-202">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="e2c4e-203">Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-203">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="e2c4e-204">Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-204">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="e2c4e-205">Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-205">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="e2c4e-206">Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-206">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="e2c4e-207">Varsayılan bir `IList` \< <xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> için tek bir giriş içeren `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-207">The default is an `IList`\<<xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> containing a single entry for `IPAddress.Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | <span data-ttu-id="e2c4e-208">İletilen üst bilgiler kabul etmek için bilinen Proxy adresleri.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-208">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="e2c4e-209">Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-209">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="e2c4e-210">Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-210">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1`).</span></span> <span data-ttu-id="e2c4e-211">Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-211">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="e2c4e-212">Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-212">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span> <span data-ttu-id="e2c4e-213">Daha fazla bilgi için [yapılandırma bir IPv4 adresi için bir IPv6 adresi olarak temsil edilen](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-213">For more information, see the [Configuration for an IPv4 address represented as an IPv6 address](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) section.</span></span><br><br><span data-ttu-id="e2c4e-214">Varsayılan bir `IList` \< <xref:System.Net.IPAddress>> için tek bir giriş içeren `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-214">The default is an `IList`\<<xref:System.Net.IPAddress>> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | <span data-ttu-id="e2c4e-215">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-215">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).</span></span><br><br><span data-ttu-id="e2c4e-216">Varsayılan, `X-Original-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-216">The default is `X-Original-For`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | <span data-ttu-id="e2c4e-217">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-217">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).</span></span><br><br><span data-ttu-id="e2c4e-218">Varsayılan, `X-Original-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-218">The default is `X-Original-Host`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | <span data-ttu-id="e2c4e-219">İle belirtilen yerine bu özelliği tarafından belirtilen üst bilgi kullan [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-219">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).</span></span><br><br><span data-ttu-id="e2c4e-220">Varsayılan, `X-Original-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-220">The default is `X-Original-Proto`.</span></span> |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | <span data-ttu-id="e2c4e-221">Üstbilgi değerleri arasında eşit olacak şekilde sayısı gerektir [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) işleniyor.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-221">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) being processed.</span></span><br><br><span data-ttu-id="e2c4e-222">ASP.NET Core 1.x olan varsayılan `true`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-222">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="e2c4e-223">ASP.NET Core 2.0 veya sonraki sürümlerde varsayılan `false`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-223">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="e2c4e-224">Senaryolar ve kullanım örnekleri</span><span class="sxs-lookup"><span data-stu-id="e2c4e-224">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="e2c4e-225">Üst bilgiler ve tüm istekleri iletilen eklemek mümkün olmadığında güvenli</span><span class="sxs-lookup"><span data-stu-id="e2c4e-225">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="e2c4e-226">Bazı durumlarda, uygulamaya proxy istekleri iletilen üstbilgilerini eklemek mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-226">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="e2c4e-227">Tüm genel dış istekler HTTPS olduğunu proxy zorlama, Düzen el ile ayarlanabilir `Startup.Configure` herhangi bir türde bir ara yazılım kullanmadan önce:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-227">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="e2c4e-228">Bu kod bir ortam değişkenine ya da diğer geliştirme ya da hazırlık ortamı yapılandırma ayarı ile devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-228">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="e2c4e-229">Yolu temel ve istek yolu değiştirmek proxy'ler ile Dağıt</span><span class="sxs-lookup"><span data-stu-id="e2c4e-229">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="e2c4e-230">Bazı Ara sunucular yolu sağlam geçirmek ancak bir uygulamayla yönlendirme böylece kaldırılmalıdır temel yolu düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-230">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="e2c4e-231">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) ara yazılım yolu böler [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) ve uygulama temel yolu [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-231">[UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) middleware splits the path into [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) and the app base path into [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).</span></span>

<span data-ttu-id="e2c4e-232">Varsa `/foo` proxy yolu olarak geçirilen uygulama temel yolu aranır `/foo/api/1`, ara yazılım kümeleri `Request.PathBase` için `/foo` ve `Request.Path` için `/api/1` aşağıdaki komutla:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-232">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="e2c4e-233">Ara yazılım tersten yeniden çağrıldığında, orijinal yolunu ve yolu tabanı yeniden uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-233">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="e2c4e-234">Ara yazılım sipariş işleme hakkında daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-234">For more information on middleware order processing, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="e2c4e-235">Proxy yolu kırpar varsa (örneğin, iletme `/foo/api/1` için `/api/1`), düzeltme yönlendirir ve bağlantılar isteğin ayarlayarak [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) özelliği:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-235">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="e2c4e-236">Proxy yol verileri ekliyorsanız yeniden yönlendirir ve bağlantıları kullanarak düzeltmek için yolunun parçası at <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> ve atama <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> özelliği:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-236">If the proxy is adding path data, discard part of the path to fix redirects and links by using <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> and assigning to the <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> property:</span></span>

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

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a><span data-ttu-id="e2c4e-237">Farklı üst bilgi adları kullanan proxy için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e2c4e-237">Configuration for a proxy that uses different header names</span></span>

<span data-ttu-id="e2c4e-238">Proxy adlı üstbilgileri kullanmıyorsa `X-Forwarded-For` ve `X-Forwarded-Proto` proxy adresini/bağlantı noktası ve şema bilgilerini kaynaklanan iletmek için ayarlanmış <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> ve <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> proxy tarafından kullanılan üstbilgi adlarını eşleştirmek için seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-238">If the proxy doesn't use headers named `X-Forwarded-For` and `X-Forwarded-Proto` to forward the proxy address/port and originating scheme information, set the <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> and <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> options to match the header names used by the proxy:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a><span data-ttu-id="e2c4e-239">Bir IPv6 adresi olarak temsil edilen bir IPv4 adresi için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e2c4e-239">Configuration for an IPv4 address represented as an IPv6 address</span></span>

<span data-ttu-id="e2c4e-240">Çift modlu yuva sunucusu kullanıyorsanız, IPv4 adresleri bir IPv6 biçiminde sağlanır (örneğin, `10.0.0.1` IPv6 temsil IPv4'te `::ffff:10.0.0.1` veya `::ffff:a00:1`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-240">If the server is using dual-mode sockets, IPv4 addresses are supplied in an IPv6 format (for example, `10.0.0.1` in IPv4 represented in IPv6 as `::ffff:10.0.0.1` or `::ffff:a00:1`).</span></span> <span data-ttu-id="e2c4e-241">Bkz: [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-241">See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*).</span></span> <span data-ttu-id="e2c4e-242">Bu biçim bakarak gerekip gerekmediğini belirleyin [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-242">Determine if this format is required by looking at the [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).</span></span>

<span data-ttu-id="e2c4e-243">Aşağıdaki örnekte, üst bilgileri iletilen sağlayan bir ağ adresi için eklenen `KnownNetworks` IPv6 biçiminde listesi.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-243">In the following example, a network address that supplies forwarded headers is added to the `KnownNetworks` list in IPv6 format.</span></span>

<span data-ttu-id="e2c4e-244">IPv4 adresi: `10.11.12.1/8`</span><span class="sxs-lookup"><span data-stu-id="e2c4e-244">IPv4 address: `10.11.12.1/8`</span></span>

<span data-ttu-id="e2c4e-245">Dönüştürülen IPv6 adresi: `::ffff:10.11.12.1`</span><span class="sxs-lookup"><span data-stu-id="e2c4e-245">Converted IPv6 address: `::ffff:10.11.12.1`</span></span>  
<span data-ttu-id="e2c4e-246">Dönüştürülen önek uzunluğu: 104</span><span class="sxs-lookup"><span data-stu-id="e2c4e-246">Converted prefix length: 104</span></span>

<span data-ttu-id="e2c4e-247">Onaltılık biçimde adresi de sağlayabilirsiniz (`10.11.12.1` IPv6 temsil edilen `::ffff:0a0b:0c01`).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-247">You can also supply the address in hexadecimal format (`10.11.12.1` represented in IPv6 as `::ffff:0a0b:0c01`).</span></span> <span data-ttu-id="e2c4e-248">Bir IPv4 adresi için IPv6 dönüştürülürken, CIDR ön ek uzunluğu için 96 ekleyin (`8` örnekte) için ek hesap için `::ffff:` IPv6 ön eki (8 + 96 = 104).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-248">When converting an IPv4 address to IPv6, add 96 to the CIDR Prefix Length (`8` in the example) to account for the additional `::ffff:` IPv6 prefix (8 + 96 = 104).</span></span> 

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

## <a name="troubleshoot"></a><span data-ttu-id="e2c4e-249">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="e2c4e-249">Troubleshoot</span></span>

<span data-ttu-id="e2c4e-250">Üst bilgiler, beklendiği gibi iletilen olmayan etkinleştirirsiniz [günlüğü](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-250">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="e2c4e-251">Günlükleri sorunu gidermek için yeterli bilgi sağlamazsanız, sunucu tarafından alınan isteği üstbilgileri sıralar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-251">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="e2c4e-252">Satır içi ara yazılım istek üstbilgileri, bir uygulama yanıtı yazmak veya üst bilgileri kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-252">Use inline middleware to write request headers to an app response or log the headers.</span></span> 

<span data-ttu-id="e2c4e-253">Uygulamanın yanıtı üstbilgileri yazmak için çağırdıktan hemen sonra aşağıdaki terminal satır içi ara yazılım yerleştirin <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> içinde `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-253">To write the headers to the app's response, place the following terminal inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`:</span></span>

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

<span data-ttu-id="e2c4e-254">Yanıt gövdesi yerine günlükleri yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-254">You can write to logs instead of the response body.</span></span> <span data-ttu-id="e2c4e-255">Günlükleri yazmak hata ayıklarken işlev siteye normalde sağlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-255">Writing to logs allows the site to function normally while debugging.</span></span>

<span data-ttu-id="e2c4e-256">Günlükleri yazmak yerine yanıt gövdesi:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-256">To write logs rather than to the response body:</span></span>

* <span data-ttu-id="e2c4e-257">Ekleme `ILogger<Startup>` içine `Startup` sınıfı açıklandığı [başlangıç günlükleri oluşturma](xref:fundamentals/logging/index#create-logs-in-startup).</span><span class="sxs-lookup"><span data-stu-id="e2c4e-257">Inject `ILogger<Startup>` into the `Startup` class as described in [Create logs in Startup](xref:fundamentals/logging/index#create-logs-in-startup).</span></span>
* <span data-ttu-id="e2c4e-258">Çağırdıktan hemen sonra aşağıdaki satır içi ara yazılım yerleştirin <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> içinde `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-258">Place the following inline middleware immediately after the call to <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> in `Startup.Configure`.</span></span>

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

<span data-ttu-id="e2c4e-259">İşlendiğinde `X-Forwarded-{For|Proto|Host}` değerleri taşınmıştır `X-Original-{For|Proto|Host}`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-259">When processed, `X-Forwarded-{For|Proto|Host}` values are moved to `X-Original-{For|Proto|Host}`.</span></span> <span data-ttu-id="e2c4e-260">Belirli bir üst bilgisinde birden çok değer varsa, ters sırada sağdan sola iletilen üstbilgileri ara yazılım işlemleri üstbilgileri unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-260">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span> <span data-ttu-id="e2c4e-261">Varsayılan `ForwardLimit` 1 (bir), bu nedenle olduğu sürece yalnızca başlıklarından en sağdaki değer işlenen değerini `ForwardLimit` artırılır.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-261">The default `ForwardLimit` is 1 (one), so only the rightmost value from the headers is processed unless the value of `ForwardLimit` is increased.</span></span>

<span data-ttu-id="e2c4e-262">İsteğin özgün uzak IP bir girişe eşleşmelidir `KnownProxies` veya `KnownNetworks` iletilen üstbilgileri işlenmeden önce listeler.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-262">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before forwarded headers are processed.</span></span> <span data-ttu-id="e2c4e-263">Bu, güvenilir olmayan proxy'si İleticilerden kabul etmeyerek üst bilgi sızdırma sınırlar.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-263">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span> <span data-ttu-id="e2c4e-264">Bilinmeyen bir proxy algılandığında, günlüğe kaydetme proxy adresini belirtir:</span><span class="sxs-lookup"><span data-stu-id="e2c4e-264">When an unknown proxy is detected, logging indicates the address of the proxy:</span></span>

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

<span data-ttu-id="e2c4e-265">Önceki örnekte 10.0.0.100 bir proxy sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-265">In the preceding example, 10.0.0.100 is a proxy server.</span></span> <span data-ttu-id="e2c4e-266">Sunucunun IP adresi için güvenilir bir proxy sunucu ise ekleme `KnownProxies` (veya güvenilen bir ağa ekleyin `KnownNetworks`) içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-266">If the server is a trusted proxy, add the server's IP address to `KnownProxies` (or add a trusted network to `KnownNetworks`) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e2c4e-267">Daha fazla bilgi için [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-267">For more information, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options) section.</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> <span data-ttu-id="e2c4e-268">Yalnızca güvenilen proxy'leri ve ağları üst bilgiler iletmek izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-268">Only allow trusted proxies and networks to forward headers.</span></span> <span data-ttu-id="e2c4e-269">Aksi takdirde, [IP yanıltma](https://www.iplocation.net/ip-spoofing) saldırıları mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e2c4e-269">Otherwise, [IP spoofing](https://www.iplocation.net/ip-spoofing) attacks are possible.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2c4e-270">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e2c4e-270">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
* [<span data-ttu-id="e2c4e-271">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı</span><span class="sxs-lookup"><span data-stu-id="e2c4e-271">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
