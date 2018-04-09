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
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="1f770-103">Proxy sunucuları ile çalışma ve yük Dengeleyiciler için ASP.NET Core yapılandırın</span><span class="sxs-lookup"><span data-stu-id="1f770-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="1f770-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Chris fillerin](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="1f770-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="1f770-105">ASP.NET Core için önerilen yapılandırma, IIS/ASP.NET çekirdek modülü, Nginx ya da Apache kullanarak uygulama barındırılır.</span><span class="sxs-lookup"><span data-stu-id="1f770-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="1f770-106">Genellikle uygulama erişmeden önce proxy sunucular, yük dengeleyicileri ve başka ağ gereçlerine isteğiyle ilgili bilgileri soyutlamaması:</span><span class="sxs-lookup"><span data-stu-id="1f770-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="1f770-107">HTTPS istekleri HTTP üzerinden yönlendirilirken, özgün düzenini (HTTPS) kaybolur ve bir üstbilgisinde gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1f770-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="1f770-108">Uygulama proxy'si ve doğru kaynağına değil Internet veya kurumsal bir ağda bir istek aldığından, kaynak istemci IP adresini de başlığı gönderilmelidir.</span><span class="sxs-lookup"><span data-stu-id="1f770-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="1f770-109">Bu bilgiler, örneğin yeniden yönlendirmeleri, kimlik doğrulama, bağlantı oluşturma, ilke değerlendirmesi ve istemci geoloation istek işleme önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="1f770-110">İletilen üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="1f770-110">Forwarded headers</span></span>

<span data-ttu-id="1f770-111">Kurala göre proxy'leri HTTP üst bilgilerinde bilgileri iletin.</span><span class="sxs-lookup"><span data-stu-id="1f770-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="1f770-112">Üstbilgi</span><span class="sxs-lookup"><span data-stu-id="1f770-112">Header</span></span> | <span data-ttu-id="1f770-113">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1f770-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="1f770-114">X-Forwarded-For</span><span class="sxs-lookup"><span data-stu-id="1f770-114">X-Forwarded-For</span></span> | <span data-ttu-id="1f770-115">İstek ve proxy'ler zinciri sonraki proxy'leri başlatılan istemci ile ilgili bilgileri tutar.</span><span class="sxs-lookup"><span data-stu-id="1f770-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="1f770-116">Bu parametre, IP adresleri (ve isteğe bağlı olarak, bağlantı noktası numaralarını) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="1f770-117">Bir proxy sunucu zincirinde ilk parametre istek ilk yapıldığı istemci gösterir.</span><span class="sxs-lookup"><span data-stu-id="1f770-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="1f770-118">Sonraki proxy tanımlayıcıları izleyin.</span><span class="sxs-lookup"><span data-stu-id="1f770-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="1f770-119">Zincirdeki son proxy parametreleri listesinde değil.</span><span class="sxs-lookup"><span data-stu-id="1f770-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="1f770-120">Son proxy'nin IP adresi ve isteğe bağlı olarak bir bağlantı noktası numarası aktarım katmanında uzak IP adresi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="1f770-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="1f770-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="1f770-122">Kaynak şema (HTTP/HTTPS) değeri.</span><span class="sxs-lookup"><span data-stu-id="1f770-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="1f770-123">İstek birden çok proxy'leri geçiş değilse değer düzenleri listesini de olabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="1f770-124">X-Forwarded-Host</span><span class="sxs-lookup"><span data-stu-id="1f770-124">X-Forwarded-Host</span></span> | <span data-ttu-id="1f770-125">Ana bilgisayar üstbilgisi alanı özgün değeri.</span><span class="sxs-lookup"><span data-stu-id="1f770-125">The original value of the Host header field.</span></span> <span data-ttu-id="1f770-126">Genellikle, proxy'leri ana bilgisayar üstbilgisi değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="1f770-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="1f770-127">Bkz: [Microsoft güvenlik önerisi CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) burada değil proxy doğrulama sistemleri etkiler ayrıcalık yükseltme güvenlik açığı veya bilinen iyi değerlere restict konak üstbilgileri hakkında bilgi için.</span><span class="sxs-lookup"><span data-stu-id="1f770-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="1f770-128">İletilen üstbilgileri Ara gelen [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) paketini, bu üstbilgileri okur ve ilişkili alanlarında doldurur [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="1f770-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="1f770-129">Ara yazılım güncelleştirmeleri:</span><span class="sxs-lookup"><span data-stu-id="1f770-129">The middleware updates:</span></span>

* <span data-ttu-id="1f770-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span><span class="sxs-lookup"><span data-stu-id="1f770-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="1f770-131">Ek ayarları etkileyen nasıl ara yazılım ayarlar `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="1f770-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="1f770-132">Ayrıntılar için bkz [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="1f770-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="1f770-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; kullanılarak ayarlanan `X-Forwarded-Proto` üstbilgi değeri.</span><span class="sxs-lookup"><span data-stu-id="1f770-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="1f770-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; kullanılarak ayarlanan `X-Forwarded-Host` üstbilgi değeri.</span><span class="sxs-lookup"><span data-stu-id="1f770-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="1f770-135">Tüm ağ cihazları ekleme Not `X-Forwarded-For` ve `X-Forwarded-Proto` ek yapılandırma olmadan üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="1f770-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="1f770-136">Uygulama ulaştığında yönlendirilirken istekleri bu üstbilgileri içermiyorsa Gereci üreticinin Kılavuzu başvurun.</span><span class="sxs-lookup"><span data-stu-id="1f770-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="1f770-137">Üstbilgiler Ara iletilen [varsayılan ayarları](#forwarded-headers-middleware-options) yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="1f770-138">Varsayılan ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="1f770-138">The default settings are:</span></span>

* <span data-ttu-id="1f770-139">Olduğundan yalnızca *bir proxy* uygulama ve istekleri kaynak arasında.</span><span class="sxs-lookup"><span data-stu-id="1f770-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="1f770-140">Yalnızca geri döngü adresleri bilinen proxy'leri için yapılandırılır ve ağları bilinen.</span><span class="sxs-lookup"><span data-stu-id="1f770-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="1f770-141">IIS/IIS Express ve ASP.NET Core Modülü</span><span class="sxs-lookup"><span data-stu-id="1f770-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="1f770-142">IIS ve ASP.NET Core modülü arkasındaki uygulama çalıştırıldığında iletilen üstbilgileri Ara IIS tümleştirme ara yazılım tarafından varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="1f770-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="1f770-143">İletilen üstbilgileri Ara iletilen üst bilgileri ile güven sorunları nedeniyle ASP.NET Core Modülü'nü ilk ara yazılım ardışık düzenini belirli kısıtlı bir yapılandırma ile olarak çalıştırmak için etkinleştirildi (örneğin, [IP yanıltma](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="1f770-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="1f770-144">Ara yazılım iletmek için yapılandırılan `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgiler ve tek localhost proxy ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="1f770-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="1f770-145">Ek yapılandırma gerekli olup olmadığını [iletilen üstbilgileri ara yazılım seçenekleri](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="1f770-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1f770-146">Diğer proxy sunucusu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="1f770-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1f770-147">IIS tümleştirme ara yazılımı kullanarak dışında iletilen üstbilgileri ara yazılımı varsayılan olarak etkin değildir.</span><span class="sxs-lookup"><span data-stu-id="1f770-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="1f770-148">İletilen üstbilgileri ara yazılım etkin, bir uygulama ile iletilen işlem başlıklar için [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="1f770-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="1f770-149">Ara yazılım yoksa etkinleştirdikten sonra [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ara yazılımı varsayılan belirtilen [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) olan [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="1f770-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="1f770-150">Ara yazılımla yapılandırma [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletmek için `X-Forwarded-For` ve `X-Forwarded-Proto` üstbilgilerinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1f770-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1f770-151">Çağırma [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) yönteminde `Startup.Configure` diğer ara yazılımdan çağırmadan önce:</span><span class="sxs-lookup"><span data-stu-id="1f770-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

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
> <span data-ttu-id="1f770-152">Öyle değilse [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) belirtilen `Startup.ConfigureServices` veya doğrudan uzantısı yöntemiyle [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), varsayılan iletmek için başlıkları [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="1f770-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="1f770-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) özelliği iletmek için üstbilgiler ile yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="1f770-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="1f770-154">İletilen üstbilgileri ara yazılım seçenekleri</span><span class="sxs-lookup"><span data-stu-id="1f770-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="1f770-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) iletilen üstbilgileri ara yazılım davranışını denetler:</span><span class="sxs-lookup"><span data-stu-id="1f770-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="1f770-156">Seçenek</span><span class="sxs-lookup"><span data-stu-id="1f770-156">Option</span></span> | <span data-ttu-id="1f770-157">Açıklama</span><span class="sxs-lookup"><span data-stu-id="1f770-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="1f770-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="1f770-159">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="1f770-160">Varsayılan, `X-Forwarded-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="1f770-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="1f770-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="1f770-162">Hangi ileticiler işlenmesi gerektiğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="1f770-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="1f770-163">Bkz: [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) için geçerli bir alanlar listesi.</span><span class="sxs-lookup"><span data-stu-id="1f770-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="1f770-164">Bu özelliğe atanmış tipik değerler <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="1f770-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="1f770-165">Varsayılan değer [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="1f770-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="1f770-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="1f770-167">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="1f770-168">Varsayılan, `X-Forwarded-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="1f770-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="1f770-170">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="1f770-171">Varsayılan, `X-Forwarded-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="1f770-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="1f770-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="1f770-173">İşlenen üstbilgileri girişlerinde sayısını sınırlar.</span><span class="sxs-lookup"><span data-stu-id="1f770-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="1f770-174">Kümesine `null` sınırı, ancak bu devre dışı bırakmak için yalnızca varsa yapılmalıdır `KnownProxies` veya `KnownNetworks` yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="1f770-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="1f770-175">Varsayılan değer 1'dir.</span><span class="sxs-lookup"><span data-stu-id="1f770-175">The default is 1.</span></span> |
| [<span data-ttu-id="1f770-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="1f770-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="1f770-177">Adres aralıklarını iletilen üstbilgileri kabul etmek için bilinen proxy.</span><span class="sxs-lookup"><span data-stu-id="1f770-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="1f770-178">Sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak IP aralıklarını belirtin.</span><span class="sxs-lookup"><span data-stu-id="1f770-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="1f770-179">Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP ağı](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> için tek bir girdi içeren `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="1f770-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="1f770-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="1f770-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="1f770-181">İletilen üstbilgileri kabul etmek için bilinen Proxy adresleri.</span><span class="sxs-lookup"><span data-stu-id="1f770-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="1f770-182">Kullanım `KnownProxies` tam IP adresini belirtmek için eşleşir.</span><span class="sxs-lookup"><span data-stu-id="1f770-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="1f770-183">Varsayılan bir [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPADDRESS](/dotnet/api/system.net.ipaddress)> için tek bir girdi içeren `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="1f770-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="1f770-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="1f770-185">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="1f770-186">Varsayılan, `X-Original-For` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="1f770-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="1f770-188">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="1f770-189">Varsayılan, `X-Original-Host` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="1f770-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="1f770-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="1f770-191">Bu özellik tarafından belirtilenden yerine tarafından belirtilen üstbilgi kullanmak [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="1f770-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="1f770-192">Varsayılan, `X-Original-Proto` değeridir.</span><span class="sxs-lookup"><span data-stu-id="1f770-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="1f770-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="1f770-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="1f770-194">Üstbilgi değerleri arasında eşit sayıda gerekir [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) işleniyor.</span><span class="sxs-lookup"><span data-stu-id="1f770-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="1f770-195">Varsayılan olarak ASP.NET 1.x olan çekirdek `true`.</span><span class="sxs-lookup"><span data-stu-id="1f770-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="1f770-196">Varsayılan ASP.NET Core 2.0 veya sonraki sürümlerde `false`.</span><span class="sxs-lookup"><span data-stu-id="1f770-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="1f770-197">Senaryolar ve kullanım örnekleri</span><span class="sxs-lookup"><span data-stu-id="1f770-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="1f770-198">Üstbilgiler ve tüm isteklerin iletilen eklemek mümkün değilse güvenli</span><span class="sxs-lookup"><span data-stu-id="1f770-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="1f770-199">Bazı durumlarda, uygulamaya yönlendirilirken istekleri iletilen üstbilgilerini eklemek mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="1f770-200">Tüm genel dış istekleri HTTPS olduğunu proxy zorlama, düzeni el ile ayarlanabilir `Startup.Configure` ara yazılım herhangi bir türde kullanmadan önce:</span><span class="sxs-lookup"><span data-stu-id="1f770-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="1f770-201">Bu kod bir ortam değişkeni veya geliştirme veya hazırlama ortamında diğer yapılandırma ayarı devre dışı bırakılabilir.</span><span class="sxs-lookup"><span data-stu-id="1f770-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="1f770-202">İstek yolu değiştirmek proxy'leri temel yolu ile Dağıt</span><span class="sxs-lookup"><span data-stu-id="1f770-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="1f770-203">Bazı proxy'leri yolun olduğu gibi geçirmek, ancak bir uygulamayla yönlendirme kaldırılması temel yolu düzgün çalışır.</span><span class="sxs-lookup"><span data-stu-id="1f770-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="1f770-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) ara yazılım böler yola [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) ve uygulama temel yola [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="1f770-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="1f770-205">Varsa `/foo` bir proxy yolu olarak geçirilen uygulama temel yolu aranır `/foo/api/1`, ara yazılımı kümeleri `Request.PathBase` için `/foo` ve `Request.Path` için `/api/1` aşağıdaki komutla:</span><span class="sxs-lookup"><span data-stu-id="1f770-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="1f770-206">Geriye doğru ara yazılımı yeniden çağrıldığında orijinal yolunu ve yolu tabanı yeniden uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1f770-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="1f770-207">Ara yazılım sipariş işleme hakkında daha fazla bilgi için bkz: [Ara](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="1f770-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="1f770-208">Proxy yolu kırpar varsa (örneğin, iletme `/foo/api/1` için `/api/1`), düzeltme yönlendirir ve bağlantıları isteğin ayarlayarak [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) özelliği:</span><span class="sxs-lookup"><span data-stu-id="1f770-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="1f770-209">Proxy yolu veri ekleme, yeniden yönlendirir ve bağlantıları kullanarak düzeltmek için yolun bir kısmı atmak [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) ve atama [yolu](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) özelliği:</span><span class="sxs-lookup"><span data-stu-id="1f770-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

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

## <a name="troubleshoot"></a><span data-ttu-id="1f770-210">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="1f770-210">Troubleshoot</span></span>

<span data-ttu-id="1f770-211">Üstbilgiler beklendiği gibi iletilen değil, etkinleştirme [günlüğü](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1f770-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="1f770-212">Günlükleri sorunu gidermek için yeterli bilgi sağlamıyorsa, sunucu tarafından alınan istek üstbilgileri sıralar.</span><span class="sxs-lookup"><span data-stu-id="1f770-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="1f770-213">Satır içi ara yazılımı kullanarak bir uygulama yanıt üstbilgileri yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="1f770-213">The headers can be written to an app response using inline middleware:</span></span>

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

<span data-ttu-id="1f770-214">-İletilen - X emin \* beklenen değerler ile sunucu tarafından alınan üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="1f770-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="1f770-215">Belirli bir üstbilgisinde birden çok değer varsa iletilen üstbilgileri Ara işlemleri üstbilgileri ters sırada sağdan sola unutmayın.</span><span class="sxs-lookup"><span data-stu-id="1f770-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="1f770-216">İsteğin özgün uzak IP bir girişe eşleşmelidir `KnownProxies` veya `KnownNetworks` X-iletilen-için işlenmeden önce listeler.</span><span class="sxs-lookup"><span data-stu-id="1f770-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="1f770-217">Bu, güvenilmeyen proxy'leri İleticilerden kabul etmeyerek üstbilgi yanıltma sınırlar.</span><span class="sxs-lookup"><span data-stu-id="1f770-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f770-218">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1f770-218">Additional resources</span></span>

* [<span data-ttu-id="1f770-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core ayrıcalık yükselmesi güvenlik açığı</span><span class="sxs-lookup"><span data-stu-id="1f770-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
